两个不兼容接口之间适配的桥梁
将一个接口转换成需求希望的另外一个接口，适配器模式使接口不兼容的那些类可以一起工作。
可以通过类结构（继承）来完成，也可以通过对象结构（组合）来完成，通常使用组合方式较多。
别名叫Wrapper，包装器
应用场景：
Tomcat中的Request流转
Spring AOP中的AdvisorAdapter
Spring MVC中的HandlerAdapter