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

