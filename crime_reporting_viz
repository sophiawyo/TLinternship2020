import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

years = [2018, 2019, 2020]
months = ["January", "February", "March", "April", "May", "June",
          "July", "August", "September", "October", "November", "December"]

df = pd.read_csv('Police_Department_Incident_Reports__2018_to_Present.csv', low_memory=False)
df['Incident Month'] = pd.Categorical(df['Incident Month'], categories=months, ordered=True)

ag = df.groupby(['Incident Month', 'Incident Year']).size().reset_index(name='counts')

# number of incident reports historical vs. 2020
fig, ax = plt.subplots()
for year in years:
    ag_y = ag.loc[ag['Incident Year'] == year]
    if year == 2020:
        ag_y = ag_y.loc[ag_y['Incident Month'] != "December"]
    ax.plot(ag_y['Incident Month'], ag_y['counts'], label=year)
ax.legend()
ax.set_title("San Francisco Crime Incident Reports Since 2018")
ax.set_xlabel("Month")
ax.tick_params(axis='x', labelrotation=90)
ax.set_ylabel("# Reports")
plt.tight_layout()
plt.savefig('yearly_reports.png')
plt.show()


ag = df.groupby(['Incident Category']).size().reset_index(name='counts')
ag = ag.loc[ag['Incident Category'] != None]
ag = ag.nlargest(10, 'counts')

# visualize top 10 crime categories
plt.figure()
plt.bar(ag['Incident Category'], ag['counts'])
plt.title("Top 10 San Francisco Crime Categories Since 2018")
plt.xlabel("Category")
plt.ylabel("# Reports")
plt.xticks(rotation=90)
plt.tight_layout()
plt.savefig('top_10.png')
plt.show()


top_categories = []
for c in ag['Incident Category']:
    top_categories.append(c)

#visualize top categories
cat = df.loc[df['Incident Category'].isin(top_categories)]
cat = cat.groupby(['Incident Category', 'Incident Month', 'Incident Year']).size().reset_index(name='counts')

fig, axs = plt.subplots(5, 2)
cols_level = [f'ax_{i}' for i in range(len(axs.flat))]
for i, ax in enumerate(axs.flat):
    c = cat.loc[cat['Incident Category'] == top_categories[i]]
    for year in years:
        ct = c.loc[c['Incident Year'] == year]
        if year == 2020:
            ct = ct.loc[ct['Incident Month'] != "December"]
        ax.plot(ct['Incident Month'], ct['counts'], label=year)
    ax.set_title(top_categories[i], size=8)
    if i not in [8, 9]:
        ax.set_xticks([])
    else:
        ax.tick_params(axis='x', labelrotation=90, length=0)
    if i == 4:
        ax.set_ylabel("# Reports")
    ax.autoscale()
plt.figlegend(years, ncol=3, loc='lower left')
fig.suptitle("Changes in San Francisco Top Crime Categories")
fig.tight_layout()
fig.subplots_adjust(top=0.88)
plt.savefig('top_10_yearly.png')
plt.show()
