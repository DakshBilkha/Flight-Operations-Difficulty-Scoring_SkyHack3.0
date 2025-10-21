**Flight Operations Difficulty Scoring (FDS)**
This repository contains the project developed for the SkyHack 3.0, a 48-hour hackathon organized by United Airlines. The project's goal was to create a data-driven Flight Difficulty Score (FDS) to quantify the operational challenge posed by incoming and outgoing flights, enabling better ground time management.

**Project Overview**
Managing ground operations efficiently is critical for any airline. Delays, tight turnarounds, and complex passenger needs can create significant operational bottlenecks. This project addresses this challenge by developing a robust scoring system that predicts the difficulty of handling any given flight.

By analyzing historical data across multiple domains (flights, baggage, passengers), we trained a machine learning model to assign dynamic weights to key performance indicators (KPIs). The resulting Flight Difficulty Score (FDS) provides a single, actionable metric that allows ground staff to proactively allocate resources, anticipate challenges, and improve overall efficiency.

**Methodology & Pipeline**
The project follows a comprehensive data analysis and machine learning pipeline to generate the FDS.

**1. Data Ingestion and Aggregation**
We started by ingesting and merging data from five disparate CSV sources into a single, cohesive master dataset:

Flight Level Data.csv

Bag Level Data.csv

PNR Flight Level Data.csv

PNR Remark Level Data.csv

Airports Data.csv

**2. Data Cleaning and Preprocessing**
To ensure the quality of our analysis, the master dataset underwent several cleaning steps:

Outlier Removal: Using the Interquartile Range (IQR) method, we identified and removed major outliers in metrics like ground time and special service counts. This refined the dataset from an initial 8,099 flights down to a cleaner set of 6,053 flights.

Data Transformation: Datetime columns were split into separate Date and Time fields. Missing time values were imputed using the mode, and missing date values were handled using forward/backward fill to maintain data integrity.

**3. Feature Engineering & KPI Definition**
We defined and engineered five core KPIs to serve as inputs for our difficulty model:

Departure Delay: The difference in minutes between actual and scheduled departure.

Ground Time Inverse: A measure of time pressure, calculated from the minimum_turn_minutes. A shorter available time results in a higher score.

Total Bag Volume: The total number of checked and transfer bags for a flight.

Transfer Bag Ratio: The proportion of transfer bags to the total bag count, indicating logistical complexity.

Special Service Count: The total number of special service requests (e.g., wheelchairs), which require additional resources.

**4. FDS Model Development**
To avoid subjective weighting, we used a machine learning model to determine the importance of each KPI:

Model Selection: We chose a RandomForestRegressor for its ability to handle non-linear relationships and provide clear feature importances.

Training: The model was trained on the five normalized KPIs to predict a composite average of those same KPIs. This innovative approach forces the model to learn the relative contribution of each factor to the overall difficulty.

Weight Extraction: The resulting feature importances from the trained model (R² Score = 0.996) were used as the final data-driven weights for the FDS calculation.

**5. Scoring and Classification**
The final FDS for each flight was calculated as the weighted sum of its normalized KPIs. Flights were then ranked daily and classified as Difficult, Medium, or Easy based on their rank percentile.

Key Findings & Insights 💡
Our Exploratory Data Analysis (EDA) revealed several key operational trends:

Average Delay: On average, flights experience a departure delay of 12.28 minutes, with 47.5% of all flights departing later than scheduled.

Tight Scheduling: 7.93% of flights have a scheduled ground time that is within 5 minutes of (or below) the required minimum turn time, indicating a high risk of cascading delays.

Confounding Factors: While flights with high special service requests often have high delays, our analysis showed this is a confounding effect. Passenger load is the true primary driver of delay, and special requests simply increase with aircraft size. After controlling for load, the correlation between special services and delay becomes statistically insignificant (p > 0.05).

Difficult Destinations: Destinations like FAI (Fairbanks), ANC (Anchorage), and ORF (Norfolk) consistently ranked as the most difficult, showing high average FDS values even with varying flight volumes.

**Operational Recommendations **
Based on the FDS and EDA, we recommend the following actions:

Proactive Resource Deployment: Use the daily FDS rankings to pre-stage staff, supervisors, and equipment for flights classified as 'Difficult' before they arrive.

Strategic Time Buffering: For flights that consistently receive high FDS scores, consider adding a strategic time buffer to their schedules to absorb potential delays and prevent knock-on effects.

Targeted Workload Management: At hubs with a high volume of difficult flights (like FAI), deploy dedicated teams to handle high-complexity tasks like transfer bags and special service requests.

**Tech Stack & Libraries**
Python

Pandas & Numpy for data manipulation and analysis.

Scikit-learn for machine learning (RandomForestRegressor).

Matplotlib & Seaborn for data visualization.

Statsmodels for regression analysis.
