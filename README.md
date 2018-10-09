# Chapter 1

## In Praise of Design

> Change is inevitable

- An application with perfect requirement.

- Well desgned application that can accomodate change. - reduces cost, maintainablility and flexible for new changes.


## Why Change is Hard.

- An Object oriented application - operates - parts and interaction
- interaction - message
- parts - Objects
- message - dependencies
- OOP - managing dependencies
- Amount of information you can hold in your head.

## Practical Definition of Design

- Design - the art of arranging code.
- Cost of change > Cost of initial investment
- an arrangement of code - cost effective

## Tools of Design

- art form ? sculpting with chisels.
- it's not how to make steps and rules

### Design Principles

#### SOLID

- SOLID acronym
    - coined by Michael Feathers
    - popularized by Robert Martin
- Single Responsibility
- Open-Closed
- Liskov Substitution
- Interface Segregation
- Dependency Inversion

#### DRY

- Don't Repeat Yourself - Andy Hunt and Dave Thomas

#### LoD

- Law of Demeter
    - Demeter project at Northeastern University

> The real question is - do they work ? How can I trust them?

- Initially opinions. Academics got involved. Measurable metrics.
- 1990s - Chidamber and Kemerer and Basili 
    - over all size of classes
    - the entanglements that classes
    - breadht and depth of inheritance hierarchy
    - the number of methods that get invoked as a result of any message sent
    - correlated these measures with arrangements of code.
    - quality vs techniques - correlation
    - small applications - students fresh out of university
- In 2001, Laing and Coleman - NASA Goddard Space Flight Center applications (rocket science) - a way to produce cheaper and higher quality software.
    - 1,617 classes and more than 500,000 lines of code.
    - supports previous studies.

### Design Patterns

- Gang of Four (Gof ), Erich Gamma, Richard Helm, Ralph Johnson, and Jon Vlissides,
wrote the seminal work on patterns in 1995
- “simple and elegant solutions to specific problems in object-oriented software
design” that you can use to “make your own designs more flexible, modular, reusable
and understandable.”
- Each pattern solves a specific problem.
- Mis application doesn't mean pattern is wrong.

https://sourcemaking.com/design-patterns-and-tips


## The ACT of Design

> How hard can it be if everything is already been solved ?

- like making furnitures or sculpture or a painting.
- you only have the tools.

## How Design fails ?

- people who don't know design.

> “Yes, I can add that feature, but it will break everything.”

- people who know some of it but doesn't understand it well.

> “No, I can’t add that feature; it wasn’t designed to do that.”

- people who are seperated from design

> “Well, I can certainly write this, but it’s not what you really want and you will eventually be sorry.”

 ## When to use Design

- Agile - iteration, changes, 
- Big Up Front Design (BUFD)
    - It will end up as a blame game.
- Agile doesn't mean no design.
    - OOD

> Accomodating for change doesn't mean anticipating future change requests and features

> Agile thus does not prohibit design, it requires it. Not only does it require design, it requires really good design. It needs your best work. Its success relies on simple, flexible, and malleable code.

## Judging Design

- Early days - source lines of code or SLOC

> Good code quality metrics doesn't mean good design but bad metrics are defenitely bad.

Real measure

> cost per feature over the time interval that matters

- Technical Debt. What are they ?

> sometimes a feature can cost you your whole business.

> Primary constraints of Design - Your Skill and Timeframe.

## Procedural Vs Object Oriented

### Procedural

- variables - data types and operations
- what happens to the data we send is upto the function

### Object Oriented

- objects - behavior - open ended

## Objective of Design

- easy to change applications

# Chapter 2 : Desgining classes with single responsibility

- The key thing is __message__

- Questions
- what are your classes
- how many should you have
- what behavior will they implement ?
- How much do they know about other classes ?
- How much should they expose ?

**Goal** : To design application which will solve current change request and leave room to accomodate new change but also not anticipate new future things.


## What belongs in a class ?

- arrange code in such a way that it makes it easy to make changes later.
- easy is vague and too broad

### What does easy mean ?

- changes have no explicit side effects
- small changes in requirements require correspondingly small changes in code
- existing code is easy to reuse
- The easiest way to make a change is to add that in itself is easy to change

### Qualities the code must have

- **Transparent** : hat is
changing and in distant code that relies upon it
- **Reasonable** : The cost of any change should be proportional to the benefits the
change achieves
- **Usable** :  Existing code should be usable in new and unexpected contexts
- **Exemplary** : The code itself should encourage those who change it to perpetuate
these qualities

## Single Responsibility

- A class should do the smallest possible useful thing; that is, it should have a single
responsibility.

### Problem

- BiCycle with different types of **chainring** and **cog**.
- The rotation of the wheel depends on the ratio of chainring and cog.

```ruby
chainring = 52                              # number of teeth
cog = 11
ratio = chainring / cog.to_f
puts ratio                                  # -> 4.72727272727273

chainring = 30
cog = 27
ratio = chainring / cog.to_f
puts ratio                                  # -> 1.11111111111111
```

- Re-usable component

```ruby
class Gear
  attr_reader :chainring, :cog
  def initialize(chainring, cog)
    @chainring = chainring
    @cog = cog
  end
  
  def ratio
    chainring / cog.to_f
  end
end
    
puts Gear.new(52, 11).ratio
puts Gear.new(30, 27).ratio
```

- New chage requirements
- Diameter of the wheel gives something called gear inch.

```
gear inches = wheel diameter * gear ratio
where
wheel diameter = rim diameter + twice tire diameter
```

```ruby
class Gear
  attr_reader :chainring, :cog,  :rim, :tire
  def initialize(chainring, cog, rim, tire)
    @chainring = chainring
    @cog = cog
    @rim = rim
    @tire = tire
  end
  
  def ratio
    chainring / cog.to_f
  end

  def gear_inches
    # tire goes around rim twice for diameter
    ratio * (rim + (tire * 2))
  end

end
    
puts Gear.new(52, 11, 26, 1.5).gear_inches
# -> 137.090909090909
puts Gear.new(52, 11, 24, 1.25).gear_inches
# -> 125.272727272727
```

- New bug after code change

## Why Single Responsibility Matters

- Easy to change aplications -> reusable classes. Plug and play. Building blocks.
- A class - morethan one responsibilities - entangled. Isolation of one behavior - XX
- Duplicate wanted behavior - Maintenance
- Effect of change - 

## Determine if class has Single Responsibility

“Please Mr. Gear, what is your ratio?” seems perfectly reasonable, while
“Please Mr. Gear, what are your gear_inches?” is on shaky ground, and “Please Mr. Gear,
what is your tire (size)?” is just downright ridiculous.
- If A can respond to some y message. Then Some X will use it.

- Try defining responsiblity of class in a single sentence.
If the simplest description you can devise uses the word “and,” the class likely has more than one responsibility. 
- If it uses the word “or,” then the class has more than one responsibility and they aren’t even very related.

- OO designers - cohesion. Class => Central Purpose => Highly cohesive. 
- SRP(Single Responsibility Principle) => Rebecca Wirfs-Brock and Brian Wilkerson’s idea of Responsibility-Driven Design (RDD)

> “A class has responsibilities that fulfill its purpose.”

- Gear class => “Calculate the ratio between two toothed sprockets”?. May be => “Calculate the effect that a gear has on a bicycle”?
- Tire is still shaky

- Gear has more than one responsibility but it’s not obvious what should be done.

## Determining When to Make Design Decisions

> “What is the future cost of doing nothing today?” 

## Writing Code that embraces change

### Depend on Behavior, Not Data

**1. Hide Instance Variables**

```ruby
class Gear
  def initialize(chainring, cog)
    @chainring = chainring
    @cog = cog
  end
  
  def ratio
    @chainring / @cog.to_f   <-- road to ruin
  end
end
    
puts Gear.new(52, 11).ratio
puts Gear.new(30, 27).ratio
```

- use attr_reader. chaging Data to behavior

```ruby
class Gear
  attr_reader :chainring, :cog
  def initialize(chainring, cog)
    @chainring = chainring
    @cog = cog
  end
  
  def ratio
    chainring / cog.to_f
  end
end
    
puts Gear.new(52, 11).ratio
puts Gear.new(30, 27).ratio
```

- the bhavior of @cog may change based on some other factor

```ruby
# a simple reimplementation of cog
def cog
  @cog * unanticipated_adjustment_factor
end

# a more complex one
def cog
  @cog * (foo? ? bar_adjustment : baz_adjustment)
end
```

- Dealing with data as if it's an object and has behaviors introduces two new issues.
    - it exposes the presence of cog to all
    - the loss of distinction between data and behavior


**2. Hide Data Structures**

```ruby
class ObscuringReferences
  attr_reader :data
  def initialize(data)
    @data = data
  end

  def diameters
    # 0 is rim, 1 is tire
    data.collect {|cell| cell[0] + (cell[1] * 2)}
  end

  # ... many other methods that index into the array
end

# rim and tire sizes (now in millimeters!) in a 2d array
@data = [[622, 20], [622, 23], [559, 30], [559, 40]]
```

- diameter method has to know what data is despite data method hiding the instance variable from itself.
- each sender of data must have complete knowledge of what piece of data is at which index in the array.
- When you have data in an array it’s not long before you have references to the array’s structure all over. The references are leaky. They escape encapsulation and insinuate themselves throughout the code. They are not DRY. The knowledge that rims are at [0] should not be duplicated; it should be known in just one place.
- Direct references into complicated structures are confusing, because they obscure what the data really is, and they are a maintenance nightmare, because every reference will need to be changed when the structure of the array changes.

```ruby
class RevealingReferences

  attr_reader :wheels

  def initialize(data)
    @wheels = wheelify(data)
  end

  def diameters
  wheels.collect {|wheel|
  wheel.rim + (wheel.tire * 2)}
  end

  # ... now everyone can send rim/tire to wheel
  Wheel = Struct.new(:rim, :tire)
  def wheelify(data)
    data.collect {|cell|
    Wheel.new(cell[0], cell[1])}
  end

end
```

- Now wheel is an enumerable that can respond to rim and tire
- Struct as “a convenient way to bundle a number of attributes together, using accessor methods, without having to write an explicit class.”
- This style of code allows you to protect against changes in externally owned data structures and to make your code more readable and intention revealing. It trades indexing into a structure for sending messages to an object. The wheelify method above isolates the messy structural information and DRYs out the code. It makes this class far more tolerant of change.

## Enforce Single Responsibility Everywhere

### Extract Extra Responsibilities from Methods

- Methods should have single responsibility as well.
- All the same techniques apply.
- ask them questions about what they do and try to describe their responsibilities in a single sentence.

```ruby
def diameters
  wheels.collect {|wheel|
  wheel.rim + (wheel.tire * 2)}
end
```

- It clearly does two things.

```
# first - iterate over the array
def diameters
  wheels.collect {|wheel| diameter(wheel)}
end
# second - calculate diameter of ONE wheel
def diameter(wheel)
  wheel.rim + (wheel.tire * 2))
end
```

- Consider sending messages as free of cost. Performance can be improved later if needed.
- Consider this method 

```
def gear_inches
  # tire goes around rim twice for diameter
  ratio * (rim + (tire * 2))
end
```

- can be written as

```
def gear_inches
  ratio * diameter
end

def diameter
  rim + (tire * 2)
end
```

- Do these refactorings even when you do not know the ultimate design. They are needed, not because the design is clear, but because it isn’t.
- Good practices reveal design
- Gear is definitely responsible for calculating gear_inches but Gear should not be calculating wheel diameter.

### Benefits of single refactoring

- **Expose previously hidden qualities** : method level single responsibility reveals core responsibility of class
- **Avoid the need for comments** : comment maintenance issues - out of date 
- **Encourage reuse** : keeps code DRY.
- **Are easy to move to another class** : Future design decisions upon more information.

## Isolate Extra Responsibilities in Classes

- Any decision you make in advance of an explicit requirement is just a guess. Don’t decide; preserve your ability to make a decision later.

```ruby
class Gear
  attr_reader :chainring, :cog, :wheel
  def initialize(chainring, cog, rim, tire)
    @chainring = chainring
    @cog = cog
    @wheel = Wheel.new(rim, tire)
  end
  
  def ratio
    chainring / cog.to_f
  end

  def gear_inches
    # tire goes around rim twice for diameter
    ratio * wheel.diameter
  end

  Wheel = Struct.new(:rim, :tire) do
    def diameter
      rim + (tire * 2)
    end
  end

end
```

- Embedding Wheel inside of Gear suggests that you expect that a Wheel will only exist in the context of a Gear. But common sense would suggest otherwise.

## A wheel class finally

- New change request. To know the circumference of the wheel.

- Gear class will become like this

```ruby
class Gear
  attr_reader :chainring, :cog, :wheel
  def initialize(chainring, cog, wheel = nil)
    @chainring = chainring
    @cog = cog
    @wheel = wheel
  end
  
  def ratio
    chainring / cog.to_f
  end

  def gear_inches
    # tire goes around rim twice for diameter
    ratio * wheel.diameter
  end
end

class Wheel
  attr_reader :rim, :tire
  def initialize(rim, tire)
    @rim = rim
    @tire = tire
  end
  def diameter
    rim + (tire * 2)
  end
  def circumference
    diameter * Math::PI
  end
end

@wheel = Wheel.new(26, 1.5)
puts @wheel.circumference                     # -> 91.106186954104
puts Gear.new(52, 11, @wheel).gear_inches     # -> 137.090909090909
puts Gear.new(52, 11).ratio                   # -> 4.72727272727273
```

- This may not be perfect but it is good enough that it maintains single responsibility principle.