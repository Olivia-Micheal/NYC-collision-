# NYC Traffic Collision Analysis: Identifying High Risk Time, Locations, Causes and Human Impact

A Power BI dashboard that analyzes motor vehicle collision data reported by the NYPD, built to uncover when, where, and why accidents happen in New York City, and how severe they really are.


238,000 collisions. One clear pattern stands out: the most common cause of accidents is not the most common cause of deaths. This dashboard separates how often accidents happen from how deadly they are, so the city can focus safety efforts where they matter most.

## Table of Contents
- [Project Overview](#project-overview)
- [Business Problem and Questions](#business-problem-and-questions)
- [Tools and Skills](#tools-and-skills)
- [Dataset Description](#dataset-description)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Data Model](#data-model)
- [DAX Measures and Design Decisions](#dax-measures-and-design-decisions)
- [Dashboard Pages](#dashboard-pages)
- [Key Insights and Findings](#key-insights-and-findings)
- [Summary and Conclusion](#summary-and-conclusion)
- [Recommendations](#recommendations)
- [How to Explore This Dashboard](#how-to-explore-this-dashboard)
- [Live Dashboard Link](#live-dashboard-link)
- [Author and Contact](#author-and-contact)

## Project Overview

This project analyzes motor vehicle collision data reported by the NYPD. I answered the four questions set out in the project brief, then went further and built an entire extra page of my own to expose a risk pattern the brief never asked about. The result is a three page Power BI dashboard built to support real road safety decisions.

The brief stated the data covered January to August 2020. I checked the data myself and found it actually covers January 2021 to April 2023. I used the real dates, not the ones written in the brief, and I am stating that clearly here so there is no confusion about which range this analysis is based on.

<img width="743" height="1280" alt="photo_2026-09-15_13-22-32" src="https://github.com/user-attachments/assets/9a902dd6-1201-4862-8453-6a38d913bbd4" />


*Figure 1: Project Brief*

## Business Problem and Questions

The brief required answers to four questions, plus original insight beyond them:

1. Compare the percentage of total accidents by month. Is there a seasonal pattern?
2. Break down accident frequency by day of the week and hour of the day. When do accidents happen most?
3. Which street had the most accidents, and what percentage of all accidents does that represent?
4. What was the most common cause of accidents overall, and what was the most common cause specifically in fatal accidents?

## Tools and Skills

**Tools used:** Power BI Desktop, Power Query, DAX, Power BI Service

**Skills demonstrated in this project:**
- Structuring an unfamiliar dataset into clear groups before writing a single formula
- Auditing data quality and making a defensible decision for every missing value, instead of deleting or guessing
- Catching a mismatch between the brief and the real data, and reporting it instead of hiding it
- Writing DAX ranking formulas that stay correct under any slicer or filter, not just the default view
- Building a proper date table in Power Query to support time based analysis
- Using map visuals to expose geographic risk
- Separating frequency from severity as two different measures of risk
- Designing a three page dashboard where every page has a clear, evidenced conclusion

## Dataset Description

Each row in the dataset represents one collision. I grouped the columns into five categories before starting any analysis:

| Group | Columns |
|---|---|
| Identity | Collision ID |
| Time | Date, Time |
| Location | Borough, Street Name, Cross Street, Latitude, Longitude |
| Cause | Contributing Factor, Vehicle Type |
| Outcome | Persons Injured, Persons Killed, Pedestrians Injured, Pedestrians Killed, Cyclists Injured, Cyclists Killed, Motorists Injured, Motorists Killed |

<img width="1888" height="723" alt="nyc collision raw dataset" src="https://github.com/user-attachments/assets/36f028ea-dca0-4a2c-bc04-c858d55e3f16" />


*Figure 2: Raw Dataset*

## Data Cleaning and Transformation

Here is what I found in each column, and the decision I made for each one:

| Column | What I found | What I did |
|---|---|---|
| Collision ID | No issues | Removed duplicate entries |
| Date and Time | Correct format | Used the real date range found in the data (2021 to 2023) |
| Borough | Some blank entries | Kept them in the data, only excluded them from borough specific charts |
| Street Name and Cross Street | Blank together in the same rows | Kept them in the data, only excluded them when finding the top street |
| Latitude and Longitude | About 8 percent blank | Kept them in the data, the map simply skips rows with no coordinates |
| Contributing Factor | About 1 percent blank | Kept in the data, only excluded from ranking the top causes |
| Vehicle Type | No blanks, but many say "Not Reported" | Kept "Not Reported" as its own valid answer |
| Injury and death columns | No missing values | No changes needed |

I followed one rule for the entire project: never delete or replace a missing value. Deleting a row throws away real information the rest of the analysis still needs. I only excluded a blank from a specific chart or measure when that chart genuinely could not use it, such as a blank street name never being able to win "top street."

<img width="1892" height="997" alt="nyccollision power query" src="https://github.com/user-attachments/assets/e5de794b-be26-4eb5-817d-42d157de5e30" />

*Figure 3: Data Cleaning*

## Data Model

I built a dedicated date table in Power Query, separate from the collision data itself, with Year, Quarter, Month, Month Number, Day Name, Day Number, Week of Year, and Hour. This let me break time down in different ways without repeating logic across multiple formulas.
<img width="1735" height="997" alt="nyc data model" src="https://github.com/user-attachments/assets/5910e95b-25c7-4ad0-ab6a-6a66912e6c44" />


*Figure 4: Data Model*

## DAX Measures and Design Decisions

My first version of the ranking measures, such as peak month and top street, used a function called `FIRSTNONBLANK`. I tested it and found a real flaw: it can return a category that simply appears first in the data, not the one that actually has the highest count. It also breaks the moment a slicer is applied, since it stops checking the true maximum for the current filter.

I rebuilt every ranking measure using a different pattern, one that builds a small summary table in memory, finds the real highest value inside it, and returns the matching category. This version stays accurate no matter what slicer someone applies, whether that is year, borough, or vehicle type. I also made sure blank categories are excluded only inside these ranking formulas, never removed from the dataset itself, so the same measure that ranks contributing factors still respects every other row that has a blank contributing factor.

This decision matters because a dashboard that gives the wrong answer the moment someone touches a filter is worse than no dashboard at all. Getting this right was a deliberate technical choice, not a default.

## Dashboard Pages

### Page 1: Time Analysis

**Questions answered:**
1. Compare the percentage of total accidents by month. Is there a seasonal pattern?
2. Break down accident frequency by day of the week and hour of the day. When do accidents happen most?
   
<img width="1277" height="687" alt="Nyc collision page1" src="https://github.com/user-attachments/assets/c10d7a68-0954-4d62-b119-30b85294ec2b" />

*Figure 5: Time Analysis Page*

Look at the bar chart at the top of this page. Each bar is one month, and the tallest one is March, sitting at 10.53 percent of all accidents. Follow the bars from left to right and you can see them shrink toward the end of the year, down to 7.36 percent in November and 7.11 percent in December, the two lowest months on the chart. That shape is the seasonal pattern: a slow decline of about 3.4 percentage points from peak to trough, not a sudden drop.

Now look at the two charts below it. The first breaks accidents down by day of the week, and Friday jumps out immediately at 37,000 accidents, ahead of Thursday at 35,000 and every other day sitting between 31,000 and 34,000. The second chart traces accidents across every hour of the day, starting at 10,800 accidents at midnight, dropping to its lowest point of 4,300 around 4 to 5 AM, then climbing steadily to its highest point of 15,100 accidents at 4 PM, right where the evening rush hour begins.

**In Summary:** Collisions in New York City are not random. They cluster around a specific season, a specific day, and a specific hour. March, Friday, and 4 PM together mark the highest risk window in the entire year, and any safety intervention should be timed around this window rather than spread evenly across the calendar.

### Page 2: Location and Causes

**Questions answered:**
1. Which street had the most accidents, and what percentage of all accidents does that represent?
2. What was the most common cause of accidents overall, and what was the most common cause specifically in fatal accidents?
   
<img width="1316" height="686" alt="Nyc collision page 2" src="https://github.com/user-attachments/assets/b661579f-b88d-4074-ad56-15186fed7247" />

*Figure 6: Location and Causes Page*

Look at the street ranking chart on this page. Belt Parkway sits at the top with 3,700 accidents, ahead of Broadway at 2,800, and Atlantic Avenue, Long Island Expressway, and Brooklyn Queens Expressway tied at 2,200 each. Belt Parkway alone accounts for 1.56 percent of all 238,000 accidents in this dataset.

Now look at the two bar charts placed side by side lower down. The chart on the left ranks every cause of accidents overall, and driver inattention or distraction is the tallest bar at 58,000 cases, tied with unspecified causes also at 58,000, followed by failure to yield right of way at 17,000 and following too closely at 16,000. But shift your eyes to the chart on the right, which only counts accidents where someone died. The ranking flips completely. Unspecified causes lead at 175 fatal cases, unsafe speed follows at 130, and driver inattention or distraction, the overall leader, drops to third place with only 74 fatal cases.

**In Summary:** Belt Parkway is the single highest priority location for road safety investment in this dataset. More importantly, the cause of most accidents is not the cause of most deaths. Distraction fills the streets with minor collisions, but speed is what turns a collision fatal. Any safety campaign that treats these two causes as the same problem is solving the wrong half of the issue.

### Page 3: Severity Analysis (My Own Insight)

The brief never asked for this page. I built it because raw accident counts do not tell you who is actually dying, and I wanted an answer to that question.

<img width="1323" height="688" alt="Nyc collision page 3" src="https://github.com/user-attachments/assets/80e44bfc-64f4-4244-bd1b-9385845ad621" />

*Figure 7: Severity Analysis Page*

Start with the four numbers at the top of this page. Out of 238,000 accidents, 116,000 caused an injury, a 48.7 percent injury rate, but only 635 caused a death, a fatality rate of just 0.27 percent. Most of what happened in this dataset was survivable. But keep looking, because the next two charts show exactly where the real danger is hiding.

Look at the vehicle type chart. Motorcycles sit far above every other category at a death rate of 3.09 percent, compared to 1.05 percent for utility vehicles, 0.89 percent for scooters, 0.67 percent for construction vehicles, and 0.61 percent for vehicles marked not reported. That means motorcycles are roughly eleven times more deadly than the citywide average of 0.27 percent, and close to three times more deadly than the next highest category.

Now look at the borough chart beside it. Staten Island sits at the top with a fatality rate of 0.32 percent, ahead of Bronx at 0.30 percent, Manhattan at 0.27 percent, Queens at 0.25 percent, and Brooklyn at 0.24 percent, even though it almost certainly has far fewer total accidents than Manhattan or Brooklyn.

**In Summary:** Accident volume and accident danger are two different problems, and this page is the proof. Motorcyclists face a level of risk more than ten times the citywide average, and Staten Island residents face more danger per accident than boroughs with heavier traffic. A safety strategy built only from the first two pages of this dashboard would completely miss both of these groups.


## Key Insights and Findings

Every page in this project points to the same conclusion stated in the opening hook: frequency and danger are not the same thing. The leading cause of accidents is not the leading cause of death. The vehicle type involved in the fewest accidents carries the highest death rate by far. The borough with likely fewer total accidents has the worst fatality rate of any borough. In every case, the number that happens most often is not the number that matters most.

## Summary and Conclusion

This project answers all four questions in the brief, and it does not stop there. The time and location pages describe where and when accidents happen most. The severity page, built entirely on my own initiative, proves that accident volume and accident danger point in two different directions. A safety strategy built only on accident counts would completely miss motorcyclists and Staten Island, even though both carry far higher real world risk than their accident numbers alone would suggest.

## Recommendations

- Increase enforcement on Fridays between 3 PM and 6 PM, and launch safety campaigns before March each year, when risk is highest
- Prioritize Belt Parkway for infrastructure and enforcement review given its outsized share of citywide accidents
- Run separate safety messaging for speeding and for distracted driving. They lead to different outcomes and cannot be treated as the same problem
- Motorcycle safety cannot be grouped with general vehicle policy. The fatality gap is too large to justify treating them the same
- Investigate Staten Island's fatality rate independently of its accident volume, since traffic density alone does not explain the gap

## How to Explore This Dashboard

Every page has slicers at the top that filter by year, borough, month, or vehicle type, depending on the page. Use these to check whether a finding still holds for a specific year or borough, rather than relying only on the citywide totals shown by default.

## Live Dashboard Link

[Insert Power BI published link, or a note that the dashboard is available as an image or PDF in this repository]

## Author and Contact

[Your name]
Data Analyst, Digitaley Drive Data Analytics Bootcamp
[LinkedIn] | [Email or contact]
