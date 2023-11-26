# Fabrique abstraite

Propriétaire: Valentin

# Principe

La fabrique abstraite['<sup>2</sup>'](# Note et références) est un patron de conception qui permet de créer des familles d’objets apparentés sans préciser leur classe concrets.

C’est un design pattern qui ne permet non pas d’instancier différents objets de la même classe mère, mais pour instancier plusieurs objets de plusieurs classes mères.

Cela consiste donc a détacher toutes les méthodes de création des méthodes d’utilisation afin de rendre la maintenance et la compréhension des applications plus simple.

![[Reférence](https://www.notion.so/Fabrique-abstraite-8a49880840a3447585c6ca78c7756dc8?pvs=21)](Fabrique%20abstraite%208a49880840a3447585c6ca78c7756dc8/Fabrique_abstraite.jpg)

Reférence['<sup>1</sup>'](# Note et références)

---

# Pourquoi l’utiliser?

La fabrique abstraite fournit une interface pour créer des objets de chaque classe de la famille de produits.

Tant que le code crée des objets via cette interface, il n’y a aucune raison de s’inquiéter a crée une mauvaise variante d’u produit qui ne correspond pas au produit déjà crées par votre application.

La fabrique abstraite est souvent utilisée lorsque la création d'objets dans un système est complexe, que les objets créés sont liés entre eux, et que la flexibilité et l'extensibilité du système sont des priorités. Elle favorise la séparation des préoccupations et permet de concevoir des systèmes modulaires et évolutifs.

| Nom | Rôle |
| --- | --- |
| Fabrique (Factory) | Créer un objet dont le type dépend du contexte |
| Fabrique abstraite (abstract Factory) | Fournir une interface unique pour instancier des objets d'une même famille sans avoir à connaître les classes à instancier |
| Monteur (Builder) |   |
| Prototype (Prototype) | Création d'objet à partir d'un prototype |
| Singleton (Singleton) | Classe qui ne pourra avoir qu'une seule instance |

Reférence['<sup>5</sup>'](# Note et références)

# Utilisation dans différents domaines

La fabrique abstraite peut être définie par une famille de produits appartenant à un même thème. On peut prendre l'exemple d'une fabrique de téléphones : le client veut un téléphone, il s’attend à avoir un téléphone permettant de téléphoner, d’envoyer des messages, de consulter les réseaux, etc.

Cependant, il ne sait pas quelle différence existe entre les téléphones ; il souhaite un téléphone et se retrouve avec un 33 10, alors qu'il s'attendait à un téléphone moderne. Il a obtenu un téléphone, mais pas celui qu’il souhaitait.

La fabrique abstraite serait, en général, une classe nommée "Téléphone" dont les interfaces associées sont “téléphone moderne”, “téléphone à clapet”, “téléphone mobile”, “téléphone à cadran”, etc.

![Untitled](Fabrique%20abstraite%208a49880840a3447585c6ca78c7756dc8/Untitled.png)

(Exemple de l’abstract factory[<sup>3</sup>](# Note et références), l’abstract factory représente ici l’usine final, la spheric et la pyramidal factory des fils envoyant a l’usine mère (une usine de forme par exemple) les produits finaux)

---

# Exemples

Voici donc quelques exemples pour vous aider à comprendre ce modèle :

## Pseudo-code

Pour cette exemple['<sup>3</sup>'](# Note et références), nous allons utiliser des éléments pour une interface qui est multiplateforme, c’est-à-dire que l’application pourra fonctionner sous différent système d’exploitation sans problème.

![Untitled](Fabrique%20abstraite%208a49880840a3447585c6ca78c7756dc8/Untitled%201.png)

Dans un premier temps, nous allons créer la fabrique abstraite qui permettra de créer les éléments de notre interface, pour notre cas il n’y a que des boutons et des  cases à cocher. 

```visual-basic
interface GUIFactory is
    method createButton():Button
    method createCheckbox():Checkbox
```

La fabrique abstraite est donc une interface qui fournit les méthodes pour créer les éléments.

Nous allons ensuite mettre en place les fabriques concrètes, qui implémente la fabrique abstraite pour utiliser ces méthodes.

```visual-basic
//Fabrique concrète permettant de créer les éléments pour Windows
class WinFactory implements GUIFactory is
    method createButton():Button is
        return new WinButton()
    method createCheckbox():Checkbox is
        return new WinCheckbox()

//Fabrique concrète permettant de créer les éléments pour Mac
class MacFactory implements GUIFactory is
    method createButton():Button is
        return new MacButton()
    method createCheckbox():Checkbox is
        return new MacCheckbox()
```

Maintenant que nous avons créer les différentes fabriques, il faut mettre en place les différents éléments créer par ces fabriques.

On commence donc par le bouton :

```visual-basic
interface Button is
    method paint()

// Les produits concrets sont créés par les fabriques concrètes
// correspondantes.
class WinButton implements Button is
    method paint() is
        // Affiche un bouton avec le style Windows.

class MacButton implements Button is
    method paint() is
        // Affiche un bouton avec le style macOS.
```

Puis les cases à cocher :

```visual-basic
interface Checkbox is
    method paint()

class WinCheckbox implements Checkbox is
    method paint() is
        // Affiche une case à cocher avec le style Windows.

class MacCheckbox implements Checkbox is
    method paint() is
        // Affiche une case à cocher avec le style macOS.
```

Pour finir, on mets en place une classe Application, qui n’interagit qu’avec les fabriques abstraites et les objets abstraits. Cette approche permet d'envoyer au code client n'importe quelle sous-classe de fabrique ou de produit, sans avoir à modifier son code.

```visual-basic
class Application is
    private field factory: GUIFactory
    private field button: Button
    constructor Application(factory: GUIFactory) is
        this.factory = factory
    method createUI() is
        this.button = factory.createButton()
    method paint() is
        button.paint()
```

Pour que la classe application fonctionne correctement, on créer une classe ApplicationConfigurator qui permet donc de définir le système d’exploitation utiliser pour que l’on puisse utiliser les bonnes fabriques et les bons éléments.

```visual-basic
class ApplicationConfigurator is
    method main() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            factory = new WinFactory()
        else if (config.OS == "Mac") then
            factory = new MacFactory()
        else
            throw new Exception("Error! Unknown operating system.")

        Application app = new Application(factory)
```

---

## Java

Cet exemple['<sup>4</sup>'](# Note et références) implémente un système pour créer deux types d'ordinateurs, soit un pc, soit un serveur.

Dans un premier temps, la classe abstraite Computer représente la structure basique d’un ordinateur en regroupant de la RAM un disque dur et un processeur.

```java
public abstract class Computer {
	public abstract String getRAM();
	public abstract String getHDD();
	public abstract String getCPU();

	@Override
	public String toString(){
		return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
	}
}
```

Ensuite, les classes pc et serveur sont crées en reprenant la structure de l’ordinateur.

```java
public class PC extends Computer {

	private String ram;
	private String hdd;
	private String cpu;

	public PC(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public String getRAM() {
		return this.ram;
	}

	@Override
	public String getHDD() {
		return this.hdd;
	}

	@Override
	public String getCPU() {
		return this.cpu;
	}

}

```

```java
public class Server extends Computer {

	private String ram;
	private String hdd;
	private String cpu;

	public Server(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public String getRAM() {
		return this.ram;
	}

	@Override
	public String getHDD() {
		return this.hdd;
	}

	@Override
	public String getCPU() {
		return this.cpu;
	}

}
```

L’interface ComputerAbstractFactory sert de modèle pour créer des ordinateurs.

```java
public interface ComputerAbstractFactory {

	public Computer createComputer();

}
```

Ces deux classes représentent des usines permettant de créer différents types d’ordinateurs (pc ou serveur) tout en suivant le modèle définit par l’interface.

```java
public class PCFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public PCFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public Computer createComputer() {
		return new PC(ram,hdd,cpu);
	}

}
```

```java
public class ServerFactory implements ComputerAbstractFactory {

	private String ram;
	private String hdd;
	private String cpu;
	
	public ServerFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	
	@Override
	public Computer createComputer() {
		return new Server(ram,hdd,cpu);
	}

}

```

Pour finir, la classe ComputerFactory permet de simplifier la création d’ordinateur en utilisant les usines correspondantes au type d’ordinateur souhaité.

```java
public class ComputerFactory {

	public static Computer getComputer(ComputerAbstractFactory factory){
		return factory.createComputer();
	}
}
```

# Note et références

1. [https://www.codingame.com/playgrounds/36103/design-pattern-factory-abstract-factory/design-pattern-abstract-factory#:~:text=Le design pattern Abstract Factory,objets de plusieurs classes mères](https://www.codingame.com/playgrounds/36103/design-pattern-factory-abstract-factory/design-pattern-abstract-factory#:~:text=Le%20design%20pattern%20Abstract%20Factory,objets%20de%20plusieurs%20classes%20m%C3%A8res).
2. [https://blog.acensi.fr/le-pattern-factory-method/#:~:text=La Factory Method représente l,sera utilisée à la compilation](https://blog.acensi.fr/le-pattern-factory-method/#:~:text=La%20Factory%20Method%20repr%C3%A9sente%20l,sera%20utilis%C3%A9e%20%C3%A0%20la%20compilation).
3. [https://refactoring.guru/fr/design-patterns/abstract-factory](https://refactoring.guru/fr/design-patterns/abstract-factory)
4. [https://www.codingame.com/playgrounds/36103/design-pattern-factory-abstract-factory/exemple-abstract-factory](https://www.codingame.com/playgrounds/36103/design-pattern-factory-abstract-factory/exemple-abstract-factory)
5. [https://stackoverflow.com/questions/2280170/why-do-we-need-abstract-factory-design-pattern](https://stackoverflow.com/questions/2280170/why-do-we-need-abstract-factory-design-pattern)
