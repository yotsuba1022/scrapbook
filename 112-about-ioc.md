# Invesion of Control

IoC, 又稱控制反轉, 是一種提供配置元件\(component\)相依性與管理相依性的機制. 而IoC其實又可以分為下列幾種:

* **Dependency Injection \(DI\)**
  * Constructor Dependency Injection
  * Setter Dependency Injection
* **Dependency Lookup**
  * Dependency Pull
  * Contextualized Dependency Lookup \(CDL\)

在這兩種分類之中, dependency lookup是比較傳統的, 不過對大多數Java developer來說, 第一眼看到這種做法反而可能是比較親切的; 至於dependency injection, 則是比較新的方法, 甚至是有點反直覺的\(counterintuitive\), 不過後者事實上是比前者更為有彈性的.

對於dependency lookup來說, 一個元件必須獲得\(acquire\)相依元件\(dependency\)的參照\(reference\); 然而對dependency injection來說, 相依元件是被IoC container注入\(**injected**\)的.

以下就針對上面提到的這幾種IoC機制做簡單的說明:

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

**定義: 這是DI\(Dependency Injection\)的一種, 透過建構子來提供dependencies. Component會把建構子中的參數視為其dependencies, 而IoC container會在初始化component的時候將這些dependencies傳遞進去.**

```java
public class ConstructorInjection {

    private Dependency dependency;

    public ConstructorInjection(Dependency dependency) {
        this.dependency = dependency;
    }


}
```

#### Setter Dependency Injection

**定義: DI的一種, IoC container會透過JavaBean-style的setter方法來注入dependency.**

```java
public class SetterInjection {

    private Dependency dependency;

    public void setDependency(Dependency dependency) {
        this.dependency = dependency;
    }

}
```

#### Injection v.s. Lookup

上面提了這麼多種IoC機制, 那該用哪個呢? 這其實不是很難的一件事, 主要就是看你現在用的container是哪種. 舉例來說, 如果你現在用的是EJB2.1或著是更早的版本, 那你大概就只能用look-up style的IoC了\(via JNDI\). 若是使用Spring, 基本上就是使用injection-style的IoC.

我們再來看一下dependency pull, 你可以發現這種做法基本上是要主動地\(actively\)去從registry獲取reference, 而CDL的方式則是會要求你的類別們去實作特定的介面並且查找dependencies.

再來, 看一下dependency injection, 基本上你只要允許dependency可以透過constructor或是setter進行注入即可, 這對你的component來說基本上是零影響的\(zero impact\).

所以在這裡你可以感覺得到, injection-style可以讓你的類別跟container解耦, 但lookup-style基本上還是會跟container有一定的關係, 如透過實作container指定的特定介面. 這基本上也會對component的測試難易度造成一定程度的影響, 這其實也是Spring後來會變得比較受歡迎的因素之一

結論, injeciton-style是屬於**被動的\(passive\)**去獲得dependency, 而lookup-style是屬於**主動的\(active\)**去獲得dependency; 而passive code相較於active code, 則相對易於維護.

