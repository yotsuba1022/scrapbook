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

