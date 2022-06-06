# Adapter

## Learning Goals

- Explain the adapter pattern
- Use the pattern in Java

## Introduction

Use the adapter pattern when you want to introduce a new interface between a
client class and existing functionality because the existing functionality does
not work as is for the client class but should not (or cannot) be modified to
meet the new requirements.

Here are the highlights from the Adapter pattern:

- Lets a client class work with a subcomponent that it wouldn't otherwise be
  able to work with because the interfaces are not compatible
- Allows the original subcomponent to be used without modification
- Useful to avoid re-writing or modifying existing functionality but still
  re-use it

## Implementation

The `Camera` class we put together so far works for traditional SLR cameras, but
it assumes a camera has film in it. What if we wanted to have a digital camera?

We can create an adapter interface that supports the same method as our original
`Camera` interface:

```java
public interface CameraAdapter {

    public void takePhotograph(double shutterSpeed);
}
```

And then we can create a `DigitalCamera` class that implements that interface:

```java
public class DigitalCameraAdapter implements CameraAdapter {
    private Camera traditionalCamera;

    public DigitalCameraAdapter(Camera camera) {
        this.traditionalCamera = camera;
    }

    public void takePhotograph(double shutterSpeed) {
        // implement a subset of the takePhotograph() functionality from the Camera class here
    }
}
```

This sets up the pattern. Now, we need access to the functionality from the
`takePhotograph()` method in the `Camera` class.

Note that the Adapter pattern could be used directly with a concrete class
`DigitalCameraAdapter` without the interface `CameraAdapter` - this would give
us the same functionality as what we outlined above, but it would be more
tightly coupled to the specific digital camera implementation we have here.
