# Data Import for PV-forecast

## Weather types:

Openweathermap and Buienradar use different weather types and/or have too many subtypes for our ML purpose.

Goal is to define and map both types in order to have 1 database "weather type(s)".

** see: solar_forecast_lighthouse.ipynb **

| Id   | pv-forecast     | Openweathermap   |  Buienradar |
| ---- | ------------    | ---------------- | ----------  |
| 211  | thunderstorm    | >= 200 and < 300      | x |
| 500  | light Rain      | >= 300 and <= 500     | Afwisselend bewolkt met wat lichte regen - F.png |
| 501  | moderate Rain   | = 501                 | Opklaringen en kans op enkele pittige buien - G.png |
| | | | Bewolkt en kans op enkele pittige buien - S.png|
| | | | Zwaar bewolkt met wat lichte regen - M.png|
| 502  | heavy Rain      | >=502 and code <=531  | Zwaar bewolkt en regen - G.png |
| | | | Zwaar bewolkt met regen en winterse neerslag - W.png|
| 600  | light snow      | = 600                 | Afwisselend bewolkt met lichte sneeuwval - Q.png |
| 601  | snow            | >=601 and code <=622  | Zware sneeuwval - T.png |o
| 801  | few clouds      | = 801                 | Mix van opklaringen en hoge bewolking - J.png|
| 802  | scattered clouds| = 802                 | Mix van opklaringen en middelbare of lage bewolking - B.png |
| 803  | broken clouds   | = 803                 |  |
| 804  | overcast clouds | = 804                 | Zwaar bewolkt - C.png |


# First we restrict number of atmospheric conditions in 'weather'
```
weather_type = {
    '211': 'thunderstorm',
    '500': 'light rain',
    '501': 'moderate rain',
    '502': 'heavy rain',
    '600': 'light snow',
    '601': 'snow',
    '701': 'fog',
    '800': 'clear sky',
    '801': 'few clouds',
    '802': 'scattered clouds',
    '803': 'broken clouds',
    '804': 'overcast clouds'
}
```

```
def map_weather_code(code):
    if code >=200 and code < 300:
        # Thunderstorm
        return 211
    elif code >=300 and code <= 500:
        #Drizzle -> light rain
        return 500
    elif code >=502 and code <=531:
        # Heavy Rain
        return 502
    elif code >=601 and code <=622:
        # Snow
        return 601
    elif code >=701 and code <=781:
        # Fog
        return 601
    else:
        return code
```