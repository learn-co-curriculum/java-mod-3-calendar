# Date and Calendar Class

## Learning Goals

- Explain the `Date` class and its shortcomings.
- Explain the `Calendar` class.

## Introduction

Sometimes, we want to calculate and store dates in Java. For example, if we
were to create an appointment system for a doctor's office, wouldn't it be nice
if the patient could pick the specific day and time of their appointment? The
developers of Java thought it would be helpful too! This is why they developed
the `Date` class and then the `Calendar` class later!

## January 1, 1970 00:00:00

Before we even talk about the `Date` or `Calendar` class, we need to talk about
the arbitrary starting date point that is used by most operating systems and
programming languages, including Java. When one of the early operating systems,
Unix, was being created, the developers needed a "starting time". For
simplicity, they chose the date January 1, 1970, at midnight (or 00:00:00).
This date time marks the "beginning of time" and is the earliest date a file
could be created in operating systems and programming languages that use this
common construct. Meaning, all other dates and times are measured off of this
datetime. We can also think of the time elapsed from January 1, 1970 at
00:00:00 as a **Unix epoch time**, which is the number of seconds or
milliseconds that have passed since the arbitrary start time. An easy resource
to obtain epoch timestamps is to use the [Epoch Converter](https://www.epochconverter.com/)
which consists of conversion tools to get a specific date as an epoch or vice
versa.

## Java's `Date` Class

The Java `Date` class represents an instant in time and can be found in the
`java.util` package. We can construct a `Date` instance in two ways:

```java
import java.util.Date;

public class DateExample {

    public static void main(String[] args) {
        // Will instantiate the current date time measured to the nearest millisecond
        Date date1 = new Date();

        // Will instantiate a date time based on the Unix epoch in milliseconds
        Date date2 = new Date(1663015601000);

    }
}
```

We can also get the time of the `Date` instance by calling the `getTime()`
method, which will return the number of milliseconds since the arbitrary start
date of January 1, 1970 at 00:00:00 UTC. (UTC is a timezone that is short for
"Universal Time Coordinated" and is a primary time standard used to set other
time zones. It is the same as the "GMT" or "Greenwich Mean Time" timezone.)

```java
import java.util.Date;

public class DateExample {

    public static void main(String[] args) {
        // Will instantiate the current date time measured to the nearest millisecond
        Date date1 = new Date();
        System.out.println(date1.getTime());    // <-- will print current date time in milliseconds

        // Will instantiate a date time based on the Unix epoch in milliseconds
        Date date2 = new Date(1663015601000);
        System.out.println(date2.getTime());    // <-- will print 1663015601000

    }
}
```

If we take a look at the Java documentation for the `Date` class here:
[Java 11 Date Class](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Date.html),
we see nearly every constructor and method is deprecated. This is due to the
shortcomings of the `Date` class. The `Date` class once "allowed the formatting
and parsing of date strings. Unfortunately, the API for these functions was not
amenable to internalization." With that said, Java decided to introduce a new
class to handle these limitations and work with the `Date` class.

## Java's `Calendar` Class

When Java deprecated most of the functionality of the `Date` class, they created
the `Calendar` class to improve the functionality and work with the `Date` class
"The `Calendar` class is an abstract class that provides methods for converting
between a specific instant in time and a set of calendar fields such as YEAR,
MONTH, DAY_OF_MONTH, HOUR, etc." Since the `Calendar` class is an abstract
class, we can't instantiate it. So we'll have to use a class that extends it or
use its static method `getInstance()`. A class that extends the `Calendar` class
is the `GregorianCalendar`, which uses the standard calendar that most of the
world uses today.

```java
import java.util.Calendar;
import java.util.GregorianCalendar;

public class CalendarExample {

    public static void main(String[] args) {

        // Will instantiate the calendar with the default time zone and local date
        Calendar calendar1 = Calendar.getInstance();

        // Will instantiate the calendar with a GregorianCalendar using the default
        // time zone, format, and current time
        Calendar calendar2 = new GregorianCalendar();
    }
}
```

Now that we have a `Calendar` instance, let's look at some of its methods!

### Get

If we want to get a certain field from a `Calendar` instance, like the year or
day of the month, we can do so like this:

```java
import java.util.Calendar;

public class CalendarExample {

    public static void main(String[] args) {
        
        Calendar calendar1 = Calendar.getInstance();
        
        // Calendar.YEAR will return the year field; in this case, it will return the current year
        System.out.println("The year is " + calendar1.get(Calendar.YEAR));
        
        /* Calendar.MONTH will return the month field; in this case, it will return the current month
           NOTE: The Calendar class starts counting the months with January == 0
           This means that if the current month is September, then Calendar.MONTH will return 8 instead
           of 9
        */
        System.out.println("The month is " + calendar1.get(Calendar.MONTH));
        
        /* Calendar.DAY_OF_MONTH will return the day of month field;
           In this case, it will return the current day of the month
         */
        System.out.println("The day is " + calendar1.get(Calendar.DAY_OF_MONTH));

    }
}
```

There are other fields that we can get from the `Calendar` class as well. For
a full list, review the Java documentation here and see the "Field Summary"
section: [Java 11 Calendar Class](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Calendar.html).
Some of the more important fields can be found in the table below:

| Field        | Description                                        |
|--------------|----------------------------------------------------|
| DAY_OF_MONTH | Gets and Sets the Day of the Month                 |
| DAY_OF_YEAR  | Gets and Sets the Day of the Year                  |
| HOUR         | Gets and Sets the Hour of the morning or afternoon |
| HOUR_OF_DAY  | Gets and Sets the Hour of the Day                  |
| MINUTE       | Gets and Sets the Minute within the Hour           |
| MONTH        | Gets and Sets the Month (January == 0)             |
| SECOND       | Gets and Sets the Second within the Minute         |
| YEAR         | Gets and Sets the Year                             |

Now if we want to get the full date, we could call the `getTime()` method.

```java
import java.util.Calendar;

public class CalendarExample {

    public static void main(String[] args) {
        
        Calendar calendar1 = Calendar.getInstance();
        
        // Will print the current date and time
        System.out.println(calendar1.getTime());

    }
}
```

An example output could be something like this:

```plaintext
Mon Sep 12 18:31:02 MDT 2022
```

### Set

We can also set the `Calendar` instance to different fields using the `set()`
method and the fields in the list we saw above! There are four different
`set()` methods that we will see in the example below.

```java
import java.util.Calendar;

public class CalendarExample {

    public static void main(String[] args) {

        // Let's travel back in time to the year November 5, 1955 at 12:00:00
        // Remember that November == 10 in this case
        
        Calendar calendar1 = Calendar.getInstance();
        
        // We will start by printing the current date time
        System.out.println(calendar1.getTime());
        
        // Then we will set just the year
        // This method takes in the field and the value we want to set the field to: set(int field, int value)
        calendar1.set(Calendar.YEAR, 1955);
        System.out.println(calendar1.getTime());
        
        // Now let's set the year, month, and day all in one method call
        // This method takes in the year field, the month field, and the day of the month field:
        // set(int year, int month, int date)
        calendar1.set(1955, 10, 5);
        System.out.println(calendar1.getTime());
        
        // Let's specify the hour and the minute
        // This method takes in the year, month, day of the month, hour, and minute fields:
        // set(int year, int month, int date, int hour, int minute)
        calendar1.set(1955, 10, 5, 12, 0);
        System.out.println(calendar1.getTime());
        
        // If we want to be extra specific, we could even set the second field
        // This method takes in the year, month, day of the month, hour, minute, and second fields:
        // set(int year, int month, int date, int hour, int minute, int second)
        calendar1.set(1955, 10, 5, 12, 0, 0);
        System.out.println(calendar1.getTime());

    }
}
```

Upon running the code, we might have an output that looks something like this:

```plaintext
Mon Sep 12 06:53:41 MDT 2022
Mon Sep 12 06:53:41 MST 1955
Sat Nov 05 06:53:41 MST 1955
Sat Nov 05 12:00:41 MST 1955
Sat Nov 05 12:00:00 MST 1955
```

### Add

We could even add time to our `Calendar` instance using the `add()` method. The
`add()` method takes in the field we want to add to and the value of how much we
would like to add:

```java
import java.util.Calendar;

public class Example {

    public static void main(String[] args) {

        // Suppose we have gone back to the 1955, and now we
        // want to go forward 30 years
        
        Calendar calendar1 = Calendar.getInstance();
        calendar1.set(1955, 10, 5, 12, 0, 0);
        System.out.println(calendar1.getTime());
        
        // Let's go forward in time by 30 years!
        calendar1.add(Calendar.YEAR, 30);
        System.out.println(calendar1.getTime());

    }
}
```

The output of the above code may look like this:

```plaintext
Sat Nov 05 12:00:00 MST 1955
Tue Nov 05 12:00:00 MST 1985
```

## Conclusion

The `Calendar` class is a step in the right direction - but it still isn't
perfect. Both the `Calendar` and the `Date` classes are considered to be in
the "legacy API" (AKA, they're older classes). In Java 8, the Date and Time API
rolled out with some much needed improvements. We'll talk about some of those
classes in the next couple of lessons and highly recommending using those over
the `Date` and `Calendar` classes we have discussed in this lesson.

You might be thinking, "Why did we learn about older classes then?" It is
critical to know where we came from - especially since these classes are not
deprecated completely and may still be in use. As a software engineer, you may
encounter code that still use the `Date` and `Calendar` class. It's important to
at least have an understanding of these classes and how they work.

## Resources

- [Unix Epoch Timestamps](https://www.epochconverter.com/)
- [Java 11 Date Class](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Date.html)
- [Java 11 Calendar Class](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Calendar.html)
- [Java 11 GregorianCalendar Class](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/GregorianCalendar.html)
