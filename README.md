# Getting Started

The crux API is built for simplicity.  Each route accepts the same set of query parameters.

| Param  | Type    | Required | Description                                                                                                                                                  |
|--------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| domain | string  | true     | The domain to query.  Domains are subdomain sensitive. `www.google.com` and `google.com` are separate targets.                                               |
| months | integer | true     | How many months to query. Not actually required by `/check` but wouldn't hurt if its there.                                                                  |
| flip   | boolean | false    | Flips the dataset into an array of objects. It will fill any singleton values. Results in a larger dataset (in size), but can be easier to consume visually. |

Example:
`https://api.crux.run/v1/histo/fp/?domain=www.google.com&months=2&flip=true`

The crux run API can return data in `json`, `csv` and `xml` format.  By default it will return data with `application/json`.  To specify a different format, set the `Accept` request header accordingly:

| Format | Accept Header      |
|--------|--------------------|
| json   | application/json   |
| xml    | application/xml    |
| csv    | text/csv           |

# API Endpoints

- `v1` https://api.crux.run/v1

All crux run API's can be queried from `https://api.crux.run`. The API will be released in versions appended to this root url.

## `GET` /check
`/check` provides a fast endpoint to see if the given `domain` is available in the CrUX database. Although the CrUX is _big_, Google does store sites which do not generate enough traffic for the data to be statistically significant. If your site is new, check back in about 30 days. CrUX data files are updated by Google on or about ever 2nd Tuesday of every month. CrUX.run updates itself approximately 24 hours after Google releases the BigQuery files for export.

### Example
https://api.crux.run/v1/check/?domain=www.google.com

### Response
A structure in your selected format with the following shape:

| Property   | Type     | Description                                                                        |
|------------|----------|------------------------------------------------------------------------------------|
| inDatabase | boolean  | Was this site in the CrUX database?                                                |
| monthCount | integer  | How many months of data are available for querying with this site.                 |
| months     | datetime | The array of months that are available.                                            |
| queryTime  | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000). |



<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON

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

#### XML

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<inDatabase>true</inDatabase>
		<monthCount>8</monthCount>
		<months>
			<item>2019-01-31T00:00:00.000Z</item>
			<item>2019-02-28T00:00:00.000Z</item>
			<item>2019-03-31T00:00:00.000Z</item>
			<item>2019-04-30T00:00:00.000Z</item>
			<item>2019-05-31T00:00:00.000Z</item>
			<item>2019-06-30T00:00:00.000Z</item>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
		</months>
		<queryTime>2000-01-01T00:00:00.010Z</queryTime>
	</data>
```

#### CSV

```csv
	inDatabase,monthCount,months,queryTime
	1,8,1548892800000,946684800011
	1,8,1551312000000,946684800011
	1,8,1553990400000,946684800011
	1,8,1556582400000,946684800011
	1,8,1559260800000,946684800011
	1,8,1561852800000,946684800011
	1,8,1564531200000,946684800011
	1,8,1567209600000,946684800011
```

#### JSON Flipped
```json
	[
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-01-31T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-02-28T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-03-31T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-04-30T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-05-31T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-06-30T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-07-31T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		},
		{
			"inDatabase": true,
			"monthCount": 8,
			"months": "2019-08-31T00:00:00.000Z",
			"queryTime": "2000-01-01T00:00:00.003Z"
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-01-31T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-02-28T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-03-31T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-04-30T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-05-31T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-06-30T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-07-31T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
		<item>
			<inDatabase>true</inDatabase>
			<monthCount>8</monthCount>
			<months>2019-08-31T00:00:00.000Z</months>
			<queryTime>2000-01-01T00:00:00.011Z</queryTime>
		</item>
	</data>
```

<!-- tabs:end -->
</p>
</details>



## `GET` /summary/overview
`/summary/overview` provides a breakdown of the aggregate metrics across all device types and network types for the given `domain`.

### Example
https://api.crux.run/v1/summary/overview/?domain=www.google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property                 | Type     | Description                                                                                                                                                                                              |
|--------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queryTime                | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                                                                                                       |
| date                     | datetime | The array of dates that correspond to this record. Date here is the last day of the queried month.                                                                                                       |
| origin                   | string   | The origin of this record.                                                                                                                                                                               |
| pctSamples               | float    | The total % of samples in this aggregate (should be 1 for /summary/overview).                                                                                                                            |
| firstPaintMAX            | integer  | The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms.            |
| firstPaintWAVG           | integer  | The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                              |
| firstPaintP25            | integer  | The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                              |
| firstPaintP50            | integer  | The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                              |
| firstPaintP75            | integer  | The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                              |
| firstPaintP90            | integer  | The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                              |
| firstPaintP95            | integer  | The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                              |
| firstPaintP99            | integer  | The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                              |
| firstContentfulPaintMAX  | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| firstContentfulPaintWAVG | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| firstContentfulPaintP25  | integer  | The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                   |
| firstContentfulPaintP50  | integer  | The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                   |
| firstContentfulPaintP75  | integer  | The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                   |
| firstContentfulPaintP90  | integer  | The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                   |
| firstContentfulPaintP95  | integer  | The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                   |
| firstContentfulPaintP99  | integer  | The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                   |
| domContentLoadedMAX      | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| domContentLoadedWAVG     | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| domContentLoadedP25      | integer  | The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                               |
| domContentLoadedP50      | integer  | The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                               |
| domContentLoadedP75      | integer  | The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                               |
| domContentLoadedP90      | integer  | The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                               |
| domContentLoadedP95      | integer  | The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                               |
| domContentLoadedP99      | integer  | The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                         |



<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON

```json
	{
		"queryTime": "2000-01-01T00:00:00.024Z",
		"date": [
			"2019-07-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z"
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
			1748,
			1649
		],
		"firstPaintP25": [
			300,
			300
		],
		"firstPaintP50": [
			600,
			600
		],
		"firstPaintP75": [
			1400,
			1400
		],
		"firstPaintP90": [
			3600,
			3400
		],
		"firstPaintP95": [
			6700,
			6000
		],
		"firstPaintP99": [
			21700,
			19500
		],
		"firstContentfulPaintMAX": [
			120100,
			120100
		],
		"firstContentfulPaintWAVG": [
		1703,
		1604
		],
		"firstContentfulPaintP25": [
		300,
		300
		],
		"firstContentfulPaintP50": [
		500,
		600
		],
		"firstContentfulPaintP75": [
		1300,
		1300
		],
		"firstContentfulPaintP90": [
		3500,
		3300
		],
		"firstContentfulPaintP95": [
		6500,
		5800
		],
		"firstContentfulPaintP99": [
		21500,
		18900
		],
		"domContentLoadedMAX": [
		120100,
		120100
		],
		"domContentLoadedWAVG": [
		2206,
		2103
		],
		"domContentLoadedP25": [
		600,
		600
		],
		"domContentLoadedP50": [
		1100,
		1100
		],
		"domContentLoadedP75": [
		1900,
		1800
		],
		"domContentLoadedP90": [
		4200,
		3900
		],
		"domContentLoadedP95": [
		7600,
		7000
		],
		"domContentLoadedP99": [
		22500,
		21600
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.026Z</queryTime>
		<date>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
		</date>
		<origin>
			<item>www.google.com</item>
			<item>www.google.com</item>
		</origin>
		<pctSamples>
			<item>1</item>
			<item>1</item>
		</pctSamples>
		<firstPaintMAX>
			<item>120100</item>
			<item>120100</item>
		</firstPaintMAX>
		<firstPaintWAVG>
			<item>1748</item>
			<item>1649</item>
		</firstPaintWAVG>
		<firstPaintP25>
			<item>300</item>
			<item>300</item>
		</firstPaintP25>
		<firstPaintP50>
			<item>600</item>
			<item>600</item>
		</firstPaintP50>
		<firstPaintP75>
			<item>1400</item>
			<item>1400</item>
		</firstPaintP75>
		<firstPaintP90>
			<item>3600</item>
			<item>3400</item>
		</firstPaintP90>
		<firstPaintP95>
			<item>6700</item>
			<item>6000</item>
		</firstPaintP95>
		<firstPaintP99>
			<item>21700</item>
			<item>19500</item>
		</firstPaintP99>
		<firstContentfulPaintMAX>
			<item>120100</item>
			<item>120100</item>
		</firstContentfulPaintMAX>
		<firstContentfulPaintWAVG>
			<item>1703</item>
			<item>1604</item>
		</firstContentfulPaintWAVG>
		<firstContentfulPaintP25>
			<item>300</item>
			<item>300</item>
		</firstContentfulPaintP25>
		<firstContentfulPaintP50>
			<item>500</item>
			<item>600</item>
		</firstContentfulPaintP50>
		<firstContentfulPaintP75>
			<item>1300</item>
			<item>1300</item>
		</firstContentfulPaintP75>
		<firstContentfulPaintP90>
			<item>3500</item>
			<item>3300</item>
		</firstContentfulPaintP90>
		<firstContentfulPaintP95>
			<item>6500</item>
			<item>5800</item>
		</firstContentfulPaintP95>
		<firstContentfulPaintP99>
			<item>21500</item>
			<item>18900</item>
		</firstContentfulPaintP99>
		<domContentLoadedMAX>
			<item>120100</item>
			<item>120100</item>
		</domContentLoadedMAX>
		<domContentLoadedWAVG>
			<item>2206</item>
			<item>2103</item>
		</domContentLoadedWAVG>
		<domContentLoadedP25>
			<item>600</item>
			<item>600</item>
		</domContentLoadedP25>
		<domContentLoadedP50>
			<item>1100</item>
			<item>1100</item>
		</domContentLoadedP50>
		<domContentLoadedP75>
			<item>1900</item>
			<item>1800</item>
		</domContentLoadedP75>
		<domContentLoadedP90>
			<item>4200</item>
			<item>3900</item>
		</domContentLoadedP90>
		<domContentLoadedP95>
			<item>7600</item>
			<item>7000</item>
		</domContentLoadedP95>
		<domContentLoadedP99>
			<item>22500</item>
			<item>21600</item>
		</domContentLoadedP99>
	</data>
```

#### CSV
```csv
	queryTime,date,origin,pctSamples,firstPaintMAX,firstPaintWAVG,firstPaintP25,firstPaintP50,firstPaintP75,firstPaintP90,firstPaintP95,firstPaintP99,firstContentfulPaintMAX,firstContentfulPaintWAVG,firstContentfulPaintP25,firstContentfulPaintP50,firstContentfulPaintP75,firstContentfulPaintP90,firstContentfulPaintP95,firstContentfulPaintP99,domContentLoadedMAX,domContentLoadedWAVG,domContentLoadedP25,domContentLoadedP50,domContentLoadedP75,domContentLoadedP90,domContentLoadedP95,domContentLoadedP99
	946684800026,1564531200000,www.google.com,1,120100,1748,300,600,1400,3600,6700,21700,120100,1703,300,500,1300,3500,6500,21500,120100,2206,600,1100,1900,4200,7600,22500
	946684800026,1567209600000,www.google.com,1,120100,1649,300,600,1400,3400,6000,19500,120100,1604,300,600,1300,3300,5800,18900,120100,2103,600,1100,1800,3900,7000,21600
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.032Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"pctSamples": 1,
			"firstPaintMAX": 120100,
			"firstPaintWAVG": 1748,
			"firstPaintP25": 300,
			"firstPaintP50": 600,
			"firstPaintP75": 1400,
			"firstPaintP90": 3600,
			"firstPaintP95": 6700,
			"firstPaintP99": 21700,
			"firstContentfulPaintMAX": 120100,
			"firstContentfulPaintWAVG": 1703,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 500,
			"firstContentfulPaintP75": 1300,
			"firstContentfulPaintP90": 3500,
			"firstContentfulPaintP95": 6500,
			"firstContentfulPaintP99": 21500,
			"domContentLoadedMAX": 120100,
			"domContentLoadedWAVG": 2206,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1100,
			"domContentLoadedP75": 1900,
			"domContentLoadedP90": 4200,
			"domContentLoadedP95": 7600,
			"domContentLoadedP99": 22500
		},
		{
			"queryTime": "2000-01-01T00:00:00.032Z",
			"date": "2019-08-31T00:00:00.000Z",
			"origin": "www.google.com",
			"pctSamples": 1,
			"firstPaintMAX": 120100,
			"firstPaintWAVG": 1649,
			"firstPaintP25": 300,
			"firstPaintP50": 600,
			"firstPaintP75": 1400,
			"firstPaintP90": 3400,
			"firstPaintP95": 6000,
			"firstPaintP99": 19500,
			"firstContentfulPaintMAX": 120100,
			"firstContentfulPaintWAVG": 1604,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 600,
			"firstContentfulPaintP75": 1300,
			"firstContentfulPaintP90": 3300,
			"firstContentfulPaintP95": 5800,
			"firstContentfulPaintP99": 18900,
			"domContentLoadedMAX": 120100,
			"domContentLoadedWAVG": 2103,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1100,
			"domContentLoadedP75": 1800,
			"domContentLoadedP90": 3900,
			"domContentLoadedP95": 7000,
			"domContentLoadedP99": 21600
		}
		]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1748</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>600</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>3600</firstPaintP90>
			<firstPaintP95>6700</firstPaintP95>
			<firstPaintP99>21700</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1703</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>500</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3500</firstContentfulPaintP90>
			<firstContentfulPaintP95>6500</firstContentfulPaintP95>
			<firstContentfulPaintP99>21500</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2206</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1100</domContentLoadedP50>
			<domContentLoadedP75>1900</domContentLoadedP75>
			<domContentLoadedP90>4200</domContentLoadedP90>
			<domContentLoadedP95>7600</domContentLoadedP95>
			<domContentLoadedP99>22500</domContentLoadedP99>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1649</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>600</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>3400</firstPaintP90>
			<firstPaintP95>6000</firstPaintP95>
			<firstPaintP99>19500</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1604</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>600</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3300</firstContentfulPaintP90>
			<firstContentfulPaintP95>5800</firstContentfulPaintP95>
			<firstContentfulPaintP99>18900</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2103</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1100</domContentLoadedP50>
			<domContentLoadedP75>1800</domContentLoadedP75>
			<domContentLoadedP90>3900</domContentLoadedP90>
			<domContentLoadedP95>7000</domContentLoadedP95>
			<domContentLoadedP99>21600</domContentLoadedP99>
		</item>
	</data>
```

<!-- tabs:end -->
</p>
</details>




## `GET` /summary/form
`/summary/form` provides a breakdown of the aggregate metrics across by form factors for all network types for the given `domain`.

### Example
https://api.crux.run/v1/summary/form/?domain=google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property                 | Type     | Description                                                                                                                                                                                              |
|--------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queryTime                | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                                                                                                       |
| date                     | datetime | The array of dates that correspond to this record. Date here is the last day of the queried month.                                                                                                       |
| origin                   | string   | The origin of this record.                                                                                                                                                                               |
| formFactor               | string   | The form factor for this record.                                                                                                                                                                         |
| pctSamples               | float    | The total % of samples in this aggregate (should sum to 1 within the month pivot).                                                                                                                       |
| firstPaintMAX            | integer  | The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms.            |
| firstPaintWAVG           | integer  | The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                              |
| firstPaintP25            | integer  | The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                              |
| firstPaintP50            | integer  | The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                              |
| firstPaintP75            | integer  | The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                              |
| firstPaintP90            | integer  | The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                              |
| firstPaintP95            | integer  | The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                              |
| firstPaintP99            | integer  | The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                              |
| firstContentfulPaintMAX  | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| firstContentfulPaintWAVG | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| firstContentfulPaintP25  | integer  | The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                   |
| firstContentfulPaintP50  | integer  | The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                   |
| firstContentfulPaintP75  | integer  | The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                   |
| firstContentfulPaintP90  | integer  | The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                   |
| firstContentfulPaintP95  | integer  | The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                   |
| firstContentfulPaintP99  | integer  | The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                   |
| domContentLoadedMAX      | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| domContentLoadedWAVG     | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| domContentLoadedP25      | integer  | The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                               |
| domContentLoadedP50      | integer  | The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                               |
| domContentLoadedP75      | integer  | The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                               |
| domContentLoadedP90      | integer  | The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                               |
| domContentLoadedP95      | integer  | The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                               |
| domContentLoadedP99      | integer  | The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                         |



<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.024Z",
		"date": [
			"2019-07-31T00:00:00.000Z",
			"2019-07-31T00:00:00.000Z",
			"2019-07-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z"
		],
		"origin": [
			"www.google.com",
			"www.google.com",
			"www.google.com",
			"www.google.com",
			"www.google.com",
			"www.google.com"
		],
		"formFactor": [
			"desktop",
			"phone",
			"tablet",
			"desktop",
			"phone",
			"tablet"
		],
		"pctSamples": [
			0.6735000014305115,
			1,
			0.27140000462532043,
			0.695900022983551,
			1,
			0.20919999480247498
		],
		"firstPaintMAX": [
			120100,
			40000,
			40000,
			120100,
			50000,
			40000
		],
		"firstPaintWAVG": [
			3515,
			789,
			1090,
			2931,
			916,
			1215
		],
		"firstPaintP25": [
		400,
		300,
		400,
		300,
		300,
		400
		],
		"firstPaintP50": [
		1300,
		500,
		700,
		900,
		500,
		800
		],
		"firstPaintP75": [
		3500,
		800,
		1100,
		2800,
		900,
		1200
		],
		"firstPaintP90": [
		8300,
		1400,
		2100,
		6600,
		1800,
		2400
		],
		"firstPaintP95": [
		14600,
		2200,
		3200,
		12300,
		2800,
		3800
		],
		"firstPaintP99": [
		35000,
		6600,
		7600,
		30300,
		7800,
		8500
		],
		"firstContentfulPaintMAX": [
		120100,
		40000,
		40000,
		120100,
		50000,
		40000
		],
		"firstContentfulPaintWAVG": [
		3480,
		793,
		1098,
		2836,
		923,
		1255
		],
		"firstContentfulPaintP25": [
		300,
		300,
		400,
		300,
		300,
		400
		],
		"firstContentfulPaintP50": [
		1200,
		500,
		700,
		900,
		500,
		800
		],
		"firstContentfulPaintP75": [
		3400,
		800,
		1100,
		2600,
		900,
		1300
		],
		"firstContentfulPaintP90": [
		8300,
		1500,
		2100,
		6400,
		1800,
		2500
		],
		"firstContentfulPaintP95": [
		14700,
		2200,
		3200,
		12000,
		2900,
		3900
		],
		"firstContentfulPaintP99": [
		34800,
		6800,
		7800,
		29500,
		7800,
		9300
		],
		"domContentLoadedMAX": [
		120100,
		50000,
		40000,
		120100,
		50000,
		25000
		],
		"domContentLoadedWAVG": [
		3563,
		1243,
		1207,
		3078,
		1358,
		1251
		],
		"domContentLoadedP25": [
		800,
		500,
		400,
		600,
		600,
		400
		],
		"domContentLoadedP50": [
		1600,
		900,
		800,
		1300,
		1000,
		800
		],
		"domContentLoadedP75": [
		3400,
		1400,
		1300,
		2700,
		1500,
		1400
		],
		"domContentLoadedP90": [
		7900,
		2100,
		2300,
		6700,
		2400,
		2500
		],
		"domContentLoadedP95": [
		13800,
		3000,
		3400,
		12300,
		3500,
		3900
		],
		"domContentLoadedP99": [
		32900,
		7700,
		7900,
		29800,
		8800,
		8700
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.027Z</queryTime>
		<date>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
		</date>
		<origin>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
		</origin>
		<formFactor>
			<item>desktop</item>
			<item>phone</item>
			<item>tablet</item>
			<item>desktop</item>
			<item>phone</item>
			<item>tablet</item>
		</formFactor>
		<pctSamples>
			<item>0.6735000014305115</item>
			<item>1</item>
			<item>0.27140000462532043</item>
			<item>0.695900022983551</item>
			<item>1</item>
			<item>0.20919999480247498</item>
		</pctSamples>
		<firstPaintMAX>
			<item>120100</item>
			<item>40000</item>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>40000</item>
		</firstPaintMAX>
		<firstPaintWAVG>
			<item>3515</item>
			<item>789</item>
			<item>1090</item>
			<item>2931</item>
			<item>916</item>
			<item>1215</item>
			</firstPaintWAVG>
			<firstPaintP25>
			<item>400</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
			<item>300</item>
			<item>400</item>
			</firstPaintP25>
			<firstPaintP50>
			<item>1300</item>
			<item>500</item>
			<item>700</item>
			<item>900</item>
			<item>500</item>
			<item>800</item>
			</firstPaintP50>
			<firstPaintP75>
			<item>3500</item>
			<item>800</item>
			<item>1100</item>
			<item>2800</item>
			<item>900</item>
			<item>1200</item>
			</firstPaintP75>
			<firstPaintP90>
			<item>8300</item>
			<item>1400</item>
			<item>2100</item>
			<item>6600</item>
			<item>1800</item>
			<item>2400</item>
			</firstPaintP90>
			<firstPaintP95>
			<item>14600</item>
			<item>2200</item>
			<item>3200</item>
			<item>12300</item>
			<item>2800</item>
			<item>3800</item>
			</firstPaintP95>
			<firstPaintP99>
			<item>35000</item>
			<item>6600</item>
			<item>7600</item>
			<item>30300</item>
			<item>7800</item>
			<item>8500</item>
			</firstPaintP99>
			<firstContentfulPaintMAX>
			<item>120100</item>
			<item>40000</item>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>40000</item>
			</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>
			<item>3480</item>
			<item>793</item>
			<item>1098</item>
			<item>2836</item>
			<item>923</item>
			<item>1255</item>
			</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>
			<item>300</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
			<item>300</item>
			<item>400</item>
			</firstContentfulPaintP25>
			<firstContentfulPaintP50>
			<item>1200</item>
			<item>500</item>
			<item>700</item>
			<item>900</item>
			<item>500</item>
			<item>800</item>
			</firstContentfulPaintP50>
			<firstContentfulPaintP75>
			<item>3400</item>
			<item>800</item>
			<item>1100</item>
			<item>2600</item>
			<item>900</item>
			<item>1300</item>
			</firstContentfulPaintP75>
			<firstContentfulPaintP90>
			<item>8300</item>
			<item>1500</item>
			<item>2100</item>
			<item>6400</item>
			<item>1800</item>
			<item>2500</item>
			</firstContentfulPaintP90>
			<firstContentfulPaintP95>
			<item>14700</item>
			<item>2200</item>
			<item>3200</item>
			<item>12000</item>
			<item>2900</item>
			<item>3900</item>
			</firstContentfulPaintP95>
			<firstContentfulPaintP99>
			<item>34800</item>
			<item>6800</item>
			<item>7800</item>
			<item>29500</item>
			<item>7800</item>
			<item>9300</item>
			</firstContentfulPaintP99>
			<domContentLoadedMAX>
			<item>120100</item>
			<item>50000</item>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>25000</item>
			</domContentLoadedMAX>
			<domContentLoadedWAVG>
			<item>3563</item>
			<item>1243</item>
			<item>1207</item>
			<item>3078</item>
			<item>1358</item>
			<item>1251</item>
			</domContentLoadedWAVG>
			<domContentLoadedP25>
			<item>800</item>
			<item>500</item>
			<item>400</item>
			<item>600</item>
			<item>600</item>
			<item>400</item>
			</domContentLoadedP25>
			<domContentLoadedP50>
			<item>1600</item>
			<item>900</item>
			<item>800</item>
			<item>1300</item>
			<item>1000</item>
			<item>800</item>
			</domContentLoadedP50>
			<domContentLoadedP75>
			<item>3400</item>
			<item>1400</item>
			<item>1300</item>
			<item>2700</item>
			<item>1500</item>
			<item>1400</item>
			</domContentLoadedP75>
			<domContentLoadedP90>
			<item>7900</item>
			<item>2100</item>
			<item>2300</item>
			<item>6700</item>
			<item>2400</item>
			<item>2500</item>
			</domContentLoadedP90>
			<domContentLoadedP95>
			<item>13800</item>
			<item>3000</item>
			<item>3400</item>
			<item>12300</item>
			<item>3500</item>
			<item>3900</item>
			</domContentLoadedP95>
			<domContentLoadedP99>
			<item>32900</item>
			<item>7700</item>
			<item>7900</item>
			<item>29800</item>
			<item>8800</item>
			<item>8700</item>
			</domContentLoadedP99>
			</data>
```

#### CSV
```csv
	queryTime,date,origin,formFactor,pctSamples,firstPaintMAX,firstPaintWAVG,firstPaintP25,firstPaintP50,firstPaintP75,firstPaintP90,firstPaintP95,firstPaintP99,firstContentfulPaintMAX,firstContentfulPaintWAVG,firstContentfulPaintP25,firstContentfulPaintP50,firstContentfulPaintP75,firstContentfulPaintP90,firstContentfulPaintP95,firstContentfulPaintP99,domContentLoadedMAX,domContentLoadedWAVG,domContentLoadedP25,domContentLoadedP50,domContentLoadedP75,domContentLoadedP90,domContentLoadedP95,domContentLoadedP99
	946684800024,1564531200000,www.google.com,desktop,0.6735000014305115,120100,3515,400,1300,3500,8300,14600,35000,120100,3480,300,1200,3400,8300,14700,34800,120100,3563,800,1600,3400,7900,13800,32900
	946684800024,1564531200000,www.google.com,phone,1,40000,789,300,500,800,1400,2200,6600,40000,793,300,500,800,1500,2200,6800,50000,1243,500,900,1400,2100,3000,7700
	946684800024,1564531200000,www.google.com,tablet,0.27140000462532043,40000,1090,400,700,1100,2100,3200,7600,40000,1098,400,700,1100,2100,3200,7800,40000,1207,400,800,1300,2300,3400,7900
	946684800024,1567209600000,www.google.com,desktop,0.695900022983551,120100,2931,300,900,2800,6600,12300,30300,120100,2836,300,900,2600,6400,12000,29500,120100,3078,600,1300,2700,6700,12300,29800
	946684800024,1567209600000,www.google.com,phone,1,50000,916,300,500,900,1800,2800,7800,50000,923,300,500,900,1800,2900,7800,50000,1358,600,1000,1500,2400,3500,8800
	946684800024,1567209600000,www.google.com,tablet,0.20919999480247498,40000,1215,400,800,1200,2400,3800,8500,40000,1255,400,800,1300,2500,3900,9300,25000,1251,400,800,1400,2500,3900,8700
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.026Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"formFactor": "desktop",
			"pctSamples": 0.6735000014305115,
			"firstPaintMAX": 120100,
			"firstPaintWAVG": 3515,
			"firstPaintP25": 400,
			"firstPaintP50": 1300,
			"firstPaintP75": 3500,
			"firstPaintP90": 8300,
			"firstPaintP95": 14600,
			"firstPaintP99": 35000,
			"firstContentfulPaintMAX": 120100,
			"firstContentfulPaintWAVG": 3480,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 1200,
			"firstContentfulPaintP75": 3400,
			"firstContentfulPaintP90": 8300,
			"firstContentfulPaintP95": 14700,
			"firstContentfulPaintP99": 34800,
			"domContentLoadedMAX": 120100,
			"domContentLoadedWAVG": 3563,
			"domContentLoadedP25": 800,
			"domContentLoadedP50": 1600,
			"domContentLoadedP75": 3400,
			"domContentLoadedP90": 7900,
			"domContentLoadedP95": 13800,
			"domContentLoadedP99": 32900
		},
		{
			"queryTime": "2000-01-01T00:00:00.026Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"formFactor": "phone",
			"pctSamples": 1,
			"firstPaintMAX": 40000,
			"firstPaintWAVG": 789,
			"firstPaintP25": 300,
			"firstPaintP50": 500,
			"firstPaintP75": 800,
			"firstPaintP90": 1400,
			"firstPaintP95": 2200,
			"firstPaintP99": 6600,
			"firstContentfulPaintMAX": 40000,
			"firstContentfulPaintWAVG": 793,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 500,
			"firstContentfulPaintP75": 800,
			"firstContentfulPaintP90": 1500,
			"firstContentfulPaintP95": 2200,
			"firstContentfulPaintP99": 6800,
			"domContentLoadedMAX": 50000,
			"domContentLoadedWAVG": 1243,
			"domContentLoadedP25": 500,
			"domContentLoadedP50": 900,
			"domContentLoadedP75": 1400,
			"domContentLoadedP90": 2100,
			"domContentLoadedP95": 3000,
			"domContentLoadedP99": 7700
			},
			{
				"queryTime": "2000-01-01T00:00:00.026Z",
				"date": "2019-07-31T00:00:00.000Z",
				"origin": "www.google.com",
				"formFactor": "tablet",
				"pctSamples": 0.27140000462532043,
				"firstPaintMAX": 40000,
				"firstPaintWAVG": 1090,
				"firstPaintP25": 400,
				"firstPaintP50": 700,
				"firstPaintP75": 1100,
				"firstPaintP90": 2100,
				"firstPaintP95": 3200,
				"firstPaintP99": 7600,
				"firstContentfulPaintMAX": 40000,
				"firstContentfulPaintWAVG": 1098,
				"firstContentfulPaintP25": 400,
				"firstContentfulPaintP50": 700,
				"firstContentfulPaintP75": 1100,
				"firstContentfulPaintP90": 2100,
				"firstContentfulPaintP95": 3200,
				"firstContentfulPaintP99": 7800,
				"domContentLoadedMAX": 40000,
				"domContentLoadedWAVG": 1207,
				"domContentLoadedP25": 400,
				"domContentLoadedP50": 800,
				"domContentLoadedP75": 1300,
				"domContentLoadedP90": 2300,
				"domContentLoadedP95": 3400,
				"domContentLoadedP99": 7900
				},
				{
					"queryTime": "2000-01-01T00:00:00.026Z",
					"date": "2019-08-31T00:00:00.000Z",
					"origin": "www.google.com",
					"formFactor": "desktop",
					"pctSamples": 0.695900022983551,
					"firstPaintMAX": 120100,
					"firstPaintWAVG": 2931,
					"firstPaintP25": 300,
					"firstPaintP50": 900,
					"firstPaintP75": 2800,
					"firstPaintP90": 6600,
					"firstPaintP95": 12300,
					"firstPaintP99": 30300,
					"firstContentfulPaintMAX": 120100,
					"firstContentfulPaintWAVG": 2836,
					"firstContentfulPaintP25": 300,
					"firstContentfulPaintP50": 900,
					"firstContentfulPaintP75": 2600,
					"firstContentfulPaintP90": 6400,
					"firstContentfulPaintP95": 12000,
					"firstContentfulPaintP99": 29500,
					"domContentLoadedMAX": 120100,
					"domContentLoadedWAVG": 3078,
					"domContentLoadedP25": 600,
					"domContentLoadedP50": 1300,
					"domContentLoadedP75": 2700,
					"domContentLoadedP90": 6700,
					"domContentLoadedP95": 12300,
					"domContentLoadedP99": 29800
					},
					{
						"queryTime": "2000-01-01T00:00:00.026Z",
						"date": "2019-08-31T00:00:00.000Z",
						"origin": "www.google.com",
						"formFactor": "phone",
						"pctSamples": 1,
						"firstPaintMAX": 50000,
						"firstPaintWAVG": 916,
						"firstPaintP25": 300,
						"firstPaintP50": 500,
						"firstPaintP75": 900,
						"firstPaintP90": 1800,
						"firstPaintP95": 2800,
						"firstPaintP99": 7800,
						"firstContentfulPaintMAX": 50000,
						"firstContentfulPaintWAVG": 923,
						"firstContentfulPaintP25": 300,
						"firstContentfulPaintP50": 500,
						"firstContentfulPaintP75": 900,
						"firstContentfulPaintP90": 1800,
						"firstContentfulPaintP95": 2900,
						"firstContentfulPaintP99": 7800,
						"domContentLoadedMAX": 50000,
						"domContentLoadedWAVG": 1358,
						"domContentLoadedP25": 600,
						"domContentLoadedP50": 1000,
						"domContentLoadedP75": 1500,
						"domContentLoadedP90": 2400,
						"domContentLoadedP95": 3500,
						"domContentLoadedP99": 8800
						},
						{
							"queryTime": "2000-01-01T00:00:00.026Z",
							"date": "2019-08-31T00:00:00.000Z",
							"origin": "www.google.com",
							"formFactor": "tablet",
							"pctSamples": 0.20919999480247498,
							"firstPaintMAX": 40000,
							"firstPaintWAVG": 1215,
							"firstPaintP25": 400,
							"firstPaintP50": 800,
							"firstPaintP75": 1200,
							"firstPaintP90": 2400,
							"firstPaintP95": 3800,
							"firstPaintP99": 8500,
							"firstContentfulPaintMAX": 40000,
							"firstContentfulPaintWAVG": 1255,
							"firstContentfulPaintP25": 400,
							"firstContentfulPaintP50": 800,
							"firstContentfulPaintP75": 1300,
							"firstContentfulPaintP90": 2500,
							"firstContentfulPaintP95": 3900,
							"firstContentfulPaintP99": 9300,
							"domContentLoadedMAX": 25000,
							"domContentLoadedWAVG": 1251,
							"domContentLoadedP25": 400,
							"domContentLoadedP50": 800,
							"domContentLoadedP75": 1400,
							"domContentLoadedP90": 2500,
							"domContentLoadedP95": 3900,
							"domContentLoadedP99": 8700
						}
						]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>desktop</formFactor>
			<pctSamples>0.6735000014305115</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>3515</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>1300</firstPaintP50>
			<firstPaintP75>3500</firstPaintP75>
			<firstPaintP90>8300</firstPaintP90>
			<firstPaintP95>14600</firstPaintP95>
			<firstPaintP99>35000</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>3480</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>1200</firstContentfulPaintP50>
			<firstContentfulPaintP75>3400</firstContentfulPaintP75>
			<firstContentfulPaintP90>8300</firstContentfulPaintP90>
			<firstContentfulPaintP95>14700</firstContentfulPaintP95>
			<firstContentfulPaintP99>34800</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>3563</domContentLoadedWAVG>
			<domContentLoadedP25>800</domContentLoadedP25>
			<domContentLoadedP50>1600</domContentLoadedP50>
			<domContentLoadedP75>3400</domContentLoadedP75>
			<domContentLoadedP90>7900</domContentLoadedP90>
			<domContentLoadedP95>13800</domContentLoadedP95>
			<domContentLoadedP99>32900</domContentLoadedP99>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>phone</formFactor>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>40000</firstPaintMAX>
			<firstPaintWAVG>789</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>500</firstPaintP50>
			<firstPaintP75>800</firstPaintP75>
			<firstPaintP90>1400</firstPaintP90>
			<firstPaintP95>2200</firstPaintP95>
			<firstPaintP99>6600</firstPaintP99>
			<firstContentfulPaintMAX>40000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>793</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>500</firstContentfulPaintP50>
			<firstContentfulPaintP75>800</firstContentfulPaintP75>
			<firstContentfulPaintP90>1500</firstContentfulPaintP90>
			<firstContentfulPaintP95>2200</firstContentfulPaintP95>
			<firstContentfulPaintP99>6800</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>1243</domContentLoadedWAVG>
			<domContentLoadedP25>500</domContentLoadedP25>
			<domContentLoadedP50>900</domContentLoadedP50>
			<domContentLoadedP75>1400</domContentLoadedP75>
			<domContentLoadedP90>2100</domContentLoadedP90>
			<domContentLoadedP95>3000</domContentLoadedP95>
			<domContentLoadedP99>7700</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>tablet</formFactor>
			<pctSamples>0.27140000462532043</pctSamples>
			<firstPaintMAX>40000</firstPaintMAX>
			<firstPaintWAVG>1090</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>700</firstPaintP50>
			<firstPaintP75>1100</firstPaintP75>
			<firstPaintP90>2100</firstPaintP90>
			<firstPaintP95>3200</firstPaintP95>
			<firstPaintP99>7600</firstPaintP99>
			<firstContentfulPaintMAX>40000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1098</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>700</firstContentfulPaintP50>
			<firstContentfulPaintP75>1100</firstContentfulPaintP75>
			<firstContentfulPaintP90>2100</firstContentfulPaintP90>
			<firstContentfulPaintP95>3200</firstContentfulPaintP95>
			<firstContentfulPaintP99>7800</firstContentfulPaintP99>
			<domContentLoadedMAX>40000</domContentLoadedMAX>
			<domContentLoadedWAVG>1207</domContentLoadedWAVG>
			<domContentLoadedP25>400</domContentLoadedP25>
			<domContentLoadedP50>800</domContentLoadedP50>
			<domContentLoadedP75>1300</domContentLoadedP75>
			<domContentLoadedP90>2300</domContentLoadedP90>
			<domContentLoadedP95>3400</domContentLoadedP95>
			<domContentLoadedP99>7900</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>desktop</formFactor>
			<pctSamples>0.695900022983551</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>2931</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>900</firstPaintP50>
			<firstPaintP75>2800</firstPaintP75>
			<firstPaintP90>6600</firstPaintP90>
			<firstPaintP95>12300</firstPaintP95>
			<firstPaintP99>30300</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>2836</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>900</firstContentfulPaintP50>
			<firstContentfulPaintP75>2600</firstContentfulPaintP75>
			<firstContentfulPaintP90>6400</firstContentfulPaintP90>
			<firstContentfulPaintP95>12000</firstContentfulPaintP95>
			<firstContentfulPaintP99>29500</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>3078</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1300</domContentLoadedP50>
			<domContentLoadedP75>2700</domContentLoadedP75>
			<domContentLoadedP90>6700</domContentLoadedP90>
			<domContentLoadedP95>12300</domContentLoadedP95>
			<domContentLoadedP99>29800</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>phone</formFactor>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>50000</firstPaintMAX>
			<firstPaintWAVG>916</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>500</firstPaintP50>
			<firstPaintP75>900</firstPaintP75>
			<firstPaintP90>1800</firstPaintP90>
			<firstPaintP95>2800</firstPaintP95>
			<firstPaintP99>7800</firstPaintP99>
			<firstContentfulPaintMAX>50000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>923</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>500</firstContentfulPaintP50>
			<firstContentfulPaintP75>900</firstContentfulPaintP75>
			<firstContentfulPaintP90>1800</firstContentfulPaintP90>
			<firstContentfulPaintP95>2900</firstContentfulPaintP95>
			<firstContentfulPaintP99>7800</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>1358</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1000</domContentLoadedP50>
			<domContentLoadedP75>1500</domContentLoadedP75>
			<domContentLoadedP90>2400</domContentLoadedP90>
			<domContentLoadedP95>3500</domContentLoadedP95>
			<domContentLoadedP99>8800</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<formFactor>tablet</formFactor>
			<pctSamples>0.20919999480247498</pctSamples>
			<firstPaintMAX>40000</firstPaintMAX>
			<firstPaintWAVG>1215</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>800</firstPaintP50>
			<firstPaintP75>1200</firstPaintP75>
			<firstPaintP90>2400</firstPaintP90>
			<firstPaintP95>3800</firstPaintP95>
			<firstPaintP99>8500</firstPaintP99>
			<firstContentfulPaintMAX>40000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1255</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>800</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>2500</firstContentfulPaintP90>
			<firstContentfulPaintP95>3900</firstContentfulPaintP95>
			<firstContentfulPaintP99>9300</firstContentfulPaintP99>
			<domContentLoadedMAX>25000</domContentLoadedMAX>
			<domContentLoadedWAVG>1251</domContentLoadedWAVG>
			<domContentLoadedP25>400</domContentLoadedP25>
			<domContentLoadedP50>800</domContentLoadedP50>
			<domContentLoadedP75>1400</domContentLoadedP75>
			<domContentLoadedP90>2500</domContentLoadedP90>
			<domContentLoadedP95>3900</domContentLoadedP95>
			<domContentLoadedP99>8700</domContentLoadedP99>
			</item>
			</data>
```

<!-- tabs:end -->

</p>
</details>






## `GET` /summary/connection
`/summary/connection` provides a breakdown of the aggregate metrics across all form factors but broken down by network types for the given `domain`.

### Example
https://api.crux.run/v1/summary/connection/?domain=google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property                 | Type     | Description                                                                                                                                                                                              |
|--------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queryTime                | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                                                                                                       |
| date                     | datetime | The array of dates that correspond to this record. Date here is the last day of the queried month.                                                                                                       |
| origin                   | string   | The origin of this record.                                                                                                                                                                               |
| effectiveConnectionType  | string   | The effective connection type for this record. Note 4G just means 'fast' and also includes cable/broadband.                                                                                              |
| pctSamples               | float    | The total % of samples in this aggregate (should sum to 1 within the month pivot).                                                                                                                       |
| firstPaintMAX            | integer  | The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms.            |
| firstPaintWAVG           | integer  | The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                              |
| firstPaintP25            | integer  | The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                              |
| firstPaintP50            | integer  | The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                              |
| firstPaintP75            | integer  | The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                              |
| firstPaintP90            | integer  | The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                              |
| firstPaintP95            | integer  | The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                              |
| firstPaintP99            | integer  | The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                              |
| firstContentfulPaintMAX  | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| firstContentfulPaintWAVG | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| firstContentfulPaintP25  | integer  | The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                   |
| firstContentfulPaintP50  | integer  | The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                   |
| firstContentfulPaintP75  | integer  | The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                   |
| firstContentfulPaintP90  | integer  | The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                   |
| firstContentfulPaintP95  | integer  | The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                   |
| firstContentfulPaintP99  | integer  | The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                   |
| domContentLoadedMAX      | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| domContentLoadedWAVG     | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| domContentLoadedP25      | integer  | The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                               |
| domContentLoadedP50      | integer  | The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                               |
| domContentLoadedP75      | integer  | The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                               |
| domContentLoadedP90      | integer  | The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                               |
| domContentLoadedP95      | integer  | The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                               |
| domContentLoadedP99      | integer  | The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                         |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.027Z",
		"date": [
			"2019-07-31T00:00:00.000Z",
			"2019-07-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z"
		],
		"origin": [
			"www.google.com",
			"www.google.com",
			"www.google.com",
			"www.google.com"
		],
		"effectiveConnectionType": [
			"3G",
			"4G",
			"3G",
			"4G"
		],
		"pctSamples": [
			0.19699999690055847,
			1,
			0.19869999587535858,
			1
		],
		"firstPaintMAX": [
			40000,
			120100,
			50000,
			120100
		],
		"firstPaintWAVG": [
			1435,
			1783,
			1613,
			1653
		],
		"firstPaintP25": [
			400,
			300,
			400,
			300
		],
		"firstPaintP50": [
			700,
			500,
			700,
			600
		],
		"firstPaintP75": [
		1400,
		1400,
		1500,
		1300
		],
		"firstPaintP90": [
		2900,
		3700,
		3400,
		3400
		],
		"firstPaintP95": [
		4900,
		6900,
		5800,
		6000
		],
		"firstPaintP99": [
		14200,
		22400,
		15500,
		19900
		],
		"firstContentfulPaintMAX": [
		40000,
		120100,
		50000,
		120100
		],
		"firstContentfulPaintWAVG": [
		1447,
		1732,
		1610,
		1603
		],
		"firstContentfulPaintP25": [
		400,
		300,
		400,
		300
		],
		"firstContentfulPaintP50": [
		700,
		500,
		800,
		600
		],
		"firstContentfulPaintP75": [
		1400,
		1300,
		1500,
		1300
		],
		"firstContentfulPaintP90": [
		2800,
		3600,
		3400,
		3200
		],
		"firstContentfulPaintP95": [
		4900,
		6700,
		5800,
		5800
		],
		"firstContentfulPaintP99": [
		14400,
		22200,
		15300,
		19400
		],
		"domContentLoadedMAX": [
		50000,
		120100,
		50000,
		120100
		],
		"domContentLoadedWAVG": [
		1961,
		2229,
		2182,
		2096
		],
		"domContentLoadedP25": [
		600,
		600,
		600,
		600
		],
		"domContentLoadedP50": [
		1200,
		1100,
		1200,
		1000
		],
		"domContentLoadedP75": [
		2000,
		1900,
		2200,
		1800
		],
		"domContentLoadedP90": [
		3900,
		4300,
		4400,
		3900
		],
		"domContentLoadedP95": [
		6200,
		7700,
		7500,
		6900
		],
		"domContentLoadedP99": [
		17300,
		22900,
		19100,
		21800
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.025Z</queryTime>
		<date>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
		</date>
		<origin>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
		</origin>
		<effectiveConnectionType>
			<item>3G</item>
			<item>4G</item>
			<item>3G</item>
			<item>4G</item>
		</effectiveConnectionType>
		<pctSamples>
			<item>0.19699999690055847</item>
			<item>1</item>
			<item>0.19869999587535858</item>
			<item>1</item>
		</pctSamples>
		<firstPaintMAX>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
		</firstPaintMAX>
		<firstPaintWAVG>
			<item>1435</item>
			<item>1783</item>
			<item>1613</item>
			<item>1653</item>
		</firstPaintWAVG>
		<firstPaintP25>
			<item>400</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
		</firstPaintP25>
		<firstPaintP50>
			<item>700</item>
			<item>500</item>
			<item>700</item>
			<item>600</item>
			</firstPaintP50>
			<firstPaintP75>
			<item>1400</item>
			<item>1400</item>
			<item>1500</item>
			<item>1300</item>
			</firstPaintP75>
			<firstPaintP90>
			<item>2900</item>
			<item>3700</item>
			<item>3400</item>
			<item>3400</item>
			</firstPaintP90>
			<firstPaintP95>
			<item>4900</item>
			<item>6900</item>
			<item>5800</item>
			<item>6000</item>
			</firstPaintP95>
			<firstPaintP99>
			<item>14200</item>
			<item>22400</item>
			<item>15500</item>
			<item>19900</item>
			</firstPaintP99>
			<firstContentfulPaintMAX>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
			</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>
			<item>1447</item>
			<item>1732</item>
			<item>1610</item>
			<item>1603</item>
			</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>
			<item>400</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
			</firstContentfulPaintP25>
			<firstContentfulPaintP50>
			<item>700</item>
			<item>500</item>
			<item>800</item>
			<item>600</item>
			</firstContentfulPaintP50>
			<firstContentfulPaintP75>
			<item>1400</item>
			<item>1300</item>
			<item>1500</item>
			<item>1300</item>
			</firstContentfulPaintP75>
			<firstContentfulPaintP90>
			<item>2800</item>
			<item>3600</item>
			<item>3400</item>
			<item>3200</item>
			</firstContentfulPaintP90>
			<firstContentfulPaintP95>
			<item>4900</item>
			<item>6700</item>
			<item>5800</item>
			<item>5800</item>
			</firstContentfulPaintP95>
			<firstContentfulPaintP99>
			<item>14400</item>
			<item>22200</item>
			<item>15300</item>
			<item>19400</item>
			</firstContentfulPaintP99>
			<domContentLoadedMAX>
			<item>50000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
			</domContentLoadedMAX>
			<domContentLoadedWAVG>
			<item>1961</item>
			<item>2229</item>
			<item>2182</item>
			<item>2096</item>
			</domContentLoadedWAVG>
			<domContentLoadedP25>
			<item>600</item>
			<item>600</item>
			<item>600</item>
			<item>600</item>
			</domContentLoadedP25>
			<domContentLoadedP50>
			<item>1200</item>
			<item>1100</item>
			<item>1200</item>
			<item>1000</item>
			</domContentLoadedP50>
			<domContentLoadedP75>
			<item>2000</item>
			<item>1900</item>
			<item>2200</item>
			<item>1800</item>
			</domContentLoadedP75>
			<domContentLoadedP90>
			<item>3900</item>
			<item>4300</item>
			<item>4400</item>
			<item>3900</item>
			</domContentLoadedP90>
			<domContentLoadedP95>
			<item>6200</item>
			<item>7700</item>
			<item>7500</item>
			<item>6900</item>
			</domContentLoadedP95>
			<domContentLoadedP99>
			<item>17300</item>
			<item>22900</item>
			<item>19100</item>
			<item>21800</item>
			</domContentLoadedP99>
			</data>
```

#### CSV
```csv
	queryTime,date,origin,effectiveConnectionType,pctSamples,firstPaintMAX,firstPaintWAVG,firstPaintP25,firstPaintP50,firstPaintP75,firstPaintP90,firstPaintP95,firstPaintP99,firstContentfulPaintMAX,firstContentfulPaintWAVG,firstContentfulPaintP25,firstContentfulPaintP50,firstContentfulPaintP75,firstContentfulPaintP90,firstContentfulPaintP95,firstContentfulPaintP99,domContentLoadedMAX,domContentLoadedWAVG,domContentLoadedP25,domContentLoadedP50,domContentLoadedP75,domContentLoadedP90,domContentLoadedP95,domContentLoadedP99
	946684800027,1564531200000,www.google.com,3G,0.19699999690055847,40000,1435,400,700,1400,2900,4900,14200,40000,1447,400,700,1400,2800,4900,14400,50000,1961,600,1200,2000,3900,6200,17300
	946684800027,1564531200000,www.google.com,4G,1,120100,1783,300,500,1400,3700,6900,22400,120100,1732,300,500,1300,3600,6700,22200,120100,2229,600,1100,1900,4300,7700,22900
	946684800027,1567209600000,www.google.com,3G,0.19869999587535858,50000,1613,400,700,1500,3400,5800,15500,50000,1610,400,800,1500,3400,5800,15300,50000,2182,600,1200,2200,4400,7500,19100
	946684800027,1567209600000,www.google.com,4G,1,120100,1653,300,600,1300,3400,6000,19900,120100,1603,300,600,1300,3200,5800,19400,120100,2096,600,1000,1800,3900,6900,21800
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.026Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"effectiveConnectionType": "3G",
			"pctSamples": 0.19699999690055847,
			"firstPaintMAX": 40000,
			"firstPaintWAVG": 1435,
			"firstPaintP25": 400,
			"firstPaintP50": 700,
			"firstPaintP75": 1400,
			"firstPaintP90": 2900,
			"firstPaintP95": 4900,
			"firstPaintP99": 14200,
			"firstContentfulPaintMAX": 40000,
			"firstContentfulPaintWAVG": 1447,
			"firstContentfulPaintP25": 400,
			"firstContentfulPaintP50": 700,
			"firstContentfulPaintP75": 1400,
			"firstContentfulPaintP90": 2800,
			"firstContentfulPaintP95": 4900,
			"firstContentfulPaintP99": 14400,
			"domContentLoadedMAX": 50000,
			"domContentLoadedWAVG": 1961,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1200,
			"domContentLoadedP75": 2000,
			"domContentLoadedP90": 3900,
			"domContentLoadedP95": 6200,
			"domContentLoadedP99": 17300
		},
		{
			"queryTime": "2000-01-01T00:00:00.026Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"effectiveConnectionType": "4G",
			"pctSamples": 1,
			"firstPaintMAX": 120100,
			"firstPaintWAVG": 1783,
			"firstPaintP25": 300,
			"firstPaintP50": 500,
			"firstPaintP75": 1400,
			"firstPaintP90": 3700,
			"firstPaintP95": 6900,
			"firstPaintP99": 22400,
			"firstContentfulPaintMAX": 120100,
			"firstContentfulPaintWAVG": 1732,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 500,
			"firstContentfulPaintP75": 1300,
			"firstContentfulPaintP90": 3600,
			"firstContentfulPaintP95": 6700,
			"firstContentfulPaintP99": 22200,
			"domContentLoadedMAX": 120100,
			"domContentLoadedWAVG": 2229,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1100,
			"domContentLoadedP75": 1900,
			"domContentLoadedP90": 4300,
			"domContentLoadedP95": 7700,
			"domContentLoadedP99": 22900
			},
			{
				"queryTime": "2000-01-01T00:00:00.026Z",
				"date": "2019-08-31T00:00:00.000Z",
				"origin": "www.google.com",
				"effectiveConnectionType": "3G",
				"pctSamples": 0.19869999587535858,
				"firstPaintMAX": 50000,
				"firstPaintWAVG": 1613,
				"firstPaintP25": 400,
				"firstPaintP50": 700,
				"firstPaintP75": 1500,
				"firstPaintP90": 3400,
				"firstPaintP95": 5800,
				"firstPaintP99": 15500,
				"firstContentfulPaintMAX": 50000,
				"firstContentfulPaintWAVG": 1610,
				"firstContentfulPaintP25": 400,
				"firstContentfulPaintP50": 800,
				"firstContentfulPaintP75": 1500,
				"firstContentfulPaintP90": 3400,
				"firstContentfulPaintP95": 5800,
				"firstContentfulPaintP99": 15300,
				"domContentLoadedMAX": 50000,
				"domContentLoadedWAVG": 2182,
				"domContentLoadedP25": 600,
				"domContentLoadedP50": 1200,
				"domContentLoadedP75": 2200,
				"domContentLoadedP90": 4400,
				"domContentLoadedP95": 7500,
				"domContentLoadedP99": 19100
				},
				{
					"queryTime": "2000-01-01T00:00:00.026Z",
					"date": "2019-08-31T00:00:00.000Z",
					"origin": "www.google.com",
					"effectiveConnectionType": "4G",
					"pctSamples": 1,
					"firstPaintMAX": 120100,
					"firstPaintWAVG": 1653,
					"firstPaintP25": 300,
					"firstPaintP50": 600,
					"firstPaintP75": 1300,
					"firstPaintP90": 3400,
					"firstPaintP95": 6000,
					"firstPaintP99": 19900,
					"firstContentfulPaintMAX": 120100,
					"firstContentfulPaintWAVG": 1603,
					"firstContentfulPaintP25": 300,
					"firstContentfulPaintP50": 600,
					"firstContentfulPaintP75": 1300,
					"firstContentfulPaintP90": 3200,
					"firstContentfulPaintP95": 5800,
					"firstContentfulPaintP99": 19400,
					"domContentLoadedMAX": 120100,
					"domContentLoadedWAVG": 2096,
					"domContentLoadedP25": 600,
					"domContentLoadedP50": 1000,
					"domContentLoadedP75": 1800,
					"domContentLoadedP90": 3900,
					"domContentLoadedP95": 6900,
					"domContentLoadedP99": 21800
				}
				]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>3G</effectiveConnectionType>
			<pctSamples>0.19699999690055847</pctSamples>
			<firstPaintMAX>40000</firstPaintMAX>
			<firstPaintWAVG>1435</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>700</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>2900</firstPaintP90>
			<firstPaintP95>4900</firstPaintP95>
			<firstPaintP99>14200</firstPaintP99>
			<firstContentfulPaintMAX>40000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1447</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>700</firstContentfulPaintP50>
			<firstContentfulPaintP75>1400</firstContentfulPaintP75>
			<firstContentfulPaintP90>2800</firstContentfulPaintP90>
			<firstContentfulPaintP95>4900</firstContentfulPaintP95>
			<firstContentfulPaintP99>14400</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>1961</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1200</domContentLoadedP50>
			<domContentLoadedP75>2000</domContentLoadedP75>
			<domContentLoadedP90>3900</domContentLoadedP90>
			<domContentLoadedP95>6200</domContentLoadedP95>
			<domContentLoadedP99>17300</domContentLoadedP99>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>4G</effectiveConnectionType>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1783</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>500</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>3700</firstPaintP90>
			<firstPaintP95>6900</firstPaintP95>
			<firstPaintP99>22400</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1732</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>500</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3600</firstContentfulPaintP90>
			<firstContentfulPaintP95>6700</firstContentfulPaintP95>
			<firstContentfulPaintP99>22200</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2229</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1100</domContentLoadedP50>
			<domContentLoadedP75>1900</domContentLoadedP75>
			<domContentLoadedP90>4300</domContentLoadedP90>
			<domContentLoadedP95>7700</domContentLoadedP95>
			<domContentLoadedP99>22900</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>3G</effectiveConnectionType>
			<pctSamples>0.19869999587535858</pctSamples>
			<firstPaintMAX>50000</firstPaintMAX>
			<firstPaintWAVG>1613</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>700</firstPaintP50>
			<firstPaintP75>1500</firstPaintP75>
			<firstPaintP90>3400</firstPaintP90>
			<firstPaintP95>5800</firstPaintP95>
			<firstPaintP99>15500</firstPaintP99>
			<firstContentfulPaintMAX>50000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1610</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>800</firstContentfulPaintP50>
			<firstContentfulPaintP75>1500</firstContentfulPaintP75>
			<firstContentfulPaintP90>3400</firstContentfulPaintP90>
			<firstContentfulPaintP95>5800</firstContentfulPaintP95>
			<firstContentfulPaintP99>15300</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>2182</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1200</domContentLoadedP50>
			<domContentLoadedP75>2200</domContentLoadedP75>
			<domContentLoadedP90>4400</domContentLoadedP90>
			<domContentLoadedP95>7500</domContentLoadedP95>
			<domContentLoadedP99>19100</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.025Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>4G</effectiveConnectionType>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1653</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>600</firstPaintP50>
			<firstPaintP75>1300</firstPaintP75>
			<firstPaintP90>3400</firstPaintP90>
			<firstPaintP95>6000</firstPaintP95>
			<firstPaintP99>19900</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1603</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>600</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3200</firstContentfulPaintP90>
			<firstContentfulPaintP95>5800</firstContentfulPaintP95>
			<firstContentfulPaintP99>19400</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2096</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1000</domContentLoadedP50>
			<domContentLoadedP75>1800</domContentLoadedP75>
			<domContentLoadedP90>3900</domContentLoadedP90>
			<domContentLoadedP95>6900</domContentLoadedP95>
			<domContentLoadedP99>21800</domContentLoadedP99>
			</item>
			</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /summary/ssl
`/summary/ssl` provides a breakdown of the aggregate metrics across all form factors and all network types, but broken down by whether SSL was enabled for the given `domain`.

### Example
https://api.crux.run/v1/summary/ssl/?domain=www.google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property                 | Type     | Description                                                                                                                                                                                              |
|--------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queryTime                | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                                                                                                       |
| date                     | datetime | The array of dates that correspond to this record. Date here is the last day of the queried month.                                                                                                       |
| origin                   | string   | The origin of this record.                                                                                                                                                                               |
| isSSL                    | boolean  | Was this record for https (true) or http (false) samples.                                                                                                                                                |
| pctSamples               | float    | The total % of samples in this aggregate (should sum to 1 within the month pivot).                                                                                                                       |
| firstPaintMAX            | integer  | The maximum observed First Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms.            |
| firstPaintWAVG           | integer  | The weighted average of observed First Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                              |
| firstPaintP25            | integer  | The 25th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                              |
| firstPaintP50            | integer  | The 50th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                              |
| firstPaintP75            | integer  | The 75th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                              |
| firstPaintP90            | integer  | The 90th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                              |
| firstPaintP95            | integer  | The 95th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                              |
| firstPaintP99            | integer  | The 99th percentile value for the First Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                              |
| firstContentfulPaintMAX  | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| firstContentfulPaintWAVG | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| firstContentfulPaintP25  | integer  | The 25th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                   |
| firstContentfulPaintP50  | integer  | The 50th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                   |
| firstContentfulPaintP75  | integer  | The 75th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                   |
| firstContentfulPaintP90  | integer  | The 90th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                   |
| firstContentfulPaintP95  | integer  | The 95th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                   |
| firstContentfulPaintP99  | integer  | The 99th percentile value for the First Contentful Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                   |
| domContentLoadedMAX      | integer  | The maximum observed First Contentful Paint starting bin (milliseconds) for this month. For example a value of 1000 means that the maximum observed first paint in the Property was between 1000-1100ms. |
| domContentLoadedWAVG     | integer  | The weighted average of observed First Contenful Paint starting bin (milliseconds) for this month. This is the density wavg binStart.                                                                    |
| domContentLoadedP25      | integer  | The 25th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.25).                                                               |
| domContentLoadedP50      | integer  | The 50th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.50).                                                               |
| domContentLoadedP75      | integer  | The 75th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.75).                                                               |
| domContentLoadedP90      | integer  | The 90th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.90).                                                               |
| domContentLoadedP95      | integer  | The 95th percentile value for the DOM Loaded metric starting bin for this month. This is the minimum starting bin where sum(density<=.95).                                                               |
| domContentLoadedP99      | integer  | The 99th percentile value for the DOM Loaded Paint metric starting bin for this month. This is the minimum starting bin where sum(density<=.99).                                                         |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.027Z",
		"date": [
			"2019-07-31T00:00:00.000Z",
			"2019-07-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z",
			"2019-08-31T00:00:00.000Z"
		],
		"origin": [
			"www.google.com",
			"www.google.com",
			"www.google.com",
			"www.google.com"
		],
		"effectiveConnectionType": [
			"3G",
			"4G",
			"3G",
			"4G"
		],
		"pctSamples": [
			0.19699999690055847,
			1,
			0.19869999587535858,
			1
		],
		"firstPaintMAX": [
			40000,
			120100,
			50000,
			120100
		],
		"firstPaintWAVG": [
			1435,
			1783,
			1613,
			1653
		],
		"firstPaintP25": [
			400,
			300,
			400,
			300
		],
		"firstPaintP50": [
			700,
			500,
			700,
			600
		],
		"firstPaintP75": [
		1400,
		1400,
		1500,
		1300
		],
		"firstPaintP90": [
		2900,
		3700,
		3400,
		3400
		],
		"firstPaintP95": [
		4900,
		6900,
		5800,
		6000
		],
		"firstPaintP99": [
		14200,
		22400,
		15500,
		19900
		],
		"firstContentfulPaintMAX": [
		40000,
		120100,
		50000,
		120100
		],
		"firstContentfulPaintWAVG": [
		1447,
		1732,
		1610,
		1603
		],
		"firstContentfulPaintP25": [
		400,
		300,
		400,
		300
		],
		"firstContentfulPaintP50": [
		700,
		500,
		800,
		600
		],
		"firstContentfulPaintP75": [
		1400,
		1300,
		1500,
		1300
		],
		"firstContentfulPaintP90": [
		2800,
		3600,
		3400,
		3200
		],
		"firstContentfulPaintP95": [
		4900,
		6700,
		5800,
		5800
		],
		"firstContentfulPaintP99": [
		14400,
		22200,
		15300,
		19400
		],
		"domContentLoadedMAX": [
		50000,
		120100,
		50000,
		120100
		],
		"domContentLoadedWAVG": [
		1961,
		2229,
		2182,
		2096
		],
		"domContentLoadedP25": [
		600,
		600,
		600,
		600
		],
		"domContentLoadedP50": [
		1200,
		1100,
		1200,
		1000
		],
		"domContentLoadedP75": [
		2000,
		1900,
		2200,
		1800
		],
		"domContentLoadedP90": [
		3900,
		4300,
		4400,
		3900
		],
		"domContentLoadedP95": [
		6200,
		7700,
		7500,
		6900
		],
		"domContentLoadedP99": [
		17300,
		22900,
		19100,
		21800
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.028Z</queryTime>
		<date>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-07-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
			<item>2019-08-31T00:00:00.000Z</item>
		</date>
		<origin>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
			<item>www.google.com</item>
		</origin>
		<effectiveConnectionType>
			<item>3G</item>
			<item>4G</item>
			<item>3G</item>
			<item>4G</item>
		</effectiveConnectionType>
		<pctSamples>
			<item>0.19699999690055847</item>
			<item>1</item>
			<item>0.19869999587535858</item>
			<item>1</item>
		</pctSamples>
		<firstPaintMAX>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
		</firstPaintMAX>
		<firstPaintWAVG>
			<item>1435</item>
			<item>1783</item>
			<item>1613</item>
			<item>1653</item>
		</firstPaintWAVG>
		<firstPaintP25>
			<item>400</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
		</firstPaintP25>
		<firstPaintP50>
			<item>700</item>
			<item>500</item>
			<item>700</item>
			<item>600</item>
			</firstPaintP50>
			<firstPaintP75>
			<item>1400</item>
			<item>1400</item>
			<item>1500</item>
			<item>1300</item>
			</firstPaintP75>
			<firstPaintP90>
			<item>2900</item>
			<item>3700</item>
			<item>3400</item>
			<item>3400</item>
			</firstPaintP90>
			<firstPaintP95>
			<item>4900</item>
			<item>6900</item>
			<item>5800</item>
			<item>6000</item>
			</firstPaintP95>
			<firstPaintP99>
			<item>14200</item>
			<item>22400</item>
			<item>15500</item>
			<item>19900</item>
			</firstPaintP99>
			<firstContentfulPaintMAX>
			<item>40000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
			</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>
			<item>1447</item>
			<item>1732</item>
			<item>1610</item>
			<item>1603</item>
			</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>
			<item>400</item>
			<item>300</item>
			<item>400</item>
			<item>300</item>
			</firstContentfulPaintP25>
			<firstContentfulPaintP50>
			<item>700</item>
			<item>500</item>
			<item>800</item>
			<item>600</item>
			</firstContentfulPaintP50>
			<firstContentfulPaintP75>
			<item>1400</item>
			<item>1300</item>
			<item>1500</item>
			<item>1300</item>
			</firstContentfulPaintP75>
			<firstContentfulPaintP90>
			<item>2800</item>
			<item>3600</item>
			<item>3400</item>
			<item>3200</item>
			</firstContentfulPaintP90>
			<firstContentfulPaintP95>
			<item>4900</item>
			<item>6700</item>
			<item>5800</item>
			<item>5800</item>
			</firstContentfulPaintP95>
			<firstContentfulPaintP99>
			<item>14400</item>
			<item>22200</item>
			<item>15300</item>
			<item>19400</item>
			</firstContentfulPaintP99>
			<domContentLoadedMAX>
			<item>50000</item>
			<item>120100</item>
			<item>50000</item>
			<item>120100</item>
			</domContentLoadedMAX>
			<domContentLoadedWAVG>
			<item>1961</item>
			<item>2229</item>
			<item>2182</item>
			<item>2096</item>
			</domContentLoadedWAVG>
			<domContentLoadedP25>
			<item>600</item>
			<item>600</item>
			<item>600</item>
			<item>600</item>
			</domContentLoadedP25>
			<domContentLoadedP50>
			<item>1200</item>
			<item>1100</item>
			<item>1200</item>
			<item>1000</item>
			</domContentLoadedP50>
			<domContentLoadedP75>
			<item>2000</item>
			<item>1900</item>
			<item>2200</item>
			<item>1800</item>
			</domContentLoadedP75>
			<domContentLoadedP90>
			<item>3900</item>
			<item>4300</item>
			<item>4400</item>
			<item>3900</item>
			</domContentLoadedP90>
			<domContentLoadedP95>
			<item>6200</item>
			<item>7700</item>
			<item>7500</item>
			<item>6900</item>
			</domContentLoadedP95>
			<domContentLoadedP99>
			<item>17300</item>
			<item>22900</item>
			<item>19100</item>
			<item>21800</item>
			</domContentLoadedP99>
			</data>
```

#### CSV
```csv
	queryTime,date,origin,effectiveConnectionType,pctSamples,firstPaintMAX,firstPaintWAVG,firstPaintP25,firstPaintP50,firstPaintP75,firstPaintP90,firstPaintP95,firstPaintP99,firstContentfulPaintMAX,firstContentfulPaintWAVG,firstContentfulPaintP25,firstContentfulPaintP50,firstContentfulPaintP75,firstContentfulPaintP90,firstContentfulPaintP95,firstContentfulPaintP99,domContentLoadedMAX,domContentLoadedWAVG,domContentLoadedP25,domContentLoadedP50,domContentLoadedP75,domContentLoadedP90,domContentLoadedP95,domContentLoadedP99
	946684800026,1564531200000,www.google.com,3G,0.19699999690055847,40000,1435,400,700,1400,2900,4900,14200,40000,1447,400,700,1400,2800,4900,14400,50000,1961,600,1200,2000,3900,6200,17300
	946684800026,1564531200000,www.google.com,4G,1,120100,1783,300,500,1400,3700,6900,22400,120100,1732,300,500,1300,3600,6700,22200,120100,2229,600,1100,1900,4300,7700,22900
	946684800026,1567209600000,www.google.com,3G,0.19869999587535858,50000,1613,400,700,1500,3400,5800,15500,50000,1610,400,800,1500,3400,5800,15300,50000,2182,600,1200,2200,4400,7500,19100
	946684800026,1567209600000,www.google.com,4G,1,120100,1653,300,600,1300,3400,6000,19900,120100,1603,300,600,1300,3200,5800,19400,120100,2096,600,1000,1800,3900,6900,21800
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.025Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"effectiveConnectionType": "3G",
			"pctSamples": 0.19699999690055847,
			"firstPaintMAX": 40000,
			"firstPaintWAVG": 1435,
			"firstPaintP25": 400,
			"firstPaintP50": 700,
			"firstPaintP75": 1400,
			"firstPaintP90": 2900,
			"firstPaintP95": 4900,
			"firstPaintP99": 14200,
			"firstContentfulPaintMAX": 40000,
			"firstContentfulPaintWAVG": 1447,
			"firstContentfulPaintP25": 400,
			"firstContentfulPaintP50": 700,
			"firstContentfulPaintP75": 1400,
			"firstContentfulPaintP90": 2800,
			"firstContentfulPaintP95": 4900,
			"firstContentfulPaintP99": 14400,
			"domContentLoadedMAX": 50000,
			"domContentLoadedWAVG": 1961,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1200,
			"domContentLoadedP75": 2000,
			"domContentLoadedP90": 3900,
			"domContentLoadedP95": 6200,
			"domContentLoadedP99": 17300
		},
		{
			"queryTime": "2000-01-01T00:00:00.025Z",
			"date": "2019-07-31T00:00:00.000Z",
			"origin": "www.google.com",
			"effectiveConnectionType": "4G",
			"pctSamples": 1,
			"firstPaintMAX": 120100,
			"firstPaintWAVG": 1783,
			"firstPaintP25": 300,
			"firstPaintP50": 500,
			"firstPaintP75": 1400,
			"firstPaintP90": 3700,
			"firstPaintP95": 6900,
			"firstPaintP99": 22400,
			"firstContentfulPaintMAX": 120100,
			"firstContentfulPaintWAVG": 1732,
			"firstContentfulPaintP25": 300,
			"firstContentfulPaintP50": 500,
			"firstContentfulPaintP75": 1300,
			"firstContentfulPaintP90": 3600,
			"firstContentfulPaintP95": 6700,
			"firstContentfulPaintP99": 22200,
			"domContentLoadedMAX": 120100,
			"domContentLoadedWAVG": 2229,
			"domContentLoadedP25": 600,
			"domContentLoadedP50": 1100,
			"domContentLoadedP75": 1900,
			"domContentLoadedP90": 4300,
			"domContentLoadedP95": 7700,
			"domContentLoadedP99": 22900
			},
			{
				"queryTime": "2000-01-01T00:00:00.025Z",
				"date": "2019-08-31T00:00:00.000Z",
				"origin": "www.google.com",
				"effectiveConnectionType": "3G",
				"pctSamples": 0.19869999587535858,
				"firstPaintMAX": 50000,
				"firstPaintWAVG": 1613,
				"firstPaintP25": 400,
				"firstPaintP50": 700,
				"firstPaintP75": 1500,
				"firstPaintP90": 3400,
				"firstPaintP95": 5800,
				"firstPaintP99": 15500,
				"firstContentfulPaintMAX": 50000,
				"firstContentfulPaintWAVG": 1610,
				"firstContentfulPaintP25": 400,
				"firstContentfulPaintP50": 800,
				"firstContentfulPaintP75": 1500,
				"firstContentfulPaintP90": 3400,
				"firstContentfulPaintP95": 5800,
				"firstContentfulPaintP99": 15300,
				"domContentLoadedMAX": 50000,
				"domContentLoadedWAVG": 2182,
				"domContentLoadedP25": 600,
				"domContentLoadedP50": 1200,
				"domContentLoadedP75": 2200,
				"domContentLoadedP90": 4400,
				"domContentLoadedP95": 7500,
				"domContentLoadedP99": 19100
				},
				{
					"queryTime": "2000-01-01T00:00:00.025Z",
					"date": "2019-08-31T00:00:00.000Z",
					"origin": "www.google.com",
					"effectiveConnectionType": "4G",
					"pctSamples": 1,
					"firstPaintMAX": 120100,
					"firstPaintWAVG": 1653,
					"firstPaintP25": 300,
					"firstPaintP50": 600,
					"firstPaintP75": 1300,
					"firstPaintP90": 3400,
					"firstPaintP95": 6000,
					"firstPaintP99": 19900,
					"firstContentfulPaintMAX": 120100,
					"firstContentfulPaintWAVG": 1603,
					"firstContentfulPaintP25": 300,
					"firstContentfulPaintP50": 600,
					"firstContentfulPaintP75": 1300,
					"firstContentfulPaintP90": 3200,
					"firstContentfulPaintP95": 5800,
					"firstContentfulPaintP99": 19400,
					"domContentLoadedMAX": 120100,
					"domContentLoadedWAVG": 2096,
					"domContentLoadedP25": 600,
					"domContentLoadedP50": 1000,
					"domContentLoadedP75": 1800,
					"domContentLoadedP90": 3900,
					"domContentLoadedP95": 6900,
					"domContentLoadedP99": 21800
				}
				]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>3G</effectiveConnectionType>
			<pctSamples>0.19699999690055847</pctSamples>
			<firstPaintMAX>40000</firstPaintMAX>
			<firstPaintWAVG>1435</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>700</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>2900</firstPaintP90>
			<firstPaintP95>4900</firstPaintP95>
			<firstPaintP99>14200</firstPaintP99>
			<firstContentfulPaintMAX>40000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1447</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>700</firstContentfulPaintP50>
			<firstContentfulPaintP75>1400</firstContentfulPaintP75>
			<firstContentfulPaintP90>2800</firstContentfulPaintP90>
			<firstContentfulPaintP95>4900</firstContentfulPaintP95>
			<firstContentfulPaintP99>14400</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>1961</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1200</domContentLoadedP50>
			<domContentLoadedP75>2000</domContentLoadedP75>
			<domContentLoadedP90>3900</domContentLoadedP90>
			<domContentLoadedP95>6200</domContentLoadedP95>
			<domContentLoadedP99>17300</domContentLoadedP99>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-07-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>4G</effectiveConnectionType>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1783</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>500</firstPaintP50>
			<firstPaintP75>1400</firstPaintP75>
			<firstPaintP90>3700</firstPaintP90>
			<firstPaintP95>6900</firstPaintP95>
			<firstPaintP99>22400</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1732</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>500</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3600</firstContentfulPaintP90>
			<firstContentfulPaintP95>6700</firstContentfulPaintP95>
			<firstContentfulPaintP99>22200</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2229</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1100</domContentLoadedP50>
			<domContentLoadedP75>1900</domContentLoadedP75>
			<domContentLoadedP90>4300</domContentLoadedP90>
			<domContentLoadedP95>7700</domContentLoadedP95>
			<domContentLoadedP99>22900</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>3G</effectiveConnectionType>
			<pctSamples>0.19869999587535858</pctSamples>
			<firstPaintMAX>50000</firstPaintMAX>
			<firstPaintWAVG>1613</firstPaintWAVG>
			<firstPaintP25>400</firstPaintP25>
			<firstPaintP50>700</firstPaintP50>
			<firstPaintP75>1500</firstPaintP75>
			<firstPaintP90>3400</firstPaintP90>
			<firstPaintP95>5800</firstPaintP95>
			<firstPaintP99>15500</firstPaintP99>
			<firstContentfulPaintMAX>50000</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1610</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>400</firstContentfulPaintP25>
			<firstContentfulPaintP50>800</firstContentfulPaintP50>
			<firstContentfulPaintP75>1500</firstContentfulPaintP75>
			<firstContentfulPaintP90>3400</firstContentfulPaintP90>
			<firstContentfulPaintP95>5800</firstContentfulPaintP95>
			<firstContentfulPaintP99>15300</firstContentfulPaintP99>
			<domContentLoadedMAX>50000</domContentLoadedMAX>
			<domContentLoadedWAVG>2182</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1200</domContentLoadedP50>
			<domContentLoadedP75>2200</domContentLoadedP75>
			<domContentLoadedP90>4400</domContentLoadedP90>
			<domContentLoadedP95>7500</domContentLoadedP95>
			<domContentLoadedP99>19100</domContentLoadedP99>
			</item>
			<item>
			<queryTime>2000-01-01T00:00:00.026Z</queryTime>
			<date>2019-08-31T00:00:00.000Z</date>
			<origin>www.google.com</origin>
			<effectiveConnectionType>4G</effectiveConnectionType>
			<pctSamples>1</pctSamples>
			<firstPaintMAX>120100</firstPaintMAX>
			<firstPaintWAVG>1653</firstPaintWAVG>
			<firstPaintP25>300</firstPaintP25>
			<firstPaintP50>600</firstPaintP50>
			<firstPaintP75>1300</firstPaintP75>
			<firstPaintP90>3400</firstPaintP90>
			<firstPaintP95>6000</firstPaintP95>
			<firstPaintP99>19900</firstPaintP99>
			<firstContentfulPaintMAX>120100</firstContentfulPaintMAX>
			<firstContentfulPaintWAVG>1603</firstContentfulPaintWAVG>
			<firstContentfulPaintP25>300</firstContentfulPaintP25>
			<firstContentfulPaintP50>600</firstContentfulPaintP50>
			<firstContentfulPaintP75>1300</firstContentfulPaintP75>
			<firstContentfulPaintP90>3200</firstContentfulPaintP90>
			<firstContentfulPaintP95>5800</firstContentfulPaintP95>
			<firstContentfulPaintP99>19400</firstContentfulPaintP99>
			<domContentLoadedMAX>120100</domContentLoadedMAX>
			<domContentLoadedWAVG>2096</domContentLoadedWAVG>
			<domContentLoadedP25>600</domContentLoadedP25>
			<domContentLoadedP50>1000</domContentLoadedP50>
			<domContentLoadedP75>1800</domContentLoadedP75>
			<domContentLoadedP90>3900</domContentLoadedP90>
			<domContentLoadedP95>6900</domContentLoadedP95>
			<domContentLoadedP99>21800</domContentLoadedP99>
			</item>
			</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /score/fp
`/summary/fp` provides a total percentage of visitors across all form factors and all network types, but broken down score of the First Paint for the given `domain`.

### Example
https://api.crux.run/v1/score/fp?domain=www.google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property  | Type     | Description                                                                                   |
|-----------|----------|-----------------------------------------------------------------------------------------------|
| queryTime | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).            |
| month     | datetime | The array of dates that correspond to this record. Month here is the first date of the month. |
| origin    | string   | The origin of this record.                                                                    |
| fast      | float    | The % of visitors (sum of density) that had a fast First Paint score of this record.          |
| average   | float    | The % of visitors (sum of density) that had a fast First Paint score of this record.          |
| slow      | float    | The % of visitors (sum of density) that had a fast First Paint score of this record.          |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.033Z",
		"month": [
			"2019-07-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z"
		],
		"fast": [
			0.668267548084259,
			0.6624149680137634
		],
		"average": [
			0.18410125374794006,
			0.19635353982448578
		],
		"slow": [
			0.1476311981678009,
			0.1412314921617508
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.033Z</queryTime>
		<month>
			<item>2019-07-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<fast>
			<item>0.668267548084259</item>
			<item>0.6624149680137634</item>
		</fast>
		<average>
			<item>0.18410125374794006</item>
			<item>0.19635353982448578</item>
		</average>
		<slow>
			<item>0.1476311981678009</item>
			<item>0.1412314921617508</item>
		</slow>
	</data>
```

#### CSV
```csv
	queryTime,month,fast,average,slow
	946684800032,1561939200000,0.668267548084259,0.18410125374794006,0.1476311981678009
	946684800032,1564617600000,0.6624149680137634,0.19635353982448578,0.1412314921617508
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.033Z",
			"month": "2019-07-01T00:00:00.000Z",
			"fast": 0.668267548084259,
			"average": 0.18410125374794006,
			"slow": 0.1476311981678009
		},
		{
			"queryTime": "2000-01-01T00:00:00.033Z",
			"month": "2019-08-01T00:00:00.000Z",
			"fast": 0.6624149680137634,
			"average": 0.19635353982448578,
			"slow": 0.1412314921617508
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.032Z</queryTime>
			<month>2019-07-01T00:00:00.000Z</month>
			<fast>0.668267548084259</fast>
			<average>0.18410125374794006</average>
			<slow>0.1476311981678009</slow>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.032Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<fast>0.6624149680137634</fast>
			<average>0.19635353982448578</average>
			<slow>0.1412314921617508</slow>
		</item>
	</data>
```

<!-- tabs:end -->

</p>
</details>






## `GET` /score/fcp
`/summary/fcp` provides a total percentage of visitors across all form factors and all network types, but broken down score of the First Contentful Paint for the given `domain`.

### Example
https://api.crux.run/v1/score/fcp?domain=www.google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property  | Type     | Description                                                                                     |
|-----------|----------|-------------------------------------------------------------------------------------------------|
| queryTime | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).              |
| month     | datetime | The array of dates that correspond to this record. Month here is the first date of the month.   |
| origin    | string   | The origin of this record.                                                                      |
| fast      | float    | The % of visitors (sum of density) that had a fast First Contentful Paint score of this record. |
| average   | float    | The % of visitors (sum of density) that had a fast First Contentful Paint score of this record. |
| slow      | float    | The % of visitors (sum of density) that had a fast First Contentful Paint score of this record. |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.035Z",
		"month": [
			"2019-07-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z"
		],
		"fast": [
			0.6773499846458435,
			0.6681833863258362
		],
		"average": [
			0.1829500049352646,
			0.19580979645252228
		],
		"slow": [
			0.1396999955177307,
			0.13600680232048035
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.033Z</queryTime>
		<month>
			<item>2019-07-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<fast>
			<item>0.6773499846458435</item>
			<item>0.6681833863258362</item>
		</fast>
		<average>
			<item>0.1829500049352646</item>
			<item>0.19580979645252228</item>
		</average>
		<slow>
			<item>0.1396999955177307</item>
			<item>0.13600680232048035</item>
		</slow>
	</data>
```

#### CSV
```csv
	queryTime,month,fast,average,slow
	946684800035,1561939200000,0.6773499846458435,0.1829500049352646,0.1396999955177307
	946684800035,1564617600000,0.6681833863258362,0.19580979645252228,0.13600680232048035
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.037Z",
			"month": "2019-07-01T00:00:00.000Z",
			"fast": 0.6773499846458435,
			"average": 0.1829500049352646,
			"slow": 0.1396999955177307
		},
		{
			"queryTime": "2000-01-01T00:00:00.037Z",
			"month": "2019-08-01T00:00:00.000Z",
			"fast": 0.6681833863258362,
			"average": 0.19580979645252228,
			"slow": 0.13600680232048035
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.033Z</queryTime>
			<month>2019-07-01T00:00:00.000Z</month>
			<fast>0.6773499846458435</fast>
			<average>0.1829500049352646</average>
			<slow>0.1396999955177307</slow>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.033Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<fast>0.6681833863258362</fast>
			<average>0.19580979645252228</average>
			<slow>0.13600680232048035</slow>
		</item>
	</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /score/dcl
`/summary/dcl` provides a total percentage of visitors across all form factors and all network types, but broken down score of the DOM Content Loaded for the given `domain`.

### Example
https://api.crux.run/v1/score/dcl?domain=www.google.com&months=2

### Response
A structure in your selected format with the following shape:

| Property  | Type     | Description                                                                                   |
|-----------|----------|-----------------------------------------------------------------------------------------------|
| queryTime | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).            |
| month     | datetime | The array of dates that correspond to this record. Month here is the first date of the month. |
| origin    | string   | The origin of this record.                                                                    |
| fast      | float    | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record.   |
| average   | float    | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record.   |
| slow      | float    | The % of visitors (sum of density) that had a fast DOM Content Loaded score of this record.   |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.033Z",
		"month": [
			"2019-07-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z"
		],
		"fast": [
			0.43102240562438965,
			0.4507528245449066
		],
		"average": [
			0.38312825560569763,
			0.375143826007843
		],
		"slow": [
			0.18584933876991272,
			0.17410334944725037
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.032Z</queryTime>
		<month>
			<item>2019-07-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<fast>
			<item>0.43102240562438965</item>
			<item>0.4507528245449066</item>
		</fast>
		<average>
			<item>0.38312825560569763</item>
			<item>0.375143826007843</item>
		</average>
		<slow>
			<item>0.18584933876991272</item>
			<item>0.17410334944725037</item>
		</slow>
	</data>
```

#### CSV
```csv
	queryTime,month,fast,average,slow
	946684800032,1561939200000,0.43102240562438965,0.38312825560569763,0.18584933876991272
	946684800032,1564617600000,0.4507528245449066,0.375143826007843,0.17410334944725037
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.033Z",
			"month": "2019-07-01T00:00:00.000Z",
			"fast": 0.43102240562438965,
			"average": 0.38312825560569763,
			"slow": 0.18584933876991272
		},
		{
			"queryTime": "2000-01-01T00:00:00.033Z",
			"month": "2019-08-01T00:00:00.000Z",
			"fast": 0.4507528245449066,
			"average": 0.375143826007843,
			"slow": 0.17410334944725037
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.033Z</queryTime>
			<month>2019-07-01T00:00:00.000Z</month>
			<fast>0.43102240562438965</fast>
			<average>0.38312825560569763</average>
			<slow>0.18584933876991272</slow>
		</item>
		<item>
			<queryTime>2000-01-01T00:00:00.033Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<fast>0.4507528245449066</fast>
			<average>0.375143826007843</average>
			<slow>0.17410334944725037</slow>
		</item>
	</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /histo/fp
`/histo/fp` the histogram of First Paint observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cumulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Example
https://api.crux.run/v1/histo/fp?domain=www.google.com&months=1

### Response
A structure in your selected format with the following shape:

| Property    | Type     | Description                                                                                                                    |
|-------------|----------|--------------------------------------------------------------------------------------------------------------------------------|
| monthCount  | int      | The number of months in this result set.                                                                                       |
| minBinStart | int      | The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value.        |
| maxBinStart | int      | The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value.        |
| minDensity  | float    | The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value. |
| maxDensity  | float    | The maximum observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value. |
| queryTime   | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                             |
| month       | string   | The array of months for these records.                                                                                         |
| binStart    | integer  | The array of starting bins for these records.                                                                                  |
| density     | float    | The array of density values for these records. The sum of density with month ~= .9901.                                         |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"monthCount": 1,
		"minBinStart": 0,
		"maxBinStart": 2800,
		"minDensity": 0.0022559843620399526,
		"maxDensity": 0.21746291903611203,
		"queryTime": "2000-01-01T00:00:00.022Z",
		"month": [
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z",
			"2019-08-01T00:00:00.000Z"
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
			2800
			],
			"density": [
			0.07167242515812375,
			0.21746291903611203,
			0.18991195405830189,
			0.10870649979748905,
			0.07321966752629808,
			0.04921240275630431,
			0.03588612086418247,
			0.029048293482857212,
			0.024755935530191755,
			0.020763042162934373,
			0.016570504499181234,
			0.01357583540340598,
			0.01222823361305628,
			0.010481343229715065,
			0.00883427499462173,
			0.007406816136441173,
			0.007406816136441173,
			0.007406816136441173,
			0.005769729744305513,
			0.005769729744305513,
			0.004531933339663037,
			0.004531933339663037,
			0.004531933339663037,
			0.0034139232340176817,
			0.0034139232340176817,
			0.0028149894148626307,
			0.0028149894148626307,
			0.0028149894148626307,
			0.0022559843620399526
			]
		}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<monthCount>1</monthCount>
		<minBinStart>0</minBinStart>
		<maxBinStart>2800</maxBinStart>
		<minDensity>0.0022559843620399526</minDensity>
		<maxDensity>0.21746291903611203</maxDensity>
		<queryTime>2000-01-01T00:00:00.023Z</queryTime>
		<month>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<binStart>
			<item>0</item>
			<item>100</item>
			<item>200</item>
			<item>300</item>
			<item>400</item>
			<item>500</item>
			<item>600</item>
			<item>700</item>
			<item>800</item>
			<item>900</item>
			<item>1000</item>
			<item>1100</item>
			<item>1200</item>
			<item>1300</item>
			<item>1400</item>
			<item>1500</item>
			<item>1600</item>
			<item>1700</item>
			<item>1800</item>
			<item>1900</item>
			<item>2000</item>
			<item>2100</item>
			<item>2200</item>
			<item>2300</item>
			<item>2400</item>
			<item>2500</item>
			<item>2600</item>
			<item>2700</item>
			<item>2800</item>
			</binStart>
			<density>
			<item>0.07167242515812375</item>
			<item>0.21746291903611203</item>
			<item>0.18991195405830189</item>
			<item>0.10870649979748905</item>
			<item>0.07321966752629808</item>
			<item>0.04921240275630431</item>
			<item>0.03588612086418247</item>
			<item>0.029048293482857212</item>
			<item>0.024755935530191755</item>
			<item>0.020763042162934373</item>
			<item>0.016570504499181234</item>
			<item>0.01357583540340598</item>
			<item>0.01222823361305628</item>
			<item>0.010481343229715065</item>
			<item>0.00883427499462173</item>
			<item>0.007406816136441173</item>
			<item>0.007406816136441173</item>
			<item>0.007406816136441173</item>
			<item>0.005769729744305513</item>
			<item>0.005769729744305513</item>
			<item>0.004531933339663037</item>
			<item>0.004531933339663037</item>
			<item>0.004531933339663037</item>
			<item>0.0034139232340176817</item>
			<item>0.0034139232340176817</item>
			<item>0.0028149894148626307</item>
			<item>0.0028149894148626307</item>
			<item>0.0028149894148626307</item>
			<item>0.0022559843620399526</item>
			</density>
			</data>
```

#### CSV
```csv
	monthCount,minBinStart,maxBinStart,minDensity,maxDensity,queryTime,month,binStart,density
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,0,0.07167242515812375
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,100,0.21746291903611203
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,200,0.18991195405830189
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,300,0.10870649979748905
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,400,0.07321966752629808
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,500,0.04921240275630431
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,600,0.03588612086418247
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,700,0.029048293482857212
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,800,0.024755935530191755
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,900,0.020763042162934373
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1000,0.016570504499181234
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1100,0.01357583540340598
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1200,0.01222823361305628
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1300,0.010481343229715065
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1400,0.00883427499462173
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1500,0.007406816136441173
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1600,0.007406816136441173
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1700,0.007406816136441173
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1800,0.005769729744305513
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,1900,0.005769729744305513
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2000,0.004531933339663037
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2100,0.004531933339663037
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2200,0.004531933339663037
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2300,0.0034139232340176817
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2400,0.0034139232340176817
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2500,0.0028149894148626307
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2600,0.0028149894148626307
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2700,0.0028149894148626307
	1,0,2800,0.0022559843620399526,0.21746291903611203,946684800023,1564617600000,2800,0.0022559843620399526
```

#### JSON Flipped
```json
	[
		{
			"monthCount": 1,
			"minBinStart": 0,
			"maxBinStart": 2800,
			"minDensity": 0.0022559843620399526,
			"maxDensity": 0.21746291903611203,
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"binStart": 0,
			"density": 0.07167242515812375
		},
		{
			"monthCount": 1,
			"minBinStart": 0,
			"maxBinStart": 2800,
			"minDensity": 0.0022559843620399526,
			"maxDensity": 0.21746291903611203,
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"binStart": 100,
			"density": 0.21746291903611203
		},
		{
			"monthCount": 1,
			"minBinStart": 0,
			"maxBinStart": 2800,
			"minDensity": 0.0022559843620399526,
			"maxDensity": 0.21746291903611203,
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"binStart": 200,
			"density": 0.18991195405830189
		},
		{
			"monthCount": 1,
			"minBinStart": 0,
			"maxBinStart": 2800,
			"minDensity": 0.0022559843620399526,
			"maxDensity": 0.21746291903611203,
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"binStart": 300,
			"density": 0.10870649979748905
		},
		{
			"monthCount": 1,
			"minBinStart": 0,
			"maxBinStart": 2800,
			"minDensity": 0.0022559843620399526,
			"maxDensity": 0.21746291903611203,
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"binStart": 400,
			"density": 0.07321966752629808
			},
			{
				"monthCount": 1,
				"minBinStart": 0,
				"maxBinStart": 2800,
				"minDensity": 0.0022559843620399526,
				"maxDensity": 0.21746291903611203,
				"queryTime": "2000-01-01T00:00:00.024Z",
				"month": "2019-08-01T00:00:00.000Z",
				"binStart": 500,
				"density": 0.04921240275630431
				},
				{
					"monthCount": 1,
					"minBinStart": 0,
					"maxBinStart": 2800,
					"minDensity": 0.0022559843620399526,
					"maxDensity": 0.21746291903611203,
					"queryTime": "2000-01-01T00:00:00.024Z",
					"month": "2019-08-01T00:00:00.000Z",
					"binStart": 600,
					"density": 0.03588612086418247
					},
					{
						"monthCount": 1,
						"minBinStart": 0,
						"maxBinStart": 2800,
						"minDensity": 0.0022559843620399526,
						"maxDensity": 0.21746291903611203,
						"queryTime": "2000-01-01T00:00:00.024Z",
						"month": "2019-08-01T00:00:00.000Z",
						"binStart": 700,
						"density": 0.029048293482857212
						},
						{
							"monthCount": 1,
							"minBinStart": 0,
							"maxBinStart": 2800,
							"minDensity": 0.0022559843620399526,
							"maxDensity": 0.21746291903611203,
							"queryTime": "2000-01-01T00:00:00.024Z",
							"month": "2019-08-01T00:00:00.000Z",
							"binStart": 800,
							"density": 0.024755935530191755
							},
							{
								"monthCount": 1,
								"minBinStart": 0,
								"maxBinStart": 2800,
								"minDensity": 0.0022559843620399526,
								"maxDensity": 0.21746291903611203,
								"queryTime": "2000-01-01T00:00:00.024Z",
								"month": "2019-08-01T00:00:00.000Z",
								"binStart": 900,
								"density": 0.020763042162934373
								},
								{
									"monthCount": 1,
									"minBinStart": 0,
									"maxBinStart": 2800,
									"minDensity": 0.0022559843620399526,
									"maxDensity": 0.21746291903611203,
									"queryTime": "2000-01-01T00:00:00.024Z",
									"month": "2019-08-01T00:00:00.000Z",
									"binStart": 1000,
									"density": 0.016570504499181234
									},
									{
										"monthCount": 1,
										"minBinStart": 0,
										"maxBinStart": 2800,
										"minDensity": 0.0022559843620399526,
										"maxDensity": 0.21746291903611203,
										"queryTime": "2000-01-01T00:00:00.024Z",
										"month": "2019-08-01T00:00:00.000Z",
										"binStart": 1100,
										"density": 0.01357583540340598
										},
										{
											"monthCount": 1,
											"minBinStart": 0,
											"maxBinStart": 2800,
											"minDensity": 0.0022559843620399526,
											"maxDensity": 0.21746291903611203,
											"queryTime": "2000-01-01T00:00:00.024Z",
											"month": "2019-08-01T00:00:00.000Z",
											"binStart": 1200,
											"density": 0.01222823361305628
											},
											{
												"monthCount": 1,
												"minBinStart": 0,
												"maxBinStart": 2800,
												"minDensity": 0.0022559843620399526,
												"maxDensity": 0.21746291903611203,
												"queryTime": "2000-01-01T00:00:00.024Z",
												"month": "2019-08-01T00:00:00.000Z",
												"binStart": 1300,
												"density": 0.010481343229715065
												},
												{
													"monthCount": 1,
													"minBinStart": 0,
													"maxBinStart": 2800,
													"minDensity": 0.0022559843620399526,
													"maxDensity": 0.21746291903611203,
													"queryTime": "2000-01-01T00:00:00.024Z",
													"month": "2019-08-01T00:00:00.000Z",
													"binStart": 1400,
													"density": 0.00883427499462173
													},
													{
														"monthCount": 1,
														"minBinStart": 0,
														"maxBinStart": 2800,
														"minDensity": 0.0022559843620399526,
														"maxDensity": 0.21746291903611203,
														"queryTime": "2000-01-01T00:00:00.024Z",
														"month": "2019-08-01T00:00:00.000Z",
														"binStart": 1500,
														"density": 0.007406816136441173
														},
														{
															"monthCount": 1,
															"minBinStart": 0,
															"maxBinStart": 2800,
															"minDensity": 0.0022559843620399526,
															"maxDensity": 0.21746291903611203,
															"queryTime": "2000-01-01T00:00:00.024Z",
															"month": "2019-08-01T00:00:00.000Z",
															"binStart": 1600,
															"density": 0.007406816136441173
															},
															{
																"monthCount": 1,
																"minBinStart": 0,
																"maxBinStart": 2800,
																"minDensity": 0.0022559843620399526,
																"maxDensity": 0.21746291903611203,
																"queryTime": "2000-01-01T00:00:00.024Z",
																"month": "2019-08-01T00:00:00.000Z",
																"binStart": 1700,
																"density": 0.007406816136441173
																},
																{
																	"monthCount": 1,
																	"minBinStart": 0,
																	"maxBinStart": 2800,
																	"minDensity": 0.0022559843620399526,
																	"maxDensity": 0.21746291903611203,
																	"queryTime": "2000-01-01T00:00:00.024Z",
																	"month": "2019-08-01T00:00:00.000Z",
																	"binStart": 1800,
																	"density": 0.005769729744305513
																	},
																	{
																		"monthCount": 1,
																		"minBinStart": 0,
																		"maxBinStart": 2800,
																		"minDensity": 0.0022559843620399526,
																		"maxDensity": 0.21746291903611203,
																		"queryTime": "2000-01-01T00:00:00.024Z",
																		"month": "2019-08-01T00:00:00.000Z",
																		"binStart": 1900,
																		"density": 0.005769729744305513
																		},
																		{
																			"monthCount": 1,
																			"minBinStart": 0,
																			"maxBinStart": 2800,
																			"minDensity": 0.0022559843620399526,
																			"maxDensity": 0.21746291903611203,
																			"queryTime": "2000-01-01T00:00:00.024Z",
																			"month": "2019-08-01T00:00:00.000Z",
																			"binStart": 2000,
																			"density": 0.004531933339663037
																			},
																			{
																				"monthCount": 1,
																				"minBinStart": 0,
																				"maxBinStart": 2800,
																				"minDensity": 0.0022559843620399526,
																				"maxDensity": 0.21746291903611203,
																				"queryTime": "2000-01-01T00:00:00.024Z",
																				"month": "2019-08-01T00:00:00.000Z",
																				"binStart": 2100,
																				"density": 0.004531933339663037
																				},
																				{
																					"monthCount": 1,
																					"minBinStart": 0,
																					"maxBinStart": 2800,
																					"minDensity": 0.0022559843620399526,
																					"maxDensity": 0.21746291903611203,
																					"queryTime": "2000-01-01T00:00:00.024Z",
																					"month": "2019-08-01T00:00:00.000Z",
																					"binStart": 2200,
																					"density": 0.004531933339663037
																					},
																					{
																						"monthCount": 1,
																						"minBinStart": 0,
																						"maxBinStart": 2800,
																						"minDensity": 0.0022559843620399526,
																						"maxDensity": 0.21746291903611203,
																						"queryTime": "2000-01-01T00:00:00.024Z",
																						"month": "2019-08-01T00:00:00.000Z",
																						"binStart": 2300,
																						"density": 0.0034139232340176817
																						},
																						{
																							"monthCount": 1,
																							"minBinStart": 0,
																							"maxBinStart": 2800,
																							"minDensity": 0.0022559843620399526,
																							"maxDensity": 0.21746291903611203,
																							"queryTime": "2000-01-01T00:00:00.024Z",
																							"month": "2019-08-01T00:00:00.000Z",
																							"binStart": 2400,
																							"density": 0.0034139232340176817
																							},
																							{
																								"monthCount": 1,
																								"minBinStart": 0,
																								"maxBinStart": 2800,
																								"minDensity": 0.0022559843620399526,
																								"maxDensity": 0.21746291903611203,
																								"queryTime": "2000-01-01T00:00:00.024Z",
																								"month": "2019-08-01T00:00:00.000Z",
																								"binStart": 2500,
																								"density": 0.0028149894148626307
																								},
																								{
																									"monthCount": 1,
																									"minBinStart": 0,
																									"maxBinStart": 2800,
																									"minDensity": 0.0022559843620399526,
																									"maxDensity": 0.21746291903611203,
																									"queryTime": "2000-01-01T00:00:00.024Z",
																									"month": "2019-08-01T00:00:00.000Z",
																									"binStart": 2600,
																									"density": 0.0028149894148626307
																									},
																									{
																										"monthCount": 1,
																										"minBinStart": 0,
																										"maxBinStart": 2800,
																										"minDensity": 0.0022559843620399526,
																										"maxDensity": 0.21746291903611203,
																										"queryTime": "2000-01-01T00:00:00.024Z",
																										"month": "2019-08-01T00:00:00.000Z",
																										"binStart": 2700,
																										"density": 0.0028149894148626307
																										},
																										{
																											"monthCount": 1,
																											"minBinStart": 0,
																											"maxBinStart": 2800,
																											"minDensity": 0.0022559843620399526,
																											"maxDensity": 0.21746291903611203,
																											"queryTime": "2000-01-01T00:00:00.024Z",
																											"month": "2019-08-01T00:00:00.000Z",
																											"binStart": 2800,
																											"density": 0.0022559843620399526
																										}
																										]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>0</binStart>
			<density>0.07167242515812375</density>
		</item>
		<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>100</binStart>
			<density>0.21746291903611203</density>
		</item>
		<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>200</binStart>
			<density>0.18991195405830189</density>
		</item>
		<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>300</binStart>
			<density>0.10870649979748905</density>
		</item>
		<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>400</binStart>
			<density>0.07321966752629808</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>500</binStart>
			<density>0.04921240275630431</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>600</binStart>
			<density>0.03588612086418247</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>700</binStart>
			<density>0.029048293482857212</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>800</binStart>
			<density>0.024755935530191755</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>900</binStart>
			<density>0.020763042162934373</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1000</binStart>
			<density>0.016570504499181234</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1100</binStart>
			<density>0.01357583540340598</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1200</binStart>
			<density>0.01222823361305628</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1300</binStart>
			<density>0.010481343229715065</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1400</binStart>
			<density>0.00883427499462173</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1500</binStart>
			<density>0.007406816136441173</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1600</binStart>
			<density>0.007406816136441173</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1700</binStart>
			<density>0.007406816136441173</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1800</binStart>
			<density>0.005769729744305513</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>1900</binStart>
			<density>0.005769729744305513</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2000</binStart>
			<density>0.004531933339663037</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2100</binStart>
			<density>0.004531933339663037</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2200</binStart>
			<density>0.004531933339663037</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2300</binStart>
			<density>0.0034139232340176817</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2400</binStart>
			<density>0.0034139232340176817</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2500</binStart>
			<density>0.0028149894148626307</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2600</binStart>
			<density>0.0028149894148626307</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2700</binStart>
			<density>0.0028149894148626307</density>
			</item>
			<item>
			<monthCount>1</monthCount>
			<minBinStart>0</minBinStart>
			<maxBinStart>2800</maxBinStart>
			<minDensity>0.0022559843620399526</minDensity>
			<maxDensity>0.21746291903611203</maxDensity>
			<queryTime>2000-01-01T00:00:00.023Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<binStart>2800</binStart>
			<density>0.0022559843620399526</density>
			</item>
			</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /histo/fcp
`/histo/fcp` the histogram of First Contentful Paint observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cumulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Example
https://api.crux.run/v1/histo/fcp?domain=www.google.com&months=1

### Response
A structure in your selected format with the following shape:

| Property    | Type     | Description                                                                                                                    |
|-------------|----------|--------------------------------------------------------------------------------------------------------------------------------|
| monthCount  | int      | The number of months in this result set.                                                                                       |
| minBinStart | int      | The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value.        |
| maxBinStart | int      | The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value.        |
| minDensity  | float    | The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value. |
| maxDensity  | float    | The maximum observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value. |
| queryTime   | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                             |
| month       | string   | The array of months for these records.                                                                                         |
| binStart    | integer  | The array of starting bins for these records.                                                                                  |
| density     | float    | The array of density values for these records. The sum of density with month ~= .9901.                                         |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.022Z",
		"month": [
			"2019-08-01T00:00:00.000Z"
		],
		"fast": [
			0.6681833863258362
		],
		"average": [
			0.19580979645252228
		],
		"slow": [
			0.13600680232048035
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.022Z</queryTime>
		<month>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<fast>
			<item>0.6681833863258362</item>
		</fast>
		<average>
			<item>0.19580979645252228</item>
		</average>
		<slow>
			<item>0.13600680232048035</item>
		</slow>
	</data>
```

#### CSV
```csv
	queryTime,month,fast,average,slow
	946684800022,1564617600000,0.6681833863258362,0.19580979645252228,0.13600680232048035
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.024Z",
			"month": "2019-08-01T00:00:00.000Z",
			"fast": 0.6681833863258362,
			"average": 0.19580979645252228,
			"slow": 0.13600680232048035
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.024Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<fast>0.6681833863258362</fast>
			<average>0.19580979645252228</average>
			<slow>0.13600680232048035</slow>
		</item>
	</data>
```

<!-- tabs:end -->

</p>
</details>







## `GET` /histo/dcl
`/histo/dcl` the histogram of DOM Content Loaded observations across all form factors and network connections. This histogram is summed up across all the default dimensions and represents the histogram of the site. We do limit the histogram length to the first element where the cumulative sum of the histogram is .9901 but we include the max for the observed samples (in the event they are not in the histogram itself).

### Example
https://api.crux.run/v1/histo/dcl?domain=www.google.com&months=1

### Response
A structure in your selected format with the following shape:

| Property    | Type     | Description                                                                                                                    |
|-------------|----------|--------------------------------------------------------------------------------------------------------------------------------|
| monthCount  | integer  | The number of months in this result set.                                                                                       |
| minBinStart | integer  | The minimum starting bin (in ms) for this result set. Were you to chart all records, this would be the minimum X value.        |
| maxBinStart | integer  | The maximum starting bin (in ms) for this result set. Were you to chart all records, this would be the maximum X value.        |
| minDensity  | float    | The minimum observed density in any bin for this result set. Were you to chart all records, this would be the minimum Y value. |
| maxDensity  | float    | The maximum observed density in any bin for this result set. Were you to chart all records, this would be the maximum Y value. |
| queryTime   | datetime | The amount of time this query took server side (ms since 2000-01-01T00:00:00.000).                                             |
| month       | string   | The array of months for these records.                                                                                         |
| binStart    | integer  | The array of starting bins for these records.                                                                                  |
| density     | float    | The array of density values for these records. The sum of density with month ~= .9901.                                         |


<details><summary>Click to Expand Results</summary><p>
<!-- tabs:start -->

#### JSON
```json
	{
		"queryTime": "2000-01-01T00:00:00.022Z",
		"month": [
			"2019-08-01T00:00:00.000Z"
		],
		"fast": [
			0.4507528245449066
		],
		"average": [
			0.375143826007843
		],
		"slow": [
			0.17410334944725037
		]
	}
```

#### XML
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<queryTime>2000-01-01T00:00:00.022Z</queryTime>
		<month>
			<item>2019-08-01T00:00:00.000Z</item>
		</month>
		<fast>
			<item>0.4507528245449066</item>
		</fast>
		<average>
			<item>0.375143826007843</item>
		</average>
		<slow>
			<item>0.17410334944725037</item>
		</slow>
	</data>
```

#### CSV
```csv
	queryTime,month,fast,average,slow
	946684800022,1564617600000,0.4507528245449066,0.375143826007843,0.17410334944725037
```

#### JSON Flipped
```json
	[
		{
			"queryTime": "2000-01-01T00:00:00.022Z",
			"month": "2019-08-01T00:00:00.000Z",
			"fast": 0.4507528245449066,
			"average": 0.375143826007843,
			"slow": 0.17410334944725037
		}
	]
```

#### XML Flipped
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<data>
		<item>
			<queryTime>2000-01-01T00:00:00.022Z</queryTime>
			<month>2019-08-01T00:00:00.000Z</month>
			<fast>0.4507528245449066</fast>
			<average>0.375143826007843</average>
			<slow>0.17410334944725037</slow>
		</item>
	</data>
```

<!-- tabs:end -->

</p>
</details>
