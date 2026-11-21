# Pakistan Census 2017: Population Analysis (1998 to 2017)

A data analysis project in Python that compares Pakistan's 1998 and 2017 census numbers at tehsil level. I wanted to answer a few plain questions: where do people live, how fast is each area growing, and how much of the country is still rural?

The notebook goes from raw CSV to cleaned data to 17 charts, with a short written explanation under each chart.

![Population by province](images/plot1_population_by_province.png)

## Questions this project answers

1. How many people live in each province, and how fast did each one grow?
2. How much of Pakistan is urban and how much is rural? Is that changing?
3. Which districts are the biggest, the most crowded and the fastest growing?
4. What do sex ratio and household size look like across provinces?
5. Is the population spread evenly, or packed into a few districts?

## The data

One row is one tehsil (a sub-division of a district). The file has 528 rows and 21 columns and covers 131 districts in Punjab, Sindh, KPK, Balochistan and the former tribal areas (listed as KPK/FATA). Every number is given separately for the rural and urban part of each tehsil: population, males, females, transgender persons, sex ratio, average household size, the 1998 population and the yearly growth rate.

Source: 2017 Census of Pakistan, Pakistan Bureau of Statistics.

Islamabad is not in this file, so the national total here (204.4 million) is a bit lower than the official figure.

## Data problems I had to fix

The file has no empty cells, so it looks clean. It is not. Four things needed fixing before any result could be trusted:

- **Area = 0.** Four tehsils in Killa Saifullah have an area of 0 sq. km. Dividing by zero makes them look infinitely dense and they would top every density ranking. I set their area to unknown.
- **Fake zeros.** In 155 tehsils nobody lives in the urban part, but the urban sex ratio and household size are stored as 0. A normal average counts those zeros and gives nonsense, like an urban sex ratio of 15.6 for FATA. I replaced them with blanks.
- **Averaging rates.** Taking a simple mean of growth rates or ratios across tehsils treats a village of 20,000 people the same as a city of 2 million. I added up the people first and calculated the ratio afterwards, every time.
- **Naming.** Division names were in two different cases (`LAHORE DIVISION` and `Karachi Division`), and FATA was merged into KPK in 2018. I made the names consistent and kept a `PROVINCE_2018` column for anyone who wants today's map.

Sex ratio in this file means males per 100 females. The first row checks out: 316,864 males / 302,644 females x 100 = 104.7.

## What I found

**Population and growth**
- Population went from 130.7 million to 204.4 million, which is 73.7 million more people (+56%). That is 2.38% a year, enough to double the population in about 30 years.
- Punjab has 109.1 million people, 53% of the total. Sindh has 23%, KPK 15%, Balochistan 6%.
- Balochistan grew the fastest (3.37% a year, +88%). Punjab grew the slowest (2.13% a year) but still added the most people, about 36 million.

**Urban and rural**
- Urban share only moved from 32.4% to 36.4%. About two out of three people still live in rural areas.
- Sindh is the only province with an urban majority (52%), mostly because of Karachi.
- Cities grew faster than villages in every province. The gap is widest in Punjab: 3.0% a year for urban, 1.7% for rural.

**Districts**
- Lahore is the biggest district (11.1 million). Karachi looks smaller only because the data splits it into six districts, which add up to 16.0 million.
- Karachi Central is the most crowded place, at about 39,600 people per sq. km. The top four densest districts are all in Karachi.
- Quetta is the fastest growing district at 5.8% a year. Its population nearly tripled. Seven of the ten fastest districts are in Balochistan.
- Only 26 of 131 districts hold half of the country's population.

**Gender and households**
- There are 104.8 men for every 100 women. The ratio is above 100 in every province, and higher in urban areas (107.1) than in rural areas (103.5).
- The average household has 6.4 people. KPK/FATA has the biggest households (8.64 in rural areas) and Sindh the smallest (5.47).
- 21,358 transgender persons were counted, the first time a census included them. About 55% of them live in urban areas.

**One result that surprised me**

I expected more urban districts to grow faster. They don't. The correlation between urban share and growth is -0.07, which is basically zero. Growth follows the region (Balochistan and KPK) much more than how urban a district is.

## Charts in the notebook

| Section | Charts |
|---|---|
| Provinces | Bar chart, dumbbell chart (1998 vs 2017), density bar chart |
| Urban and rural | 100% stacked bar, slope chart, grouped bar of growth |
| Spread of population | Histogram (log scale), box plot, top districts, density ranking, fastest and slowest districts, Pareto curve, bubble chart |
| Gender and households | Sex ratio, household size, transgender population |
| Relationships | Spearman correlation heatmap |

Spearman correlation is used instead of Pearson because a few huge districts like Lahore and Karachi would distort Pearson.

Keep `Population-Dataset.csv` in the same folder as the notebook, then run all cells from top to bottom.

## Project structure

```
pakistan-census-2017-analysis/
├── Pakistan_Census_2017_Analysis_Final.ipynb
├── Population-Dataset.csv
├── requirements.txt
├── README.md
└── images/
    └── (chart screenshots used in this README)
```

## Limitations

- Only two census years are used, so I can't separate natural growth, migration and villages being reclassified as towns.
- District boundaries change rankings. Karachi's split into six districts is the clearest example.
- Islamabad is missing, and the area of four Killa Saifullah tehsils is unknown, so Balochistan's density is very slightly overstated.

## What I would add next

- The 2023 census, to get a third point in time
- A district-level map with geopandas or plotly
- Literacy or income data, to try to explain why some areas grow faster than others

## Tools

Python, pandas, NumPy, matplotlib, seaborn, Jupyter

## About me

**YOUR NAME**
[LinkedIn](https://www.linkedin.com/in/daniyal-arqam)
