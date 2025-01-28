
# Nobel Prize Data Analysis (1901-2023)

The Nobel Prize, one of the most prestigious international awards, has been recognizing extraordinary contributions in chemistry, literature, physics, physiology or medicine, economics, and peace since 1901. Along with the honor, prestige, and substantial prize money, each recipient receives a gold medal bearing the likeness of Alfred Nobel, the founder of the award.

In this project, I analyze a comprehensive dataset provided by the Nobel Foundation, which includes information about every Nobel Prize winner from 1901 to 2023. Using Python for data exploration and analysis, I uncovered key trends and patterns in the data, answering several intriguing questions about the history of the prize and its recipients.

## Key Insights Uncovered:

- **Most Commonly Awarded Gender and Birth Country**  
  Analysis of gender and birth countries of the laureates over time.

- **Decade with the Highest Ratio of US-born Nobel Laureates**  
  Examination of which decade saw the highest proportion of US-born winners compared to other countries.

- **Female Representation in Specific Decades and Categories**  
  Exploration of the decade and category with the highest proportion of female laureates.

- **First Female Nobel Prize Winner**  
  Identification of the first woman to win the Nobel Prize and in which category.

- **Multiple-time Nobel Prize Winners**  
  Investigation into individuals or organizations that have won more than one Nobel Prize.

## Tools and Libraries Used:
This project leverages Python's powerful libraries such as:
- **Pandas**: for data cleaning and manipulation
- **Matplotlib**: for visualizations
- **Seaborn**: for statistical plots

## Key Findings

### Most Commonly Awarded Gender and Birth Country

```python
# Loading in required libraries
import pandas as pd
import seaborn as sns
import numpy as np

# Read in the Nobel Prize data
nobel = pd.read_csv('data/nobel.csv')

# Store and display the most commonly awarded gender and birth country in requested variables
top_gender = nobel['sex'].value_counts().index[0]
top_country = nobel['birth_country'].value_counts().index[0]

print("\n The gender with the most Nobel laureates is :", top_gender)
print(" The most common birth country of Nobel laureates is :", top_country)
```

**Result:**
- The gender with the most Nobel laureates is: **Male**
- The most common birth country of Nobel laureates is: **United States of America**

### Decade with the Highest Ratio of US-born Nobel Prize Winners

```python
# Calculate the proportion of USA born winners per decade
nobel['usa_born_winner'] = nobel['birth_country'] == 'United States of America'
nobel['decade'] = (np.floor(nobel['year'] / 10) * 10).astype(int)
prop_usa_winners = nobel.groupby('decade', as_index=False)['usa_born_winner'].mean()

# Identify the decade with the highest proportion of US-born winners
max_decade_usa = prop_usa_winners[prop_usa_winners['usa_born_winner'] == prop_usa_winners['usa_born_winner'].max()]['decade'].values[0]

# Optional: Plotting USA born winners
ax1 = sns.relplot(x='decade', y='usa_born_winner', data=prop_usa_winners, kind="line")
```

### Decade and Nobel Prize Category with the Highest Proportion of Female Laureates

```python
# Calculating the proportion of female laureates per decade
nobel['female_winner'] = nobel['sex'] == 'Female'
prop_female_winners = nobel.groupby(['decade', 'category'], as_index=False)['female_winner'].mean()

# Find the decade and category with the highest proportion of female laureates
max_female_decade_category = prop_female_winners[prop_female_winners['female_winner'] == prop_female_winners['female_winner'].max()][['decade', 'category']]

# Create a dictionary with the decade and category pair
max_female_dict = {max_female_decade_category['decade'].values[0]: max_female_decade_category['category'].values[0]}

# Optional: Plotting female winners with % winners on the y-axis
ax2 = sns.relplot(x='decade', y='female_winner', hue='category', data=prop_female_winners, kind="line")
```

### First Woman to Receive a Nobel Prize and Multiple-time Nobel Prize Winners

```python
# Finding the first woman to win a Nobel Prize
nobel_women = nobel[nobel['female_winner']]
min_row = nobel_women[nobel_women['year'] == nobel_women['year'].min()]
first_woman_name = min_row['full_name'].values[0]
first_woman_category = min_row['category'].values[0]
print(f"\n The first woman to win a Nobel Prize was {first_woman_name}, in the category of {first_woman_category}.")

# Selecting the laureates that have received 2 or more prizes
counts = nobel['full_name'].value_counts()
repeats = counts[counts >= 2].index
repeat_list = list(repeats)
print("\n The repeat winners are :", repeat_list)
```

**Result:**
- The first woman to win a Nobel Prize was **Marie Curie, née Sklodowska**, in the category of **Physics**.
- The repeat winners are:  
  - Comité international de la Croix Rouge (International Committee of the Red Cross)
  - Linus Carl Pauling
  - John Bardeen
  - Frederick Sanger
  - Marie Curie, née Sklodowska
  - Office of the United Nations High Commissioner for Refugees (UNHCR)

## Conclusion
Through this analysis, we were able to uncover some fascinating trends in the history of the Nobel Prize. The most common gender and birth country are **Male** and the **United States of America**, respectively. Furthermore, we observed that the proportion of US-born Nobel Laureates peaked during a specific decade, and the first woman to receive a Nobel Prize was **Marie Curie**, in **Physics**. Additionally, we explored the repeat winners, highlighting organizations like the **International Committee of the Red Cross** and notable scientists like **Marie Curie** and **Linus Pauling** who have made multiple groundbreaking contributions.

This analysis provides valuable insights into the evolving trends of Nobel Prize winners over more than a century of awards.
```

This format follows the typical structure of a GitHub README, with clear sections and code snippets for users to understand and replicate the analysis.
