# Chapter6_2
Servlet提交表单

```aidl
吐槽一下自己,真的是眼高手低,本以为自己理解了写的
时候就瓜皮了,以下将主要知识点提取出来

```

### servlet.java
```aidl
这一部分主要是完善doGet()和doPost()的方法
 public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        request.setCharacterEncoding("utf-8");//设置request的编码格式
        response.setContentType("text/html");
        String name = request.getParameter("name");//获取用户名
        String ageStr = request.getParameter("age");//获取用户年龄
        String sex = request.getParameter("sex");//获取性别
        String address = request.getParameter("address");//获取地址
        String regex = "^\\+?[1-9][0-9]*$";//正则表达式
        int age = 0;
        if (ageStr.matches(regex)) {
            age = Integer.parseInt(ageStr);
        }

        User user = new User();//实例化User
        user.setName(name);
        user.setAddress(address);
        user.setAge(age);
        user.setSex(sex);
        ServletContext application = getServletContext();//获取ServletContext对象
        List<User> lt = (List<User>) application.getAttribute("users");
        if (lt == null) {
            lt = new ArrayList<User>();
        }
        lt.add(user);
        application.setAttribute("users", lt);
        request.getRequestDispatcher("/list.jsp").forward(request, response);

    }

```

其中的核心代码
```aidl
 ServletContext application = getServletContext();//获取ServletContext对象
 request.getRequestDispatcher("/list.jsp").forward(request, response);   //请求转发
 这里的request不是javax里的,而是HttpServletRequest的变量
 
```


### web.xml
将类和网址配置好
```aidl
<servlet>
        <servlet-name>AddServlet</servlet-name>
        <servlet-class>com.itzcn.servlet.AddServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>AddServlet</servlet-name>
        <url-pattern>/servlet/AddServlet</url-pattern>
    </servlet-mapping>
```

### index.jsp
提交处理的地方就是上面配置的网址,onsubmit处理的是javascript的脚本
```aidl
 <form action="servlet/AddServlet" method="post" onsubmit="return check(this);">
```

```aidl
<script type="text/javascript">
        function check(form) {
            with (form) {
                if (name.value == "") {
                    alert("姓名不能为空");
                    return false;
                }
                if (age.value == "") {
                    alert("年龄不能为空");
                    return false;
                }
                if (address.value == "") {
                    alert("地址不能为空");
                    return false;
                }
            }
        }
    </script>
```
