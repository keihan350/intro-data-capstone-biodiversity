Completed code in the learning environment. 

1: biodiversity project

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt
species = pd.read_csv('species_info.csv')
print(species.head())

2: Inspected the DataFrame

species_count = species.scientific_name.nunique()
print(species_count)
species_type = species.category.unique()
print(species_type)
conservation_statuses = species.conservation_status.unique()
print(conservation_statuses)

3: Analyze Species Conservation Status

conservation_counts = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print(conservation_counts)

4: Analyze Conservation Status II

species.fillna('No Intervention', inplace = True)
conservation_counts_fixed = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print(conservation_counts_fixed)

5: Plotting Conservation status by species

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
xlabels = ["In Recovery", "Threatened", "Endangered",'Species of Concern','No Intervention']

ylabels = [4, 10, 15, 151, 5363]
species.fillna('No Intervention', inplace = True)
protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')
print(protection_counts)
plt.figure(figsize=(10,4))
ax = plt.subplot()
plt.bar(range(len(xlabels)),ylabels)
ax.set_xticks(range(len(xlabels)))
ax.set_xticklabels(xlabels)
plt.ylabel('Number of Species')
plt.title("Conservation Status by Species")
plt.show()


6: Investigating Endangered Species

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)

species['is_protected'] = species.conservation_status != 'No Intervention'

category_counts = species.groupby(['category', 'is_protected']).scientific_name.nunique().reset_index()

print category_counts.head()

category_pivot = category_counts.pivot(columns='is_protected',
                      index='category',
                      values='scientific_name')\
                      .reset_index()
print(category_pivot)  

7: Investigating Endangered Species II

category_pivot.columns = ['category','not_protected','protected']
category_pivot['percent_protected']= category_pivot.protected / (category_pivot.protected + category_pivot.not_protected)

print category_pivot


8: Chi-Squared Test for Significance

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt
from scipy.stats import chi2_contingency

contingency = [[30, 146],
              [75, 413]]

pval = chi2_contingency(contingency)[1]
print(pval)
# No significant difference because pval > 0.05

contingency_reptile_mammal = [[30, 146],
                              [5, 73]]

pval_reptile_mammal = chi2_contingency(contingency_reptile_mammal)[1]
print(pval_reptile_mammal)


10 - 12: Observations DataFrame, In Search of Sheep, and Merging Sheep and Observation DataFrames

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species.fillna('No Intervention', inplace = True)
species['is_protected'] = species.conservation_status != 'No Intervention'
observations = pd.read_csv('observations.csv')
print(observations.head())
species['is_sheep'] = species.common_names.apply( lambda x: True if 'Sheep' in x \
                                                else False)
species_is_sheep = species[species.is_sheep == True]
print(species_is_sheep.head())
sheep_species = species[(species.is_sheep == True) & (species.category == 'Mammal')]
print(sheep_species.head())
sheep_observations = pd.merge(sheep_species, observations)
print(sheep_observations.head())
obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()
print(obs_by_park)

13: Plotting Sheep Sightings 

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

observations = pd.read_csv('observations.csv')

sheep_observations = observations.merge(sheep_species)

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()
print(obs_by_park)
y_vals = [250, 149, 507, 282]
x_vals = ['Bryce National Park','Great Smoky Mountains National Park','Yellowstone National Park','Yosemite National Park']
plt.figure(figsize = (16, 4))
ax = plt.subplot()
plt.bar(range(len(obs_by_park.observations)), obs_by_park.observations)
ax.set_xticks(range(len(obs_by_park.observations)))
ax.set_xticklabels(obs_by_park.park_name)
plt.ylabel('Number of Observations')
plt.title('Observations of Sheep per Week')
plt.show()

14: Foot and Mouth Reduction Effort - Sample Size Determination 

baseline = 15

minimum_detectable_effect = 100*5./15

sample_size_per_variant = 870

yellowstone_weeks_observing = sample_size_per_variant/507.

bryce_weeks_observing = sample_size_per_variant/250.
