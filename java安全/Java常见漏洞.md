# Java常见漏洞

## fastjson反序列化

#### 解析：

Fastjson是一个JSON工具库，它的作用是把Java对象转换为JSON形式，也可以用来将JSON转换为Java对象。在反序列化时，会进入parseField方法，然后调用setValue(object, value)方法，这里是执行构造的恶意代码，最后造成代码执行。

该漏洞的利用点有两个：

第一是需要我们指定一个类，这个类的作用是为了让程序获取这个类来进行反序列化操作。

第二是需要将需要执行的代码提供给程序，所以这里使用了rmi。然后反序列化的时候会去请求rmi服务器，地址为：dnslog.cn/aaa。然后加载aaa这个恶意class文件从而造成代码执行。

请注意，目标服务器存在fastjson，没有对用户传输的数据进行严格过滤。如果有主机B的LDAP服务指定加载主机C的恶意java类，主机A通过主机B的LDAP服务最终加载并执行主机C的恶意java类。

为了防止Fastjson反序列化漏洞，需要对用户传输的数据进行严格过滤，并确保不加载恶意的java类。同时，请及时更新Fastjson库，并避免在不安全的网络环境中使用含有Fastjson库的系统。

#### 案例讲解：

假设有一个名为"User"的Java类，具有以下属性：

```java
public class User {  
    private String name;  
    private int age;  
    // 省略其他属性  
}
```

现在，我们使用Fastjson库将一个JSON字符串转换为User对象。例如：

```java
String json = "{\"name\":\"Alice\",\"age\":20}";  
User user = JSON.parseObject(json, User.class);
```

在这个例子中，我们将JSON字符串解析为User对象，并成功地获取了name和age属性。

然而，如果攻击者在JSON字符串中添加了一些恶意代码，例如：

```java
String json = "{\"name\":\"Alice\",\"age\":20,\"maliciousCode\":\"System.out.println(\"Hello, World!\");\"}";  
User user = JSON.parseObject(json, User.class);
```

在这个例子中，攻击者在JSON字符串中添加了一个名为"maliciousCode"的属性，它的值为一段恶意代码。当程序执行JSON反序列化时，这段恶意代码将被执行。

现在，假设我们有一个名为"deserialize"的方法，它接受一个JSON字符串作为输入，并将其反序列化为指定的Java对象。例如：

```java
public Object deserialize(String json, Class<?> clazz) {  
    return JSON.parseObject(json, clazz);  
}
```

这个方法很简单，它只是将JSON字符串解析为指定的Java对象。然而，如果攻击者在JSON字符串中添加了恶意代码，这个方法就会执行这段恶意代码。例如：

```java
String json = "{\"name\":\"Alice\",\"age\":20,\"maliciousCode\":\"System.out.println(\"Hello, World!\");\"}";  
User user = (User) deserialize(json, User.class);
```

在这个例子中，攻击者在JSON字符串中添加了恶意代码，然后将其传递给deserialize方法。当方法执行时，恶意代码将被执行。由于这个方法返回的是User对象，所以恶意代码将在User对象中执行。

为了防止Fastjson反序列化漏洞，我们需要对用户传输的数据进行严格过滤，并确保不加载恶意的Java类。同时，我们还需要及时更新Fastjson库，并避免在不安全的网络环境中使用含有Fastjson库的系统。

#### 正常解析：

```
package myJavaSecurity;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;


public class myFastJasonDemo {
    public static void main(String[] args) {
        String p= "{\"param1\":\"test1\",\"param2\":\"test2\"}";
        JSONObject jsonObject = JSON.parseObject(p);//将字符串转换为json对象
        System.out.println(jsonObject);
        System.out.println(jsonObject.getString("param1"));////获取其中一个叫param1的值
        
    }
}

```

![image-20231101221951652](image/image-20231101221951652.png)