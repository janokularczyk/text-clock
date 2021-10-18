# Text Clock
project developed by Jan Okularczyk

## Description: 
The following program is used to create a simple textual clock. Project was developed as a terminal application with usage of functional programming and ANSI colors.

Program prints the current time in the console window on display created with letters. It rounds off the current time to 5min (41 -> 45). For better visibility letters representing current time are coloured using ANSI color code. 

```
I T L I S A S A M P M
A C Q U A R T E R D C
T W E N T Y F I V E X
H A L F B T E N F T O
P A S T E R U N I N E
O N E S I X T H R E E
F O U R F I V E T W O
E I G H T E L E V E N
S E V E N T W E L V E
T E N S E O'C L O C K
```

## Time:
In order to get current time the Time class has to be imported from Java library. String containing the value has to be stripped to two substrings because of LocalTime data format (HH:MM:SS.ss) and then converted to Integer value in order to work on intervals. 

``` java
String time = java.time.LocalTime.now().toString();
int hours = Integer.parseInt(time.substring(0, 2));
int minutes = Integer.parseInt(time.substring(3, 5));

getTimeOfDay(hours, minutes);
displayTime(hours, minutes);
```

After that two variables are passed to two methods `getTimeOfDay()` and `displayTime()`. Which are responsible for printing textual display in the console and colouring the right words.  

## Colors:
By using ANSI colour codes the user has the ability to colour the current time in the console window. Three `final` variables have to be created, one responsible for resetting colour text in the console window and two responsible for colouring the text. 

``` java
public static final String ANSI_RESET = "\u001b[0m";
public static final String ANSI_GREEN = "\u001b[32m";
public static final String ANSI_PURPLE = "\u001b[35m";
```

Now to use colours in the console user only has to provide the name of the final String before the word and reset it after the word ends `ANSI_GREEN + "P M" + ANSI_RESET`.  

## Methods:
Overall program uses four methods from which two are invoked from the 'main' method `getTimeOfDay()` and `displayTime()` and two invoked by `displayTime()` method. 
1. `getTimeOfDay()`
2. `displayTime()`
3. `getHour()` and `getMinutes()`

### §1. getTimeOfDay()
This method is responsible for outputting the time of the day (AM/PM) because of that our display only operates in twelve hours format and LocalTime is using twentyfour format. We have to define the time of the day. 

``` java
public static void getTimeOfDay(int hours, int minutes) {
    String s;
    if (hours > 12 || (hours == 0 && minutes == 0)) {
        s = "A S A M " + ANSI_GREEN + "P M" + ANSI_RESET;
    } else {
        s = "A S " + ANSI_GREEN + "A M " + ANSI_RESET + "P M";
    }
    System.out.println(ANSI_PURPLE + "I T " + ANSI_RESET + "L " + ANSI_PURPLE + "I S " + ANSI_RESET + s);
}```

Integer containing houres value and minutes value is passed to the method and method checks if the value is bigger than twelve or if it is equal to zero. This method also colours the part of code that is always repeated `IT` and `IS `. 

### §2. displayTime()
This method is responsible for the main logic of the program it calls methods 'getHours()` and `getMinutes()`. 

```java
public static void displayTime(int hours, int minutes) {
    String hoursOut;
    String minutesOut;
    if (minutes == 0) {
        hoursOut = getHour(hours);
        System.out.println("A C Q U A R T E R D C\n" +
                "T W E N T Y F I V E X\n" +
                "H A L F B T E N F T O\n" +
                "P A S T E R U " + hoursOut + ANSI_PURPLE + "O'C L O C K" + ANSI_RESET);
    } else if (minutes > 30 && minutes < 55) {
        if (hours == 23) {
            hoursOut = getHour(0);
        } else {
            hoursOut = getHour(hours + 1);
        }
        minutesOut = getMinutes(minutes);
        System.out.println(minutesOut + hoursOut + "O'C L O C K" );
    } else if (minutes > 55) {
        if (hours == 23) {
            hoursOut = getHour(0);
        } else {
            hoursOut = getHour(hours + 1);
        }
        System.out.println("A C Q U A R T E R D C\n" +
                "T W E N T Y F I V E X\n" +
                "H A L F B T E N F T O\n" +
                "P A S T E R U " + hoursOut + ANSI_PURPLE + "O'C L O C K" + ANSI_RESET);
    } else {
        hoursOut = getHour(hours);
        minutesOut = getMinutes(minutes);
        System.out.println(minutesOut + hoursOut + "O'C L O C K");
    }

}
```
It runs a simple algorithm that takes care of all cases. It creates two local String variables `hoursOut` and `minutesOut` which are getting value from calling methods 'getHours()` and `getMinutes()` according to algorithm logic. 

In some cases there is no need to call every method for example when an hour is full:

```java
if (minutes == 0) {
    hoursOut = getHour(hours);
    System.out.println("A C Q U A R T E R D C\n" +
            "T W E N T Y F I V E X\n" +
            "H A L F B T E N F T O\n" +
            "P A S T E R U " + hoursOut + ANSI_PURPLE + "O'C L O C K" + ANSI_RESET);
}

```
We also have to take care of two cases near midnight because of a 5 minute round off.

### §3. getHour() and getMinutes()
Two methods that work on simple logic gates and return colouerd String with current time. They take Integer values of hour and minutes and process them in Intervals.

```java
private static String getHour(int hours) {
    if (hours == 0 || hours == 12) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N " + ANSI_PURPLE + "T W E L V E \n" + ANSI_RESET + "T E N S E ";
    } else if (hours == 1 || hours == 13) {
        return "N I N E\n" + ANSI_PURPLE + "O N E " + ANSI_RESET + "S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 2 || hours == 14) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E " + ANSI_PURPLE + "T W O\n" + ANSI_RESET + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 3 || hours == 15) {
        return "N I N E\n" + "O N E S I X " + ANSI_PURPLE + "T H R E E\n" + ANSI_RESET + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 4 || hours == 16) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + ANSI_PURPLE + "F O U R " + ANSI_RESET + "F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 5 || hours == 17) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R " + ANSI_PURPLE + "F I V E " + ANSI_RESET + "T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 6 || hours == 18) {
        return "N I N E\n" + "O N E " + ANSI_PURPLE + "S I X " + ANSI_RESET + "T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 7 || hours == 19) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + ANSI_PURPLE + "S E V E N " + ANSI_RESET + "T W E L V E\n" + "T E N S E ";
    } else if (hours == 8 || hours == 20) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + ANSI_PURPLE + "E I G H T " + ANSI_RESET + "E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 9 || hours == 21) {
        return ANSI_PURPLE + "N I N E\n" + ANSI_RESET + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    } else if (hours == 10 || hours == 22) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + ANSI_PURPLE + "T E N " + ANSI_RESET + "S E ";
    } else if (hours == 11 || hours == 23) {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T " + ANSI_PURPLE + "E L E V E N\n" + ANSI_RESET + "S E V E N T W E L V E\n" + "T E N S E ";
    } else {
        return "N I N E\n" + "O N E S I X T H R E E\n" + "F O U R F I V E T W O\n" + "E I G H T E L E V E N\n" + "S E V E N T W E L V E\n" + "T E N S E ";
    }
}

private static String getMinutes (int minutes) {
    if (minutes > 0 && minutes <= 5) {
        return "A C Q U A R T E R D C\n" + "T W E N T Y " + ANSI_PURPLE + "F I V E " +ANSI_RESET+ "X\n" + "H A L F B T E N F T O\n" + ANSI_PURPLE +"P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 5 && minutes <= 10) {
        return "A C Q U A R T E R D C\n" + "T W E N T Y F I V E X\n" + "H A L F B " + ANSI_PURPLE + "T E N " + ANSI_RESET + "F T O\n" + ANSI_PURPLE + "P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 10 && minutes <= 15) {
        return ANSI_PURPLE + "A " + ANSI_RESET + "C " + ANSI_PURPLE + "Q U A R T E R " + ANSI_RESET + "D C\n" + "T W E N T Y F I V E X\n" + "H A L F B T E N F T O\n" + ANSI_PURPLE + "P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 15 && minutes <= 20) {
        return "A C Q U A R T E R D C\n" + ANSI_PURPLE + "T W E N T Y " + ANSI_RESET + "F I V E X\n" + "H A L F B T E N F T O\n" + ANSI_PURPLE + "P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 20 && minutes <= 25) {
        return "A C Q U A R T E R D C\n" + ANSI_PURPLE + "T W E N T Y F I V E " + ANSI_RESET + "X\n" + "H A L F B T E N F T O\n" + ANSI_PURPLE + "P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 25 && minutes <= 30) {
        return "A C Q U A R T E R D C\n" + "T W E N T Y F I V E X\n" + ANSI_PURPLE + "H A L F " + ANSI_RESET + "B T E N F T O\n" + ANSI_PURPLE + "P A S T " + ANSI_RESET + "E R U ";
    } else if (minutes > 30 && minutes <= 35) {
        return "A C Q U A R T E R D C\n" + ANSI_PURPLE + "T W E N T Y F I V E " + ANSI_RESET + "X\n" + "H A L F B T E N F " + ANSI_PURPLE + "T O\n" + ANSI_RESET + "P A S T E R U ";
    } else if (minutes > 35 && minutes <= 40) {
        return "A C Q U A R T E R D C\n" + ANSI_PURPLE + "T W E N T Y " + ANSI_RESET + "F I V E X\n" + "H A L F B T E N F " + ANSI_PURPLE + "T O\n" + ANSI_RESET + "P A S T E R U ";
    } else if (minutes > 40 && minutes <= 45) {
        return ANSI_PURPLE + "A " + ANSI_RESET + "C " + ANSI_PURPLE + "Q U A R T E R " + ANSI_RESET + "D C\n" + "T W E N T Y F I V E X\n" + "H A L F B T E N F " + ANSI_PURPLE + "T O\n" + ANSI_RESET + "P A S T E R U ";
    } else if (minutes > 45 && minutes <= 50) {
        return "A C Q U A R T E R D C\n" + "T W E N T Y F I V E X\n" + "H A L F B " + ANSI_PURPLE + "T E N " + ANSI_RESET + "F " + ANSI_PURPLE + "T O\n" + ANSI_RESET + "P A S T E R U ";
    } else if (minutes > 50 && minutes <= 55) {
        return "A C Q U A R T E R D C\n" + "T W E N T Y " + ANSI_PURPLE + "F I V E " + ANSI_RESET +"X\n" + "H A L F B T E N F " + ANSI_PURPLE + "T O\n" + ANSI_RESET + "P A S T E R U ";
    } else {
        return "A C Q U A R T E R D C\n" + "T W E N T Y F I V E X\n" + "H A L F B T E N F T O\n" + "P A S T E R U ";
    }
}
``` 
## Example:
![console example](display.PNG)

