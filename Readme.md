# Teensy4Time Library

Teensy4Time is a simplified time library for the Teensy 4.x built-in RTC. It is a altered version of the [Time library](https://github.com/PaulStoffregen/Time) included in the Teensy4 core which is originally by Michael Margolis and maintained by Paul Stoffregen.

The goal of this library is to provide a really easy way to interface with the built-in Teensy 4.x RTC

## Functionality

To use this library in an Arduino sketch, include `Teensy4Time.h`.

```c
#include <TimeLib.h>
```

The functions available in the library include:

```c
now();             // returns the current time as seconds since Jan 1 1970
time_t t = now();  // example of storing the current time in time variable t

// these functions automatically call now() to get the latest time
hour();            // returns the hour now (0-23)
minute();          // returns the minute now (0-59)
second();          // returns the second now (0-59)
day();             // returns the day now (1-31)
weekday();         // returns day of the week (1-7), Sunday is day 1
month();           // returns the month now (1-12)
year();            // returns the full four digit year: (2009, 2010 etc)
```

There are also functions to return the hour in 12-hour format:

```c
// these functions automatically call now() to get the latest time
hourFormat12();    // returns the hour now in 12 hour format
isAM();            // returns true if time now is AM
isPM();            // returns true if time now is PM
```

Functions for managing the RTC include:

```c
setTime(t);                      // set the RTC to the give time t
setTime(hr,min,sec,day,mnth,yr); // alternative to above, yr is 2 or 4 digit yr
                                 // (2010 or 10 sets year to 2010)
adjustTime(adjustment);          // adjust RTC time by adding the adjustment value (in seconds)
```

The time and date functions can take an optional parameter for the time. This prevents
errors if the time rolls over between elements. For example, if a new minute begins
between getting the minute and second, the values will be inconsistent. Using the
following functions eliminates this problem:

```c
hour(t);               // returns the hour for the given time t
minute(t);             // returns the minute for the given time t
second(t);             // returns the second for the given time t
day(t);                // returns the day for the given time t
weekday(t);            // returns day of the week for the given time t
month(t);              // returns the month for the given time t
year(t);               // returns the year for the given time t

hourFormat12(t);       // returns the hour for the given time t in 12 hour format
isAM(t);               // returns true if the given time t is AM
isPM(t);               // returns true if the given time t is PM

adjustTime(t, adjustment);  // returns adjusted time_t by adding the adjustment value (in seconds) to 't'
adjustTime(&t, adjustment); // adjusts value of 't' by adding the adjustment value (in seconds)
                            // to the value at the address of 't'
```

There are many convenience macros in the `Teensy4Time.h` file for time constants and conversion
of time units.

To use the library, copy the download to the Library directory.

## Example

You can find the `TimeTeensy4` example in the `examples` folder

## Technical Notes

Internal system time is based on the standard Unix `time_t`.
The value is the number of seconds since Jan 1, 1970.

The Teensy4Time library defines a structure for holding time elements that is a compact version of the C `tm` structure.
All the members of the Arduino `tm` structure are bytes and the year is offset from 1970.
Convenience macros provide conversion to and from the Arduino format.

Low-level functions to convert between system time and individual time elements are provided:

```c

breakTime(time, &tm);  // break time_t into elements stored in tm struct
makeTime(&tm);         // return time_t from elements stored in tm struct
```
