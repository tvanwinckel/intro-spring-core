# Exercise 1

Given are a couple of classes: ``Artefact``, ``GhostJar``, ``Ghost`` and ``StorageRoom``.

Now create a storageroom object, full of artefacts. Do this by using the IOC/DI principles in plain Java.

```java
public interface Artefact {
}
```

```java
public record RandomArtefact(String value) implements Artefact {
}


public class GhostJar implements Artefact{

    private final Ghost ghost;

    public GhostJar(final Ghost ghost) {
        this.ghost = ghost;
    }

    public int containsGhostType() {
        return ghost.type();
    }

    public String containsGhostWithName() {
        return ghost.name();
    }
}
```

```java
public record Ghost(int type, String name) {
}
```

```java
public record StorageRoom(List<Artefact> artefacts) {

    public List<GhostJar> getAllGhostJars() {
        return artefacts.stream()
                .filter(artefact -> artefact instanceof GhostJar)
                .map(artefact -> (GhostJar) artefact)
                .toList();
    }
}
```

**Extra:**

Create a GhostService that accepts a list of Ghosts. Enable the service to return all ghosts, ghosts of a secific type. And return the number of ghosts with a specific name.

Now instead of creating a ghost object to put into a ghost jar, use the service to fetch a certain ghost.


**Solution**

```java
public record StorageRoom(List<Artefact> artefacts) {

    public List<GhostJar> getAllGhostJars() {
        return artefacts.stream()
                .filter(artefact -> artefact instanceof GhostJar)
                .map(artefact -> (GhostJar) artefact)
                .toList();
    }

    public static void main(String[] args) {
        final Ghost poltergeist = new Ghost(2, "Poltergeist");
        final GhostJar jar = new GhostJar(poltergeist);

        System.out.println("Contains ghost of type: " + jar.containsGhostType());
        System.out.println("Contains ghost: " + jar.containsGhostWithName());


        final RandomArtefact silverLocket = new RandomArtefact("Silver locket");
        final RandomArtefact pirateHand = new RandomArtefact("Old severed pirate hand");

        List<Artefact> artefacts = List.of(jar, silverLocket, pirateHand);
        final StorageRoom storageRoom = new StorageRoom(artefacts);

        System.out.println(storageRoom.getAllGhostJars());
        System.out.println(storageRoom.artefacts());
    }
}
```
