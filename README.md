

```python
%matplotlib notebook
```


```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```


```python
city_path ="./raw_data/city_data.csv"
ride_path ="./raw_data/ride_data.csv"
```


```python
city_df = pd.read_csv(city_path,encoding="UTF-8")
city_df.count()
```




    city            120
    driver_count    120
    type            120
    dtype: int64




```python
ride_df=pd.read_csv(ride_path,encoding="UTF-8")
ride_df.count()
```




    city       2375
    date       2375
    fare       2375
    ride_id    2375
    dtype: int64




```python
merge_uber = pd.merge(ride_df,city_df,how='left',on="city")
merge_uber.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2018-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2018-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
      <td>72</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2018-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
      <td>57</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2018-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2018-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
merge_uber.dtypes
```




    city             object
    date             object
    fare            float64
    ride_id           int64
    driver_count      int64
    type             object
    dtype: object




```python
urbanCity = merge_uber[merge_uber['type']=="Urban"]

urban = urbanCity.groupby("city").agg({"fare":"mean",
                                        "ride_id":"count",
                                        "driver_count":"mean"})


urban = urban.rename(columns={"fare":"Average Fare per City","ride_id":"Total Rides per City","driver_count":"Total Drivers per City"})

urban["Average Fare per City"] = round(urban["Average Fare per City"],2)
urban["City Type"] = "Urban"
urban.head()
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
      <th>Average Fare per City</th>
      <th>Total Rides per City</th>
      <th>Total Drivers per City</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Amandaburgh</th>
      <td>24.64</td>
      <td>18</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Barajasview</th>
      <td>25.33</td>
      <td>22</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Carriemouth</th>
      <td>28.31</td>
      <td>27</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Christopherfurt</th>
      <td>24.50</td>
      <td>27</td>
      <td>41</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Deanville</th>
      <td>25.84</td>
      <td>19</td>
      <td>49</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
suburbanCity = merge_uber[merge_uber['type']=="Suburban"]

suburban = suburbanCity.groupby("city").agg({"fare":"mean",
                                        "ride_id":"count",
                                        "driver_count":"mean"})


suburban = suburban.rename(columns={"fare":"Average Fare per City", 
                                      "ride_id":"Total Rides per City",
                                      "driver_count":"Total Drivers per City"})
suburban["Average Fare per City"] = round(suburban["Average Fare per City"],2)
suburban["City Type"] = "Suburban"
suburban.head()

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
      <th>Average Fare per City</th>
      <th>Total Rides per City</th>
      <th>Total Drivers per City</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Barronchester</th>
      <td>36.42</td>
      <td>16</td>
      <td>11</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Bethanyland</th>
      <td>32.96</td>
      <td>18</td>
      <td>22</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Brandonfort</th>
      <td>35.44</td>
      <td>19</td>
      <td>10</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Colemanland</th>
      <td>30.89</td>
      <td>22</td>
      <td>23</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Davidfurt</th>
      <td>32.00</td>
      <td>17</td>
      <td>23</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>




```python
ruralCity = merge_uber[merge_uber['type']=="Rural"]

rural = ruralCity.groupby("city").agg({"fare":"mean",
                                        "ride_id":"count",
                                        "driver_count":"mean"})

rural = rural.rename(columns={"fare":"Average Fare per City", 
                              "ride_id":"Total Rides per City",
                              "driver_count":"Total Drivers per City"})
rural["Average Fare per City"] = round(rural["Average Fare per City"],2)
rural["City Type"] = "Rural"
rural.head()
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
      <th>Average Fare per City</th>
      <th>Total Rides per City</th>
      <th>Total Drivers per City</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bradshawfurt</th>
      <td>40.06</td>
      <td>10</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Garzaport</th>
      <td>24.12</td>
      <td>3</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Harringtonfort</th>
      <td>33.47</td>
      <td>6</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Jessicaport</th>
      <td>36.01</td>
      <td>6</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Lake Jamie</th>
      <td>34.36</td>
      <td>6</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>




```python
perCityType_App = pd.concat([urban,suburban,rural])
perCityType_App
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
      <th>Average Fare per City</th>
      <th>Total Rides per City</th>
      <th>Total Drivers per City</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Amandaburgh</th>
      <td>24.64</td>
      <td>18</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Barajasview</th>
      <td>25.33</td>
      <td>22</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Carriemouth</th>
      <td>28.31</td>
      <td>27</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Christopherfurt</th>
      <td>24.50</td>
      <td>27</td>
      <td>41</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Deanville</th>
      <td>25.84</td>
      <td>19</td>
      <td>49</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>East Kaylahaven</th>
      <td>23.76</td>
      <td>29</td>
      <td>65</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Erikaland</th>
      <td>24.91</td>
      <td>12</td>
      <td>37</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Grahamburgh</th>
      <td>25.22</td>
      <td>25</td>
      <td>61</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Huntermouth</th>
      <td>28.99</td>
      <td>24</td>
      <td>37</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Hurleymouth</th>
      <td>25.89</td>
      <td>28</td>
      <td>36</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Jerryton</th>
      <td>25.65</td>
      <td>25</td>
      <td>64</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Johnton</th>
      <td>26.79</td>
      <td>21</td>
      <td>27</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Joneschester</th>
      <td>22.29</td>
      <td>25</td>
      <td>39</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Justinberg</th>
      <td>23.69</td>
      <td>30</td>
      <td>39</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Karenberg</th>
      <td>26.34</td>
      <td>17</td>
      <td>22</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Karenside</th>
      <td>27.45</td>
      <td>28</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Lake Danielberg</th>
      <td>24.84</td>
      <td>26</td>
      <td>19</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Lake Jonathanshire</th>
      <td>23.43</td>
      <td>24</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Lake Scottton</th>
      <td>23.81</td>
      <td>24</td>
      <td>58</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Leahton</th>
      <td>21.24</td>
      <td>21</td>
      <td>17</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Liumouth</th>
      <td>26.15</td>
      <td>33</td>
      <td>69</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Loganberg</th>
      <td>25.29</td>
      <td>28</td>
      <td>23</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Martinezhaven</th>
      <td>22.65</td>
      <td>24</td>
      <td>25</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>New Jacobville</th>
      <td>26.77</td>
      <td>18</td>
      <td>50</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>New Kimberlyborough</th>
      <td>22.59</td>
      <td>30</td>
      <td>33</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>New Paulton</th>
      <td>27.82</td>
      <td>19</td>
      <td>44</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>New Paulville</th>
      <td>21.68</td>
      <td>22</td>
      <td>44</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>North Barbara</th>
      <td>23.49</td>
      <td>22</td>
      <td>18</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>North Jasmine</th>
      <td>25.21</td>
      <td>30</td>
      <td>33</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>North Jason</th>
      <td>22.74</td>
      <td>35</td>
      <td>6</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>North Richardhaven</th>
      <td>24.70</td>
      <td>14</td>
      <td>1</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>North Timothy</th>
      <td>31.26</td>
      <td>15</td>
      <td>7</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Port Shane</th>
      <td>31.08</td>
      <td>19</td>
      <td>7</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Rodriguezview</th>
      <td>30.75</td>
      <td>15</td>
      <td>20</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Sotoville</th>
      <td>31.98</td>
      <td>11</td>
      <td>10</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>South Brenda</th>
      <td>33.96</td>
      <td>24</td>
      <td>1</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>South Teresa</th>
      <td>31.22</td>
      <td>22</td>
      <td>21</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Veronicaberg</th>
      <td>32.83</td>
      <td>17</td>
      <td>20</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Victoriaport</th>
      <td>27.78</td>
      <td>14</td>
      <td>16</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>West Hannah</th>
      <td>29.55</td>
      <td>21</td>
      <td>12</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>West Kimmouth</th>
      <td>29.87</td>
      <td>20</td>
      <td>4</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Williamsonville</th>
      <td>31.87</td>
      <td>14</td>
      <td>2</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Bradshawfurt</th>
      <td>40.06</td>
      <td>10</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Garzaport</th>
      <td>24.12</td>
      <td>3</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Harringtonfort</th>
      <td>33.47</td>
      <td>6</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Jessicaport</th>
      <td>36.01</td>
      <td>6</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Lake Jamie</th>
      <td>34.36</td>
      <td>6</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Lake Latoyabury</th>
      <td>26.06</td>
      <td>11</td>
      <td>2</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Michaelberg</th>
      <td>35.00</td>
      <td>12</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>New Ryantown</th>
      <td>43.28</td>
      <td>6</td>
      <td>2</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Newtonview</th>
      <td>36.75</td>
      <td>4</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>North Holly</th>
      <td>29.13</td>
      <td>9</td>
      <td>8</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>North Jaime</th>
      <td>30.80</td>
      <td>8</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Penaborough</th>
      <td>35.25</td>
      <td>5</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Randallchester</th>
      <td>29.74</td>
      <td>5</td>
      <td>9</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>South Jennifer</th>
      <td>35.26</td>
      <td>7</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>South Marychester</th>
      <td>41.87</td>
      <td>8</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>South Saramouth</th>
      <td>36.16</td>
      <td>4</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Taylorhaven</th>
      <td>42.26</td>
      <td>6</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>West Heather</th>
      <td>33.89</td>
      <td>9</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
<p>120 rows × 4 columns</p>
</div>




```python
plt.xlim(0,45)
plt.ylim(19,45)
plt.grid()
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,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BmpbNoEEVl4AUC0yBQDV9PNbm4t65733HioKCzH04ovRPDPTtp50BgmxzbYwiXpD4ruoS35ah6oAyCyZvy+BROLol2ioJQAYDZWd60MAUE17AUA1/RypLS9aR2RX7lRnv0kGUDk8lBuwAwDNDQzKg3FJAwKALnFEhIYhAKgmrACgmn6O1NYZJMQ2R0LKlk7Fd7bIGHYjAoBnSicAGHY4xURFAUA1NwkAqunnSG150Toiu3KnOvtNMoDK4aHcgACgAKByEMVYAwKAag4TAFTTz5HaOoOE2OZISNnSqfjOFhnDbkQAUAAw7OCJ0YoCgGqOEwBU08+R2vKidUR25U519ptkAJXDQ7kBAUABQOUgirEGBADVHCYAqKafI7V1BgmxzZGQsqVT8Z0tMobdiACgAGDYwROjFQUA1RwnAKimnyO15UXriOzKnersN8kAKoeHcgMCgAKAykEUYw0IAKo5TABQTT9HausMEmKbIyFlS6fiO1tkDLsRAUABwLCDJ0YrCgCqOU4AUE0/R2rLi9YR2ZU71dlvkgFUDg/lBgQAwwfAjh074r777jP+cVORL4EE9oYAoFq0CgCq6edIbZ1BQmxzJKRs6VR8Z4uMYTcSywB444034o033jBsT0hIQOvWrTFlyhTju70ZGRlha2L1HEABwLAldrSiAKCa/AKAavo5UltetI7Irtypzn6TDKByeCg3EOsAeOjQIbz22muorKzE1q1bcfPNN2PEiBF4++23w9KGn+PkPVdaWgp+5i7QV04EAMOS2PFKAoBqLhAAVNPPkdo6g4TY5khI2dKp+M4WGcNuJNYBsKCgAJ988skp+++//368/vrryM/Px549e9CpUyesX78e/fr1M67h9cwOLly4EKNHj8aiRYswZswYfPHFF3j44YfxzTffYPbs2WjSpAl+//vfY+XKlSgpKcHZZ5+Nxx57DBdccMGpvgQAww47RysKAKrJLwCopp8jteVF64jsyp3q7DfJACqHh3IDdgFg9t692DhvHuLLylCVloZ+48ejbYcOyuML1ACngL0BcNeuXZg6daoBfwcPHgwJAPv06YN//OMf6Ny5s5H5++6777B582acf/75SE1NNaaan3jiCWzbtg3t27c3hiUAGFH3RqxxAUA1aQUA1fRzpLbOICG2ORJStnQqvrNFxrAbsQMAc/bvx9YZM3Bu69anxvF1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" width="640">



```python
plt.title("Average Fare and Total Rides per City")
plt.xlabel("Total Rides per City")
plt.ylabel("Average Fare per City")
```




    Text(0,0.5,'Average Fare per City')




```python
plt.scatter(urban["Total Rides per City"],urban["Average Fare per City"],s=4*urban['Total Drivers per City'],c='gold',edgecolors ='black',alpha=0.75,label="Urban", linewidths=0.25)
plt.scatter(suburban["Total Rides per City"],suburban["Average Fare per City"],s=4*suburban['Total Drivers per City'],c='lightskyblue',edgecolors ='black',alpha=0.75,label="Suburban",linewidths=0.25)
plt.scatter(rural["Total Rides per City"],rural["Average Fare per City"],s=4*rural['Total Drivers per City'],c='lightcoral',edgecolors ='black', alpha=0.75,label="Rural",linewidths=0.25)
plt.legend(loc = "best")
```




    <matplotlib.legend.Legend at 0x885f7dcb38>




```python


```


```python
percentTotalFares = merge_uber.groupby("type").agg({"fare":"sum"})
percentTotalFares 
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
      <th>fare</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4327.93</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>19356.33</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>39854.38</td>
    </tr>
  </tbody>
</table>
</div>




```python
percentTotalDrivers = perCityType_App.groupby("City Type").agg({"Total Drivers per City":"sum"})
percentTotalDrivers
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
      <th>Total Drivers per City</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>78</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>490</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>2405</td>
    </tr>
  </tbody>
</table>
</div>




```python
TotalFare = percentTotalFares["fare"].sum()
percentTotalFares["% of Total Fare"] = [round((f/TotalFare*100),2)for f in percentTotalFares["fare"]]
```


```python
explode = (0.1,0.1,0.1)
sizes = percentTotalFares["% of Total Fare"]
labels =["Rural","Suburban","Urban"]
colors =["lightcoral","lightskyblue","gold"]

```


```python
fig1, ax1 = plt.subplots()
ax1.pie(sizes,explode=explode,labels=labels, autopct='%1.1f%%',shadow=True,startangle=90,colors=colors)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
plt.title("Percentage of Total Fares by City Type")
plt.grid()
plt.show
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,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" width="640">





    <function matplotlib.pyplot.show>




```python
percentTotalRides = perCityType_App.groupby("City Type").agg({"Total Rides per City":"sum"})
#percentTotalRides = percentTotalRides.rename(columns={"ride_id":"Number of Rides"})
percentTotalRides
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
      <th>Total Rides per City</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
TotalRides = percentTotalRides["Total Rides per City"].sum()
percentTotalRides["% of Total Rides"] = [round((f/TotalRides*100),2)for f in percentTotalRides["Total Rides per City"]]
explode = (0.1, 0, 0)
percentTotalRides
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
      <th>Total Rides per City</th>
      <th>% of Total Rides</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>5.26</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
      <td>26.32</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>68.42</td>
    </tr>
  </tbody>
</table>
</div>




```python
explode = (0.1,0.1,0.1)
ride_sizes = percentTotalRides["% of Total Rides"]
r_labels = ["Rural","Suburban","Urban"]
r_colors =["lightcoral", "lightskyblue", "gold"]

```


```python
fig1, ax2 = plt.subplots()
ax2.pie(ride_sizes,explode=explode,labels=r_labels, autopct='%1.1f%%',shadow=True,startangle=90,colors=r_colors)
ax2.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
plt.title("Percentage of Total Rides by City Type")
plt.show
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAoAAAAHgCAYAAAA10dzkAAAgAElEQVR4XuydCXhU1fnG33NnSUjCvoV9S2YyE7YkCAIhCWs2wLCLOy4oogLKkgSUgIigVeu+1lpr1WoXW7W1rf1bu1htrdZ9KVZR666goggkc/7PF+fSIc0kk2SWOzPveR6e1sxZvu93zsx973c2BSYSIAESIAESIAESIIGkIqCSyls6SwIkQAIkQAIkQAIkAApADgISIAESIAESIAESSDICFIBJ1uF0lwRIgARIgARIgAQoADkGSIAESIAESIAESCDJCFAAJlmH010SIAESIAESIAESoADkGCABEiABEiABEiCBJCNAAZhkHU53SYAESIAESIAESIACkGOABEiABEiABEiABJKMAAVgknU43SUBEiABEiABEiABCkCOARIgARIgARIgARJIMgIUgEnW4XSXBEiABEiABEiABCgAOQZIgARIgARIgARIIMkIUAAmWYfTXRIgARIgARIgARKgAOQYIAESIAESIAESIIEkI0ABmGQdTndJgARIgARIgARIgAKQY4AESIAESIAESIAEkowABWCSdTjdJQESIAESIAESIAEKQI4BEiABEiABEiABEkgyAhSASdbhdJcESIAESIAESIAEKAA5BkiABEiABEiABEggyQhQACZZh9NdEiABEiABEiABEqAA5BggARIgARIgARIggSQjQAGYZB1Od0mABEiABEiABEiAApBjgARIgARIgARIgASSjAAFYJJ1ON0lARIgARIgARIgAQpAjgESIAESIAESIAESSDICFIBJ1uF0lwRIgARIgARIgAQoADkGSIAESIAESIAESCDJCFAAJlmH010SIAESIAESIAESoADkGCABEiABEiABEiCBJCNAAZhkHU53SYAESIAESIAESIACkGOABEiABEiABEiABJKMAAVgknU43SUBEiABEiABEiABCkCOARIgARIgARIgARJIMgIUgEnW4XSXBEiABEiABEiABCgAOQZIgARIgARIgARIIMkIUAAmWYfHmbunAPh+gM0NAD4A8DsAmwD8J878ac5cL4DFAO4A8FYC+GO6MB3ATgAeAGkA5gF4oIl/fwBQHILPWwDUhZDPzHIegM8A3NWGMk2zyjgTe89qpQ7J1zcgz9cAXgFwK4Cbm5QtA/BrABMBPNlKvfcCGAsgpwM+tLWo+PJnAAvbWrAd+bsBkH6qApAFIAXAewAeBXAtgOf9dQr/GwH083/35c8nAegC4Lp2tNtckR0ANoRQ128ASB8ykUBCEKAATIhuTFgnTAG4DMCrADoBKAJQ439YjALwVZx7Lw/b+wFMBSCCKBGS/K58AuB1v1CXPnoNwJ4mzon4lQe5mSr9+c3+Nv/+LgD5F2raBUD+deRh3RYB+DKAWr9xAwGsBTABwAUArgwwuqtfEL8IYF8rziSyABRR+wiA7n5x9zgAEc7DARwLYBaAVAAHAPTx//0fAA75mYlIFM7hEseDAAwI6I8hAIT/FQB+EvD3vf7foVDHIfORgKUJUABaunuS3jhTAB4F4OkAGlsBXAjgBAA/6iAlGwC7/2HTwaraVTwRBaA8TEWwSVTlsjZQCdbfbaiiMWu0BWDTqFkPAG/7I9Tuthrvz5+oAtAB4AW/sJNIqLwYNE1z/ZFSU/A1/TzcArBp/SIsJYp7bhijjO0cBixGApEjQAEYObasueMEggmCCgAPA9gIYLu/mUwAMlUoUSSJGsj0sEyrXgKg3p9nKIA3/cLECeA0APL2P9sfkZBpKRGWMl0pIuZzv/A8P+DNX8qt94vPYQC+APCQ/28fB7gs07kS6ZFpKrFRpkJ3+wXR7f58Tae4zeISARPbZwI4B0ABgF5+UfV7v98SYQtMxwC4GIAIDvH9an+EZTOAwO+5/P8VAJb7834DQOoUn/4dQpcV+jmPByDi+Z9+xtIfkmSqVtoMTOK3sG8ttSYAZSpZ+limzGVK8EMAP/NHDb/0V950Slb+LCJDHurpALYBkOlpsUcEhkSWpX9M+00b2xIBbG7aVKYwBwOQMWWmYFPA0hfr/GPxDb+N0p9Np4BlmlSi30v99ktE6pcAqgF8GtBOqX+MjPRHzT8C8DcAxwM42EInmFPA9wG4CEA2gHf8kTCZhpUk/oi4/x6AVU3qkrEnwkn+LtO4zSWxQabm1wD4bmsDwj8FHzgFLFPnEl0NTBIplL6V8SvjUdgFJrFZvhM3+aOyrTXbkgCUqKB8fyQCKcsMApOMxXEA5HdBlqtI/8hvg0Q7JUI8wv8bIEsjhF9g6un/7ojt8lsmfXG3/7sk/jGRQNgJUACGHSkrDCOBYIJA1g6JwJEHp6y1kh9MecD5/A9zeYhKdEHWCd4DQASVJFMAysNApiev9wu4f/kfoH/155Ef6KcAZPinnH8B4DEAhl8oTPELuScAyHSRiBIRi/Ljv9/flghAEUgiEGWNkYiV0wEs8q97+yOA3v6/iQBZCeAZf1mxX8SkrH+Sh9dL/vrFfhGjMj0m099mhESEhQgYqVO4SERTpiFlbZqUCfye3wJAuF7jX0sp0Sp52Mt03Bi/ncG6UNbryfpLETcS2ZMH09l+oSqi5Mf+B6NEbOVhKCJAHmKS79kQxkVLAlDYi1Cd7BdI0lf5/gek2CN9Ijzkb9JfIlJEZEiSPnnOz1v64v/8SwhEUIlYEsEiU48ifMzUEQEoLwnvA5B+FKFspuYEoLnGTaYaZb2rCAEZTxIpk6lzc5pT+vS3/jEmPsh4lylTiYaLrSKKhLPLH2GTKJkIHhl/8pIjbZ8RMD6b6w6pRxjKC5OIeHnJONkvuAOjYTcAOM7/khS4BEPGlHzX5OVJ2m0u/cC/hk9EUihrXpuuAcz1c5KXPOkzSfK9Fx7yEnOpn4u8dJhJvjPf8bOR6HBrqSUBKHZLHRLdljrNJIzl5VK+S+ZLqQhA4SCiW16MZAmEvHTKC6b0xW3+wvIdl9+bzv6XKXkpMce2fN8kPxMJhJ0ABWDYkbLCMBIwBcHRAGQNkAgfESHyoJT/LxEKEVbyoJPIgjwcZOrNTLIGS36k5e+yTssUgPJglohc4BSTRP7kYSpRN3l4NpfkgSOCcoFf4Jh5RPj93S+GzEiJPNxEgElUxLRJbBbxKWv+zM0FoU4By3dVBGV/fxRBIgUS/ZEkDz8RwbKY3ozwiHgVG0RQmN9z4SjCqenaNIlmiCAWwdbSYngpK6JDIhnmGjYzCigPMYl46QDOEtUKfEi2NjRaEoDir2zKEPEfGF2SDQGmqPihv4FQp4DFdmEj0Vbxa1KAgW0RgH8BsMRfVljKw17Gyhy/YDarbSoAReTJxgdhL8LWTDKuJZImfpgC0GQjEe5fBeSVchKBPNX/vTAjbFKuuenVlvpAfJaXEvm+iAgxk7xYyHpNGXsyvqRu+T6J+JfvniQZbzK2ZUmG/D1YkhcpWccrglbGSmupuU0gwaaA5SVGhL+MD4mKSpL+lRc8YSwzB6Gk1qaAZRzKC5j0k4hPSSL6RGiKEDRnAkQASmRSfgPM6LrYI1FM+a7IWJFIoQh6eVmRiK/0u5lELIpIlHEp3z0mEggrAQrAsOJkZWEmEGyKVNYQyTSMPHglyY++RM/mN2lfoiESPZMHkggzUwBe5f+xDswu0TwRSy2t2ZKpK3OKuenDS6bK5EFpCgERXxIFkkhkYJIfcnkwlPv/2JIAlCiHiFJpUx6+EgUzkzzgJFIpDxiZ/pSpZhFHgUmEsjA0v+cy/SlTUSIWm05f/clff9PpNbM+sx3hKNHKwCSRF3PHrwgHk3M4BaBENsU/2UgRGF0SEScRPok0iq+SWhKAErmSaNZo/+5k0w/pExEQZmqLAAzcBSzlZWyIIBNhGZiaCsA8/7iVaX6JRgcmEQkiqk0BKBFCiXIGblYw88sU74P+aJ282EhEVF4KRJxJv4YSaZO6xGcZxxLBDUymCAtciys7YmVMihCSJN8x8UGmneU7FyxFUgBKm7LzWn4HRFxJRNRcLtJUOLdgYiPzltYAyoYtiSLL0hGJvEskWbjJVK+8kJhJxpT8Vkm/BabVAOQ3SNjJMhH5J1P4sjQhMJlLWeQ7K5FNJhIIKwEKwLDiZGVhJmAKQPlRlR9kmZqSiJ8Iq8AkkTyJKARLMi0j6+NMYSKC5fImmSVKIJG6pj/CgdlkOmZGC+3IQ8Esb64BlIdEYDJ3+pb4/xhMAIrYE1ErD1mxXR4kMt0mfxdxYB6NIg86efjIdLesdwxM5vEW5vdcpstlGjpYkiiFRPeaS2Y7EikVIRmYZDOORN9kfaCI8kgIQBHfcmSIRJqaJnkBkClmibhJCiYARfxJhErEokz3yliSMSUPZOkHidCaqS0CUASPRE5FjIp4ED4iCiU6J5FhMzUVgDKWZExJ2z9t4pREmaQuUwCKkBO+wZJEBUXkSBKBIksAZIzJuknhIbuRzeh0sDrEZ7HX5GjmE+4/908ji/CTZAoricjLi48wEJ7TWrBRPuroFLDU0dImEIleiqCSqes7/dFSeRGUaF0oEUepvzUBKHlkSYFEPIWDGYWWlycR3mYSASj9K8s+ApM5kyD9IzugJWIoa3yDJek7idozkUBYCVAAhhUnKwszgdY2BZjNiSCUqIdsCmkuyTSb/GtJmIQSAZTpX5kiDna8iETizGm3jgpAiVDJQ0YYyEPTTDLNK2LVFIAtRQAlAiUPQvN7LlEEESoSkWhuYbn8TYRmc0nakcibRJWCRQDNacdICEAzAijHxpgbPsROMwIows5c6xlMAIpIkjVcEiULTBJdE6HeXgHYdBOITJeKeBeWsgbQFB4diQCKAJO6mm5wMP2QNagyLgKTvBRJ1E6mF0WENHcWY2D+tkQAZUzJWJdNFzImZH1mc0K26VgyRXh7N4FIfa3tApbPZZzIulRhIpFo2bwRagpFAMqLlKynFXEpLxTCo2nkNNQIoLzcSkS+6aYa014R1vKSx0QCYSVAARhWnKwszARCFYAS2ZI3cZl+anrWXKBJLQkTcw2gRPAkktdcMtdXyVo6WbTdUgpVAEq0Rdbyif1ySLCZZHpIRK08xORIEDNJ5FKiO4GHI4e6BtBcLybT1IEbHkLtNhHJIqBkvZy52UUikiICZPo0kmsA5WgQ2dxhTuebNpvRxxMDDn6WaJRsYGh6yLRM18kUqqy1MpOs2RIhI36ESwBK3Wb0VaYjRbxJ6sgaQFkPJuNcNgcI77YkiUaKuJNIskTDg6WW1gBKZE12XgfuIpapdIlOSURavlvyT9a0tZRCOQZGxLhEGiWy39waQOlH+a7LBqzmkvmdkulmicpJ9Lql34WmdYQiAOVMUhFl8h2ViGvTFzWps6U1gGK7jEXhJd9p6V95cZA+YCKBqBCgAIwKZjbSTgKhCkB5MMnaOhElshNRHujyMJcHkggreYjINGFLAlB24Ekd8sNs7rKUH3kREXKUgzxMJNoka63koSIRKRFe8pCSB4w8BESgmA/7UAWgCCqZepUpP4lSyLEssptQom0SGZAojhz9IRECebBJBFKiDoECsOkuYLFToh6y1k9EWeDaQVkjJaJJ1mvJ1J1MKws/mV6UiFVL04TmLmCJbsnmDhEDIsjk4F5zF7B0dSQigOYuYFlTKUJGBLisoZMNF+Y6K3NTjwhmYSXRT+kHGRciCmXdqOxglb4T0S12iiASP4RTOAWgrFWUfpVpQtldLVHA5nYBSzRV1m+GsgtYRJG8GIj9ci6miAcZe/LSItFpiXBKFEleUGQ9mixpkClg2XEqAlrGqEw5tiQAA3cBy9SkfAfl2B2pV75bgUm+M/K9kmhbc0sQgrUjAkt8kTWO0h/mQdDyXZBIpUxlmwdBNycA5fspyzjkFAARYDKNb+6glzZlrMimD1nOIJsoxP+2pFAEoNQnGz/kuykvG+aaw8B2RABKtFoi6zJO5TsskUOJxJ7pjyBKfnl5kpcr2T0uR+PIBhsRyvJbJGNGZjbkb0wkEFYCFIBhxcnKwkwgVAEozcoaGoniyYNffozlh1eElDwIZepThE5rwkQeSPJDLT/QIookaiBromT9jTm1K4JMHoYScZINI/LwkYegPMREFJnHTIQqAMV2qU/+iQgR8WaeAyhTlfKwlwe6tCNTW2KLPNibXo8m67Rkw4jYJFEEebDK+kGxU456CUxSvzyAJIoiD0uZHpe1e/KAl93WLSXzHEARwVJWpqll7aGIZDO1xjlY/a31t0xDBzsHMHBjiDz4ReiKjbJm0DwHUH7vRKhItEXEsfSVRF+Es6wDDKcAFB9lV6iIejPiGuwcQBE5Ithl3IpoFJ4SBWt6DqAIBJk6lUi0rGmTMSFRKHk5kbEn412m92WMSKRQNhHIbm3pI/FTvgstJfMcQFmPKN8l4ShjW6J8TTepmPWI8JQop0RSZTNKqElEj3kVnPgivolYlu+RvISYEfbmBKB812X6VdYbitAWgRXYd2KDfOdlo5QwFP/bkkIVgCLsJRorglSEYNNkngMox/fIRg4RuPLdlSOUJJobmMQPEXry2yO/A3IzivyGSFmpvy0RzLb4yrxJTIACMIk7n64nNAGJIMjDSR6qEqFjIoFwE5AIuQgaWboQuPs13O20tT55rklUWCJucuRMpJKIZDkvUZZEBB4/ZbZnCkCJuDORgOUIUABarktoEAm0i4DcLCA7DmVDjES3JHIiU7Yi/oKda9iuhlgo6QlIZFGWIUgUWab+JcomO29jnSSKJmsVJYom62TlqKXWop7tsVnO/ZToqETx5MBz8/ihpnVRALaHLstEjQAFYNRQsyESiCgB2dQhB8bKQb6yjkvWRMkapUg8ACPqCCu3PAFzWlaiyxIFk/MmrZDMKXZZkyfT1pE6O0+EnZz9JzufZYlFsOlZCkArjAraEJQABSAHBwmQAAmQAAmQAAkkGQEKwCTrcLpLAiRAAiRAAiRAAhSAHAMkQAJWJCA7IOVIDPnHRAIkQAIkEGYCFIBhBsrqSCBBCJi3iIg7ct6cHBUjB/DKcRbROJKCAjBBBhLdIAESsCYBCkBr9gutIoFYExABKDdIyJmBcvah3FJwOwC5k1Z2frYnye+NnHMo59e1ligAWyPEz0mABEigAwQoADsAj0VJIIEJiACUg7HlgGkzyaHGcuRFz4BDteU2DvNqMskv0UG5ceIPAOSyezmkWHZnyuHGcr9xqf/MNNmlKQdcy+HOcuOJHKQbeFwNBWACDy66RgIkEHsCFICx7wNaQAJWJNBUAMpht3INnog/OWfQvO0jFAEo13XJuWxyy4UcjSE3Xoj4k+uv5Oo7ubJNbq+QW0zMA3UpAK04KmgTCZBAwhCgAEyYrqQjJBBWAiIA5QYDEWgybWtetSXXm13VRgEoUUS5J7mlJHf1yhVgci+uJArAsHYnKyMBEiCBIwlQAHJEkAAJNEdABOAAACsApPkvsZfbH+SOWlnD15YIoET85NBgM8m0r1yhJXXJfcWyxlCuFZMp5vUUgByQJEACJBB5AhSAkWfMFkggHgk0twZQ1vP92X/7g1xYvxtAPoBn/Q7KLSQfNbMGsLt/6tfkcIN/LaBMC+8CsB/AT/zrBldTAMbjcKHNJEAC8UaAAjDeeoz2kkB0CDQnAGVTx6/996DKZo+vAVQC+JXfpJkAfhuCAHwBgFxdd7G/XAaAdwFImxSA0elftkICJJDkBCgAk3wA0H0SCEKgOQEoWZ8G8CSAcwD81X/vsNwN2wvA5QDGhyAAf+6fQpYjZrRfCIq4lGNmKAA5JEmABEggCgQoAKMAmU2QQBwSCCYAjwPwfQBZACRyJ6JtDIDX/Ov3QokAyvpBKSc7gT8BsBPAIv9xMhSAcThYaDIJkED8EaAAjL8+o8UkQAIkQAIkQAIk0CECFIAdwsfCJEACJEACJEACJBB/BCgA46/PaDEJkAAJkAAJkAAJdIgABWCH8LEwCZAACZAACZAACcQfAQrA+OszWkwCJEACJEACJEACHSJAAdghfCxMAiRAAiRAAiRAAvFHgAIw/vqMFpMACZAACZAACZBAhwhQAHYIHwuTAAmQAAmQAAmQQPwRoACMvz6jxSRAAiRAAiRAAiTQIQIUgB3Cx8IkQAJWJ/DR5s0ZTru9t2po6A2levt8vt4K6AOlugCwyz8F2Hzf/q8dSj3abfPmn1rdL9pHAiRAAh0hQAHYEXosSwIkEDMC+pprUvZ89lm2oZQbPt8IKNVHA30U0BtaN4o9//92aqORV3Srq1vbxjLMTgIkQAJxRYACMK66i8aSQPIReK+uLi0dGKmVGqO09mrAjW//yZ3CRgSIUABGACqrJAESsBYBCkBr9QetIYGkJvBpXV0XO1AIpQq01mOg9WgoNSJCQi8YawrApB6FdJ4EkoMABWBy9DO9JAFLEpD1eSlKFWpgKoASAAUAbDE2lgIwxh3A5kmABCJPgAIw8ozZAgmQgJ+ATOemKTUZPt+3gk+po/wbMazEiALQSr1BW0iABCJCgAIwIlhZKQmQgBDQdXX2vUpNgdbTDKBEA+MBOC1OhwLQ4h1E80iABDpOgAKw4wxZAwmQQAABvXmz8wvDmAmtF2pgLoAecQaIAjDOOozmkgAJtJ0ABWDbmbEECZBAMwT2btkyU2t9ktJ6DpTqGseQKADjuPNoOgmQQGgEKABD48RcJEACrRDYW1d3A4AVCQCKAjABOpEukAAJtEyAApAjhARIICwEnl29unJYt24PhaWy2FZCARhb/mydBEggCgQoAKMAmU2QQKISmONy9WrQ2ttgGHmG1rm3LFhwQueUlLbevGE1PBSAVusR2kMCJBB2AhSAYUfKCkkg8QnM9HiybQ0NcwB4lFLdfVrXK+CTDSUl4/L6958Q5wQoAOO8A2k+CZBA6wQoAFtnxBwkQAJNCJS73cdrrU9QwC5ovUcr5ZMshUOGDDhn8uTT4xwYBWCcdyDNJwESaJ0ABWDrjJiDBEigCYFZOTkFhs+3HsAbAA4GfnzbggWrMlJSusUxNArAOO48mk4CJBAaAQrA0DgxFwmQQACBOf37px3KyNgBQNb7/ScQztopU6aPGzSoMI6BUQDGcefRdBIggdAIUACGxom5SIAEmhAodblONJQ6Rmv9YuBHBf37911XUnJWHAOjAIzjzqPpJEACoRGgAAyNE3ORQNIR2Pyidm4ZqY6Y3g2EUOpyjQFQo4A3ARwI/Ozm+fNXdk1N7RWn0CgA47TjaDYJkEDoBCgAQ2fFnCSQ8AS2P6N7G/rQYq1wnIb+sjY/pSyY0yVDh6amOByXKqU6A3g3MN+awsKSCYMHF8cpMArAOO04mk0CJBA6AQrA0FkxJwkkJIH77tO2XSMaypXSywFdDsDud7RBw9GvNl99HMzxMpfrWKXUwqbTwKMyM3ttnDZtZZwCowCM046j2SRAAqEToAAMnRVzkkBCEdj2z68H2H2O0zS0HNsyqFnnFFbW5DnlirdmU2lWVq4yjI1K63e0UvsDM91QVXVWj7S0vnEIjQIwDjuNJpMACbSNAAVg23gxNwnENYE6rQ3nPxvKlG6M9s0GYGvFoT/V5DuLguVZ5PU699XXb4dSPbTWbwfmWzlxYuGUYcOmxyEwCsA47DSaTAIk0DYCFIBt48XcJBCXBC5/Tqcfqj90hlJYBWBoG5zQDUb9oE1j04446iWwfLnbLVPASwG8EPh3V69e3bbOmiXtxVuiAIy3HqO9JEACbSZAAdhmZCxAAvFDQDZ1KBw8V0OtVECPdlmucX5NgfOqYGVnuVw5htabYBjvQeuvA/NdO3fu6b0zMga0q93YFaIAjB17tkwCJBAlAhSAUQLNZkggmgQu/fs3w2EYF0Bhmf+w5nY3r4G/1eY7g97vW1JSYk99772LAfQD8FZgQ2dOmHD01BEjStvdeGwKdkgA6lfwCDS6Q6EBGg2N/yu3pWjshcIeAJ9BY0/j/zf/uwF7YGAP7PhMZeOL2LjNVkmABJKJAAVgMvU2fU14Ajv+eSBX+9SFABaGsL4vZB71tobhF47pJOf9NZvKXa4qDZzYdBp4ULdunS+rqFijgHj6remoAPwAQEc2v3wO4N+N1+wpvAGNN+DDv2HgDeTgHfWtoGQiARIggQ4RiKcf5Q45ysIkkMgELn5u/zB7g20LgOMBGGH3VanamjzHpcHqLR0xIkvZbBcp4CMN7AvM9905c07J7Nx5SNhtilyFsRaALXl2CMBu/x3MLwH4BzT+Dg92KQUdOSSsmQRIINEIUAAmWo/Sn6QicNnfdGa949AmpXEGAGcEnX++Jt8pN380mxYBti/c7i1K68H+6NXhfMuOOmpcaXZ2ZQRtC3fVIQvAkhKkdrGhq80GXX8QPrsN+ifX4GXDQJ9wG9VifQp7ofEMFP4O4Gn48HflbRSKTCRAAiTQLAEKQA4MEohDAnXP6m4p+uB6rbFKKZUWDRcMrb0bClJeCdbWrOzs2YZSsubwiN3AmV26pF1ZWXmBoVT4I5ORcTxkAThnGs4FkA8NrQGtAH3PFTguvRM6Rca0NtUqB3hLhPAJaPwWXvxdKfjaVAMzkwAJJCwBCsCE7Vo6logE6h7T9tRuh1b6fHqzUqp7NH3U0Ftr81M2B2uzLCdnqPb5tijgU+DIjQzfqag4YWC3biOiaW8H2mqLANwGjeEAPoCG0gbUj7+DM9I6ISqivI0+7oHGowB+iwb8Ro3CO20sz+wkQAIJRIACMIE6k64kNoHtzxwqVlpfB4WRMfFU47WaAmdOsLbrAOMpl2uT1toFpXYF5jtp7Ni8Cq93bkzsbnujbRWA/ZXC4UOw770CazulIr3tzUa9xKuNYlDjN/gaf1DjcMQRPlG3hg2SAAlElQAFYFRxszESaDsBubLN8Nm/o4Bj2146vCV8Cnkb85z/DFZrqdstt4ycoYEXZTrUzNcrPT316jlz1toMo7WbR8JrcPtqSxYBGEjnIIDHANwLO37Go2jaN3BYigTiiQAFYDz1Fm1NKgI3P60dn6r6NUnnbNIAACAASURBVIDvQqVUhjWc1ztr8lOqg9lSnpU1UBvGxUrrz7VSewPz7SwrWzqkRw+XNfxo0YpkFICBQL4B8Cso3IMUPKSGQf6biQRIIMEIUAAmWIfSncQgsOPpQ4U+5btVKRV0yjVGnr5Vk+8c1kLbqtTlqlFonKZ+PTDfktGjR80bOXJ+jOxuS7PJLgADWX0JhQcA3I338aiaivq2gGReEiAB6xKgALRu39CyJCRQ97ROS1GHtmvoc5VFd80aGhM3FDifDNY9s7KzZ9oM40yf1i8FTgNnOJ2Om+bPX2c3DIfFu5YCsPkO+hgaP4GB21UOnrZ4H9I8EiCBVghQAHKIkIBFCEjUrwH1dxqGraUIW+yt1bi6psC5OpghpR5PP9TXb1NKfdV47VlA2l5aumh4z57e2DvRogUUgK130FNQuBb7cJ8aBzmcmokESCDOCFAAxlmH0dzEI3DlE7rTfsf+ncqwrbRq1O9I6uq9A3n2QXVKBTtTTpVlZ6+FUvkAXgssOz8317N4zJjFFu9FCsDQO0iuvbsZGjcrL94PvRhzkgAJxJoABWCse4DtJzWBHc8emlR/6OCPbHbH0HgCoaFKavMdjweNArrdUw1gJXy+l3WAUOzkcNhvmT9/ncNmi+StJR1FSQHYdoISBfwJgGuVB39te3GWIAESiDYBCsBoE2d7JACgTmvDeGrfZrvDuSk+on7/02031eQ7VwTrzPKsrN7aMLYDOADgk8B8F5eWzsvu2XO0hQcCBWDHOkduH7kGH+JubhrpGEiWJoFIEqAAjCRd1k0CzRC45AXdt/7LvT9zpqZPimNAHx/43NG/bqoKuiu03O1e7dP6aAXIgcOH02y3O/uEgoLjLOw7BWA4OkfjDShcjBzcpRQawlEl6yABEggfAQrA8LFkTSTQKoGL/vhhqSMl/R67wxnVa9xaNawdGZRWZdUFjt8EK1rqck0xlFqlgVeg9WEB4LTZjFsXLFibYrdb4b7c5synAGzHeGihyOuNQtCNu3kXcXjBsjYS6AgBCsCO0GNZEgiRgEz5Hvy/976b0a3nOUoZifG907ijpsC5LKgAzM3toQ4duhRoPDvu48B8F02bNsebmSmbRKyYKAAj0SsarwDYCg9+rNR/b4mJRFOskwRIoHUCifEgat1P5iCBmBGo/ePH/RzAr1Mzuo6JmRERaFgBe/c7HX23jFRyjVizqczlOhtAMSQKGJBmuVzDTx037sQImBWOKikAw0ExeB0vQmEL3PgphWBkQbN2EmiJAAUgxwcJRJDAugdemtK5z8BfOFI7xf2Ub3OYNHzH1Oan/jKoAMzKmgil1mjDeF1pfXi9oM0w1G0LFlzQyeFIjyD+9lZNAdhecm0r9xyA9cqD37atGHOTAAmEgwAFYDgosg4SaIbAqvv+eU7voa4rbXa71W++6Ej/3VOT7wy6oaM8K6sLbLad0oDW+sPAhmqnTasYnZl5VEcaj1BZCsAIgQ1S7S9hx/kqG29Et1m2RgLJTYACMLn7n95HgsCiRbY1x170/d5DXScqlfBfsa8O+Bx96sapr4NGAbOzlwOYAaVeDsxTMmLE4LMmTAi6hjASXRNinRSAIYIKY7aDULgK9dimRmJfGOtlVSRAAkEIJPzTiT1PAtEkMG/t5X2yS6oe7t5/yLhothvLtpTWS6oLUu4LZkOpyzVeab1WKfUvjf9eG2YAuHXhwjXpTmeXWNrfTNsUgLHrkPegcb7y4sexM4Etk0ByEKAATI5+ppdRILDksh/mZxfMeDC9e8/+UWjOOk0o9fOaPMf8YAaVeL0ZqfX1OwDIVPgR14VtKCqalTdw4ETrONNoCQVg7Dvkd9BYqbz4V+xNoQUkkJgEKAATs1/pVZQJLN125xx3ceVdKemdrRbNigaJb4w0R58NOerLoFHA7OxlCqiAUi8F5pk4eHD/VYWFZ0TDyDa0QQHYBlgRzCq3yOyEHdtVduONMkwkQAJhJEABGEaYrCopCailO+9e4ZlSeaUjtVNKUhKQDR7ASbX5zh8GFYBZWfkwjPUK+DeAI46NuXXBgvM6p6RYaZc0BaC1BvILMHCickN2DTORAAmEiQAFYJhAsprkI+D1LnLmnbJgi6d4zjqbw2FLPgKBHutf1eSnVAZjsGjgwE5fpqfvgNZpAP4TmO+CKVOmHTVo0BQL8aMAtFBn+E2Rl4bNyMFlvE3Eep1Di+KTAAVgfPYbrY4xgawJ5V0mLDr5am/xMScbNhu/R8ChlBRH5vm56rNgXVPudh8PYJ7W+sXAPPkDBvRZX1y8IsZdGtg8BaCFOqOJKX+BHSfzyBjrdhAtix8CfHDFT1/RUosQyJpS3nvSgjNuzimsmKcM2cvKJAS0wvLaPOetwWiUud2jtda1CngTOHJN103z5p3drVOn3hYhSQFokY4IYoYcE3OB8uAWa5tJ60jA2gQoAK3dP7TOYgRGFh0zaNzCU291F5aXWsw0K5jzfzX5zulBI4BZWSnaMC7VWndVSr0TmG/V5MlFE4cMmWoFJ7gL2CK90LoZD6MBp6uR+KD1rMxBAiTQlAAFIMcECYRIwFM0O3tc1Sk3eEvmzgixSLJla/A5HAM2jlJH3PgRCKHc5VoCpRY1nQb29u3b86Lp08+xCDBGAC3SESGY8QmAM5UHPwshL7OQAAkEEKAA5HAggRAIeCfPyR17zHFXj5w+f3oS3O4RApEgWTTOrSlwXtdCFNCrDWOT0vodrdT+wHzXV1Wd2TMtLbP9jYetJAVg2FBGraLvIAfVSqEhai2yIRKIcwIUgHHegTQ/8gRyCisLxlYsuWx06ZJpFH+t8v5LTb6zMFiugoICR+99+y6B1rLeb3dgvpVHHz15yvDhVoiuUgC22s2WzPAogGOVB59a0joaRQIWI0ABaLEOoTnWIpBbPOco7/R5O/MqjitRhsHvS+vdoxvq64dsGp92xBq/wGKl2dkLlFLHAXgh8O/ZPXt23Tpr1moLiGwKwNb72ao53kID5qmR+KdVDaRdJGAVAnygWaUnaIflCEjkL3dG1c6C2SdOo/gLvXu0Uutq8xzfCVaizO12w+e7EIbxPrT+KjDfNXPnntYnI2PgpY89hp2PP35EFX3S0/H6unXNVvvX3btR9+ijeP2TT7D/0CEM6toVp4wbh5UT/3vL3H3PP48tjz6Krw4exIn5+bh41qzDde3eswfzf/hDPLZ8ObqkplIAht7dVsy5HwrLVQ7usqJxtIkErEKAAtAqPUE7LEXAW1IxdsT4GduOXrSi3LDZeNZL23rnHzX5znHBipSUlNhT33tvqwYG+I+EOZz1tPHjJ8zMyioTAfjLl1/GAyeddPgzm2GgV3p6s9U+9/77+NcnnyC3b1+kORx48u23seahh7C9tLRRCH761VfIveoq3FBVhaHdu2Pxj36E66uqUOpyNda38K67cFJ+PuZ6vfKfFIBt629r5ta4Gh9irZqKemsaSKtIILYEKABjy5+tW5CAe8rc0QM9ozeVnLphnt2ZYregiZY3yebzZa0fl/pGMENL3e5jlNai7o6YBh7YvXvG5eXl5+947DH18Kuv4s8r2n8+9An33os0pxO3zJ+Pf7z7Lpbec8/hCOKy++9HXv/+OG/yZNz//PP42Usv4Z6lS01zKQAtP8JCNvAPsGOxysbHIZdgRhJIEgIUgEnS0XQzNAKeksqR3TOHrp95zsULU9MyOoVWirn+h4BWm2oKHJcEI1OelTVCG8ZmoPHB/GVgvqtmzz75jn/8Y+i1TzyBLikpcNrtGDdgAC6aPh1De/QICbZEBBfddRc2TZuGkwoKsHf/foy66io8tGwZBnftipJbbsGVs2cjv39/TLv1Vjx4yikY2LUrBWBIdOMu09uwoUy58ErcWU6DSSCCBCgAIwiXVccXgZFFlZ6Urj3PL1996eL07r27xJf1FrNW48WaAueoYFbVAcZT2dl1WqlhAI6IFJ6Sn19gs9lmf33oELJ69sTH+/bh8j/+sXGK98mVK9EjTa4Tbj55r7gCn3z9Nep9PlSXlGB9cfHhjA++8gpkalnWCC4ePRo1U6di5QMPYGRmJkZnZqL6kUdQ39CARaNHP7j197+fGwrROdOwDRr9lcLbZv57r8DaTqlofq46lEqZJxIEPoNGpfLiyUhUzjpJIB4JUADGY6/R5rATyCkpG+qwp19QvnrHku79h1jlSrKw+xnNCpWhR1aPTXkpWJul2dmVSqlTm04D983I6HTVnDlrDaUOr72UjRt5V1/dOGV7zqRJQd14a8+exk0eT7/7buOmkMsrKrBwVPM69E9vvomLfvc7PHzKKci/5hrctnAh+mZkoPCmmw4eqK8fBOCj1nhRALZGyFKffw0DC5Ubv7aUVTSGBGJEgAIwRuDZrHUIjJ40q0+9w7l6+oq6E/q7RsuDnykMBDT0ttr8lAuDVVXh8QzxNTRsAfAZgC8C832nsvL4gV27ZgX+rerOOzG8R4/GqdtQ0uWPP44fP/88nj733P/JfqC+HkU33YSb58+H3TAgde9av74x36irrvrgnc8/Xw7gwdbaoQBsjZDlPq+HwjLuELZcv9CgGBCgAIwBdDZpHQLuyXM7G7aG845efNYJrkmlOdaxLCEs+VdNvvPbbbbNJ1Xucl2otXZBqV2BWY4bO3bMXK+3yvybCDaJAJ5cUIANJSUhwbns8cfxw2eewQtr1vxP/m2//z3219fjktJSyHrBY37wA7xVXd2Yz3vllR+/98UXIgAfaK0hCsDWCFnycw1grfLgSktaR6NIIEoEKACjBJrNWI9AVnl5ivNrY/mICTOPm7hkxdEWOIDYepA6aJHPh4KN45zPBKumLCenFD7fcg28qAB5MDemFz74oPyGqqqCId272z756qvGNYBPvPUW/nL22RjcrVvjeX7vffFFYwRP0q1/+1vjJg5Xr16N//3Xt99G7SOPYPn48dg0ffoRzb/y0Uc4/t578aezzkK609m4JjD3yitRN3Nm4xTw8ffe21Dv8w0B8J/W3KcAbI2QhT9XuEzlYIOFLaRpJBBRAhSAEcXLyi1LYNEim/ej/Sf1HJR17Kxzt5XYHU6nZW2NZ8O0vrymIOXbudVm0myXa0CDUhfD5/tCK7XXzPLE7t0L7Ybh/vLAAbuc/Tdu4EBsnDoVOX36NGZZ8fOf4+29e/HwsmWN/33zU0/hjqefxu69exundOWsP4kWLisogGH89xhHrTXKbr8dawoLUeZ2H7bokddew9pf/QoH6+tx7Jgxv73miSdKQ8FOARgKJUvn+T5ycAbvELZ0H9G4CBGgAIwQWFZraQIqp6hyXmpa5xPmVH+3OK1LiGeLWNolyxq3uzrPMUwpdTi618RSVeZySRRmNIDXAz9bNGbMyAW5uQti4BnPAYwB9Bg2+TN8gCU8MDqGPcCmY0KAAjAm2NloLAm4p5RPVsq2omLVzkm9h2XLMSRMESSglJpcned4IlgTpS7XDAWsaDoNnOF0Om6aP3+d3TAcETSvuaopAKMM3ALN3Y0cnKgUfBawhSaQQFQIUABGBTMbsQqBkdPKRzQcMi44etGZR7sKy/KsYlci26GBa2vznecF87HC68301ddvA/C1f0fw4azbSksXZvXsmRtlPhSAUQZuieYUboMby5X671pUS9hFI0ggQgQoACMEltVaj8Cowsru9QYuGJo/uXDKiecXKWVw/Eelm/QHB/KcA+qUChZdUeXZ2RdopQoAvBZoUpXXm3Ps2LFLomLmfxuhAIwycAs1d63yIOjLioXspCkk0GECfAB2GCEriAcCBQUFjv0ZmWeldu4x65iaa4qdndI7x4PdiWKjz1DTNo51PBbMn7KcnBKl9Tnw+V7WAUIx1eGw3TJ//jqnzZYSRRYUgFGEbbmmuDvYcl1CgyJDgAIwMlxZq7UIqJwpFfNhqGMrV18+pteQrGxrmZcE1ijcUpPnPDOYp3Ncrl6HgEsBHADwSWC+LTNnVrl79x4TRUoUgFGEbdGmNisPtlrUNppFAmEhQAEYFoysxMoEPCXlR0MbK8dWnjBw9IwFoZ0ibGWH4tO2Tw987sism6rqg0YBs7PPg1KTAbwSmKciJyfrpPz846PoNgVgFGFbuKl1yoPvWNg+mkYCHSJAAdghfCxsdQKjS8oH1vuMDT2HZg8uPWd7uc1uj/aOUqsjipp9CkZFdb496D2sZS5XIbReBcN4FVo3mIY5lDJuW7RobYrd3ilKxlIARgm05ZtROFvl4EbL20kDSaAdBCgA2wGNReKDgNz0Yf9KrbHZU8bPq73+6PQevfrFh+UJa+WdNfnOk4N5VzlqVPeGAwd2aK0blFIfBea7cPr02bl9+8omkWgkCsBoUI6PNnzQmKu8eDg+zKWVJBA6AQrA0FkxZ5wRyCmqnK+A40uWrR8weMzECXFmfiKa+0V6Z0ef87KVrPNrNpW73Su0zzcVSr0cmGHGiBFDT58wIah4DDMsCsAwA43z6r6ExiTlxYtx7gfNJ4EjCFAAckAkJAFPSeVIpXHBQO9RnUtOq56vDB75YoWO1krPq81LeSCoAMzKOlordYE2jNeU1ofXC0r/3b5gwfmdHI6MKPhBARgFyHHWxFuwY7zKxsdxZjfNJYGgBCgAOTgSjsDYkqpuB/XB9TDsIxZcdEtxWtcefRPOyfh16Mc1+c5jWxCAXXxK7VByga/WHwTmq506tXx0v37jo+A6BWAUIMdhE3+BHdNVduNOdSYSiHsCFIBx34V04EgCdUZO8dOnKfgqCo9b03X4UcVTSchSBL622xx91o1RXwWzqtTlOl1pPavpNHDxsGGDVkyceGoUvKEAjALkOG3iTuVBtJYixCkimh0vBCgA46WnaGdIBLzFlVM0cHafoTnfzDrn4hMMm90eUkFmihoBBb20Oj/l3mANlrlcRwFYq4BdGjhk5jMA3LJw4eoMp7NrhI2lAIww4DivvkZ5sCPOfaD5JAAKQA6ChCGQNaW8t0OpjYDRvWrjdUVdevcfmjDOJZQj6hc1+Y6qYC7NGj063fbNNzt8QIoC3gvMt664eGbBgAGTIoyDAjDCgOO8eg2NBcqLn8e5HzQ/yQlQACb5AEgg95W3qGK5hpo1rmqZ01syd3YC+ZZorhxQPkff6nHq86BRwOzsUwBUQqmXAvNMGDSo35opU5ZHGAgFYIQBJ0D1X8HAZOXGcwngC11IUgIUgEna8Ynmdm7xnKO01qszevX7cs6GK0+2O1JSE83HRPJHK5xSm+f8QTCfyrOy8nyGsUEB/wZwMDDfLQsWnNslJaVHBHlQAEYQbgJV/S80IF+NxL4E8omuJBEBCsAk6uxEdTVrQnkXR4pRq6AGzVy5xZ2ZPSpaBwYnKtIo+KUfqclPKQ/WUMnQoamdHI4dWik59uXdwHxrCgunThg8uCiCRlIARhBuglX9A+WBRKuZSCDuCFAAxl2X0eAmBFROUcVSBbVgkPeoT0tOrz6dZ/7FxRip14ccmbUT1KfBrC3Nzj7OMIz5WusjDuAd269f7+qpU8+OoJcUgBGEm3BVK5ygcvCjhPOLDiU8AQrAhO/ixHYwd0q516eM9YDaN7f22opufQYMS2yPE8c7pXFWdYHz5mAeVeTkjPL5fLUa2K2AbwLz3Thv3orunTr1iRANCsAIgU3Qar+ED/kqF7sS1D+6laAEKAATtGOTwS3/Xb/VCsqbO22er2DuSUEPGE4GHnHo4x9q8p1Bz2lc5PU6v6yvv1Rr3U0p9U6gf+dOmjRl8tCh0yLkMwVghMAmcLVP4ytMUuP+e2xRAvtK1xKEAAVg/HVkCYDHAHQHsDcC5r8F4Lv+fxGoPnxVekoqZ2iNMx32lH/P33zz6akZXSO5MSB8hrMmk4DP53MM3DhOvR8MSanbvdgAFjedBvb06dN984wZ50UIJQVghMAmeLVXKA/WJriPdC+BCFAARr8zZdrqYgCyAF6uKNsDNB4lUAfgryGYQwEIYFRhZfd6Q28GVLeJx67sl330jFkhsGMWixHQwKrafOc1wcyqzM72+Axjkwb+A62/Dsx3fVXV8p5paf0i4BIFYASgJkGVGgoVKgePJIGvdDEBCFAARr8T/wTAAaAG3x5xISJwOoDnATwcgjmREoBO/3EbcREB9BTNXgLoJWnde71RVXvdOTz2JYSRY80sf63JdwY92LmgoMDR54svtmml5MVpd6ALZ06cOGnqsGEzI+AWBWAEoCZJlR+hAWPUSBxxj3WS+E4344wABWB0O6ybP+InIu7xZpqWmyveBJAH4J/+z80yslbqDwBMASgHHW8H4PZHEE8H8IK/jEQT5aaFsQFtrAYg/8zbMe4AIHU/BeBcv/iTz0QAfg+AB8BcAF8AuBTAtQF1nQ9gGYDhAD4D8CCA9cDh87DkWASZRl7i/99BAP7sLxN0ui/UrvBMqxiCenUhoA8VL9vgHTJmYiSPBAnVLOZrHwHdgIZhm/I7HSHuAqsqc7nmATghYHw3fjy8R48ul5SWrlEq7D9jFIDt60uW+pbAw8oDHkTP0WB5AmH/5bS8x7E1UO6llSnf2wBUAzjQxJy2CMBXAKwCGt80RQiOBODCt3enhioAFwCN1xntBBqvBZTjNkQAylo6qfNnAEoBXOWfsv6d314RkjJtLXll1+0NAP4PgHk0hwjAW/wiVyKdPgB3AXgWwPEd7AKVM2X2SqX01PQefXZV1Vy7yuZwpnSwThaPIQGt1YbaAsdlwUyoyMlx+Xy+C6HUB9D6q8B8V8+de2rfjAx5wQhnogAMJ81krEthicrBfcnoOn2OHwIUgNHvKxFdtwLoBOAZv0i61z8F3BYBKDtef+w3XwSbHJYrwkt+dEIVgGUABje5aUFEnYjLwEN6xb4uACqC4FoE4EYAvfyfix3fB5AF4A3/30QcXgQgsyPIvSUVY+Ez1mmlPyw+pXrckDETpnSkPpa1AAGNZ2sKnPnBLFkE2Pa5XFt9wED1bYT8cDrtqKPGz8zODnqgdDu9owBsJzgWO0zgA3wDj8qLyEY9YiaBsBCgAAwLxjZXIteUiXCZCEBE2HgAMoUrU7yhTgEPAfB2QMsSXXsAwJY2CMABAJquoRIBeDuArQF1S6RRon7mGXsyHV0LwOsXhhLZFJ/k1gaJ0IgAvB5AekAdMo33UwBGm2n5C8h6sK8yMmuU1t70Hn13V9Vct9rmcMjaRaY4J6ANn7t2bOrrwdwoc7vnQuuTm04DD+jcOf3yOXMuML6NYIcrUQCGi2Ry13Or8iDS91YnN2F63yEC4fzR7JAhSV5YpoRFiIkolLVQEg0RQSepN4CPADRdA9icAJTpXBFuEmmTSOOYAK7rAKxsZg2grBUMTMEEoIhAWfMn7b4K4CZ/BFLWABb61w2aR9OYawBljaGZpB2xr91jLqd49kSl9Wpo/VbJaTUTB49m9C9xvjfqopp8h+yObzZVulzDG76NbH8M4MvATFfOnn1S/y5dwnkAOAVg4gysWHqi4UOxyoVs/GMiAcsRaPfD2HKexLdBsqlCImqylkmOuqgE8Cu/SyIMf9uMAJQNFuYaExFeMgUsGzPkbyv8UUCZbtX+euSqoskhCsCXm0z33gOgq/9vIixlSljW3cnaPkmb/EfbREwASvRvf3q/jVprV3rvzHeqNlzD6F98j/mm1r9ck+/MDeZSHWD81eXarL59CTGXFTRmPyUvL7/M45kTRhwUgGGEmeRVverfFXwwyTnQfQsSoACMbqf0BHC/f4pVjn2RSMY4/w5bOQLmNP9ZgLKR4yz/mrrL/VPETSOAL/k3gXwI4BL/jt9s/3o+2cErn8sGjJ/4p5kluiI7epvuAm4uAihCTuqUKWURoFf7Relv/O1IdFKmhGX3r4hK2SUs08kRE4A5RZWT1LebXt4sObV6EqN/0R240WhN+fTo6nEp5k72/2myzOWSNajyHTkiT9+0tE5XzZ271jCMdi8vaNIYBWA0Ojx52qhTnsalOUwkYCkCFIDR7Q6Jmsk0lhxaPMJ/HqBccSWiUHbd7vcfvyJr8GT69jX/8SrNRQAl4rEDgIg+2ZF7hv9/TY9EQEpUUTaIyNo7qUvWo4QiAKV9icbIUQYiUkXgiQg00xoAMqUsU7x/BBovQr8zUgLQ613kRM+vN2ogO7Vzl7fmb75lDc/9i+7AjUZrWuvttQUpG4O1NWP48MF2u32r1nqPUurzwHyXV1QcN6hbN/kuhCNRAIaDIuswCRyAgbHK3bh0hokELEOAAtAyXUFDghGQ6B80ViuFf09cvGJM9qRZsnGGKcEIaOCN2nyn7BwPllSp273JAHK01v8KzLR0zJjRx+TmykajcCQKwHBQZB3/JaDwJ7hRrNThJTmkQwIxJ0ABGPMuoAEtEZDon+799Sal1Qhls+1atPX281LSOwduLiHARCKgML4mz/n3YC6VZ2XN0oZxpgZeVP9d34puqanO66qq1tkNQ3akdzRRAHaUIMs3R+B45cHdREMCViFAAWiVnqAdzRIIjP6NmrlgWF7lCYuJKqEJXFGT71wbzMOZbnd/O7BNy9IEreVQ9cNpR1nZkqE9euSEgQ4FYBggsor/IbAbdrhV9v9cAEBUJBATAhSAMcHORkMisGiRLeejrzcqH3KUUq8fs+n6U7r06ifH0DAlLoF3qvMcQ5RS5u71pp6qcrd7vdZarjmUda2H06LRo3MXjBy5MAxoKADDAJFVNENAY4PyIuitN2RGAtEkQAEYTdpsq00Ecgtnj/EpvQGGenegd1znaWfUysYWpgQnoHxqSvU4h9wd3Wwqd7unaa1XNp0GznA6HTfOm7fWYbN19HBwCsAEH2MxdE82L41QHnwaQxvYNAk0EqAA5ECwKgHlLa441+dThYZSL8889+K5mSNy86xqLO0KHwGtcH1tnvOcYDVOHzasr8PhuEQB+zWOfJBumzVrQVavXnIvdkcSBWBH6LFsawSuUZ7GI62YSCCmBCgAY4qfpSP8sAAAIABJREFUjQcj4C6aM8yAb7OG+rxTRtevFtTdutZmtztILCkIfDhil2PA4sWqIYi3qszlWqOB8erbW2kOp7kej/u4vDy5J7sjiQKwI/RYtjUCcii0W3kgty4xkUDMCFAAxgw9G26JQE5xxVKl1UIF9UJB1bJ8b8mccN70QPiWJ6Bm1uQ7Hg1mZnlWVjFstnPh872slTJvpEGqw2G7Zd68tU67Xe6mbm+iAGwvOZYLlcAPlKfxznQmEogZAQrAmKFnw8EIjCqs7F6vcIlSsAHq/aoLbzy1c8++ck0eU5IQUMD3qvOdpwdzd3pOTk+7z3epAuTWHLkf+HCqmzHjmJw+fWSTSHsTBWB7ybFcqAR88GGUyoVcu8lEAjEhQAEYE+xstCUCOUVzZin4zoTCS31do7uXrtgSdD0YSSYsgT09fI6+Z45TIvCaTeXZ2edqpQoBvBKYocztHnFKQcEJHSBDAdgBeCwaMoGfKw/mh5ybGUkgzAQoAMMMlNV1jEBJSYn9Q196HRSGKK3eKD5tw/TBoybIQ54pyQgoZcyuzrPLHdnNC0C3e7L2+VbDMF6F1ofXCzqUMm5duPCCVIcjrZ3IKADbCY7F2kigAXlqJP7ZxlLMTgJhIUABGBaMrCRcBHKnlHu1sm3UCv8xDNv+xdvuWOPslN45XPWznjgioHBXTZ7zxGAWVw0d2u2blJQd2ufzKaU+Csy3adq0ypGZmePa6S0FYDvBsVibCfxIedCRaHWbG2QBEjAJUAByLFiKgKe44kRoVSWbP3KKKrOOmn/a8ZYykMZEj4DGlwe+cPSpm6q+CdZoWXb2mQCmQ6kj1lJNy8oasnz8+PYusqcAjF4vJ3tL9WjACDUSbyc7CPoffQIUgNFnzhaDEHBPntvZsDXsgIZDKfVe+ZqdC3oNye7omW7kHdcE9MKa/JSfBnOhIjt7gg+4QBvG60rr+sNvtoahvjd//po0p7M90WMKwLgeM3Fn/FXKg/PjzmoaHPcEKADjvgsTxwFvyexCn9arlMZrjtQ0LN52xzqb3dHRWx0SB1ASeqKgflKd71gUzPW5bnfng1rvgFI2aP1BYL6akpKyMf37T2gHNgrAdkBjkXYT2IdvMEjlYW+7a2BBEmgHAQrAdkBjkYgQUN6iygs09DgF41XvtCp3wdyTOnqgb0QMZaVRJbD/G6ejz5aRal+wVsvd7lO1z1cOpV4KzFM4dOjAcyZNOq0d1lIAtgMai3SIQI3yYEeHamBhEmgjAQrANgJj9sgQGF1SPvBgg9oKZXxpAHtKz9tW1We4d0xkWmOt8URAKX18dV7K3cFsnpWTU2D4fOsVsEt/ey7g4XTbggWrMlJSurXRXwrANgJj9g4TeB8NGKpGQm4JYSKBqBCgAIwKZjbSGoGcworZylDLZPOHctiNYy/54Vq7M6VTa+X4eTIQUA/W5DvmBvN0Tv/+aYcyMnZoIFUB7wXmW1dUNKNg4MDJbaREAdhGYMweBgIKp6kc3B6GmlgFCYREgAIwJEzMFFkCdYan6O9bodVgpfBv15Sy4RMWLA96/EdkbWHtFiRw8IBy9K3LU0HXSJW7XCdBqbla6xcD7R8/YEDm+cXFslO4LYkCsC20mDc8BDRegQe5SkGHp0LWQgItE6AA5AiJOYGckrKhymdsURqfyBTwzLM3V2a6xrT3DLeY+0MDIkLgtJp8Z9DoSEV29tgGpaoV8CaAA4EW3Dx//jldU1N7tsEqCsA2wGLWMBLQmK28CHr4eRhbYlUkAApADoKYE/AWl5drbZwu078wgCXb77rAmZqWEXPDaIBlCGjgd7X5zlnBDCoZOjS1k8OxQysl4+bdwHxrCgtLJgweXNwGZygA2wCLWcNIQOMXyouqMNbIqkggKAEKQA6OWBNQnimVm7SC24DaNSSvsH/RyeefEWujQm3/D7fvxIv/9wA+fus1OFI6YciYo1F23nb0Huo+oordzz2J315/Ed558W+w2R3o5x6DZdc+CEdq88scn7z/Zjx1/83Y8/7uxnr6DPdi+vKNcE8uO1zvQ1eswzMP3glnWgbKV23HmNIlhz97/rf349mHf4STr34gVFesnq9Bw9GvNl99HMzQsuzspVBqIYAXAvOMyszstXHatJVtcJACsA2wmDWsBOrhwACVhSNutglrC6yMBPwEKAA5FGJKwDVt9gB7g96mtf5cwdg76cRVhSMKiqfH1Kg2NH77ytkYU7oYA3ML4Guox2+u24wPd72INT99Ds5O6Y01ifj7/rmzUbJsPTxFlbA5nHj/9efhKZoNuzOl2dZeefwhKJsNPQeNaPz8mQd/iD/deSXOvedv6DsiF/L5z7ataBR4n7z9L/x0y3JU//pNpHfrif1f7sX1J0zC6Tc9gm79BrfBG2tnVcDZ1fnOG4NZOcvjGWk0NGzUwG4FHHF7yI1VVWd1T0vrG6KHFIAhgmK2iBC4QHlwZURqZqUkEECAApDDIaYEPCWVM+DDCqXxIpTSc2qvPalbnwHDYmpUBxrft+djXDJ9AJbf+nsMK5jSWNMNJxUi6+jpmHX2lg7UDGwt6Yvy1TtwVNUyPH7Hd/Deq89i6Y4fNdZ5yYyBOOnqBzAodxx+dvEK9BmWg8ITVnWoPQsW/mNNvjPoVO4ir9e5r75+O5TqobU+4mqtlRMnFk4ZNizUFwsKQAt2fhKZ9ILyYHQS+UtXY0SAAjBG4NlsIwHlKZ69DtB5SqvX7Kmd7Esu+cEGw2a3xyufT97ehSuqvFh13zPIzBqJfZ991CjO5qy/Cs898mN89u6/G6eHZ63ciqF5oZ1O4mtowAuP/gT3X3TatxHA4V68/sRv8Ysdq3DOXU/gs3ffxK1nzsSGh3fhwzdexkPfuQBn3/kXGDZbvGIMZrevwagfvGls2n+CZShzuxcpYEnT3cCuXr26bZ01K1RFTAGYaCMn/vwZpzz4R/yZTYvjiQAFYDz1VoLZ6iqZ08um9aVK628A9Wm8H/+itcYP18zH/i/24szbH2vsrbeffwo3njIFnbr2QMXqnejvHo1nHvoRnrz/Jqy+/1n0GpwdtFc/+NcLuPGUItQf/AbOThlYsv1O5BSWH87/6E1b8eyv7oEjNRUzz9oM95QKXHf8BCyq+x52P/8k/vrj65HerRfmbbqhcdo4IZLG+TUFzquC+TLL5cqxKXWhBv4Drb8OzHftMcec3js9fUAIHCgAQ4DELBElcL3y4JyItsDKk54ABWDSD4HYAcidMnt8g6HXKa1eVUBD8WnVMwaPGh9aWCx2Zgdt+ReXnodX//xrnHX7Y+jad2Bjvt3P/RU3LStuXP9Xeu62w2WvXpwP95RylJ17SdD66g8dxN7338Y3+z7Hi7//GZ7++fdxxm2PNkYAm0siCL/Z9wUK5p6E21dWNkYhX/3jr/DXH9+Ac+9+yoLE2m6SBv5Wm+8Mer9vSUmJPeW997YpIBPAW4EtnDl+/MSpWVlBdxIH5KUAbHvXsER4CeyBHf1U9pFHGoW3CdaW7AQoAJN9BMTQ/5ziiqWGNhYAaDy8d/7mm89I7967fwxNanfTv9y5Gi//4ZdYftvv0SNgCeNn/3kTl89xY/HF30de5fGH6797w3Ew7HYce8mdIbd521ll6DlweGNEr2n66M1Xcefq+Y1TxE//4g7s/udfcNzOe3Bw/1fYPLk7Nv/xE6RmdAm5LStnrLc1DL9wTCc576/ZVO5yVWlADhI/YjfwsG7dOm8vLz9fqVZ/9igArTwAksU2hSUqB/cli7v0M/oEWv0ljL5JbDE5CNQZ3qKnL4HSmdBqd3r3XqnzLrppvVJGXI1JmfZtFH+P/QJn3Pq7/5nSlc93lA1DwTEnH7EJ5JqlR8E9qfSIqGBr/X7bmaXomjkQi7Z874is0sYtp0/HlJPWwFs8B3++62q8+eyfcOIVP2ncEby1uA8uevwjdOrc1itxW7MoNp8rqJrqfMeOYK2XjhiRpWy2zQr4UAP7AvN9d/bsZZldurS2NZoCMDZdy1YDCWj8WnlRQSgkECkCcfWwjRQE1ht9AoePfwH2KK2+8Eydkz3umGXHRd+SjrX4wKXn4rlf34sTr/opeg9xHa4sNaPr4TP+/vyja/DozVux4KKb0c81Bs889EP86YdXYfV9zx4+5kXEnXfqMZh07NmNdfzm2k1wTS5Dt8yBOPDVl3juN/fh8Tsux7LrHkL20TOOMPpvP70N/3rydzj+8h83/v2dF/+O751djlOvexiv/eWRxunjNT95rmOOWqv0czX5zrHBTFoE2L5wu7corUXo/Tsw36njxh01y+Vq7aFKAWit/k5WaxpQjwFqFD5MVgD0O7IEKAAjy5e1ByHgLa6cAq1WQeuX5PiXolMuKBkydnJbbmuwBNuafGezdiysu61xLZ6Z/vD9y/DkfTfh688/Qz/XaJSvuvSIXcA7K7NRMOdEzDjrosYicq7frr89hi8/eR8iJjOzR6H4lLX/I/6+/PTDxmNmVtzxOLoEzJ7//pZt+Ms91yGje28s2no7Bo08yhK8wmWE9mlP7biUV4PVV56VNUcbxilNp4Ezu3RJu3L27LUGWrwFiQIwXB3FejpGQON05cWRIf+O1cjSJHCYAAUgB0NMCHiLKk/1aZQZSr0sBsxZ/93ju/UfnBUTY9ho/BHQektNQUpdMMNnud3DlNZ1CvgUwBeB+a6orDxxQNeuw1twmgIw/kZEolr8S+XBMYnqHP2KLQEKwNjyT8rWCwoKHF+nZ14GoLOCary39did96xzpKSkJSUQOt0eAq/W5Ds9wQrWAcaTLtdF0DoLSu0KzHdSfn5eRU7OXArA9mBnmSgT+BoZ6KUGYX+U22VzSUCAAjAJOtlqLuaUlA1VDfatSumPALWv19DsbuWrd4Z6SK/V3KE9MSKggLHV+c6gixvLXa5yDZyugRcVoE0ze6Wnp149Z85am2EEOymbEcAY9SmbbZbAHOXBQ2RDAuEmQAEYbqKsr1UCOUWVkwyo8831f6PLluSOKVuysNWCzEACAQQ09I7a/JSaYFBmjRgxyLDZtiqtP9dK7Q3Md1lFxdLB3br9d9fOkZVQAHKkWYnArcqD5VYyiLYkBgEKwMTox7jywltcuQhaLTHP/5u6vHbWQO+4iXHlBI2NOQEFvFmd72xpLZ8qz86uhWF4tdb/CjR46Zgxo47JzZ0fxAkKwJj3Lg0IIPA+cjBAqf9GsUmHBMJBgAIwHBRZR5sIeIoqN0IhR2n1hhQ8ZtP1p3Tp1W9ImyphZhIAoH04unacM+g1J7Oys2caSp3VdBq4W2qq87qqqnV2w2ju3mkKQI4uaxEwMF658XdrGUVr4p0ABWC892Cc2T961qz0g984rpCr3xTUx2L+0svuWW93pnSKM1dorgUIKIXvVuc51wQzpdTj6Yf6+m3KMPZB6z2B+baXlS0a3qNHc/fqUQBaoG9pQgABjW3KiwvJhATCSYACMJw0WVerBHIKK1yGYWzW0O8oqG869+6fVrXxunWtFmQGEmiWgHrvQJ59UJ1SviCAVKnbvU5pLQdHvx6YZ/7Ikd7Fo0cvaqYcBSBHm9UIPK88GGM1o2hPfBOgAIzv/os7693FlVNtWq001/+NGD9t8KTjzlkWd47QYMsQUFDF1fmOPwaNArrdUw1gJXy+l3WAUEx3Ou03VlWtd9rtjiZlKQAt07s05DABAwOVG/8hERIIFwEKwHCRZD0hEfAUzT5JKz3H0OolKTBu/qkFnqLZs0MqzEwk0AwBDdxYm+/89g69ZlJ5VlZvbRjbFfCN/vZg6MNp07RpJ4zMzBxBAcihZXkCGscqL76975GJBMJAgAIwDBBZRegEvEWz6zR8wxSMN6XUtOUbSwd4C44OvQbmJIH/IfDxiF2OfosXq4ZgbMpcrjUamKCAI66Pm5GVNen08eNnUgByVMUBgWuVB+fFgZ00MU4IUADGSUclgplZ5eUpjn3GVVBaKRiNF5zzCrhE6FkL+GCo0pqxjt8Gs6QiO7tIG8Z5TaeBHYYx+LaFC5em2O2pAWU5BWyBLqUJ/0PgGeVBAbmQQLgIUACGiyTraZWAa9rsAbZ6fYnS+lMo40spsPiSH6xKSe/crdXCzEACLRBQCt+vznOeGixLaW5uDxw6tEMBhwA07j6XpLUedNGMGQW5ffsGLrCnAORosyKBBjSgmxqJfVY0jjbFHwEKwPjrs7i1OLdw9pgGpTcqpV6TY2AczlTbkp13bVTK4DiM2161huEK2Lvf6ei7ZaQ6GFQEulznKK2LoNTLgQJwpsuVdvpRRx3LCKA1+pJWtEDAhxkqF78nIxIIBwE+eMNBkXWERMA7ZfZ0KKwwdwD3HpbTvWzVdq5pCYkeM7VGQGnf3OqC1AeD5ZuVkzPJaGhYA8N4FVo3rheUCKDdMD7+0dKlshO9j78sI4CtwebnsSJQpzzYEqvG2W5iEaAATKz+tLQ3nqLZS7TSC80dwDwCxtLdFXfGaeDu2nzn8cEMnzF8eFe7w7FTNeo+3bgGVQSgUuqje487rgsAcycxBWDc9X7SGPw75cGspPGWjkaUAAVgRPGy8kAC3qLKtYDKA9B4L+uY8qUjR5cuWkBKJBAmAvtSvnH0OX+S2h+svrLs7OUAZpjTwKYAvPuEE35l+HzmWYIUgGHqEFYTdgJfIgfdlULQHe9hb5EVJiwBCsCE7VqrOVZneIr+fgU00pVS74l1Rx979sTso2fwbdZqXRXP9ii9uCYv5f5gLpS6XOOV1mu1YbyutK43BeCvX3ut9vMtW94GMBAABWA8j4FEt70BeWok/pnobtK/yBOgAIw8Y7YAYPiMGV1TDqRcroFvDKU+EyhTl2+aNdCbP5GASCBcBBTUz6rzHUGjyiVeb0Zqff1OAHYA75sC8JHXX6/5vK7uCg2cTwEYrt5gPREhoHC2ysGNEamblSYVAQrApOru2Dk7uqR84CGfukRDfWRAfSWWlK/ZuaDXkOyRsbOKLScggW+MNEefDTmq8Zih5lJpdvYyBVRAqZeaCMDxGniKAjABR0UiuaRwg8qBXKfJRAIdIkAB2CF8LBwqgZFFlZ4G6M1aqzcMpeQsNlRtumFZ516Zg0Otg/lIIBQCSusTqwtS7goqALOy8pVhbADwhta6r2wCkQig5N9bV/cGgJ93q6tbG0pbc6ZhGzT6KwWZPm5M916BtZ1SkR5KeeYhgXYQeEx5MK0d5ViEBI4gQAHIAREVAt7Cigk+m1pr7gCWRhdtu+Pc1IwuPaJiABtJHgJaP1xTkBL0fulFAwd2+jItTaaBO2mtjSME4ObN26GUkwIweYZLHHr6ofIgMw7tpskWI0ABaLEOSVRzPCWVM7TGmYECcOlld6+3O1M7JarP9CtmBA7B7uhbM1rtCRoFdLlONJQ6xufzfR4oAD+rqxttACdRAMas79hwKAQOoYcajaDjO5QqmIcEKAA5BqJCwFM8e57SkDPaXjQbPP7K+zcZhs0WFQPYSHIRUDijJs95WwsCUK5+qzGUatBav21OAUv+PVu2LOu+efP3QwHGKeBQKDFP2AkoFKoc/CXs9bLCpCJAAZhU3R07Z71Flaf6NMoM/zVcNmeq7bjL7t4UO4vYciITUMDvq/OdM4L5WJ6VleJTaoehVK4G/nGEAKyr69a9rm5vKHwoAEOhxDxhJ6BxhvIi6AtO2NtjhQlJgAIwIbvVek55imevB/QopZUsskdGz8y0eRfesM56ltKiBCHQ4HM4BmwcpRpv/Ggulblccv/vaQCeDhSAbfGfArAttJg3jASuUp7GI4uYSKDdBCgA242OBdtCwFNUuU0pDIBWu6Vcr6HZ3cpX71zVljqYlwTaQkBpnFNd4Lw+WJnSrKxcZRgbAeymAGwLWea1AIFHlAflFrCDJsQxAQrAOO68eDLdU1x5JaA6K43/iN0Dcsf1nXZG7Vnx5ANtjTMCCn+uyXNOCWZ1QUGBo/e+fduhdT0FYJz1Lc19W3kwhBhIoCMEKAA7Qo9lQyOwaJHN8+H+qzW03YD6QAoNH18yePJx5y0LrQLmIoF2EdCGrX7whjFp7waNAmZnL5B1gL9+/fWt7WmBU8DtocYyYSCgcRCd1Rg0HqrPRALtIUAB2B5qLNMmAgMnLuqU4fj6KgU0KKiPpXBOUWXWUfNPk13BTCQQQQJqbU2+44pgDchmkIxdu+rvBxraYwQFYHuosUxYCCjkqBy8Fpa6/r+9+4Czqjr3//959pkZZmDovSg2ygwIIgqxwYCVDir2HjXRqFeNSTQxSoo37Sa55ia5ifGm/v6xEGMXRcoIIkPvvVfpHaaevf6vfWbQESnDMGdO++7Xyxcqe6/1PO+1gYe9115LjaSkgArAlBz22k36vLzhjYpd2S+c45BH+dpVuf2Hdew59M6bazcS9ZZqAg5mfvf8jAujlbcKwGjJqt0qCORZDh9V4TydIoGjCqgA1I0RdYFuF1/VoiSU9lPzbI852xd02LX/iE49ht4efIWpQwJRFQj5/jnfviAz8vV5TR8qAGtaVO1VWcBxk+XySpXP14kSOEJABaBuiagLdO0z7LSwCz/nzG31sMicla5XXpvTY9BtN0S9c3WQ8gJm9r0ne6T/ZzQgVABGQ1VtVknAeNQ683yVztVJEjiKgApA3RZRF+iSN/Qc5/s/ANYDxUGH3a68Pqf7oFtUAEZdXx2AvfnU+enDoyGhAjAaqmqzigI/tRyequK5Ok0CXxJQAaibIuoCnS65ppOXFhrlfFZ7ZqUqAKNOrg4ABx9i3n8/dV5ojJm5aKCoAIyGqtqsosBfLQetpFBFLJ32ZQEVgLoroi7QMW9I55Dzn61cAHa58trO5w+67caod64OUkogXFbqH9q7+50GzZt/98nz6iyKdvIqAKMtrPaPKeAYY7kMlJAEqiugArC6crquygJd+wzKCeOeBVtpWFlwYW7/4Z16Dr1DH4FUWVEnHk+gpOjQgc2LZk6f+fbf9x/cvWvc0snv/LY2xFQA1oay+jiGwBzL4XzpSKC6AioAqyun66oskHvJkC4uFP5+5QIwp9+wDhcMu/OWKjeiEyVwFIGDu3Z8umr2RwXzP3hloSst8x3+mTgr9srKnlw09YNd0UZTARhtYbV/HIFPLYc2EpJAdQVUAFZXTtdVWaDLZQNyfbNnKheAHS++8ozeNzxwZ5Ub0YkSqBBwvu92bV67dNmk9wpWTZ+w3kEIoxXONXXO7THPZpq5fy/Ofy+y60w0DxWA0dRV2ycQKKMzdczwJSWB6gioAKyOmq45KYGjvQI+vdtFrfve8637T6ohnZzSAuHSkuItKxbMmTv21Wm71q7YA9RxjraYqwf2qeFP8kifunDSm8HWb1H56OPIAVABmNK3ZOyTz6CRnc3e2AeiCBJRQAVgIo5agsV8tI9AWnTo2vTqb/zwoQRLReHGQKD44L7d6+cVTJvz3ktzig/sLXHmGhAUfjhnxmoHE4vTS6avHjeu1v8gVAEYgxtCXX4ukE5LO4dtIpFAdQRUAFZHTdeclMDRloFp0Pr07GHf+e9vnlRDOjmlBPZt37x21dTxBQsnvrEc3w8e6TU3sxbBloLgL/DwPnI7s+YuXjy6JFYwKgBjJa9+IwJlnG7nskEaEqiOgArA6qjpmpMSONpC0Bn1stNvfO7v3z2phnRy0gv44XB4x7plCxdNfLNg44IZWxwuzTnXxswa4thpnk31w/6UpR+/t6K2XvMeD10FYNLfkvGdoE8H68LK+A5S0cWrgArAeB2ZJIrraFvBBend9ut/PWPm6R5MorGubiqlxYUHNy+ZPXPeey/P2Ltt00Fn1MV37ZxHujk2ejDR0kPTFo5/a2t1+4jGdSoAo6GqNqss4NPFurC4yufrRAlUEtAfvrodoi7Q7eKrWpSE0n5qnu0xZ/sOd3jLL15+MpSeUSfqAaiDuBU4tHfX1tWzJxcsHPPKgtKSorAPjXGutXmEzdky39nEjKzimfPHjo3sIR1vhwrAeBuRFIvH53zrwpwUy1rp1pCACsAaglQzxxY4L294o2JX9otg7pYHuw+feeNP/vFYRla9BrJLLQHnfLfn0w0rln08pmDFJ2PXOHOeOVo6aGbGPt+3WSHH5OZpBxbl5+dHFg6P10MFYLyOTIrE5bjIcilIkWyVZg0LqACsYVA192WBdheNzMpOP/Rrg7Bh2w+fce0zL9xfr0mz1jJLDYFwWWnJ1lWL5i788LVpW1cu2oX5GZFlXLBsHMGr3Umku6lLJry3Ph7m91VlVFQAVkVJ50RNwNHXcpkUtfbVcFILqABM6uGNk+RGjgzlbC18PpjQ72GfLc476Fu/uqlJ2zM6xUmUCiNKAkUHD+zduGjatLnvvjS7cO+uYpxf3xltnQsmgLo1XoiJXplNW/Dxu589HY5SKDXerArAGidVgycj4LjKcvnwZC7RuRI4LKACUPdCrQjkXjbwl87zGphj0+EOr3zw2UGtOna/oFYCUCe1LrB/x5YNq2ZMKFg47vUlrqwMZzQzrKUzV+icW2Seyy/NYs7KMWOKaz24GuqwcgEYMuz6a8i54RqGp6WRXkNdqBkJHE9giOXwjogkUB0BFYDVUdM1Jy2Q02fQj81oi7N1hy++9LbH+px5wWX9TroxXRC3Ar4f9neuX7VoyUfvFKyb8/Hm4KkvELzmb0Qw/9O5gnAo9PHy/J7LYVTCb2EVFIANs2l/13Ba9u5Or+y6NIzbwVFgyShwneXw72RMTDlFX0AFYPSN1QOQ03fwt8Gda85WHQbpMezO87r2GzZMQIkvUFZSVLh56bxZ8z54dfqeTWv2O+dnObO2HpbpwyYz8vFLC5ZM/uDTxM+2PAO3mA5zlvDPLh04LyOdoNDVIYHaFtATwNoWT6L+VAAm0WDGcyq5fQff7ftugGf22ZpVnfoMPLvXtffeFs9xK7bjCxTu27197dwp0+aOeWVeWeHBMoffyDlrEyzjAqzwYSJloZnLprwl0VJAAAAgAElEQVS1P1ks3SIux+NRYBCg30OTZWATMQ/NAUzEUYubmPWbV9wMRXIHktN38Ahz3AosPJxpm9zzW1x+/9MPJHfmyZedc469WzesXP7J2IJlH7+3yjnnOUcLgxbAPpybB6GPWob2z4/3ZVyqOjpuBXUIcyuO/wC6VfU6nSeBqAr49LEuTI5qH2o8aQVUACbt0MZXYjl5g65wjq95zhYdjqxe42aZ1z77wnfiK1JFcyyBcFlZ6fY1S+bPH/9awdal83eUL+NibYAGDrZ5jo/LHJ8s//jdNYmyjMuJRtstoCVpPAh8nfICV4cE4kfAo5d1Ykb8BKRIEklABWAijVYCx5p76cDefsieqFwABunc/PN/fjstIzMrgVNL+tBLCg/u37BwxvS5Y16adWjX9kJw2ZFlXHxCnrHeYRNcevq0peNf35ksGG4h5xHiMeAmICNZ8lIeSSZgdLfOzE+yrJROLQmoAKwl6FTvpmufQTlh3LPO2SrPrPSwx/Cnf39P/WatTkt1n3jM/8CubZtWz/yoYP6Hoxe7klKH0QQs+KK32OEW+87PP1iWPXvj1NGF8Rj/ycbkHB5LGIpF5vf1Pdnrdb4Eal0gxNnWkdW13q86TAoBFYBJMYzxn0S3vAHtSn17zmHbPOyzfV2vfPhHQ1ud3aVH/GeQGhE633e7Nq5esmTyewVrZuRvcBDCaIVzTYA9hptOOPTx4ik9lyTDMi7BqLql1AfuwfEIcFZqjLSyTAoBj+bWiR1JkYuSqHUBFYC1Tp6aHZ51xRUN6xTX+YWDIs9s12GFi25+6OJzeve/MjVV4ifrstLiok+XzZ+94MNXp+9ct2qvw2USWbeRugabzdlHpelMXT7hnc8W8o6f6KsXiVvKmcAjOO4J5jFWrxVdJYEYCqSRaR1I2IXUYyinrrWEge6B2hMY5eX0mfFLzLIr7waS239Yx55D77y59uJQT5UFig7s2bluXsG0ue/9c27JwQOlDhcsZNwGnAte11uIiaWF/vSV08bsSxY5t5g+Fa95gzUovWTJS3mknECJ5VAn5bJWwjUmoCeANUaphk4kkNtn0BNgweveFYfPbXlOlyZXPfSjh090rX6+ZgX2bN20ZtW0cQWL899cTtiZM9fC8Jo7/IOGLQh7Lj97/9Z5s2bN+my+Zs1GULutuYVk4HFjReF3fu32rt4kEBWBnZZDs6i0rEZTQkAFYEoMc3wkmdNn8I3O3PWVvwS2UMhu+fnL3/NCoVB8RJm8UfjhsrJta5cvWDLhjYKNi2Zu851L9zzaOEcDgx0+fBLyQlMW5b8V7NbikkHCLaMZYR7ACNabDD5g0SGBZBFYZjl0TpZklEftC6gArH3zlO0x97LBl1f8QfzZYtABxnU/fPGBug2aaI21KN0ZpUWFBzYunjlj3phXZu7fvvmQj6vnYW18cxkGGxw2scwPF6ycPGZ7lEKo9WbdYrpGdusoX3w8s9YDUIcSiLaAMd46c0W0u1H7ySugAjB5xzbuMuty6eDuYXPfM7NlRmSrsMhx9SM/Ht7irNzucRdwggd0cM/OLatnTipYMHb0wnBJUdgPvuT1aG0+pc5jqQcTsw54s2bNevtQgqcaCd8FC9UsYWDFa179wZgMg6ocjifwN8vhLhFJoLoCKgCrK6frTlqgY//BbUNl7jlzbifmfbY3bK+R91/Y6ZJrBp50g7rgSwLO+W73pnXLlk8ZU7Bi6rh1kWVcnGuJ0dQ5t9eMWQ4mLW1ZbzGjR39WhCcypZtHPdK5E4ts09YxkXNR7BKosoDjOcvl6SqfrxMlcISACkDdErUmcM6AAXXSD3i/xpwZ3tbDHbfvcWmbPnc+fl+tBZKEHYVLS0u2rFg4Z8GHo6dtX7N0N1DHOdpirh6wBWyShZm6eMo7G5Jmft8CTiPEwxj3Ao2TcFiVkgSOLWA8YJ35g4gkUF0BFYDVldN11RLI7TN4lMM/0/CC/WIjRygjM3TTT//xlOfpQ5CTRS0+uH/P+gXTps1576U5xft2FztzDXCuDVjwa3uNw5uQ6YWmz81/Y8/Jth2v57slXITjUYxrgbR4jVNxSSDKAkMsh3ei3IeaT2IBFYBJPLjxmFpOn8F3OHNDjtwTePizf7yvfuPmbeIx5niMad+OT9etnjahYMG4fy/D94NPdptjBK96D4Et8Hzyi7PD81aOGZMUi8S6iaTRkusr5vf1jscxUUwSqFWBMD2sK3NrtU91llQCKgCTajjjP5lOfQf1Czn7BvCFL4Evf+DZgW06db8w/jOIXYS+Hw7vWLdi0ZKJbxWsn1/wqcMFT7+CpU0aYbbLnJsaDoenLJvy/vKkec27iODDlfuB4J5pFzt99SyBOBNIo4V1IGm+3I8z3ZQIRwVgSgxz/CTZ+dKBHT3Pe9bhNhhWdDiy8wbf3v3cK0YMj59I4yeS0uLiQ5uXzpo5d8yrM/ZtWX/AGXUtmN8XzPPD32iOiYSYtjj/vS3xE/WpReKW0Zkw/4FxB1D31FrT1RJIOoFiy9HyRkk3qrWckArAWgZP9e66XXVVvZKi9F8Gy8AY9tnfXlt06Nr06m/88KFU96mc/6F9u7etmzOpYO6YVxeUFRWW+cGHDs61No+w+Sz3QzbRI2vm4vzRB5LFzS3hKuCxYHUgbVWZLKOqPKIgsNpyODsK7arJFBJQAZhCgx0vqeb0GfQ9jM7mLNhx4rPjxp/84/GMrHr14yXOWMThnGPPp+tXLJ/6QcHyye+vduY8c7R00MyMfeDmYExqwaGF+fn5ZbGIsab7dBvI4gC3A48AXWq6fbUngSQUmGg59E/CvJRSLQqoAKxFbHVVLpDbd9BInN145DzAq//jP4e3OLNzSi4IHS4rLd22avHc+eNem7ZtxcKdmJ8RWcYFywa3zRmTMH/q0vz31yXN/L6ltMFF5vYFc/y0p6l+g5BA1QX+x3Iif2HSIYFqC6gArDadLqyuQOc+gy72sMdxbhFmn+0522Pw7d26XjFiRHXbTcTrSgoP7guWcZk/5uXZB3fvKAKX7Yy2zidknq01whOtxJ+2aOoHuxIxv6PF7JbQs+I17w1AerLkpTwkUGsCjvsslxdrrT91lJQCKgCTcljjO6mc/gPbU+r9yMxtA/ts/lqjtu3rD/nWrx+P7+hrJrr9O7ZuXD0zv2DB+NeWuJJS58yaGrTycUU4W4RHfpEdmLM2P/+zD2VqpufYtOIcIZYSfOQTzO+7JDZRqFcJJImAz1esC9OSJBulESMBFYAxgk/lbnv27Jl+qF6rnwP1DdtY2eL6H/35G1n1GyXl60Dn+/7ODasWL/nonYK1sydvqljGpRXQBNjlzKalOffxwkkXLoNRfjLcI24VDSmJ7NQRfOBzRjLkpBwkEGMBRwn1rTsHYxyHuk9wARWACT6AiRp+bp9B9/jGgCMXhL78688MaNP5vF6JmtfR4i4rKS7cvGzurPkfjJ6xe+Pqfc75Wc6srYdl+s5txsh34VDBsilvbU6WvN0KzqYssjfv3UB2suSlPCQQcwHHKsvlnJjHoQASXkAFYMIPYWImkNt30GU4+48j5wF27T+iU4+ht9+UmFl9MerC/Xt2rJv7ybR57788r+TggVKH38g5axMs44KzlR7hiWXh9BnLpry1PxnyDXJwS+kX2aYNBgNesuSlPCQQRwKvW05kG0QdEjglARWAp8Sni6sr0LH/4LZpYfdjB7vN2b7D7dTJbphx/Q9e/LYXStx9gfds2bBqxScfFiz9+J2VzjnPOVpYsFUb7gAu2Lop9FHdQ5sXzJo1q7S6fvF0nVtBHcq4GSKFX0p+xR1P46FYkl7gh5bDs0mfpRKMuoAKwKgTq4OjC4zycvvMfA5zrXAWLG3y2TH4O7++pXHr9h0SSc4Pl5VtW7N0/sLxrxV8umTedt+5dM+jjXM0MNjhzH0cCrlPFk4YszpplnFZSQtKeRD4OtAykcZLsUoggQWutxxeS+D4FXqcCKgAjJOBSMUwOvcdeLPnvOuOXA/wghF39sjpO2xoIpiUFB7av3HRzBlz3n9p5qEdWwt9XL1gmzYHaZ6x3jkmhkPetOX5b+9IhHyqEqNbRnf8yNO+4Klfnapco3MkIIEaE+hkOQT7feuQwCkJqAA8JT5dfCoCXS4b3CvsuW+Zs6XB1nCH28pu2qru8O/99gnzvLi9Pw/u3r559cxJBfPGvrLIlQa7tLmmvtHKc1bi8Jf4zk08WJY9e+PU0YWnYhQv1zqHxzIG4yLLuOTFS1yKQwIpJnCIztQ3IylWCUixsYu7dOP2D9i4k1JANS7QMW9Is5BzPzHnisB2Vu5g2Pd+e2eD5m3iatkQ5/tu1+bVS5d+9N7U1TPyNzgIYbTCuabAboyZGJOXNK+7hNGjPytoaxyuFht0C8kmjXtwPAz68rAW6dWVBL4sYIy3zlwhGgnUhIAKwJpQVBvVFbDcPoO+6Yye5mxZ5UZ63/i1Xh0vunpAdRuuyevKSkuKt6xYOHve2Fem71q7Yo/DZWK0xVHXIFjGZVKa+VPn54/5wpqGNRlDbbfllkTW7AuKvq8CDWu7f/UnAQkcRcDxjOXyI9lIoCYEVADWhKLaqLZA7mWDL8d44MjlYBq3O6vBoG/+4jGz2N2iRQf27Vq/YOq0ue/+c27xgf0lzlwDIvvzgnOstBATi9OKZ6weN25vtQHi7EK3jEvxI695hxE84dQhAQnEj4BPH+vC5PgJSJEkskDs/nRNZDXFXmMCnS4Z2sa8sh+bsc/w9lRueMQzf7g3u0mLSMFVm8e+bZvXrigYV7B44pvL8X1csISL0RJnB8Ff4HmhfLc9c97ixaNLajOuaPXlZpJONjfiIgs3XxCtftSuBCRwSgKFpNHYOlB8Sq3oYglUCKgA1K0QawHL6TPwe4aXA6yoHEzvkff36njJNbXyGtgPh8M71i1fsHDC6wWbFs7cGizjAq61mTXCscPhPjHzpiyZ9M7KpFnGZQlNMb6Oiyzl0ibWN4L6l4AEjisw0XLoLyMJ1JSACsCaklQ71RbokjfwGt+3+wxbULmRuk2aZ4343u+/Gc1FoUuLCg9uWjp7xtz3Xp65f9umg86oi+/aOY90c2z0YGKorLRg/idjt1U7wTi70C0iFy+yjMttQFachadwJCCBowuMshx+IBwJ1JSACsCaklQ71RbI6T+wPWX8ELOdlXcFCRoc+PjPRzY9/Zzcajd+jAsP7d25dfXsyQULx7y6oLSkKOw71wRoZVDmPJaac/npmWWz5o8dmxQbrjuHsYQBWKTwu7KmPdWeBCQQdYE8y+GjqPeiDlJGQAVgygx1PCc6ysvpO2MUvp1hRrBTxmdHTr8hHS4YdvctNRG9c77bvXn98hVTxhQs/+TDtc6ch08rjKbBHETft1kOb9KyVnUWJc0yLjOpSzZ34ngE6FwTjmpDAhKodYFiMmlkZ1JU6z2rw6QVUAGYtEObWIl1vnTgYPPsbnMsxMwdjt5CIbvhx397LCOrbv3qZhQuKy3ZunLR3IXjXpu2deWiXcHuFQ6/DVg2jq3AJNLd1CUT3lufNPP7VtCOMA/huA8Inm7qkIAEElfgI8vRAuyJO3zxGbkKwPgcl5SLqlvegHYlYfsh5u33gkWVKx19v/rkFaef2+uSk0UpPrR/z/r506bPG/Py7MK9u4pxfn0XrN9HZG2ZNcE2bemO6Qs+fvcL/Z1sP/F0vltEb0I8hiPYYi8tnmJTLBKQQLUFfmA5jKr21bpQAkcRUAGo2yJeBMoXhcZdYHhLKwfVotO5Ta9+4AcPVTXQ/Tu2rF89bULBggmvL3VlZTijmWEtnblC59wiz2diSX03d+WYMUmxnIKbSBotua5ift9Xquqk8yQggQQRMC60zsxMkGgVZoIIqABMkIFKhTA7XTbgEvO8R82xzLCyyjkPf/r3d9dv1ur0Yzn4ftjfuX7lokUT3y7YMO+TzQ4XPP1qDTQOPi7BuYKw501Znt9zOYxKin003Xwak8Z9eJFXvaelwj2iHCWQggLrLYf2KZi3Uo6ygArAKAOr+aoLdLpkaH0vFP4pjnQz21z5ym4Db+zS/aobrz+ytbKSosJNS+bNnPv+S9P3fbr+gHN+lpnXzjeXYc5twoXyLRQuWJz/3paqRxLfZ7oldMTxKMYdQL34jlbRJavApq3wnV/CmElQWAwdz4D/+zH07FKe8YGD8OSv4I3xsHMPnNEWHrkNHri5aiIvvws3PwHDLoc3fvv5Nf/1Z/jFn8v/+8l74bG7Pv+5afPgwR/C9FchlCz72Diet9zI1/s6JFCjAioAa5RTjZ2qQE7fgbeb84JtyBZWbsvS07wbfviXRzOy6kU+Binct3v7ujkfF8x5/9X5ZYUHy/zgSZ9zrc0jHCwo7cwmeGTNXJw/+sCpxhQv17vFXBlZv88RLI6tX7vxMjApGMfuvdDjWujXGx64CVo0hVXry4u8syue09/3fZg4HV78Ufn/HzulvDh77fnyou54x7pNcMmtcFY7aNLo8wJwwXLofSO887+R7RgZ/ADMeBW6doTSUuh1I7zwA7jw3KQaFC3/klTDGT/J6A+R+BkLRQJ0uWxArrPQ95yxyRyHKqNcctujlzVpc8bpywvGFiyb9N6qYBkX52hh0ALYh2Oumf9RC69wQX5+/hdeIScqrltDJoXchkW2aeuaqHko7uQSePKXMGUOTP5/x86r6xC4cQB8P9hnpuLoeR0M7AM/Cu7mYxzhMPS9A+4eAZNnwZ79nxeAr46BX/0VCl4pvzgoBp+4G0ZeA//5R9i6E57/blJZb6Mzrc1IimkrSTUySZCMCsAkGMRkSiEvLy9tq19vFEZ7c7bqqLmZn+GcBVuX1Qe3zfA+DmOfLJv09tqkWcZlcWT+4oORrdqgWTKNsXJJfIHcwXD1JbBxK3w0A9q2hAdvgvtu+Dy3r4+CWYvKi7c2LSB/Ogx9EMa8AJf2PLbBs/8D85fB67+Fu576YgG4ZFX5k8G5/y5/AnjetfDJPyE9DQZ+DWa9BvWTaVKE8aJ1jizlpEMCNS6gArDGSdXgqQp07jPkKsP/GthiI/JKt+Jw2cEyLs4nhLHOM3+Cn5Y5fen413eeap/xcr1bzPnAYxjBH6UZ8RKX4pBAZYHM7uX/9fhdMPJqmL4AHv0J/HEU3DG8/OdKSuC+Z+Dvb0JaGnhW/jr49mCCxzGOKbPhxsdh7uvQrPGXC8Dgsj+8DL/+W3kDj90JX78JrrgbHroVysIw6reQng7PPwV9LkzwcfMYaJ0Yk+BZKPw4FVABGKcDk8phnZc3vFFxuPQ/zQjh2OLMmhq08nFFBkscTCzyDs5em5+fFKviO4fHUoZHlnFxXJbKY6/cE0Mgoxtc0AU+eenzeB95DmYsgKkvl/+/4GONP42G//o2tG8Dk2bCU7+C1/8Hrrj4y3nuPwjdhsHvn4EBfcp//sgngEfT+evr8OZ4+MMo6DSwfE5g8GTy1m/BmnFQJ3H/GrWXMC2sKyWJcVcoykQTUAGYaCOWIvF27jvwZpzdVPEEcJeD6SHnf7xocu+lSbOMywoaUMpXMR4GzkyRoVWaSSDQvj9ceTG8+OPPk/nfl+DHf4BNH0FhETTsBa//BgblfX7OvU+XF2fv/+nLCHOXlH9YUvnrXb9i5pvnwbL3Pv/A5PDVO3ZDrxtg0j9g9uLy/oMvgIOj+cUw4a9wbseEBf+n5XBrwkavwONeQAVg3A9RagYY7AxS6nt3es6WlaYzdfmEdzYli4RbzlmEIx913F0+j1GHBBJL4JYnYMOWL34E8thPYNr88qeC+w5AwwvhvT9+/jQvyPBrz8KajTD2/76cb1ExrFz3xf//9G8geDIYvM4NlpnJOOJp3m3fht7d4OHb4PUP4Yf/C3P+Xd5G494w8a9wXk5i2VaK9jrLoSKbhM1BgcexgArAOB4chZZcAm4JeRXr9w0BvOTKTtmkkkDwqvfiWyDYn+eGa8rnAAbz/YIlWG4N7m4g7w4IntD99vvlr4CDj0Ue+AH86jufrwV4x3fKPyD5yeNH1zveK+APp0BQIE59CYInhMG6hOdcDf/+TXlx+t1fw4aJkJWZkCOzgzBt9fo3IccuYYJWAZgwQ6VAE1HALSSDEMHSt8ETvx6JmINilsDRBN6ZCE/9GlasgzPbweN3fvEr4C3by38+WP9v197yIvD+G8o/3Ijsxl1RJAZrBP71JydXAAavmM8bAa/86otP+F4cXV4U1kkvn0tY+fVzgo3iLy2HJxIsZoWbYAIqABNswBRuYgi4FTSnjAciS7lAy8SIWlFKQAJxIWB0ts4si4tYFETSCqgATNqhVWKxEHBL6RZ5zQu3AHViEYP6lIAEElpgkuXQN6EzUPAJIaACMCGGSUHGs4BzGEsZDJHCr388x6rYJCCBOBcwbrfOHGePlTiPX+EljIAKwIQZKgUabwJuHvXIiHzJ+wjQId7iUzwSkEDCCewmkzZ2JkmxxmnC6adYwCoAU2zAle6pC7jFtI+s3Wd8FUejU29RLUhAAhKICPzGciIfjOmQQNQFVABGnVgdJIuAW8olFfP7RgChZMlLeUhAAnEi4DjXclkYJ9EojCQXUAGY5AOs9E5NwM0knXqMrJjfl+g7i54ahq6WgASiKTDVcjjKJnnR7FJtp7KACsBUHn3lflwBt4QngYeAtqKSgAQkEFUBxx2Wyz+i2ocal0AlARWAuh0kcAwBfzHvmTFAQBKQgASiLLCWLXSwfpRFuR81L4HPBFQA6maQwBECeXmk1YWu113JvfdcyzcEJAEJSCCqAo6HLJffRbUPNS6BIwRUAOqWkECFwMg8sgvhAjP6OaMjPqH/+zF5LZrSXEgSkIAEoiSwlUzO0NIvUdJVs8cUUAGom0MCwNDL6ec7hnnQ1jlKDDZhHLptCOeOvIZrhSQBCUggKgKOJy2Xn0WlbTUqgeMIqADU7SEBYEge33NGLzMWGJ/Pw0kL4f3tpzySXZeGgpKABCRQowLGHuB068z+Gm1XjUmgCgIqAKuApFOSX2DQ5VxmYR42Y4UZpZUzvutaeoy4nKHJr6AMJSCBWhVwPGe5PF2rfaozCVQIqADUrSABYORFZBVl8SMczcxYWxklZNhffsI3GtanqbAkIAEJ1JDAITzaWyd21FB7akYCJyWgAvCkuHRyMgsM6c8AHPcCi8zwK+d682C63DSA65M5f+UmAQnUooDjecvl0VrsUV1J4AsCKgB1Q0igQmDQpTT20vmxQTrG5iNh/vwcX2/aiJYCk4AEJHCKAiWkcbZ1YOMptqPLJVBtARWA1abThckoMDiP6z24xRkLj3wKOPwKOt49gpuTMW/lJAEJ1KrALy2HJ2q1R3UmgSMEVADqlpBAJYGrr6ZJejE/MCPL7Mt/O//TD/hqi2a0E5oEJCCBagrspIhzrEfkC2AdEoiZgArAmNGr43gVGNyfIZ7P3cFcQDzCleO86mLO+Mat3BmvsSsuCUgg7gUesRz+J+6jVIBJL6ACMOmHWAmerMDIK2hYVMYoPBoZrDvy+t89wx3tWnLmybar8yUggZQXWMYWumrP35S/D+ICQAVgXAyDgog3gaF5XOOM+zCWVF4YOojz0p60/dY9ka+FdUhAAhKouoAxzDrzVtUv0JkSiJ6ACsDo2arlBBa46irqZZbyrINWZqw+MpWfPc7wzmfTPYFTVOgSkEDtCkywHC6v3S7VmwSOLaACUHeHBI4hMKQ//Q0e8B0rPKOk8mmtm1P3+e/ycJ0MMgUoAQlI4HgCzuGbT0/rylxJSSBeBFQAxstIKI64E8jLIzPbeMagfbBF3JEB3ncDFwzuy6C4C1wBSUAC8SbwF8vhnngLSvGktoAKwNQef2V/AoHP9gj2WG1QVPl0M+yFYFmYprQVpAQkIIGjCTjHQfPoaJ2/vLi8xCQQSwEVgLHUV99xLzByJBlFO3jSObp6HouPDPii7rT+9r3c53no11Lcj6YClEBMBJ6wHH4Zk57VqQSOI6A/tHR7SOAEAkP60cXg275jn+ex88jTRz3EgB459BKkBCQggcoCzjHLcuht9sX1RKUkgXgQUAEYD6OgGOJdwIbkcafB0KNtEdekIXV+9wwP1c0kO94TUXwSkEDtCDhHmflcqA8/asdbvZy8gArAkzfTFSkoMOJympY6vu85GmOsOZLgtiGcO/Iark1BGqUsAQkcTcD4uXXmO8KRQLwKqACM15FRXHEnMPRy+vlhHvCMtRiHjgzwd89we7uWnBV3gSsgCUigVgXCYVaHGtLVTqOwVjtWZxI4CQEVgCeBpVNTW+D+nqRvacBjDnqZsfBIjbNPo8FPHucBrQ2Y2veJsk9tgciafx59rDNTUltC2ce7gArAeB8hxRdXAoP70MFCPAUUmbHtyOBuGUzXGwdwXVwFrWAkIIFaEyjzeT69C4/WWodf7CgfIotNx6r/GKWtbqsjoAKwOmq6JqUFBudxowc3Oo/FR+4THMD8/Amu7XQm56Y0kpKXQAoKlJaxJr0RXU7x1e+xirjhwOtw3CWnVACm4H1X3ZRVAFZXTtelrMCAATRIL+JpB22PtkNI8FXw/zzNA9l1aZiySEpcAikmEHn1a1xmOXxyiqlXpwBMB0oBFYCniJ9Kl6sATKXRVq41JjAkj69gPAzsMGP3kQ1ffhGnP3Qrd3mmBaJrDF0NSSCOBUqKeabOefyoBkKsSgE4CgieCP4GeBo4AwgBE+Gz+cm3QWT9wf8Fvg+4itiC/x+8Iu4EHAQmVPz34SkteRXtXAH8DMiteK18N7CsBvJTE3EioAIwTgZCYSScgA3uz12ezxBnLDGL/O37C8dT93H5V87j0oTLTAFLQAInJXCwkI/q9aCf2WdF1kldf8TJVS0AnwA+hsic5KDQW1BRuPUE/q+i8LsAeKGiwPtTRT/BnsSfVhRzLYBfQ+QvsQMrfv5wATgNIsvYbAf+UFFgXnIqiena+BJQARhf44PIyNgAABufSURBVKFoEkhg6CXUd3X4Do4OZiw5MvS0dLw/PMu9zRvTOoHSUqgSkMBJCJSUsDMjg06W8+Vdgk6imcqnVrUA/C5E9iEPCrTDR3BtUNR1qfTE76fA0IoneUcL6UJgOlAfOABUfgI4vuKCoDh8F8gKPoCrZl66LM4EVADG2YAonMQSGNSHHC/Et4ASM7YcGf25HWk66ht8LS2NYI6ODglIIIkEfB9/1z4ub35RZO5dTR1VLQBvBToc0Wlw7WogeMp3+BgG/AvIrHhS2AMIXiGfBzQBPKBuRdEY7Hd+uAAMCsnDxWVwzWygPbC+phJVO7EVUAEYW3/1ngQCQ/oR/AZ7u4NVnn154dc7htP9uisj83V0SEACSSSwZRs/a92XJ2s4pbcg8jQxmHNX+bgLeB4iH5cdngMYFHGVjxMVgEERuBYYW/FaNyjwTgc+AIIiL1hC5nAB2BjYU9F40M8c4MyK62s4ZTUXCwEVgLFQV59JJTByJBnFO3jUQe9gAvbR5gE98yBX9+zCV5IqcSUjgRQW2LWXqU16c0kNzfurLPlzYAB8aSmp3wHB69peJygAgyd3wYcbh4+fQOQvqcH/C+YHzqwo+jZUnBB8FPIPFYCpdzOrAEy9MVfGURAYkEe7dONJB9kWbBV3xBEy7Dff5zZtFRcFfDUpgVoWOFTEznAJnRr0rrF5f5UzCL7oDV7F/qXiA45gO7krgV8GbxqA0ScoAIMiL/jg44/A+RX//s2K/24ObKx4khh82NEV+AXQUQVgLd9EcdCdCsA4GASFkBwCQ/rRF3gA2GzGviOzatmErP/6Dvc1yCZ4taJDAhJIQIGwj79lO1e0y4ssuRKtIyjinqsoyoLXtssrCsCXKzo83ivgRRXz+m6pmPMXFILBByOHl4G5GfhPiHycFszrC54QBq+d9Qo4WqMZp+2qAIzTgVFYiScwahTerHzuM7j6WEvDnJ9L8+9+jXvT08hIvAwVsQQksH4zT7a/PLI+ng4JJLSACsCEHj4FH28CkV1CinncucirlUVHmx903dV0vn0IN5p+9cXb8CkeCRxXYPVG/r+zrySYM6dDAgkvoD+CEn4IlUC8CQy+gtMtzDcNmmKsPFp837qHvpf2jHxtp0MCEkgAgXWbmfbXyVw8ahR+AoSrECVwQgEVgCck0gkSOHmBQf3o6RHZKq7YLLLq/peOX32HG84+nZyTb11XSEACtSmwZTvrX5pAj8dHsas2+1VfEoimgArAaOqq7ZQWGNqPQQ7uwLHZPPYeiZFdj/T/fpK7mjehTUpDKXkJxLHA7r3s+X9v0P+Rn0bWwdMhgaQRUAGYNEOpROJNYORIQkXbudtgoINl5lF8ZIwtm5H1829yT6MGNIu3+BWPBFJd4FARxf98ixvve5Y3U91C+SefgArA5BtTZRRHAlddRb06pfyHOS5wFlkk+kvzh85oQ/3nHuOr2XUjK/zrkIAE4kCgtIzwGx/yzRsej+y+oUMCSSegAjDphlQJxZvAwDxahTyewNHOjKVHi6/L2TT5/oPck5VJvXiLX/FIINUEnIOxU/jva+7j8Urr56Uag/JNcgEVgEk+wEovPgSG9KMLxqM40o+2U0gQZa9zafmtr3J3Rjp14iNqRSGB1BMIir9xU/nHv37PV1+YRWnqCSjjVBFQAZgqI608Yy4wOI9LPeM+B6Vmke2YvnT068VpD93K7WlppMc8YAUggRQTCIq/MZP59/OvccfYsRxMsfSVbooJqABMsQFXurEVGHo5Vzqfu4D9Zmw5WjRD+nHO3ddyc8jDi2206l0CqSMQFH/v5DPmL69x++vjo7LHb+pgKtOEEFABmBDDpCCTSMCG9mOIg1sMdmDsOFpuNw+myw3XcJ1n6NdoEg2+UolfgTfHM/GFf3P3exNYF79RKjIJ1JyA/nCpOUu1JIEqCUT2DP6I680Y6cMmD/YcqwgceTXX6klglVh1kgSqLfDmeKb85VUeeHMSC6rdiC6UQIIJqABMsAFTuMkhkJdHWn24DWOwwTqM/UfLbGg/Otw5ghvSQqQlR+bKQgLxJfDmBD758ys89NYkLfQcXyOjaKItoAIw2sJqXwLHEBg5koyKhaKvjuwZbBw62qlXXUT7+27klox0MoQpAQnUnMDbEyn407/4xtsTmF1zraolCSSGgArAxBgnRZmkAiMvIqu4Dvc7jzyC3UKg6GipXnIebR65g9sy65CVpBRKSwK1JuD7uDcmMvXPr/HIuxOZVWsdqyMJxJGACsA4GgyFkpoCI/PILjYedMZF5lhxrCeB5+fS/Nt3c3tWXeqnppSylsCpCwQ7fPz1dSa+M4Gn3spn5qm3qBYkkJgCKgATc9wUdZIJBEVgkce95tMHY82x5gR2PovGT3+dO+rXo1GSESgdCURdINjb9zd/Z+wn8/nR2+OZEfUO1YEE4lhABWAcD45CSy2ByOvgLO5yjitwrDePvUcTaN+G+j94mNsbN6B5agkpWwlUX2DPPg4890feX7qWX7wzgenVb0lXSiA5BFQAJsc4KoskERgwgDrpRdzqYCCw2YxdR0uteWMyn32Ykae15KwkSV1pSCBqAp9uY9ezv2PMlm389u18CqLWkRqWQAIJqABMoMFSqKkhECwRkw03AsPM2B78c7TMQ4Y9/SBXn59L79SQUZYSOHmB5WvZMup3vL3vIH/UBx8n76crkldABWDyjq0yS2CBkSMJFW5nBB7XmWPvsbaNC1K89zrOH5jHwJBHKIFTVugSqHGB6fNZ+9MX+Xexzx/fG8/yGu9ADUoggQVUACbw4Cn0pBewIXkMwrgZKDRj47Ey7t+b0++/gRuyMqmX9CpKUAInEPB9/DfHs+gvr/O6M154ZwKbhCYBCXxRQAWg7ggJxLeADcvjct/jVlxkIeiVZrijhXzO6TT87te4qWkjWsV3SopOAtETOFjIwef/zqyp83inNIP/++CDo8+jjV4EalkCiSGgAjAxxklRprjAoH709OAOoG1kwWij9Ggk2fVI/8HDDD/nNHJTnEzpp6DAhk/ZNOr3zNm2i3etHn9/++2j766TgjRKWQJfElABqJtCAgkiMLA/7UOOezC64bPSPA4eK/Rv3kWfSy8gzzP0azxBxldhVl/AOZg8i7m//gvLw/BG673864VZR/9LUvV70ZUSSC4B/eGQXOOpbJJcYHgejcIWeRLYx2ALxo5jpXzVxZxx1wiuraedQ5L8rkjt9IpLKPrza0x7fzLrfHjlgr68P2oUfmqrKHsJnFhABeCJjXSGBOJKYORIMoIvhM0YhlFssO5YAbZqTt2n7mPYGW3pGFdJKBgJ1IDAjt1s++HvmLPmU5Y6xz/enchsOPoc2RroTk1IIKkEVAAm1XAqmRQSsMF59LXyj0OCvYGXmx37qceDN9P7iou5UkvFpNAdkuSpzl7Ewp/+iVXFJUwjjb+99SGbkzxlpSeBGhVQAVijnGpMArUrMOxycn3HPeZzlm+s8IzCY0XQswstvnEL1zZtRMvajVK9SaDmBA4cYu+Lo5k2YRo7MN7PLORfo6ce+76vuZ7VkgSSS0AFYHKNp7JJQYGBebRKM+5w0AvYZsa2YzFkphF6/Kv0u/BcLtYHIil4syR4yvOXMednL7LuwAG24PHSWxOYrFe+CT6oCj9mAioAY0avjiVQcwLBHsJpRQw2GOobGZ7PSjzCx+rhiotof9cIhtevR6Oai0ItSSA6AgcOse+vbzBu7BQ8HAv8EH99dxyro9ObWpVAagioAEyNcVaWqSFgA/PoHoJbDc52HqsNDhwr9XpZpD1yG30v7MbFIQ8vNYiUZaIJLFzO7J+9yOK9B8jAMaEsi5fGjGFfouWheCUQbwIqAONtRBSPBE5RYEgezczjJue4DCJrBW441u4hQVc9cmn+tRsY1Lo57U+xa10ugRoTOHiIff94m7HvfYRnjm2E+HedJkwYPfrYT7ZrrHM1JIEUEFABmAKDrBRTT2DkSEKFO8jzHNc7owXGCoOi40ncNZzzBvThysw61E09MWUcLwLBPr5zlzLj139jxZ591DGP6WU+L4/JP/Ze2PESu+KQQCIJqABMpNFSrBI4SYHBV3A6PreY4wKDXdjxl8po2YSsh+/gyq7n0MP0u8NJauv0UxVY/ymrXhhN/vylNA6e+jmP11vvZaJ29ThVWV0vgS8L6Ld43RUSSHKB4AORUDFXeY4hDppirDne3MCAo18vTrtjOIObNKRFkvMovTgQ2Lufna+NZezr4ymMrGtpzAiFefnNSWyIg/AUggSSUkAFYFIOq5KSwJcFgqeBFuY6oDdGqfmsPd6XwmkhvHtHckHehfTJyqSeTCVQ0wIlpRR/NIOPXniZBcVlnGGO7WHjjbb7mKCnfjWtrfYk8EUBFYC6IySQQgLB3MDiHXzFGSOCxaMjr4SPs59wQJNdj/T7r+crF/Xgkox06qQQl1KNkoDvcMHXvb9/iY82b6O5c2QGT/0I8co741gfpW7VrAQkUElABaBuBwmkoMDwPBr5MNgZVzhHXStfMua4H4k0b0zmfTdwac8u9E4LkZaCbEq5BgTWf8rKf7zJuGnzMXO0cMZa83i71R4+1lO/GgBWExKoooAKwCpC6TQJJKGADcmjE3CdeZznHPtPtGRMYHBaa7Lvv56+XTpyvtYPTMK7IkopBR94/Ot98vOnsx/jdIxdBmPreIwdPY69UepWzUpAAscQUAGoW0MCKS4wciQZRTvJM8dQHG2Arc4iW8q549HknkXju66lX4cz6Kpt5VL8JjpO+hu2snr0GCbmT2M7xplA2Dk+IY239bpX940EYiegAjB29upZAnElMOIqWpSVcTnQH0cT59jseew8UZDndaLZ9dfwlZyz6a5XwyfSSo2fdw7WbGTpv8Yy+eNZbAFON4t8SLSIMt58axJztYdvatwLyjJ+BVQAxu/YKDIJxERgQB7t0o2rHFwKNHTGBg/2nCiYVs2pe+sgLrjwXHrpq+ETaSXnzweLOC9by8JXxvDx7EXsMGiNo5nvsT7k805JFpPHjKE4ObNXVhJILAEVgIk1XopWArUlYIOu4EwrY4AZvc3IwrEei8wTPO6RmUHopoGc2/dCvtKkES1PdL5+PvEFDhVxYN5S5vzrA2atWBfZp7eVOZoH0wl8GF9ahwkffMCuxM9UGUggeQRUACbPWCoTCURDwAb1obOFGGDQE0gLCkHzInsMn/AY0IezBlzGRae35hztLHJCroQ7YeNW1kyawczXxrG0tCQSfsvDhR8eE0pLyR8zme0Jl5gClkAKCKgATIFBVooSOFWBUaPwpufTLQSDMLqYkR7METRjd1XaDuYJDu5Hjy7n0K1uFtlVuUbnxKdAcQmFC1cw7/VxzFywnJ34hJzR2hxNKj4gmpiWTv7rY9kWnxkoKglIIBBQAaj7QAISqLJAsJB04Q66GvTF6IGjAUSe8ARfDfsnaihk2DV9OavvBXQ/+zQ6p6WRfqJr9PPxIbBtBxs/nsPMV8ewqLCYMuciY9cWqA9sAsbjmPJ2PjviI2JFIQEJHE9ABaDuDwlIoDoCNqQPZziPi824DGiOsY/yp4KlVWmwYQMyRvQnt3d3urduzhl6RVwVtdo9Z9deti1czqLxU1k8d1l5Yed8GhI88TNCwXQAZ4zL9PhEa/nV7tioNwmcqoAKwFMV1PUSSHGBIXk0Cz4UiSwfY5zuHCUWPBEyDlWV5pzTaTi0H926d6ZbowY0q+p1Oq/mBXbtYevCFSwe+wmLIq94y9drCXZ+aRksD2RwIFjOxYyP/XrMffvtqo9zzUerFiUggeoKqACsrpyuk4AEviAwZAh1/UP0tDD9zSI7jGQY7IosKg1lVeXqcjZN+vaiQ24HOrZpTvuQR6iq1+q86gns3MOWhctZ/OEUFi9Y+fnajw6ycZGnfRm+zxYPpoTTmP7uONZoHb/qWesqCcSLgArAeBkJxSGBJBHIyyOtHnT2vMgcwa+Y0cI5fOci8wR3nWiHkcoM2fVIv+oizjq/Cx3PakeHenUj8810nKJASSnFm7exbsU61kwsYPmiVZ8v0RJ52uciT2GD1/qHHCwzx2QrYfZbU068DNAphqbLJSCBWhJQAVhL0OpGAqkoMPQS6vuZdDWfC4FuQGODwuBr0aqsKXikWe9utLrkfDp0OpMOzZvQRk8Hq3ZXlYUp27KD9SvXsWbGItZMncXmsPt8q7/DRZ8r/5IXM3ZgTDXH9PP7smLUqBN/4FO1SHSWBCQQLwIqAONlJBSHBJJcYGAerUIe3Qwudo6zgHqRD0d8dlR1XcHKRMGC07270bpLJ9qd1ZZ2rZvTLrsuDZOcsUrphX3C23exedUG1sxdzJpJM9lQVEK48sVHLfpgrgfzSjJZMmZMZEFnHRKQQJIKqABM0oFVWhKIV4FgTcHZ+ZzljO4GvYM15HBkASXOsTOy7Zz3xWKlqrm0b0P9XufSrtOZtGvXinbBU8Jk35+4uISinXvZ+uk2Pl23mS1LVrFl9lK2l5V++and4aLPjKa+jwue9DljTsgx35WwWK94q3qn6TwJJL6ACsDEH0NlIIGEFQjmCzbyaO/7dHDQHY9zDBoFCTnY4xy7PIu8Mq7WkRbCyzmbxh1Op2m7VjRr2YymTRvRrFF9mibafsVhH//gIfbu3MO2T7exZdVGtixYypZl6469T7NzeMF+zsGrd6BuMBfTjO045jiP+V4xS1T0VevW0kUSSHgBFYAJP4RKQALJIzDicpqWQkccnYHu5mgB1AnmDTrYi2OfeRTXRMZNGlIn9xyandmWpm1aRArDxvXqkl0vi+ysOmTXySCrNtcmDD7MOHCIvfsOsHfPfvbs3svebTvZu2k7e9dtZM+6TzngKs3bO5qBc1hkcW6LFHzZwQc3zrG3YqHm+RhrMn2Wj86PLOWiQwISSGEBFYApPPhKXQLxLDBgAHW8g5wVCtHBOc7FOA0iO49kBE+ygo9IzLEv8mMVdiE52VzT0vHaNqdes8ZkNW1AVoMGZDXKpm7dLDLTQoQ8D88L4YWs4sfgvw0v5OFZ8O8eXjhMuLiE4sIiigtLKTlUSHFhIcWHiijZd5DiA8E/ByjZvofCXXtPvrCt2I0j2Fqvvhn1IwWgsR+fLQbzfI/V4VLWjJkcWcTZnayBzpeABJJXQAVg8o6tMpNAUgmMzCO7CNp50C7scaYRWWuwqUF2UPgYFDkiBeEh8yms7jzCeEXzHVlBrsE/GHWDOJ2LzJU8EKy36MNCD1aRxprzL2GLvtyN15FUXBKIDwEVgPExDopCAhI4SYFgX+LwFlqUpHOa52jnGx3NcbpBvaBY8jws8kSMyJO1YB7hocjuJI7ik1mL8CTDOqXTgyd6BpnBa29X/mPwcUydoNGKAveAGVvwWekbnxpsSfPY8vr4yDp+esJ3Svq6WAKpJaACMLXGW9lKIKkFRl5E1oE6NA0ZTYFmRuTHtgbtgtekkQ8hILNiblzw+1+wQ0nJ4X+cRbaxK8OnNPJjNb9GPoxc8Uo2hB/ZzSSNYP9cCJkjw3lk4iLFXcbheJxRai5SsBZhkXl6W3CsCQo9z9iS7rNV8/eS+hZWchKoNQEVgLVGrY4kIIEYCtjwPBri0dQ5moWDwtBR3/No4nya4NHYfOo5Iz1SqEF65Gmc4Z3q08KK17ThoKB0EMYI4yJF5x6MLc5nmwd7g6+efZ+9oTrsdZnsefvtyFNLPdWL4U2jriWQzAIqAJN5dJWbBCRQVQEbMCCy32299FKyPJ8s88kKp5HljDqeX/5hh+9XfNwR/BgUhw4vMv/QIj8GxWLkCZ7zKAl+LHUUp4fK/92VUeJlUpzmU0xjCkePjhSBOiQgAQnEREAFYEzY1akEJCABCUhAAhKInYAKwNjZq2cJSEACEpCABCQQEwEVgDFhV6cSkIAEJCABCUggdgIqAGNnr54lIAEJSEACEpBATARUAMaEXZ1KQAISkIAEJCCB2AmoAIydvXqWgAQkIAEJSEACMRFQARgTdnUqAQlIQAISkIAEYiegAjB29upZAhKQgAQkIAEJxERABWBM2NWpBCQgAQlIQAISiJ2ACsDY2atnCUhAAhKQgAQkEBMBFYAxYVenEpCABCQgAQlIIHYCKgBjZ6+eJSABCUhAAhKQQEwEVADGhF2dSkACEpCABCQggdgJqACMnb16loAEJCABCUhAAjERUAEYE3Z1KgEJSEACEpCABGInoAIwdvbqWQISkIAEJCABCcREQAVgTNjVqQQkIAEJSEACEoidgArA2NmrZwlIQAISkIAEJBATARWAMWFXpxKQgAQkIAEJSCB2AioAY2evniUgAQlIQAISkEBMBFQAxoRdnUpAAhKQgAQkIIHYCagAjJ29epaABCQgAQlIQAIxEVABGBN2dSoBCUhAAhKQgARiJ6ACMHb26lkCEpCABCQgAQnEREAFYEzY1akEJCABCUhAAhKInYAKwNjZq2cJSEACEpCABCQQEwEVgDFhV6cSkIAEJCABCUggdgIqAGNnr54lIAEJSEACEpBATARUAMaEXZ1KQAISkIAEJCCB2AmoAIydvXqWgAQkIAEJSEACMRFQARgTdnUqAQlIQAISkIAEYiegAjB29upZAhKQgAQkIAEJxERABWBM2NWpBCQgAQlIQAISiJ2ACsDY2atnCUhAAhKQgAQkEBMBFYAxYVenEpCABCQgAQlIIHYCKgBjZ6+eJSABCUhAAhKQQEwEVADGhF2dSkACEpCABCQggdgJqACMnb16loAEJCABCUhAAjERUAEYE3Z1KgEJSEACEpCABGInoAIwdvbqWQISkIAEJCABCcREQAVgTNjVqQQkIAEJSEACEoidgArA2NmrZwlIQAISkIAEJBATARWAMWFXpxKQgAQkIAEJSCB2AioAY2evniUgAQlIQAISkEBMBFQAxoRdnUpAAhKQgAQkIIHYCagAjJ29epaABCQgAQlIQAIxEVABGBN2dSoBCUhAAhKQgARiJ6ACMHb26lkCEpCABCQgAQnEREAFYEzY1akEJCABCUhAAhKInYAKwNjZq2cJSEACEpCABCQQEwEVgDFhV6cSkIAEJCABCUggdgIqAGNnr54lIAEJSEACEpBATARUAMaEXZ1KQAISkIAEJCCB2AmoAIydvXqWgAQkIAEJSEACMRFQARgTdnUqAQlIQAISkIAEYiegAjB29upZAhKQgAQkIAEJxERABWBM2NWpBCQgAQlIQAISiJ2ACsDY2atnCUhAAhKQgAQkEBMBFYAxYVenEpCABCQgAQlIIHYCKgBjZ6+eJSABCUhAAhKQQEwEVADGhF2dSkACEpCABCQggdgJqACMnb16loAEJCABCUhAAjERUAEYE3Z1KgEJSEACEpCABGInoAIwdvbqWQISkIAEJCABCcREQAVgTNjVqQQkIAEJSEACEoidgArA2NmrZwlIQAISkIAEJBATgf8f+pZuSCXWMe4AAAAASUVORK5CYII=" width="640">





    <function matplotlib.pyplot.show>




```python
percentTotalDrivers = perCityType_App.groupby("City Type").agg({"Total Drivers per City":"sum"})
percentTotalDrivers
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
      <th>Total Drivers per City</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>78</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>490</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>2405</td>
    </tr>
  </tbody>
</table>
</div>




```python
TotalDrivers = percentTotalDrivers["Total Drivers per City"].sum()
percentTotalDrivers["% of Total Drivers"] = [round((f/TotalDrivers*100),2)for f in percentTotalDrivers["Total Drivers per City"]]
explode = (0.1, 0, 0)
percentTotalDrivers
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
      <th>Total Drivers per City</th>
      <th>% of Total Drivers</th>
    </tr>
    <tr>
      <th>City Type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>78</td>
      <td>2.62</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>490</td>
      <td>16.48</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>2405</td>
      <td>80.89</td>
    </tr>
  </tbody>
</table>
</div>




```python
explode = (0.1,0.1,0.1)
driver_sizes = percentTotalDrivers["% of Total Drivers"]
d_labels = ["Rural","Suburban","Urban"]
d_colors =["lightcoral", "lightskyblue", "gold"]
```


```python
fig1, ax3 = plt.subplots()
ax3.pie(driver_sizes,explode=explode,labels=d_labels, autopct='%1.1f%%',shadow=True,startangle=90,colors=d_colors)
ax3.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
plt.title("Percentage of Total Drivers by City Type")
plt.show
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAoAAAAHgCAYAAAA10dzkAAAgAElEQVR4XuydB3hcxdWGv3NXq5Usd8u9ybbaqmvlgotsuWBb7k226R1MCc0USwYsA8EQIKRAIPRQAiEkIZ2S5E9CftIglACmJwRCN802btLO/3zre/2vhcqutLvacuZ5/CCke2fOvGf23m9n5pwRaFECSkAJKAEloASUgBJIKQKSUr3VzioBJaAElIASUAJKQAlABaAOAiWgBJSAElACSkAJpBgBFYAp5nDtrhJQAkpACSgBJaAEVADqGFACSkAJKAEloASUQIoRUAGYYg7X7ioBJaAElIASUAJKQAWgjgEloASUgBJQAkpACaQYARWAKeZw7a4SUAJKQAkoASWgBFQA6hhQAkpACSgBJaAElECKEVABmGIO1+4qASWgBJSAElACSkAFoI4BJaAElIASUAJKQAmkGAEVgCnmcO2uElACSkAJKAEloARUAOoYUAJKQAkoASWgBJRAihFQAZhiDtfuKgEloASUgBJQAkpABaCOASWgBJSAElACSkAJpBgBFYAp5nDtrhJQAkpACSgBJaAEVADqGFACSkAJKAEloASUQIoRUAGYYg7X7ioBJaAElIASUAJKQAWgjgEloASUgBJQAkpACaQYARWAKeZw7a4SUAJKQAkoASWgBFQA6hhQAkpACSgBJaAElECKEVABmGIO1+4qASWgBJSAElACSkAFoI4BJaAElIASUAJKQAmkGAEVgCnmcO2uElACSkAJKAEloARUAOoYUAJKQAkoASWgBJRAihFQAZhiDtfuKgEloASUgBJQAkpABaCOASWgBJSAElACSkAJpBgBFYAp5nDtrhJQAkpACSgBJaAEVADqGFACSkAJKAEloASUQIoRUAGYYg7X7ioBJaAElIASUAJKQAWgjgEloASUgBJQAkpACaQYARWAKeZw7a4SUAJKQAkoASWgBFQA6hhQAkpACSgBJaAElECKEVABmGIO1+4qASWgBJSAElACSkAFoI4BJaAElIASUAJKQAmkGAEVgCnmcO2uElACSkAJKAEloARUAOoYUAJKQAkoASWgBJRAihFQAZhiDo/D7h4L4I4gu5oBvAfgMQAXAfhvHNocrklFAFYDuBPAv8O9OY6vnw3gKgBeAD0ALAfwUAt7fw9gRgh92AygMYTrnEvOBPAxgHvC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" width="640">





    <function matplotlib.pyplot.show>


