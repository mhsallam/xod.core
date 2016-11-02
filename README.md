# XOD for (.NETCore)
Fast xml-based object-oriented relational thread-safe embedded database
![XOD](https://raw.githubusercontent.com/mhsallam/xod.core/master/logo.jpg)


## Introduction
XOD database -pronounced as “ZOD”- is a thread-safe object oriented relational database for .NET and JAVA developers (JAVA version is still half cooked). XOD uses embedded XML files for storage, that means the data will be readable even without the application the database created for, and it is also better for integration and simple for data migration. Even If you didn’t like your database being readable like text files, you can always secure XOD files with encryption.

Based on the development approach, developers might start designing the application model classes, design the actual database storage and then build a data mapping layer between the two. They might go back and forth between these two design layers whenever new update comes up. But what if you can skip the second, and third layers, and just work on the model classes layer only, not warring about the database design and mapping objects to database records, because XOD will take it from there; that would be great, right! Even if your application is big that one XOD database might not be sufficient for, you can use unlimited number XOD databases, even for supportive tasks like configuration data storage. You can also use XOD at the development stages only, until you feel satisfied about your application model classes and business logic, then push your verified models design and build the actual database.

This little documentation gives you brief guide lines for how to utilize XOD for .NET developers.


## Features
1. Support all CRUD Operations: Create, Read, Update and Delete
2. Supported data types:
     * Primitive data types
     * Value type properties like struct and enum
     * String and DateTime objects are treated as value types as well
     * Reference type objects
     * Complex types (explained in [ForeignKey] Attribute and Complex Types sections)
     * Anonymous reference types … awesome! (explained in Anonymous (Dynamic) Types section)
     * One dimensional arrays of any of the above types
     * One dimensional generic collection of any of the above types (e.g. List<int>, List<Book>)
2. Queries
3. Self-join
4. Triggers
5. 1-1, 1-Shared, 1-M, and M-M Relationships
6. Cascade Update/Delete
7. Password/encryption security options
8. Primary Keys, Composite Keys
9. Unique-Value and Required validation rules
10. Autonumber/Autogenerate values
11. Encrypted properties
12. Special-character string properties (useful for HTML contents)
13. Excludable properties
14. Eager/Lazy data loading options
15. Thread-safe

## Getting Started
You can download Xod library from dist folder in this repository or add it directly to your application as NuGet package, to do this:

* Open your command prompt and execute the following:

```mkdir xodConsoleApp```
```cd xodConsoleApp```
```dotnet new```
```code ./```

Executing the above sequence of commands will:

..+ Create a new folder named ```xodConsoleApp```

..+ Open ```xodConsoleApp``` folder

..+ Initiate new .NETCore project in this folder

..+ Open this project with Visual Studio Code

* Edit project.json file and add Xod library to the dependencies of the project:

```
"dependencies": {
   "Xod.Core": "0.1.0-beta"
}
```


* Because we edited ```project.json``` file we need to go back to the command prompt and run this command:

```
dotnet restore
```

* Now, let's try some code; in Program.cs add ```using Xod;``` namespace

* Create a new class ```ToDo``` with the following properties 

```csharp
public class ToDo
{
   public int Id { get; set; }
   public string Title { get; set; }
   public bool Done { get; set; }
}
``` 

* Add this code to ```Main()``` method:

```csharp
XodContext db = new XodContext(@"c:\xod\data.xod");
db.Insert(new ToDo() { Title = "Read a book" });
```

* Run the application using this command:

```
dotnet run
```

When running the application, a new Xod database will be created unless it was already exist, then a new object of ToDo class will be inserted into the database.

* Go to the database path (in our exampe c:\\xod) and check out the database contents in xml-format files.


#### Complete Example

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Xod;

namespace XodConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            XodContext db = new XodContext(@"c:\xod\data.xod");

            //insert new object
            db.Insert(new ToDo() { Title = "Read a book" });

            //read the new inserted object
            var tdi = db.FirstMatch<ToDo>(s => s.Id == 1);

            //update the object
            tdi.Done = true;
            db.Update(tdi);
            
            Console.WriteLine("Xod operations completed!");
            Console.ReadKey();
        }
    }

    public class ToDo
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public bool Done { get; set; }
    }
}
```

## .NET Framework Version
This repository has been compiled for .NETCore framework, if you want to use XOD with .NET Framework please visit:
[XOD (for .NET Framework)](https://github.com/mhsallam/xod)

