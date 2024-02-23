# Investigating Corporate Ownership Rates in Boston

<b>By: Evan Hernandez</b>

In this report, I study corporate home ownership and owner occupancy rates in Boston, MA, using a dataset provided by the MAPC. Specifically, [this dataset](https://vis-society.github.io/theme/evictions.html).

I began my exploration by reading up on corporate owernship, specifically these articles:

- [NYT: In 83 Million Eviction Records, a Sweeping
and Intimate New Look at Housing in America](https://www.nytimes.com/interactive/2018/04/07/upshot/millions-of-eviction-records-a-sweeping-new-look-at-housing-in-america.html?smid=pl-share), which discusses eviction trends in the United States. The MAPC dataset for this assignment does not directly measure evictions, but the eviction trends provide motivation for studying corporate owernship trends in general.
- [ProPublica: When Private Equity Becomes Your Landlord](https://www.propublica.org/article/when-private-equity-becomes-your-landlord). Though not directly related to this topic, it inspired me to think about how corporate ownership might price people out of their homes over time.
- [The Washington Post: Where We Build Homes Helps Explain America's Political Divide](https://www.washingtonpost.com/business/2023/11/24/counties-building-new-housing/?pwapi_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyZWFzb24iOiJnaWZ0IiwibmJmIjoxNzA3NDU0ODAwLCJpc3MiOiJzdWJzY3JpcHRpb25zIiwiZXhwIjoxNzA4ODM3MTk5LCJpYXQiOjE3MDc0NTQ4MDAsImp0aSI6IjU3Y2E0OGI3LTQyMjQtNGM1YS04YTQ0LTUxNTlkYjFmNTQ1NiIsInVybCI6Imh0dHBzOi8vd3d3Lndhc2hpbmd0b25wb3N0LmNvbS9idXNpbmVzcy8yMDIzLzExLzI0L2NvdW50aWVzLWJ1aWxkaW5nLW5ldy1ob3VzaW5nLyJ9.zlLvIGBPbd97Jt9v7sc-3VwpVKn5PnxA_Jj3rvOSYwI). Again, not directly related, but suggests that part of the housing crisis has to due with where developable land is. I wanted to see if this was related to corporate ownership.

Based on my reading, the three overall questions I chose to investigate were:

1. **What is the distribution of owner occupied vs. corporate owned homes across the Boston Area?** The Washington Post article posits that housing affordability is due to lack of developable land. As a corollary, homes near tight, but “pretty” or “popular” coastal areas should be better investments, and I would expect to see high corporate ownership rates here. Is there more corporate ownership in communities like, e.g., seaport?
1. **How has corporate ownership changed over time in the Boston Area?** One of the most interesting aspects of the dataset is that it provides twenty years of corporate ownership rates and owner occupancy rates. If higher corporate ownership is intertwined with the current housing crisis, we would expect to see staunch increases in corporate ownership recent years. How have these numbers (and the ratio between them) evolved over time in Boston?
1. **How has corporate ownership influenced housing availability in student communities?** I’ve been at MIT for almost five years now, and also lived in Boston prior to attending. Over the years, campus has rapidly transformed dramatically and is now surrounded by freshly built and very expensive apartment complexes, likely owned by big corporations. Even the new graduate dorms are outrageously expensive. I want to get some idea of how this is impacting where students choose to live.

## Preliminary Data Analysis

To begin, I perform a few basic checks on the dataset to get familiar with it. I start by looking at the distribution of corporate ownership rates by neighborhood for one year.

![Corporate Ownership Rates 2020](./figures/p1_corp_ownership_neighborhood.png)
<em>Figure 1: Corporate ownership rates in Boston neighborhoods as of 2020. There is a wide spread across neighborhoods, ranging from about 12% on the low end to 38% on the high end.</em>

The spread of this data matches my expectations, as there is a wide spread of corporate ownership rates and no outstanding or weird values. As we will see later, it turns out that the largest corporate ownership values are the coastal-adjacent cities.

The next aspect I wanted to look at was the demographic composition of Boston, since that's the most fundamental information that the Census data provides. I started small by visualizing the count of college students in each year, and noticed something odd...

![College Students Over Time](./figures/p2_college_students_time.png)
<em>Figure 2: College student population over time. This plot indicates a quirk in the dataset, in that the census data is for only one year, likely as an artifact of merging the census data with the corporate ownership data.</em>

The odd thing should be obvious: the value is constant! From this, I hypothesized that there was actually only one year in which the Census data was taken. Looking directly at the dataset, this appears to be the case.

![Explaining College Student Data Quirk](./figures/p2_1_explaining_constant.png)
<em>Figure 3: Example entries from the dataset showing that the census data is indeed duplicated.</em>

From what I can tell from the metadata, the Census data is from 2020, so from now on whenever I look at demographics, or when I need to choose a specific year, I will choose 2020. With that in mind, I turned to looking at the actual racial composition of Boston:

![Demographic Spread](./figures/p3_demographics.png)
<em>Figure 4: Demographic group counts, taken from 2020 Census data. Unsurprisingly, Boston is majority white, but is also home to hundreds of thousands of Hispanic, Black, and AAPI people, and very few AIAN people.</em>

Again, this plot roughly matched my expectations, with Boston being overwhelmingly white. However, I was surprised to see the actual scale of other communities in Boston, and noted down that my next analyses should look at how demographic composition relates to corporate ownership rates.

Finally, I noticed that some of the neighborhoods in the Census data did not have corresponding latitudes and longitudes. I decided to exclude these from all of my map plots that follow.

## Question 1: Spatial Distribution of Owner Occupied vs. Corporate Owned Homes

For my first question, I wanted to understand how corporate ownership is distributed spatially. This connects to the Washington Post article, which argues that the availability of developable land is closely related to the cost. In the case of Boston, I would expect the coastal neighborhoods to be the best investments for corporations, resulting in more corporate ownership. This is straightforward to plot as a map, using the data from 2020:

![Corporate Ownership Map](./figures/q1_1_corp_own_map.png)
<em>Figure 5: Map of corporate owernship rates across different Boston neighborhoods. The coast-adjacent neighborhoods are significantly more corporately owned than the inland neighborhoods.</em>

As expected, the costal neighborhoods appear to be hotbeds for corporate ownership. The further out you go (and, I would expect, the more developable land there is), the less corporate ownership.

### Followup: Demographics of High Owner-Occupancy Neighborhoods

This raises another question, though: what do these corporately owned neighborhoods look like demopgrahically? Based on the readings about evictions (and from watching shows like Last Week Tonight, which frequently cover these subjects), I might guess that corporate owned neighborhoods tend to be more expensive and predominantly white.

A good way to see this relationship is to look at pie charts representing the demographic composition of each neighborhood, and arrange them in order of how corporately owned the neighborhood is. The next figure does just that.

![Corporate Ownership and Demographics Plot](./figures/q1_2_demographic_pies.png)
<em>Figure 6: Demographic composition of Boston neighborhoods in 2020, sorted by corporate ownership rates for those neighborhoods from 2020. The most corporately owned neighborhoods are overwhelmingly white. The least corporately owned neighborhoods are more diverse and are often majority non-white.</em>

From Figure 6, it's clear that the neighborhoods with the most corporate ownership have mostly white populations, while the least corporately owned neighborhoods are more diverse.

## Question 2: Change in Corporate Ownership Over Time

![Corporate Ownership vs. Owner Occupancy Differential Timelapse](./figures/q2_1_differential_timelapse.gif)
<em>Figure 7: Timelapse map of the differential between corporate ownership rate and owner occupancy rate. A value of -1 would mean the neighborhood is fully corporately owned, while 1 would mean it is all owner occupied. While an imperfect metric, it captures the transition of some neighborhoods from mostly owner occupied to mostly corporately owned over the last twenty years.</em>

### Followup: Was change in ownership accelerated by the pandemic?

![Corporate Ownership and Owner Occupancy Rate vs. Time Line Plot](./figures/q2_2_corp_own_line.png)
<em>Figure 8: Plot of median corporate ownership rates and owner occupancy rates over time. The highlighted region shows the peak years of the pandemic. Corporate ownership appears to have aggressively increased in the last five years, while owner occupancy rate has dropped, albeit at a slower rate. This trend started prior to and continued throughout the pandemic.</em>

## Question 3: Influence of Corporate Owneship on Student Communities

![College Population Fractions and Vacancy Rates](./figures/q3_college_vacancy.png)
<em>Figure 9: (Left) Fraction of neighborhood population that is college students, sorted by corporate ownership rate of the neighborhood in 2020. (Right) Fraction of units that were vacant in 2020, sorted by the same criterion. There appears to be a clear correlation between corporate ownership and student population and vacancy.</em>

### Followup: What about influence on vacancy?

## Conclusions
