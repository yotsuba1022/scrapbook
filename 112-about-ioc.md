# Invesion of Control

IoC, 又稱控制反轉, 是一種提供配置元件\(component\)相依性與管理相依性的機制. 而IoC其實又可以分為下列幾種:

* Dependency Injection \(DI\)
  * Constructor Dependency Injection
  * Setter Dependency Injection
* Dependency Lookup
  * Dependency Pull
  * Contextualized Dependency Lookup \(CDL\)

在這兩種分類之中, dependency lookup是比較傳統的, 不過對大多數Java developer來說, 第一眼看到這種做法反而可能是比較親切的; 至於dependency injection, 則是比較新的方法, 甚至是有點反直覺的\(counterintuitive\), 不過後者事實上是比前者更為有彈性的.

對於dependency lookup來說, 一個元件必須獲得\(acquire\)相依元件\(dependency\)的參照\(reference\); 然而對dependency injection來說, 相依元件是被IoC container注入\(injected\)的.

以下就針對上面提到的這幾種IoC手段做簡單的說明:

#### Dependency Pull

**定義: 當需要某些相依性的時候, 從某個registry中去pull\(拉\). 這在EJB裡面應該算是很常見的做法 \(e.g. via JNDI API去查找一個EJB component\).**

在Spring裡面要做到這種方式也是可以的, 大概會長得像這樣:

```java
public static void main(String[] args) throws Exception {
    // get the bean factory
    BeanFactory factory = getBeanFactory();

    MessageRenderer mr = (MessageRenderer) factory.getBean("renderer");
    mr.render();
}
```

#### Contextualized Dependency Lookup \(CDL\)

**定義: 這跟dependency pull很像, 但對CDL來說, 查找\(look up\)這個動作是針對管理資源的container去執行的, 而不是從一些像是central registry之類的地方去找.**

CDL通常會要求component去實作一些特定的介面, 譬如說:

```java
public interface ManagedComponent {

    public void performLookup(Container container);

}
```

當實作了這個界面之後, component就可以跟container說: "我想要獲得某些dependency".

至於container, 通常會是由底層的appication server, 如Tomcat, JBoss等等或著是如Spring這類的framework來提供的, 而這些container通常都會提供跟dependency lookup service有關的介面:

```java
public interface Container {

    public Object getDependency(String key);

}
```

所以, 當container準備好可以提供component所需要的dependency時, 它會呼叫各個component上的performLookup\(\)方法, 因此component們就可以透過Container介面去獲得所需的dependencies.

```java
public class ContextualizedDependencyLookup implements ManagedComponent {

    private Dependency dependency;
    public void performLookup(Container container) {
        this.dependency = (Dependency) container.getDependency("myDependency");
    }

}
```

#### Constructor Dependency Injection



