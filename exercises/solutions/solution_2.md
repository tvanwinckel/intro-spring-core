# Solution


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