# NYC Traffic Collision Analysis: Identifying High Risk Time, Locations, Causes and Human Impact

## Table of Contents
- [Opening Hook](#opening-hook)
- [Project Overview](#project-overview)
- [Business Problem and Questions](#business-problem-and-questions)
- [Tools and Skills](#tools-and-skills)
- [Dataset Description](#dataset-description)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Data Model](#data-model)
- [Dashboard Pages](#dashboard-pages)
- [Key Insights and Findings](#key-insights-and-findings)
- [Summary and Conclusion](#summary-and-conclusion)
- [Recommendations](#recommendations)
- [How to Explore This Dashboard](#how-to-explore-this-dashboard)
- [Live Dashboard Link](#live-dashboard-link)
- [Author and Contact](#author-and-contact)

## Opening Hook

238,000 collisions. One clear pattern stands out: the most common cause of accidents is not the most common cause of deaths. This dashboard separates how often accidents happen from how deadly they are, so the city can focus safety efforts where they matter most.

## Project Overview

This project looks at motor vehicle collision data reported by the NYPD. The goal was to answer four specific questions from the project brief, and then go further by finding an extra insight on my own. The result is a three page Power BI dashboard built to support road safety decisions.

One thing worth mentioning upfront: the brief said the data covered January to August 2020. When I explored the data myself, it actually covered January 2021 to April 2023. I used the real dates found in the data instead of the dates stated in the brief, and I am reporting that clearly here so there is no confusion.

**[Figure 1: Project brief screenshot]**

## Business Problem and Questions

The brief asked for answers to four questions, plus any extra insight I could find on my own:

1. Compare the percentage of total accidents by month. Is there a seasonal pattern?
2. Break down accident frequency by day of the week and hour of the day. When do accidents happen most?
3. Which street had the most accidents, and what percentage of all accidents does that represent?
4. What was the most common cause of accidents overall, and what was the most common cause specifically in fatal accidents?

## Tools and Skills

**Tools used:** Power BI Desktop, Power Query, DAX, Power BI Service

**Skills used in this project:**
- Organizing an unfamiliar dataset into clear groups before starting any analysis
- Checking data quality and deciding how to handle missing values, with clear reasoning for each decision
- Noticing and reporting a mismatch between the brief and the real data, instead of ignoring it
- Writing DAX formulas that stay accurate even when filters or slicers are applied
- Building a date table in Power Query to support time based analysis
- Working with map visuals to show accident locations
- Comparing frequency and severity as two different ways of measuring risk
- Designing a multi page dashboard where each page has its own clear findings

## Dataset Description

Each row in the dataset represents one collision. The columns fall into five groups:

| Group | Columns |
|---|---|
| Identity | Collision ID |
| Time | Date, Time |
| Location | Borough, Street Name, Cross Street, Latitude, Longitude |
| Cause | Contributing Factor, Vehicle Type |
| Outcome | Persons Injured, Persons Killed, Pedestrians Injured, Pedestrians Killed, Cyclists Injured, Cyclists Killed, Motorists Injured, Motorists Killed |

**[Figure 2: Raw dataset preview, showing column headers and a sample of rows]**

## Data Cleaning and Transformation

Here is what I found in each column, and what I decided to do about it:

| Column | What I found | What I did |
|---|---|---|
| Collision ID | No issues | Removed duplicate entries |
| Date and Time | Correct format | Used the real date range found in the data (2021 to 2023) |
| Borough | Some blank entries | Kept them in the data, only left them out when a chart was specifically about boroughs |
| Street Name and Cross Street | Blank together in the same rows | Kept them in the data, only left them out when finding the top street |
| Latitude and Longitude | About 8 percent blank | Kept them in the data, the map simply skips rows with no coordinates |
| Contributing Factor | About 1 percent blank | Kept in the data, only left out when ranking the top causes |
| Vehicle Type | No blanks, but many say "Not Reported" | Kept "Not Reported" as its own valid answer |
| Injury and death columns | No missing values | No changes needed |

My general rule throughout this project: never delete or replace missing values. Only leave them out of a specific chart or calculation when they genuinely cannot answer that specific question.

**[Figure 3: Power Query step showing how blank values were handled, cropped to the specific step rather than the full Applied Steps pane]**

## Data Model

I built a separate date table in Power Query with Year, Quarter, Month, Month Number, Day Name, Day Number, Week of Year, and Hour. This made it possible to break time down in different ways without repeating work.

For the "top" style measures, like the busiest month or the most common street, I made sure the formulas would still give the correct answer even if someone filters the dashboard by year, borough, or vehicle type.

**[Figure 4: Data model view, showing table relationships]**

## Dashboard Pages

### Page 1: When Do Collisions Happen?

**[Figure 5: Page 1 screenshot, full page]**

**Question 1: Compare the percentage of total accidents by month. Is there a seasonal pattern?**

March had the highest share of accidents, at 10.53 percent of the total. After March, the numbers slowly go down through the rest of the year, with November and December being the quietest months, at around 7 percent each. So yes, there is a mild seasonal pattern, but it is a gradual decline rather than a sharp spike.

**Question 2: Break down accident frequency by day of the week and hour of the day. When do accidents happen most?**

Friday is the busiest day, with 37,000 accidents. Looking at the hour of the day, accidents build up through the morning, and peak at 4 PM, which lines up with evening rush hour.

### Page 2: Where and Why Do Collisions Happen?

**[Figure 6: Page 2 screenshot, full page]**

**Question 3: Which street had the most accidents, and what percentage of all accidents does that represent?**

Belt Parkway has the most accidents of any street, making up 1.56 percent of all 238,000 accidents in the dataset. The next closest street, Broadway, has fewer.

**Question 4: What was the most common cause of accidents overall, and what was the most common cause specifically in fatal accidents?**

Overall, the most common cause is driver inattention or distraction, responsible for 58,000 accidents. But when you look only at accidents where someone died, the picture changes. Unsafe speed and unspecified causes become the top reasons, and driver inattention drops down to third place. This is one of the most important findings in the whole project: the biggest cause of accidents is not the biggest cause of deaths.

### Page 3: How Severe Are These Collisions? (My Own Insight)

**[Figure 7: Page 3 screenshot, full page]**

This page was not asked for directly in the brief. I built it to answer a question I thought mattered: out of all these accidents, who is actually most at risk of dying, and where?

Out of 238,000 accidents, 116,000 led to an injury, but only 635 led to a death. That means most accidents in this data are survivable, but a small number are far deadlier than the rest.

The most surprising finding is about motorcycles. Motorcycles have a death rate of 3.09 percent, which is more than eleven times higher than the average across all vehicle types, and about three times higher than the next most dangerous vehicle type. Motorcycle riders face a level of risk that is completely different from everyone else on the road.

Looking at boroughs, Staten Island has the highest death rate of any borough, even though it almost certainly has far fewer total accidents than busier boroughs like Manhattan or Brooklyn. This means a crash on Staten Island is more likely to be fatal than a crash somewhere with much more traffic.

## Key Insights and Findings

The same idea shows up again and again across all three pages: how often something happens is not the same as how dangerous it is. The most common cause of accidents is not the most common cause of deaths. The vehicle type involved in the fewest accidents, the motorcycle, has by far the highest death rate. The borough with likely fewer total accidents has the highest fatality rate. Every time, looking past the raw numbers changes what the real priority should be.

## Summary and Conclusion

This project answers the four questions the brief asked, and then goes one step further. The time and location pages show where and when most accidents happen. But the severity page, which I built on my own initiative, shows that the accidents happening most often are not the ones causing the most harm. A safety plan built only on accident counts would miss motorcyclists and Staten Island entirely, even though both groups face much higher real world risk.

## Recommendations

- Increase enforcement on Fridays between 3 PM and 6 PM, and run safety campaigns before March each year, when accidents are highest
- Prioritize Belt Parkway for road safety improvements, and use different safety messages for speeding versus distracted driving, since they lead to different outcomes
- Treat motorcycle safety as its own priority rather than grouping it with general vehicle safety
- Look further into why Staten Island has a higher death rate than busier boroughs, since this cannot be explained by traffic volume alone

## How to Explore This Dashboard

Each page has slicers at the top that let you filter by year, borough, month, or vehicle type, depending on the page. Use these to check whether a finding still holds true for a specific year or a specific borough, rather than just looking at the citywide totals shown by default.

## Live Dashboard Link

[Insert Power BI published link, or a note that the dashboard is available as an image or PDF in this repository]

## Author and Contact

[Your name]
Data Analyst, Digitaley Drive Data Analytics Bootcamp
[LinkedIn] | [Email or contact]
