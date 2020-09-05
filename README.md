# Event-Schedule.js - A timeline scheduler

This module allows you to define a timeline comprising events with start and end time, and
have s user-supplied function called when an event is scheduled to start or end.
If an event is already scheduled to be in progress, your function will be called in a
way indicating that.

## Quick start 

* Setup an array of events:

```javascript
const events = [
    { startTime: '2020-08-20T10:00:00Z' },
    { startTime: '2020-08-20T14:00:00Z' },
    { startTime: '2020-08-20T11:00:00Z' },
];
```

* Define your handler function

```javascript
const handleEvent = (state, event) => {
    console.log('State', state, 'signaled for event:', JSON.stringify(event));
};
```

The function should be able to handle the following state values:
  * `start`: The event is starting now.
  * `inProgress`: The event has already started, and hasn't ended. No event with a later start is in the schedule.
  * `end`: The event end time has arrived
  * `past`: The time of this event has passed. There will no no further callbacks for it.

For Typescript the possible values are defined in the type `EventState`.

* Get your events scheduled. Your handler will be called at least once for each event, as its state changes.


```javascript
const es = new ScheduledEventTimeline(events, handleEvent);
es.start();
```

## Details

### Event specification format

The events must include `startTime`, and may optionally include `endTime`. The following formats may be used:

* ISO 8601 timestamp including offset from UTC. E.g.: `2020-08-20T11:00:00Z`, `2020-08-25T11:00:00+02`
* Numeric timestamp in the format returned by `Date.now()` (milliseconds from the epoch).
* Relative time as `hh:mm:ss` or `hh:mm`.

ISO 8601 and numeric timestamps are always interpreted as points in time. Relative timestamps are interpreted a little differently for start and end times. For _start_ times, they're assumed to be relative to the time the `start()` method is called. For _end_ times, they're interpreted as relative to the start time.

### Typescript

This library is written in Typescript and hence provides full type definitions. You're encouraged to look at the exported type and method definitions.
