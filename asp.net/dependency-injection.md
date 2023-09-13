# What I learned about Dependency Injection in C# and ASP .NET core
Today I learned about Dependency Injections and how they work in C#. Has someone who has been studying computer sciences / programing since 2018 I've always heard of Dependency injection but never really knew what that meant. So I wanted to dive right into it!
## What is dependency injection?
Dependency Injection is a programming pattern that helps a program get its parts from outside the current scope, rather than letting it create or find them itself. This makes the program have decency's on certain outside classes or methods.

After doing some initial research on what a dependency injection is, I wanted to get a better understanding so I created a coffee machine analogy. 
### Analogy to help me understand:
Imagine you have a coffee machine (the class in code). Instead of this machine always using the same coffee beans container built into it, you want the flexibility to use any coffee beans container brand you like. Dependency Injection is like designing your coffee machine with a slot where you can put in any coffee beans container (your dependencies) you want, giving you the freedom to change it whenever you like.

Using this Analogy I created an example program that uses a interface with dependency injection based on a coffee machine using different containers holding different beans.
### C# Coffee Machine Analogy Example
``` c#
public interface ICoffeeBeansContainer
{
    string GetBeans();
}

public class BrandAContainer : ICoffeeBeansContainer
{
    public string GetBeans()
    {
        return "Beans from Brand A";
    }
}

public class BrandBContainer : ICoffeeBeansContainer
{
    public string GetBeans()
    {
        return "Beans from Brand B";
    }
}

public class CoffeeMachine
{
    private ICoffeeBeansContainer _container;

    public CoffeeMachine(ICoffeeBeansContainer container)
    {
        _container = container;
    }

    public string MakeCoffee()
    {
        string beans = _container.GetBeans();
        return $"Making coffee using {beans}";
    }
}

public class Program
{
    static void Main()
    {
          // Using Brand A container with the coffee machine
        ICoffeeBeansContainer containerA = new BrandAContainer();
        CoffeeMachine machineWithA = new CoffeeMachine(containerA);
        Console.WriteLine(machineWithA.MakeCoffee());

          // Using Brand B container with the coffee machine
        ICoffeeBeansContainer containerB = new BrandBContainer();
        CoffeeMachine machineWithB = new CoffeeMachine(containerB);
        Console.WriteLine(machineWithB.MakeCoffee());
    }
}
```

## Personal Insight
### Benefits and Disadvantages
**Benefits of Dependency Injection:** Dependency Injection? It's great if you know how and when to use it. By making your code modular, it becomes a whole lot simpler to mesh it with interfaces. With everything neatly arranged, your code isn’t just more reliable—it's also easier to handle and update. Testing? Less of a hassle. Instead of letting the code be tied down by built-in components, you're the one in control, deciding what gets passed in. Dependency Injection can avoid null reference errors. Plus, if you ever think about moving your code elsewhere, it's very easy to swap out parts with different data or change the environment.

**Drawbacks of Dependency Injection:** Now, while Dependency Injection has its benefits, it’s not without issues. The more you lean into it, you might notice your code getting more complex. Why is that? You're handling more pieces and often doing a lot of extra method setups using interfaces. The way you bring Dependency Injection into the mix can sometimes be too tightly integrated with your main code. On rare occasions, things might slow down a little with the added complexity at runtime. As with many coding techniques, there's a risk of overdoing it—making things complex when simplicity would do the trick.

## Dependency injection real world example from AdoptiverseAPI:
To really get a good understanding I decided to dig into projects I've worked on in the past and find examples of where I was using dependency injection. I chose my **AdoptiverseAPI** project to look around in. The first thing I realized that in most of all my MVC projects the initialization of the private `_context` database connection is being initialized in the constructor using dependency injection.

``` c#
 private readonly AdoptiverseApiContext _context;  
  
        public SheltersController(AdoptiverseApiContext context)  
        {  
            _context = context;  
        }
```
### How is dependency injection used here?
The `SheltersController` class asks for an object of `AdoptiverseApiContext` to be passed in when it's created, rather than creating it itself. When the constructor is called on creation, the `AdoptiverseApiContext` object is passed ("injected") from outside the constructor.
### Why is dependency injection used here?
Dependency injection is used to provide the `SheltersController` with the dependencies it needs to work with data, from postgresSQL. By getting its dependencies from outside the method, the controller can be more easily tested, modified, or used with different data in a different environment.
## Advanced topics relating to dependency injection
To wrap up my research on dependency injection I decided to look into some realted advanced topics that could help me get an even better understanding. I chose to look into what exactly a design pattern is in C# and why dependency injection is a design pattern. 
### What is a Design Pattern in software development?
A design pattern is a re-usable solution to a common problem that occurs a lot while in the development process. It can be thought of as a blueprint or a template for solving certain problems. Instead of reinventing the wheel every time a common issue happens, developers can use these trusted coding patterns to address them.
### Why is Dependency Injection a design pattern?
Dependency Injection is a design pattern because it provides a solution for a common problem; how to manage and supply dependencies in a modular and testable way.  Dependency Injection helps in achieving loose coupling. Loose coupling refers to designing systems so that individual components or parts have minimal knowledge and dependency on each other.

## Final Thoughts on Dependency Injection:
Today's deep dive into Dependency Injection has been insightful got me. Thinking back to that coffee machine analogy; it's about giving our programs the ability to swap out parts as needed. But, like most things in coding, it's not all smooth sailing.

While Dependency Injection offers a streamlined way to boost flexibility and make testing a bit more straightforward, it can sometimes add layers to our code we didn't anticipate. Looking back at my work with **AdoptiverseAPI**, it's evident that Dependency Injection isn't just a concept we learn about but something we've used in this course often without realizing it.

As a design pattern, Dependency Injection offers a solid solution to a common challenge in coding: managing external objects/components. But, as always, context is key. It's vital to know when and how to use it effectively. As I continue exploring the vast landscape of programming, understanding tools like Dependency Injection will help me craft better, more adaptable code.  
  
Thank you for your time and interest, happy coding

Sources:
Benefits of Dependency Injection:
[Piotr Bach's Blog](https://piotrbach.com/top-benefits-of-dependency-injection/)  

Disadvantages of Dependency Injection (from Stack Exchange):
[Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/371722/criticism-and-disadvantages-of-dependency-injection)  

Downsides of Dependency Injection (from Stack Overflow):
[Stack Overflow Discussion](https://stackoverflow.com/questions/2407540/what-are-the-downsides-to-using-dependency-injection)  

What People Don't Know About Dependency Injection:
[Medium Article by Analytics Vidhya](https://medium.com/analytics-vidhya/what-people-dont-know-about-dependency-injection-835a59fa5eea)  

Community Discussion About Dependency Injection:
[GitHub Discussion](https://gist.github.com/r00k/3105024)
