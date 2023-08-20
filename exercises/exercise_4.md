# Exercise 4: Spring Annotations

## Part 1 - Configuration and Class Annotations

Create a simple structure of some POJO classes, services and repositories. Preferebly use interfaces where possible/needed. Once you have these classes, wire them up in a DI fashion using ``@Beans``.

* Move all your beans into a configuration file. Apply the appropriate 'discovery' annotations where needed.
* Replace your configuration file with Spring class annotations and Autowired.

Having set up you application using both methods, which method do you prefer and why would you choose one above the other?

## Part 2 - Qualifiers

Create a second implementation of a service/repository you just created and configure/annotate it accordingly. 

* What would happen now if you run your application? 
* How can you fix it?

## Part 3 - Properties

Use one of your existing classes, or create a new class and have its fields populated by using properties, provided by a properties file.

___

**Part 1A - Solution**

```java
public record Agent(String name) {
}

public record Agency(String name, List<Agent> agents) {

}
```


```java
public interface AgencyRepository {

    List<Agency> getAllAgencies();
}


public class AgencyRepositoryImpl implements AgencyRepository{

    private final List<Agency> agencies = new ArrayList<>();

    {
        final Agent lucy = new Agent("Lucy Carlyle");
        final Agent anthony = new Agent("Anthony Lockwood");
        final Agent george = new Agent("George Cubbins");
        final Agency lockwood = new Agency("Lockwood & Co", List.of(lucy, anthony, george));
        agencies.add(lockwood);

        final Agent quill = new Agent("Quill Kipps");
        final Agent kat = new Agent("Kat Godwin");
        final Agency fittes = new Agency("Fittes", List.of(quill, kat));
        agencies.add(fittes);
    }

    @Override
    public List<Agency> getAllAgencies() {
        return agencies;
    }
}
```

```java
public interface AgencyService {
}

public class AgencyService {

    private final AgencyRepository repository;

    public AgencyService(final AgencyRepository repository) {
        this.repository = repository;
    }

    public Optional<Agency> findAgencyThatHasAgent(final Agent agent) {
        final List<Agency> agencies = repository.getAllAgencies();
        return agencies.stream()
                .filter(agency -> agency.agents().contains(agent))
                .findFirst();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Running Application");

        final ApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
        context.getBeanDefinitionNames();

        final AgencyService agencyService = context.getBean(AgencyService.class);

        System.out.println("Analysing...");
    }

    @Bean
    public AgencyService agencyService(final AgencyRepository agencyRepository) {
        return new AgencyServiceImpl(agencyRepository);
    }

    @Bean
    public AgencyRepository agencyRepository() {
        return new AgencyRepositoryImpl();
    }
}
```

**Part 1B - Solution**

```java
@Configuration
public class SpookyConfig {
    @Bean
    public AgencyService agencyService(final AgencyRepository agencyRepository) {
        return new AgencyServiceImpl(agencyRepository);
    }

    @Bean
    public AgencyRepository agencyRepository() {
        return new AgencyRepositoryImpl();
    }
}
```

```java
@ComponentScan
public class Main {
    public static void main(String[] args) {
        System.out.println("Running Application");

        final ApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
        context.getBeanDefinitionNames();

        final AgencyService agencyService = context.getBean(AgencyService.class);

        System.out.println("Analysing...");
    }
}
```

**Part 2 - Solution**

```java
@Repository("emptyRepository")
public class EmptyAgencyRepository implements AgencyRepository {

    ...
}

@Repository("filledRepository")
public class AgencyRepositoryImpl implements AgencyRepository{
    ...
}


@Service
public class AgencyService {

    private final AgencyRepository repository;

    @Autowired
    public AgencyService(@Qualifier("filledRepository") final AgencyRepository repository) {
        this.repository = repository;
    }

    ...
}
```

## Properties

Create a new class (bean). And have its fields populated by properties

Solution:

```java
@Component
@PropertySource(value = "classpath:application.properties")
public class TheProblem {

    private final String locationOfOrigin;
    private final String yearItStarted;
    private final String discoveredBy;

    public TheProblem(@Value("${locationOfOrigin}") final String locationOfOrigin,
                      @Value("${yearItStarted}") final String yearItStarted,
                      @Value("${discoveredBy}") final String discoveredBy) {
        this.locationOfOrigin = locationOfOrigin;
        this.yearItStarted = yearItStarted;
        this.discoveredBy = discoveredBy;
    }
}
```

```txt
locationOfOrigin=London
yearItStarted=1973
discoveredBy=Marissa Fittes
```