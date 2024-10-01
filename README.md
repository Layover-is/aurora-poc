# aurora-poc
Aurora Borealis PoC

Premis: A web component that might later be wrapped into an app.

The idea is to provide the end user information on wether they can see the northern lights (NL from now on) in the next three days (this can only reliably be forcasted 3 days ahead)

There are 3 main factors that predict wether one can see it
 1) Is there any NL activity ?
 2) Is it cloudy where I am ?
 3) Is it dark enough outside to see it ?

The building blocks I have found to answer these questions:

1) Forecast data from GFZ German Research Centre for Geosciences (https://www.gfz-potsdam.de/en/) which gives us a number from 1 to 8 which indicates the solar activity hitting the earth which correlates to likeliness of seeing the NL.

The data is provided as 3 hour time frames.

I have taken the relevant parts out their much larger dataset and provide it here: https://api.layover.lol/aurora/forecast/3h-window.json

Example:
```
{
    "time": "02-10-2024 15:00",
    "median": 1.7,
    "maximum": 3.0
},
{
    "time": "02-10-2024 18:00",
    "median": 1.7,
    "maximum": 2.7
}
```

2) openweathermap.org does provide cloud forcast map tiles that are transparent and can be projected over any other map, a rather bad example can be found here:
https://openweathermap.org/weathermap?basemap=map&cities=false&layer=clouds&lat=65&lon=-18.5&zoom=7

The reason I call it a bad example is due to white clouds are projected over a white map, there are many other options for map tiles, examples:
 - https://openmaptiles.org/
 - https://protomaps.com/

This example also does provide an example of a slider that one can drag to see future cloud tiles overlayed.

There are also some pay to use maps, and I wouldn't mind that if the perfect fit wouldn't be found in the OSS space


3) The icelandic Met Office does provide this info in an XML api which I have made a proxy for here: https://api.layover.lol/aurora/forecast/sun-moon-set.json (Nobody likes XML)

Possibly we wouldn't need this api since this can be calculated given lat/lon and time, but for now this api has the goods, example:
```
            {
                "evening_date": "2024-10-03",
                "activity_forecast": "1",
                "sun": {
                    "sunset": "18:49",
                    "darkness": "19:36",
                    "dawn": "06:59",
                    "sunrise": "07:46"
                },
                "moon": {
                    "age": "1",
                    "schedule_type": "0",
                    "schedule_description": "Moon does not rise",
                    "moonrise": null,
                    "moonset": null
                }
            }
```
The time between `sun:darkness` and `sun:dawn` is the best time to be able to see the NL, moon:age might play a role too, to understand these `moon:age` numbers it's best to see them mapped out here: https://www.almanac.com/astronomy/moon/calendar
I still don't know the exact effect of the moon in this case, but the answer could lie somewhere in "the moon could be an issue if the number is between 11 and 24 f.ex)

**Presentation**

The presentation of the data could be the tricky bit, my idea is that a novice user could get an answer to the question "Can I see the NL tonight or in the next 3 days?"

I had spent quite some time thinking about this when I got sent a screenshot that more or less had all the ideas I had gotten so far, the future values from the solar activity on a timeline with a bar graph, darkness/brightness on the same timeline.
Clickable time slots (shown in the screenshot as 1h window, but I have not found any data with smaller than 3h windows, they show the same values in all 3 timeslots so they are probably using the same 3h data)
When you click the timeslot the map would show correct cloud tiles for that time.

Screenshot: https://is1-ssl.mzstatic.com/image/thumb/PurpleSource126/v4/b4/cf/7b/b4cf7b33-7ab1-a701-2990-f31b957e75b7/db63a22b-a075-4759-9392-4da0baf9f8ca_2-67.png/460x0w.webp



    
