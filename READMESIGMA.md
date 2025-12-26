Newark International Airport Dashboard (Sigma Computing)

This dashboard was built in Sigma Computing to evaluate Newark International Airport’s current operational capabilities and highlight key administrative metrics. The analysis uses multiple CSV datasets and presents insights across airport connectivity, delays, airline operations, and aircraft composition.

Data Sources (CSV)

The dashboard is powered by the following CSV files:
	•	nyc_airlines.csv
	•	nyc_airports.csv
	•	nyc_flights.csv
	•	nyc_planes.csv
	•	nyc_weather.csv

Download / Access

All datasets can be downloaded as CSV and loaded directly into Sigma (via Upload, Cloud Storage, or a connected warehouse).
Place them in your preferred location and map each file to a Sigma table/model.

Dashboard Layout

1) Overall Panel

1. Distinct destinations connected to the airport
	•	Result: 113K for EWR
	•	Definition: Count of unique destination airport codes (dest) for flights originating from Newark (EWR).

2. Rounded average of distinct destinations per day
	•	Result: 15 for each destination
	•	Definition: For each calendar day, count distinct destinations; then average across days and round.

3. Average number of destinations per day of the week
	•	Definition: Group daily distinct destinations by day-of-week (Mon–Sun) and compute the mean.

4. Month with highest number of Flights
   • Result: 24.3K in August

5. Month with the highest accumulated departure delay
	•	Result: July with 541K Minutes Combined
	•	Definition: Sum total departure delay (dep_delay) by month and identify the maximum.

6. Correlation between departure delay and weather conditions
	•	Method (Sigma approach):
	•	Join nyc_flights.csv to nyc_weather.csv using shared time keys (typically year, month, day, hour and airport/origin).
	•	Compare dep_delay against weather fields such as:
	•	precipitation (precip)
	•	visibility (visib)
	•	wind speed / gust (wind_speed, wind_gust)
	•	pressure (pressure)
	•	temperature / humidity (temp, humid)
	•	Use Sigma correlation (or calculated Pearson correlation) and supporting visuals (scatterplots / trend charts).

7. Ranking of airlines by number of flights
	•	Result: UA/United Air Lines is the N1 with 55.8K Flights , N2 EV/ExpressJetAirlines with 54.1K , N3 B6 Jet Blue Airways with 50.2K.
	•	Definition: To determine which airlines operate the highest number of flights, the analysis ranks airlines by their total flight count.

This metric is calculated by:
	•	Using nyc_flights_fixed.csv as the fact table containing individual flight records
	•	Joining nyc_airlines.csv to nyc_flights_fixed.csv on the carrier code
	•	Aggregating the total number of flights per airline
	•	Sorting airlines in descending order by flight count

The airline with the highest aggregated flight count is ranked first.

Purpose:
This ranking highlights the airlines with the greatest operational presence at Newark International Airport and helps quantify airline-level traffic volume.

9. Ranking of airlines by daily availability of seats
	•	Join flights to planes using tailnum
	•	Use seats from nyc_planes.csv
	•	Aggregate total seats by carrier per day and compute average daily seats
	•	Rank carriers by average daily available seats

9. Most common plane manufacturer
	•	Definition: Join flights to planes by tailnum, then count most frequent manufacturer.

10. Most common plane model
	•	Definition: Join flights to planes by tailnum, then count most frequent model.

Notes on Implementation in Sigma
	•	Data Modeling: Each CSV is loaded as a Sigma table, with joins configured between Flights ↔ Weather, Flights ↔ Planes, and Flights ↔ Airlines.
	•	Filters: Dashboard includes filters for date range and (if available) origin airport = EWR.
	•	Visualizations: KPI tiles for headline metrics, bar charts for rankings, and time series for monthly trends.
	•	Reproducibility: Any user can reproduce the analysis by loading the same CSV files into Sigma and applying the metric definitions above.
