# Exercise 3

## Part 1 - Scopes: Singleton & Prototype

In this part of the exercise we will be examining the different scopes our beans can be given.

Select a bean you have created earlier and add a scope to it. Once you have added a scope, analyse its behaviour.

## Part 2 - Lifecycles

For this part of the exercise we are given a new class: ``AgencyService``. The class currently only has an empty list as field variable. Fill up this list after a bean the object has been created. And clear the list once the bean is being destroyed, using a beans lifecyle properties.

```java

public class AgencyService implements InitializingBean, DisposableBean {

    private final List<Agency> agencyRepository = new ArrayList<>();

}
```

---

* **[Solution](https://github.com/tvanwinckel/intro-spring-core/blob/main/exercises/solutions/solution_3.md)**

---
