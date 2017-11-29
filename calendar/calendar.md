# How to generate Calendar Dates

Generate one month, since usually only one calendar month is visible at a time.

__*The main objective is to figure out how many days the requested month has (28, 29, 30, or 31).*__

Create function that takes in the __month__ needed and __year__.
@params __month__ is a number 0-11 and year is a number. 

```
  generateCalendarDates(year, month){ }
  
```
Next lets create a couple variables:

* an empty array inside the function that will hold the generated CalendarDates
* last Day  - the last date of the month
* start - the first day of the month

```
  generateCalendarDates(year, month){ 
      
     let calendarDates = [];
     
     let lastDay;
      
     let start;
  
  }
  
```
Notice the difference between __date__ and __day__. __Day__ is the actual number of the day in the week. For example Sunday is 0, Monday is 1, Tuesday is 2, etc. I need the day number later when I am populating the actual calendar and can match the start day with the day of the week. 

First lets figure out what the last __date__ of that month is.
We will use the JS [Date Obj](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date). 
The Date Object creates an instance of a date starting 1 January, 1970 UTC.

To get the last __date__ of the month we need to __step in to the next month__ ( *month + 1*), and pass 0 as the __day__, which will select the last day of the previous month - bringing us back to the current month. 

 ```javascript
  
  generateCalendarDates(year, month){ 
      
     let calendarDates = [];
     
     let start;
     
    /* to get last day of month, step into next month,
        the 0 will select the last day of prev month */
    
    let lastDay = new Date(year, month +1, 0).getDate();
    
    /* add +1 to the month to step into next month and the 0 for day will bring the month back to current month */
  
  }
  
```   
We can get the start day of the month by calling the .getDay() function on a new Date Object.

```javascript
    
    let start = new Date(year, month, 1).getDay();

```
And now we can run a for loop that generates all of the dates in a month 1 thru lastDay.

```javascript
  
  for (let i = 1; i <= lastDay; i++) {
    calendarDates.push(i);
    /* push the dates into the array */
  }

```

The function looks like this right now:

```javascript
 generateCalendarDates(year, month){ 
      
     let calendarDates = [];
     
     let start = new Date(year, month, 1).getDay();
     
     let lastDay = new Date(year, month +1, 0).getDate();
     
     for (let i = 1; i <= lastDay; i++) {
        calendarDates.push(i);
     }
    
```

```javascript
    for(var i = 1; i <= lastDay; i++){
      let dateObj: CalendarObj = new CalendarObj(undefined, undefined);
      dateObj.day = CALENDAR.WEEKDAYS[new Date(year, month, i).getDay()];
      dateObj.date = i;
      calendarDates.push(dateObj);
    }
    return {dates: calendarDates, start: start};
```

# Generate the Calendar

The purpose of this function is to generate a data structure that I can iterate over and create a calendar. The main challenge is to match the dates with the day of the week headers.
For example the November 2017 would look like this at the end:

Sun | Mon | Tue | Wed | Thu | Fri | Sat |
--- | --- | --- | --- | --- | --- | ---
''  |'' | ''|  1  |  2  |  3  |  4 
  5 |  6  |  7  |  8  |  9  | 10  | 11 
 12 | 13  | 14  | 15 | 16  | 17  | 18 
 19 | 20 | 21  | 22 | 23  | 24 | 25
 26 | 27 | 28  | 29 | 30  | '' | ''




