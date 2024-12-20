# Demoghraphics-Data-Analysis
import pandas as pd

def demographic_data_analyzer():
    # Read data from file
    df = pd.read_csv('adult.data.csv', header=None, names=[
        'age', 'workclass', 'fnlwgt', 'education', 'education-num', 'marital-status',
        'occupation', 'relationship', 'race', 'sex', 'capital-gain', 'capital-loss',
        'hours-per-week', 'native-country', 'salary'
    ])

    # How many of each race are represented in this dataset?
    race_count = df['race'].value_counts()

    # What is the average age of men?
    average_age_men = round(df[df['sex'] == 'Male']['age'].mean(), 1)

    # What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = round((df['education'] == 'Bachelors').mean() * 100, 1)

    # Advanced education includes Bachelors, Masters, or Doctorate
    advanced_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])

    # Percentage of people with advanced education earning >50K
    higher_education_rich = round((df[advanced_education & (df['salary'] == '>50K')].shape[0] / df[advanced_education].shape[0]) * 100, 1)

    # Percentage of people without advanced education earning >50K
    lower_education_rich = round((df[~advanced_education & (df['salary'] == '>50K')].shape[0] / df[~advanced_education].shape[0]) * 100, 1)

    # What is the minimum number of hours a person works per week?
    min_work_hours = df['hours-per-week'].min()

    # Percentage of people who work the minimum number of hours and earn >50K
    min_workers = df[df['hours-per-week'] == min_work_hours]
    rich_percentage = round((min_workers[min_workers['salary'] == '>50K'].shape[0] / min_workers.shape[0]) * 100, 1)

    # Country with highest percentage of people earning >50K
    country_earning = df[df['salary'] == '>50K']['native-country'].value_counts()
    country_total = df['native-country'].value_counts()
    highest_earning_country = (country_earning / country_total * 100).idxmax()
    highest_earning_country_percentage = round((country_earning / country_total * 100).max(), 1)

    # Most popular occupation for those earning >50K in India
    top_occupation_india = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]['occupation'].value_counts().idxmax()

    # Return a dictionary of results
    return {
        'race_count': race_count,
        'average_age_men': average_age_men,
        'percentage_bachelors': percentage_bachelors,
        'higher_education_rich': higher_education_rich,
        'lower_education_rich': lower_education_rich,
        'min_work_hours': min_work_hours,
        'rich_percentage': rich_percentage,
        'highest_earning_country': highest_earning_country,
        'highest_earning_country_percentage': highest_earning_country_percentage,
        'top_occupation_india': top_occupation_india
    }

# Example usage
# results = demographic_data_analyzer()
# print(results)
