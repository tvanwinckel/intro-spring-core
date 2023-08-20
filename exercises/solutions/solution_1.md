# Solution

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