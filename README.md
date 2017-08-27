# JSON-Based-Data-Exercises
Completed by: Chien-Cheng (Jimmy) Yang
From Work with Data in Files, Springboard Career Track

JSON exercises
1. Find the 10 countries with most projects
2. Find the top 10 major project themes (using column 'mjtheme_namecode')
3. In 2. above you will notice that some entries have only the code and the name is missing. Create a dataframe with the missing names filled in.

Solutions:
# 1. Find the 10 countries with most projects
# Read world_bank_projects.json
wb_proj = pd.read_json("data/world_bank_projects.json")
#sort countryname by size and display top 10 and store in top10_countries
top10_countries = wb_proj.groupby('countryname').size().sort_values(ascending = False).head(10)
#transform top10_countries into DataFrame
top10_countries = pd.DataFrame(data=top10_countries)
#print top10_countries
top10_countries

# 2. Find the top 10 major project themes (using column 'mjtheme_namecode')
# load world_bank_projects.json as string
str_wb_proj = json.load((open('data/world_bank_projects.json')))
# normalize column 'mjtheme_namecode', group by code, sort by size, and store top 10 into top10_proj
top10_proj = json_normalize(str_wb_proj, 'mjtheme_namecode').groupby(['code','name']).size().sort_values(ascending = False).head(10)
# change top10_proj into DataFrame and display
top10_proj = pd.DataFrame(data = top10_proj)
top10_proj

# 3. In 2. above you will notice that some entries have only the code and the name is missing. Create a dataframe with the missing names filled in.
# Display missing names, sorted by code
proj_sort = json_normalize(str_wb_proj, 'mjtheme_namecode').sort_values('code')
proj_sort
#Replace blank entries with NaN and use fillna method. Display filled proj_sort
import numpy as np
nan_missing = proj_sort.replace('', np.NaN)
proj_sort = nan_missing.fillna(method = 'ffill')
proj_sort
