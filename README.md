# Vacation Weather

## Configure the map    
    # Configure the map
    map_plot_1 = city_data_df.hvplot.points(
    "Lng",
    "Lat",
    geo = True,
    tiles = "OSM",
    color = "City",
    scale = 1,
    alpha = 0.5,
    frame_width = 700,
    frame_height = 500    
    )

    # Display the map plot
    map_plot_1

**Initial Results**  
<img width="659" alt="image" src="https://github.com/SavannahWithAnH/VacationWeather_APIs/assets/126124356/a2b90e42-a1fe-43eb-8fa2-53d3549253c9">   


**Hotel Search**      
    # Print a message to follow up the hotel search
    print("Starting hotel search")

    # Iterate through the hotel_df DataFrame
    for index, row in hotel_df.iterrows():
    lat = row["Lat"]
    lon = row["Lng"]
    
    # Add filter and bias parameters with the current city's latitude and longitude to the params dictionary
    params["filter"] = f"circle:{lon},{lat},{radius}"
    params["bias"] = f"proximity:{lon},{lat}"
    
    # Set base URL
    base_url = "https://api.geoapify.com/v2/places"


    # Make and API request using the params dictionary
    name_address = requests.get(base_url, params=params)
    
    # Convert the API response to JSON format
    name_address = name_address.json()  
    
    # Grab the first hotel from the results and store the name in the hotel_df DataFrame
    try:
        hotel_df.loc[index, "Hotel Name"] = name_address["features"][0]["properties"]["name"]
    except (KeyError, IndexError):
        # If no hotel is found, set the hotel name as "No hotel found".
        hotel_df.loc[index, "Hotel Name"] = "No hotel found"
        
    # Log the search results
    print(f"{hotel_df.loc[index, 'City']} - nearest hotel: {hotel_df.loc[index, 'Hotel Name']}")

    # Display sample data
    hotel_df  


**Results**  
<img width="553" alt="image" src="https://github.com/SavannahWithAnH/VacationWeather_APIs/assets/126124356/1f9a0028-d861-4bd3-bdf8-4cdd593b4063">  


