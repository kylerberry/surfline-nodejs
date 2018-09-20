# surfline-nodejs
A NodeJs Client for surfline data

* __disclaimer__: This is not an official library and it's possible that surfline could restrict access to or change their api at any time.

## Methods:
- [Search](#search)
- [ForecastOverview](#forecast-overview)
- [ForecastWaves](#forecast-waves)
- [ForecastWind](#forecast-wind)
- [ForecastTides](#forecast-tides)
- [ForecastWeather](#forecast-weather)
### search
- _required_ String `query`
- _optional_ Integer `querySize`

Useful for finding general information and spot suggestions for a location name

Example Reponse:
```
[ { took: [Integer],
    timed_out: false,
    _shards: { total: 5, successful: 5, failed: 0 },
    hits: { total: 5, max_score: 9.764475, hits: [Array] },
    suggest: { 'spot-suggest': [Array] },
    status: 200 },
  { took: 6,
    timed_out: false,
    _shards: { total: 5, successful: 5, failed: 0 },
    hits: { total: 0, max_score: null, hits: [] },
    suggest: { 'subregion-suggest': [Array] },
    status: 200 },
  { took: 5,
    timed_out: false,
    _shards: { total: 5, successful: 5, failed: 0 },
    hits: { total: 0, max_score: null, hits: [] },
    suggest: { 'geoname-suggest': [Array] },
    status: 200 },
  { took: 5,
    timed_out: false,
    _shards: { total: 5, successful: 5, failed: 0 },
    hits: { total: 0, max_score: null, hits: [] },
    suggest: { 'travel-suggest': [Array] },
    status: 200 } ]
```

### forecastOverview
- _required_ String `spotId`
- _required_ String `subregionId` (either spotId or subregionId are required, not both)
- _optional_ Integer `days` number of days to forecast

Example Response:
```
{ _id: '5b71b95fc27dc6001ab8becf',
  name: 'Seal Beach Pier, Northside',
  conditions: { human: false, value: null },
  waveHeight:
   { human: false,
     min: 1,
     max: 2,
     occasional: null,
     humanRelation: '1-2 ft – ankle to knee high',
     plus: false },
  lat: 33.73717424759846,
  lon: -118.1114494033468,
  cameras:
   [ { _id: '5b6df48f9aa2b6001b9f19de',
       title: 'SOCAL - Seal Beach Pier, Northside',
       streamUrl:
        'https://cams.cdn-surfline.com/wsc-west/wc-sealbeachnscam.stream/playlist.m3u8',
       stillUrl:
        'https://camstills.cdn-surfline.com/sealbeachnscam/latest_small.jpg',
       rewindBaseUrl:
        'https://camrewinds.cdn-surfline.com/sealbeachnscam/sealbeachnscam',
       isPremium: false,
       status: [Object],
       control:
        'https://camstills.cdn-surfline.com/sealbeachnscam/latest_small.jpg',
       nighttime: false,
       rewindClip:
        'https://camrewinds.cdn-surfline.com/sealbeachnscam/sealbeachnscam.1300.2018-09-17.mp4' } ],
  rank: 0 }
```

Useful for getting humanized info for a region or spot.

### forecastWaves
- _required_ String `spotId`
- _optional_ Integer `days`
- _optional_ Integer `intervalHours`

Useful for getting wave forecast for a spotId

Example Reponse:
```
{ associated:
   { units:
      { temperature: 'F',
        tideHeight: 'FT',
        waveHeight: 'FT',
        windSpeed: 'KTS' },
     utcOffset: -7,
     location: { lon: -117.8819918632, lat: 33.5930302087 },
     forecastLocation: { lon: -117.882, lat: 33.593 },
     offshoreLocation: { lon: -117.9, lat: 33.55 } },
  data:
   { wave:
      [ [Object],
        [Object],
        [Object],
        ...
      ]
    }
}
```

### forecastTides
- _required_ String `spotId`
- _optional_ Integer `days`
- _optional_ Integer `intervalHours`

Useful for getting tides forecast for a spotId

Example Response:
```
{ associated:
   { utcOffset: -7,
     units:
      { temperature: 'F',
        tideHeight: 'FT',
        waveHeight: 'FT',
        windSpeed: 'KTS' },
     tideLocation:
      { name: 'Newport Bay Entrance, California',
        min: -1.97,
        max: 7.18,
        lon: -117.8833,
        lat: 33.6033,
        mean: 2.76 } },
  data:
   { tides:
      [ [Object],
        [Object],
        [Object],
        ...
        ]
    }
}
```

### forecastWeather
- _required_ String `spotId`
- _optional_ Integer `days`
- _optional_ Integer `intervalHours`

Useful for getting weather forecast for a spotId

Example Response:
```
{ associated:
   { units:
      { temperature: 'F',
        tideHeight: 'FT',
        waveHeight: 'FT',
        windSpeed: 'KTS' },
     utcOffset: -7,
     weatherIconPath: 'https://wa.cdn-surfline.com/quiver/0.6.2/weathericons' },
  data:
   { sunlightTimes: [ [Object] ],
     weather:
      [ [Object],
        [Object],
        [Object],
        ...
       ]
    }
}
```

### forecastWind
- _required_ String `spotId`
- _optional_ Integer `days`
- _optional_ Integer `intervalHours`

Useful for getting wind forecast for a spotId

Example Response:
```
{ associated:
   { units:
      { temperature: 'F',
        tideHeight: 'FT',
        waveHeight: 'FT',
        windSpeed: 'KTS' },
     utcOffset: -7,
     weatherIconPath: 'https://wa.cdn-surfline.com/quiver/0.6.2/weathericons' },
  data:
   {
     wind:
      [ [Object],
        [Object],
        [Object],
        ...
       ]
    }
}
```

### Examples
- Get a spotId for a location query: `wedge`
```
const surfline = require('surfline')
surfline.search({ q: 'wedge' }).then(({ data }) => {
     console.log(data[0].hits.hits[0]._id)
     // logs '5842041f4e65fad6a770882b'
})
```
- Get a wave forecast for the next 3 days at 6 hour intervals

```
const surfline = require('surfline')
surfline.forecastTides({
    spotId: '5842041f4e65fad6a770882b',
    days: 3,
    intervalHours: 6
}).then(({data}) => {
    console.log(data.data.waves);
    // logs array of wave objects
})
```

- Get a humanized 1-day overview for a region
```
const surfline = require('surfline')
surfline.forecastOverview({
    spotId: '5842041f4e65fad6a770882b',
    days: 1
}).then(({data}) => {
    console.log(data.data.forecastSummary.highlights[0]);
    // logs String: 'S swell holds early this AM, then eases midweek'
})
```