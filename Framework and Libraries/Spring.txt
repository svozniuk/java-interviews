-----------------     Spring      -----------------

Basic idea of IoC pattern. Benefits.
    By applying DI in your projects, you�ll find that your code will
  become significantly simpler, easier to understand, and easier to test.
    With DI, objects are given their dependencies at creation time
  by some third party that coordinates each object in the system. Objects aren�t
  expected to create or obtain their dependencies�dependencies are injected into the
  objects that need them.
    The key benefit of DI�loose coupling. If an object only knows about its
  dependencies by their interface (not by their implementation or how they�re
  instantiated), then the dependency can be swapped out with a different
  implementation without the depending object knowing the difference.
    One of the most common ways that a dependency will be swapped out is with a
  mock implementation during testing.

What is Spring configuration file? How does it look like?
  <?xml version="1.0"encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" [...] >

    [...]
    <bean id="knight"class="com.springinaction.knights.BraveKnight"> <constructor-argref="quest"/> </bean>
    <bean id="quest" class="com.springinaction.knights.SlayDragonQuest"/>
    [...]

  </beans>

Out of the box bean scopes (singleton, prototype, request, session, global session)
  singleton       Scopes the bean definition to a single instance per Spring container (default).
  prototype       Allows a bean to be instantiated any number of times (once per use).
  request         Scopes a bean definition to an HTTP request. Only valid when used with a
                  web-capable Spring context (such as with Spring MVC).
  session         Scopes a bean definition to an HTTP session. Only valid when used with a
                  web-capable Spring context (such as with Spring MVC).
  global-session  Scopes a bean definition to a global HTTP session. Only valid when used in a portlet context.

What are the types of Dependency Injection Spring supports?
  Injecting through constructors
  Injecting into bean properties

Autowiring. Types of autowiring.
  byName      Attempts to match all properties of the autowired bean with beans
              that have the same name (or ID) as the properties. Properties for which there�s
              no matching bean will remain unwired.
  byType      Attempts to match all properties of the autowired bean with beans
              whose types are assignable to the properties. Properties for which there�s no
              matching bean will remain unwired.
  constructor Tries to match up a constructor of the autowired bean with
              beans whose types are assignable to the constructor arguments.
  autodetect  Attempts to apply constructor autowiring first. If that fails,
              byType will be tried.

What are inner beans.
  Inner beans are beans that are defined within the scope of another bean.
    Note that the inner beans don�t have an id attribute set. Though it�s perfectly legal
  to declare an ID for an inner bean, it�s not necessary because you�ll never refer to the
  inner bean by name. This highlights the main drawback of using inner beans: they
  can�t be reused. Inner beans are only useful for injection once and can�t be referred
  to by other beans.
    You may also find that using inner-bean definitions has a negative impact on the
  readability of the XML in the Spring context files.

What modules does Spring Framework have?
  Spring Framework Runtime
    DataAccess/Integration
      JDBC ORM OXM JMS
      Transactions
    Web (MVC/Remoting)
      Web Servlet Portlet Struts
    AOP Aspects Instumentation
    Core Container
      Beans Core Context ExpressionLanguage
    Test
    http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/overview.html

Could you please tell us about Spring? What is the most important feature(s) of Spring?
  Lightweight and minimally invasive development with plain old Java objects(POJOs)
  Loose coupling through dependency injection and interface orientation
  Declarative programming through aspects and common conventions
  Boilerplate reduction through aspects and templates

What are ORM�s Spring supports ?
  Spring provides support for several persistence frameworks, including Hibernate,
  iBATIS, Java Data Objects (JDO), and the Java Persistence API (JPA).

How to integrate Spring and Hibernate using HibernateDaoSupport?
  not reccomended http://stackoverflow.com/questions/5104765/hibernatedaosupport-is-not-recommended-why
  1)configure session factory
    <bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation. AnnotationSessionFactoryBean">
      <propertyname="dataSource"ref="dataSource"/>
      <propertyname="packagesToScan" value="com.habuma.spitter.domain"/>
      <propertyname="hibernateProperties">
        <props>
          <propkey="dialect">org.hibernate.dialect.HSQLDialect</prop>
        </props>
      </property>
    </bean>
  2)create dao class by extending HibernateDaoSupport
    class ExampleDao extends HibernateDaoSupport {
      ...
      List<aa> bb = (List<aa>)getHibernateTemplate().find("from cc");
    }

