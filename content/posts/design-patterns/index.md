---
title: "Design Patterns"
date: "2025-02-09"
draft: false
summary: "This book gives you names and solutions for common problems in coding. It helps you talk about code with other programmers. It can help you make your code easier to reuse, fix, and change. But you can't just use them everywhere. You need to know **when** to use them."
tags: ["Design Patterns" , "Software Design", "C++", "OOP", "GoF", "Gang of Four", "Creational Design Patterns", "Structural Design Patterns", "Behavioral Design Patterns"]
---

# Design Patterns 

In software engineering, design patterns are reusable solutions to commonly occurring problems in software design. They provide a structured approach to solving specific design issues, helping developers create software that is more maintainable, scalable, and flexible. Design patterns are not specific to any programming language or technology but represent general principles and best practices applicable to various software development scenarios. They are like ready-made blueprints that can be altered to address persistent design issues in code. They ensure code reusability, scalability, and simple bug fixing.

[Christopher Alexander](https://en.wikipedia.org/wiki/Christopher_Alexander), an architect, introduced the idea of design patterns. They gained popularity in computer science with the book ["Design Patterns: Elements of Reusable Object-Oriented Software"](https://en.wikipedia.org/wiki/Design_Patterns) (1994) by [Erich Gamma](https://en.wikipedia.org/wiki/Erich_Gamma), [Richard Helm](https://en.wikipedia.org/wiki/Richard_Helm), [Ralph Johnson](https://en.wikipedia.org/wiki/Ralph_Johnson_(computer_scientist)), and [John Vlissides](https://en.wikipedia.org/wiki/John_Vlissides), known as the "Gang of Four" (GoF).
Design patterns in software engineering offer solutions to common design problems. They can be classified into creational, structural, and behavioral patterns.


## 1. **Creational Design Patterns**

Creational design patterns deal with object creation mechanisms, aiming to create objects in a way that suits the situation. Instead of directly creating objects, these patterns control the creation process, offering more flexibility and reusability. 

Creational patterns are divided into object-creational patterns (deal with object creation) and class-creational patterns (deal with class-instantiation).


### Examples of creational design patterns:

*  **Abstract Factory**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

```cpp
#include <iostream>

// Abstract Products
class AbstractProductA {
public:
    virtual ~AbstractProductA() {}
};

class AbstractProductB {
public:
    virtual void Interact(AbstractProductA* a) = 0;
    virtual ~AbstractProductB() {}
};

// Concrete Products
class ConcreteProductA1 : public AbstractProductA {};
class ConcreteProductA2 : public AbstractProductA {};

class ConcreteProductB1 : public AbstractProductB {
public:
    void Interact(AbstractProductA* a) override {
        std::cout << "ConcreteProductB1 interacts with ConcreteProductA1\n";
    }
};

class ConcreteProductB2 : public AbstractProductB {
public:
    void Interact(AbstractProductA* a) override {
        std::cout << "ConcreteProductB2 interacts with ConcreteProductA2\n";
    }
};

// Abstract Factory
class AbstractFactory {
public:
    virtual AbstractProductA* CreateProductA() = 0;
    virtual AbstractProductB* CreateProductB() = 0;
    virtual ~AbstractFactory() {}
};

// Concrete Factories
class ConcreteFactory1 : public AbstractFactory {
public:
    AbstractProductA* CreateProductA() override { return new ConcreteProductA1(); }
    AbstractProductB* CreateProductB() override { return new ConcreteProductB1(); }
};

class ConcreteFactory2 : public AbstractFactory {
public:
    AbstractProductA* CreateProductA() override { return new ConcreteProductA2(); }
    AbstractProductB* CreateProductB() override { return new ConcreteProductB2(); }
};

// Client
class Client {
public:
    Client(AbstractFactory* factory) : factory_(factory) {
        productA_ = factory_->CreateProductA();
        productB_ = factory_->CreateProductB();
    }

    void Run() {
        productB_->Interact(productA_);
    }

    ~Client() {
        delete productA_;
        delete productB_;
        delete factory_;
    }

private:
    AbstractFactory* factory_;
    AbstractProductA* productA_;
    AbstractProductB* productB_;
};

int main() {
    AbstractFactory* factory1 = new ConcreteFactory1();
    Client* client1 = new Client(factory1);
    client1->Run(); // Output: ConcreteProductB1 interacts with ConcreteProductA1
    delete client1;

    AbstractFactory* factory2 = new ConcreteFactory2();
    Client* client2 = new Client(factory2);
    client2->Run(); // Output: ConcreteProductB2 interacts with ConcreteProductA2
    delete client2;

    return 0;
}
```

*   **Builder**: Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

```cpp
#include <iostream>
#include <string>

// Product
class Product {
public:
    std::string part1;
    std::string part2;

    void show() {
        std::cout << "Part1: " << part1 << std::endl;
        std::cout << "Part2: " << part2 << std::endl;
    }
};

// Builder Interface
class Builder {
public:
    virtual void BuildPart1() = 0;
    virtual void BuildPart2() = 0;
    virtual Product* GetProduct() = 0;
    virtual ~Builder() {}
};

// Concrete Builder
class ConcreteBuilder : public Builder {
private:
    Product* product;

public:
    ConcreteBuilder() {
        product = new Product();
    }

    void BuildPart1() override {
        product->part1 = "Part1";
    }

    void BuildPart2() override {
        product->part2 = "Part2";
    }

    Product* GetProduct() override {
        return product;
    }
};

// Director
class Director {
private:
    Builder* builder;

public:
    Director(Builder* builder) : builder(builder) {}

    void Construct() {
        builder->BuildPart1();
        builder->BuildPart2();
    }
};

int main() {
    ConcreteBuilder* builder = new ConcreteBuilder();
    Director* director = new Director(builder);

    director->Construct();
    Product* product = builder->GetProduct();
    product->show();

    delete product;
    delete builder;
    delete director;

    return 0;
}
```

*   **Factory Method**: Allows a class to defer instantiation to subclasses.

```cpp
#include <iostream>
#include <string>

// Product Interface
class IceCream {
public:
    virtual std::string flavor() = 0;
    virtual ~IceCream() {}
};

// Concrete Products
class ChocolateIceCream : public IceCream {
public:
    std::string flavor() override {
        return "Chocolate";
    }
};

class VanillaIceCream : public IceCream {
public:
    std::string flavor() override {
        return "Vanilla";
    }
};

// Creator Interface
class IceCreamFactory {
public:
    virtual IceCream* createIceCream() = 0;
    virtual ~IceCreamFactory() {}
};

// Concrete Creators
class ChocolateIceCreamFactory : public IceCreamFactory {
public:
    IceCream* createIceCream() override {
        return new ChocolateIceCream();
    }
};

class VanillaIceCreamFactory : public IceCreamFactory {
public:
    IceCream* createIceCream() override {
        return new VanillaIceCream();
    }
};

int main() {
    IceCreamFactory* chocolateFactory = new ChocolateIceCreamFactory();
    IceCream* chocolateIceCream = chocolateFactory->createIceCream();
    std::cout << "Flavor: " << chocolateIceCream->flavor() << std::endl; // Output: Flavor: Chocolate
    delete chocolateIceCream;
    delete chocolateFactory;

    IceCreamFactory* vanillaFactory = new VanillaIceCreamFactory();
    IceCream* vanillaIceCream = vanillaFactory->createIceCream();
    std::cout << "Flavor: " << vanillaIceCream->flavor() << std::endl; // Output: Flavor: Vanilla
    delete vanillaIceCream;
    delete vanillaFactory;

    return 0;
}
```

*   **Object Pool**: Avoids expensive acquisition and release of resources by recycling objects that are no longer in use.

```cpp
#include <iostream>
#include <list>

class Resource {
    int value;

public:
    Resource() {
        value = 0;
    }

    void reset() {
        value = 0;
    }

    int getValue() {
        return value;
    }

    void setValue(int number) {
        value = number;
    }
};

/* Note, that this class is a singleton. */
class ObjectPool {
private:
    std::list<Resource*> resources;
    static ObjectPool* instance;
    ObjectPool() {}

public:
    /**
     * Static method for accessing class instance.
     * Part of Singleton design pattern.
     * @return ObjectPool instance.
     */
    static ObjectPool* getInstance() {
        if (instance == 0) {
            instance = new ObjectPool;
        }
        return instance;
    }

    /**
     * Returns instance of Resource.
     * New resource will be created if all the resources
     * were used at the time of the request.
     * @return Resource instance.
     */
    Resource* getResource() {
        if (resources.empty()) {
            std::cout << "Creating new." << std::endl;
            return new Resource;
        }
        else {
            std::cout << "Reusing existing." << std::endl;
            Resource* resource = resources.front();
            resources.pop_front();
            return resource;
        }
    }

    /**
     * Return resource back to the pool.
     * The resource must be initialized back to
     * the default settings before someone else
     * attempts to use it.
     * @param object Resource instance.
     * @return void
     */
    void returnResource(Resource* object) {
        object->reset();
        resources.push_back(object);
    }
};

ObjectPool* ObjectPool::instance = 0;

int main() {
    ObjectPool* pool = ObjectPool::getInstance();
    Resource* one;
    Resource* two;

    /* Resources will be created. */
    one = pool->getResource();
    one->setValue(10);
    std::cout << "one = " << one->getValue() << " [" << one << "]" << std::endl;
    two = pool->getResource();
    two->setValue(20);
    std::cout << "two = " << two->getValue() << " [" << two << "]" << std::endl;

    pool->returnResource(one);
    pool->returnResource(two);

    /* Resources will be reused.
     * Notice that the value of both resources were reset back to zero.
     */
    one = pool->getResource();
    std::cout << "one = " << one->getValue() << " [" << one << "]" << std::endl;
    two = pool->getResource();
    std::cout << "two = " << two->getValue() << " [" << two << "]" << std::endl;

    return 0;
}
```

*   **Prototype**: Creates new objects by cloning a prototypical instance.

```cpp
#include <iostream>
#include <string>

// Prototype class
class Document {
public:
    virtual Document* clone() const = 0;
    virtual void store() const = 0;
    virtual ~Document() {}
};

// Concrete prototypes
class xmlDoc : public Document {
public:
    Document* clone() const override { return new xmlDoc(); }
    void store() const override { std::cout << "xmlDoc\n"; }
};

class plainDoc : public Document {
public:
    Document* clone() const override { return new plainDoc(); }
    void store() const override { std::cout << "plainDoc\n"; }
};

class spreadsheetDoc : public Document {
public:
    Document* clone() const override { return new spreadsheetDoc(); }
    void store() const override { std::cout << "spreadsheetDoc\n"; }
};

// Client
int main() {
    Document* d1 = new xmlDoc();
    Document* d2 = d1->clone();
    d1->store();  // Output: xmlDoc
    d2->store();  // Output: xmlDoc

    delete d1;
    delete d2;

    return 0;
}

```

*   **Singleton**: Ensures that a class has only one instance and provides a global point of access to it.

```cpp
#include <iostream>

class Singleton {
private:
    static Singleton* instance;
    Singleton() {
        // Private constructor to prevent external instantiation
    }

public:
    static Singleton* getInstance() {
        if (!instance) {
            instance = new Singleton();
        }
        return instance;
    }

    void showMessage() {
        std::cout << "Singleton instance is working!\n";
    }

    // Delete copy constructor and assignment operator
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

Singleton* Singleton::instance = nullptr;

int main() {
    Singleton* singleton = Singleton::getInstance();
    singleton->showMessage(); // Output: Singleton instance is working!

    Singleton* anotherSingleton = Singleton::getInstance();
    if (singleton == anotherSingleton) {
        std::cout << "Both instances are the same.\n";  //This will be printed
    }

    return 0;
}
```

**Prototype**

```cpp
#include <iostream>
#include <string>

// Prototype Interface
class Prototype {
public:
    virtual Prototype* clone() = 0;
    virtual std::string getName() = 0;
    virtual ~Prototype() {}
};

// Concrete Prototype
class ConcretePrototype : public Prototype {
private:
    std::string name_;

public:
    ConcretePrototype(std::string name) : name_(name) {}

    Prototype* clone() override {
        return new ConcretePrototype(name_);
    }

    std::string getName() override {
        return name_;
    }
};

int main() {
    ConcretePrototype* prototype = new ConcretePrototype("Original");
    Prototype* clone = prototype->clone();

    std::cout << "Original: " << prototype->getName() << std::endl; // Output: Original: Original
    std::cout << "Clone: " << clone->getName() << std::endl; // Output: Clone: Original

    delete prototype;
    delete clone;

    return 0;
}
```


## 2. **Structural Design Patterns**

Structural design patterns focus on how classes and objects are organized to form larger structures, while maintaining flexibility and efficiency. They simplify design by identifying ways to realize relationships between entities.

### Examples of structural design patterns:

*   **Adapter**: Allows objects with incompatible interfaces to collaborate. The Adapter pattern allows objects with incompatible interfaces to work together. It acts as a wrapper, translating the interface of one class into an interface that another class expects.

```cpp
#include <iostream>

// Target interface
class Target {
public:
    virtual void request() {
        std::cout << "Target: The default target's behavior.\n";
    }
    virtual ~Target(){}
};

// Adaptee class
class Adaptee {
public:
    void specificRequest() {
        std::cout << "Adaptee: The adaptee's specific behavior.\n";
    }
};

// Adapter class
class Adapter : public Target {
private:
    Adaptee* adaptee;
public:
    Adapter(Adaptee* adaptee) : adaptee(adaptee) {}

    void request() override {
        std::cout << "Adapter: (TRANSLATED) ";
        adaptee->specificRequest();
    }
};

int main() {
    Adaptee* adaptee = new Adaptee();
    Target* target = new Adapter(adaptee);

    target->request(); // Output: Adapter: (TRANSLATED) Adaptee: The adaptee's specific behavior.

    delete adaptee;
    delete target;
    return 0;
}
```

*   **Bridge**: Decouples an abstraction from its implementation so that the two can vary independently. The Bridge pattern decouples an abstraction from its implementation, allowing the two to vary independently. It involves an interface that acts as a bridge, making the concrete classes independent from the interface class.

```cpp
#include <iostream>
#include <string>

// Implementor interface
class DrawingAPI {
public:
    virtual void drawCircle(int x, int y, int radius) = 0;
    virtual ~DrawingAPI() {}
};

// Concrete Implementors
class DrawingAPI1 : public DrawingAPI {
public:
    void drawCircle(int x, int y, int radius) override {
        std::cout << "API1.circle at " << x << ":" << y << " radius " << radius << std::endl;
    }
};

class DrawingAPI2 : public DrawingAPI {
public:
    void drawCircle(int x, int y, int radius) override {
        std::cout << "API2.circle at " << x << ":" << y << " radius " << radius << std::endl;
    }
};

// Abstraction
class Shape {
public:
    Shape(DrawingAPI* drawingAPI) : drawingAPI(drawingAPI) {}
    virtual void draw() = 0;
    virtual void resizeByPercentage(double pct) = 0;
    virtual ~Shape() {}

protected:
    DrawingAPI* drawingAPI;
};

// Refined Abstraction
class CircleShape : public Shape {
public:
    CircleShape(int x, int y, int radius, DrawingAPI* drawingAPI) : Shape(drawingAPI), x(x), y(y), radius(radius) {}

    void draw() override {
        drawingAPI->drawCircle(x, y, radius);
    }

    void resizeByPercentage(double pct) override {
        radius *= pct;
    }

private:
    int x, y, radius;
};

int main() {
    DrawingAPI1* api1 = new DrawingAPI1();
    CircleShape* circle1 = new CircleShape(1, 2, 3, api1);
    circle1->draw(); // Output: API1.circle at 1:2 radius 3

    DrawingAPI2* api2 = new DrawingAPI2();
    CircleShape* circle2 = new CircleShape(5, 7, 11, api2);
    circle2->draw(); // Output: API2.circle at 5:7 radius 11

    delete api1;
    delete circle1;
    delete api2;
    delete circle2;

    return 0;
}
```

*   **Composite**: Composes objects into tree structures to represent part-whole hierarchies. The Composite pattern lets you compose objects into tree structures and then work with these structures as if they were individual objects.

```cpp
#include <iostream>
#include <vector>

// Component interface
class Component {
public:
    virtual void operation() = 0;
    virtual void add(Component* component) {}
    virtual void remove(Component* component) {}
    virtual ~Component() {}
};

// Leaf class
class Leaf : public Component {
public:
    void operation() override {
        std::cout << "Leaf\n";
    }
};

// Composite class
class Composite : public Component {
private:
    std::vector<Component*> children;

public:
    void add(Component* component) override {
        children.push_back(component);
    }

    void remove(Component* component) override {
        for (int i = 0; i < children.size(); ++i) {
            if (children[i] == component) {
                children.erase(children.begin() + i);
                break;
            }
        }
    }

    void operation() override {
        std::cout << "Composite\n";
        for (Component* child : children) {
            child->operation();
        }
    }
};

int main() {
    Composite* composite = new Composite();
    Leaf* leaf1 = new Leaf();
    Leaf* leaf2 = new Leaf();
    composite->add(leaf1);
    composite->add(leaf2);
    composite->operation();
    // Output:
    // Composite
    // Leaf
    // Leaf

    delete composite;
    delete leaf1;
    delete leaf2;

    return 0;
}
```

*   **Decorator**: Adds additional functionality to an object dynamically. The Decorator pattern lets you add new behaviors to objects by placing them inside special wrapper objects that contain the behaviors.

```cpp
#include <iostream>
#include <string>

// Component interface
class Component {
public:
    virtual std::string operation() const = 0;
    virtual ~Component() {}
};

// Concrete Component
class ConcreteComponent : public Component {
public:
    std::string operation() const override {
        return "ConcreteComponent";
    }
};

// Decorator class
class Decorator : public Component {
protected:
    Component* component;

public:
    Decorator(Component* component) : component(component) {}

    std::string operation() const override {
        return component->operation();
    }
};

// Concrete Decorators
class ConcreteDecoratorA : public Decorator {
public:
    ConcreteDecoratorA(Component* component) : Decorator(component) {}

    std::string operation() const override {
        return "ConcreteDecoratorA(" + Decorator::operation() + ")";
    }
};

class ConcreteDecoratorB : public Decorator {
public:
    ConcreteDecoratorB(Component* component) : Decorator(component) {}

    std::string operation() const override {
        return "ConcreteDecoratorB(" + Decorator::operation() + ")";
    }
};

int main() {
    Component* component = new ConcreteComponent();
    ConcreteDecoratorA* decoratorA = new ConcreteDecoratorA(component);
    ConcreteDecoratorB* decoratorB = new ConcreteDecoratorB(decoratorA);

    std::cout << "Result: " << decoratorB->operation() << std::endl;
    // Output: Result: ConcreteDecoratorB(ConcreteDecoratorA(ConcreteComponent))

    delete component;
    delete decoratorA;
    delete decoratorB;

    return 0;
}
```

*   **Facade**: Provides a simplified interface to a complex subsystem.

```cpp
#include <iostream>

// Subsystem classes
class SubsystemA {
public:
    void operationA() {
        std::cout << "Subsystem A operation\n";
    }
};

class SubsystemB {
public:
    void operationB() {
        std::cout << "Subsystem B operation\n";
    }
};

class SubsystemC {
public:
    void operationC() {
        std::cout << "Subsystem C operation\n";
    }
};

// Facade class
class Facade {
private:
    SubsystemA* subsystemA;
    SubsystemB* subsystemB;
    SubsystemC* subsystemC;

public:
    Facade() {
        subsystemA = new SubsystemA();
        subsystemB = new SubsystemB();
        subsystemC = new SubsystemC();
    }
    ~Facade() {
        delete subsystemA;
        delete subsystemB;
        delete subsystemC;
    }

    void operation1() {
        std::cout << "Facade operation 1:\n";
        subsystemA->operationA();
        subsystemB->operationB();
    }

    void operation2() {
        std::cout << "Facade operation 2:\n";
        subsystemB->operationB();
        subsystemC->operationC();
    }
};

int main() {
    Facade* facade = new Facade();
    facade->operation1();
    // Output:
    // Facade operation 1:
    // Subsystem A operation
    // Subsystem B operation
    facade->operation2();
    // Output:
    // Facade operation 2:
    // Subsystem B operation
    // Subsystem C operation
    delete facade;
    return 0;
}
```

*   **Flyweight**: Uses sharing to support a large number of fine-grained objects efficiently.The Flyweight pattern lets you fit more objects into the available amount of RAM by sharing common parts of the state between multiple objects instead of keeping all of the data in each object.

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

// Intrinsic state
class Character {
public:
    virtual void display(int pointSize) = 0;
    virtual ~Character() {}
};

// Concrete Flyweight
class ConcreteCharacter : public Character {
private:
    char character;
public:
    ConcreteCharacter(char character) : character(character) {}

    void display(int pointSize) override {
        std::cout << character << " (pointSize: " << pointSize << ")" << std::endl;
    }
};

// Flyweight Factory
class CharacterFactory {
private:
    std::unordered_map<char, Character*> characters;

public:
    Character* getCharacter(char character) {
        if (characters.find(character) == characters.end()) {
            characters[character] = new ConcreteCharacter(character);
        }
        return characters[character];
    }
};

int main() {
    CharacterFactory factory;
    int fontSize = 12;

    Character* a = factory.getCharacter('A');
    a->display(fontSize);  // Output: A (pointSize: 12)

    Character* b = factory.getCharacter('B');
    b->display(fontSize);  // Output: B (pointSize: 12)

    Character* a2 = factory.getCharacter('A');
    a2->display(fontSize + 2);  // Output: A (pointSize: 14)

    return 0;
}
```

*   **Proxy**: Provides a substitute or placeholder for another object to control access to it.
The Proxy pattern provides a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

```cpp
#include <iostream>

// Subject interface
class Image {
public:
    virtual void display() = 0;
    virtual ~Image() {}
};

// Real Subject
class RealImage : public Image {
private:
    std::string filename;

public:
    RealImage(const std::string& filename) : filename(filename) {
        loadFromDisk();
    }

    void display() override {
        std::cout << "Displaying " << filename << std::endl;
    }

private:
    void loadFromDisk() {
        std::cout << "Loading " << filename << std::endl;
    }
};

// Proxy
class ProxyImage : public Image {
private:
    RealImage* realImage;
    std::string filename;

public:
    ProxyImage(const std::string& filename) : filename(filename), realImage(nullptr) {}

    void display() override {
        if (realImage == nullptr) {
            realImage = new RealImage(filename);
        }
        realImage->display();
    }
};

int main() {
    ProxyImage* image1 = new ProxyImage("test_image.jpg");

    // Image will be loaded from disk when display is called
    image1->display();
    // Output:
    // Loading test_image.jpg
    // Displaying test_image.jpg

    ProxyImage* image2 = new ProxyImage("test_image.jpg");
    // Image will not be loaded from disk as it was already loaded
    image2->display();
    // Output: Displaying test_image.jpg

    return 0;
}
```


## 3. **Behavioral Design Patterns**

Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects. They identify common communication patterns between objects and increase flexibility in carrying out this communication.

### Examples of behavioral design patterns:

*   **Chain of Responsibility**: Passes requests along a chain of handlers. The Chain of Responsibility pattern lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain. This avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request.

```cpp
#include <iostream>

class Handler {
protected:
    Handler *next;
public:
    Handler() { next = NULL; }
    virtual ~Handler() { }
    virtual void request(int value) = 0;
    void setNextHandler(Handler *nextInLine) { next = nextInLine; }
};

class SpecialHandler : public Handler {
private:
    int myLimit;
    int myId;
public:
    SpecialHandler(int limit, int id) {
        myLimit = limit;
        myId = id;
    }
    ~SpecialHandler() { }
    void request(int value) {
        if(value < myLimit) {
            std::cout << "Handler " << myId << " handled the request with a limit of " << myLimit << std::endl;
        } else if(next != NULL) {
            next->request(value);
        } else {
            std::cout << "Sorry, I am the last handler (" << myId << ") and I can't handle the request." << std::endl;
        }
    }
};

int main () {
    Handler *h1 = new SpecialHandler(10, 1);
    Handler *h2 = new SpecialHandler(20, 2);
    Handler *h3 = new SpecialHandler(30, 3);
    h1->setNextHandler(h2);
    h2->setNextHandler(h3);
    h1->request(18);
    h1->request(40);
    delete h1;
    delete h2;
    delete h3;
    return 0;
}
```

*   **Command**: Encapsulates a request as an object, allowing for parameterization of clients with queues, requests, and operations. The Command pattern turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as method arguments, delay or queue a request's execution, and support undoable operations. The pattern decouples sender and receiver by encapsulating a request as an object.

```cpp
#include <iostream>
#include <vector>

// Receiver class
class Light {
public:
    void turnOn() {
        std::cout << "Light is on\n";
    }
    void turnOff() {
        std::cout << "Light is off\n";
    }
};

// Command interface
class Command {
public:
    virtual ~Command() {}
    virtual void execute() = 0;
};

// Concrete Command
class TurnOnCommand : public Command {
private:
    Light* light;
public:
    TurnOnCommand(Light* light) : light(light) {}
    void execute() override {
        light->turnOn();
    }
};

class TurnOffCommand : public Command {
private:
    Light* light;
public:
    TurnOffCommand(Light* light) : light(light) {}
    void execute() override {
        light->turnOff();
    }
};

// Invoker
class RemoteControl {
private:
    Command* command;
public:
    void setCommand(Command* command) {
        this->command = command;
    }
    void pressButton() {
        command->execute();
    }
};

int main() {
    Light* light = new Light();
    TurnOnCommand* turnOn = new TurnOnCommand(light);
    TurnOffCommand* turnOff = new TurnOffCommand(light);

    RemoteControl* remote = new RemoteControl();
    remote->setCommand(turnOn);
    remote->pressButton(); // Output: Light is on

    remote->setCommand(turnOff);
    remote->pressButton(); // Output: Light is off

    delete light;
    delete turnOn;
    delete turnOff;
    delete remote;

    return 0;
}
```

*   **Interpreter**: Provides a way to include language elements in a program.

```cpp
#include <iostream>
#include <string>
#include <map>

// Forward declaration of Expression
struct Expression {
    virtual int interpret(std::map<std::string, Expression*> &variables) = 0;
    virtual ~Expression() {}
};

// Number class
class Number : public Expression {
private:
    int number;

public:
    Number(int number) { this->number = number; }
    int interpret(std::map<std::string, Expression*> &variables) { return number; }
    ~Number() {}
};

// Plus class
class Plus : public Expression {
    Expression* leftOperand;
    Expression* rightOperand;

public:
    Plus(Expression* left, Expression* right) {
        leftOperand = left;
        rightOperand = right;
    }
    ~Plus() {
        delete leftOperand;
        delete rightOperand;
    }
    int interpret(std::map<std::string, Expression*> &variables) {
        return leftOperand->interpret(variables) + rightOperand->interpret(variables);
    }
};

// Minus class
class Minus : public Expression {
    Expression* leftOperand;
    Expression* rightOperand;

public:
    Minus(Expression* left, Expression* right) {
        leftOperand = left;
        rightOperand = right;
    }
    ~Minus() {
        delete leftOperand;
        delete rightOperand;
    }
    int interpret(std::map<std::string, Expression*> &variables) {
        return leftOperand->interpret(variables) - rightOperand->interpret(variables);
    }
};

// Variable class
class Variable : public Expression {
    std::string name;

public:
    Variable(std::string name) { this->name = name; }
    int interpret(std::map<std::string, Expression*> &variables) {
        if (variables.find(name) == variables.end()) return 0;
        return variables[name]->interpret(variables);
    }
};

int main() {
    // Build the expression: w + x - y
    std::map<std::string, Expression*> variables;

    Variable* w = new Variable("w");
    Variable* x = new Variable("x");
    Variable* y = new Variable("y");

    Plus* plus = new Plus(w, x);
    Minus* expression = new Minus(plus, y);

    // Set the variables
    variables["w"] = new Number(5);
    variables["x"] = new Number(10);
    variables["y"] = new Number(3);

    int result = expression->interpret(variables);
    std::cout << "Result: " << result << std::endl;  // Output: Result: 12

    // Cleanup
    delete expression;
    delete plus;
    delete w;
    delete x;
    delete y;
    for (auto const& [key, val] : variables) {
        delete val;
    }

    return 0;
}
```

*   **Iterator**: Provides a way to access the elements of a collection sequentially without exposing its underlying representation.

```cpp
#include <iostream>
#include <vector>

// Aggregate interface
class Aggregate {
public:
    virtual ~Aggregate() {}
    virtual class Iterator* createIterator() = 0;
};

// Iterator interface
class Iterator {
public:
    virtual ~Iterator() {}
    virtual void first() = 0;
    virtual void next() = 0;
    virtual bool isDone() = 0;
    virtual int currentItem() = 0;
};

// Concrete Aggregate
class ConcreteAggregate : public Aggregate {
private:
    std::vector<int> collection;
public:
    ConcreteAggregate() {
        collection = {1, 2, 3, 4, 5};
    }
    Iterator* createIterator() override;

    int getItem(int index) { return collection[index]; }
    int size() { return collection.size(); }
};

// Concrete Iterator
class ConcreteIterator : public Iterator {
private:
    ConcreteAggregate* aggregate;
    int current;
public:
    ConcreteIterator(ConcreteAggregate* aggregate) : aggregate(aggregate) {
        current = 0;
    }

    void first() override { current = 0; }
    void next() override { current++; }
    bool isDone() override { return current >= aggregate->size(); }
    int currentItem() override { return aggregate->getItem(current); }
};

Iterator* ConcreteAggregate::createIterator() {
    return new ConcreteIterator(this);
}

int main() {
    ConcreteAggregate* aggregate = new ConcreteAggregate();
    Iterator* iterator = aggregate->createIterator();

    for (iterator->first(); !iterator->isDone(); iterator->next()) {
        std::cout << iterator->currentItem() << std::endl;
    }
    // Output:
    // 1
    // 2
    // 3
    // 4
    // 5

    delete aggregate;
    delete iterator;

    return 0;
}
```

*   **Mediator**: Defines simplified communication between classes. The Mediator pattern lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

```cpp
#include <iostream>
#include <string>

class Mediator;

// Colleague interface
class Colleague {
protected:
    Mediator* mediator;
public:
    Colleague(Mediator* mediator) : mediator(mediator) {}
    virtual void receive(const std::string& message) = 0;
    virtual void send(const std::string& message) = 0;
    virtual ~Colleague() {}
};

// Mediator interface
class Mediator {
public:
    virtual void sendMessage(const std::string& message, Colleague* colleague) = 0;
    virtual ~Mediator() {}
};

// Concrete Colleague
class ConcreteColleague1 : public Colleague {
public:
    ConcreteColleague1(Mediator* mediator) : Colleague(mediator) {}
    void receive(const std::string& message) override {
        std::cout << "Colleague1 received: " << message << std::endl;
    }
    void send(const std::string& message) override {
        std::cout << "Colleague1 sends: " << message << std::endl;
        mediator->sendMessage(message, this);
    }
};

class ConcreteColleague2 : public Colleague {
public:
    ConcreteColleague2(Mediator* mediator) : Colleague(mediator) {}
    void receive(const std::string& message) override {
        std::cout << "Colleague2 received: " << message << std::endl;
    }
    void send(const std::string& message) override {
        std::cout << "Colleague2 sends: " << message << std::endl;
        mediator->sendMessage(message, this);
    }
};

// Concrete Mediator
class ConcreteMediator : public Mediator {
private:
    ConcreteColleague1* colleague1;
    ConcreteColleague2* colleague2;

public:
    void setColleague1(ConcreteColleague1* colleague1) { this->colleague1 = colleague1; }
    void setColleague2(ConcreteColleague2* colleague2) { this->colleague2 = colleague2; }

    void sendMessage(const std::string& message, Colleague* colleague) override {
        if (colleague == colleague1) {
            colleague2->receive(message);
        } else {
            colleague1->receive(message);
        }
    }
};

int main() {
    ConcreteMediator* mediator = new ConcreteMediator();
    ConcreteColleague1* c1 = new ConcreteColleague1(mediator);
    ConcreteColleague2* c2 = new ConcreteColleague2(mediator);

    mediator->setColleague1(c1);
    mediator->setColleague2(c2);

    c1->send("Hello from Colleague1");
    // Output: Colleague1 sends: Hello from Colleague1
    //         Colleague2 received: Hello from Colleague1

    c2->send("Hi from Colleague2");
    // Output: Colleague2 sends: Hi from Colleague2
    //         Colleague1 received: Hi from Colleague2

    delete mediator;
    delete c1;
    delete c2;

    return 0;
}
```

*   **Memento**: Captures and externalizes an object's internal state so it can be restored later. The Memento pattern lets you save and restore the previous state of an object without revealing the details of its implementation. The memento pattern is used to implement the undo operation i.e., it allows us to save and restore an object to its previous state. It wraps the entire object state in a single object known as the Memento that allows the entire state to be saved and restored in a single action.

```cpp
#include <iostream>
#include <string>

// Memento class
class Memento {
private:
    std::string state;
public:
    Memento(std::string state) : state(state) {}
    std::string getState() const { return state; }
};

// Originator class
class Originator {
private:
    std::string state;
public:
    void setState(std::string state) {
        std::cout << "Originator: Setting state to " << state << std::endl;
        this->state = state;
    }
    Memento* saveToMemento() {
        std::cout << "Originator: Saving to Memento.\n";
        return new Memento(state);
    }
    void restoreFromMemento(Memento* memento) {
        state = memento->getState();
        std::cout << "Originator: State after restoring from Memento: " << state << std::endl;
    }
};

// Caretaker
class Caretaker {
private:
    std::vector<Memento*> mementos;
public:
    void addMemento(Memento* m) {
        mementos.push_back(m);
    }
    Memento* getMemento(int index) {
        return mementos[index];
    }
};

int main() {
    Originator* originator = new Originator();
    Caretaker* caretaker = new Caretaker();

    originator->setState("State #1");
    caretaker->addMemento(originator->saveToMemento());

    originator->setState("State #2");
    caretaker->addMemento(originator->saveToMemento());

    originator->setState("State #3");
    std::cout << "Current State: " << std::endl;

    originator->restoreFromMemento(caretaker->getMemento(1));
    //Output
    //Originator: Setting state to State #1
    //Originator: Saving to Memento.
    //Originator: Setting state to State #2
    //Originator: Saving to Memento.
    //Originator: Setting state to State #3
    //Current State:
    //Originator: State after restoring from Memento: State #2
    return 0;
}
```

*   **Observer**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified. The Observer pattern lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing. The Observer pattern provides a way to subscribe and unsubscribe to and from these events for any object that implements a subscriber interface. It is pretty common in C++ code, especially in the GUI components. It provides a way to react to events happening in other objects without coupling to their classes. The Observer pattern is a behavioral pattern that is used to monitor the state of multiple objects or classes. It acts as a notifier of change to multiple classes.

```cpp
#include <iostream>
#include <vector>
#include <string>

class Observer;

// Subject (Observable)
class Subject {
public:
    virtual ~Subject() {}
    virtual void attach(Observer* observer) = 0;
    virtual void detach(Observer* observer) = 0;
    virtual void notify() = 0;

protected:
    std::vector<Observer*> observers;
};

// Observer interface
class Observer {
public:
    virtual ~Observer() {}
    virtual void update(Subject* subject) = 0;
};

// Concrete Subject
class ConcreteSubject : public Subject {
private:
    std::string state;
public:
    void attach(Observer* observer) override {
        observers.push_back(observer);
    }
    void detach(Observer* observer) override {
        for (int i = 0; i < observers.size(); ++i) {
            if (observers[i] == observer) {
                observers.erase(observers.begin() + i);
                break;
            }
        }
    }
    void notify() override {
        for (Observer* observer : observers) {
            observer->update(this);
        }
    }
    void setState(std::string state) {
        this->state = state;
        notify();
    }
    std::string getState() const { return state; }
};

// Concrete Observers
class ConcreteObserver : public Observer {
private:
    std::string name;
    std::string observerState;
public:
    ConcreteObserver(std::string name) : name(name) {}
    void update(Subject* subject) override {
        observerState = dynamic_cast<ConcreteSubject*>(subject)->getState();
        std::cout << "Observer " << name << "'s new state is: " << observerState << std::endl;
    }
};

int main() {
    ConcreteSubject* subject = new ConcreteSubject();
    ConcreteObserver* observer1 = new ConcreteObserver("X");
    ConcreteObserver* observer2 = new ConcreteObserver("Y");

    subject->attach(observer1);
    subject->attach(observer2);

    subject->setState("ABC");
    // Output:
    // Observer X's new state is: ABC
    // Observer Y's new state is: ABC

    subject->detach(observer1);

    subject->setState("123");
    // Output:
    // Observer Y's new state is: 123

    delete subject;
    delete observer1;
    delete observer2;

    return 0;
}
```

*   **State**: Allows an object to alter its behavior when its internal state changes.

```cpp
#include <iostream>

// Context
class Context;

// State
class State {
public:
    virtual ~State() {}
    virtual void handle(Context* context) = 0;
};

// Concrete States
class ConcreteStateA : public State {
public:
    void handle(Context* context) override;
};

class ConcreteStateB : public State {
public:
    void handle(Context* context) override;
};

// Context
class Context {
private:
    State* state;

public:
    Context(State* state) : state(state) {}
    ~Context() { delete state; }

    void setState(State* state) {
        std::cout << "Context: Transition to " << typeid(*state).name() << "\n";
        delete this->state;
        this->state = state;
    }

    void request() {
        state->handle(this);
    }
};

void ConcreteStateA::handle(Context* context) {
    std::cout << "ConcreteStateA handles request.\n";
    context->setState(new ConcreteStateB());
}

void ConcreteStateB::handle(Context* context) {
    std::cout << "ConcreteStateB handles request.\n";
    context->setState(new ConcreteStateA());
}

int main() {
    Context* context = new Context(new ConcreteStateA());
    context->request();
    context->request();
    context->request();

    delete context;
    return 0;
}
```

*   **Strategy**: Encapsulates different algorithms and lets you switch them at runtime.

```cpp
#include <iostream>

// Strategy interface
class Strategy {
public:
    virtual ~Strategy() {}
    virtual int execute(int a, int b) = 0;
};

// Concrete Strategies
class ConcreteStrategyAdd : public Strategy {
public:
    int execute(int a, int b) override {
        return a + b;
    }
};

class ConcreteStrategySubtract : public Strategy {
public:
    int execute(int a, int b) override {
        return a - b;
    }
};

// Context
class Context {
private:
    Strategy* strategy;
public:
    Context(Strategy* strategy) : strategy(strategy) {}
    ~Context() { delete strategy; }

    void setStrategy(Strategy* strategy) {
        delete this->strategy;
        this->strategy = strategy;
    }

    int executeStrategy(int a, int b) {
        return strategy->execute(a, b);
    }
};

int main() {
    Context* context = new Context(new ConcreteStrategyAdd());
    std::cout << "Result: " << context->executeStrategy(4, 2) << std::endl;

    context->setStrategy(new ConcreteStrategySubtract());
    std::cout << "Result: " << context->executeStrategy(4, 2) << std::endl;

    delete context;
    return 0;
}
```

*   **Template Method**: Defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

```cpp
#include <iostream>

// Abstract class
class AbstractClass {
public:
    void templateMethod() {
        baseOperation1();
        requiredOperation1();
        baseOperation2();
        hook1();
        requiredOperation2();
        baseOperation3();
        hook2();
    }

protected:
    virtual void requiredOperation1() = 0;
    virtual void requiredOperation2() = 0;

    virtual void hook1() {}
    virtual void hook2() {}

    void baseOperation1() {
        std::cout << "AbstractClass: BaseOperation1\n";
    }
    void baseOperation2() {
        std::cout << "AbstractClass: BaseOperation2\n";
    }
    void baseOperation3() {
        std::cout << "AbstractClass: BaseOperation3\n";
    }
};

// Concrete Classes
class ConcreteClassA : public AbstractClass {
protected:
    void requiredOperation1() override {
        std::cout << "ConcreteClassA: RequiredOperation1\n";
    }
    void requiredOperation2() override {
        std::cout << "ConcreteClassA: RequiredOperation2\n";
    }
};

class ConcreteClassB : public AbstractClass {
protected:
    void requiredOperation1() override {
        std::cout << "ConcreteClassB: RequiredOperation1\n";
    }
    void requiredOperation2() override {
        std::cout << "ConcreteClassB: RequiredOperation2\n";
    }

    void hook1() override {
        std::cout << "ConcreteClassB: Hook1\n";
    }
};

int main() {
    std::cout << "Same client code can work with different subclasses:\n";
    AbstractClass* classA = new ConcreteClassA();
    classA->templateMethod();
    std::cout << "\n";
    std::cout << "Same client code can work with different subclasses:\n";
    AbstractClass* classB = new ConcreteClassB();
    classB->templateMethod();

    delete classA;
    delete classB;
    return 0;
}
```

*   **Visitor**: Separates an algorithm from the objects on which it operates.

```cpp
#include <iostream>
#include <string>
#include <vector>

class ConcreteComponentA;
class ConcreteComponentB;

// Visitor interface
class Visitor {
public:
    virtual void visitConcreteComponentA(ConcreteComponentA* element) = 0;
    virtual void visitConcreteComponentB(ConcreteComponentB* element) = 0;
    virtual ~Visitor() {}
};

// Component interface
class Component {
public:
    virtual ~Component() {}
    virtual void accept(Visitor* visitor) = 0;
};

// Concrete Components
class ConcreteComponentA : public Component {
public:
    void accept(Visitor* visitor) override {
        visitor->visitConcreteComponentA(this);
    }
    std::string exclusiveMethodOfConcreteComponentA() {
        return "A";
    }
};

class ConcreteComponentB : public Component {
public:
    void accept(Visitor* visitor) override {
        visitor->visitConcreteComponentB(this);
    }
    std::string specialMethodOfConcreteComponentB() {
        return "B";
    }
};

// Concrete Visitors
class ConcreteVisitor1 : public Visitor {
public:
    void visitConcreteComponentA(ConcreteComponentA* element) override {
        std::cout << element->exclusiveMethodOfConcreteComponentA() << " + ConcreteVisitor1\n";
    }
    void visitConcreteComponentB(ConcreteComponentB* element) override {
        std::cout << element->specialMethodOfConcreteComponentB() << " + ConcreteVisitor1\n";
    }
};

class ConcreteVisitor2 : public Visitor {
public:
    void visitConcreteComponentA(ConcreteComponentA* element) override {
        std::cout << element->exclusiveMethodOfConcreteComponentA() << " + ConcreteVisitor2\n";
    }
    void visitConcreteComponentB(ConcreteComponentB* element) override {
        std::cout << element->specialMethodOfConcreteComponentB() << " + ConcreteVisitor2\n";
    }
};

int main() {
    std::vector<Component*> components = {new ConcreteComponentA(), new ConcreteComponentB()};
    ConcreteVisitor1* visitor1 = new ConcreteVisitor1();
    ConcreteVisitor2* visitor2 = new ConcreteVisitor2();
    for (Component* component : components) {
        component->accept(visitor1);
    }
    std::cout << "\n";
    for (Component* component : components) {
        component->accept(visitor2);
    }

    delete visitor1;
    delete visitor2;
    for (Component* component : components) {
        delete component;
    }

    return 0;
}
```

*   **Null Object**: The Null Object pattern provides a default object that does nothing, minimizing null checks.

```cpp
#include <iostream>

// Abstract Service
class AbstractService {
public:
    virtual void doSomething() = 0;
    virtual ~AbstractService() {}
};

// Real Service
class RealService : public AbstractService {
public:
    void doSomething() override {
        std::cout << "RealService: Doing something\n";
    }
};

// Null Object
class NullService : public AbstractService {
public:
    void doSomething() override {}  // Does nothing
};

// Client
class Client {
private:
    AbstractService* service;
public:
    Client(AbstractService* service) : service(service) {}
    void doAction() {
        service->doSomething();
    }
};

int main() {
    AbstractService* realService = new RealService();
    Client* client1 = new Client(realService);
    client1->doAction();  // Output: RealService: Doing something

    AbstractService* nullService = new NullService();
    Client* client2 = new Client(nullService);
    client2->doAction();  // Does nothing

    delete realService;
    delete nullService;
    delete client1;
    delete client2;

    return 0;
}
```


## Behavioral Design Patterns most commonly used in real-world Applications

*   **Observer Pattern:** This is extensively used in UI development to handle events like button clicks and mouse movements. It's also used in social media apps to update followers' feeds when a user posts content.

*   **Mediator Pattern:** The Mediator design pattern is often used in event-driven systems, such as graphical user interfaces, where events need to be handled by different components.

*   **Strategy Pattern:** It can be seen in payment processing systems where different payment methods (credit card, PayPal, etc.) are interchangeable and can be selected at runtime.

*   **Template Method Pattern:** Some of the most popular behavioral patterns include the Observer, Mediator, and template method, strategy each of which addresses specific challenges related to object communication and coordination.

*   **Chain of Responsibility Pattern:** Is employed in middleware systems to process requests by multiple components sequentially until one of them handles the request.

*   **Iterator Pattern:** Is commonly used in various data structures like lists, trees, and maps to traverse and access elements without exposing the underlying structure.

*   **Command Pattern:** Finds application in command-line interfaces where user commands are encapsulated as objects and executed by invokers.


## Crticisms of This Book

The "Gang of Four" (GoF) book, titled "Design Patterns: Elements of Reusable Object-Oriented Software," is a highly influential software engineering book that describes [23 classic software design patterns](#1-creational-design-patterns). This book is considered a foundational text for object-oriented design theory and practice. The book divides design patterns into creational, structural, and behavioral categories.

Despite its influence, the book and the concept of design patterns have faced criticism:

*   **Overuse:** Patterns can be overused or misapplied, leading to unnecessarily complex solutions.

*   **Language limitations:** Some argue that these patterns are just workarounds for limitations in certain programming languages.

*   **Inefficiency:** Blindly applying patterns without considering the project's specific context can result in inefficient solutions.

*   **Maintenance:** Code strictly adhering to textbook patterns may be difficult to maintain in real-world scenarios.

*   **Lack of understanding:** Programmers may not fully grasp design patterns, making maintenance challenging.

*   **Dogmatic application:** Treating patterns rigidly can lead to designs that don't fit project needs.

*   **Grime Occurrence:** "Grime" can undermine design pattern benefits, increasing maintenance costs and negatively impacting software sustainability.


## Over-Engineering (Definition)

**Over-Engineering** in software development is designing a product or providing a solution that is **more complex** or has **more features than necessary** for the client's current needs. 

It means writing code that solves problems the client doesn't have. Overengineering can manifest as excessive features, redundancy, and overly advanced technology in simple products. 

While sometimes intentional to provide a wide margin of error, it's generally an error of design that wastes time and resources and introduces unnecessary points of failure. It violates value engineering and the minimalist ethos of **"less is more"**.

Reasons for over-engineering include **striving for perfectionism**, a proclivity for overcomplicating, speculation on future needs, poor management, and a lack of expertise.

Over-engineering can lead to **missed deadlines**, **increased costs**, and **cluttered functionality and design**. It can also **decrease the productivity** of a development team because the value realized might be less than if the team produced only what the user needs and wants. 

It can also involve **premature optimization**, which can hurt the project due to diminishing returns on time and effort invested in the design process.


## The Double-Edged Sword: Limitations and Drawbacks of Design Patterns

While design patterns offer **invaluable solutions** to **recurring** software design challenges, they are not without limitations and can even be detrimental if applied inappropriately. 

Prudent software engineering requires recognizing these constraints and employing design patterns **judiciously**, ensuring that the chosen patterns align with the specific needs and context of the project at hand.

### The Pitfalls of [Over-Engineering](#over-engineering-definition): 

A common trap is overusing or misapplying design patterns, leading to unnecessarily complex code that is difficult to understand and maintain. Implementing a design pattern without a genuine need can introduce unnecessary complexity and overhead. It’s important to strike a balance between using design patterns to solve specific problems and keeping the code base straightforward.

Some of the most common pitfalls of over-engineering are:

- **When Patterns Hinder Innovation:** An overemphasis on established patterns can curb the inclination to think outside the box. Relying solely on known patterns might limit the exploration of novel solutions to unique problems. Design patterns may prevent proper consideration of a problem. Sometimes a problem is not what it appears to be – reaching for a pre-built solution to the wrong problem can add time and cost more money to design and development lifecycles.

- **The Learning Curve and Maintenance Burden:** Design patterns require developers to have a solid understanding of their concepts and implementation details. Learning and mastering design patterns can take time and effort, especially for developers who are new to software design principles. Moreover, when people analyze the code and spend more time trying to understand the pattern than the business logic, then design patterns can be more damaging than useful.

- **Performance and Flexibility Trade-offs:** Certain patterns, particularly those introducing additional layers of abstraction, can impact system performance. The Proxy or Decorator patterns, for instance, while providing flexibility, may introduce latency in system operations. Once a particular design pattern is deeply embedded into a system, evolving or changing the system's design can become challenging without a comprehensive refactoring, which can be costly and time-consuming.

- **Technology and Paradigm Shifts:** While patterns abstract from specific technologies, they don't always adapt seamlessly to new technological advancements or programming paradigms. Over-reliance on established patterns may hinder the adoption of innovative techniques or solutions. Patterns may be irrelevant in certain circumstances. Software design patterns, in particular, may be irrelevant if the style of programming language is different to the previous style that the pattern was developed for.


# My Understanding of and opinion on Design Patterns

After studying design patterns, and reading other peoples opinions, its obvious that design patterns helpful but also can be tricky.

This book gives you names and solutions for common problems in coding. It helps you talk about code with other programmers. It can help you make your code easier to **reuse**, **fix**, and **change**. But you can't just use them everywhere. You need to know **when** to use them.

It's a **common misconception** that design patterns were primarily **useful due to the limitations of older programming languages and computing hardware**. 

While it's true that modern languages offer increased expressiveness and efficiency, rendering some patterns less essential, this **does not invalidate the fundamental value** of design patterns. 

Design patterns **provide a framework** for approaching software design problems in a **structured and thoughtful manner**, promoting better organization, maintainability, and scalability, regardless of the specific language or technology used.

The most important thing is to really understand the problem you're trying to fix. **Don't just use a design pattern because you think you have to!**

**Design patterns are tools, not rules.** Think about whether the pattern will really help, or if it just makes things more complicated.

**Experience is key.** The more you code, the better you'll get at knowing when a pattern is a good idea and when it's not. And it's okay to change a pattern or even forget about it if that makes your code better.

So, learn about design patterns from the ["Design Patterns: Elements of Reusable Object Oriented Software"](#design-patterns) and other sources. But remember, they're just one part of coding. 

The best code solves the problem well, is easy to understand, and simple to change later.
That's what really matters! 

**Code for the ***problem***, not for the ***pattern***.**
