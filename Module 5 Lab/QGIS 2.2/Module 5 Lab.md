# GST 101: Introduction to Geospatial Technology
## Lab 5 - Creating Geospatial Data
### Objective – Digitize Information from a Scanned Hardcopy Source

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

In this lab, students will learn how to georeference a scanned map. Georeferencing is the process transforming the coordinate system of the scanned map, from the coordinate system produced by the scanning process, into a real world projected coordinate reference system. The student will then learn how to digitize information contained in the scanned map into a shapefile. The first task will be to create the empty shapefile to digitize features into. The student will also learn how to edit existing vector datasets.

This lab will continue to introduce students to the QGIS interface. It is important to learn the concepts in this lab as future courses will require the skills covered in this lab. 

This lab includes the following tasks:

+	Task 1 – Create a new shapefile.

+	Task 2 – Transforming coordinate system of source data.

+	Task 3 – Heads-up digitizing from transformed source data.

+	Task 4 – Editing existing geospatial data.


###	2.	Objective: Digitize Information from a Scanned Hard Copy Source

While there is a large amount of digital information readily available to users of GIS, there’s still a large amount of information that is not been converted to digital format. For hundreds of year’s hard copy paper maps contained all the geospatial data. Many historic and even newer hard copy maps have never been digitized. It is possible to extract the information from hardcopy sources through process called digitizing. In this lab, you will use heads-up digitizing to digitize parcels in a portion of Albuquerque, New Mexico from a scanned map. This will be accomplished through a five-step digitizing process:

1.	Create a shapefile to store the data that will be digitized.

2.	Load the scanned map source data  into QGIS

3.	Georeference the source map

4.	Digitize parcels

5.	Save

###	3.	How Best to Use Video Walk Through with this Lab

To aid in your completion of this lab, each lab task has an associated video that demonstrates how to complete the task.  The intent of these videos is to help you move forward if you become stuck on a step in a task, or you wish to visually see every step required to complete the tasks.

We recommend that you do not watch the videos before you attempt the tasks.  The reasoning for this is that while you are learning the software and searching for buttons, menus, etc…, you will better remember where these items are and, perhaps, discover other features along the way.  With that being said, please use the videos in the way that will best facilitate your learning and successful completion of this lab.


###	Task 1	Create a New Shapefile

In Task 3 you will be digitizing parcels from a georeferenced data source. In this first task you will learn how to create the new shapefile you will eventually digitize into. 

1.	The data for this lab is located at C:\GST101\Lab 5 on the lab machine. Copy this data to a new working directory of your choosing.

2.	Open QGIS Browser 2.2.0.  

3.	Navigate to the GST 101 Lab 5/Data/New Data folder in the File tree and select the New Data folder by clicking once on it so that it is highlighted. 

4.	Click on the New Shapefile button at the top of the Browser window

![QGIS Browser New Shapefile button](figures/QGIS_Browser_New_Shapefile_button.png "QGIS Browser New Shapefile button")

5.	The New Vector Layer window opens. You will choose a geometry type, the coordinate reference system of the new shapefile and add an attribute. Choose a type of ‘Polygon’, and click the Specify CRS button to open the Coordinate Reference System Selector (Figure below).

![New Vector Layer window](figures/New_Vector_Layer_window.png "New Vector Layer window")

The City of Albuquerque, like most municipalities, uses the State Plane Reference System (SPRC) for their data. You’ll use the same CRS for your new shapefile. 

6.	In the Coordinate Reference System Selector type New Mexico into the Filter. This will limit the list below to just those with New Mexico in their name. These are different SPRC CRS’s for New Mexico. New Mexico has 3 zones and Albuquerque is in the Central zone. Select the NAD83(HARN) / New Mexico Central (ftUS) with an EPSG code of 2903 (Figure below). Click OK once you’ve selected this CRS to be returned to the New Vector Layer window.

![Browsing for the correct CRS](figures/Browsing_for_the_correct_CRS.png "Browsing for the correct CRS")

While creating your new shapefile you have the option of adding attribute columns. It is possible to add them later, but if you know of some attribute columns you’ll need in the layer it makes the most sense to define them here. You will need an attribute column to hold the zoning code. 

7.	In the New attribute section of the New Vector Layer window define a new field with: a name of zonecode, as Text data with a width of 5 (Figure above). This means the new zonecode attribute column will store data as text and will only be able to accommodate five characters of data. Since our longest zoning code is 4 digits this is more than enough. 

8.	Click Add to attribute list and you’ll see the new zonecode attribute added. The ID attribute is automatically added to every shapefile you create. Click OK.

9.	The Save As window opens. Since you had the New Data folder selected when you clicked the New Shapefile button it will default to that folder. If it doesn’t just navigate to that folder now. Name the shapefile parcels.shp and click Save to create the shapefile.

![Save As window](figures/Save_As_window.png "Save As window")

10.	Click the Refresh button in the upper left hand corner of the QGIS Browser window. Expand the New Data folder and you will see the parcels.shp file.
11.	Select the parcels.shp dataset and click the Metadata tab. You’ll see that it has 0 features and has the Spatial Reference System you specified. The New Mexico Central State Plane zone uses the Mercator projection since it is a north – south oriented zone.

![QGIS Browser with the new parcel shapefile metadata](figures/QGIS_Browser_with_the_new_parcel_shapefile_metadata.png "QGIS Browser with the new parcel shapefile metadata")

###	Task	2	Transforming Coordinate System of Source Data

Now that you have created an empty shapefile to store the digitized information, you will perform a coordinate transformation (also known as georeferencing) on the source data set so that it is in an Earth-based coordinate system. In this case the coordinate system will match your parcel shapefile (NAD83(HARN) / New Mexico Central (ftUS)).

To perform this task you will be using a Plugin. Plugins are small add-ons to QGIS. Some are created by the core QGIS development team and others are created by third party developers. 

1.	Open QGIS Desktop 2.2.0.

2.	Open  QGIS Browser 2.2.0.

3.	Arrange Browser and Desktop so that you can see both windows simultaneously on your desktop. 

4.	In Browser find the new parcels shapefile. Select it and drag it onto the map window of QGIS Desktop. This is another way to add data to Desktop. 

5.	From the Menu bar choose Project -> Properties. Click the CRS tab and Enable ‘on the fly’ CRS transformation. Click OK to save the setting.

6.	The project should now have a CRS of EPSG 2903 which is NAD83(HARN) / New Mexico Central (ftUS)). You can check this by looking at the lower right hand corner of QGIS Desktop and ensuring that EPSG: 2903 is listed. If not right click on the parcels layer and from the context menu choose Set project CRS from layer.

7.	Save the project to the Lab 5 folder and name it Lab5.qgs.

8.	From the menu bar choose Plugins -> Manage and Install Plugins

9.	The Plugins manager will open. Options along the left side allow you to switch between Installed, Not Installed and Settings. The plugin you’ll use is a Core QGIS Plugin called Georeferencer GDAL. 

10.	Since it is a Core plugin it will already be installed. You just need to enable it. Click on Installed plugins and check the box next to Georeferencer GDAL (Figure below). Click Close.

![Plugin Manager](figures/Plugin_Manager.png "Plugin Manager")

11.	To open the Georeferencer plugin go to the menu bar choose Raster -> Georeferencer Georeferencer (Figure below).

![Opening the Georeferener Plugin](figures/Opening_the_Georeferener_Plugin.png "Opening the Georeferener Plugin")

12.	The Georeferencer window opens. Click the Open Raster button at the upper left hand side (Figure below).

![Open Raster button](figures/Open_Raster_button.png "Open Raster button")

13.	Navigate to the Lab 5/Data folder and select the zone_map.bmp and click Open. Note: If the Coordinate Reference System Selector window opens click Cancel to close. This dataset does not yet have an Earth-based coordinate system.

![Georeferencer with source data loaded](figures/Georeferencer_with_source_data_loaded.png "Georeferencer with source data loaded")

On the map there are 5 points. These are benchmarks maintained by the National Geodetic Survey. To georeference this scanned map you will create control points at these five locations. The plugin will develop a georeferencing equation based off the set of source and target coordinates at these five locations. QGIS will obtain the source coordinates from your mouse click on those points. You will look up the target coordinates for these benchmarks from the NGS website. 

14.	The NGS website is at [http://www.ngs.noaa.gov/cgi-bin/datasheet.prl](http://www.ngs.noaa.gov/cgi-bin/datasheet.prl). Open the site. You will search for each of the benchmarks that appear on the map by searching for each benchmark’s datasheet.  You will use the Station Name option to do the search.  

15.	On the website click on the DATASHEETS button. Then click on the link for Station Name.

16.	For example, to find the first station, my search would look like Figure 10. Enter the Station name, pick New Mexico as the State and click Submit. NOTE: the station name is I25 27 with a capitalized letter i.

![NGS Datasheet Search](figures/NGS_Datasheet_Search.png "NGS Datasheet Search")

17.	The search should return the page shown in Figure below. Highlight the station name and click the Get Datasheets button and you will get something that looks like Figure below.

![NGS Datasheet Search Result](figures/NGS_Datasheet_Search_Result.png "NGS Datasheet Search Result")

![NGS Datasheet](figures/NGS_Datasheet.png "NGS Datasheet")

18.	This is an NGS Data Sheet.  It gives measurement parameters for NGS benchmarks located throughout the United States.  One piece of information it includes are coordinates for benchmarks in State Plane feet. In the Figure 12, the SPCS coordinates are circled in red. There are two sets of State Plane coordinates one in meters and one in feet. Be sure to use the set in feet. Note: there is a dash before the northing. It is not a negative number.  

19.	Find each benchmarks data sheet and fill in the coordinates below. The coordinates for the first station have been entered already.

<table>
<thead>
<tr>
  <th>Benchmark</th>
  <th>Northing</th>
  <th>Easting</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I25 27</td>
  <td>1,484,404.48</td>
  <td>1,524,608.32</td>
</tr>
<tr>
  <td> 	</td>
  <td>	</td>
  <td> 	</td>
</tr>
<tr>
  <td> 	</td>
  <td>	</td>
  <td> 	</td>
</tr>
<tr>
  <td> 	</td>
  <td>	</td>
  <td> 	</td>
</tr>
<tr>
  <td> 	</td>
  <td>	</td>
  <td> 	</td>
</tr>
</tbody>
</table>

20.	The next step is to inter the control points in the Georeferencer. Click on the Add point button ![Add point button](figures/Add_point_button.png "Add point button"). 

21.	Click on point I25 27.  (It is important to be precise and click directly on the point. The accuracy of your transformation depends on precisely locating the points. If you want to redo a control point click the ![Delete point button](figures/Delete_point_button.png "Delete point button") Delete point button and click on the point to delete.) The Enter map coordinates window opens. Enter the easting and northing State Plane Coordinates into the two boxes. Make sure you enter them correctly. The Enter map coordinates window is asking for the easting first and the NGS site listed the northing first (Figure below). Click OK and a red control point will appear on the map. The ‘to’ and ‘from’ coordinates will display in a table at the bottom of the window.

![Adding a Control Point for ‘I25 27’](figures/Adding_a_Control_Point.png "Adding a Control Point for ‘I25 27’")

22.	Repeat this procedure for points ‘I25 28’, I25 29’, K 15 S’ and ‘STADIUM’. After the 5 control points have been entered your Georeferencer window should look like Figure below.

![All control points entered](figures/All_control_points_entered.png "All control points entered")

23.	To perform the transformation click the Start georeferencing ![Start georeferencing button](figures/Start_georeferencing_button.png "Start georeferencing button") button.

24.	The Transformation settings window will open (Figure below). If beforehand you get a message saying ‘Please set transformation’ type click OK. 

a.	In the Transformation window choose the Polynomial 1 as the Transformation type. 

b.	Choose Nearest neighbor as the Resampling method. This is the standard raster resampling method for discrete data such as a scanned map. 

c.	Click the browse button to the right of Output raster. Navigate to your Lab 5/Data/New Data folder and name the file zone_map_modified_spcs.tif. 

d.	Click the browse button to the right of Target SRS. Type 2903 into the Filter and then double click the NAD83(HARN)/N… EPSG:2903 CRS to make it the Selected CRS (Figure below).

e.	Click ‘Load in QGIS when done’.

f.	Click OK.

![Transformation Settings](figures/Transformation_Settings.png "Transformation Settings")

![Selecting the CRS of the Output Raster ](figures/Selecting_the_CRS_of_the_Output_Raster.png "Selecting the CRS of the Output Raster")

25.	Close the Georeferencer and Save GCP points.

26.	Right click on the zone_map_modified.tif and choose Zoom to layer extent to see the georeferenced image.

27.	Using the Add vector data button add the netcurr.shp shapefile in the Lab 5/Data folder to QGIS. This is a shapefile representing city streets produced by the City of Albuquerque. If the transformation was done correctly, the streets will line up with the georeferenced parcel map image (Figure below). Save your map file.

![Georeferenced parcel map image ](figures/Georeferenced_parcel_map_image.png "Georeferenced parcel map image")

###	Task 3		Heads-up Digitizing From Transformed Source Data

Now you will digitize the parcels off the georeferenced image into the parcels shapefile.

1.	Drag the parcels layer above the zone_map_modified_spcs layer in the Table of Contents. Right click on parcels and choose Toggle editing. This puts the parcels layer into edit mode. Notice that a pencil appears next to the layer in the Table of Contents telling you that layer is in edit mode. Only one layer can be edited at a time. Turn off the streets layer.

2.	Using the Zoom in tool, drag a box around the M-1 parcels in the northwest corner of the image. You’ll digitize these first. There is an Editing toolbar for editing vector datasets (Figure below). If you don’t see that go to the menu bar to View -> Toolbars and turn it on. The tools available change slightly depending on the geometry of the data you are editing (polygon, line, point).When editing a polygon layer you will have a tool for adding polygon features.

![Editing toolbar ](figures/Editing_toolbar.png "Editing toolbar")

3.	Click on the Add Feature tool ![Add Feature tool](figures/Add_Feature_tool.png "Add Feature tool"). Your cursor will change to an editing cursor that looks like a set of cross hairs.

4.	Polygons are constructed of a series of nodes which define their shape. Here you’ll trace the outline of the first parcel clicking to create each node on the polygons boundary. Put your cursor over a corner of one of the polygons. Left click to add the first point, left click again to add the second, and continue to click around the perimeter of the parcel. After you have added the final node finish the polygon with a right click. 

5.	An Attributes window will open asking you to populate the two attributes for this layer: id and zonecode. Give the parcel an id of 0 and the zonecode is M-1 (Figure 19). Each parcel feature will receive a unique id starting here with zero. The next parcel you digitize will be id 1, the one after that id 2 etc. Click OK. 

If you want to delete the polygon you’ve just added click the Current Edits tool dropdown menu ![Current Edits tool dropdown menu ](figures/Current_Edits_tool_dropdown_menu.png "Current Edits tool dropdown menu") and choose Roll Back Edits to undo your polygon.

![Attributes window ](figures/Attributes_window.png "Attributes window")

6.	Adding single isolated polygons is pretty straightforward. Zoom back to the extent of the image. You can do this by right clicking on the source data raster and choose Zoom to layer extent or by clicking the Zoom last ![Zoom last button](figures/Zoom_last_button.png "Zoom last button") button.

7.	Find the big parcel in the south central area. There is a parcel with zoning code SU-1 that wraps around O-1. Zoom to that area.

8.	Open the Layer properties -> Style tab for the parcels layer and set the Layer transparency to 50% so that you can see the source data underneath your parcels.

9.	Digitize the outer boundary of the SU-1 parcel ignoring the O-1 parcel for the moment. Fill in the attributes when prompted. The SU-1 polygon will be a ring when completed but for now it covers the O-1 parcel. 

10.	To finish SU-1 you will use a tool on the Advanced Editing toolbar. To turn that on go to the menu bar and choose View -> Toolbars. Turn on the tool bar and dock it where you’d like. All toolbars in the QGIS interface can be moved by grabbing the stippled left side and dragging them to different parts of the interface.

11.	Now you’ll use the Add Ring tool ![Add Ring tool](figures/Add_Ring_tool.png "Add Ring tool"). Select it and click around the perimeter of the O-1 parcel. Right click to finish. This creates a ring polygon (Figure below).

![SU-1 Ring Polygon ](figures/SU-1_Ring_Polygon.png "SU-1 Ring Polygon")

12.	Now you will digitize O-1. To do so first you will set your snapping environment. Go to the menu bar and choose Settings -> Snapping options. This is a window that lets you configure what layers you can snap to while editing and set the snapping tolerance. The Mode lets you control what portions of a feature are being snapped to. To Vertex will snap to vertices, To Segment will snap to any part of another layers edge, and To Vertex and Segment will snap to both.  The Tolerance determines how close your cursor needs to be to another layer before it snaps to it. It can be set in screen pixels or map units. In our case map units are feet. 

13.	Uncheck netcurr since we won’t want to snap our parcels to that layer. Set the tolerance for parcels to 50 map units and choose a Mode of to vertex (Figure below). The map units are feet so when you get within 50 feet of a node (aka vertex) you will snap to it. This allows you to be much more precise than you could otherwise. Click OK.

![Final Map Extent](figures/Final_Map_Extent.png "Final Map Extent")

14.	To Digitize O-1 you'll use a tool that is part of the Digitizing Tools Plugin. First open the Plugin Manager and search for 'Digitizing Tools' in the All category. Select the Plugin and click the Install Plugin button. You should get the message Plugin Installed Successfully. Once it has been installed switch to the Installed plugins and make sure the Digitizing Tools plugin is enabled. The plugin is a toolbar.

![Digitizing Tools plugin](figures/Digitizing_Tools_plugin.png "Digitizing Tools plugin")

15.	Dock the toolbar, and select the Fill ring with a new feature (interactive) tool ![new feature (interactive) tool](figures/new_feature_interactive_tool.png "new feature interactive tool") . Left click on one of the vertices that defines the inner SU-1 polygon ring. You will immediately be prompted to enter the attributes for the new O-1 polygon. Click OK when done and the new polygon will appear. It automatically fills the space leaving no gaps. 

16.	Use the Identify tool ![Identify tool](figures/Identify_tool.png "Identify tool") to click on O-1 and SU-1 and verify that they are digitized correctly. 

NOTE: If you end up needing to move one or two misplaced vertices on a finished polygon you can do that. Use the Select Single Feature tool ![Single Feature tool](figures/Single_Feature_tool.png "Single Feature tool") to select the polygon, then use the Node Tool ![Node Tool](figures/Node_Tool.png "Node Tool") to select the individual node and move it.

If snapping is interfering with digitizing a parcel polygon you can go to Settings -> Snapping options at any time (even during digitizing) and turn snapping off until you need it again. 

17.	Finish digitizing the polygons. Anytime you have a parcel that shares a boundary with another, use snapping to make sure you create two parcels without a gap in between. Go back into Settings -> Snapping options and check the box under Avoid Int. to the right of Units. This enables Topological editing. When digitizing a shared boundary with this option checked you can begin with one of the vertices at one end of the shared boundary. Then continue digitizing the boundary of the new polygon and end at a vertex at the other end of the shared boundary. The shared boundary will be created automatically eliminating digitizing errors.  

Remember you can adjust the snapping tolerance and what features are being snapped to Vertex, Segment and Vertex and Segment. 

18.	When finished, click the Toggle Editing ![Toggle Editing button](figures/Toggle_Editing_button.png "Toggle Editing button") button to exit out of editing mode. You will be prompted to save your changes. Click Yes. 

19.	Turn off the zone_map_modified_spcs raster. You’re done with that now. It was an intermediate step necessary to get the parcel boundaries digitized. 

20.	Save your project.

###	Task 4		Editing Existing Geospatial Data

Now that you have digitized data into the empty shapefile you created you will learn how to modify existing shapefiles.

1.	You will add an aerial photograph raster. Click the Add Raster Layer button. Set the filter to Multi-resolution Seamless Image Database (*.sid, *.SID) .  Add all four SID images. 

2.	Drag the parcels layer above the image in the Table of Contents. 

3.	Click the Add Vector Layer button and add the parks.shp shapefile.

4.	Drag the parks layer above the raster imagery in the Table of Contents. 

5.	Turn off the parcels layer.

6.	Open the Layer properties -> Style tab for the parks layer and set the Layer transparency to 50% so that you can see the source data underneath your parcels.

7.	Zoom into ![Zoom into](figures/Zoom_into.png "Zoom into") the area highlighted in Figure below.

![Parks and Aerial Photography](figures/Parks_and_Aerial_Photography.png "Parks and Aerial Photography")

8.	This park polygon is incomplete. You will add the missing piece. Right click on parks and Toggle editing. 

9.	Set your Snapping Options so that you are only snapping to park vertices with a tolerance of 25 feet (map units).

10.	Using the Advanced editing toolbar click on the Reshape Features button ![Reshape Features button](figures/Reshape_Features_button.png "Reshape Features button"). This tool allows you to add to an existing feature. You must add your first vertex within the boundary of the  existing polygon and add your last vertex within the boundary of the polygon that you are adding to. The other vertices you add can be outside. When you finish the extra area will be added to the existing feature. 

11.	Click within the park boundary then click on the corner vertex in the northwest corner. Continue to trace the park boundary using the outside of the perimeter sidewalk as the border. As you’re working your way back south on the eastern side snap to the existing vertex on the northeast side.  Continue into the park boundary adding one more vertex to the interior of the existing park. Right click to finish. 

12.	Toggle off editing. Click Yes to save your changes to the parks layer. 

13.	Now you will make an edit to a line layer. Turn on the netcurr layer.

14.	Zoom into the location highlighted in Figure below.

![Roads, Parks and Aerial Photography](figures/Roads_Parks_and_Aerial_Photography.png "Roads, Parks and Aerial Photography")

15.	You will digitize the missing main road, shown in yellow in Figure below.

![Missing Road](figures/Missing_Road.png "Missing Road")

16.	Toggle on editing for netcurr.

17.	Set your Snapping options so that only netcurr is being snapped to, with a Mode of To Vertex and a Tolerance of 20 feet. 

18.	Using the Add Feature tool on the Editing toolbar ![Add Feature tool](figures/Add_Feature_tool1.png "Add Feature tool"),  digitize the new road making sure to snap to the roads at the northern and southern ends. Use the centerline of the road while digitizing.

19.	There are many attributes for this layer. You will just enter a few. Enter the STREETNAME as Park, the STREETDESI as Place, the STREETQUAD as SE and the COMMENTS as Lab 5. Click OK.

20.	Toggle off editing and Save.

###	5.	Conclusion

In this lab, you have successfully digitized information using the five-step digitizing process.  Additionally, you have recreated the original source data (scanned as a raster) in the vector format.  Digitizing can be a time-consuming and tedious process, but can yield useful geographic information.


###	6.	Discussion Questions

1.	What can contribute to errors in the georeferencing process?

2.	What other vector geometries (point/line/polygon) could be appropriate for digitizing a road?  In which instances would you use one vector geometry type over another?

3.	When you created the parcels shapefile you added a text field to hold the zoning codes. What are the possible field types?  Explain what each field type contains, and provide an example of a valid entry in the field.

4.	Aerial photography has a lot of information in it. What other features could you digitize off of the imagery in this lab? Explain what vector geometry you would use for each. 

###	7.	Challenge Assignment

You have successfully created the parcel data from a scanned map. You have also fixed the parks and roads data in this part of town. There are some sports facilities visible: two football fields and a baseball field. Create a new layer and digitize those three facilities (include the grassy field areas at a minimum). 

Create a simple page sized color map composition using the QGIS Desktop Print Composer showing your results. Show the parcels, sports facilities, parks, roads and aerial photography. Use Categorized styling to give a unique color to each zone code in the parcel data.  Include:

+	Title

+	Legend (be sure to rename your layers so that the legend will be meaningful.)

+	Date and Data Sources

You can credit the data sources as the City of Albuquerque and yourself. If you need to refresh your memory, review GST 101 Lab 4.

