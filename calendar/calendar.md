# How to generate Calendar Dates

Generate one month, since usually only one calendar month is visible at a time.

__*The main objective is to figure out how many days the requested month has.*__

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
Notice the difference between __date__ and __day__. __Day__ is the actual number of the day in the week. For example Sunday is 0, Monday is 1, Tuesday is 2, etc. 

First lets figure out what the last __date__ of that month is.
We will use the JS [Date Obj](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date). 
The Date Object creates an instance of a date starting 1 January, 1970 UTC.

To get the last __date__ of the month we need to *step in to the next month*  __month + 1__, and pass 0 as the __day__, which will select the last day of the previous month.

 ```
  generateCalendarDates(year, month){ 
      
     let calendarDates = [];
     
     let start;
     
    /* to get last day of month, step into next month,
        the 0 will select the last day of prev month */
    
    let lastDay = new Date(year, month +1, 0).getDate();
    
    /* add +1 to the month to step into next month and the 0 for day will bring the month back to current month */
  
  }
  
```   


    let start = new Date(year, month, 1).getDay();

    for(var i = 1; i <= lastDay; i++){
      let dateObj: CalendarObj = new CalendarObj(undefined, undefined);
      dateObj.day = CALENDAR.WEEKDAYS[new Date(year, month, i).getDay()];
      dateObj.date = i;
      calendarDates.push(dateObj);
    }

    return {dates: calendarDates, start: start};

  }

```



