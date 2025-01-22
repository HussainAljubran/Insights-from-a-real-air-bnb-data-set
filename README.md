# insights-on-Airbnb-Manchester-
python analysis using calendar and listing dataset of Manchester 
## For the following analysis I have download the data from https://data.insideairbnb.com/united-kingdom/england/greater-manchester/2024-09-23/data/calendar.csv.gz
``` diff
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```
## 1 want to know the number of available and unavailable rooms
``` diff
calendar.available.value_counts()
```
<img width="360" alt="image" src="https://github.com/user-attachments/assets/abf39c86-5b3d-46da-a0c1-295c911aa600" />

f(false) means not available, t(true) means available 

## 2 purpose: calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) give proportion. Multiplaying by 100 converts the proportion into percentage


``` diff
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="439" alt="image" src="https://github.com/user-attachments/assets/1b037fef-de38-408d-b111-6ff580538c48" />

## 3 Let's Count the busiest day!
Hint We will be counting the most unavailable days (given by f)
``` diff
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="439" alt="image" src="https://github.com/user-attachments/assets/cff9c014-4e09-4095-ae53-541a9ce0a5c1" />

