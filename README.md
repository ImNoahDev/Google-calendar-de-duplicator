## How it works:
The script automatically deletes any google calendar events that have the same name and occur at the same time from a year ago to a year from the date it was run.
This allows for deletion of duplicate google calendar events.

## How to use:
Copy the script from the deduplicator file and paste into google apps script.
run the script and set a trigger to run it daily.

## Code:

```
function removeDuplicateEvents() {
  var calendarName = "Timetable"; // Name of your calendar
  var calendar = CalendarApp.getCalendarsByName(calendarName)[0];

  // Calculate date range from a year ago to a year from today
  var today = new Date();
  var oneYearAgo = new Date(today.getFullYear() - 1, today.getMonth(), today.getDate());
  var oneYearFromToday = new Date(today.getFullYear() + 1, today.getMonth(), today.getDate());
  
  var events = calendar.getEvents(oneYearAgo, oneYearFromToday);

  var eventMap = {}; // Map to store unique event details
  var deletedEventsLog = []; // Array to store details of deleted events

  // Iterate through events to identify and remove duplicates
  events.forEach(function(event) {
    var eventKey = event.getTitle() + "|" + event.getStartTime().getTime(); // Generating a unique key based on event title and start time
    if (eventMap[eventKey]) {
      // Duplicate event found, delete it
      event.deleteEvent();
      // Log the deleted event
      deletedEventsLog.push("Deleted event: " + event.getTitle() + " at " + event.getStartTime());
    } else {
      // Unique event, store it in the map
      eventMap[eventKey] = true;
    }
  });

  // Output the log of deleted events
  if (deletedEventsLog.length > 0) {
    Logger.log("Deleted Events:");
    deletedEventsLog.forEach(function(log) {
      Logger.log(log);
    });
  } else {
    Logger.log("No duplicate events found and deleted.");
  }
}
```
