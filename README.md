
[![Arduino CI](https://github.com/RobTillaart/runTime/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/runTime/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/runTime/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/runTime/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/runTime/actions/workflows/jsoncheck.yml)
[![GitHub issues](https://img.shields.io/github/issues/RobTillaart/runTime.svg)](https://github.com/RobTillaart/runTime/issues)

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/runTime/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/runTime.svg?maxAge=3600)](https://github.com/RobTillaart/runTime/releases)
[![PlatformIO Registry](https://badges.registry.platformio.org/packages/robtillaart/library/runTime.svg)](https://registry.platformio.org/libraries/robtillaart/runTime)


# runTime

Arduino library to measure cumulative series of run times.


## Description

**Experimental**

This library is used to add up one or more periods of time.
This sum is counted in seconds.
The goal of the library is e.g. to track the runtime (uptime or elapsed time)
of a device over longer periods of time.
This runtime can be requested in seconds, minutes, hours or days.
Besides the runtime the library also counts how often 
the clock (== device) has started.
Finally the library can report the average runtime in seconds, 
minutes, hours or days.

The maximum duration is 49.710 days or about 136 years.

The library uses **millis()** so the internal values need to be 
updated at least once per 49 days when running continuously.
This must be done by calling **update()**.
When the clock is stopped one does not need to update every 49 days.

The library does truncate the **millis()** internally when converting 
to seconds, however it tracks the milliseconds remaining to add later.
This improves the accuracy, especially when the individual runs are 
relative short.

The library does not track the "operational time" (yet). 
Therefore it is not possible to calculate the runtime as a percentage
of the total time.

Note: this library is only as accurate as the underlying millis() is.
The library does not support compensation for drift.

Note: the library does not keep the counting persistent over reboots.

Feedback as always is welcome.


### Related

- https://github.com/RobTillaart/DS1682 - hardware runtime counter
- https://github.com/RobTillaart/DS1683 - hardware runtime counter
- https://github.com/RobTillaart/millis64
- https://github.com/RobTillaart/stopWatch_RT

Other
- https://github.com/RobTillaart/dateTimeHelpers - formatting
- https://github.com/RobTillaart/printHelpers


### Tested

On Arduino UNO R3.


## Interface

```cpp
#include "runTime.h"
```

### Constructor

- **runTime()** creates the time counter and resets internals.

### Control

- **void start()** starts the internal counting. If the counting is running
it is not started again. One must use reset() or stop() first.
- **void stop()** stops the internal counting and adds the last run to the
internal runtime counter. 
- **void update()** updates the internal counting.
- **void reset(uint32_t startValue = 0)** resets the internal counters to zero.
Optional one can set a number of start seconds
- **bool isRunning()** returns true is counting is running.

### Measurement

- **uint32_t seconds()** returns the seconds runtime, includes the active run.
- **float minutes()** wrapper around seconds, partial minutes is in decimal format.
- **float hours()** wrapper around seconds, partial hours is in decimal format.
- **float days()** wrapper around seconds, partial days is in decimal format.
- **uint32_t runCount()** returns the number of runs (== starts).

### Statistics

- **float averageSeconds()** returns seconds / runCount.
- **float averageMinutes()** returns minutes / runCount.
- **float averageHours()** returns hours / runCount.
- **float averageDays()** returns days / runCount.


### PrintTo

The class implements the **Printable** interface, allowing to
print the object directly.
Currently it returns seconds, might change in the future.

```cpp
runTime rt;
...
Serial.println(rt);
```

## Future

#### Must

- improve documentation
- test

#### Should

- investigate persistence over reboot support.
  - external storage?
  - watchdog persistence?
- investigate operational time.
  - runtime percentage
- investigate drift support
  - drift depends on runtime, interrupts and more?
  - driftCorrect(ms) - call daily / hourly?
  - setDriftFactor(float) - 

#### Could

- add examples
- add unit tests (if possible)
- redefine printTo() layout?
- access to remainder?


#### Wont


## Support

If you appreciate my libraries, you can support the development and maintenance.
Improve the quality of the libraries by providing issues and Pull Requests, or
donate through PayPal or GitHub sponsors.

Thank you,


