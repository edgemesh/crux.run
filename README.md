# Introduction
The CrUX.run REST API is designed with speed an simplicity in mind. As such, with exception of the `/check` route, all API methods take the same two arguments: a `domain` and the number of months of data you would like (`months`). That's it.

>NOTE:
> CrUX.run expects the domain to match the website under study. For example, `www.test.com` and `test.com` will NOT return the same results. 

# API Endpoints
You can query all API endpoints at https://api.crux.run 

# /check
`/check` provides a fast endpoint to see if the `domain` is available in the CrUX database. Although the CrUX is _big_ Google does store sites which do not generate enough traffic for the data to be statistically signifigant. If your site is new, check back in about 30 days. CrUX data files are updated by Google on or about ever 2nd Tuesday of the month. CrUX.run updates itself approximately 24 hours after Google releases the BigQuery files for export.

### Parameter
```domain= [string]```

### Example
https://api.crux.run/check/?domain=www.google.com

### Results
A JSON object of the following structure:

| field 	| type 	| description 	|
|------------	|--------------	|-----------------------------------------------------------------------------------	|
| inDatabase 	| boolean 	| Was this site in the CrUX database 	|
| monthCount 	| int 	| How many months of data are available for querying with this site 	|
| months 	| [ datetime ] 	| The array of months that are available  	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|


```json
	{
	  "inDatabase": true,
	  "monthCount": 6,
	  "months": [
	    "2019-01-31T00:00:00.000Z",
	    "2019-02-28T00:00:00.000Z",
	    "2019-03-31T00:00:00.000Z",
	    "2019-04-30T00:00:00.000Z",
	    "2019-05-31T00:00:00.000Z",
	    "2019-06-30T00:00:00.000Z"
	  ],
	  "queryTime": "2000-01-01T00:00:00.011Z"
	}
```

# /summary
`/summary` provides fast access to aggregated metrics (percentiles) for the given `domain`. The `/summary` path is not an actionable route but requires a subpath of one of `overview` (summary without any further pivot but month) , `connection` (summarize by connection speed), `form` (summarize by form factor) or `ssl` (summarize by https vs. http endpoints).

## /summary/overview
`/summary/overview` provides a breakdown of the aggregate metrics across all device types (form factors) and network types for the given `domain` broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/summary/overview/?domain=www.google.com&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| date 	| [datetime] 	| The array of dates that correspond to this record. Date here is the last day of the queried month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| pctSamples 	| [float] 	| The total % of samples in this aggregate (should be 1 for /summary/overview) 	|
| firstPaintMAX 	| [int] 	| The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstPaintWAVG 	| [int] 	| The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstPaintP25 	| [int] 	| The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstPaintP50 	| [int] 	| The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstPaintP75 	| [int] 	| The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstPaintP90 	| [int] 	| The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstPaintP95 	| [int] 	| The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstPaintP99 	| [int] 	| The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| firstContentfulPaintMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstContentfulPaintWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstContentfulPaintP25 	| [int] 	| The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstContentfulPaintP50 	| [int] 	| The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstContentfulPaintP75 	| [int] 	| The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstContentfulPaintP90 	| [int] 	| The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstContentfulPaintP95 	| [int] 	| The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstContentfulPaintP99 	| [int] 	| The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|

| domContentLoadedMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| domContentLoadedWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| domContentLoadedP25 	| [int] 	| The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| domContentLoadedP50 	| [int] 	| The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| domContentLoadedP75 	| [int] 	| The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| domContentLoadedP90 	| [int] 	| The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| domContentLoadedP95 	| [int] 	| The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| domContentLoadedP99 	| [int] 	| The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|


```json
{
  "queryTime": "2000-01-01T00:00:00.566Z",
  "date": [
    "2019-05-31T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z"
  ],
  "origin": [
    "www.google.com",
    "www.google.com"
  ],
  "pctSamples": [
    1,
    1
  ],
  "firstPaintMAX": [
    120100,
    120100
  ],
  "firstPaintWAVG": [
    1591,
    1703
  ],
  "firstPaintP25": [
    300,
    300
  ],
  "firstPaintP50": [
    500,
    500
  ],
  "firstPaintP75": [
    1200,
    1300
  ],
  "firstPaintP90": [
    3200,
    3500
  ],
  "firstPaintP95": [
    6000,
    6700
  ],
  "firstPaintP99": [
    19800,
    20700
  ],
  "firstContentfulPaintMAX": [
    120100,
    120100
  ],
  "firstContentfulPaintWAVG": [
    1530,
    1661
  ],
  "firstContentfulPaintP25": [
    300,
    300
  ],
  "firstContentfulPaintP50": [
    500,
    500
  ],
  "firstContentfulPaintP75": [
    1200,
    1300
  ],
  "firstContentfulPaintP90": [
    3000,
    3400
  ],
  "firstContentfulPaintP95": [
    5700,
    6400
  ],
  "firstContentfulPaintP99": [
    19100,
    20600
  ],
  "domContentLoadedMAX": [
    120100,
    120100
  ],
  "domContentLoadedWAVG": [
    1804,
    2093
  ],
  "domContentLoadedP25": [
    400,
    600
  ],
  "domContentLoadedP50": [
    900,
    1100
  ],
  "domContentLoadedP75": [
    1600,
    1800
  ],
  "domContentLoadedP90": [
    3300,
    4000
  ],
  "domContentLoadedP95": [
    6200,
    7200
  ],
  "domContentLoadedP99": [
    19000,
    20800
  ]
}
```

## /summary/form
`/summary/form` provides a breakdown of the aggregate metrics across by form factors for all network types for the given `domain` broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/summary/form/?domain=www.reddit.com&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| date 	| [datetime] 	| The array of dates that correspond to this record. Date here is the last day of the queried month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| formFactor 	| [[string]] 	| The form factor for this record 	|
| pctSamples 	| [float] 	| The total % of samples in this aggregate (should sum to 1 within the month pivot) 	|
| firstPaintMAX 	| [int] 	| The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstPaintWAVG 	| [int] 	| The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstPaintP25 	| [int] 	| The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstPaintP50 	| [int] 	| The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstPaintP75 	| [int] 	| The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstPaintP90 	| [int] 	| The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstPaintP95 	| [int] 	| The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstPaintP99 	| [int] 	| The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| firstContentfulPaintMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstContentfulPaintWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstContentfulPaintP25 	| [int] 	| The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstContentfulPaintP50 	| [int] 	| The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstContentfulPaintP75 	| [int] 	| The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstContentfulPaintP90 	| [int] 	| The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstContentfulPaintP95 	| [int] 	| The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstContentfulPaintP99 	| [int] 	| The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| domContentLoadedMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| domContentLoadedWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| domContentLoadedP25 	| [int] 	| The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| domContentLoadedP50 	| [int] 	| The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| domContentLoadedP75 	| [int] 	| The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| domContentLoadedP90 	| [int] 	| The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| domContentLoadedP95 	| [int] 	| The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| domContentLoadedP99 	| [int] 	| The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|


```json
{
  "queryTime": "2000-01-01T00:00:00.345Z",
  "date": [
    "2019-05-31T00:00:00.000Z",
    "2019-05-31T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z"
  ],
  "origin": [
    "www.reddit.com",
    "www.reddit.com",
    "www.reddit.com",
    "www.reddit.com"
  ],
  "formFactor": [
    "desktop",
    "phone",
    "desktop",
    "phone"
  ],
  "pctSamples": [
    0.5638999938964844,
    0.435699999332428,
    0.5587000250816345,
    0.44110000133514404
  ],
  "firstPaintMAX": [
    40000,
    25000,
    40000,
    25000
  ],
  "firstPaintWAVG": [
    1569,
    906,
    1547,
    922
  ],
  "firstPaintP25": [
    900,
    300,
    900,
    400
  ],
  "firstPaintP50": [
    1400,
    700,
    1400,
    700
  ],
  "firstPaintP75": [
    1900,
    1100,
    1900,
    1200
  ],
  "firstPaintP90": [
    2700,
    1700,
    2600,
    1700
  ],
  "firstPaintP95": [
    3300,
    2200,
    3300,
    2200
  ],
  "firstPaintP99": [
    5700,
    4800,
    5700,
    4700
  ],
  "firstContentfulPaintMAX": [
    40000,
    25000,
    40000,
    25000
  ],
  "firstContentfulPaintWAVG": [
    1577,
    911,
    1553,
    926
  ],
  "firstContentfulPaintP25": [
    900,
    300,
    900,
    400
  ],
  "firstContentfulPaintP50": [
    1400,
    700,
    1400,
    700
  ],
  "firstContentfulPaintP75": [
    2000,
    1100,
    1900,
    1200
  ],
  "firstContentfulPaintP90": [
    2700,
    1700,
    2600,
    1700
  ],
  "firstContentfulPaintP95": [
    3300,
    2200,
    3300,
    2200
  ],
  "firstContentfulPaintP99": [
    5800,
    4800,
    5700,
    4700
  ],
  "domContentLoadedMAX": [
    50000,
    25000,
    50000,
    25000
  ],
  "domContentLoadedWAVG": [
    2382,
    1034,
    2328,
    1057
  ],
  "domContentLoadedP25": [
    1200,
    300,
    1100,
    400
  ],
  "domContentLoadedP50": [
    1900,
    700,
    1900,
    700
  ],
  "domContentLoadedP75": [
    3000,
    1300,
    2900,
    1300
  ],
  "domContentLoadedP90": [
    4400,
    2100,
    4300,
    2100
  ],
  "domContentLoadedP95": [
    5700,
    2900,
    5500,
    2900
  ],
  "domContentLoadedP99": [
    10100,
    6100,
    9900,
    6500
  ]
}
```

## /summary/connection
`/summary/connection` provides a breakdown of the aggregate metrics across all form factors but broken down by network types for the given `domain` broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/summary/connection/?domain=news.ycombinator.com&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| date 	| [datetime] 	| The array of dates that correspond to this record. Date here is the last day of the queried month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| effectiveConnectionType 	| [[string]] 	| The effective connection type for this record. Note 4G just means 'fast' and also includes cable/broadband	|
| pctSamples 	| [float] 	| The total % of samples in this aggregate (should sum to 1 within the month pivot) 	|
| firstPaintMAX 	| [int] 	| The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstPaintWAVG 	| [int] 	| The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstPaintP25 	| [int] 	| The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstPaintP50 	| [int] 	| The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstPaintP75 	| [int] 	| The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstPaintP90 	| [int] 	| The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstPaintP95 	| [int] 	| The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstPaintP99 	| [int] 	| The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| firstContentfulPaintMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstContentfulPaintWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstContentfulPaintP25 	| [int] 	| The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstContentfulPaintP50 	| [int] 	| The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstContentfulPaintP75 	| [int] 	| The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstContentfulPaintP90 	| [int] 	| The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstContentfulPaintP95 	| [int] 	| The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstContentfulPaintP99 	| [int] 	| The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| domContentLoadedMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| domContentLoadedWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| domContentLoadedP25 	| [int] 	| The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| domContentLoadedP50 	| [int] 	| The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| domContentLoadedP75 	| [int] 	| The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| domContentLoadedP90 	| [int] 	| The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| domContentLoadedP95 	| [int] 	| The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| domContentLoadedP99 	| [int] 	| The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|

```json
{
  "queryTime": "2000-01-01T00:00:00.529Z",
  "date": [
    "2019-05-31T00:00:00.000Z",
    "2019-05-31T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z"
  ],
  "origin": [
    "news.ycombinator.com",
    "news.ycombinator.com",
    "news.ycombinator.com",
    "news.ycombinator.com"
  ],
  "effectiveConnectionType": [
    "3G",
    "4G",
    "3G",
    "4G"
  ],
  "pctSamples": [
    0.04230000078678131,
    0.957099974155426,
    0.03790000081062317,
    0.9617000222206116
  ],
  "firstPaintMAX": [
    20000,
    20000,
    20000,
    20000
  ],
  "firstPaintWAVG": [
    1178,
    508,
    1055,
    495
  ],
  "firstPaintP25": [
    200,
    200,
    200,
    100
  ],
  "firstPaintP50": [
    600,
    300,
    500,
    300
  ],
  "firstPaintP75": [
    1500,
    700,
    1300,
    700
  ],
  "firstPaintP90": [
    2600,
    1100,
    2400,
    1100
  ],
  "firstPaintP95": [
    3900,
    1500,
    3600,
    1400
  ],
  "firstPaintP99": [
    8300,
    2700,
    8100,
    2600
  ],
  "firstContentfulPaintMAX": [
    20000,
    20000,
    20000,
    20000
  ],
  "firstContentfulPaintWAVG": [
    1184,
    507,
    1037,
    495
  ],
  "firstContentfulPaintP25": [
    300,
    200,
    200,
    100
  ],
  "firstContentfulPaintP50": [
    700,
    300,
    500,
    300
  ],
  "firstContentfulPaintP75": [
    1500,
    700,
    1200,
    700
  ],
  "firstContentfulPaintP90": [
    2600,
    1100,
    2300,
    1100
  ],
  "firstContentfulPaintP95": [
    3800,
    1500,
    3600,
    1400
  ],
  "firstContentfulPaintP99": [
    8300,
    2600,
    8000,
    2600
  ],
  "domContentLoadedMAX": [
    20000,
    20000,
    20000,
    20000
  ],
  "domContentLoadedWAVG": [
    1169,
    497,
    1085,
    501
  ],
  "domContentLoadedP25": [
    200,
    100,
    100,
    100
  ],
  "domContentLoadedP50": [
    600,
    300,
    500,
    300
  ],
  "domContentLoadedP75": [
    1500,
    700,
    1300,
    700
  ],
  "domContentLoadedP90": [
    2700,
    1100,
    2600,
    1100
  ],
  "domContentLoadedP95": [
    3900,
    1500,
    3800,
    1500
  ],
  "domContentLoadedP99": [
    8300,
    3100,
    8600,
    3100
  ]
}
```

## /summary/ssl
`/summary/ssl` provides a breakdown of the aggregate metrics across all form factors and all network types, but broken down by wether SSL was enabled for the given `domain` broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/summary/ssl/?domain=www.cia.gov&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| date 	| [datetime] 	| The array of dates that correspond to this record. Date here is the last day of the queried month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| isSSL 	| [boolean] 	| Was this record for https (true) or http (false) samples	|
| pctSamples 	| [float] 	| The total % of samples in this aggregate (should sum to 1 within the month pivot) 	|
| firstPaintMAX 	| [int] 	| The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstPaintWAVG 	| [int] 	| The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstPaintP25 	| [int] 	| The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstPaintP50 	| [int] 	| The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstPaintP75 	| [int] 	| The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstPaintP90 	| [int] 	| The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstPaintP95 	| [int] 	| The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstPaintP99 	| [int] 	| The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| firstContentfulPaintMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| firstContentfulPaintWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| firstContentfulPaintP25 	| [int] 	| The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| firstContentfulPaintP50 	| [int] 	| The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| firstContentfulPaintP75 	| [int] 	| The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| firstContentfulPaintP90 	| [int] 	| The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| firstContentfulPaintP95 	| [int] 	| The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| firstContentfulPaintP99 	| [int] 	| The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|
| domContentLoadedMAX 	| [int] 	| The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the field was between 1000-1100ms 	|
| domContentLoadedWAVG 	| [int] 	| The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg startingBin. 	|
| domContentLoadedP25 	| [int] 	| The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25) 	|
| domContentLoadedP50 	| [int] 	| The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50) 	|
| domContentLoadedP75 	| [int] 	| The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75) 	|
| domContentLoadedP90 	| [int] 	| The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90) 	|
| domContentLoadedP95 	| [int] 	| The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95) 	|
| domContentLoadedP99 	| [int] 	| The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99) 	|

```json
{
  "queryTime": "2000-01-01T00:00:00.280Z",
  "date": [
    "2019-05-31T00:00:00.000Z",
    "2019-06-30T00:00:00.000Z"
  ],
  "origin": [
    "www.cia.gov",
    "www.cia.gov"
  ],
  "isSSL": [
    true,
    true
  ],
  "pctSamples": [
    0.9995999932289124,
    1
  ],
  "firstPaintMAX": [
    40000,
    50000
  ],
  "firstPaintWAVG": [
    1154,
    1401
  ],
  "firstPaintP25": [
    400,
    400
  ],
  "firstPaintP50": [
    700,
    800
  ],
  "firstPaintP75": [
    1400,
    1700
  ],
  "firstPaintP90": [
    2400,
    3100
  ],
  "firstPaintP95": [
    3300,
    4300
  ],
  "firstPaintP99": [
    7200,
    9500
  ],
  "firstContentfulPaintMAX": [
    40000,
    50000
  ],
  "firstContentfulPaintWAVG": [
    1157,
    1399
  ],
  "firstContentfulPaintP25": [
    400,
    400
  ],
  "firstContentfulPaintP50": [
    700,
    800
  ],
  "firstContentfulPaintP75": [
    1400,
    1700
  ],
  "firstContentfulPaintP90": [
    2400,
    3100
  ],
  "firstContentfulPaintP95": [
    3300,
    4300
  ],
  "firstContentfulPaintP99": [
    7200,
    9300
  ],
  "domContentLoadedMAX": [
    50000,
    50000
  ],
  "domContentLoadedWAVG": [
    1553,
    1663
  ],
  "domContentLoadedP25": [
    600,
    500
  ],
  "domContentLoadedP50": [
    1100,
    1000
  ],
  "domContentLoadedP75": [
    1900,
    2100
  ],
  "domContentLoadedP90": [
    3000,
    3500
  ],
  "domContentLoadedP95": [
    4100,
    4800
  ],
  "domContentLoadedP99": [
    8300,
    10400
  ]
}
```

# /score
`/score` provides a breakdown of the aggregate scores (fast/average/slow) for the given inputs. These boundaries are defined in the Google PageSpeed definitions. They are shown below (all times are in milliseconds):

| Metric 	| Fast 	| Average 	| Slow 	|
|----------------------	|------	|---------	|-------	|
| FirstPaint 	| 1000 	| 2500 	| >2500 	|
| FirstContentfulPaint 	| 1000 	| 2500 	| >3500 	|
| DOM Content Loaded 	| 1500 	| 3500 	| >3500 	|

## /score/fp
`/summary/fp` provides a total percentage of visitors across all form factors and all network types, but broken down score of the First Paint for the given `domain`. This is further broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/score/fp?domain=www.tesla.com&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| month 	| [datetime] 	| The array of dates that correspond to this record. Month here is the first date of the month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| fast 	| [float]	| The % of visitors (sum of density) that had a fast First Paint score of this record 	|
| average 	| [float] 	| The % of visitors (sum of density) that had a fast First Paint score of this record	|
| slow 	| [float] 	| The % of visitors (sum of density) that had a fast First Paint score of this record	|

```json
{
  "queryTime": "2000-01-01T00:00:00.146Z",
  "month": [
    "2019-05-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "fast": [
    0.44699999690055847,
    0.46160000562667847
  ],
  "average": [
    0.27274999022483826,
    0.2632499933242798
  ],
  "slow": [
    0.28025001287460327,
    0.27515000104904175
  ]
}
```

## /score/fcp
`/summary/fcp` provides a total percentage of visitors across all form factors and all network types, but broken down score of the First Contentful Paint for the given `domain`. This is further broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
	domain= [string]
	months= int
```

### Example
https://api.crux.run/score/fcp?domain=www.tesla.com&months=2

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|--------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| month 	| [datetime] 	| The array of dates that correspond to this record. Month here is the first date of the month 	|
| origin 	| [[string]] 	| The origin of this record 	|
| fast 	| [float]	| The % of visitors (sum of density) that had a fast First Contentful Paint score of this record 	|
| average 	| [float] 	| The % of visitors (sum of density) that had a fast First Contentful Paint score of this record	|
| slow 	| [float] 	| The % of visitors (sum of density) that had a fast First Contentful Paint score of this record	|

```json
{
  "queryTime": "2000-01-01T00:00:00.146Z",
  "month": [
    "2019-05-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "fast": [
    0.44699999690055847,
    0.46160000562667847
  ],
  "average": [
    0.27274999022483826,
    0.2632499933242798
  ],
  "slow": [
    0.28025001287460327,
    0.27515000104904175
  ]
}
```

## /score/dcl
`/summary/dcl` provides a total percentage of visitors across all form factors and all network types, but broken down score of the DOM Content Loaded for the given `domain`. This is further broken down by months where the number of months is specificed via `months`. The `months` parameter specifes how many months _back_ of data you would like. For example passing `months=2` will return data for the most recent 2 months and `months=6` will return data for the most recent 6 months.

### Parameter
```
  domain= [string]
  months= int
```

### Example
https://api.crux.run/score/dcl?domain=www.tesla.com&months=2

### Results
A JSON object of the following structure

| field   | type  | description   |
|-------------------------- |------------ |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| queryTime   | datetime  | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000)   |
| month   | [datetime]  | The array of dates that correspond to this record. Month here is the first date of the month  |
| origin  | [[string]]  | The origin of this record   |
| fast  | [float] | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record  |
| average   | [float]   | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record  |
| slow  | [float]   | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record  |

```json
{
  "queryTime": "2000-01-01T00:00:00.155Z",
  "month": [
    "2019-05-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "fast": [
    0.26589998602867126,
    0.27639999985694885
  ],
  "average": [
    0.36240002512931824,
    0.35489997267723083
  ],
  "slow": [
    0.3716999888420105,
    0.3687000274658203
  ]
}
```

# /histo
`/histo` the histogram of a metric across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cummulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

## /histo/fp
`/histo/fp` the histogram of First Paint observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cummulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Parameter
```
  domain= [string]
  months= int
```

### Example
https://api.crux.run/histo/fp?domain=www.pearson.com&months=1

### Results
A JSON object of the following structure

| field 	| type 	| description 	|
|-------------	|------------	|-------------------------------------------------------------------------------------------------------------------------------	|
| monthCount 	| int 	| The number of months in this result set. 	|
| minBinStart 	| int 	| The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value 	|
| maxBinStart 	| int 	| The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value 	|
| minDensity 	| float 	| The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value 	|
| maxDensity 	| float 	| The maxium observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value 	|
| queryTime 	| datetime 	| The amount of time this query took server side (ms since 2000-01-01T00:00:00.000) 	|
| month 	| [[string]] 	| The array of months for these records 	|
| binStart 	| [int] 	| The array of starting bins for these records 	|
| density 	| [float] 	| The array of density values for these records. The sum of density with month ~= .9901 	|

```json
{
  "monthCount": 1,
  "minBinStart": 0,
  "maxBinStart": 15700,
  "minDensity": 0.00010200000542681664,
  "maxDensity": 0.05230000242590904,
  "queryTime": "2000-01-01T00:00:00.093Z",
  "month": [
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "binStart": [
    0,
    100,
    200,
    300,
    400,
    500,
    600,
    700,
    800,
    900,
    1000,
    1100,
    1200,
    1300,
    1400,
    1500,
    1600,
    1700,
    1800,
    1900,
    2000,
    2100,
    2200,
    2300,
    2400,
    2500,
    2600,
    2700,
    2800,
    2900,
    3000,
    3100,
    3200,
    3300,
    3400,
    3500,
    3600,
    3700,
    3800,
    3900,
    4000,
    4100,
    4200,
    4300,
    4400,
    4500,
    4600,
    4700,
    4800,
    4900,
    5000,
    5100,
    5200,
    5300,
    5400,
    5500,
    5600,
    5700,
    5800,
    5900,
    6000,
    6100,
    6200,
    6300,
    6400,
    6500,
    6600,
    6700,
    6800,
    6900,
    7000,
    7100,
    7200,
    7300,
    7400,
    7500,
    7600,
    7700,
    7800,
    7900,
    8000,
    8100,
    8200,
    8300,
    8400,
    8500,
    8600,
    8700,
    8800,
    8900,
    9000,
    9100,
    9200,
    9300,
    9400,
    9500,
    9600,
    9700,
    9800,
    9900,
    10000,
    10100,
    10200,
    10300,
    10400,
    10500,
    10600,
    10700,
    10800,
    10900,
    11000,
    11100,
    11200,
    11300,
    11400,
    11500,
    11600,
    11700,
    11800,
    11900,
    12000,
    12100,
    12200,
    12300,
    12400,
    12500,
    12600,
    12700,
    12800,
    12900,
    13000,
    13100,
    13200,
    13300,
    13400,
    13500,
    13600,
    13700,
    13800,
    13900,
    14000,
    14100,
    14200,
    14300,
    14400,
    14500,
    14600,
    14700,
    14800,
    14900,
    15000,
    15100,
    15200,
    15300,
    15400,
    15500,
    15600,
    15700
  ],
  "density": [
    0.007149999961256981,
    0.007149999961256981,
    0.04320000112056732,
    0.04320000112056732,
    0.05230000242590904,
    0.05230000242590904,
    0.04334999993443489,
    0.04334999993443489,
    0.03804999962449074,
    0.03804999962449074,
    0.03610000014305115,
    0.03610000014305115,
    0.03134999796748161,
    0.03134999796748161,
    0.0272000003606081,
    0.0272000003606081,
    0.024150000885128975,
    0.024150000885128975,
    0.021250000223517418,
    0.021250000223517418,
    0.018649999052286148,
    0.018649999052286148,
    0.01640000008046627,
    0.01640000008046627,
    0.014399999752640724,
    0.014399999752640724,
    0.013050000183284283,
    0.013050000183284283,
    0.011500000022351742,
    0.011500000022351742,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664
  ]
}
```

## /histo/fcp
`/histo/fcp` the histogram of First Contentful Paint observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cummulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Parameter
```
  domain= [string]
  months= int
```

### Example
https://api.crux.run/histo/fcp?domain=www.pearson.com&months=1

### Results
A JSON object of the following structure

| field   | type  | description   |
|-------------  |------------ |-------------------------------------------------------------------------------------------------------------------------------  |
| monthCount  | int   | The number of months in this result set.  |
| minBinStart   | int   | The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value  |
| maxBinStart   | int   | The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value  |
| minDensity  | float   | The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value   |
| maxDensity  | float   | The maxium observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value  |
| queryTime   | datetime  | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000)   |
| month   | [[string]]  | The array of months for these records   |
| binStart  | [int]   | The array of starting bins for these records  |
| density   | [float]   | The array of density values for these records. The sum of density with month ~= .9901   |

```json
{
  "monthCount": 1,
  "minBinStart": 0,
  "maxBinStart": 15700,
  "minDensity": 0.00010200000542681664,
  "maxDensity": 0.05230000242590904,
  "queryTime": "2000-01-01T00:00:00.093Z",
  "month": [
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "binStart": [
    0,
    100,
    200,
    300,
    400,
    500,
    600,
    700,
    800,
    900,
    1000,
    1100,
    1200,
    1300,
    1400,
    1500,
    1600,
    1700,
    1800,
    1900,
    2000,
    2100,
    2200,
    2300,
    2400,
    2500,
    2600,
    2700,
    2800,
    2900,
    3000,
    3100,
    3200,
    3300,
    3400,
    3500,
    3600,
    3700,
    3800,
    3900,
    4000,
    4100,
    4200,
    4300,
    4400,
    4500,
    4600,
    4700,
    4800,
    4900,
    5000,
    5100,
    5200,
    5300,
    5400,
    5500,
    5600,
    5700,
    5800,
    5900,
    6000,
    6100,
    6200,
    6300,
    6400,
    6500,
    6600,
    6700,
    6800,
    6900,
    7000,
    7100,
    7200,
    7300,
    7400,
    7500,
    7600,
    7700,
    7800,
    7900,
    8000,
    8100,
    8200,
    8300,
    8400,
    8500,
    8600,
    8700,
    8800,
    8900,
    9000,
    9100,
    9200,
    9300,
    9400,
    9500,
    9600,
    9700,
    9800,
    9900,
    10000,
    10100,
    10200,
    10300,
    10400,
    10500,
    10600,
    10700,
    10800,
    10900,
    11000,
    11100,
    11200,
    11300,
    11400,
    11500,
    11600,
    11700,
    11800,
    11900,
    12000,
    12100,
    12200,
    12300,
    12400,
    12500,
    12600,
    12700,
    12800,
    12900,
    13000,
    13100,
    13200,
    13300,
    13400,
    13500,
    13600,
    13700,
    13800,
    13900,
    14000,
    14100,
    14200,
    14300,
    14400,
    14500,
    14600,
    14700,
    14800,
    14900,
    15000,
    15100,
    15200,
    15300,
    15400,
    15500,
    15600,
    15700
  ],
  "density": [
    0.007149999961256981,
    0.007149999961256981,
    0.04320000112056732,
    0.04320000112056732,
    0.05230000242590904,
    0.05230000242590904,
    0.04334999993443489,
    0.04334999993443489,
    0.03804999962449074,
    0.03804999962449074,
    0.03610000014305115,
    0.03610000014305115,
    0.03134999796748161,
    0.03134999796748161,
    0.0272000003606081,
    0.0272000003606081,
    0.024150000885128975,
    0.024150000885128975,
    0.021250000223517418,
    0.021250000223517418,
    0.018649999052286148,
    0.018649999052286148,
    0.01640000008046627,
    0.01640000008046627,
    0.014399999752640724,
    0.014399999752640724,
    0.013050000183284283,
    0.013050000183284283,
    0.011500000022351742,
    0.011500000022351742,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664
  ]
}
```

## /histo/dcl
`/histo/dcl` the histogram of DOM Content Loaded observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cummulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Parameter
```
  domain= [string]
  months= int
```

### Example
https://api.crux.run/histo/dcl?domain=www.pearson.com&months=1

### Results
A JSON object of the following structure

| field   | type  | description   |
|-------------  |------------ |-------------------------------------------------------------------------------------------------------------------------------  |
| monthCount  | int   | The number of months in this result set.  |
| minBinStart   | int   | The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value  |
| maxBinStart   | int   | The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value  |
| minDensity  | float   | The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value   |
| maxDensity  | float   | The maxium observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value  |
| queryTime   | datetime  | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000)   |
| month   | [[string]]  | The array of months for these records   |
| binStart  | [int]   | The array of starting bins for these records  |
| density   | [float]   | The array of density values for these records. The sum of density with month ~= .9901   |

```json
{
  "monthCount": 1,
  "minBinStart": 0,
  "maxBinStart": 15700,
  "minDensity": 0.00010200000542681664,
  "maxDensity": 0.05230000242590904,
  "queryTime": "2000-01-01T00:00:00.093Z",
  "month": [
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z",
    "2019-06-01T00:00:00.000Z"
  ],
  "binStart": [
    0,
    100,
    200,
    300,
    400,
    500,
    600,
    700,
    800,
    900,
    1000,
    1100,
    1200,
    1300,
    1400,
    1500,
    1600,
    1700,
    1800,
    1900,
    2000,
    2100,
    2200,
    2300,
    2400,
    2500,
    2600,
    2700,
    2800,
    2900,
    3000,
    3100,
    3200,
    3300,
    3400,
    3500,
    3600,
    3700,
    3800,
    3900,
    4000,
    4100,
    4200,
    4300,
    4400,
    4500,
    4600,
    4700,
    4800,
    4900,
    5000,
    5100,
    5200,
    5300,
    5400,
    5500,
    5600,
    5700,
    5800,
    5900,
    6000,
    6100,
    6200,
    6300,
    6400,
    6500,
    6600,
    6700,
    6800,
    6900,
    7000,
    7100,
    7200,
    7300,
    7400,
    7500,
    7600,
    7700,
    7800,
    7900,
    8000,
    8100,
    8200,
    8300,
    8400,
    8500,
    8600,
    8700,
    8800,
    8900,
    9000,
    9100,
    9200,
    9300,
    9400,
    9500,
    9600,
    9700,
    9800,
    9900,
    10000,
    10100,
    10200,
    10300,
    10400,
    10500,
    10600,
    10700,
    10800,
    10900,
    11000,
    11100,
    11200,
    11300,
    11400,
    11500,
    11600,
    11700,
    11800,
    11900,
    12000,
    12100,
    12200,
    12300,
    12400,
    12500,
    12600,
    12700,
    12800,
    12900,
    13000,
    13100,
    13200,
    13300,
    13400,
    13500,
    13600,
    13700,
    13800,
    13900,
    14000,
    14100,
    14200,
    14300,
    14400,
    14500,
    14600,
    14700,
    14800,
    14900,
    15000,
    15100,
    15200,
    15300,
    15400,
    15500,
    15600,
    15700
  ],
  "density": [
    0.007149999961256981,
    0.007149999961256981,
    0.04320000112056732,
    0.04320000112056732,
    0.05230000242590904,
    0.05230000242590904,
    0.04334999993443489,
    0.04334999993443489,
    0.03804999962449074,
    0.03804999962449074,
    0.03610000014305115,
    0.03610000014305115,
    0.03134999796748161,
    0.03134999796748161,
    0.0272000003606081,
    0.0272000003606081,
    0.024150000885128975,
    0.024150000885128975,
    0.021250000223517418,
    0.021250000223517418,
    0.018649999052286148,
    0.018649999052286148,
    0.01640000008046627,
    0.01640000008046627,
    0.014399999752640724,
    0.014399999752640724,
    0.013050000183284283,
    0.013050000183284283,
    0.011500000022351742,
    0.011500000022351742,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.009240000508725643,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0066200001165270805,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.0049600000493228436,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.00343999988399446,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0026599999982863665,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0019600000232458115,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0017000000225380063,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0012400000123307109,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0010400000028312206,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007600000244565308,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0007000000332482159,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.0006399999838322401,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.000539999979082495,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.00043999997433274984,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.0002620000159367919,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664,
    0.00010200000542681664
  ]
}
```