[![Coverage Status](https://coveralls.io/repos/github/AbakirH/OOP-Design-Patterns/badge.svg?branch=master)](https://coveralls.io/github/AbakirH/OOP-Design-Patterns?branch=master)
[![Build Status](https://travis-ci.com/AbakirH/OOP-Design-Patterns.svg?branch=master)](https://travis-ci.com/github/AbakirH/OOP-Design-Patterns)

**<h1 align="center"> Factory Method Design Pattern </h1>**

This design pattern allows sub classes to manipulate a parent class to instianite the class that is needed at the moment.

For examble,:
 ~~~javascript
class BallFactory {
  constructor() {
    this.createBall = function(type) {
      let ball;
      if (type === 'football' || type === 'soccer') ball = new Football();
      else if (type === 'basketball') ball = new Basketball();
      ball.roll = function() {
        return `The ${this._type} is rolling.`;
      };

      return ball;
    };
  }
}

class Football {
  constructor() {
    this._type = 'football';
    this.kick = function() {
      return 'You kicked the football.';
    };
  }
}

class Basketball {
  constructor() {
    this._type = 'basketball';
    this.bounce = function() {
      return 'You bounced the basketball.';
    };
  }
}
 ~~~

 In this examble we see how if need a new football all we need to do is make a new instance of the Ballfactory class and create a footbal by two easy commands:
 ~~~javascript
 const factory = new BallFactory();
 const myFootball = factory.createBall('football');
 ~~~

This type of pattern is used when we want to manipulate objects that are different,but have many similar characteristics. 

Another examble is our Caculator class, where the operation subtraction is different than addition, but are all created through the Calculation class:
~~~javascript
  class Calculation {
    constructor(a, b, op) {
      this.a = a;
      this.b = b;
      this.op = op;
    }
    static Create(a, b, op){
      return new Calculation(a,b,op);
    }
    GetResults() {
      return this.op(this.a,this.b)
    }
}

class Calculator{
    static Calculations = [];
    static addCalculation(calculation){
        Calculator.Calculations.push(calculation);
    }
    static getLastCalculation(){
        return this.Calculations[this.Calculations.length-1];
    }
    static Sum(a,b) {
        let calculation = Calculation.Create(a,b,Sum);
        this.addCalculation(calculation);
        return calculation;
    }
    static Difference(a,b) {
        let calculation = Calculation.Create(a,b,Difference);
        this.addCalculation(calculation);
        return calculation;
    }
}
~~~

When we create a instance of the Addition calculation class we use the following commands:
~~~javascript
let calculation = Calculation.Create(a,b,Sum);
~~~
When we create a instance of the subtraction calculation class we use the following commands:
~~~javascript
let calculation = Calculation.Create(a,b,Difference);
~~~
We can see that these commands are very similar and how the subclass can create different types of calculations for our calculator without ever having to touch the Calculation class again

**<h1 align="center"> Adapter Design Pattern </h1>** 

This design pattern allows for objects that are incompatible with another to collabrate

This pattern is usally used when API's are updated and you need to configure them to still work with old APIs

For examble, you will see how an adabter class is created to work with the OldCalulator and NewCalculator Class and use the updated code
~~~javascript
// old interface
class OldCalculator {
  constructor() {
    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          return term1 + term2;
        case 'sub':
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

// new interface
class NewCalculator {
  constructor() {
    this.add = function(term1, term2) {
      return term1 + term2;
    };
    this.sub = function(term1, term2) {
      return term1 - term2;
    };
  }
}

// Adapter Class
class CalcAdapter {
  constructor() {
    // The adapter class creates an instance of the NewCalculator class
    const newCalc = new NewCalculator();
    
    // The adapter class adds the operations functions from the OldCalculator Class
    // However, it uses functions from the NewCalculator class for implementing addition and subtraction
    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          return newCalc.add(term1, term2);
        case 'sub':
          return newCalc.sub(term1, term2);
        default:
          return NaN;
      }
    };
  }
}

// usage
const oldCalc = new OldCalculator();
console.log(oldCalc.operations(10, 5, 'add')); // 15

const newCalc = new NewCalculator();
console.log(newCalc.add(10, 5)); // 15

//We see how the adaptar Calculatar class can use the operations function similar to how the OldCalulator Class did, while still being an instance of the NewCalculator class and using the updated code 
const adaptedCalc = new CalcAdapter();
console.log(adaptedCalc.operations(10, 5, 'add')); // 15;
~~~

**<h1 align="center"> Chain of Responsibility Pattern </h1>** 

The Chain of Responsibility pattern is pretty simple to understand. It is where you keep calling functions or requests right after one another and each request either processes it or calls the next function to take a look at the problem.

A real world examble would be when your computer has a weird problem that you can not figure out that you call Technical Support to assist you.
1st request: They may first ask you to turn it off and on again
             Problem still exists, try request 2
2nd request: Try unistalling the new application you installed  
            Problem still exists, try request 3       
3rd request: Try typing this command in your terminal
            Problem is solved, no more requests needed to be made

Another example would be if a user is logging into canvas, the user should not be able to proceed in the web application without correctly being logged in. However with the Chain of Responsibility pattern, first the user would log in and a class would handle that function, but only when that succeeds. Then the loggedInUser class can be called and populate the page with the apprioate information for the user.

~~~javascript
Coding Examble:
class CumulativeSum {
  constructor(intialValue = 0) {
    this.sum = intialValue;
  }

  add(value) {
    this.sum += value;
    return this;
  }
}

// We see how only after each add function is completed succesfully that the next add 
// function is called upon each other to get a sum that builds on one another
const sum1 = new CumulativeSum();
console.log(sum1.add(10).add(2).add(50).sum); // 62
~~~
