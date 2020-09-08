# Event Bus

## Introduction

This library allows passing events between different objects without them having a direct reference to each other.
Any class can be made an event by implementing the `IEvent` interface.

Using an instance of the `EventBus` class, an instant of the event class can be dispatched.
This means that it will be forwarded to all listeners registered for it at the event bus.

In addition, a singleton instance of the event bus is provided by the `EventBus#getInstance()` method.

To listen to events, register event handling methods using the `Event` annotation.
For this to work, the method must have a return type of `void` and declare a single parameter of the desired event type.
Additionally, the class containing the method must implement the `EventListener` interface.

## A Simple Example

Lets look at a simple example: we declare the empty class `SimpleEvent` that implements `IEvent` and can thus be used as an event.

```java
import dev.kske.eventbus.IEvent;

public class SimpleEvent implements IEvent {}
```

Next, an event listener for the `SimpleEvent` is declared:

```java
import dev.kske.eventbus.*;

public class SimpleEventListener implements EventListener {

    public SimpleEventListener() {

        // Register this listener at the event bus
        EventBus.getInstance().register(this);

        // Dispatch a SimpleEvent
        EventBus.getInstance().dispatch(new SimpleEvent());
    }

    @Event
    private void onSimpleEvent(SimpleEvent event) {
        System.out.println("SimpleEvent received!");
    }
}
```

In this case, an event bus is created and used locally. In a more sophisticated example the class would acquire an external event bus that is used by multiple classes.

## Installation

Event Bus is currently hosted at [kske.dev](https://kske.dev).
To include it inside your project, just add the Maven repository and the dependency to your `pom.xml`:

```xml
<repositories>
	<repository>
		<id>kske-repo</id>
		<url>https://kske.dev/maven-repo</url>
	</repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>dev.kske</groupId>
        <artifactId>event-bus</artifactId>
        <version>0.0.2</version>
    </dependency>
</dependencies>
```
