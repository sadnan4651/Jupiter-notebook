# Jupiter-notebook
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="whitegrid")

df = pd.read_csv("car_price_dataset.csv").drop_duplicates()

print(df.describe(include='all'))

num_columns = ['Year', 'Engine_Size', 'Mileage', 'Doors', 'Owner_Count', 'Price']
stats = df[num_columns].agg(['mean', 'median', lambda x: x.mode()[0]])
stats.index = ['Mean', 'Median', 'Mode']
print(stats.T)

plt.figure(figsize=(20, 15))
colors = ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728', '#9467bd', '#8c564b']

for i, col in enumerate(num_columns, 1):
    plt.subplot(3, 2, i)
    sns.histplot(df[col], bins=30, kde=True, color=colors[i % len(colors)])
    for stat, color in zip(['mean', 'median', 'mode'], ['red', 'green', 'blue']):
        plt.axvline(stats.loc[stat.capitalize(), col], color=color, linestyle='dashed', linewidth=2, label=f"{stat.capitalize()}: {stats.loc[stat.capitalize(), col]:.2f}")
    plt.title(f"{col} Distribution", fontsize=16)
    plt.xlabel(col, fontsize=14)
    plt.ylabel("Frequency", fontsize=14)
    plt.legend()
    plt.grid(True, linestyle="--", alpha=0.7)

plt.tight_layout()
plt.show()
