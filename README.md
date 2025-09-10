# Java Exception Handling

In Java, an exception is an event that interrupts the normal flow of a program. Exception handling provides a mechanism to transfer control when errors occur, allowing programs to respond gracefully instead of terminating abruptly.

Think of Java exceptions like unexpected problems in real life.
* Checked exception → like a librarian making sure you have a card before lending you a book. You must prove you can handle it before you can continue.
* Unchecked exception → like tripping while walking. Nobody checks you in advance. It just happens.

## Flow of Exceptions

When a method encounters an exception:
* It can handle it locally using a try/catch.
* Or it can propagate it to the caller with throws.

```
time -->

MainClass: call main() --------+ O                 +----------------
                               |   \               |
object1	     call filter() +---+ O           +-----+
                                   |  \      |
object2              call remove() +---X-----+
```

If `remove()` throws an exception, control moves up the call stack until it finds a suitable handler. If none exists, the program terminates.

## Exception Categories

All exceptions derive from Throwable:
```
               +-----------+
               | Throwable |
               +-----------+
                /         \
               /           \
        +-------+        +-----------+
        | Error |        | Exception |
        +-------+        +-----------+
         /  |  \          / |        \
       \________/	     \______/       \
        unchecked       checked   +------------------+
                                  | RuntimeException |
                                  +------------------+
                                   /   |    |      \
                                  \_________________/
                                       unchecked

```
* Checked exceptions: Enforced by the compiler. Methods that may throw them must declare this fact, and callers must either handle them or re-declare.

* Unchecked exceptions: Subclasses of RuntimeException (and Error). They are **not**  checked by the compiler.


### Example of Unchecked Exception
```java
int x = 0;
int y = 10;
int z = y / x; // ArithmeticException at runtime
```
The code compiles, but throws an exception during execution.

Other common unchecked exceptions:
* [NullPointerException](https://docs.oracle.com/javase/7/docs/api/java/lang/NullPointerException.html)
* [IndexOutOfBoundsException](https://docs.oracle.com/javase/7/docs/api/java/lang/IndexOutOfBoundsException.html)
* [EmptyStackException](https://docs.oracle.com/javase/7/docs/api/java/util/EmptyStackException.html)

### Example of Checked Exception
```java
Scanner sc = new Scanner(new File("data.txt")); // may throw FileNotFoundException
```
The compiler enforces handling with either try/catch or throws.

Other common checked exceptions:
* [IOException](https://docs.oracle.com/javase/7/docs/api/java/io/IOException.html)
* [FileNotFoundException](https://docs.oracle.com/javase/7/docs/api/java/io/FileNotFoundException.html)

## Design Guidelines

According to the [official Java tutorial](http://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html):
* Use a checked exception if the caller can reasonably recover.
* Use an unchecked exception if the error is unrecoverable.

### Example Usecases

This [example](https://replit.com/@lubaochuan/DapperKeenAstrophysics#Main.java) show how to recover from an input format mismatch exception:
```java
import java.util.Scanner;

class Main {
  public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);

    int input = 0;
    boolean done = false;
    while(!done){
      try{
        System.out.print("Please enter an integer: ");
        input = Integer.parseInt(scan.nextLine());
        System.out.println("You entered: "+input);
        done = true;
      }catch(Exception e){
        System.out.println(e);
        System.out.println("Please try again!");
      }
    }
  }
}
```

In the following [example](https://replit.com/@lubaochuan/DapperKeenAstrophysics#Main2.java), the compiler forces you to either
because "FileNotFoundException" is a checked exception:
* Put the call inside a try/catch, or
* Declare throws FileNotFoundException.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
   public static void main(String[] args) throws FileNotFoundException {
      Scanner sc = new Scanner(new File("data.txt"));
      System.out.println(sc.nextLine());
   }
}
```

Source: http://www.geeksforgeeks.org/checked-vs-unchecked-exceptions-in-java/