# ORM框架

Object Relational Mappings 对象关系映射

---

#### Python实现

设计ORM要从上层调用者角度来看
首先是如何定义一个User对象，把数据库的User表和它对应起来，可以使用dict来完成，key和value分别对应表中字段和值，不同的类对应不同的表，即不同类继承自dict，同时可以有一些方法可以实现增删改查等操作

```python
# ORM框架提供了基类Model和各种字段类型，使用框架时
# 编写子类继承基类Model，并在子类中，使用类属性来制定和表中字段的对应关系
from orm import Model, StringField, IntegerField

class User(Model):
    # id, name, email, password是Field类实例化的对象
    id = IntegerField('id', primary_key=True)
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')
    # 这里的通过类属性来对应数据库表的字段
    # 实际上是并没有绑定了数据库表和Field类型对象的对应关系
    # 只是通过类型的限制规范，以使得数据库操作可以正常进行
```

通过类属性来描述User对象和表的映射关系，实例属性则通过`__init__()`方法去初始化

```python
# 该实例是一个dict，其中的id=123，name='Ihsin'是key-value，对应表中相应字段的值
user = User(id=123, name='Ihsin')

user.insert()
users = User.findAll()
```

元类ModelMetaclass为：

```python
class ModelMetaclass(type):
    def __new__(cls, name, bases, attrs):
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        tableName = attrs.get('__table__', None) or name
        logging.info('found model: %s (table: %s)' % (name, tableName))
        mappings = dict()
        fields = []
        primaryKey = None
        # 此处k经验证是字符串str类型，v是原来的类型，即Field类型
        for k, v in attrs.items():
            if isinstance(v, Field):
                logging.info(' found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
                if v.primary_key:
                    if primaryKey:
                        raise Exception('Duplicate primary key for field: %s' % k)
                    primaryKey = k
                else:
                    fields.append(k)
        if not primaryKey:
            raise Exception('Primary key not found.')
        for k in mappings.keys():
            attrs.pop(k)
        escaped_fields = list(map(lambda f: '`%s`' % f, fields))
        attrs['__mappings__'] = mappings
        attrs['__table__'] = tableName
        attrs['__primary_key__'] = primaryKey
        attrs['__fields__'] = fields
        attrs['__select__'] = 'select `%s`, %s from `%s`' % (primaryKey, ', '.join(escaped_fields), tableName)
        attrs['__insert__'] = 'insert into `%s` (%s, `%s`) values (%s)' % (tableName, ', '.join(escaped_fields), primaryKey, create_args_string(len(escaped_fields) + 1))
        attrs['__update__'] = 'update `%s` set %s where `%s`=?' % (tableName, ', '.join(map(lambda f: '`%s`=?' % (mappings.get(f).name or f), fields)), primaryKey)
        attrs['__delete__'] = 'delete from `%s` where `%s`=?' % (tableName, primaryKey)
        return type.__new__(cls, name, bases, attrs)
```

基类Model为：

```python
# 这里需要借助ModelMetaclass元类来自动的完成一些工作
class Model(dict, metaclass=ModelMetaclass):
    def __init__(self, **kw):
        super().__init__(**kw)
    
    # __getattr__(self, key)和__setattr__(self, key, value)两个方法
    # __getattr__即对象.属性。如果找不到属性，则会用属性名作为key，调用该方法
    # __setattr__即在设置__dict__的item时就会调用
    # 本例中__setattr__函数中并没有进行self.__dict__[key] = value，因此
    # 本例相当于将 对象.key = value 也等价于对象[key] = value
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)
    
    def __setattr__(self, key, value):
        self[key] = value
        
    def getValue(self, key):
        return getattr(self, key, None)
    
    def getValueOrDefault(self, key):
        value = getattr(self, key, None)
        if value is None:
            field = self.__mappings__[key]
            if field.default is not None:
                value = field.default() if callable(field.default) else field.default
                logging.debug('using default value for %s: %s' % (key, str(value)))
                setattr(self, key, value)
        return value
    
    @classmethod
    async def findAll(cls, where=None, args=None, **kw):
        ' find objects by where clause. '
        sql = [cls.__select__]
        if where:
            sql.append('where')
            sql.append(where)
        if args is None:
            args = []
        orderBy = kw.get('orderBy', None)
        if orderBy:
            sql.append('order by')
            sql.append(orderBy)
        limit = kw.get('limit', None)
        if limit is not None:
            sql.append('limit')
            if isinstance(limit, int):
                sql.append('?')
                args.append(limit)
            elif isinstance(limit, tuple) and len(limit) == 2:
                sql.append('?, ?')
                args.extend(limit)
            else:
                raise ValueError('Invalid limit value: %s' % str(limit))
        rs = await select(' '.join(sql), args)
        return [cls(**r) for r in rs]

    @classmethod
    async def findNumber(cls, selectField, where=None, args=None):
        ' find number by select and where. '
        # 这里的_num_是将select返回结果以新一栏名为_num_来表示，_num_可以是任意名字
        sql = ['select %s _num_ from `%s`' % (selectField, cls.__table__)]
        if where:
            sql.append('where')
            sql.append(where)
        rs = await select(' '.join(sql), args, 1)
        if len(rs) == 0:
            return None
        return rs[0]['_num_']
    
    @classmethod
    async def find(cls, pk):
        ' find object by primary key. '
        rs = await select('%s where `%s`=?' % (cls.__select__, cls.__primary_key__), [pk], 1)
        if len(rs) == 0:
            return None
        return cls(**rs[0])
    
    async def save(self):
        args = list(map(self.getValueOfDefault, self.__fields__))
        args.append(self.getValueOfDefault(self.__primary_key__))
        rows = await execute(self.__insert__, args)
        if rows != 1:
            logging.warn('failed to insert record: affected rows: %s' % rows)
            
    async def update(self):
        # getattr是调用属性的
        # 不过由于没有相应属性，将会调用__getattr__
        args = list(map(self.getValue, self.__fields__))
        args.append(self.getValue(self.__primary_key__))
        rows = await execute(self.__update__, args)
        if rows != 1:
            logging.warn('failed to update by primary key: affected rows: %s' % rows)
        
    async def remove(self):
        args = [self.getValue(self.__primary_key__)]
        rows = await execute(self.__delete__, args)
        if rows != 1:
            logging.warn('failed to remove by primary key: affected rows: %s' % rows)
```

字段类为：

```python
class Field(object):
    def __init__(self, name, column_type, primary_key, default):
        self.name = name
        self.column_type = column_type
        self.primary_key = primary_key
        self.default = default
        
    def __str__(self):
        return '<%s, %s:%s>' % (self.__class__.__name__, self.column_type, self.name)

class StringField(Field):
    def __init__(self, name=None, primary_key=False, default=None, ddl='varchar(100)'):
        super().__init__(name, ddl, primary_key, default)

class IntegerField(Field):
    def __init__(self, ...)
    ...
```

