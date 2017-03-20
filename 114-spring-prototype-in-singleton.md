# Spring - Prototype in Singleton

問題: 若今天有個@Autowired bean \(此稱bean A\)存在於某個Singleton bean \(此稱bean B\)中, 且bean A必須為prototype bean, 請問該如何實現此種配置呢?

思路: 這裡可以考慮以下情境, 假設有個singleton bean \(bean B\), 此bean的角色是個controller, 然後在bean B中注入一個bean A \(@Autowired\), 而我們希望每次有request進到這個controller的時候, 這個bean A都會是一個新的bean \(即prototype bean\).

以下先定義Request:

```java
/**
 * @author Carl Lu
 */
public class Request {

    Long id;
    String content;

    public static Request getInstance(Long id, String content) {
        Request request = new Request();
        request.setId(id);
        request.setContent(content);
        return request;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```

再來是要被設定成prototype的bean A, 此處為validator:

```java
import java.util.ArrayList;
import java.util.List;

import org.springframework.context.annotation.Scope;
import org.springframework.context.annotation.ScopedProxyMode;
import org.springframework.stereotype.Component;

/**
 * @author Carl Lu
 */
@Component
@Scope(value = "prototype")
public class Validator {

    private List<String> validationMessages = new ArrayList<>();

    public Validator() {}

    public List<String> validate(Request request) {
        if (request.getContent() == null) {
            validationMessages.add("Request: " + request.getId() + " failed (no content).");
        }
        return validationMessages;
    }

}
```

然後是身為controller的singleton bean \(bean B\)

```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

/**
 * @author Carl Lu
 */
@Component
public class RequestHandler {

    @Autowired
    private Validator validator;

    public void handle(Request request) {
        List<String> validationMsgs = validator.validate(request);
        if (!validationMsgs.isEmpty()) {
            validationMsgs.stream().forEach(msg -> System.out.println(msg));
        } else {
            System.out.println("Validation passed for request: " + request.getId());
        }
        System.out.println("Validator: " + validator.toString());
    }

}
```

-&gt; 就只是一個要處理request的class而已, 不過其中會注入bean A \(prototype scope\)

寫個test case來驗證看看:

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.SpringApplicationConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.rga.RgaCustomerModuleApplication;

/**
 * @author Carl Lu
 */
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = RgaCustomerModuleApplication.class)
public class RequestHandlerTest {

    @Autowired
    private RequestHandler requestHandler;

    @Test
    public void testPrototype() {
        Request req1 = Request.getInstance(1l, "R1");
        Request req2 = Request.getInstance(2l, "R2");
        Request req3 = Request.getInstance(3l, null);
        Request req4 = Request.getInstance(4l, "R4");
        Request req5 = Request.getInstance(5l, null);

        requestHandler.handle(req1);
        requestHandler.handle(req2);
        requestHandler.handle(req3);
        requestHandler.handle(req4);
        requestHandler.handle(req5);
    }

}
```

結果:

![](/assets/1.14-001.png)

-&gt; 看起來光是把Validator設定成prototype scope是沒有用的, 因為都是同一個instance. 原因我想應該是因為在create RequestHandler的時候, container已經把RequestHandler默認為一個singleton bean了, 所以它也只會把validator bean注入一次並且重用validator bean.

所以現在來修改一下, 以下是改良過後的validator:

```java
import java.util.ArrayList;
import java.util.List;

import org.springframework.context.annotation.Scope;
import org.springframework.context.annotation.ScopedProxyMode;
import org.springframework.stereotype.Component;

/**
 * @author Carl Lu
 */
@Component
@Scope(proxyMode = ScopedProxyMode.TARGET_CLASS, value = "prototype")
public class Validator {

    private List<String> validationMessages = new ArrayList<>();

    public Validator() {}

    public List<String> validate(Request request) {
        if (request.getContent() == null) {
            validationMessages.add("Request: " + request.getId() + " failed (no content).");
        }
        return validationMessages;
    }

}
```

-&gt; 我後來查了一些資料, 我想這個解法是我覺得比較好看的, 首先要include cglib, 這邊的原理是利用AOP scoped proxies在每次RequestHandler被呼叫的時候都注入一個新的Validator bean

重新測試:![](/assets/1.14-002.png)-&gt; 這樣看起來是有達到效果了, 每個Validator看來都是一個新的instance.

