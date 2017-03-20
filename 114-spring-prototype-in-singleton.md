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



