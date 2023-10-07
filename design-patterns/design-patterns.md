## Design patterns

### Factory
Define an interface, for creating an object, but let sub-classes to handle instantiation.

### Builder
Instantiate a default object, add properties with methods, which return a reference to the object and finally build it with `build()` method.

### Singleton
A class, which can have only one instance of itself. Methods check if there is an existing instance and modify that, but otherwise they create it.

### Observer
A subject is a source of events and this subject has a list of observers, which get notified about new events.

### Iterator
Defines how values in an object can be iterated. 

### Strategy
An approach to modify the behavior of an object without directly changing it. A strategy (e.g. a class) is passed into the object, so the object can follow that strategy.

### Adapter
Allows the interface of an existing class to be used as another interface. Also known as the wrapper or the decorator patter. 

### Facade
A wrapper class used to abstract lower level details. For example HTTP-clients.
