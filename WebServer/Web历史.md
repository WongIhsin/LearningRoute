### Web开发的历史

1. 静态Web页面：由文本编辑器直接编辑并生成静态的HTML页面，如果要修改Web页面的内容，就需要再次编辑HTML源文件，早期互联网Web页面就是静态的
2. CGI：C/C++编写的，静态页面无法与用户交互，比如用户添加了一个注册表单，静态Web页面就无法处理，要处理用户发送的动态数据，出现了Common Gateway Interface，CGI
3. **ASP/JSP/PHP**：由于Web应用特点是修改频繁，用C/C++这样的低级语言非常不适合Web开发，而脚本语言由于开发效率高，与HTML结合紧密，迅速取代了CGI模式。ASP是微软推出的用VBScript脚本编程的Web开发技术，而JSP用Java来编写脚本，PHP本身则是开源的脚本语言
4. **MVC**：为了解决直接用脚本语言嵌入HTML导致的可维护性差的问题，Web应用也引入了Model-View-Controller的模式，来简化Web开发，ASP发展为ASP.Net，JSP和PHP也有一大堆MVC框架

Python诞生比Web早，Python作为一种解释型的脚本语言，开发Web效率高，非常适用于Web开发