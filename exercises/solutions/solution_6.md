
**Solution**

```java
@EnableScheduling
public class SpringCoreApplication {
    ...
}
```

```java
@Repository
public class GhostRepositoryImpl implements GhostRepository {

    private final List<Ghost> ghosts = new ArrayList<>();

    public GhostRepositoryImpl() {
        ghosts.add(new Ghost(1, "Cold Maiden"));
        ghosts.add(new Ghost(1, "Grey Haze"));
        ghosts.add(new Ghost(1, "Lurker"));
        ghosts.add(new Ghost(1, "Shade"));
        ghosts.add(new Ghost(2, "Dark Spectre"));
        ghosts.add(new Ghost(2, "Phantasm"));
        ghosts.add(new Ghost(2, "Raw Bones"));
        ghosts.add(new Ghost(2, "Poltergeist"));
    }

    @Override
    public List<Ghost> getRandomGhosts() {
        final Random rand = new Random();
        return IntStream.range(0, 4)
                .mapToObj(i -> ghosts.get(rand.nextInt(ghosts.size())))
                .toList();
    }
}
```

```java
@Repository
public class LocationRepositoryImpl implements LocationRepository {

    private final List<String> locations = new ArrayList<>();

    public LocationRepositoryImpl() {
        locations.add("Chelsea");
        locations.add("Vauxhall");
        locations.add("Kensington");
        locations.add("Stratford");
        locations.add("Paddington");
        locations.add("Greenwich");
    }

    @Override
    public String getRandomLocation() {
        final Random rand = new Random();
        return locations.get(rand.nextInt(locations.size()));
    }
}
```

```java
@Service
public class OutbreakService {

    private final GhostRepository ghostRepository;
    private final LocationRepository locationRepository;

    public OutbreakService(final GhostRepository ghostRepository, final LocationRepository locationRepository) {
        this.ghostRepository = ghostRepository;
        this.locationRepository = locationRepository;
    }

    @Scheduled(fixedDelay = 3000)
    public void start() {
        System.out.println("Starting a new outbreak!");
        final String location = locationRepository.getRandomLocation();
        final List<Ghost> ghosts = ghostRepository.getRandomGhosts();
        final Outbreak outbreak = new Outbreak(location, ghosts);

        System.out.println("New outbreak: " + outbreak);
    }
}
```