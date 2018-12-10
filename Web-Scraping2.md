
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





    (103,)




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
      <th>Postal Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>




```python
New_file.columns=['PostalCode', 'Latitude', 'Longitude']
```


```python
New_DataFrame= pd.merge(final9,New_file, on=['PostalCode'])
```


```python
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



Here, We get the solution to the 2nd part of the assignment.
