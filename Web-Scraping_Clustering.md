
First of all, I have to import the libraries


```python
import pandas as pd
from bs4 import BeautifulSoup
import requests
```

Now, I am fetching the contents of the website using **BeautifulSoup** package.


```python
source = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M').text
soup = BeautifulSoup(source, 'lxml')
#print(soup.prettify())
table= soup.find('table', class_='wikitable sortable')
table_rows = table.find_all('tr')
#print(table_rows)
list = []
for tr in table_rows:
    td= tr.find_all('td')
    row = [pr.text for pr in  td ]
    list.append(row)
```

Now, I am taking only those rows that have an Assigned "Bouorugh"


```python
df = pd.DataFrame(list, columns=['PostalCode', 'Borough', 'Neighborhood'])[1:]
df = df[df['Borough'] != 'Not assigned']
df.head()


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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods\n</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village\n</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront\n</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park\n</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights\n</td>
    </tr>
  </tbody>
</table>
</div>



Now, I am separating the Postal Codes which have no duplicates from those which have duplicates.


```python
bool_series = df["PostalCode"].duplicated(keep= False ) 
df1=df[-bool_series]  
print(df1.head(10)) 
df2= df[bool_series]
print(df2.head(10)) 
  
# display data 
#df2= df[bool_series]
#df2= df2.pivot_table(index='PostalCode', aggfunc= False)
#df2

```

       PostalCode           Borough          Neighborhood
    3         M3A        North York           Parkwoods\n
    4         M4A        North York    Victoria Village\n
    9         M7A      Queen's Park        Not assigned\n
    11        M9A         Etobicoke    Islington Avenue\n
    15        M3B        North York     Don Mills North\n
    20        M6B        North York           Glencairn\n
    34        M4C         East York    Woodbine Heights\n
    35        M5C  Downtown Toronto      St. James Town\n
    36        M6C              York  Humewood-Cedarvale\n
    48        M4E      East Toronto         The Beaches\n
       PostalCode           Borough        Neighborhood
    5         M5A  Downtown Toronto      Harbourfront\n
    6         M5A  Downtown Toronto       Regent Park\n
    7         M6A        North York  Lawrence Heights\n
    8         M6A        North York    Lawrence Manor\n
    12        M1B       Scarborough             Rouge\n
    13        M1B       Scarborough           Malvern\n
    16        M4B         East York  Woodbine Gardens\n
    17        M4B         East York     Parkview Hill\n
    18        M5B  Downtown Toronto           Ryerson\n
    19        M5B  Downtown Toronto   Garden District\n



```python
import numpy as np  #Importing numpy for working with NaN values
```

Now, I have repeatedly applied 'duplicated' function to get the dataframes with no duplicates of Postal Code.
I could have written a function of my own, but since here I had to apply 'duplicated' function only a few times so, I refrained myself from writing a function of my own.


```python
bool1= df2.duplicated('PostalCode', keep='last')
df3= df2[bool1]
df4 = df2[-bool1]
bool2 = df3.duplicated('PostalCode', keep='last')
df5 = df3[bool2]
df6 = df3[-bool2]
#print(df4)
#print(df5)
bool3 = df5.duplicated('PostalCode', keep='last')
df7= df5[bool3]
df8 = df5[-bool3]
bool4= df7.duplicated('PostalCode', keep='last')
df9 = df7[bool4]
df10 = df7[-bool4]
bool5= df9.duplicated('PostalCode', keep='last')
df11 = df9[bool5]
df12 = df9[-bool5]
bool6= df11.duplicated('PostalCode', keep='last')
df13 = df11[bool6]
df14 = df11[-bool6]
bool7= df13.duplicated('PostalCode', keep='last')
df15 = df13[bool7]
df16 = df13[-bool7]
bool8= df15.duplicated('PostalCode', keep='last')
df17 = df15[bool8]
df18 = df15[-bool8]
final1 = pd.merge(df18, df16, on= ['PostalCode', 'Borough'],how='outer')
final2 = pd.merge(final1, df16, on= ['PostalCode', 'Borough'],how='outer')
final3 = pd.merge(final2, df14, on= ['PostalCode', 'Borough'],how='outer')
final4=  pd.merge(final3, df12, on= ['PostalCode', 'Borough'],how='outer')
final5=  pd.merge(final4, df10, on= ['PostalCode', 'Borough'],how='outer')
final6=  pd.merge(final5, df8, on= ['PostalCode', 'Borough'],how='outer')
final7=  pd.merge(final6, df6, on= ['PostalCode', 'Borough'],how='outer')
final8=  pd.merge(final7, df4, on= ['PostalCode', 'Borough'],how='outer')
final9=  pd.merge(final8, df1, on= ['PostalCode', 'Borough'],how='outer')
final9 = final9.replace(np.nan, '', regex=True)
columnNumbers = [x for x in range(final9.shape[1])]  

columnNumbers.remove(4) #removing column integer index 0
final9 = final9.iloc[:, columnNumbers]





```


```python
final9
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighborhood_x</th>
      <th>Neighborhood_y</th>
      <th>Neighborhood_y</th>
      <th>Neighborhood_x</th>
      <th>Neighborhood_y</th>
      <th>Neighborhood_x</th>
      <th>Neighborhood_y</th>
      <th>Neighborhood_x</th>
      <th>Neighborhood_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M9V</td>
      <td>Etobicoke</td>
      <td>Albion Gardens\n</td>
      <td>Beaumond Heights\n</td>
      <td>Humbergate\n</td>
      <td>Jamestown\n</td>
      <td>Mount Olive\n</td>
      <td>Silverstone\n</td>
      <td>South Steeles\n</td>
      <td>Thistletown\n</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Humber Bay\n</td>
      <td>King's Mill Park\n</td>
      <td>Kingsway Park South East\n</td>
      <td>Mimico NE\n</td>
      <td>Old Mill South\n</td>
      <td>The Queensway East\n</td>
      <td>Royal York South East\n</td>
      <td>Sunnylea\n</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td>CN Tower\n</td>
      <td>Bathurst Quay\n</td>
      <td>Island airport\n</td>
      <td>Harbourfront West\n</td>
      <td>King and Spadina\n</td>
      <td>Railway Lands\n</td>
      <td>South Niagara\n</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td>Cloverdale\n</td>
      <td>Islington\n</td>
      <td>Martin Grove\n</td>
      <td>Princess Gardens\n</td>
      <td>West Deane Park\n</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td>Deer Park\n</td>
      <td>Forest Hill SE\n</td>
      <td>Rathnelly\n</td>
      <td>South Hill\n</td>
      <td>Summerhill West\n</td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td>Kingsway Park South West\n</td>
      <td>Mimico NW\n</td>
      <td>The Queensway West\n</td>
      <td>Royal York South West\n</td>
      <td>South of Bloor\n</td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>M9C</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Bloordale Gardens\n</td>
      <td>Eringate\n</td>
      <td>Markland Wood\n</td>
      <td>Old Burnhamthorpe\n</td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>M6M</td>
      <td>York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Del Ray\n</td>
      <td>Keelesdale\n</td>
      <td>Mount Dennis\n</td>
      <td>Silverthorn\n</td>
      <td></td>
    </tr>
    <tr>
      <th>8</th>
      <td>M9R</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Kingsview Village\n</td>
      <td>Martin Grove Gardens\n</td>
      <td>Richview Gardens\n</td>
      <td>St. Phillips\n</td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>M1V</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Agincourt North\n</td>
      <td>L'Amoreaux East\n</td>
      <td>Milliken\n</td>
      <td>Steeles East\n</td>
      <td></td>
    </tr>
    <tr>
      <th>10</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Highland Creek\n</td>
      <td>Rouge Hill\n</td>
      <td>Port Union\n</td>
      <td></td>
    </tr>
    <tr>
      <th>11</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Guildwood\n</td>
      <td>Morningside\n</td>
      <td>West Hill\n</td>
      <td></td>
    </tr>
    <tr>
      <th>12</th>
      <td>M3H</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Bathurst Manor\n</td>
      <td>Downsview North\n</td>
      <td>Wilson Heights\n</td>
      <td></td>
    </tr>
    <tr>
      <th>13</th>
      <td>M5H</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Adelaide\n</td>
      <td>King\n</td>
      <td>Richmond\n</td>
      <td></td>
    </tr>
    <tr>
      <th>14</th>
      <td>M2J</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Fairview\n</td>
      <td>Henry Farm\n</td>
      <td>Oriole\n</td>
      <td></td>
    </tr>
    <tr>
      <th>15</th>
      <td>M5J</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Harbourfront East\n</td>
      <td>Toronto Islands\n</td>
      <td>Union Station\n</td>
      <td></td>
    </tr>
    <tr>
      <th>16</th>
      <td>M1K</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>East Birchmount Park\n</td>
      <td>Ionview\n</td>
      <td>Kennedy Park\n</td>
      <td></td>
    </tr>
    <tr>
      <th>17</th>
      <td>M6K</td>
      <td>West Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Brockton\n</td>
      <td>Exhibition Place\n</td>
      <td>Parkdale Village\n</td>
      <td></td>
    </tr>
    <tr>
      <th>18</th>
      <td>M1L</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Clairlea\n</td>
      <td>Golden Mile\n</td>
      <td>Oakridge\n</td>
      <td></td>
    </tr>
    <tr>
      <th>19</th>
      <td>M6L</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Maple Leaf Park\n</td>
      <td>North Park\n</td>
      <td>Upwood Park\n</td>
      <td></td>
    </tr>
    <tr>
      <th>20</th>
      <td>M1M</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Cliffcrest\n</td>
      <td>Cliffside\n</td>
      <td>Scarborough Village West\n</td>
      <td></td>
    </tr>
    <tr>
      <th>21</th>
      <td>M1P</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Dorset Park\n</td>
      <td>Scarborough Town Centre\n</td>
      <td>Wexford Heights\n</td>
      <td></td>
    </tr>
    <tr>
      <th>22</th>
      <td>M5R</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>The Annex\n</td>
      <td>North Midtown\n</td>
      <td>Yorkville\n</td>
      <td></td>
    </tr>
    <tr>
      <th>23</th>
      <td>M1T</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Clarks Corners\n</td>
      <td>Sullivan\n</td>
      <td>Tam O'Shanter\n</td>
      <td></td>
    </tr>
    <tr>
      <th>24</th>
      <td>M5T</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Chinatown\n</td>
      <td>Grange Park\n</td>
      <td>Kensington Market\n</td>
      <td></td>
    </tr>
    <tr>
      <th>25</th>
      <td>M8V</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Humber Bay Shores\n</td>
      <td>Mimico South\n</td>
      <td>New Toronto\n</td>
      <td></td>
    </tr>
    <tr>
      <th>26</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>The Kingsway\n</td>
      <td>Montgomery Road\n</td>
      <td>Old Mill North\n</td>
      <td></td>
    </tr>
    <tr>
      <th>27</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Harbourfront\n</td>
      <td>Regent Park\n</td>
      <td></td>
    </tr>
    <tr>
      <th>28</th>
      <td>M6A</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Lawrence Heights\n</td>
      <td>Lawrence Manor\n</td>
      <td></td>
    </tr>
    <tr>
      <th>29</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Rouge\n</td>
      <td>Malvern\n</td>
      <td></td>
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
    </tr>
    <tr>
      <th>73</th>
      <td>M6G</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Christie\n</td>
    </tr>
    <tr>
      <th>74</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Cedarbrae\n</td>
    </tr>
    <tr>
      <th>75</th>
      <td>M2H</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Hillcrest Village\n</td>
    </tr>
    <tr>
      <th>76</th>
      <td>M4H</td>
      <td>East York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Thorncliffe Park\n</td>
    </tr>
    <tr>
      <th>77</th>
      <td>M1J</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Scarborough Village\n</td>
    </tr>
    <tr>
      <th>78</th>
      <td>M4J</td>
      <td>East York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>East Toronto\n</td>
    </tr>
    <tr>
      <th>79</th>
      <td>M2K</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Bayview Village\n</td>
    </tr>
    <tr>
      <th>80</th>
      <td>M3L</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Downsview West\n</td>
    </tr>
    <tr>
      <th>81</th>
      <td>M9L</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Humber Summit\n</td>
    </tr>
    <tr>
      <th>82</th>
      <td>M3M</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Downsview Central\n</td>
    </tr>
    <tr>
      <th>83</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Studio District\n</td>
    </tr>
    <tr>
      <th>84</th>
      <td>M2N</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Willowdale South\n</td>
    </tr>
    <tr>
      <th>85</th>
      <td>M3N</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Downsview Northwest\n</td>
    </tr>
    <tr>
      <th>86</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Lawrence Park\n</td>
    </tr>
    <tr>
      <th>87</th>
      <td>M5N</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Roselawn\n</td>
    </tr>
    <tr>
      <th>88</th>
      <td>M9N</td>
      <td>York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Weston\n</td>
    </tr>
    <tr>
      <th>89</th>
      <td>M2P</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>York Mills West\n</td>
    </tr>
    <tr>
      <th>90</th>
      <td>M4P</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Davisville North\n</td>
    </tr>
    <tr>
      <th>91</th>
      <td>M9P</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Westmount\n</td>
    </tr>
    <tr>
      <th>92</th>
      <td>M2R</td>
      <td>North York</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Willowdale West\n</td>
    </tr>
    <tr>
      <th>93</th>
      <td>M4R</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>North Toronto West\n</td>
    </tr>
    <tr>
      <th>94</th>
      <td>M7R</td>
      <td>Mississauga</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Canada Post Gateway Processing Centre\n</td>
    </tr>
    <tr>
      <th>95</th>
      <td>M1S</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Agincourt\n</td>
    </tr>
    <tr>
      <th>96</th>
      <td>M4S</td>
      <td>Central Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Davisville\n</td>
    </tr>
    <tr>
      <th>97</th>
      <td>M4W</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Rosedale\n</td>
    </tr>
    <tr>
      <th>98</th>
      <td>M5W</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Stn A PO Boxes 25 The Esplanade\n</td>
    </tr>
    <tr>
      <th>99</th>
      <td>M9W</td>
      <td>Etobicoke</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Northwest\n</td>
    </tr>
    <tr>
      <th>100</th>
      <td>M1X</td>
      <td>Scarborough</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Upper Rouge\n</td>
    </tr>
    <tr>
      <th>101</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Church and Wellesley\n</td>
    </tr>
    <tr>
      <th>102</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>Business reply mail Processing Centre969 Easte...</td>
    </tr>
  </tbody>
</table>
<p>103 rows × 11 columns</p>
</div>



Now, almost all the work is done with, but we have to combine the different Neighborhood columns into a single one.



```python
final9.columns =['PostalCode', 'Borough', 'Neighborhood_a', 'Neighborhood_b',
       'Neighborhood_c', 'Neighborhood_d', 'Neighborhood_e', 'Neighborhood_f',
       'Neighborhood_g', 'Neighborhood_h', 'Neighborhood_i']
```


```python
final9['combined']=final9['Neighborhood_i']+','+final9['Neighborhood_h']+','+final9['Neighborhood_g']+','+final9['Neighborhood_f']+','+final9['Neighborhood_e']+','+final9['Neighborhood_d']+','+final9['Neighborhood_c']+','+final9['Neighborhood_b']+','+final9['Neighborhood_a']
```


```python
final9
final9= final9.loc[:,['PostalCode', 'Borough', 'combined']]
final9.columns=['PostalCode', 'Borough', 'Neighborhood']
final9.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M9V</td>
      <td>Etobicoke</td>
      <td>,Thistletown\n,South Steeles\n,Silverstone\n,M...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>,Sunnylea\n,Royal York South East\n,The Queens...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>,South Niagara\n,Railway Lands\n,King and Spad...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>,West Deane Park\n,Princess Gardens\n,Martin G...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>,Summerhill West\n,South Hill\n,Rathnelly\n,Fo...</td>
    </tr>
  </tbody>
</table>
</div>




```python
def clean(x):
    x=x.replace("\n","").replace(",,,,","").replace(",,,","").replace(",,","")
    return x
final9['Neighborhood']= final9['Neighborhood'].apply(clean)
final9.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M9V</td>
      <td>Etobicoke</td>
      <td>,Thistletown,South Steeles,Silverstone,Mount O...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>,Sunnylea,Royal York South East,The Queensway ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>,West Deane Park,Princess Gardens,Martin Grove...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>,Summerhill West,South Hill,Rathnelly,Forest H...</td>
    </tr>
  </tbody>
</table>
</div>


Here, we get to the desired form of the DataFrame. The only thing I could not do is to remove the 'commas' that appear in the beginning of the values in "Neighburhood'
Now, moving to the 2nd part of the question.


```python
New_file= pd.read_csv("Geospatial_Coordinates.csv")

```


```python
New_file.columns=['PostalCode', 'Latitude', 'Longitude']
```


```python
New_DataFrame= pd.merge(final9,New_file, on=['PostalCode'])
New_DataFrame.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M9V</td>
      <td>Etobicoke</td>
      <td>,Thistletown,South Steeles,Silverstone,Mount O...</td>
      <td>43.739416</td>
      <td>-79.588437</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>,Sunnylea,Royal York South East,The Queensway ...</td>
      <td>43.636258</td>
      <td>-79.498509</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>,West Deane Park,Princess Gardens,Martin Grove...</td>
      <td>43.650943</td>
      <td>-79.554724</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>,Summerhill West,South Hill,Rathnelly,Forest H...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
  </tbody>
</table>
</div>




```python
New_DataFrame = New_DataFrame.loc[:,['Borough','Neighborhood','Latitude','Longitude']]
New_DataFrame.head()
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
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Etobicoke</td>
      <td>,Thistletown,South Steeles,Silverstone,Mount O...</td>
      <td>43.739416</td>
      <td>-79.588437</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Etobicoke</td>
      <td>,Sunnylea,Royal York South East,The Queensway ...</td>
      <td>43.636258</td>
      <td>-79.498509</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Etobicoke</td>
      <td>,West Deane Park,Princess Gardens,Martin Grove...</td>
      <td>43.650943</td>
      <td>-79.554724</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Central Toronto</td>
      <td>,Summerhill West,South Hill,Rathnelly,Forest H...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
  </tbody>
</table>
</div>



Here, We get the solution to the 2nd part of the assignment.

Now, moving on to the 3rd and final part of the assignment. I am taking the entries for Borough 'Toronto'.


```python
ser = [True if "Toronto" in dummy else False for dummy in New_DataFrame['Borough']]
New_DataFrame= New_DataFrame[ser]
New_DataFrame.head()
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
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Central Toronto</td>
      <td>,Summerhill West,South Hill,Rathnelly,Forest H...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Downtown Toronto</td>
      <td>,Richmond,King,Adelaide,</td>
      <td>43.650571</td>
      <td>-79.384568</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Downtown Toronto</td>
      <td>,Union Station,Toronto Islands,Harbourfront East,</td>
      <td>43.640816</td>
      <td>-79.381752</td>
    </tr>
    <tr>
      <th>17</th>
      <td>West Toronto</td>
      <td>,Parkdale Village,Exhibition Place,Brockton,</td>
      <td>43.636847</td>
      <td>-79.428191</td>
    </tr>
  </tbody>
</table>
</div>




```python
!conda install -c conda-forge folium=0.5.0 --yes 
import folium
```

    Solving environment: done
    
    ## Package Plan ##
    
      environment location: /home/jupyterlab/conda
    
      added / updated specs: 
        - folium=0.5.0
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        openssl-1.0.2p             |       h470a237_1         3.1 MB  conda-forge
        numpy-1.14.2               |   py36hdbf6ddf_0         4.0 MB
        vincent-0.4.4              |             py_1          28 KB  conda-forge
        altair-2.3.0               |        py36_1000         534 KB  conda-forge
        branca-0.3.1               |             py_0          25 KB  conda-forge
        certifi-2018.11.29         |        py36_1000         145 KB  conda-forge
        pandas-0.23.4              |   py36hf8a1672_0        27.8 MB  conda-forge
        conda-4.5.11               |        py36_1000         651 KB  conda-forge
        ca-certificates-2018.11.29 |       ha4d7672_0         143 KB  conda-forge
        folium-0.5.0               |             py_0          45 KB  conda-forge
        ------------------------------------------------------------
                                               Total:        36.5 MB
    
    The following NEW packages will be INSTALLED:
    
        altair:          2.3.0-py36_1000       conda-forge
        branca:          0.3.1-py_0            conda-forge
        folium:          0.5.0-py_0            conda-forge
        vincent:         0.4.4-py_1            conda-forge
    
    The following packages will be UPDATED:
    
        ca-certificates: 2018.8.24-ha4d7672_0  conda-forge --> 2018.11.29-ha4d7672_0 conda-forge
        certifi:         2018.8.24-py36_1001   conda-forge --> 2018.11.29-py36_1000  conda-forge
        conda:           4.5.11-py36_0         conda-forge --> 4.5.11-py36_1000      conda-forge
        openssl:         1.0.2p-h470a237_0     conda-forge --> 1.0.2p-h470a237_1     conda-forge
        pandas:          0.23.0-py36h637b7d7_0             --> 0.23.4-py36hf8a1672_0 conda-forge
    
    The following packages will be DOWNGRADED:
    
        numpy:           1.14.3-py36hcd700cb_1             --> 1.14.2-py36hdbf6ddf_0            
    
    
    Downloading and Extracting Packages
    openssl-1.0.2p       | 3.1 MB    | ##################################### | 100% 
    numpy-1.14.2         | 4.0 MB    | ##################################### | 100% 
    vincent-0.4.4        | 28 KB     | ##################################### | 100% 
    altair-2.3.0         | 534 KB    | ##################################### | 100% 
    branca-0.3.1         | 25 KB     | ##################################### | 100% 
    certifi-2018.11.29   | 145 KB    | ##################################### | 100% 
    pandas-0.23.4        | 27.8 MB   | ##################################### | 100% 
    conda-4.5.11         | 651 KB    | ##################################### | 100% 
    ca-certificates-2018 | 143 KB    | ##################################### | 100% 
    folium-0.5.0         | 45 KB     | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done



```python
CLIENT_ID = 'KUN320FISBMCXXD1FSZSMHZDPPXBCR1LPTLL1J0DMFTRCPZN' 
CLIENT_SECRET = 'BC51R2HDZOOHHCVN0XDV0LKOVPV4SHYI1A4D51QC2F15G510' 
VERSION = '20180605' # Foursquare API version
```


```python
def getNearVenues(names, latitudes, longitudes, radius=500):
    
    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
                  
        url ='https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID, 
            CLIENT_SECRET, 
            VERSION, 
            lat, 
            lng, 
            50, 
            100)
            
        results = requests.get(url).json()["response"]['groups'][0]['items']
        
        venues_list.append([(
            name, 
            lat, 
            lng, 
            v['venue']['name'], 
            v['venue']['location']['lat'], 
            v['venue']['location']['lng'],  
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['Neighborhood', 
                  'Neighborhoods centre Latitude', 
                  'Neighborhoods centre Longitude', 
                  'Venue', 
                  'Venue Latitude', 
                  'Venue Longitude', 
                  'Venue Category']
    
    return(nearby_venues)
```


```python
Toronto_data = getNearVenues(names=New_DataFrame['Neighborhood'],
                                   latitudes=New_DataFrame['Latitude'],
                                   longitudes=New_DataFrame['Longitude']
                                  )
```


```python
Toronto_data.head()
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
      <th>Neighborhood</th>
      <th>Neighborhoods centre Latitude</th>
      <th>Neighborhoods centre Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
      <td>Hanlan's Point</td>
      <td>43.628947</td>
      <td>-79.394420</td>
      <td>Performing Arts Venue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>,Richmond,King,Adelaide,</td>
      <td>43.650571</td>
      <td>-79.384568</td>
      <td>Estiatorio Volos</td>
      <td>43.650329</td>
      <td>-79.384533</td>
      <td>Greek Restaurant</td>
    </tr>
    <tr>
      <th>2</th>
      <td>,Richmond,King,Adelaide,</td>
      <td>43.650571</td>
      <td>-79.384568</td>
      <td>Ninki Izakaya</td>
      <td>43.650228</td>
      <td>-79.384863</td>
      <td>Japanese Restaurant</td>
    </tr>
    <tr>
      <th>3</th>
      <td>,Kensington Market,Grange Park,Chinatown,</td>
      <td>43.653206</td>
      <td>-79.400049</td>
      <td>El Rey</td>
      <td>43.652764</td>
      <td>-79.400048</td>
      <td>Cocktail Bar</td>
    </tr>
    <tr>
      <th>4</th>
      <td>,Kensington Market,Grange Park,Chinatown,</td>
      <td>43.653206</td>
      <td>-79.400049</td>
      <td>FIKA Cafe</td>
      <td>43.653560</td>
      <td>-79.400402</td>
      <td>Café</td>
    </tr>
  </tbody>
</table>
</div>



Now, I am checking how many unique categories can be found in the resulting dataframe


```python
print("Number of categories that are unique :" + str(len(Toronto_data['Venue Category'].unique())))
```

    Number of categories that are unique :26



```python
# one hot encoding
Toronto_onehot = pd.get_dummies(Toronto_data[['Venue Category']], prefix="", prefix_sep="")

Toronto_onehot['Neighborhood'] = Toronto_data['Neighborhood'] 

fixed_columns = [Toronto_onehot.columns[-1]] + list(Toronto_onehot.columns[:-1])
Toronto_onehot = Toronto_onehot[fixed_columns]

Toronto_onehot.head()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-27-31550c6967fa> in <module>()
          4 Toronto_onehot['Neighborhood'] = Toronto_data['Neighborhood']
          5 
    ----> 6 fixed_columns = [Toronto_onehot.columns[-1]] + list(Toronto_onehot.columns[:-1])
          7 Toronto_onehot = Toronto_onehot[fixed_columns]
          8 


    TypeError: 'list' object is not callable



```python
Toronto_grouped = Toronto_onehot.groupby('Neighborhood').mean().reset_index()
Toronto_grouped.shape
```




    (11, 27)




```python
def return_most_common_venues(row, num_top_venues):
    row_categories = row.iloc[1:]
    row_categories_sorted = row_categories.sort_values(ascending=False)
    
    return row_categories_sorted.index.values[0:num_top_venues]
```


```python
num_top_venues = 8

indicators = ['st', 'nd', 'rd']

columns = ['Neighborhood']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

neighborhoods_venues_sorted = pd.DataFrame(columns=columns)
neighborhoods_venues_sorted['Neighborhood'] = Toronto_grouped['Neighborhood']

for ind in np.arange(Toronto_grouped.shape[0]):
    neighborhoods_venues_sorted.iloc[ind, 1:] = return_most_common_venues(Toronto_grouped.iloc[ind, :], num_top_venues)

neighborhoods_venues_sorted
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
      <th>Neighborhood</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>,Kensington Market,Grange Park,Chinatown,</td>
      <td>Bistro</td>
      <td>Liquor Store</td>
      <td>Café</td>
      <td>Cocktail Bar</td>
      <td>Cluster Labels</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bookstore</td>
    </tr>
    <tr>
      <th>1</th>
      <td>,Richmond,King,Adelaide,</td>
      <td>Cluster Labels</td>
      <td>Japanese Restaurant</td>
      <td>Greek Restaurant</td>
      <td>Dessert Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
    </tr>
    <tr>
      <th>2</th>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>Cluster Labels</td>
      <td>Performing Arts Venue</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
      <td>Building</td>
    </tr>
    <tr>
      <th>3</th>
      <td>,Toronto Dominion Centre,Design Exchange</td>
      <td>Cluster Labels</td>
      <td>Restaurant</td>
      <td>Coffee Shop</td>
      <td>Deli / Bodega</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
    </tr>
    <tr>
      <th>4</th>
      <td>,Trinity,Little Portugal</td>
      <td>Cluster Labels</td>
      <td>Coffee Shop</td>
      <td>Yoga Studio</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
    </tr>
    <tr>
      <th>5</th>
      <td>,Underground city,First Canadian Place</td>
      <td>Gastropub</td>
      <td>Bakery</td>
      <td>Salad Place</td>
      <td>Building</td>
      <td>Café</td>
      <td>Cupcake Shop</td>
      <td>Flower Shop</td>
      <td>Bistro</td>
    </tr>
    <tr>
      <th>6</th>
      <td>,Victoria Hotel,Commerce Court</td>
      <td>Cluster Labels</td>
      <td>Gym</td>
      <td>Bookstore</td>
      <td>Coffee Shop</td>
      <td>Deli / Bodega</td>
      <td>Art Gallery</td>
      <td>Nightclub</td>
      <td>Cupcake Shop</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Christie</td>
      <td>Cluster Labels</td>
      <td>Nightclub</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
      <td>Building</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Davisville</td>
      <td>Flower Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Coffee Shop</td>
      <td>Dessert Shop</td>
      <td>Cluster Labels</td>
      <td>Bakery</td>
      <td>Bistro</td>
    </tr>
    <tr>
      <th>9</th>
      <td>St. James Town</td>
      <td>Cluster Labels</td>
      <td>Japanese Restaurant</td>
      <td>Coffee Shop</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Stn A PO Boxes 25 The Esplanade</td>
      <td>Hotel</td>
      <td>Steakhouse</td>
      <td>Breakfast Spot</td>
      <td>Cluster Labels</td>
      <td>Dessert Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
    </tr>
  </tbody>
</table>
</div>



Now, I am doing "KMeans-Clustering" on the obtained dataset.


```python
from sklearn.cluster import KMeans
```


```python
kclusters = 5

Toronto_grouped_clustering = Toronto_grouped.drop('Neighborhood', 1)
#print(Toronto_grouped_clustering.shape)

kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(Toronto_grouped_clustering)


kmeans.labels_[0:10]
Toronto_grouped= pd.merge(New_DataFrame,Toronto_grouped, on='Neighborhood', how='right')
```


```python

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
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Art Gallery</th>
      <th>Bakery</th>
      <th>Bistro</th>
      <th>Bookstore</th>
      <th>Breakfast Spot</th>
      <th>Building</th>
      <th>...</th>
      <th>Italian Restaurant</th>
      <th>Japanese Restaurant</th>
      <th>Liquor Store</th>
      <th>Nightclub</th>
      <th>Performing Arts Venue</th>
      <th>Restaurant</th>
      <th>Salad Place</th>
      <th>Steakhouse</th>
      <th>Yoga Studio</th>
      <th>Cluster Labels</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Downtown Toronto</td>
      <td>,Richmond,King,Adelaide,</td>
      <td>43.650571</td>
      <td>-79.384568</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Downtown Toronto</td>
      <td>,Kensington Market,Grange Park,Chinatown,</td>
      <td>43.653206</td>
      <td>-79.400049</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Toronto</td>
      <td>,Trinity,Little Portugal</td>
      <td>43.647927</td>
      <td>-79.419750</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Downtown Toronto</td>
      <td>,Toronto Dominion Centre,Design Exchange</td>
      <td>43.647177</td>
      <td>-79.381576</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.4</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>




```python
Toronto_merged = Toronto_grouped
Toronto_merged['Cluster Labels'] = kmeans.labels_

Toronto_merged = Toronto_merged.join(neighborhoods_venues_sorted.set_index('Neighborhood'), on='Neighborhood', how='left')
Toronto_merged.head() 
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
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Art Gallery</th>
      <th>Bakery</th>
      <th>Bistro</th>
      <th>Bookstore</th>
      <th>Breakfast Spot</th>
      <th>Building</th>
      <th>...</th>
      <th>Yoga Studio</th>
      <th>Cluster Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Downtown Toronto</td>
      <td>,South Niagara,Railway Lands,King and Spadina,...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0</td>
      <td>Cluster Labels</td>
      <td>Performing Arts Venue</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
      <td>Building</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Downtown Toronto</td>
      <td>,Richmond,King,Adelaide,</td>
      <td>43.650571</td>
      <td>-79.384568</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>3</td>
      <td>Cluster Labels</td>
      <td>Japanese Restaurant</td>
      <td>Greek Restaurant</td>
      <td>Dessert Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Downtown Toronto</td>
      <td>,Kensington Market,Grange Park,Chinatown,</td>
      <td>43.653206</td>
      <td>-79.400049</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>4</td>
      <td>Bistro</td>
      <td>Liquor Store</td>
      <td>Café</td>
      <td>Cocktail Bar</td>
      <td>Cluster Labels</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bookstore</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Toronto</td>
      <td>,Trinity,Little Portugal</td>
      <td>43.647927</td>
      <td>-79.419750</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.5</td>
      <td>2</td>
      <td>Cluster Labels</td>
      <td>Coffee Shop</td>
      <td>Yoga Studio</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
      <td>Breakfast Spot</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Downtown Toronto</td>
      <td>,Toronto Dominion Centre,Design Exchange</td>
      <td>43.647177</td>
      <td>-79.381576</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>2</td>
      <td>Cluster Labels</td>
      <td>Restaurant</td>
      <td>Coffee Shop</td>
      <td>Deli / Bodega</td>
      <td>Flower Shop</td>
      <td>Bakery</td>
      <td>Bistro</td>
      <td>Bookstore</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
Toronto_merged.columns
```




    Index(['Neighborhood', 'Art Gallery', 'Bakery', 'Bistro', 'Bookstore',
           'Breakfast Spot', 'Building', 'Café', 'Cocktail Bar', 'Coffee Shop',
           'Cupcake Shop', 'Deli / Bodega', 'Dessert Shop', 'Flower Shop',
           'Gastropub', 'Greek Restaurant', 'Gym', 'Hotel', 'Italian Restaurant',
           'Japanese Restaurant', 'Liquor Store', 'Nightclub',
           'Performing Arts Venue', 'Restaurant', 'Salad Place', 'Steakhouse',
           'Yoga Studio', 'Cluster Labels', '1st Most Common Venue',
           '2nd Most Common Venue', '3rd Most Common Venue',
           '4th Most Common Venue', '5th Most Common Venue',
           '6th Most Common Venue', '7th Most Common Venue',
           '8th Most Common Venue'],
          dtype='object')




```python
import matplotlib.cm as cm
import matplotlib.colors as colors
```


```python
map_clusters = folium.Map(location=[43.6532, -79.3832], zoom_start=11)


x = np.arange(kclusters)
ys = [i+x+(i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]


markers_colors = []
for lat, lon, poi, cluster in zip(Toronto_merged['Latitude'], Toronto_merged['Longitude'], Toronto_merged['Neighborhood'], Toronto_merged['Cluster Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=10,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_clusters)
       
map_clusters
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfYWViY2MyNzcxYWUxNGY5NmEyODMzNWYwYmZjMTNjYzcgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2FlYmNjMjc3MWFlMTRmOTZhMjgzMzVmMGJmYzEzY2M3IiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNyA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNycsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUzMiwtNzkuMzgzMl0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfYzgwZjc4ZTVhMTAwNDQ3MzkwNjk3NzM1Zjk4NTU4MjMgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWViY2MyNzcxYWUxNGY5NmEyODMzNWYwYmZjMTNjYzcpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzcyNzJhYWNiYmVmNjQwN2JhMGQzNzAyOWU3OGIyOWE2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjI4OTQ2NywtNzkuMzk0NDE5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMTAsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWViY2MyNzcxYWUxNGY5NmEyODMzNWYwYmZjMTNjYzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOGM4Y2IzYzdjZDBjNGNmYTgzNDRkOWNkZmI2YTM3OTMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDVkMDQ4ZGM2ZDIyNDUzYzg5ZmEyYjFjMDBjNzU4M2UgPSAkKCc8ZGl2IGlkPSJodG1sX2Q1ZDA0OGRjNmQyMjQ1M2M4OWZhMmIxYzAwYzc1ODNlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4sU291dGggTmlhZ2FyYSxSYWlsd2F5IExhbmRzLEtpbmcgYW5kIFNwYWRpbmEsSGFyYm91cmZyb250IFdlc3QsSXNsYW5kIGFpcnBvcnQsQmF0aHVyc3QgUXVheSxDTiBUb3dlciwgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84YzhjYjNjN2NkMGM0Y2ZhODM0NGQ5Y2RmYjZhMzc5My5zZXRDb250ZW50KGh0bWxfZDVkMDQ4ZGM2ZDIyNDUzYzg5ZmEyYjFjMDBjNzU4M2UpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzI3MmFhY2JiZWY2NDA3YmEwZDM3MDI5ZTc4YjI5YTYuYmluZFBvcHVwKHBvcHVwXzhjOGNiM2M3Y2QwYzRjZmE4MzQ0ZDljZGZiNmEzNzkzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdlMDgzMjgzMTU4YjQ1NDdiNWM2ZjY1NTc2ODkxY2U3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUwNTcxMjAwMDAwMDEsLTc5LjM4NDU2NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDEwLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FlYmNjMjc3MWFlMTRmOTZhMjgzMzVmMGJmYzEzY2M3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RiNmU0MWFjYzE4MDRmMWJiMTU0Yjk3Mzc5Zjc4MGU5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ3Yjk4YzMyYmVkNDQwYzE4YzM2YmYzMmM5YjFjNDEyID0gJCgnPGRpdiBpZD0iaHRtbF80N2I5OGMzMmJlZDQ0MGMxOGMzNmJmMzJjOWIxYzQxMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+LFJpY2htb25kLEtpbmcsQWRlbGFpZGUsIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGI2ZTQxYWNjMTgwNGYxYmIxNTRiOTczNzlmNzgwZTkuc2V0Q29udGVudChodG1sXzQ3Yjk4YzMyYmVkNDQwYzE4YzM2YmYzMmM5YjFjNDEyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdlMDgzMjgzMTU4YjQ1NDdiNWM2ZjY1NTc2ODkxY2U3LmJpbmRQb3B1cChwb3B1cF9kYjZlNDFhY2MxODA0ZjFiYjE1NGI5NzM3OWY3ODBlOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80ZDA4Y2FiZGQxYmM0MGVjOWViYWNkMWExNjE3YmFlZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MzIwNTcsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDEwLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FlYmNjMjc3MWFlMTRmOTZhMjgzMzVmMGJmYzEzY2M3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVhYjBlYmMyZmViMzQ5NzdhOGZjZjM5OGU1YTA3YTU2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhmNjhjYjE1OGJjNzQ3MTA4NmY3Nzc0YTY1M2JkZDRmID0gJCgnPGRpdiBpZD0iaHRtbF84ZjY4Y2IxNThiYzc0NzEwODZmNzc3NGE2NTNiZGQ0ZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+LEtlbnNpbmd0b24gTWFya2V0LEdyYW5nZSBQYXJrLENoaW5hdG93biwgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81YWIwZWJjMmZlYjM0OTc3YThmY2YzOThlNWEwN2E1Ni5zZXRDb250ZW50KGh0bWxfOGY2OGNiMTU4YmM3NDcxMDg2Zjc3NzRhNjUzYmRkNGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNGQwOGNhYmRkMWJjNDBlYzllYmFjZDFhMTYxN2JhZWQuYmluZFBvcHVwKHBvcHVwXzVhYjBlYmMyZmViMzQ5NzdhOGZjZjM5OGU1YTA3YTU2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg3OTVkZmEzNTgwODRhMDBhOGFkNWUwNTFiYmRmODVmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ3OTI2NzAwMDAwMDA2LC03OS40MTk3NDk3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83YjRkZWQxYTc2ZGE0ZGQ0YmE3MDQzYmZhY2U4MGM1MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iYmEyYjQ2OGM1MTI0YTMxODkyYmY3YzcxOTBlMDNiMSA9ICQoJzxkaXYgaWQ9Imh0bWxfYmJhMmI0NjhjNTEyNGEzMTg5MmJmN2M3MTkwZTAzYjEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPixUcmluaXR5LExpdHRsZSBQb3J0dWdhbCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdiNGRlZDFhNzZkYTRkZDRiYTcwNDNiZmFjZTgwYzUzLnNldENvbnRlbnQoaHRtbF9iYmEyYjQ2OGM1MTI0YTMxODkyYmY3YzcxOTBlMDNiMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84Nzk1ZGZhMzU4MDg0YTAwYThhZDVlMDUxYmJkZjg1Zi5iaW5kUG9wdXAocG9wdXBfN2I0ZGVkMWE3NmRhNGRkNGJhNzA0M2JmYWNlODBjNTMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWJhYjg0OGRlMWExNGRhMzhjNjNjOTUzMzZjNGY2YjcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDcxNzY4LC03OS4zODE1NzY0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMTAsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWViY2MyNzcxYWUxNGY5NmEyODMzNWYwYmZjMTNjYzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjhhOTBjZjdkMjM4NDk5Y2FkOTYxM2Y0M2Q5OGNlN2IgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzY2YzBkNjVmYjBhNDM2YzgxNGNhYWEyZjY4MjU2MzYgPSAkKCc8ZGl2IGlkPSJodG1sXzc2NmMwZDY1ZmIwYTQzNmM4MTRjYWFhMmY2ODI1NjM2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4sVG9yb250byBEb21pbmlvbiBDZW50cmUsRGVzaWduIEV4Y2hhbmdlIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYjhhOTBjZjdkMjM4NDk5Y2FkOTYxM2Y0M2Q5OGNlN2Iuc2V0Q29udGVudChodG1sXzc2NmMwZDY1ZmIwYTQzNmM4MTRjYWFhMmY2ODI1NjM2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FiYWI4NDhkZTFhMTRkYTM4YzYzYzk1MzM2YzRmNmI3LmJpbmRQb3B1cChwb3B1cF9iOGE5MGNmN2QyMzg0OTljYWQ5NjEzZjQzZDk4Y2U3Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hNDBhZGViNThmYjU0MjliYWExOTJiNjMxMmRlM2FkNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODE5ODUsLTc5LjM3OTgxNjkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hZGJiNDBmNTg4MmM0NGMwYTQwM2I2NmFmNGI4ZjA5YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jYTE3NmE3NjMxOGI0MWNlYWYzOWE2MTIzYmFiZWIyMCA9ICQoJzxkaXYgaWQ9Imh0bWxfY2ExNzZhNzYzMThiNDFjZWFmMzlhNjEyM2JhYmViMjAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPixWaWN0b3JpYSBIb3RlbCxDb21tZXJjZSBDb3VydCBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2FkYmI0MGY1ODgyYzQ0YzBhNDAzYjY2YWY0YjhmMDliLnNldENvbnRlbnQoaHRtbF9jYTE3NmE3NjMxOGI0MWNlYWYzOWE2MTIzYmFiZWIyMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hNDBhZGViNThmYjU0MjliYWExOTJiNjMxMmRlM2FkNy5iaW5kUG9wdXAocG9wdXBfYWRiYjQwZjU4ODJjNDRjMGE0MDNiNjZhZjRiOGYwOWIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfY2RhYTFhODJmMjliNDc2OGI4YzUyOGRhMWM5MWZmZGYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDg0MjkyLC03OS4zODIyODAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wYzFmNWVhNGQxZDM0YjA4OGQ1YjNjMDY0MTc4OWNjNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zY2UwNDJiNDRmMTM0MDY5YTJhZGNiYzAwMGY0NWUyOCA9ICQoJzxkaXYgaWQ9Imh0bWxfM2NlMDQyYjQ0ZjEzNDA2OWEyYWRjYmMwMDBmNDVlMjgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPixVbmRlcmdyb3VuZCBjaXR5LEZpcnN0IENhbmFkaWFuIFBsYWNlIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGMxZjVlYTRkMWQzNGIwODhkNWIzYzA2NDE3ODljYzYuc2V0Q29udGVudChodG1sXzNjZTA0MmI0NGYxMzQwNjlhMmFkY2JjMDAwZjQ1ZTI4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2NkYWExYTgyZjI5YjQ3NjhiOGM1MjhkYTFjOTFmZmRmLmJpbmRQb3B1cChwb3B1cF8wYzFmNWVhNGQxZDM0YjA4OGQ1YjNjMDY0MTc4OWNjNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lN2QyYzIzOWNhNWE0MzU0YjBhNzEzNDk5ODhjYmMwMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTQ5MzksLTc5LjM3NTQxNzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDEwLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FlYmNjMjc3MWFlMTRmOTZhMjgzMzVmMGJmYzEzY2M3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzgzYzY2NDY3MTk3MjRmZjA5ZDg3ODQ1ODYyZDExOTk1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYwZWY5MTRkNjRkMTQ2MjA4NWMzNDkxYTg2NTk4NWM2ID0gJCgnPGRpdiBpZD0iaHRtbF82MGVmOTE0ZDY0ZDE0NjIwODVjMzQ5MWE4NjU5ODVjNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QuIEphbWVzIFRvd24gQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84M2M2NjQ2NzE5NzI0ZmYwOWQ4Nzg0NTg2MmQxMTk5NS5zZXRDb250ZW50KGh0bWxfNjBlZjkxNGQ2NGQxNDYyMDg1YzM0OTFhODY1OTg1YzYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTdkMmMyMzljYTVhNDM1NGIwYTcxMzQ5OTg4Y2JjMDIuYmluZFBvcHVwKHBvcHVwXzgzYzY2NDY3MTk3MjRmZjA5ZDg3ODQ1ODYyZDExOTk1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZmMjA2MzY3YzBjOTRhNGVhYzEwMjRkNWZkNWUwOTU0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY5NTQyLC03OS40MjI1NjM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hZWJjYzI3NzFhZTE0Zjk2YTI4MzM1ZjBiZmMxM2NjNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jNzFjZjc4MDJlNjY0ODZkOTVlMmY4OWU4MGVhZjZhNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mODg2ODg3Zjc3MjM0MDE1ODI5NWZmZjgxODVkODhmYiA9ICQoJzxkaXYgaWQ9Imh0bWxfZjg4Njg4N2Y3NzIzNDAxNTgyOTVmZmY4MTg1ZDg4ZmIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNocmlzdGllIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYzcxY2Y3ODAyZTY2NDg2ZDk1ZTJmODllODBlYWY2YTYuc2V0Q29udGVudChodG1sX2Y4ODY4ODdmNzcyMzQwMTU4Mjk1ZmZmODE4NWQ4OGZiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZmMjA2MzY3YzBjOTRhNGVhYzEwMjRkNWZkNWUwOTU0LmJpbmRQb3B1cChwb3B1cF9jNzFjZjc4MDJlNjY0ODZkOTVlMmY4OWU4MGVhZjZhNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80YTA0Y2E0ODY5YzU0NGYwODUwMTUxMTQ1ZDA2M2QwMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNDMyNDQsLTc5LjM4ODc5MDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDEwLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FlYmNjMjc3MWFlMTRmOTZhMjgzMzVmMGJmYzEzY2M3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzUwN2VlMDg4ZTRhMjRiMTliN2Y0Yzc2YWVmMzBlYTIxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzI1MGJiMzAxMzA4MDQxYTBhNTU1ODIzMTc5ZGIwZDdmID0gJCgnPGRpdiBpZD0iaHRtbF8yNTBiYjMwMTMwODA0MWEwYTU1NTgyMzE3OWRiMGQ3ZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RGF2aXN2aWxsZSBDbHVzdGVyIDM8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzUwN2VlMDg4ZTRhMjRiMTliN2Y0Yzc2YWVmMzBlYTIxLnNldENvbnRlbnQoaHRtbF8yNTBiYjMwMTMwODA0MWEwYTU1NTgyMzE3OWRiMGQ3Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80YTA0Y2E0ODY5YzU0NGYwODUwMTUxMTQ1ZDA2M2QwMy5iaW5kUG9wdXAocG9wdXBfNTA3ZWUwODhlNGEyNGIxOWI3ZjRjNzZhZWYzMGVhMjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDYxOTkzODAyMGQyNDUyYWI3ODUyZDg1NzMxMjQ3MDIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDY0MzUyLC03OS4zNzQ4NDU5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMTAsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWViY2MyNzcxYWUxNGY5NmEyODMzNWYwYmZjMTNjYzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMWEzMzljZGYwYzk0NGY5ZTgwZjY5MzRmYjVkZDgzNTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTYyMGE4NWNmZmNmNDUzN2FlM2UwMjBjMzhkZTQ2NTcgPSAkKCc8ZGl2IGlkPSJodG1sX2E2MjBhODVjZmZjZjQ1MzdhZTNlMDIwYzM4ZGU0NjU3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdG4gQSBQTyBCb3hlcyAyNSBUaGUgRXNwbGFuYWRlIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWEzMzljZGYwYzk0NGY5ZTgwZjY5MzRmYjVkZDgzNTAuc2V0Q29udGVudChodG1sX2E2MjBhODVjZmZjZjQ1MzdhZTNlMDIwYzM4ZGU0NjU3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA2MTk5MzgwMjBkMjQ1MmFiNzg1MmQ4NTczMTI0NzAyLmJpbmRQb3B1cChwb3B1cF8xYTMzOWNkZjBjOTQ0ZjllODBmNjkzNGZiNWRkODM1MCk7CgogICAgICAgICAgICAKICAgICAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>


