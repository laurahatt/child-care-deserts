# Walking to Child Care in Lakeview, Chicago

## Executive Summary

Can families in Lakeview walk to child care in fifteen minutes or less? This analysis provides a partial answer to this question by generating estimates of child care availability at many specific geographic locations in Lakeview. It finds that potential competition for child care slots is extremely stiff, ranging from 0.00 to about 0.12 slots per child in different parts of the neighborhood. 

### Introduction

#### I. Conceptualizing Child Care Access

Child care access is a multi-faceted concept. In 2017, an expert working group offered this definition: “Access to early care and education means that parents, with reasonable effort and affordability, can enroll their child in an arrangement that supports the child’s development and meets the parents’ needs.”[^1] This definition of access can be broken down into numerous dimensions and sub-dimensions. Consider, for instance, the dimension of “reasonable effort”. Researchers consider child care to be accessible with reasonable effort if age-appropriate child care slots are available near parents’ homes or work and information about those slots is readily available. This definition incorporates at least four sub-dimensions: the availability of slots relative to children in need of care, the geographic location of providers, the ages of children served by each provider, and the availability of information about those providers.[^2]

In theory, a study of child care access in a particular region might examine every relevant dimension and sub-dimension. In practice, however, a single study usually examines just a few elements. According to one literature review, the most commonly studied sub-dimension of child care access is child care slot availability, which is usually assessed based on estimates of supply, demand, and/or utilization. Researchers also frequently studied access in terms of provider type, geographic location, care quality, and the availability of subsidies, among other concepts.[^3]

This analysis defines child care access in terms of availability and location. This is a very common way of measuring child care access; after all, spatial proximity to an available amenity is a necessary (though of course not sufficient) component of access.  The concept map below locates this narrow approach within a fuller conceptualization of child care access. Factors not incorporated within this analysis are shown in gray.

![Concept Map](/Diagrams/concept_map_v3.png)


#### II. Operationalizing Child Care Availability and Proximity

Both availability and proximity can be operationalized in many different ways. 

##### a. Availability

This analysis measures child care availability in terms of supply and demand. It estimates supply of child care based on the maximum capacity of all licensed daycare providers in a given area.[^4] It estimates demand for child care based on the number of families with children under six years old in a given area.[^5] Finally, it divides the number of slots hypothetically available to a family by the number of children hypothetically competing for those slots, producing a “slots per tot” ratio. The “slots per tot” ratio is a common measure of child care availability, although different studies calculate it in a host of different ways.[^6]

##### b. Proximity

Guided by the work of Davis, Lee & Sojourner (2019),[^7] this analysis operationalizes the spatial proximity of providers to families with a simplified version of the “two-stage floating catchment area” (2SFCA) approach. As the name suggests, this approach involves two stages. First, the analyst estimates the slots per tot at a particular provider, based on the number of children living within that provider’s catchment area. Second, the analyst estimates the total slots per tot available to each individual family, based on the slots per tot at all providers in that family’s catchment area. This approach generates many spatially-located estimates of child care access - one for each family. These points can be summarized, mapped, and interpolated.


#### III. The Fifteen-Minute Daycare

Counting the number of families or providers in a catchment area necessitates defining the size of the catchment area. But how far are families willing to travel for child care? One analysis, based on a national survey conducted in 2012, found that children under three travelled an average of 4.6 miles (as the crow flies) for center-based care, although distances varied considerably by child age, care type, and household income.[^8] It seems clear, given this distance, that many of these trips were taken by car. Accordingly, Davis, Lee & Sojourner (2019) conducted their 2SFCA analysis using catchment areas defined by a 20-minute drive.[^9]

This analysis uses a higher standard for child care proximity and defines catchment areas based on a 15-minute walk. This threshold is inspired by the “fifteen minute city” – an ideal place where all residents can access key amenities within a short walk of their home. Citing goals like improved accessibility and reduced car dependence, many cities in the U.S. and elsewhere have adopted some kind of x-minute goal, although the precise number of minutes (an arbitrary threshold) varies.[^10] Defining access based on a 15-minute walk time isochrone has various limitations; for example, it may overestimate access if families with young children are not able or willing to walk 15 minutes each way. Nonetheless, this analyst believes that mapping the 15-minute daycare can offer a fresh perspective on child care access in Lakeview and beyond. 


### Methods

This analysis is based on the following data sources: 
*	the official community area boundary of Lakeview;[^11]
*	the number of households with children under six per census block in 2010;[^12] and
*	a list of licensed private daycare providers in Cook County.[^13]

These data sources, their transformations, and their relationships to one another, are summarized in the entity-relationship diagram below.

![Entity-Relationship Diagram](/Diagrams/entity_relationship_diagram.png)

These data were transformed and analyzed with R as follows.

Step 1: Geocode child care providers
Download Chicago community areas boundary shapefile, select the Lakeview polygon, and save. Download list of child care providers, with addresses, from the Illinois Department of Children and Facility Services. Geocode addresses, using the tidygeocoder package to access the Census Bureau, OpenStreetMap, and ArcGIS geocoding services. Select all child care providers within one mile of Lakeview. Save shapefile.

Step 2: Count families with children under six per census block
Use the tidycensus library’s get_decennial() function to automatically download counts of households with children under six, by census block, based on the results of the 2010 decennial census. The counts are disaggregated by household type; reshape data from long to wide, so that each observation corresponds to one census block. Improve processing speed by downloading counts without spatial information, then download and merge spatial information for blocks. Save shapefile.

Step 3: Estimate precise locations of families
Load Lakeview shapefile and shapefile with number of families by census block. Select census blocks that fall at least partly within the Lakeview community area boundary; use st_intersects(), not st_intersection(), so as to preserve the geometry of census blocks that cross the Lakeview boundary. For each census block, use st_sample() to estimate exact family locations, assuming that families are randomly distributed within the block. Save shapefile. Use st_buffer() to create 1.5-mile buffer around Lakeview. Select census blocks that fall at least partly within this buffer. For each census block, use st_sample() to estimate exact family locations, assuming that families are randomly distributed within the block. Save shapefile.

Step 4: Estimate Slot-to-Population Ratio for each child care provider
Load child care provider shapefile and shapefile with families within 1.5 miles of Lakeview. Use a for-loop and the osrm (Open Source Routing Machine) package’s osrmIsochrone() function to create a fifteen minute walking radius around each child care provider, adding each isochrone to a list before proceeding. Use do.call(rbind) to turn the list into a dataframe. Use a for-loop and st_intersects() to count the number of families within each isochrone, adding each count to a list before proceeding. Add the list to the isochrone dataframe as a column. Multiply the number of families column by 1.94 to create a number of children column. Divide capacity by number of children to estimate slot-to-population ratio (SPR or “slots to tot”). Finally, add SPR column to original dataframe of provider point locations. Save shapefile.

Step 5: Estimate total supply available to each family
Load child care provider SPR shapefile and shapefile with locations of families in Lakeview. Select every tenth family (to speed up processing). Use a for-loop and the osrmIsochrone() function to create a 15-minute walking radius around each family. Use st_intersects() to select the child care providers that fall within the isochrone. Take the sum of the SPR of all providers in the isochrone to estimate Total Supply available to the family. Add Total Supply column to original dataframe of family point locations. Save shapefile.

Step 6: Interpolate point estimates of child care availability
Load shapefile with Lakeview boundary and shapefile with familes’ Total Supply. Use st_voronoi() to create Voronoi polygons around each family. Use st_intersection() to clip Voronoi polygons at Lakeview boundaries. Round Total Supply to two decimal places, then use group_by() and summarize() to combine polygons with equal values of Total Supply. Plot.


### Results

Very few families in Lakeview can walk from their homes to licensed child care in 15 minutes. As shown in the maps below, child care availability in Lakeview ranges from 0 to 0.12 slots per tot, based on the supply of providers within a 15 minute walk from family’s homes. Availability is highest in the southeast part of the neighborhood, where there are two relatively large providers. However, availability is zero in most of the rest of the neighborhood. 

![Map - Slots Per Child (Point)](/Maps/map_slots_per_child_point.png)

![Map - Slots Per Child (Voronoi)](/Maps/map_slots_per_child_voronoi.png)

![Map - Zero Slots (Point)](/Maps/map_zero_slots_point.png)

![Map - Zero Slots (Voronoi)](/Maps/map_zero_slots_voronoi.png)


### Discussion

As stated above, this analysis finds that child care access in Lakeview is generally very poor. In fact, these estimates (ranging from 0.00 to 0.12 slots per child) are far lower than those usually produced in child care access research. For instance, by one estimate, the median access ratio across the U.S. is 1.6 children per slot (0.625 slots per child) for families with preschool-aged children and 4.3 children per slot (0.233 slots per child) for families with an infant or toddler.[^14]

This study’s unusually low estimates are likely due, in part, to its many limitations. For instance:
* Supply: The list of licensed daycare providers published by the Illinois DCFS is an imperfect indicator of child care supply. This list excludes many types of child care providers, including parent and other relative providers, public programs like Head Start, Early Head Start, and public preschool, and various kinds of license-exempt providers. In addition, not all licensed daycare slots are available to all families, due to a host of obstacles to access such as provider age restrictions and cost of care. 
* Demand: The 2010 census count of families with children under six per census block is a deeply flawed indicator of child care demand. This count is already more than a decade out of date. Furthermore, many families with young children do not seek licensed care. Finally, even when a family does seek licensed daycare, that family likely will not compete for every nearby slot, for a host of reasons such as slot age restrictions and cost. 

This study’s relatively low estimates may also be due in part to its esoteric focus on foot travel. Theoretically, if children and child care were perfectly evenly distributed across a region, the slot-to-population ratio would remain approximately constant no matter how catchment areas were defined. In practice, children and child care slots are probably unevenly distributed across Chicago. This means that, although there are few slots within a short walk of Lakeview, there could be an area of dense child care supply (and relatively sparse local demand) a short drive away. An analysis that accounts for car travel could capture that supply; this analysis does not.

However, these estimates could reflect a real scarcity of child care in the Lakeview area. Indeed, an interactive child care map maintained by the Center for American Progress identifies most of the neighborhood as having “scarce” supply.[^15] This begs an important question: Why? It is possible that families in Lakeview are content to meet their child care needs without licensed providers; for example, they might hire nannies, rely on grandparents, or arrange for one parent to provide full-time unpaid care. Alternatively, Lakeview families might be experiencing a true child care shortage, fueled by factors such as high rents or “NIMBY” neighborhood opposition. Future analyses (conducted with better indicators of supply and demand) could add great value by exploring these and other potential explanations.



  [^1]: Friesen, S., Lin, V.-K., Forry, N., & Tout, K. (2017). Defining and Measuring Access to High Quality Early Care and Education: A Guidebook for Policymakers and Researchers (No. OPRE Report #2017-08). Office of Planning, Research and Evaluation, Administration for Children and Families U.S. Department of Health and Human Services. 
  [^2]: Thomson, D., Cantrell, E., Guerra, G., Gooze, R., & Tout, K. (2020). Conceptualizing and Measuring Access to Early Care and Education. OPRE Report #2020-106. Washington, DC: Office of Planning, Research, and Evaluation, Administration for Children and Families, U.S. Department of Health and Human Services.
  [^3]: Ibid.
  [^4]: Note that this is an imperfect indicator of child care supply. First: Many families obtain child care from providers other than licensed, private daycares. For instance, families may arrange some or all care through relative providers, license-exempt providers, public programs like Head Start, Early Head Start, or public preschool, and/or other sources. Second: Not all licensed daycare slots are available to all families, due to a host of obstacles to access such as provider age restrictions, cost, subsidy acceptance, language. As a result, the list of licensed daycare providers is likely an under-estimate of the full supply of child care available to a family.
  [^5]: Note that this is an imperfect indicator of child care demand. First: Many families with young children are not seeking licensed daycare. Second: Some families seeking licensed daycare may not compete for a given slot, due to family preferences and/or obstacles to access. As a result, the number of families with young children is likely an over-estimate of the full demand for child care.
  [^6]: See for example: Paschall, K., Davis, E. E., & Tout, K. (2021). Measuring and comparing multiple dimensions of early care and education access. OPRE Report #2021-08. Washington, DC: Office of Planning, Research, and Evaluation, Administration for Children and Families, U.S. Department of Health and Human Services
  [^7]: Davis, E., Lee, Won, & Sojourner, A. (2019). “Family-centered measures of access to early care and education.” Early Childhood Research Quarterly, vol. 47, pp. 472-486.
  [^8]: National Survey of Early Care and Education Project Team (2016). Fact Sheet: How Far Are Early Care and Education Arrangements from Children’s Homes? OPRE Report No. 2016-10, Washington DC: Office of Planning, Research and Evaluation, Administration for Children and Families, U.S. Department of Health and Human Services.
  [^9]: Davis, E., Lee, Won, & Sojourner, A. (2019). “Family-centered measures of access to early care and education.” Early Childhood Research Quarterly, vol. 47, pp. 472-486.
  [^10]: For discussion of x-minute city goals in 500 U.S. cities and 43 New Zealand urban areas, see: Logan, T., Hobbs, M., Conrow, M., Reid, N., Young, R., & Anderson, M. (2022). “The x-minute city: Measuring the 10, 15, 20-minute city and an evaluation of its use for sustainable urban design.” Cities, vol. 131.
  [^11]: Chicago Data Portal, Boundaries - Community Areas (current), downloaded 12 January 2023.
  [^12]: Data retrieved using the tidycensus get_decennial() API for the year 2010 and the following variables: P020005, P020006, P020010, P020011, P020014, P020015, P020019, P020020, P020023, P020024.
  [^13]: Illinois Department of Children and Family Services, Day Care Provider Lookup, downloaded 6 February 2023.
  [^14]: Paschall, K., Davis, E. E., & Tout, K. (2021). Measuring and comparing multiple dimensions of early care and education access. OPRE Report #2021-08. Washington, DC: Office of Planning, Research, and Evaluation, Administration for Children and Families, U.S. Department of Health and Human Services.
  [^15]: Interactive map available at https://childcaredeserts.org/.

