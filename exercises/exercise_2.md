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
    implementation 'org.springframework:spring-core:5.2.19.RELEASE'
    implementation 'org.springframework:spring-context:5.2.21.RELEASE'
    implementation 'org.springframework:spring-beans:5.2.22.RELEASE'
    implementation 'joda-time:joda-time:2.10'

    testImplementation 'junit:junit:4.13.1'

    implementation('org.springframework:spring-context:5.2.21.RELEASE') {
        exclude group: 'spring.framework', module: 'commons-logging'
    }
}

test {
    useJUnitPlatform()
}
```

**Solution**


```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Running Application");

        final ApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
        context.getBeanDefinitionNames();

        Ghost ghotBeanByClass = context.getBean(Ghost.class);
        Ghost ghostBeanByName = (Ghost) context.getBean("poltergeist");
    }

    @Bean
    public Ghost poltergeist() {
        return new Ghost(2, "Poltergeist");
    }

    @Bean
    public GhostJar ghostJar(final Ghost poltergeist) {
        return new GhostJar(poltergeist);
    }

    @Bean
    public RandomArtefact silverLocket() {
        return new RandomArtefact("Silver locket");
    }

    @Bean
    public StorageRoom storageRoom(final GhostJar ghostJar, final RandomArtefact silverLocket) {
        final List<Artefact> artefacts = null;
        return new StorageRoom(artefacts);
    }
}
```