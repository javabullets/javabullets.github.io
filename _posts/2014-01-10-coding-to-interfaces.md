---
id: 2099
title: 'Coding To Interfaces'
date: '2014-01-10T12:52:55+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=21'
permalink: /coding-to-interfaces/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - interface
---

Coding to interfaces is common in Java, for example –

```
List<String> stringList = new ArrayList<String>();
```

Here the reference type List is the interface, and it uses the concrete class ArrayList(which implements the List interface) as its implementation.

**So why do we do this?**

Consider two classes –

```

class Bicycle {
   void pedal() {
    System.out.println("Bike - Woohoo - Im pedalling");
  }
}
```

```

class Car {
  void drive() {
    System.out.println("Car - Oh no - stuck in traffic");
  }
}
```

These are similar, and poorly designed as the calling class would look similar to –

```

class RoadTrafficSimulator {
   List<Bicycle> bicycleList = new ArrayList<Bicycle> ();
   List<Car> carList = new ArrayList<Car> ();
   public RoadTrafficSimulator() {
      bicycleList.add( new Bicycle() );
      carList .add( new Car () );
   }
   public void simulateRoad() {
      for ( Bicycle b : this.bicycleList ) {
         b.pedal();
     }
     for ( Car c : carList ) {
        c.drive();
     }
   }
   public static void main(String args[]) {
      RoadTrafficSimulator roadTrafficSimulator = new RoadTrafficSimulator();
      roadTrafficSimulator.simulateRoad();
   }
}
```

Output –

Bike – Woohoo – Im pedalling  
Car – Oh no – stuck in traffic

If we consider both the Bike and Car as having a shared function to move, we can create an interface, IVehicle, to define this contract –

```

interface IVehicle {
   void move();
}
```

```

class Bicycle implements IVehicle {
   public void move() {
      pedal();
   }
   void pedal() {
      System.out.println("Bike - Woohoo - Im pedalling");
   }
}
```

```

class Car implements IVehicle {
   public void move() {
      drive();
   }
   void drive() {
      System.out.println("Car - Oh no - stuck in traffic");
   }
}
```

The calling class could then be written as –

```

class RoadTrafficSimulator {
   List<IVehicle> iVehicleList = new ArrayList<IVehicle>();
   public RoadTrafficSimulator() {
      iVehicleList.add(new Bicycle());
      iVehicleList.add(new Car());
   }
   public void simulateRoad() {
      for (IVehicle b : this.iVehicleList) {
         b.move();
      }
   }
   public static void main(String args[]) {
      RoadTrafficSimulator roadTrafficSimulator = new RoadTrafficSimulator();
         roadTrafficSimulator.simulateRoad();
      }
   }
}
```

This means the iVehicleList only requires the objects it stores to implement the IVehicle interface contract, and is now more flexible. The code is also more loosely coupled as we only require implementations of IVehicle, instead of individual lists for each object type

**Why not use a class?**

This example is good for demonstrating why you would chose to use an interface rather than a class for IVehicle. Its clear that both the bicycle and car do move, but the nature of their movement is different(the bicycle is pedalled, and the car is driven). Other than that there is little in common – so an interface is the better choice. It would also allow us to extend the Bicycle and Car classes to further specialisms (eg ElectricCar) which could still populate Ivehicle provided they implement the move() contract

**Conclusion**

To summarize, the advantages of coding to interfaces are –

- Software easier to extend
- Only concerned with API and not implementation
- Loosely coupled

The RoadTrafficSimulator now uses a single iVehicleList to store all objects implementing IVehicle. This has made the code more flexible, because as long as the objects conform to the IVehicle contract we can call the associated move() method.