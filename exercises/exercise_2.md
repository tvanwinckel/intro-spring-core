# Exercise 2: DI with Spring Beans

We use the same setup as we did for exercise 1. But instead of instantiating our classes through normal Java construction. We will be using ``@Bean`` annotations instead. Beans can be constructed in the projects ``Main`` class.

We can use ``ApplicationContext`` to verify if our beans have been created.

**Given**

The following Gradle dependencies can be used:

```txt
>> build.gradle <<


plugins {
    id 'java'
}

group = 'com.tvanwinckel'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework:spring-core:6.0.11'
    implementation 'org.springframework:spring-context:6.0.11'
    implementation 'org.springframework:spring-beans:6.0.11'

    implementation 'joda-time:joda-time:2.10'

    testImplementation 'junit:junit:4.13.1'
    testImplementation group: 'org.assertj', name: 'assertj-core', version: '3.6.1'


    implementation('org.springframework:spring-context:6.0.11') {
        exclude group: 'spring.framework', module: 'commons-logging'
    }
}

test {
    useJUnitPlatform()
}
```

---

* **[Solution](https://github.com/tvanwinckel/intro-spring-core/blob/main/exercises/solutions/solution_2.md)**

---