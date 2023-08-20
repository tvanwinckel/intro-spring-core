# Solution
**Solution Part 1**

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Running Application");

        final ApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
        context.getBeanDefinitionNames();

        Ghost ghostBeanByClass = context.getBean(Ghost.class);
        Ghost ghostBeanByName = (Ghost) context.getBean("poltergeist");

        System.out.println("Analysing...");
    }

    @Bean
    @Scope("singleton")
    public Ghost poltergeist() {
        return new Ghost(2, "Poltergeist");
    }
```

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Running Application");

        final ApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
        context.getBeanDefinitionNames();

        Ghost ghostBeanByClass = context.getBean(Ghost.class);
        Ghost ghostBeanByName = (Ghost) context.getBean("poltergeist");

        System.out.println("Analysing...");
    }

    @Bean
    @Scope("prototype")
    public Ghost poltergeist() {
        return new Ghost(2, "Poltergeist");
    }
```

---

**Solution Part 2**

```java
@SpringBootApplication
public class SpringCoreApplication {

    public static void main(String[] args) {
        final ApplicationContext context = SpringApplication.run(SpringCoreApplication.class, args);

        final AgencyService agencyService = (AgencyService) context.getBean("agencyService");
    }

    @Bean
    public AgencyService agencyService() {
        return new AgencyService();
    }

    @Bean
    public CommandLineRunner run(final StorageRoom storageRoom) {
        return args -> {
            System.out.println(storageRoom.getAllGhostJars());
            System.out.println(storageRoom.artefacts());
        };
    }
}
```

```java
public class AgencyService implements InitializingBean, DisposableBean {

    private final List<Agency> agencyRepository = new ArrayList<>();

    @Override
    public void afterPropertiesSet() {
        // For this example, the repository is a simple list instead of an actual class.
        // The principle stays: for example the connections is being set up to the repo
        // before data comes available. In the meantime the service object has been created.

        System.out.println("Setting up the repository...");

        final Agency lucy = new Agency(List.of(new Agent("Lucy")));
        final Agency anthony = new Agency(List.of(new Agent("Anthony")));
        final Agency george = new Agency(List.of(new Agent("George")));
        agencyRepository.add(lucy);
        agencyRepository.add(anthony);
        agencyRepository.add(george);
    }

    @Override
    public void destroy() {
        System.out.println("Clearing the repository...");
        agencyRepository.clear();
    }
}
```
