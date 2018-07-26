# Kata Design : A Temporaly Coupled pipeline

## Definitions

The goal of this exercice is to find a nice design solution for a pipeline of tasks with these constraints in mind:

* ensure temporal coupling : the tasks have to be done in a certain order

* one in the future can insert an aditional task

* in a SOLID mindset, no changes of the implemented class are allowed if this happend

* design is based on abstractions and should work with any implementations.

For the exemple, let’s imagine an engine that:

* reads a model from a source

* apply some work on this model

* writes the result to a destination.

This could be defined by the respective interfaces.

```csharp
public interface IReadModel<T> { 
      T Read(string source);
}
public interface IAdaptModel<T,U> {
    U Transform(T model);
}
public interface IWriteModel<T> {
    bool Write(T model, string destination)
}
```

Imagine that the concrete implementation is about:

* read a csv file of records (Id, First name, Last name)

* transform the records in a model with properties Id and Name (where name = FirstName + “ “ + LastName)

* write the models in a array of objects in a json file

Your work is to desing an Engine that works with these implementations based on the defined interfaces and respect the constrains defined in the beginning.

*note: If you need you can change the definition of the interfaces, they are here uniquely as an illustration purpose and maybe you need to tune them a little to fit your design.*

## Verification

In order to verify that our design is good, we’ll introduce a new step that will be pluged between the ReadModel and AdapModel steps. This step will be a filter that will be used to reduce the input:

```csharp
public interface IFilterInput<T> {
    T Filter(T input);
}
```

You have to introduce this new step with the following constraints:

* you need to ensure that it is done between Read and Adapt steps

* you need to minimise the amount of code changed.
