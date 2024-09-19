__Google Maps Directions API__ <br>

import pandas as pd <br>
import googlemaps <br>
from itertools import tee <br>

df = pd.read_excel(//data.xls") <br>
print(df) <br>

API_key = '???' <br>
gmaps = googlemaps.Client(key=API_key) <br>

time_list = [] <br>
distance_list = [] <br>
origin_id_list = [] <br>
destination_id_list = [] <br>

for (i1, row1) in df.iterrows():
<ul>
  <li> print("origin") </li>
  <li> print(row1['ID']) </li>
  <li> LatOrigin = row1['latitude'] </li>
  <li> LongOrigin = row1['longitude'] </li>
  <li> origin = (LatOrigin, LongOrigin) </li>
  <li> origin_id = row1['ID'] </li>
  </ul>
  for (i2, row2) in  df.iterrows():
    print("destination id")
    print(row2['ID'])
    LatDestination = row2['latitude']
    LongDestination = row2['longitude']
    destination_id = row2['ID']
    destination = (LatDestination, LongDestination)
    result = gmaps.distance_matrix(origin, destination, mode = 'driving')
    result_distance = result["rows"][0]["elements"][0]["distance"]["value"]
    result_time = result["rows"][0]["elements"][0]["duration"]["value"]

    
    time_list.append(result_time)
    distance_list.append(result_distance)
    origin_id_list.append(origin_id)
    destination_id_list.append(destination_id)
    
output = pd.DataFrame(distance_list, columns = ['Distance in meter'])
output['duration in seconds'] = time_list
output['origin_id'] = origin_id_list
output['destination_id'] = destination_id_list    

output_v1 = pd.merge(output, df, how = "left", left_on = "origin_id", right_on = "ID")

output_v2 = pd.merge(output_v1, df, how = "left", left_on = "destination_id", right_on = "ID")

output_v2

output_v2[['city_x', 'city_y', 'Distance in meter', 'duration in seconds']]

output_v2.to_excel(r"F:\Paper\Journal\External quality assessment of OpenStreetMap street networks A validation framework for routing and navigation applications\Coding\Google Distance Matrix API/WGS84 POIs Table_distancematrix.xls")
