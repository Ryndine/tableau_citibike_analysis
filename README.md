# NYC Citibike Trip Analysis with Tableau

## Objective: 
The goal of this project is to gain insight into the NYC Citibike data for 2021.

## Tools & databases used:
- Jupyter
- Pandas
- [NYC Citibike Data](https://ride.citibikenyc.com/system-data)
- Tableau

## Tableau analysis:
[Dashboard Analysis](https://public.tableau.com/app/profile/ryan.m.7482/viz/2021-Citibike-Analysis/Dashboard1)  
[Story Analysis](https://public.tableau.com/app/profile/ryan.m.7482/viz/2021-Citibike-Analysis/Story1)  
Please visit above links to view the tableau project!

## Preparing my data:
 
**Collecting the data**

I'm downloading all the 2021 data for NYC citibike which is openly attainable to the public as a csv file.

**Cleaning with pandas:**

To start saw that the data in the start_at and ending_at columns were dates, so I formatted them to datetime.

After, I noticed they removed the "tripduration" column. So, to start I took "ending_at" and subtracted "started_at" and divided by panda's timedelta with a parameter of seconds. Then converting that column to datetime for better analysis parameters in Tableau.
```
raw_nycitibike_df['duration_seconds'] = (raw_nycitibike_df.ended_at - raw_nycitibike_df.started_at) / pd.Timedelta(seconds=1)
raw_nycitibike_df['duration_seconds'] = pd.to_datetime(raw_nycitibike_df['duration_seconds'], unit='s')
```
Final step was to trim the dataset. The original file contains 26,500,000 rows of data, but Tableau Public only allows 15,000,000. So I trimmed off what I needed to in order to load it into Tableau.

**Calculated field with tableau:**

I noticed the datetime was in military format, so within tableau I created a new calculated field to convert 24 hours to 12 hour time.
```
IF STR(DATEPART('hour',[Started At]))='12' THEN "12 PM"
ELSEIF STR(DATEPART('hour',[Started At]))='0' THEN "12 AM"
ELSEIF DATEPART('hour',[Started At])>12 THEN STR(DATEPART('hour',[Started At])-12)+" PM"
ELSE STR(DATEPART('hour',[Started At]))+" AM"
End
```
This produces an unsorted result (1AM, 1PM, 2AM...), so I had to go in and manually adjust the sorting so that it displayed data correctly.
