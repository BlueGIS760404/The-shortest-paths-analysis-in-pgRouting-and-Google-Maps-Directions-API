import pandas as pd <br>
import googlemaps <br>
from itertools import tee <br>

df = pd.read_excel(r"F:\Paper\Journal\External quality assessment of OpenStreetMap street networks A validation framework for routing and navigation applications\Coding\Google Distance Matrix API/WGS84 POIs Table.xls")
print(df)

API_key = 'AIzaSyB7veDIBRdQIL0gNY43QX2eFKhcQWqkMTY'
gmaps = googlemaps.Client(key=API_key)

time_list = []
distance_list = []
origin_id_list = []
destination_id_list = []

for (i1, row1) in df.iterrows():
  print("origin")
  print(row1['ID'])
  LatOrigin = row1['latitude']
  LongOrigin = row1['longitude']
  origin = (LatOrigin, LongOrigin)
  origin_id = row1['ID'] 
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
