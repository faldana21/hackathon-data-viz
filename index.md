
# Instructions
## Please read carefully

### Starting Your Notebook

This is a template notebook to get you started with the data visualization impact project. __Please do not edit this notebook directly.__ Take the following steps to begin:


1.   Save a copy of this notebook - File > Save a copy in Drive
2.   Rename the copy - remove "Copy of" from the name, and replace "template" with your name (e.g., comic_book_characters_sam.ipynb)
3.   Move the notebook to our project folder in the Girls Who Code Josephinum shared Drive ([data_viz_impact_project/notebooks](https://drive.google.com/drive/u/0/folders/1Y1MhbAtWkHbN_GHCvroDRNb4-AWs4stR)). Within this notebook, you can go to File > Locate in Drive if you're not sure where the notebook is located, or navigate to the Colab Notebooks folder in your Google Drive.

# Comic Book Characters Data
__Source:__ https://github.com/fivethirtyeight/data/tree/master/comic-characters
<br>
<br>

This is the data behind the story [Comic Books Are Still Made By Men, For Men And About Men](https://fivethirtyeight.com/features/women-in-comic-books/). The data comes from [Marvel Wikia](http://marvel.wikia.com/Main_Page) and [DC Wikia](http://dc.wikia.com/wiki/Main_Page). Characters were scraped on August 24, 2018. Appearance counts were scraped on September 2, 2018. The month and year of the first issue each character appeared in was pulled on October 6, 2018.

The data is split into two files, for DC and Marvel, respectively: dc-wikia-data.csv and marvel-wikia-data.csv. Each file has information on characters' gender, alignment (good/bad/neutral), number of appearances, date of first appearance, and more. __Be careful__ because there is some missing information that shows up as `NaN` in the data, and there is some messy text and dates you may need to parse.

## Boilerplate code


1.   Import Python libraries and update Seaborn if needed
2.   Read in datasets
3.   Quickly examine a few rows of data




```
import seaborn as sns
# Check seaborn version - Make sure you restart runtime if seaborn updates!
sns_version = (sns .__version__)
if sns_version == "0.9.0":
  print("You are good to go!")
else:
  !pip install seaborn --upgrade
  print("\n \n Seaborn was updated. Please restart the notebook by going to Runtime > Restart runtime before running any further code.")
```

    You are good to go!



```
import pandas as pd
import numpy as np
import seaborn as sns
from matplotlib import pyplot
```

We can read the data directly from github. See additional details about the datasets [here](https://github.com/fivethirtyeight/data/tree/master/comic-characters).


```
dc = pd.read_csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/comic-characters/dc-wikia-data.csv")
marvel = pd.read_csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/comic-characters/marvel-wikia-data.csv")
```


```
dc.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>page_id</th>
      <th>name</th>
      <th>urlslug</th>
      <th>ID</th>
      <th>ALIGN</th>
      <th>EYE</th>
      <th>HAIR</th>
      <th>SEX</th>
      <th>GSM</th>
      <th>ALIVE</th>
      <th>APPEARANCES</th>
      <th>FIRST APPEARANCE</th>
      <th>YEAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1422</td>
      <td>Batman (Bruce Wayne)</td>
      <td>\/wiki\/Batman_(Bruce_Wayne)</td>
      <td>Secret Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>Black Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>3093.0</td>
      <td>1939, May</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>23387</td>
      <td>Superman (Clark Kent)</td>
      <td>\/wiki\/Superman_(Clark_Kent)</td>
      <td>Secret Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>Black Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>2496.0</td>
      <td>1986, October</td>
      <td>1986.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1458</td>
      <td>Green Lantern (Hal Jordan)</td>
      <td>\/wiki\/Green_Lantern_(Hal_Jordan)</td>
      <td>Secret Identity</td>
      <td>Good Characters</td>
      <td>Brown Eyes</td>
      <td>Brown Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>1565.0</td>
      <td>1959, October</td>
      <td>1959.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1659</td>
      <td>James Gordon (New Earth)</td>
      <td>\/wiki\/James_Gordon_(New_Earth)</td>
      <td>Public Identity</td>
      <td>Good Characters</td>
      <td>Brown Eyes</td>
      <td>White Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>1316.0</td>
      <td>1987, February</td>
      <td>1987.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1576</td>
      <td>Richard Grayson (New Earth)</td>
      <td>\/wiki\/Richard_Grayson_(New_Earth)</td>
      <td>Secret Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>Black Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>1237.0</td>
      <td>1940, April</td>
      <td>1940.0</td>
    </tr>
  </tbody>
</table>
</div>




```
marvel.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>page_id</th>
      <th>name</th>
      <th>urlslug</th>
      <th>ID</th>
      <th>ALIGN</th>
      <th>EYE</th>
      <th>HAIR</th>
      <th>SEX</th>
      <th>GSM</th>
      <th>ALIVE</th>
      <th>APPEARANCES</th>
      <th>FIRST APPEARANCE</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1678</td>
      <td>Spider-Man (Peter Parker)</td>
      <td>\/Spider-Man_(Peter_Parker)</td>
      <td>Secret Identity</td>
      <td>Good Characters</td>
      <td>Hazel Eyes</td>
      <td>Brown Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>4043.0</td>
      <td>Aug-62</td>
      <td>1962.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7139</td>
      <td>Captain America (Steven Rogers)</td>
      <td>\/Captain_America_(Steven_Rogers)</td>
      <td>Public Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>White Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>3360.0</td>
      <td>Mar-41</td>
      <td>1941.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>64786</td>
      <td>Wolverine (James \"Logan\" Howlett)</td>
      <td>\/Wolverine_(James_%22Logan%22_Howlett)</td>
      <td>Public Identity</td>
      <td>Neutral Characters</td>
      <td>Blue Eyes</td>
      <td>Black Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>3061.0</td>
      <td>Oct-74</td>
      <td>1974.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1868</td>
      <td>Iron Man (Anthony \"Tony\" Stark)</td>
      <td>\/Iron_Man_(Anthony_%22Tony%22_Stark)</td>
      <td>Public Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>Black Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>2961.0</td>
      <td>Mar-63</td>
      <td>1963.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2460</td>
      <td>Thor (Thor Odinson)</td>
      <td>\/Thor_(Thor_Odinson)</td>
      <td>No Dual Identity</td>
      <td>Good Characters</td>
      <td>Blue Eyes</td>
      <td>Blond Hair</td>
      <td>Male Characters</td>
      <td>NaN</td>
      <td>Living Characters</td>
      <td>2258.0</td>
      <td>Nov-50</td>
      <td>1950.0</td>
    </tr>
  </tbody>
</table>
</div>



## Your code
### Start asking questions and exploring the data! Here are some ideas:


*   Check the number of rows, or observations, in the data
*   Examine the columns in the data and the unique values of each (for categorical variables)
*   Check the data for missing values, misspellings, duplicates, or any other problems
*   Compute descriptive statistics (for continous variables)
*   Create any new variables you think would be useful - e.g., grouping/categorizing in new ways, calculating additional information
*   Start examining variables in conjunction with one another - e.g., does a result differ by gender, year, gender _and_ year?
*   Use visualizations to answer your questions!

__Make sure you use text cells to break up, organize, and add comments and thoughts to your code.__ If you find something interesting, call it out. If you have thoughts for what to do next time, write them down. Start telling your story with data. Notebooks are great for this. 


# STEPS FOR HEATMAP
- group data
- count of each combination
- pivot
- graph


```
marvel.describe
```


```
# male_female = marvel limited to men and women
# male_felmale = groupby....
male_fem = marvel[marvel["SEX"].isin(["Male Characters","Female Characters"])]

gb = male_fem.groupby(["Year", "SEX"]).count().reset_index()
gb



```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>SEX</th>
      <th>page_id</th>
      <th>name</th>
      <th>urlslug</th>
      <th>ID</th>
      <th>ALIGN</th>
      <th>EYE</th>
      <th>HAIR</th>
      <th>GSM</th>
      <th>ALIVE</th>
      <th>APPEARANCES</th>
      <th>FIRST APPEARANCE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1939.0</td>
      <td>Female Characters</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>8</td>
      <td>10</td>
      <td>1</td>
      <td>6</td>
      <td>0</td>
      <td>10</td>
      <td>9</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1939.0</td>
      <td>Male Characters</td>
      <td>56</td>
      <td>56</td>
      <td>56</td>
      <td>50</td>
      <td>56</td>
      <td>7</td>
      <td>26</td>
      <td>0</td>
      <td>56</td>
      <td>54</td>
      <td>56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1940.0</td>
      <td>Female Characters</td>
      <td>33</td>
      <td>33</td>
      <td>33</td>
      <td>30</td>
      <td>33</td>
      <td>6</td>
      <td>31</td>
      <td>0</td>
      <td>33</td>
      <td>33</td>
      <td>33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1940.0</td>
      <td>Male Characters</td>
      <td>183</td>
      <td>183</td>
      <td>183</td>
      <td>166</td>
      <td>180</td>
      <td>57</td>
      <td>157</td>
      <td>1</td>
      <td>183</td>
      <td>179</td>
      <td>183</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1941.0</td>
      <td>Female Characters</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>14</td>
      <td>14</td>
      <td>2</td>
      <td>14</td>
      <td>0</td>
      <td>15</td>
      <td>13</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1941.0</td>
      <td>Male Characters</td>
      <td>180</td>
      <td>180</td>
      <td>180</td>
      <td>161</td>
      <td>173</td>
      <td>30</td>
      <td>149</td>
      <td>0</td>
      <td>180</td>
      <td>177</td>
      <td>180</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1942.0</td>
      <td>Female Characters</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>0</td>
      <td>13</td>
      <td>0</td>
      <td>14</td>
      <td>13</td>
      <td>14</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1942.0</td>
      <td>Male Characters</td>
      <td>220</td>
      <td>220</td>
      <td>220</td>
      <td>213</td>
      <td>216</td>
      <td>9</td>
      <td>206</td>
      <td>0</td>
      <td>220</td>
      <td>214</td>
      <td>220</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1943.0</td>
      <td>Female Characters</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>2</td>
      <td>12</td>
      <td>0</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1943.0</td>
      <td>Male Characters</td>
      <td>178</td>
      <td>178</td>
      <td>178</td>
      <td>166</td>
      <td>177</td>
      <td>8</td>
      <td>162</td>
      <td>1</td>
      <td>178</td>
      <td>174</td>
      <td>178</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1944.0</td>
      <td>Female Characters</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>11</td>
      <td>2</td>
      <td>12</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1944.0</td>
      <td>Male Characters</td>
      <td>121</td>
      <td>121</td>
      <td>121</td>
      <td>117</td>
      <td>120</td>
      <td>7</td>
      <td>113</td>
      <td>0</td>
      <td>121</td>
      <td>119</td>
      <td>121</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1945.0</td>
      <td>Female Characters</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>11</td>
      <td>11</td>
      <td>2</td>
      <td>10</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1945.0</td>
      <td>Male Characters</td>
      <td>85</td>
      <td>85</td>
      <td>85</td>
      <td>82</td>
      <td>85</td>
      <td>5</td>
      <td>79</td>
      <td>0</td>
      <td>85</td>
      <td>85</td>
      <td>85</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1946.0</td>
      <td>Female Characters</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>10</td>
      <td>11</td>
      <td>1</td>
      <td>11</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1946.0</td>
      <td>Male Characters</td>
      <td>78</td>
      <td>78</td>
      <td>78</td>
      <td>75</td>
      <td>78</td>
      <td>3</td>
      <td>74</td>
      <td>0</td>
      <td>78</td>
      <td>77</td>
      <td>78</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1947.0</td>
      <td>Female Characters</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>4</td>
      <td>13</td>
      <td>0</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1947.0</td>
      <td>Male Characters</td>
      <td>58</td>
      <td>58</td>
      <td>58</td>
      <td>55</td>
      <td>58</td>
      <td>0</td>
      <td>51</td>
      <td>0</td>
      <td>58</td>
      <td>57</td>
      <td>58</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1948.0</td>
      <td>Female Characters</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>12</td>
      <td>6</td>
      <td>14</td>
      <td>1</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1948.0</td>
      <td>Male Characters</td>
      <td>99</td>
      <td>99</td>
      <td>99</td>
      <td>95</td>
      <td>98</td>
      <td>9</td>
      <td>93</td>
      <td>0</td>
      <td>99</td>
      <td>96</td>
      <td>99</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1949.0</td>
      <td>Female Characters</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1949.0</td>
      <td>Male Characters</td>
      <td>53</td>
      <td>53</td>
      <td>53</td>
      <td>52</td>
      <td>51</td>
      <td>12</td>
      <td>50</td>
      <td>0</td>
      <td>53</td>
      <td>51</td>
      <td>53</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1950.0</td>
      <td>Female Characters</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1950.0</td>
      <td>Male Characters</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>5</td>
      <td>24</td>
      <td>0</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1951.0</td>
      <td>Female Characters</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1951.0</td>
      <td>Male Characters</td>
      <td>43</td>
      <td>43</td>
      <td>43</td>
      <td>42</td>
      <td>41</td>
      <td>6</td>
      <td>42</td>
      <td>0</td>
      <td>43</td>
      <td>41</td>
      <td>43</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1952.0</td>
      <td>Female Characters</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1952.0</td>
      <td>Male Characters</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>19</td>
      <td>20</td>
      <td>2</td>
      <td>16</td>
      <td>0</td>
      <td>20</td>
      <td>16</td>
      <td>20</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1953.0</td>
      <td>Female Characters</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>9</td>
      <td>1</td>
      <td>9</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1953.0</td>
      <td>Male Characters</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>23</td>
      <td>25</td>
      <td>7</td>
      <td>18</td>
      <td>0</td>
      <td>26</td>
      <td>23</td>
      <td>26</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>118</th>
      <td>1999.0</td>
      <td>Female Characters</td>
      <td>72</td>
      <td>72</td>
      <td>72</td>
      <td>59</td>
      <td>52</td>
      <td>42</td>
      <td>60</td>
      <td>0</td>
      <td>72</td>
      <td>69</td>
      <td>72</td>
    </tr>
    <tr>
      <th>119</th>
      <td>1999.0</td>
      <td>Male Characters</td>
      <td>147</td>
      <td>147</td>
      <td>147</td>
      <td>118</td>
      <td>98</td>
      <td>62</td>
      <td>101</td>
      <td>0</td>
      <td>147</td>
      <td>138</td>
      <td>147</td>
    </tr>
    <tr>
      <th>120</th>
      <td>2000.0</td>
      <td>Female Characters</td>
      <td>87</td>
      <td>87</td>
      <td>87</td>
      <td>65</td>
      <td>74</td>
      <td>34</td>
      <td>65</td>
      <td>1</td>
      <td>87</td>
      <td>84</td>
      <td>87</td>
    </tr>
    <tr>
      <th>121</th>
      <td>2000.0</td>
      <td>Male Characters</td>
      <td>221</td>
      <td>221</td>
      <td>221</td>
      <td>167</td>
      <td>178</td>
      <td>95</td>
      <td>143</td>
      <td>0</td>
      <td>221</td>
      <td>204</td>
      <td>221</td>
    </tr>
    <tr>
      <th>122</th>
      <td>2001.0</td>
      <td>Female Characters</td>
      <td>73</td>
      <td>73</td>
      <td>73</td>
      <td>64</td>
      <td>66</td>
      <td>44</td>
      <td>68</td>
      <td>2</td>
      <td>73</td>
      <td>67</td>
      <td>73</td>
    </tr>
    <tr>
      <th>123</th>
      <td>2001.0</td>
      <td>Male Characters</td>
      <td>148</td>
      <td>148</td>
      <td>148</td>
      <td>113</td>
      <td>121</td>
      <td>84</td>
      <td>115</td>
      <td>5</td>
      <td>148</td>
      <td>138</td>
      <td>148</td>
    </tr>
    <tr>
      <th>124</th>
      <td>2002.0</td>
      <td>Female Characters</td>
      <td>85</td>
      <td>85</td>
      <td>85</td>
      <td>70</td>
      <td>68</td>
      <td>50</td>
      <td>74</td>
      <td>1</td>
      <td>85</td>
      <td>82</td>
      <td>85</td>
    </tr>
    <tr>
      <th>125</th>
      <td>2002.0</td>
      <td>Male Characters</td>
      <td>214</td>
      <td>214</td>
      <td>214</td>
      <td>170</td>
      <td>167</td>
      <td>89</td>
      <td>158</td>
      <td>0</td>
      <td>214</td>
      <td>210</td>
      <td>214</td>
    </tr>
    <tr>
      <th>126</th>
      <td>2003.0</td>
      <td>Female Characters</td>
      <td>84</td>
      <td>84</td>
      <td>84</td>
      <td>72</td>
      <td>73</td>
      <td>62</td>
      <td>71</td>
      <td>2</td>
      <td>84</td>
      <td>82</td>
      <td>84</td>
    </tr>
    <tr>
      <th>127</th>
      <td>2003.0</td>
      <td>Male Characters</td>
      <td>170</td>
      <td>170</td>
      <td>170</td>
      <td>142</td>
      <td>145</td>
      <td>105</td>
      <td>142</td>
      <td>5</td>
      <td>170</td>
      <td>160</td>
      <td>170</td>
    </tr>
    <tr>
      <th>128</th>
      <td>2004.0</td>
      <td>Female Characters</td>
      <td>108</td>
      <td>108</td>
      <td>108</td>
      <td>91</td>
      <td>84</td>
      <td>71</td>
      <td>100</td>
      <td>2</td>
      <td>108</td>
      <td>107</td>
      <td>108</td>
    </tr>
    <tr>
      <th>129</th>
      <td>2004.0</td>
      <td>Male Characters</td>
      <td>168</td>
      <td>168</td>
      <td>168</td>
      <td>135</td>
      <td>139</td>
      <td>71</td>
      <td>130</td>
      <td>0</td>
      <td>168</td>
      <td>158</td>
      <td>168</td>
    </tr>
    <tr>
      <th>130</th>
      <td>2005.0</td>
      <td>Female Characters</td>
      <td>112</td>
      <td>112</td>
      <td>112</td>
      <td>84</td>
      <td>80</td>
      <td>69</td>
      <td>94</td>
      <td>1</td>
      <td>112</td>
      <td>109</td>
      <td>112</td>
    </tr>
    <tr>
      <th>131</th>
      <td>2005.0</td>
      <td>Male Characters</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>165</td>
      <td>166</td>
      <td>101</td>
      <td>156</td>
      <td>3</td>
      <td>211</td>
      <td>188</td>
      <td>211</td>
    </tr>
    <tr>
      <th>132</th>
      <td>2006.0</td>
      <td>Female Characters</td>
      <td>105</td>
      <td>105</td>
      <td>105</td>
      <td>79</td>
      <td>87</td>
      <td>56</td>
      <td>82</td>
      <td>0</td>
      <td>105</td>
      <td>97</td>
      <td>105</td>
    </tr>
    <tr>
      <th>133</th>
      <td>2006.0</td>
      <td>Male Characters</td>
      <td>259</td>
      <td>259</td>
      <td>259</td>
      <td>183</td>
      <td>201</td>
      <td>118</td>
      <td>179</td>
      <td>4</td>
      <td>259</td>
      <td>243</td>
      <td>259</td>
    </tr>
    <tr>
      <th>134</th>
      <td>2007.0</td>
      <td>Female Characters</td>
      <td>82</td>
      <td>82</td>
      <td>82</td>
      <td>70</td>
      <td>63</td>
      <td>54</td>
      <td>66</td>
      <td>2</td>
      <td>82</td>
      <td>78</td>
      <td>82</td>
    </tr>
    <tr>
      <th>135</th>
      <td>2007.0</td>
      <td>Male Characters</td>
      <td>209</td>
      <td>209</td>
      <td>209</td>
      <td>179</td>
      <td>179</td>
      <td>126</td>
      <td>154</td>
      <td>0</td>
      <td>209</td>
      <td>196</td>
      <td>209</td>
    </tr>
    <tr>
      <th>136</th>
      <td>2008.0</td>
      <td>Female Characters</td>
      <td>101</td>
      <td>101</td>
      <td>101</td>
      <td>85</td>
      <td>85</td>
      <td>69</td>
      <td>91</td>
      <td>1</td>
      <td>101</td>
      <td>92</td>
      <td>101</td>
    </tr>
    <tr>
      <th>137</th>
      <td>2008.0</td>
      <td>Male Characters</td>
      <td>244</td>
      <td>244</td>
      <td>244</td>
      <td>204</td>
      <td>205</td>
      <td>148</td>
      <td>195</td>
      <td>1</td>
      <td>244</td>
      <td>219</td>
      <td>244</td>
    </tr>
    <tr>
      <th>138</th>
      <td>2009.0</td>
      <td>Female Characters</td>
      <td>85</td>
      <td>85</td>
      <td>85</td>
      <td>77</td>
      <td>79</td>
      <td>44</td>
      <td>74</td>
      <td>1</td>
      <td>85</td>
      <td>78</td>
      <td>85</td>
    </tr>
    <tr>
      <th>139</th>
      <td>2009.0</td>
      <td>Male Characters</td>
      <td>199</td>
      <td>199</td>
      <td>199</td>
      <td>147</td>
      <td>171</td>
      <td>97</td>
      <td>136</td>
      <td>1</td>
      <td>199</td>
      <td>178</td>
      <td>199</td>
    </tr>
    <tr>
      <th>140</th>
      <td>2010.0</td>
      <td>Female Characters</td>
      <td>110</td>
      <td>110</td>
      <td>110</td>
      <td>98</td>
      <td>108</td>
      <td>76</td>
      <td>102</td>
      <td>2</td>
      <td>110</td>
      <td>107</td>
      <td>110</td>
    </tr>
    <tr>
      <th>141</th>
      <td>2010.0</td>
      <td>Male Characters</td>
      <td>200</td>
      <td>200</td>
      <td>200</td>
      <td>184</td>
      <td>186</td>
      <td>110</td>
      <td>160</td>
      <td>2</td>
      <td>200</td>
      <td>191</td>
      <td>200</td>
    </tr>
    <tr>
      <th>142</th>
      <td>2011.0</td>
      <td>Female Characters</td>
      <td>99</td>
      <td>99</td>
      <td>99</td>
      <td>77</td>
      <td>86</td>
      <td>68</td>
      <td>91</td>
      <td>1</td>
      <td>99</td>
      <td>92</td>
      <td>99</td>
    </tr>
    <tr>
      <th>143</th>
      <td>2011.0</td>
      <td>Male Characters</td>
      <td>235</td>
      <td>235</td>
      <td>235</td>
      <td>203</td>
      <td>227</td>
      <td>142</td>
      <td>191</td>
      <td>3</td>
      <td>235</td>
      <td>225</td>
      <td>235</td>
    </tr>
    <tr>
      <th>144</th>
      <td>2012.0</td>
      <td>Female Characters</td>
      <td>56</td>
      <td>56</td>
      <td>56</td>
      <td>46</td>
      <td>51</td>
      <td>35</td>
      <td>48</td>
      <td>1</td>
      <td>56</td>
      <td>50</td>
      <td>56</td>
    </tr>
    <tr>
      <th>145</th>
      <td>2012.0</td>
      <td>Male Characters</td>
      <td>140</td>
      <td>140</td>
      <td>140</td>
      <td>121</td>
      <td>127</td>
      <td>78</td>
      <td>101</td>
      <td>3</td>
      <td>140</td>
      <td>122</td>
      <td>140</td>
    </tr>
    <tr>
      <th>146</th>
      <td>2013.0</td>
      <td>Female Characters</td>
      <td>54</td>
      <td>54</td>
      <td>54</td>
      <td>38</td>
      <td>50</td>
      <td>42</td>
      <td>46</td>
      <td>2</td>
      <td>54</td>
      <td>52</td>
      <td>54</td>
    </tr>
    <tr>
      <th>147</th>
      <td>2013.0</td>
      <td>Male Characters</td>
      <td>97</td>
      <td>97</td>
      <td>97</td>
      <td>88</td>
      <td>86</td>
      <td>52</td>
      <td>68</td>
      <td>2</td>
      <td>97</td>
      <td>94</td>
      <td>97</td>
    </tr>
  </tbody>
</table>
<p>148 rows × 13 columns</p>
</div>




```
marvelpivot = gb.pivot("Year","SEX","page_id")
marvelpivot
```


```
marvelpivot
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>SEX</th>
      <th>Female Characters</th>
      <th>Male Characters</th>
    </tr>
    <tr>
      <th>Year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1939.0</th>
      <td>10.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>1940.0</th>
      <td>33.0</td>
      <td>183.0</td>
    </tr>
    <tr>
      <th>1941.0</th>
      <td>15.0</td>
      <td>180.0</td>
    </tr>
    <tr>
      <th>1942.0</th>
      <td>14.0</td>
      <td>220.0</td>
    </tr>
    <tr>
      <th>1943.0</th>
      <td>13.0</td>
      <td>178.0</td>
    </tr>
    <tr>
      <th>1944.0</th>
      <td>12.0</td>
      <td>121.0</td>
    </tr>
    <tr>
      <th>1945.0</th>
      <td>12.0</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>1946.0</th>
      <td>12.0</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>1947.0</th>
      <td>13.0</td>
      <td>58.0</td>
    </tr>
    <tr>
      <th>1948.0</th>
      <td>14.0</td>
      <td>99.0</td>
    </tr>
    <tr>
      <th>1949.0</th>
      <td>6.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>1950.0</th>
      <td>3.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1951.0</th>
      <td>9.0</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>1952.0</th>
      <td>5.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1953.0</th>
      <td>10.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1954.0</th>
      <td>19.0</td>
      <td>57.0</td>
    </tr>
    <tr>
      <th>1955.0</th>
      <td>4.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>1956.0</th>
      <td>1.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1957.0</th>
      <td>NaN</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1958.0</th>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1959.0</th>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1960.0</th>
      <td>2.0</td>
      <td>36.0</td>
    </tr>
    <tr>
      <th>1961.0</th>
      <td>5.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>1962.0</th>
      <td>13.0</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>1963.0</th>
      <td>25.0</td>
      <td>149.0</td>
    </tr>
    <tr>
      <th>1964.0</th>
      <td>23.0</td>
      <td>144.0</td>
    </tr>
    <tr>
      <th>1965.0</th>
      <td>21.0</td>
      <td>132.0</td>
    </tr>
    <tr>
      <th>1966.0</th>
      <td>19.0</td>
      <td>107.0</td>
    </tr>
    <tr>
      <th>1967.0</th>
      <td>10.0</td>
      <td>102.0</td>
    </tr>
    <tr>
      <th>1968.0</th>
      <td>18.0</td>
      <td>111.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1984.0</th>
      <td>61.0</td>
      <td>143.0</td>
    </tr>
    <tr>
      <th>1985.0</th>
      <td>69.0</td>
      <td>171.0</td>
    </tr>
    <tr>
      <th>1986.0</th>
      <td>66.0</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>1987.0</th>
      <td>45.0</td>
      <td>129.0</td>
    </tr>
    <tr>
      <th>1988.0</th>
      <td>81.0</td>
      <td>215.0</td>
    </tr>
    <tr>
      <th>1989.0</th>
      <td>71.0</td>
      <td>237.0</td>
    </tr>
    <tr>
      <th>1990.0</th>
      <td>92.0</td>
      <td>243.0</td>
    </tr>
    <tr>
      <th>1991.0</th>
      <td>92.0</td>
      <td>232.0</td>
    </tr>
    <tr>
      <th>1992.0</th>
      <td>118.0</td>
      <td>318.0</td>
    </tr>
    <tr>
      <th>1993.0</th>
      <td>125.0</td>
      <td>401.0</td>
    </tr>
    <tr>
      <th>1994.0</th>
      <td>117.0</td>
      <td>344.0</td>
    </tr>
    <tr>
      <th>1995.0</th>
      <td>79.0</td>
      <td>210.0</td>
    </tr>
    <tr>
      <th>1996.0</th>
      <td>101.0</td>
      <td>192.0</td>
    </tr>
    <tr>
      <th>1997.0</th>
      <td>91.0</td>
      <td>194.0</td>
    </tr>
    <tr>
      <th>1998.0</th>
      <td>76.0</td>
      <td>190.0</td>
    </tr>
    <tr>
      <th>1999.0</th>
      <td>72.0</td>
      <td>147.0</td>
    </tr>
    <tr>
      <th>2000.0</th>
      <td>87.0</td>
      <td>221.0</td>
    </tr>
    <tr>
      <th>2001.0</th>
      <td>73.0</td>
      <td>148.0</td>
    </tr>
    <tr>
      <th>2002.0</th>
      <td>85.0</td>
      <td>214.0</td>
    </tr>
    <tr>
      <th>2003.0</th>
      <td>84.0</td>
      <td>170.0</td>
    </tr>
    <tr>
      <th>2004.0</th>
      <td>108.0</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>2005.0</th>
      <td>112.0</td>
      <td>211.0</td>
    </tr>
    <tr>
      <th>2006.0</th>
      <td>105.0</td>
      <td>259.0</td>
    </tr>
    <tr>
      <th>2007.0</th>
      <td>82.0</td>
      <td>209.0</td>
    </tr>
    <tr>
      <th>2008.0</th>
      <td>101.0</td>
      <td>244.0</td>
    </tr>
    <tr>
      <th>2009.0</th>
      <td>85.0</td>
      <td>199.0</td>
    </tr>
    <tr>
      <th>2010.0</th>
      <td>110.0</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>2011.0</th>
      <td>99.0</td>
      <td>235.0</td>
    </tr>
    <tr>
      <th>2012.0</th>
      <td>56.0</td>
      <td>140.0</td>
    </tr>
    <tr>
      <th>2013.0</th>
      <td>54.0</td>
      <td>97.0</td>
    </tr>
  </tbody>
</table>
<p>75 rows × 2 columns</p>
</div>




```
 fig, ax = pyplot.subplots(figsize=(15,15))

marvel_hm = sns.heatmap(marvelpivot,  cmap="YlGnBu", ax=ax)
marvel_hm
```


```
dcmale_fem = dc[dc["SEX"].isin(["Male Characters","Female Characters"])]

gbdc = dcmale_fem.groupby(["YEAR", "SEX"]).count().reset_index()
gbdc
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YEAR</th>
      <th>SEX</th>
      <th>page_id</th>
      <th>name</th>
      <th>urlslug</th>
      <th>ID</th>
      <th>ALIGN</th>
      <th>EYE</th>
      <th>HAIR</th>
      <th>GSM</th>
      <th>ALIVE</th>
      <th>APPEARANCES</th>
      <th>FIRST APPEARANCE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1935.0</td>
      <td>Male Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1936.0</td>
      <td>Female Characters</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1936.0</td>
      <td>Male Characters</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>7</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1937.0</td>
      <td>Female Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1937.0</td>
      <td>Male Characters</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1938.0</td>
      <td>Female Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1938.0</td>
      <td>Male Characters</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>8</td>
      <td>7</td>
      <td>5</td>
      <td>7</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1939.0</td>
      <td>Female Characters</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1939.0</td>
      <td>Male Characters</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
      <td>10</td>
      <td>12</td>
      <td>7</td>
      <td>11</td>
      <td>0</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1940.0</td>
      <td>Female Characters</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>10</td>
      <td>10</td>
      <td>9</td>
      <td>11</td>
      <td>0</td>
      <td>11</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1940.0</td>
      <td>Male Characters</td>
      <td>51</td>
      <td>51</td>
      <td>51</td>
      <td>45</td>
      <td>47</td>
      <td>39</td>
      <td>40</td>
      <td>0</td>
      <td>51</td>
      <td>50</td>
      <td>51</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1941.0</td>
      <td>Female Characters</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>8</td>
      <td>0</td>
      <td>8</td>
      <td>7</td>
      <td>8</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1941.0</td>
      <td>Male Characters</td>
      <td>53</td>
      <td>53</td>
      <td>53</td>
      <td>39</td>
      <td>47</td>
      <td>34</td>
      <td>46</td>
      <td>0</td>
      <td>53</td>
      <td>52</td>
      <td>53</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1942.0</td>
      <td>Female Characters</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1942.0</td>
      <td>Male Characters</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>39</td>
      <td>40</td>
      <td>23</td>
      <td>40</td>
      <td>0</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1943.0</td>
      <td>Male Characters</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>13</td>
      <td>13</td>
      <td>10</td>
      <td>8</td>
      <td>1</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1944.0</td>
      <td>Female Characters</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1944.0</td>
      <td>Male Characters</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>10</td>
      <td>11</td>
      <td>6</td>
      <td>6</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1945.0</td>
      <td>Male Characters</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>6</td>
      <td>6</td>
      <td>4</td>
      <td>5</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1946.0</td>
      <td>Female Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1946.0</td>
      <td>Male Characters</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>5</td>
      <td>7</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>8</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1947.0</td>
      <td>Female Characters</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1947.0</td>
      <td>Male Characters</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>12</td>
      <td>14</td>
      <td>8</td>
      <td>10</td>
      <td>0</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1948.0</td>
      <td>Female Characters</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1948.0</td>
      <td>Male Characters</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>9</td>
      <td>12</td>
      <td>6</td>
      <td>9</td>
      <td>0</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1949.0</td>
      <td>Male Characters</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>4</td>
      <td>5</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1950.0</td>
      <td>Male Characters</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>8</td>
      <td>7</td>
      <td>8</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1951.0</td>
      <td>Female Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1951.0</td>
      <td>Male Characters</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>8</td>
      <td>10</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1952.0</td>
      <td>Male Characters</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>5</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>118</th>
      <td>1998.0</td>
      <td>Male Characters</td>
      <td>92</td>
      <td>92</td>
      <td>92</td>
      <td>60</td>
      <td>90</td>
      <td>34</td>
      <td>51</td>
      <td>0</td>
      <td>92</td>
      <td>79</td>
      <td>92</td>
    </tr>
    <tr>
      <th>119</th>
      <td>1999.0</td>
      <td>Female Characters</td>
      <td>61</td>
      <td>61</td>
      <td>61</td>
      <td>41</td>
      <td>57</td>
      <td>39</td>
      <td>56</td>
      <td>0</td>
      <td>61</td>
      <td>56</td>
      <td>61</td>
    </tr>
    <tr>
      <th>120</th>
      <td>1999.0</td>
      <td>Male Characters</td>
      <td>116</td>
      <td>116</td>
      <td>116</td>
      <td>77</td>
      <td>110</td>
      <td>46</td>
      <td>78</td>
      <td>0</td>
      <td>116</td>
      <td>105</td>
      <td>116</td>
    </tr>
    <tr>
      <th>121</th>
      <td>2000.0</td>
      <td>Female Characters</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>36</td>
      <td>41</td>
      <td>24</td>
      <td>39</td>
      <td>0</td>
      <td>46</td>
      <td>39</td>
      <td>46</td>
    </tr>
    <tr>
      <th>122</th>
      <td>2000.0</td>
      <td>Male Characters</td>
      <td>102</td>
      <td>102</td>
      <td>102</td>
      <td>77</td>
      <td>96</td>
      <td>46</td>
      <td>67</td>
      <td>2</td>
      <td>102</td>
      <td>94</td>
      <td>102</td>
    </tr>
    <tr>
      <th>123</th>
      <td>2001.0</td>
      <td>Female Characters</td>
      <td>42</td>
      <td>42</td>
      <td>42</td>
      <td>28</td>
      <td>38</td>
      <td>27</td>
      <td>31</td>
      <td>1</td>
      <td>42</td>
      <td>38</td>
      <td>42</td>
    </tr>
    <tr>
      <th>124</th>
      <td>2001.0</td>
      <td>Male Characters</td>
      <td>54</td>
      <td>54</td>
      <td>54</td>
      <td>40</td>
      <td>51</td>
      <td>33</td>
      <td>37</td>
      <td>0</td>
      <td>54</td>
      <td>48</td>
      <td>54</td>
    </tr>
    <tr>
      <th>125</th>
      <td>2002.0</td>
      <td>Female Characters</td>
      <td>41</td>
      <td>41</td>
      <td>41</td>
      <td>29</td>
      <td>34</td>
      <td>20</td>
      <td>32</td>
      <td>1</td>
      <td>41</td>
      <td>41</td>
      <td>41</td>
    </tr>
    <tr>
      <th>126</th>
      <td>2002.0</td>
      <td>Male Characters</td>
      <td>70</td>
      <td>70</td>
      <td>70</td>
      <td>55</td>
      <td>68</td>
      <td>24</td>
      <td>42</td>
      <td>2</td>
      <td>70</td>
      <td>66</td>
      <td>70</td>
    </tr>
    <tr>
      <th>127</th>
      <td>2003.0</td>
      <td>Female Characters</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>22</td>
      <td>26</td>
      <td>23</td>
      <td>32</td>
      <td>4</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
    </tr>
    <tr>
      <th>128</th>
      <td>2003.0</td>
      <td>Male Characters</td>
      <td>71</td>
      <td>71</td>
      <td>71</td>
      <td>49</td>
      <td>66</td>
      <td>25</td>
      <td>44</td>
      <td>1</td>
      <td>71</td>
      <td>67</td>
      <td>71</td>
    </tr>
    <tr>
      <th>129</th>
      <td>2004.0</td>
      <td>Female Characters</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>24</td>
      <td>30</td>
      <td>19</td>
      <td>27</td>
      <td>0</td>
      <td>31</td>
      <td>29</td>
      <td>31</td>
    </tr>
    <tr>
      <th>130</th>
      <td>2004.0</td>
      <td>Male Characters</td>
      <td>71</td>
      <td>71</td>
      <td>71</td>
      <td>52</td>
      <td>61</td>
      <td>35</td>
      <td>49</td>
      <td>2</td>
      <td>71</td>
      <td>67</td>
      <td>71</td>
    </tr>
    <tr>
      <th>131</th>
      <td>2005.0</td>
      <td>Female Characters</td>
      <td>54</td>
      <td>54</td>
      <td>54</td>
      <td>42</td>
      <td>52</td>
      <td>28</td>
      <td>45</td>
      <td>1</td>
      <td>54</td>
      <td>53</td>
      <td>54</td>
    </tr>
    <tr>
      <th>132</th>
      <td>2005.0</td>
      <td>Male Characters</td>
      <td>102</td>
      <td>102</td>
      <td>102</td>
      <td>84</td>
      <td>101</td>
      <td>42</td>
      <td>57</td>
      <td>0</td>
      <td>102</td>
      <td>96</td>
      <td>102</td>
    </tr>
    <tr>
      <th>133</th>
      <td>2006.0</td>
      <td>Female Characters</td>
      <td>96</td>
      <td>96</td>
      <td>96</td>
      <td>71</td>
      <td>78</td>
      <td>52</td>
      <td>78</td>
      <td>2</td>
      <td>96</td>
      <td>93</td>
      <td>96</td>
    </tr>
    <tr>
      <th>134</th>
      <td>2006.0</td>
      <td>Male Characters</td>
      <td>198</td>
      <td>198</td>
      <td>198</td>
      <td>159</td>
      <td>174</td>
      <td>97</td>
      <td>108</td>
      <td>2</td>
      <td>198</td>
      <td>186</td>
      <td>198</td>
    </tr>
    <tr>
      <th>135</th>
      <td>2007.0</td>
      <td>Female Characters</td>
      <td>58</td>
      <td>58</td>
      <td>58</td>
      <td>33</td>
      <td>47</td>
      <td>30</td>
      <td>46</td>
      <td>0</td>
      <td>58</td>
      <td>56</td>
      <td>58</td>
    </tr>
    <tr>
      <th>136</th>
      <td>2007.0</td>
      <td>Male Characters</td>
      <td>124</td>
      <td>124</td>
      <td>124</td>
      <td>84</td>
      <td>115</td>
      <td>57</td>
      <td>64</td>
      <td>0</td>
      <td>124</td>
      <td>121</td>
      <td>124</td>
    </tr>
    <tr>
      <th>137</th>
      <td>2008.0</td>
      <td>Female Characters</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>54</td>
      <td>67</td>
      <td>40</td>
      <td>54</td>
      <td>1</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
    </tr>
    <tr>
      <th>138</th>
      <td>2008.0</td>
      <td>Male Characters</td>
      <td>133</td>
      <td>133</td>
      <td>133</td>
      <td>84</td>
      <td>129</td>
      <td>51</td>
      <td>68</td>
      <td>0</td>
      <td>133</td>
      <td>131</td>
      <td>133</td>
    </tr>
    <tr>
      <th>139</th>
      <td>2009.0</td>
      <td>Female Characters</td>
      <td>70</td>
      <td>70</td>
      <td>70</td>
      <td>46</td>
      <td>63</td>
      <td>30</td>
      <td>50</td>
      <td>0</td>
      <td>70</td>
      <td>68</td>
      <td>70</td>
    </tr>
    <tr>
      <th>140</th>
      <td>2009.0</td>
      <td>Male Characters</td>
      <td>152</td>
      <td>152</td>
      <td>152</td>
      <td>92</td>
      <td>136</td>
      <td>51</td>
      <td>82</td>
      <td>2</td>
      <td>152</td>
      <td>151</td>
      <td>152</td>
    </tr>
    <tr>
      <th>141</th>
      <td>2010.0</td>
      <td>Female Characters</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>63</td>
      <td>60</td>
      <td>45</td>
      <td>64</td>
      <td>0</td>
      <td>74</td>
      <td>72</td>
      <td>74</td>
    </tr>
    <tr>
      <th>142</th>
      <td>2010.0</td>
      <td>Male Characters</td>
      <td>199</td>
      <td>199</td>
      <td>199</td>
      <td>156</td>
      <td>153</td>
      <td>54</td>
      <td>94</td>
      <td>1</td>
      <td>199</td>
      <td>192</td>
      <td>199</td>
    </tr>
    <tr>
      <th>143</th>
      <td>2011.0</td>
      <td>Female Characters</td>
      <td>51</td>
      <td>51</td>
      <td>51</td>
      <td>41</td>
      <td>48</td>
      <td>32</td>
      <td>43</td>
      <td>0</td>
      <td>51</td>
      <td>49</td>
      <td>51</td>
    </tr>
    <tr>
      <th>144</th>
      <td>2011.0</td>
      <td>Male Characters</td>
      <td>98</td>
      <td>98</td>
      <td>98</td>
      <td>84</td>
      <td>91</td>
      <td>36</td>
      <td>53</td>
      <td>0</td>
      <td>98</td>
      <td>97</td>
      <td>98</td>
    </tr>
    <tr>
      <th>145</th>
      <td>2012.0</td>
      <td>Female Characters</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>146</th>
      <td>2012.0</td>
      <td>Male Characters</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>147</th>
      <td>2013.0</td>
      <td>Male Characters</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>148 rows × 13 columns</p>
</div>




```
dcpivot = gbdc.pivot("YEAR","SEX","page_id")
dcpivot
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>SEX</th>
      <th>Female Characters</th>
      <th>Male Characters</th>
    </tr>
    <tr>
      <th>YEAR</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1935.0</th>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1936.0</th>
      <td>2.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1937.0</th>
      <td>1.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1938.0</th>
      <td>1.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>1939.0</th>
      <td>5.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1940.0</th>
      <td>11.0</td>
      <td>51.0</td>
    </tr>
    <tr>
      <th>1941.0</th>
      <td>8.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>1942.0</th>
      <td>5.0</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>1943.0</th>
      <td>NaN</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>1944.0</th>
      <td>3.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1945.0</th>
      <td>NaN</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1946.0</th>
      <td>1.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1947.0</th>
      <td>5.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>1948.0</th>
      <td>4.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>1949.0</th>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1950.0</th>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>1951.0</th>
      <td>1.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1952.0</th>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1953.0</th>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1954.0</th>
      <td>NaN</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1955.0</th>
      <td>2.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1956.0</th>
      <td>2.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1957.0</th>
      <td>2.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1958.0</th>
      <td>2.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1959.0</th>
      <td>7.0</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th>1960.0</th>
      <td>4.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>1961.0</th>
      <td>7.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>1962.0</th>
      <td>5.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>1963.0</th>
      <td>6.0</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>1964.0</th>
      <td>6.0</td>
      <td>23.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1984.0</th>
      <td>40.0</td>
      <td>98.0</td>
    </tr>
    <tr>
      <th>1985.0</th>
      <td>39.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>1986.0</th>
      <td>37.0</td>
      <td>92.0</td>
    </tr>
    <tr>
      <th>1987.0</th>
      <td>87.0</td>
      <td>163.0</td>
    </tr>
    <tr>
      <th>1988.0</th>
      <td>88.0</td>
      <td>193.0</td>
    </tr>
    <tr>
      <th>1989.0</th>
      <td>103.0</td>
      <td>156.0</td>
    </tr>
    <tr>
      <th>1990.0</th>
      <td>53.0</td>
      <td>120.0</td>
    </tr>
    <tr>
      <th>1991.0</th>
      <td>49.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>1992.0</th>
      <td>41.0</td>
      <td>134.0</td>
    </tr>
    <tr>
      <th>1993.0</th>
      <td>56.0</td>
      <td>148.0</td>
    </tr>
    <tr>
      <th>1994.0</th>
      <td>70.0</td>
      <td>153.0</td>
    </tr>
    <tr>
      <th>1995.0</th>
      <td>40.0</td>
      <td>129.0</td>
    </tr>
    <tr>
      <th>1996.0</th>
      <td>62.0</td>
      <td>125.0</td>
    </tr>
    <tr>
      <th>1997.0</th>
      <td>51.0</td>
      <td>136.0</td>
    </tr>
    <tr>
      <th>1998.0</th>
      <td>45.0</td>
      <td>92.0</td>
    </tr>
    <tr>
      <th>1999.0</th>
      <td>61.0</td>
      <td>116.0</td>
    </tr>
    <tr>
      <th>2000.0</th>
      <td>46.0</td>
      <td>102.0</td>
    </tr>
    <tr>
      <th>2001.0</th>
      <td>42.0</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>2002.0</th>
      <td>41.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>2003.0</th>
      <td>32.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>2004.0</th>
      <td>31.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>2005.0</th>
      <td>54.0</td>
      <td>102.0</td>
    </tr>
    <tr>
      <th>2006.0</th>
      <td>96.0</td>
      <td>198.0</td>
    </tr>
    <tr>
      <th>2007.0</th>
      <td>58.0</td>
      <td>124.0</td>
    </tr>
    <tr>
      <th>2008.0</th>
      <td>74.0</td>
      <td>133.0</td>
    </tr>
    <tr>
      <th>2009.0</th>
      <td>70.0</td>
      <td>152.0</td>
    </tr>
    <tr>
      <th>2010.0</th>
      <td>74.0</td>
      <td>199.0</td>
    </tr>
    <tr>
      <th>2011.0</th>
      <td>51.0</td>
      <td>98.0</td>
    </tr>
    <tr>
      <th>2012.0</th>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2013.0</th>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>79 rows × 2 columns</p>
</div>




```
fig, ax = pyplot.subplots(figsize=(15,15))

dc_hm = sns.heatmap(dcpivot,  cmap="YlGnBu", ax=ax)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f2c2408f908>




![png](FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_files/FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_18_1.png)


#Comparison of Marvel and DC


```
fig, (ax1, ax2) = pyplot.subplots(1,2,figsize=(15,15))

dc_hm = sns.heatmap(dcpivot,  cmap="YlGnBu", ax=ax1)
marvel_hm = sns.heatmap(marvelpivot,  cmap="YlGnBu", ax=ax2)

ax1.title.set_text('DC')
ax2.title.set_text('MARVEL')
```


![png](FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_files/FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_20_0.png)


Although the data may show that Marvel has had more women than Marvel the bar on the righthand side shows that Marvel has more characters in total than DC. The highest appearance of male characters in Marvel is around 400 but in DC it is only near 200. Note that the closer to the present day there are more women, also that near the beginning of the creation of Marvel there isn't even data on new female characters.

# Appearance distribution by type


```
g = sns.FacetGrid(marvel, row="ALIGN",
                  height=2, aspect=4,)
g.map(sns.distplot, "Year", hist=False, rug=True);

```


![png](FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_files/FINAL%20ALDANA%20_%20comic_book_characters_FATIMA_23_0.png)

