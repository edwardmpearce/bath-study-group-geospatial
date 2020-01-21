# Optimal Placement of Trees

## Simon Doxford (Natural England)

---

#### Team Members

* Tosin Babasola (University of Bath)
* Eduard Campillo-Funollet (Sussex University)
* Jonathan Dawes (University of Bath)
* Simon Doxford (Natural England)
* Laura Hattam (University of Bath)
* Anush Poshosyan (University of Bath)
* Christian Rohrbeck (University of Bath)
* Shaerdan Shataer (University of Bath)

### Aside - EP

The problem as initially stated has massive scope for interpretation, with an immense number of factors to consider - ecological, legal, economical, political, ...

When considering reforestation for climate change mitigation specifically, reforestation stands to have the greatest global impact 
in countries with large areas of suitable land and also in tropical regions due to biophysical considerations.
See [here](https://en.wikipedia.org/wiki/Reforestation#For_climate_change_mitigation) for further details on reforestation for climate change mitigation, 
and read [this](https://founderspledge.com/research/fp-climate-change) for a thorough assessment of various climate related interventions.
It should be noted that reforestation alone is insufficient to completely counteract carbon emissions, and that reduction in greenhouse gas emissions 
on both a national and international scale will also be necessary.

### General background

- https://en.wikipedia.org/wiki/Forestry
- https://en.wikipedia.org/wiki/Reforestation
- https://forestrycommission.blog.gov.uk/2019/10/29/trees-for-zero/
- https://www.gov.uk/government/organisations/natural-england
- https://www.gov.uk/government/organisations/forestry-commission
- https://www.forestresearch.gov.uk/

### Further references

- [Use and transfer of forest reproductive material in Europe in the context of climate change][euforgen]
- https://www.gov.uk/government/collections/forestry-commission-operations-notes-england
- https://www.forestresearch.gov.uk/research/archive-the-role-of-forest-genetic-resources-in-helping-british-forests-respond-to-climate-change-2/
- https://www.sciencedirect.com/science/article/pii/S037811271400231X#bb0350
- https://www.forestresearch.gov.uk/research/biocore-interactiveadaptable-landscape-ecology-approach-targeting-restoration/

> "In the United Kingdom, the Forestry Commission publishes Information Notes, which provide recommendations for provenance selection for UK conditions. 
The first choice of provenance should be a “selected” seed stand, preferably growing in the same region of provenance as the planting site. 
As an alternative, material from selected stands in northwestern Europe (Netherlands, northwest France, northern Germany and Belgium) is indicated. 
Seed stock from central and eastern continental Europe should not be planted, as it is poorly adapted to the UK in terms of growth rate, 
survival, phenology and resistance to foliar diseases. In some respects, the climatic dimension is taken into account by the statement that northward 
movement of genetic material (by hundreds of kilometres) may result in a gain in vigour compared with local-source provenances, whereas any 
southward movement is considered ill advised." - ([source][euforgen])

[euforgen]: http://www.euforgen.org/fileadmin/templates/euforgen.org/upload/Publications/Thematic_publications/EUFORGEN_FRM_use_transfer.pdf

### Research Question to Address?

Background: the Committee on Climate Change suggests an ambition for England should be to plant 10,000 hectares of trees per year. Given that NE have identified approx 3.4 million hectares of "low risk" land (in England?) this equates to approx 300 years of tree planting. 

In the recent past we have not managed to get up to as many as 10,000 hectares per year - 6,000 per year at the most, it seems:
![](https://i.imgur.com/1luU5yf.png)

Research Question: Where are the optimal locations to plant the annual allocation of 30,000 ha?

### Approach to the Research Question:

Three key criteria for the value of new woodland are:
- biodiversity
- commercial revenue
- water management, e.g. flood risk reduction.

General objectives are to maximise the carbon capture ('Net Primary Production') of the woodland that is planted, and to minimise the level of incentives/subsidies required in order to achieve these goals.

## Initial Task List (Tuesday 21, lunchtime - needs updating)

1a. Understand the definition of "low risk" land
1b. Understand how to place a value on each of the 3 primary criteria: biodiversity, commercial revenue, flood risk reduction.
1c. Explore the available model for Net Primary Production and investigate sensitivities to parameters

Note: NE has a 'Priority Places' dataset

## 2. The 'woodland band' model. (Jon/Eduardo)

### 2.1 The basic model

Suppose a circularly symmetric city produces air pollution (e.g. particulate matter) that is absorbed by a 'green lung' belt of woodland surrounding it. Let the radius of the city be $r_0$, and the woodland occupy the belt $r_0 \leq r \leq r_w$. For the simplest possible model, assume that the city emits uniformly and that the woodland absorbs uniformly; so that the air pollution density $\rho(r,t)$ evolves according to

\begin{equation}
\frac{\partial \rho}{\partial t} = \kappa \nabla^2 \rho + \mathbb{1}_{\{r<r_0\} } - \alpha s(v) \qquad\qquad\qquad (\ast)
% \label{eqn:rho}
\end{equation}

where $s(v)$ describes the absorption of pollution for a given level of woodland vegetation. We propose the functional form
$$ s(v) = \frac{v}{v_0+ v} $$
so that the absorption of the woodland saturates at the value 1 when $v$ becomes large. We also need to impose the constraint that the vegetation level $v(r,t)$ always lies in the range $0 \leq v(r,t) \leq V_{max}$.

Parameters: $r_0$, $r_w$, $\alpha$, $v_0$, $V_{max}$. Note that we should take $v_0<V_{max}$ for the saturation effect to come into play.

### 2.2 Possible extensions to the model

Include biodiversity by replacing $v$ by a collection of $n$ different species with vegetation levels $v_1,\ldots, v_n$, each evolving according to a diffusion equation with local interaction terms describing competition between species for resources.

Include wind $\mathbf{u}$ by adding the advection term $\mathbf{u} \cdot \nabla \rho$ to the left hand side of $(\ast)$.

### People working on tasks

1. Eduardo - Python code for the 'woodland band' model.
2. Jon - analytic solution for the 'woodland band' model.
3. 
4.
5. Shaerdan - detailed NPP model.

### Progress Against Tasks

- Links to the BIOME-BGC model: https://www.ntsg.umt.edu/project/biome-bgc.php
- Slides writen so far: https://www.overleaf.com/project/5e26ee4ee4390500013d8674
- NASA NPP data: https://earthobservatory.nasa.gov/global-maps/MOD17A2_M_PSN/CERES_NETFLUX_M

A planning tool for tree species selection and planting schedule in forestation projects considering environmental and socio-economic benefits
https://www.sciencedirect.com/science/article/pii/S030147971731040X

ForestSim: Spatially explicit agent-based modeling of non-industrial forest owner policies
https://www.sciencedirect.com/science/article/pii/S2352711018302310

### Data Links 

Excel tables from Forestry Statistics 2019
https://www.forestresearch.gov.uk/tools-and-resources/statistics/forestry-statistics/

Indicative Flood Risk Areas
https://environment.data.gov.uk/DefraDataDownload/?mapService=EA/IndicativeFloodRiskAreas&Mode=spatial

National Forest Estate Soil England 
https://www.mediafire.com/file/0agx7m1qyp4w425/NATIONAL_FOREST_ESTATE_SOIL_ENGLAND.zip/file

Ancient Woodland Data
https://environment.data.gov.uk/DefraDataDownload/?mapService=NE/AncientWoodlandEngland&Mode=spatial

- https://www.gov.uk/guidance/how-to-access-natural-englands-maps-and-data

### Low risk areas (Forestry Commission data)

![](https://i.imgur.com/64b7Mdm.png)

Plotted using Geopandas. TODO: intersection with other data sets.

### Next Steps

