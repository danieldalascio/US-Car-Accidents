# Data Folder

This folder contains the dataset used in the project.

## Included files
- **US_accidents_dataset.parquet**  
  Main dataset used for all analyses and models.  
  It contains U.S. traffic accident records with timestamps, weather information, location data and various contextual features.

## Dataset Schema

The CSV file contains **47 columns**. The full attribute descriptions are listed below:

### Identification and Target

| # | Column | Description |
|---|--------|-------------|
| 1 | `ID` | Unique identifier for the accident record. |
| 2 | `Severity` | **Target variable.** Accident severity on a scale of 1–4: `1` = minimal traffic impact, `4` = significant traffic impact (long delay). |

### Time

| # | Column | Description |
|---|--------|-------------|
| 3 | `Start_Time` | Start date and time of the accident (local timezone). |
| 4 | `End_Time` | Date and time when the traffic impact was cleared (local timezone). |

### Geographic Location

| # | Column | Description |
|---|--------|-------------|
| 5 | `Start_Lat` | GPS latitude of the start point. |
| 6 | `Start_Lng` | GPS longitude of the start point. |
| 7 | `End_Lat` | GPS latitude of the end point. |
| 8 | `End_Lng` | GPS longitude of the end point. |
| 9 | `Distance(mi)` | Length of the road segment affected by the accident (miles). |
| 11 | `Number` | Street number in the address. |
| 12 | `Street` | Street name. |
| 13 | `Side` | Relative side of the street (Right/Left). |
| 14 | `City` | City. |
| 15 | `County` | County. |
| 16 | `State` | State. |
| 17 | `Zipcode` | ZIP code. |
| 18 | `Country` | Country. |
| 19 | `Timezone` | Timezone based on accident location (Eastern, Central, Mountain, Pacific). |
| 20 | `Airport_Code` | Code of the nearest airport-based weather station. |

### Weather Conditions

| # | Column | Description |
|---|--------|-------------|
| 21 | `Weather_Timestamp` | Timestamp of the weather observation associated with the accident. |
| 22 | `Temperature(F)` | Temperature in Fahrenheit. |
| 23 | `Wind_Chill(F)` | Perceived temperature (wind chill) in Fahrenheit. |
| 24 | `Humidity(%)` | Relative humidity (%). |
| 25 | `Pressure(in)` | Air pressure (inches). |
| 26 | `Visibility(mi)` | Visibility in miles. |
| 27 | `Wind_Direction` | Wind direction. |
| 28 | `Wind_Speed(mph)` | Wind speed (mph). |
| 29 | `Precipitation(in)` | Precipitation amount in inches, if any. |
| 30 | `Weather_Condition` | General weather condition (rain, snow, fog, thunderstorm, etc.). |

### Points of Interest (POI) — Road Infrastructure

All of the following columns are boolean and indicate the **presence of a nearby infrastructure element** at the accident location.

| # | Column | Description |
|---|--------|-------------|
| 31 | `Amenity` | Presence of a nearby amenity. |
| 32 | `Bump` | Presence of a speed bump or hump. |
| 33 | `Crossing` | Presence of a pedestrian crossing. |
| 34 | `Give_Way` | Presence of a give-way sign. |
| 35 | `Junction` | Presence of a junction. |
| 36 | `No_Exit` | Presence of a no-exit sign. |
| 37 | `Railway` | Presence of a railway. |
| 38 | `Roundabout` | Presence of a roundabout. |
| 39 | `Station` | Presence of a station (bus, train, etc.). |
| 40 | `Stop` | Presence of a stop sign. |
| 41 | `Traffic_Calming` | Presence of traffic calming measures. |
| 42 | `Traffic_Signal` | Presence of a traffic signal. |
| 43 | `Turning_Loop` | Presence of a turning loop. |

### Light Conditions

| # | Column | Description |
|---|--------|-------------|
| 44 | `Sunrise_Sunset` | Period of day (Day/Night) based on sunrise/sunset. |
| 45 | `Civil_Twilight` | Period of day based on civil twilight. |
| 46 | `Nautical_Twilight` | Period of day based on nautical twilight. |
| 47 | `Astronomical_Twilight` | Period of day based on astronomical twilight. |

### Free Text

| # | Column | Description |
|---|--------|-------------|
| 10 | `Description` | Natural language description of the accident. |


The version here is a **local copy**.
