# Adapter

## Learning Goals

- Explain the adapter pattern.
- Use the pattern in Java.

## Introduction

An **adapter** is a structural design pattern that acts as a connector between
two incompatible interfaces. This pattern uses a single class to bridge the
functionalities of two interfaces that otherwise could not be directly
connected.

We use the adapter pattern when we want to introduce a new interface between a
client class and existing functionality. This is done usually when the existing
functionality does not work as is for the client class but should not (or cannot)
be modified to meet the new requirements.

Here are the highlights from the adapter pattern:

- Lets a client class work with a subcomponent that it wouldn't otherwise be
  able to work with because the interfaces are not compatible.
- Allows the original subcomponent to be used without modification.
- Useful to avoid re-writing or modifying existing functionality but still
  re-use it.

The adapter design pattern is appropriately named because it does just that -
adapt the code through the use of a singular class so that everything (new and
old) can function properly. To provide a real-world analogy, think of a power
plug adapter that can be used when traveling to different countries. Different
countries around the world have a different standard socket when you go to
plug in your laptop, phone, or other electronic devices. To still use your
power plugs, you can buy an adapter. This will allow you to use your original
charger and have it plugged into a different country's socket.

## Implementation

One more time, let us consider our transportation example from the factory
lesson. Each of our vehicles can travel at a certain speed, so let's go ahead
and add a new method called `getSpeed()` to each of the `Vehicle`
implementations that will return a speed in miles per hour:

```java
package com.flatiron.transportation;

public interface Vehicle {
  void travel();
  
  // New method - return the speed in miles per hour
  double getSpeed();
}
```

```java
package com.flatiron.transportation;

public class Car implements Vehicle {

    @Override
    public void travel() {
        System.out.println("Traveling by car!");
    }
    
    @Override
    public double getSpeed() {
        return 120;
    }
}
```

```java
package com.flatiron.transportation;

public class Boat implements Vehicle {

    @Override
    public void travel() {
        System.out.println("Traveling by boat!");
    }

    @Override
    public double getSpeed() {
      return 50;
    }
}
```

```java
package com.flatiron.transportation;

public class Plane implements Vehicle {

    @Override
    public void travel() {
        System.out.println("Traveling by plane!");
    }

    @Override
    public double getSpeed() {
      return 700;
    }
}
```

Somewhere down the line, let's assume someone asks if we could convert the units
of miles per hour to kilometers per hour instead. We don't really want to change
the existing code, as it may break something. Instead, we can use the adapter
design pattern!

We first need an adapter interface to help make the connection:

```java
package com.flatiron.transportation;

public interface KilometerSpeed {
    
    // Return the speed in kilometers per hour
    double getSpeed();
}
```

Now we can create the adapter class that will implement the interface we created
above:

```java
package com.flatiron.transportation;

public class SpeedAdapter implements KilometerSpeed {
    
    private Vehicle vehicle;
    
    public SpeedAdapter(Vehicle vehicle) {
        this.vehicle = vehicle;
    }
    
    @Override
    public double getSpeed() {
        return convertMphToKmph(vehicle.getSpeed());
    }
    
    private double convertMphToKmph(double mph) {
        return mph * 1.609344;
    }
    
}
```

It's time to test out the adapter design pattern! We'll go ahead and create a
driver class:

```java
package com.flatiron.transportation;

public class TransportationDriver {
    
    public static void main (String[] args) {
      Vehicle car = TransportationFactory.buildVehicle("CAR");
      Vehicle boat = TransportationFactory.buildVehicle("BOAT");
      Vehicle plane = TransportationFactory.buildVehicle("PLANE");
      
      KilometerSpeed carInKmph = new SpeedAdapter(car);
      System.out.println("The speed of a car in kilometers per hour is " + carInKmph.getSpeed());
      
      KilometerSpeed boatInKmph = new SpeedAdapter(boat);
      System.out.println("The speed of a boat in kilometers per hour is " + boatInKmph.getSpeed());
      
      KilometerSpeed planeInKmph = new SpeedAdapter(plane);
      System.out.println("The speed of a plane in kilometers per hour is " + planeInKmph.getSpeed());
    }
}
```

Expected output of the above code:

```plaintext
The speed of a car in kilometers per hour is 193.12128
The speed of a boat in kilometers per hour is 80.4672
The speed of a plane in kilometers per hour is 1126.5408
```
