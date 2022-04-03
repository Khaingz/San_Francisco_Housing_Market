# San_Francisco_Housing_Market

## Challenge_6

## Housing Rental Analysis for San Francisco

In this challenge, your job is to use your data visualization skills, including aggregation, interactive visualizations, and geospatial analysis, to find properties in the San Francisco market that are viable investment opportunities.

Instructions
Use the san_francisco_housing.ipynb notebook to visualize and analyze the real-estate data.

Note that this assignment requires you to create a visualization by using hvPlot and GeoViews. Additionally, you need to read the sfo_neighborhoods_census_data.csv file from the Resources folder into the notebook and create the DataFrame that you’ll use in the analysis.

The main task in this Challenge is to visualize and analyze the real-estate data in your Jupyter notebook. Use the san_francisco_housing.ipynb notebook to complete the following tasks:

- Calculate and plot the housing units per year.

- Calculate and plot the average prices per square foot.

- Compare the average prices by neighborhood.

- Build an interactive neighborhood map.

- Compose your data story.

#### Calculate and Plot the Housing Units per Year
For this part of the assignment, use numerical and visual aggregation to calculate the number of housing units per year, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Use the groupby function to group the data by year. Aggregate the results by the mean of the groups.

###### Create a numerical aggregation that groups the data by the year and then averages the results.
housing_units_by_year = sfo_data_df.groupby('year').mean()

###### Review the DataFrame
housing_units_by_year.head()

2. Use the hvplot function to plot the housing_units_by_year DataFrame as a bar chart. Make the x-axis represent the year and the y-axis represent the housing_units.

3. Style and format the line plot to ensure a professionally styled visualization.

###### Create a visual aggregation explore the housing units by year
###### added format label values, round the labels for the y-axis amounts to whole numbers
###### highlight axes inversion. With the invert_axes option, invert the x and y axes along with the data
housing_units_by_year.hvplot.bar(
    x="year",
    y="housing_units",
    title="Housing unit per year",
    rot=90
).opts(
    yformatter='%.0f')

4. Note that your resulting plot should appear similar to the following image:

The result image show in jupyter lab. 

5. Answer the following question:

Question: What’s the overall trend in housing units over the period that you’re analyzing?

Answer: After being analyzed, The overall trend in housing_units over the period seem slightly increase over the year. But in 2013 the housing unites requirement was drop a bit but was increased in 2014.


#### Calculate and Plot the Average Sale Prices per Square Foot
For this part of the assignment, use numerical and visual aggregation to calculate the average prices per square foot, and then visualize the results as a bar chart. To do so, complete the following steps:

1. Group the data by year, and then average the results. 

###### Create a numerical aggregation by grouping the data by year and averaging the results
prices_square_foot_by_year = sfo_data_df.groupby('year').mean()

###### Review the resulting DataFrame
prices_square_foot_by_year

Question: What is the lowest gross rent reported for the years included in the DataFrame?

Answer: The lowest gross rent reported for the years included in the DataFrame is 2010.

2. Create a new DataFrame named prices_square_foot_by_year by filtering out the “housing_units” column. The new DataFrame should include the averages per year for only the sale price per square foot and the gross rent.

###### Filter out the housing_units column, creating a new DataFrame 
###### Keep only sale_price_sqr_foot and gross_rent averages per year
prices_square_foot_by_year = prices_square_foot_by_year[['sale_price_sqr_foot','gross_rent']]

###### Review the DataFrame
prices_square_foot_by_year

3. Use hvPlot to plot the prices_square_foot_by_year DataFrame as a line plot.

      Hint This single plot will include lines for both sale_price_sqr_foot and gross_rent.

4. Style and format the line plot to ensure a professionally styled visualization.

###### Plot prices_square_foot_by_year. 
###### Inclued labels for the x- and y-axes, and a title.
prices_square_foot_by_year.hvplot.bar(
    x="sale_price_sqr_foot",
    y="gross_rent",
    rot=90
)    

5. Note that your resulting plot should appear similar to the following image:

The result image show in jupyter lab. 

6. Use both the prices_square_foot_by_year DataFrame and interactive plots to answer the following questions:

   Question: Did any year experience a drop in the average sale price per square foot compared to the previous year?

   Answer: Yes, I can see that in year 2011 the average sale price per square foot was droped compare to year 2010.

   Question: If so, did the gross rent increase or decrease during that year?

   Answer: Yes, eventhough the average sale price per square foot was droped in 2010, the gross rent was increase to 1530.0 in that year.
   
#### Compare the Average Sale Prices by Neighborhood

For this part of the assignment, use interactive visualizations and widgets to explore the average sale price per square foot by neighborhood. To do so, complete the following steps:

Step 1. Create a new DataFrame that groups the original DataFrame by year and neighborhood. Aggregate the results by the mean of the groups.
##### Group by year and neighborhood and then create a new dataframe of the mean values
prices_by_year_by_neighborhood = sfo_data_df.groupby(['year','neighborhood']).mean()

##### Review the DataFrame
prices_by_year_by_neighborhood

Step 2.Filter out the “housing_units” column to create a DataFrame that includes only the sale_price_sqr_foot and gross_rent averages per year.

##### Filter out the housing_units
prices_by_year_by_neighborhood_averages = prices_by_year_by_neighborhood[["sale_price_sqr_foot","gross_rent"]]

##### Review the first and last five rows of the DataFrame
display(prices_by_year_by_neighborhood_averages.head())
display(prices_by_year_by_neighborhood_averages.tail())

Step 3.Create an interactive line plot with hvPlot that visualizes both sale_price_sqr_foot and gross_rent. Set the x-axis parameter to the year (x="year"). Use the groupby parameter to create an interactive widget for neighborhood.

Step 4.Style and format the line plot to ensure a professionally styled visualization.

##### Use hvplot to create an interactive line plot of the average price per square foot
##### The plot should have a dropdown selector for the neighborhood
prices_by_year_by_neighborhood.hvplot.line(
    x="year",
    y=["sale_price_sqr_foot","gross_rent"],
    rot=45,
    groupby="neighborhood",
    value_label='Sale price square foot/gross rent',
    title="SFO neighborhood-Sale price per square foot and average gross rent"
)   

Step 6: Use the interactive visualization to answer the following question:
   Question: For the Anza Vista neighborhood, is the average sale price per square foot for 2016 more or less than the price that’s listed for 2012?

   Answer: For the Anza Vista neighborhood, the average sale price per square foot for 2016 is less than the price that's listed for 2012.
   
#### Build an Interactive Neighborhood Map
For this part of the assignment, explore the geospatial relationships in the data by using interactive visualizations with hvPlot and GeoViews. To build your map, use the sfo_data_df DataFrame (created during the initial import), which includes the neighborhood location data with the average prices. To do all this, complete the following steps:

Step 1: Read the neighborhood_coordinates.csv file from the Resources folder into the notebook, and create a DataFrame named neighborhood_locations_df. Be sure to set the index_col of the DataFrame as “Neighborhood”.

#####  Load neighborhoods coordinates data
neighborhood_locations_df = pd.read_csv(
    Path('./Resources/neighborhoods_coordinates.csv'),
    index_col = 'Neighborhood'
)

#####  Review the DataFrame
neighborhood_locations_df

Step 2: Using the original sfo_data_df Dataframe, create a DataFrame named all_neighborhood_info_df that groups the data by neighborhood. Aggregate the results by the mean of the group.

##### Calculate the mean values for each neighborhood
all_neighborhood_info_df = sfo_data_df.groupby('neighborhood').mean()

##### Review the resulting DataFrame
all_neighborhood_info_df

Step 3: Review the two code cells that concatenate the neighborhood_locations_df DataFrame with the all_neighborhood_info_df DataFrame.
Note that the first cell uses the Pandas concat function to create a DataFrame named all_neighborhoods_df.

The second cell cleans the data and sets the “Neighborhood” column.

Be sure to run these cells to create the all_neighborhoods_df DataFrame, which you’ll need to create the geospatial visualization.

##### Using the Pandas `concat` function, join the 
##### neighborhood_locations_df and the all_neighborhood_info_df DataFrame
##### The axis of the concatenation is "columns".
##### The concat function will automatially combine columns with
##### identical information, while keeping the additional columns.
all_neighborhoods_df = pd.concat(
    [neighborhood_locations_df, all_neighborhood_info_df], 
    axis="columns",
    sort=False
)

##### Review the resulting DataFrame
display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())

##### Call the dropna function to remove any neighborhoods that do not have data
all_neighborhoods_df = all_neighborhoods_df.reset_index().dropna()

##### Rename the "index" column as "Neighborhood" for use in the Visualization
all_neighborhoods_df = all_neighborhoods_df.rename(columns={"index": "Neighborhood"})

##### Review the resulting DataFrame
display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())

Step 4: Using hvPlot with GeoViews enabled, create a points plot for the all_neighborhoods_df DataFrame. Be sure to do the following:
- Set the geo parameter to True.
- Set the size parameter to “sale_price_sqr_foot”.
- Set the color parameter to “gross_rent”.
- Set the frame_width parameter to 700.
- Set the frame_height parameter to 500.
- Include a descriptive title.

#####  Create a plot to analyze neighborhood info
all_neighborhoods_df.hvplot.points(
    'Lon', 
    'Lat',
    geo=True,
    size='sale_price_sqr_foot',
    title="SFO_neighborhoods",
    tiles="OSM",
    color='gross_rent',
    frame_width=700,
    frame_height=500
 )
 
Step 5: Use the interactive map to answer the following question:
   Question: Which neighborhood has the highest gross rent, and which has the highest sale price per square foot?

   Answer: Westwood Park has the highest gross rent $ 4390 per months and the highest sale price per square foot is $ 687.08.
   
Compose Your Data Story
Based on the visualizations that you have created, compose a data story that synthesizes your analysis by answering the following questions:

Question: How does the trend in rental income growth compare to the trend in sales prices? Does this same trend hold true for all the neighborhoods across San Francisco?

Answer: From the plot visualizations, we can see that once the rental income growth and the sales prices was growth together. Yes, this same trend hold true for all the neighborhoods across San Francisco.

**Question:** What insights can you share with your company about the potential one-click, buy-and-rent strategy that they're pursuing? Do neighborhoods exist that you would suggest for investment, and why?

**Answer:**  Overall San Francisco housing market will continuous booming. I do recommend the potential one-click, buy-and-rent strategy that the company pursuing is do able. I was suggest four neighborhoods for investment Russian Hill, Presidio Heights, Outer Richmond, Merced Heights, these neighborhoods has potential market grows and re-sale square price are higher than others neighborhoods and also the rental price are higher than others.