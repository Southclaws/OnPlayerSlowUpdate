# OnPlayerSlowUpdate

[![sampctl](https://shields.southcla.ws/badge/sampctl-OnPlayerSlowUpdate-2f2f2f.svg?style=for-the-badge)](https://github.com/Southclaws/OnPlayerSlowUpdate)

This library provides the event `OnPlayerSlowUpdate` which is like
`OnPlayerUpdate` except it only fires every 100ms instead of every ~20ms. This
allows multiple packages using tickers to align to a more central point of truth
for sync etc.

It's simply a y_timers ptask under the hood, and you are encouraged to use these
more often than a monolithic timer but sometimes it's necessary to call events
in some libraries in sequence in the same callback chain. This library is for
those situations where `OnPlayerUpdate` is too fast and unpredictable (the time
between calls varies a lot depending on many factors).

Throughout Scavenge and Survive, there were many modules that ran on a 100ms
timer, a lot of these modules queried each other for data. At one point I
started to add events to each module so it then consisted of a ton of timers
calling a ton of further events and it became ridiculous. This was one case that
the architecture fit better around a central "event loop" kind of mechanism that
consisted of a single callback that was fired every 100ms and allowed each
module to communicate without unnecessary events and slightly out of sync data.

This is mainly just a fix for a problem I introduced myself, eventually, this
will most likely disappear in favour of a better architecture.

## Installation

Simply install to your project:

```bash
sampctl package install Southclaws/OnPlayerSlowUpdate
```

Include in your code and begin using the library:

```pawn
#include <OnPlayerSlowUpdate>
```

## Usage

Simply hook `OnPlayerSlowUpdate` and shove your code in it:

```pawn
hook OnPlayerSlowUpdate(playerid) {
    doStuff();
}
```

Return values have no effect.
