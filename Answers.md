# Database Challenges

## Question 1

ERD

Please view ```ERD.png``` for the answer.


Please note I did not include one field (age) as I feel that can be calculated using the birthdate. It can then be extracted through a query or view.

## Question 2

```sql

UPDATE Employee SET [EmpEmail] = REPLACE([EmpEmail], SUBSTRING([EmpEmail], charindex('@',[EmpEmail])+1, (CHARINDEX('.',[EmpEmail]) - CHARINDEX('@',[EmpEmail]) -1)), 'company')
Where [EmpEmail] LIKE '%@%.%'

```

# Backend Challenges

## Question 1

You are probably familiar with social media "like" systems. People can like posts and the UI displays who likes certain posts.
Write a method that takes in a list of names and produces the format below:

```csharp
Likes(new List<string>()) => "no one likes this"
Likes(new List<string> {"Peter"}) => "Peter likes this"
Likes(new List<string> {"Jacob", "Alex"}) => "Jacob and Alex like this"
Likes(new List<string> {"Max", "John", "Mark"}) => "Max, John and Mark like this"
Likes(new List<string> {"Alex", "Jacob", "Mark", "Max"}) => "Alex, Jacob and 2 others like this"
```
Answer:

```csharp

	 private static string Likes(List<string> names)
        {
            string result = "";
            if (names != null)
            {
                switch (names.Count)
                {
                    case (0):
                        result = "no one likes this";
                        break;
                    case (1):
                        result = names[0] + " likes this";
                        break;
                    case (2):
                        result = $"{names[0]} and {names[1]} like this";
                        break;
                    case (3):
                        result = $"{names[0]}, {names[1]}, and {names[2]} like this";
                        break;
                    default:
                        result = $"{names[0]}, {names[1]}, and {names.Count - 2} others like this";
                        break;
                }
            }
            else
                result = "no one likes this";

            return result;
        }
```

## Question 2

The last task is about refactoring of code. Below you are given a class that creates robots and cars, this class uses a robot service, a part service, and a car service, which can be any web services (RESTful or SOAP).

You need to refactor this class as you see fit. The goal is to make the class more maintainable. You should apply any principles or patterns you think are necessary.

Don't write out the web service classes. Assume they have an implementation and you are just consuming them.

Bonus: what can be done to reduce tight coupling in a class?

```csharp

    public abstract class Factory<T>
    {
        protected RobotService _robotService;
        protected PartsService _partsService;
        protected CarService _carService;

        public Factory()
        {

        }

        public Factory(PartsService partsService)
        {
            _partsService = partsService;
        }

        public List<Parts> GetPartsFor(Enum ProductType)
        {
            return _partsService.GetParts(ProductType);
        }

        public abstract T BuildProduct(Enum ProductType);
       
    }

  class RobotFactory : Factory<Robot>, IFactory<Robot>
    {
        public RobotFactory(PartsService partsService) 
            : base(partsService)
        {
            _robotService = new RobotService();
        }

        public RobotFactory(RobotService robotService, PartsService partsService) 
            : base( partsService)
        {
            _robotService = robotService;
        }

        public Robot BuildActualProduct(Enum ProductType)
        {
            if (ProductType != null)
            {
                var parts = GetPartsFor(ProductType);
                if (ProductType == RoboticDog)
                {
                    return _robotService.BuildRobotDog(parts);
                }
                else if (ProductType == RoboticCat)
                {
                    return _robotService.BuildRobotCat(parts);
                }
                else if (ProductType == RoboticDrone)
                {
                    return _robotService.BuildRobotDrone(parts);
                }
                else if (ProductType == RoboticCar)
                {
                    return _robotService.BuildRobotCar(parts);
                }
                else
                    return null;

            }
            return null;
        }

        public override Robot BuildProduct(Enum RobotType)
        {
            Console.WriteLine("Robot Factory");

            return BuildActualProduct(RobotType);
        }
    }

    class CarFactory : Factory<Car>, IFactory<Car>
    {
        public CarFactory(PartsService partsService) : base(partsService)
        {
            _carService = new CarService();
        }

        public CarFactory(CarService carService, PartsService partsService)
            : base(partsService)
        {
            _carService = carService;
        }


        public override Car BuildProduct(Enum CarType)
        {
            return BuildActualProduct(CarType);
        }

        public Car BuildActualProduct(Enum ProductType)
        {
            Console.WriteLine("Car Factory");
            if (ProductType != null)
            {
                var parts = GetPartsFor(ProductType);
                return _carService.BuildCar(parts);
            }
            return null;
        }
    }


    interface IFactory<T> 
    {
        T BuildActualProduct(Enum ProductType);
    }

   
```

Bonus: To reduce coupling, always code to an interface . This basically means that we are not coding to the object itself, and thus we won't need to interact with the original object should we need to use the functionality it contained. It is better to interact with the interface of the object and not the object itself. That way, when the object dies, the interface remains and thus any interacting classess remain intact.

# Front-end Code Challenges

## Question 1
Given the below code snippet, solve the problems that follow:

```html
<div id="firstDiv" class="red-card">
<div id="secondDiv" class="red-card">

<style>
    #secondDiv {
        background: orange;
    }

    div {
        height: 150px;
        width: 150px;
        background: green;
    }

    .red-card {
        background: red;
    }

    .yellow-card {
        background: yellow;
    }
</style>
```

1) What will the colour of both div elements be?

	```firstDiv``` is red, ```secondDiv``` is orange


2) How would you dynamically target firstDiv and make it's colour pink? (provide the code snippet)

```javascript
	document.getElementById("firstDiv").style.background = "pink";
```
3) How would you dynamically target secondDiv and add the yellow-card class to its class list? (provide the code snippet)
```javascript
	document.getElementById("secondDiv").classList.add('yellow-card');
```

## Question 2
Consider the ```compareIt``` function definition

```javascript
function compareIt(num1, num2) {
    return num1 == num2;
}

compareIt(5, "5");
```

1. Why will the function call return true? 
``` 
    The abstract equality operator (==) does not factor the data types of the operands. The two values' types are firstly converted and only after are they checked for equality.
```

2. How could one change this function so that data types are checked as well as values?
``` 
   To ensure equality to the highest measure, the strict equality operator (===) must be used. This will ensure that the data type is also factored in the check for equality between the two operands.
```

## Question 3
1. How would you make a web page mobile friendly (i.e responsive)? 
    There are multiple ways through which one can make a web page mobile friendly.
* Percentages:

    The first is usage of percentages in CSS when sizing elements. This makes sure the element is always taking up the specified percentage of the space. Even when loaded on a different-sized screen, the element retains its size percentage.
```
    .largeBlock { width: 80%;}
```

 * Media Queries:

    The second is usage of the ```@media``` rule. It ensures that a block of CSS properties become active only when a certain condition is met. This condition could be the size of the screen. Below, the background colour changes when a smaller screen than 600px is used.
```
@media only screen and (max-width: 600px) {
  body {
     background-color: pink;
  }
}
```

 * CSS Frameworks:

    Lastly is usage of CSS Frameworks such as Bootstrap. These frameworks facilitate the development process and make writing css code which is responsive a lot easier. This can be used to make a 3 column layout on a desktop screen change to become a single column after a specific screen threshold.
```html 
<div class="row">
    <div class="col-sm-4"></div>
    <div class="col-sm-4"></div>
    <div class="col-sm-4"></div>
</div>
```

* Honourable mention
    It is very important to include the meta viewport in every page. This ensures that the web content fits onto the screen of what ever device is used.
    ```
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    ```
2. What is the benefit of bundling .js scripts into one file?

    Bundling the .js scripts into one files reduces the number of HTTP requests to the server; which could result in better page performance.
3. What needs to be done to ensure the browser understands Sass styling?

    In order to get the browser to understand Saas code, you will need a Sass transpiler that will convert Sass code into normal CSS. You can install it on the command line, through libraries and and certain applications. 
4. What should be done to ensure browser compatibility with newer flavours of JavaScript like ES6/7?

    You can use a JavaScript transcompiler to convert newer JavaScript like ES6 into a version of JavaScript that can run on older engines. Babel is one of the most common transcompilers that can do this.