# GST 101: Introduction to Geospatial Technology
## Lab 7 - Basic Geospatial Analysis Techniques
### Objective – Use Basic Spatial Analysis Techniques to Solve a Problem

Document Version: 9/10/2014

**FOSS4G Lab Author:**
Kurt Menke, GISP
Bird's Eye View GIS

**Original Lab Content Author:**
Richard Smith, Ph.D.  
Texas A&M University - Corpus Christi

---

Copyright © National Information Security, Geospatial Technologies Consortium (NISGTC)

The development of this document is funded by the Department of Labor (DOL) Trade Adjustment Assistance Community College and Career Training (TAACCCT) Grant No.  TC-22525-11-60-A-48; The National Information Security, Geospatial Technologies Consortium (NISGTC) is an entity of Collin College of Texas, Bellevue College of Washington, Bunker Hill Community College of Massachusetts, Del Mar College of Texas, Moraine Valley Community College of Illinois, Rio Salado College of Arizona, and Salt Lake Community College of Utah.  This work is licensed under the Creative Commons Attribution 3.0 Unported License.  To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.  

This document was original modified from its original form by Kurt Menke and continues to be modified and improved by generous public contributions.

---

### 1. Introduction

In this lab, the student will explore a small set of analysis tools available in QGIS Desktop 2.2.0.  The student will conduct a spatial analysis and create a map of the results for a team of surveyors visiting National Geodetic Survey Monuments in Albuquerque, New Mexico.  The surveyors wish to have a map showing monuments within the Albuquerque city limits. They will use this map to plan their fieldwork for the week. 

This lab includes the following tasks:

+	Task 1 – Data Preparation

+	Task 2 – Querying and Extracting Subsets of Data

+	Task 3 – Buffering and Clipping Data

+	Task 4 – Preparing a Map


###	2.	Objective: Use Basic Spatial Analysis Techniques to Solve a Problem

Conducting effective spatial analysis in a GIS does not require the use of extremely complex algorithms and methods.  By combining multiple simple spatial analysis operations, you can answer many questions and provide useful results.  Determining the order in which these simple spatial analysis operations are executed, is often the hardest part of conducing spatial analysis.  Additionally, data is rarely available in exactly the format and subset that you require.  A large part of almost all GIS projects is simply obtaining and preparing data for use.  

In this lab, the student will utilize four basic geospatial analysis techniques: selection, buffer, clip, and dissolve.  

+	Selection uses set algebra and Boolean algebra to select records of interest.

+	Buffer is the definition of a region that is less than or equal to a distance from one or more features.

+	Clip defines the areas for which features will be output based on a ‘clipping’ polygon.

+	Dissolve combines similar features within a data layer based on an attribute.

###	3.	How Best to Use Video Walk Through with this Lab

To aid in your completion of this lab, each lab task has an associated video that demonstrates how to complete the task.  The intent of these videos is to help you move forward if you become stuck on a step in a task, or you wish to visually see every step required to complete the tasks.

We recommend that you do not watch the videos before you attempt the tasks.  The reasoning for this is that while you are learning the software and searching for buttons, menus, etc…, you will better remember where these items are and, perhaps, discover other features along the way.  With that being said, please use the videos in the way that will best facilitate your learning and successful completion of this lab.

###	Task 1	Data Preparation

In this task, you will obtain GIS data for this lab by visiting several online GIS data portals, A) the National Geodetic Survey (NGS) website, B) City of Albuquerque GIS Department, C) the New Mexico Resource Geographic Information System (RGIS) and D) the Bernalillo County GIS Department. All of these websites provide free geospatial information.

Note: Copies of this data have already been obtained and are available in the Lab 7/Data/Raw Data folder. If you are unable to obtain the data yourself, you may skip to Task 2 and use the Raw Data.

###	Task 1.1	Obtain Shapefiles of NGS Monuments

We first want to go to the same National Geodetic Survey (NGS) website you visited in Lab 5. This time you will download a shapefile of the monuments in the Bernalillo County, New Mexico. This is the county in which Albuquerque is situated.

1.	In a web browser, navigate to [http://www.ngs.noaa.gov](http://www.ngs.noaa.gov)

2.	Click on the Survey Mark Datasheets link of the left side of the page.

3.	Click the Shapefiles button.

4.	Use the COUNTY retrieval method:

	a.	Pick a State = New Mexico then click Get County List

	b.	Pick a County = Bernalillo

	c.	Data Type Desired = Any Vertical Control

	d.	Stability Desired = Any Stability

	e.	Compression Options = Send me all the Shapefiles compressed into one ZIP file…

	f.	File Prefix = Bern

	g.	(Leave all other options as the default values)

	h.	Click the Submit button

	i.	Click the Select All button

	j.	Click Get Shapefile

	i.	When the dialog box appears to save the ZIP file, save it into the Lab 7/Data/MyData directory.  

	ii.	Extract the ZIP file into the MyData directory.


###	Task 1.2	Obtain the Municipal Boundaries

Since you will identify monuments within the Albuquerque City limits, you’ll need an Albuquerque City limit dataset.  You will download the data from the City of Albuquerque GIS Department.

1.	In a web browser, navigate to [http://www.cabq.gov/gis/geographic-information-systems-data](http://www.cabq.gov/gis/geographic-information-systems-data)

2.	Scroll down until you find the Municipal Limits data. 

3.	Download the Boundaries shapefile to your folder.

	a.	Save the ZIP file into your Lab 7/MyData directory.

	b.	Extract this ZIP file into the lab directory.

###	Task 1.3	Obtain the Census Tract Boundaries

You will visit the RGIS clearinghouse. This is the main source for geospatial data for New Mexico. You will download census tract boundaries for Bernalillo County.
	
4.	In a web browser, navigate to [http://rgis.unm.edu/](http://rgis.unm.edu/)

5.	Click the Browse for Data button

6.	In the folder tree underneath Filter data by Theme, expand Census Data

7.	Expand 2010 Census

8.	Click on 2010 Census Tracts

9.	Download the Bernalillo County 2010 Census Tracts shapefile to your folder.

	a.	Save the ZIP file into your Lab 7/MyData directory.
	
	b.	Extract this ZIP file into the lab directory.


###	Task 1.4	Obtain Road Data

Finally, you will visit the Bernalillo County GIS Program to download a roads data set. This is the main source for geospatial data for New Mexico. You will download census tract boundaries for Bernalillo County.
	
10.	In a web browser, navigate to [http://www.bernco.gov/Download-GIS-Data/](http://www.bernco.gov/Download-GIS-Data/)

11.	Find the Download Shapefiles section

12.	Find Road Inventory

13.	Download the Road Inventory Zip file to your folder.

	a.	Save the ZIP file into your Lab 7/MyData directory.

	b.	Extract this ZIP file into the lab directory.

###	Task 2	Querying and Extracting Subsets of Data

Now that you have collected the necessary data, you will add it to a blank QGIS map document. Take a moment to familiarize yourself with the data and what information it contains. As with any project, you will have to do some data preparation to make it useful for the analysis.

###	Task 2.1	Working with coordinate reference systems

1.	Open QGIS Desktop 2.2.0.

2.	Using the Add Vector Layer button add all four shapefiles to QGIS Desktop. (Figure below).

![Add Vector Data](figures/Add_Vector_Data.png " Add Vector Data ")

3.	Organize the layers in the Table of Contents so that the Bern monuments layer is on top, followed by the RoadInventory, tl_2010_35001_tract10 (tracts)  and jurisdiction.

4.	Save your project to the Lab 7 folder as Lab7.qgs

5.	Does it look like all the layers are lining up together? Open the Layer properties for each layer and investigate their CRS’s. The Census Tracts (tl_2010_35001_tract10) and Monuments (Bern) are in geographic coordinates and the Road Inventory and jurisdiction are in the  State Plane Coordinate System (SPCS). From the menu bar choose Project -> Project Properties. Open the CRS tab and Enable ‘on the fly’ CRS Transformation and click Apply. The data should now all align properly. While the CRS tab is still open choose NAD83(HARN)/New Mexico Central (ftUS) as the CRS for the map. Click OK. 

Projecting on the fly is fine for cartographic purposes. However, when conducting a geospatial analysis all the data layers involved should be in the same CRS. Typically data layers will also be clipped to the extent of the study area to reduce rendering and data processing time. These procedures are often referred to as normalizing your data. For the typical analysis most of the time is spent obtaining data and normalizing it. Once all the data is organized and normalized the analysis often straightforward. 
 
6.	You will want to put all four layers into the same CRS for this analysis. You will put them all into the SPCS. Right click on the Bern in the Table of Contents and choose Save As…

7.	This will let you save out a copy of the layer in a different CRS. The Save vector layer as… window opens (Figure below). 
	
	a.	Click the Browse button to the right of Save as and save it into you’re MyData folder as Bern_spcs.shp. (It is useful to have a naming convention for new data layers. Here you are including the CRS in the name of the copy.) 
	
	b.	Next click the Browse button for the CRS. The Coordinate Reference System Selector window will open. From the Recently used coordinate reference systems choose NAD83(HARN) / New Mexico Central (ftUS) EPSG:2903. 
	
	c.	Check the box for Add saved file to map.
	
	d.	Click OK.

![Reprojecting the Bern layer](figures/Reprojecting_the_Bern_layer.png " Reprojecting the Bern layer ")

8.	You no longer need the original Bern layer in your map. Right click on the original Bern layer and choose Remove. Click OK on the Remove Objects window.

9.	Repeat steps 4-6 for the Census Tracts (tl_2010_35001_tract10).

10.	Save your project.

###	Task 2.2	Dissolving Tract Boundaries into a County boundary

For the map, you will need a polygon that represents the county boundary.  The tl_2010_35001_tract10_spcs Census tracts collectively define the county, so you will use the dissolve spatial analysis technique to create a county boundary from the Census tracts.

1.	From the menu bar choose Vector -> Geoprocessing Tools -> Dissolve (Figure below).

![Dissolve tool](figures/Dissolve_tool.png " Dissolve tool ")

2.	The input vector data will be tl_2010_35001_tract10_spcs. You can dissolve based on attributes. For example, if you had counties of the United States you could dissolve them based on the State name attribute and create a state boundaries layer. Here you will dissolve all the tract polygons into one to create the county boundary. For Dissolve field choose – Dissolve all --. Name the output shapefile Bernalillo_county.shp and save it to your MyData folder (Figure below).

![Dissolve tool settings](figures/Dissolve_tool_settings.png " Dissolve tool settings ")

3.	Remove the tl_2010_35001_tract10_spcs layer. It was an intermediate dataset. All you need is the Bernalillo County Boundary. 

4.	Save your project.

###	Task 2.3	Select Monuments

You will want to filter the monuments so that you only have the ones with the orders and classes you’re interested in. Here you only want monuments that meet the following requirements:
	
	a.	Elevation Order = 1
	
	b.	Last recovered on or after 1995
	
	c.	Satellite Observations were used for monument coordinate determination.
	
	d.	a, b, and c are stored in these attribute columns:
	
	i.	ELEV_ORDER
	
	ii.	LAST_RECV
	
	iii.	SAT_USE

(For information on what an elevation order and class is, visit [http://www.ngs.noaa.gov/heightmod/Leveling/](http://www.ngs.noaa.gov/heightmod/Leveling/))  

1.	Double click the Bern_spcs layer to open the Layer Properties. Select the General tab. Find Feature subset. This is where you can define the contents of a layer based on the attributes. It is a way to filter a layer. Click the Query Builder button to open the Query Builder. Here you can write a SQL query to filter your data. All the attribute fields are listed on the left. Below the fields are operators you can use to build your SQL expression. The expression is built in the blank window at the bottom. It is best to double click fields and field values while building the expression so that you avoid syntax errors. Double click on the field ELEV_ORDER and it will appear in the expression window surrounded by double quotes. Click the = sign under operators. Then click the All button below Values to get a list of the values contained in that field. Double click the 1 value so that your expression reads "ELEV_ORDER" = '1’.  Since you want monuments that have both an elevation order of 1 and were last recovered on or after 1995 you will now use the AND operator. The AND operator selects records that meet conditions on both sides. After the AND operator create the portion of the expression dealing with LAST_RECV. Add another AND operator and create the third portion of the expression dealing with the SAT_USE. The final expression should look like Figure below.

![Monuments SQL Filter](figures/Monuments_SQL_Filter.png " Monuments SQL Filter ")

2.	Click the Test button. You should get a Query result of 47 rows. If you have a syntax error you will be notified and you’ll have to figure out where the error lies. Any extra tics (‘) or quotes (“) will throw an error. Click OK.

3.	Click OK.

4.	It is always a good idea to open the attribute table to ensure that the layer has been filtered the way you needed.

5.	QGIS should now resemble Figure below.

![QGIS with filtered monuments](figures/QGIS_with_filtered_monuments.png " QGIS with filtered monuments ")

6.	Save your project.


###	Task 3	Buffering and Clipping Data

Now that you have the county boundary layer and the monuments selected, you will identify just the monuments within the Albuquerque City limits.

1.	First, you will create a filter on the jurisdiction layer and the RoadInventory layer as you did for monuments. 

2.	The jurisdiction layer covers much more than Bernalillo County. Albuquerque covers just a portion of the county and jurisdiction extends north and south of the county boundary. Open the attribute table for jurisdiction. The first field ‘JURISDICTI’ has the city names. Notice that the majority consists of unincorporated areas. You can click the field header and you will see a small arrow appear. This lets you toggle back and forth between an ascending and descending sort of the records making it easier to find certain values. Close the Table.

3.	Open the Layer properties for jurisdiction and go to the General tab. Under Feature subset click the Query Builder button and create a query that selects only the JURISDICTI of Albuquerque. Click OK on the Query Builder and close the Layer Properties.  Drag jurisdicti above the Bernalillo County layer and turn off RoadInventory. Your map should resemble Figure below.

![QGIS with filtered jurisdiction](figures/QGIS_with_filtered_jurisdiction.png " QGIS with filtered jurisdiction ")

4.	Open the attribute table for RoadInventory. There is a lot of information in here. So far you have filtered a layer within QGIS but left the data on disk the same. Here you will select out the major roads and save them to a new shapefile. What field would you use to select out major roads?

5.	Click on the Select features using an expression button. ![expression button](figures/expression_button.png " expression button ")  

6.	A similar query window opens as when you are filtering a layer. Instead of the fields being listed on the left, here you have a series of functions. If you scroll down though you will see that one category is Fields and Values. Expand Fields and Values. Scroll down until you find the Class field. Double click on Class to add it to the expression window at the bottom. Click all unique under field values. Click the = operator and the Major value (Figure below). Click Select and Close the Select by Expression window.

![Select by Expression in Attribute Table](figures/Select_by_Expression_in_Attribute_Table.png " Select by Expression in Attribute Table ")

7.	You now have 4593 out of 37963 records selected. You can use the Toggle at the lower left corner of the attribute table to show just the selected set of records (Figure below). Close the table.

![Toggle Attribute Table View](figures/Toggle_Attribute_Table_View.png " Toggle Attribute Table View ")

8.	Right click on the RoadInventory layer and choose Save Selection as… and fill it out as in Figure below.

![Save Selection As](figures/Save_Selection_As.png " Save Selection As ")

9.	Remove RoadInventory it too was an intermediate dataset. All you need for your map is major roads. 

Now that you have the Albuquerque City limits isolated, you will buffer Albuquerque by one mile. Then you will be able to identify monuments that are either inside, or close to the city limits. Buffer is an operation that creates a new polygon layer that is a buffer distance from another layer. 

10.	From the menu bar choose Vector -> Geoprocessing Tools -> Buffer(s). The input will be the jurisdiction layer that now equals the Albuquerque city boundary. Use the default for Segments to approximate. You will enter a Buffer distance in map units. The SPCS has units in feet. Therefore, to buffer the city boundary by a mile, enter the number of feet in a mile (5280). Name the output Albuquerque_buffer.shp. Check Add result to canvas. Click OK and then Close.

![Buffer](figures/Buffer.png " Buffer ")

11.	Drag the new buffer layer beneath jurisdiction and you’ll see that it is a one mile buffer of the boundary.

Now that you have the search area for the selected monuments, you will clip the monument layer to the buffered city limits to create a new shapefile with only the monuments the surveyors should visit. Clip acts like a cookie cutter. It cuts data out that falls within the clip layer boundary. 


12.	From the menu bar choose Vector -> Geoprocessing Tools -> Clip. The input vector layer will be the Bern_spcs. The Clip layer will be the Albuquerque_buffer. Name the output Albuquerque_monuments.shp. Check Add result to canvas and click OK and Close (Figure below).

![Clip](figures/Clip.png " Clip ")

13.	Remove Bern_spcs from the Table of Contents.

Finally, you will label the monuments with the FeatureID attribute.

14.	Open the Layer properties for the Albuquerque Monuments and select the Labels tab. Check the Label this layer with box and choose FeatureID as the field. Select the Buffer item and check Draw text buffer with the defaults (Figure below). This will create a white halo around the labels, which can make them easier to read against a busy background. Click the Placement option and give a Distance of 2. This will offset the label from the point a bit giving more room for a bigger point symbol. There are many options for label placement!

![Feature Labels](figures/Feature_Labels.png " Feature Labels ")

15.	Label the roads as well using the StreetName field. Use a font size of 5.25. On the label Rendering tab under Feature options choose Merge connected lines to avoid duplicate labels. This will clean up duplicate labels. 

16.	Your data should now resemble Figure below.

17.	Save your project.

![Final Data](figures/Final_Data.png " Final Data ")

###	Task 4		Preparing a Map

Now that you have identified where the monuments are that the surveyors should visit, you will make a map of the result of your analysis. You should show the major roads to give them a general idea of how to access the monuments.

1.	Rename the layers:

	a.	Albuquerque Monuments

	b.	Major Roads

	c.	City of Albuquerque 

	d.	Bernalillo County

2.	You do not necessarily have to show the buffer layer. It was just a means of identifying the monuments to map. However, you have cartographers license on that choice!

3.	Provide meaningful styles to the layers.

4.	Zoom into the monuments layer so that you can show as much detail as possible.

5.	Use the Print Composer

6.	Include the following map elements:

	a.	Title: Albuquerque Vertical Control Monuments

	b.	Legend

	c.	Your Name

	d.	Sources of Data

	e.	Scale Bar: Use the Add Scale Bar button ![Add Scale Bar button](figures/Add_Scale_Bar_button.png " Add Scale Bar button "). QGIS uses map units for scale bars. Here our map units are feet. Therefore, to make a scalebar read in miles you need to enter a Map units per bar unit value of 5280 (the number of feet in a mile) (Figure below).

![Scale Bar Parameters](figures/Scale_Bar_Parameters.png " Scale Bar Parameters ")

![Sample Final Map](figures/Sample_Final_Map.png " Sample Final Map ")

###	5.	Conclusion
In this lab, you used several basic spatial analysis techniques to prepare data for analysis and conduct the analysis. You reprojected data, queried and extracted data, conducted a dissolve operation and used buffer and clip to identify the final set of monuments. While none of these individual operations are necessarily complex, the sequence in which they were combined allowed you to answer a spatial questions quickly and easily.


###	6.	Discussion Questions

1.	Export the final map for your instructor to grade.

2.	You used different methods to create the major roads layer and the City of Albuquerque boundary. Why would you choose to filter a layer via a SQL query versus export a new layer based on a selection? What are the advantages/disadvantages to each?

3.	Think of another use of a clip operation with these data. 

4.	Could you use the dissolve tool to create a municipal boundary data set whereby all the unincorporated areas were merged together? If so describe how you would set up the tool. 


###	7.	Challenge Assignment

The surveyors work was streamlined and efficient due to your GIS analysis. They now have extra time to visit The Village of Tijeras while they are in town. Generate the same analysis and accompanying map for monuments meeting the same criteria for Tijeras. You can use all the same data so you will not have to download anything else.



