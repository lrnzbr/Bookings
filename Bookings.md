
# Room Bookings Challenge

## Problem Statement

It is important for us to know when something goes wrong or our key business metrics

are moving outside of our expectations. To this end, we set up alerts that are triggered when a

metric moves outside of our expectations. These alerts allow us to be proactive in fixing

problems rather than waiting for catastrophes.

The product team has asked you to follow the number of bookings per day in two key markets

and alert them when the daily fluctuation is too high. For this alert, they’ve asked you to create

a 30­day rolling average and alert them when the daily value is above or below two standard

deviations of the rolling mean.

We have provided a dataset in csv format for you to test out your alert. When you apply this alert

over the past year for these two markets, how many alerts would have been triggered by this rule

for each market?


```python
import pandas as pd
import numpy as np
import datetime
from datetime import date, timedelta


def ActivityAlarm(datafile):
    HighAlertCity1 = 0
    LowAlertCity1 = 0
    HighAlertCity2 = 0
    LowAlertCity2 = 0
    
    dataframe = pd.read_csv(datafile)
    citydict = dict()
    
    for i in xrange(0, dataframe.shape[0]):
        city = dataframe.iloc[i]['city']
        date = dataframe.iloc[i]['ds']
        bookings = dataframe.iloc[i]['bookings']
        #Has my algorithm seen this city before? If not, add it to a dictionary of dictionaries containing each city and the number of bookings for each seen day.
        if city not in citydict:
            citydict[city] = dict()
            citydict[city][date] = dict()
            citydict[city][date] = bookings
            pass
        elif city in citydict:
            citydict[city][date] = bookings
        #Do I have at least 30 days of recent activity? If so, I have enough dates to calculate a moving average, if not, add this date to my dictionary and continue.
        if len(citydict[city]) >= 30:
            thirtydaysbookings = list()
            #Gather the bookings for the past 30 days.
            for i in xrange(30):
                d = datetime.datetime.strptime(date, "%m/%d/%y") - timedelta(days=i)
                #Check and make sure there is value for that date in the table
                date_new = str(d.month) +'/' + str(d.day) + '/'+str(d.year)[2:4]
                if date_new not in citydict[city]:
                    print "Warning! Gap in daily data -- %s not found for %s!\n" % (date_new, city)
                else:
                    thirtydaysbookings.append(citydict[city][date_new])
            #CALCULATE THE MOVING AVERAGE AND STANDARD DEVIATION
            if len(thirtydaysbookings)==30:
                averagebookings = np.mean(thirtydaysbookings)
                stddevbookings = np.std(thirtydaysbookings)
                if bookings <= averagebookings - 2.0*stddevbookings:
                    print "Low Alert!-- In %s on %s there were %s bookings, more than 2 standard deviations (%s ) below the moving average of %s \n" % (city, date, bookings,2 * stddevbookings, averagebookings)
                    if city == "City_1":
                        LowAlertCity1 += 1
                    else:
                        LowAlertCity2 += 1
                elif bookings >= averagebookings + 2.0*stddevbookings:
                    print "High Alert!-- In %s on %s there were %s bookings, more than 2 standard deviations (%s ) above the moving average of %s \n" % (city, date, bookings,2 * stddevbookings, averagebookings)
                    if city == "City_1":
                        HighAlertCity1 += 1
                    else:
                        HighAlertCity2 += 1
                else:
                    pass #<--- No unusual fluctuation detected 
    print "High Alert Count City_1:", HighAlertCity1
    print "Low Alert Count City_1:", LowAlertCity1
    print "High Alert Count City_2:", HighAlertCity2
    print "Low Alert Count City_2: ", LowAlertCity2
    
        
            

    
    
```


```python
ActivityAlarm('bookings.csv')
```

    High Alert!-- In City_2 on 2/13/13 there were 223 bookings, more than 2 standard deviations (54.464667446 ) above the moving average of 152.0 
    
    High Alert!-- In City_2 on 2/20/13 there were 221 bookings, more than 2 standard deviations (55.1533216487 ) above the moving average of 161.166666667 
    
    High Alert!-- In City_2 on 4/2/13 there were 283 bookings, more than 2 standard deviations (59.8717147092 ) above the moving average of 206.333333333 
    
    High Alert!-- In City_2 on 6/19/13 there were 326 bookings, more than 2 standard deviations (80.3427380387 ) above the moving average of 243.833333333 
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Warning! Gap in daily data -- 6/24/13 not found for City_2!
    
    Low Alert!-- In City_2 on 8/24/13 there were 209 bookings, more than 2 standard deviations (104.483682937 ) below the moving average of 317.7 
    
    Low Alert!-- In City_2 on 8/31/13 there were 210 bookings, more than 2 standard deviations (104.108063515 ) below the moving average of 314.666666667 
    
    Low Alert!-- In City_2 on 10/19/13 there were 173 bookings, more than 2 standard deviations (101.470521171 ) below the moving average of 284.0 
    
    Low Alert!-- In City_2 on 10/20/13 there were 163 bookings, more than 2 standard deviations (110.357882465 ) below the moving average of 279.866666667 
    
    High Alert!-- In City_1 on 2/5/13 there were 34 bookings, more than 2 standard deviations (9.46197066625 ) above the moving average of 22.8666666667 
    
    High Alert!-- In City_1 on 2/6/13 there were 38 bookings, more than 2 standard deviations (10.5369192208 ) above the moving average of 23.1 
    
    High Alert!-- In City_1 on 2/13/13 there were 35 bookings, more than 2 standard deviations (10.3728277512 ) above the moving average of 24.3666666667 
    
    High Alert!-- In City_1 on 2/26/13 there were 46 bookings, more than 2 standard deviations (13.3900460542 ) above the moving average of 26.1 
    
    Low Alert!-- In City_1 on 3/16/13 there were 13 bookings, more than 2 standard deviations (14.0374103341 ) below the moving average of 29.0666666667 
    
    High Alert!-- In City_1 on 4/2/13 there were 44 bookings, more than 2 standard deviations (12.7994791561 ) above the moving average of 29.1 
    
    Low Alert!-- In City_1 on 4/20/13 there were 16 bookings, more than 2 standard deviations (11.7494207895 ) below the moving average of 28.4333333333 
    
    High Alert!-- In City_1 on 5/8/13 there were 45 bookings, more than 2 standard deviations (11.4970527624 ) above the moving average of 28.4333333333 
    
    High Alert!-- In City_1 on 5/20/13 there were 45 bookings, more than 2 standard deviations (12.1488911245 ) above the moving average of 29.6333333333 
    
    High Alert!-- In City_1 on 6/3/13 there were 45 bookings, more than 2 standard deviations (12.238554744 ) above the moving average of 32.2333333333 
    
    High Alert!-- In City_1 on 6/27/13 there were 49 bookings, more than 2 standard deviations (12.4951546164 ) above the moving average of 35.3666666667 
    
    High Alert!-- In City_1 on 7/1/13 there were 51 bookings, more than 2 standard deviations (13.4470153648 ) above the moving average of 36.8333333333 
    
    High Alert!-- In City_1 on 7/16/13 there were 69 bookings, more than 2 standard deviations (17.5171027538 ) above the moving average of 41.4333333333 
    
    High Alert!-- In City_1 on 8/28/13 there were 67 bookings, more than 2 standard deviations (15.7953579679 ) above the moving average of 48.6 
    
    High Alert!-- In City_1 on 9/2/13 there were 69 bookings, more than 2 standard deviations (18.2404921961 ) above the moving average of 49.7666666667 
    
    High Alert!-- In City_1 on 9/9/13 there were 76 bookings, more than 2 standard deviations (21.3087358194 ) above the moving average of 51.8666666667 
    
    High Alert!-- In City_1 on 11/4/13 there were 96 bookings, more than 2 standard deviations (23.5668316826 ) above the moving average of 68.5333333333 
    
    High Alert!-- In City_1 on 11/6/13 there were 96 bookings, more than 2 standard deviations (24.9836390909 ) above the moving average of 69.7666666667 
    
    High Alert!-- In City_1 on 11/11/13 there were 97 bookings, more than 2 standard deviations (25.915160728 ) above the moving average of 71.0333333333 
    
    Low Alert!-- In City_1 on 11/23/13 there were 39 bookings, more than 2 standard deviations (29.1658857038 ) below the moving average of 69.2666666667 
    
    High Alert Count City_1: 17
    Low Alert Count City_1: 3
    High Alert Count City_2: 4
    Low Alert Count City_2:  4

