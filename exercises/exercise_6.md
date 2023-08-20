# Exercise 6 : Scheduled tasks

We are given a ``GhostRepository`` containing a list of ghosts. An ``Outbreak`` class that contains a list of ghots and a location. And a ``LocationsRepository`` interface.

Create a new (service) class that uses the Ghostrepository and the LocationRepository (will still have to be implented) to create a new outbreak once in a while.

* Once every few seconds/minutes/hours.
* Once on a specific point in time.

**Given:**

```java
public interface GhostRepository {
    List<Ghost> getRandomGhosts();
}

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
        // To be implemented.
    }
}
```

```java
public record Outbreak(String location, List<Ghost> ghosts) {
}
```

```java
public interface LocationRepository {
    
    String getRandomLocation();
}
```

---

* **[Solution](https://github.com/tvanwinckel/intro-spring-core/blob/main/exercises/solution/solution_6.md)**

---
