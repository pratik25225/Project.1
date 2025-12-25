# Project.1
Mobile Data Usage Pattern Analysis
A Python-based statistical analysis tool that processes mobile network logs to identify usage patterns, peak consumption hours, and the correlation between session duration and data volume.
Project Overview
This project was developed as a part of a Statistics Minor to explore how different mobile applications consume data over time. Instead of looking at total usage alone, this script calculates the Consumption Rate (MB/min) to distinguish between "low-intensity" apps (like messaging) and "high-intensity" apps (like 4K streaming).

Key Features:
Feature Engineering: Derives consumption rates to normalize data usage.
Peak Detection: Automatically identifies the time and application responsible for the highest network load.
Correlation Analysis: Calculates the Pearson correlation coefficient to see how time spent relates to data consumed.
Visualization: Generates a trend line plot using Matplotlib.

Theoretical Framework
The script calculates:Consumption Rate: Rate = \frac{Usage (MB)}{Duration (min)}
Pearson Correlation (r): Measures the linear relationship between duration and usage.

Line by line Explanation

import pandas as pd

import matplotlib.pyplot as plt

df = pd.read_csv("/content/data_usage.csv.txt")

df

Lines 1-2: We import pandas for handling the table and pyplot for the graphing.
Line 4: We read the file. Pandas automatically parses the headers (Time, App, Duration, Usage).

df["Consumption Rate"] = df["Usage_MB"] / df["Duration_Min"]

df["data_usage"] = df["Duration_Min"] * df["Consumption Rate"]

Consumption Rate: This creates a new column by dividing total MB by minutes. This is vital because 500MB used in 5 minutes (Heavy Streaming) is very different from 500MB used in 5 hours (Messaging).
data_usage: Here, we recalculate the usage to ensure our derived rates align with the raw logs.

plt.figure()

plt.plot(df.index, df["data_usage"])

plt.xlabel('Entry Index')

plt.ylabel('Data Usage (MB)')

plt.title('Data Usage Profile')

plt.show()

plt.plot: We use the index (the sequence of events) on the X-axis and the usage on the Y-axis. This creates a "Usage Profile" that visually identifies spikes.

peak_index = df['Usage_MB'].idxmax()

peak_hour = df.loc[peak_index, 'Time_Start']

peak_app = df.loc[peak_index, 'App_Name']

idxmax(): This is a powerful pandas function that returns the index label of the maximum value in the column.
df.loc: Once we have the index, we "locate" the specific timestamp and application name associated with that peak.

correlation = df['Duration_Min'].corr(df['Usage_MB'])

print(f"Peak Consumption Hour: {peak_hour} (App: {peak_app})")

print(f"Correlation between Duration and Usage: {correlation:.4f}")

corr(): This computes the Pearson correlation.
If the result is 0.9, it means time and data are perfectly linked.
If it is 0.4 (as seen in our dataset), it suggests that the type of app (Category) is a more significant factor than just the amount of time spent.
