## Workshop-Data Cleaning and Data Analysis

## 1. Perform Data cleaning process wherever necessary
``` py
import pandas as pd
data = pd.read_csv('/content/supermarket.csv')
```
``` py
duplicates = data.duplicated().sum()
print(f"Duplicate Rows: {duplicates}")
data = data.drop_duplicates()
```

![image](https://github.com/user-attachments/assets/c9bd06db-2371-4ec3-82d1-61fc5eaa84cb)

``` py 
missing_values = data.isnull().sum()
print(f"Missing Values:\n{missing_values}")
```
![image](https://github.com/user-attachments/assets/9ebc5986-f611-4a93-b635-5029dd03d046)

``` py
data['Date'] = pd.to_datetime(data['Date'], format='%m/%d/%Y') 
data['Time'] = pd.to_datetime(data['Time'], format='%H:%M').dt.time 
data_cleaned = data.drop(columns=['Invoice ID']) 
data_cleaned['Branch'] = data_cleaned['Branch'].str.strip()
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
for col in numerical_cols:
    print(f"{col} Min: {data_cleaned[col].min()}, Max: {data_cleaned[col].max()}")
print(data_cleaned.info())
```

![image](https://github.com/user-attachments/assets/2bd45d37-fa32-417d-80d7-fada1082cba8)

## 2. Implement Boxplot method to detect outliers

``` py
import matplotlib.pyplot as plt
import seaborn as sns
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
plt.figure(figsize=(15, 10))
for i, col in enumerate(numerical_cols, 1):
    plt.subplot(3, 3, i) 
    sns.boxplot(data=data_cleaned, x=col, color='skyblue') 
    plt.title(f'Boxplot of {col}') 
plt.tight_layout() 
plt.show()
```

![image](https://github.com/user-attachments/assets/5f4454f9-9028-4d95-a74b-818573cbfccf)

## 3. Implement IQR method to Remove Outliers


``` py
def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)  
        Q3 = df[col].quantile(0.75)  
        IQR = Q3 - Q1 
        lower_bound = Q1 - 1.5 * IQR
        upper_bound = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower_bound) & (df[col] <= upper_bound)]
    return df
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
data_no_outliers = remove_outliers_iqr(data_cleaned, numerical_cols)
print(f"Original Shape: {data_cleaned.shape}")
print(f"Shape After Outlier Removal: {data_no_outliers.shape}")
```
![image](https://github.com/user-attachments/assets/f842266c-4577-4c32-ac1e-4fc468535323)


## 4. Implement Count plot method for univariate analysis
``` py
import seaborn as sns
import matplotlib.pyplot as plt
categorical_cols = ['Branch', 'City', 'Customer type', 'Gender', 'Product line', 'Payment']
plt.figure(figsize=(15, 10))
for i, col in enumerate(categorical_cols, 1):
    plt.subplot(3, 2, i) 
    sns.countplot(data=data_cleaned, x=col, palette='viridis')  
    plt.title(f'Count Plot of {col}') 
    plt.xticks(rotation=45)  
plt.tight_layout()  
plt.show()
```

![image](https://github.com/user-attachments/assets/b527c2d0-4fe1-432a-adee-9544c5feb029)

## 5.Implement DistPlot method for multivariate analysis
``` py
import seaborn as sns
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
sns.kdeplot(data=data_cleaned, x='Rating', hue='Branch', fill=True, alpha=0.5, palette='Set2')
plt.title('Distribution of Ratings Across Branches')
plt.xlabel('Rating')
plt.ylabel('Density')
plt.show()
plt.figure(figsize=(10, 6))
sns.histplot(data=data_cleaned, x='Total', hue='Customer type', kde=True, palette='Set1', bins=30, alpha=0.7)
plt.title('Distribution of Total Across Customer Types')
plt.xlabel('Total')
plt.ylabel('Frequency')
plt.show()

```

![image](https://github.com/user-attachments/assets/b019c5ef-dda8-4454-b394-81dbf2276992)

![image](https://github.com/user-attachments/assets/13e61feb-9077-4e38-add5-d90c41527145)














