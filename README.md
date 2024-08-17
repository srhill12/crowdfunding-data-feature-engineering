
# Crowdfunding Data Analysis with New Features

This project involves analyzing a crowdfunding dataset and engineering new features to better understand the factors that contribute to the success of crowdfunding campaigns. The dataset includes information such as the funding goal, amount pledged, number of backers, and various categorical features.

## Dataset Overview

The dataset includes the following columns:

- `goal`: The funding goal of the project.
- `pledged`: The total amount pledged by backers.
- `backers_count`: The number of backers who supported the project.
- `country`: The country code where the project is based.
- `staff_pick`: Whether the project was selected as a "staff pick" (binary).
- `spotlight`: Whether the project was highlighted (binary).
- `category`: The category of the project.
- `days_active`: The number of days the project was active.
- `outcome`: The target variable indicating whether the project was successful (`1`) or not (`0`).

### Initial Data Preview

The dataset is loaded and a brief preview is shown:

```python
df = pd.read_csv("https://static.bc-edx.com/ai/ail-v-1-0/m14/datasets/crowdfunding-data.csv")
df.head()
```

## Feature Engineering

To enhance the dataset, several new features are created:

### 1. `pledged_per_backer`

This column represents the average amount pledged per backer. It is calculated by dividing the total amount pledged by the number of backers:

```python
df['pledged_per_backer'] = df['pledged'] / df['backers_count']
```

**Handling Missing Values:**  
For cases where `backers_count` is zero, the resulting `pledged_per_backer` will be `NaN`. These missing values are filled with zero:

```python
df['pledged_per_backer'] = df['pledged_per_backer'].fillna(0)
```

### 2. `backers_per_day`

This column calculates the average number of backers per day by dividing the total number of backers by the number of active days:

```python
df['backers_per_day'] = df['backers_count'] / df['days_active']
```

### 3. `days_to_goal`

The `days_to_goal` column estimates the number of days required to reach the funding goal based on the current pledging rate. The calculation is as follows:

```python
def days_to_goal(row):
    amount_remaining = row['goal'] - row['pledged']
    pledged_per_day = row['pledged_per_backer'] * row['backers_per_day']
    # Handle cases where pledged_per_day is zero to avoid division by zero
    if pledged_per_day == 0:
        return 10000  # Assign a large number if no pledges are expected
    return amount_remaining / pledged_per_day

df['days_to_goal'] = df.apply(days_to_goal, axis=1)
```

This new feature helps to predict how many days are needed to reach the goal, given the current pace of funding.

## Updated Dataset

After adding the new features, the dataset looks as follows:

```python
df.head()
```

| goal  | pledged | backers_count | country | staff_pick | spotlight | category | days_active | outcome | pledged_per_backer | backers_per_day | days_to_goal |
|-------|---------|---------------|---------|------------|-----------|----------|-------------|---------|-------------------|-----------------|--------------|
| 100   | 0       | 0             | 3       | 0          | 0         | 0        | 17          | 0       | 0.000000          | 0.000000        | 10000.000000 |
| 1400  | 14560   | 158           | 0       | 0          | 1         | 1        | 27          | 1       | 92.151899         | 5.851852        | -24.403846   |
| 108400| 142523  | 1425          | 4       | 0          | 0         | 2        | 20          | 1       | 100.016140        | 71.250000       | -4.788420    |
| 4200  | 2477    | 24            | 0       | 0          | 0         | 1        | 40          | 0       | 103.208333        | 0.600000        | 27.823981    |
| 7600  | 5265    | 53            | 0       | 0          | 0         | 3        | 4           | 0       | 99.339623         | 13.250000       | 1.773979     |

## Conclusion

This project successfully demonstrates how to engineer new features from existing data to gain deeper insights into the dynamics of crowdfunding campaigns. The new features `pledged_per_backer`, `backers_per_day`, and `days_to_goal` provide valuable metrics that can help in understanding the factors that contribute to the success or failure of a campaign.

These features can now be used for further analysis or as input variables in predictive models.
```

This README provides a detailed and structured overview of the project, including the steps taken to engineer new features and their significance in analyzing crowdfunding campaigns.
