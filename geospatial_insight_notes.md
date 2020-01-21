# Estimating the Heights of Buildings from Satellite Imagery

## Chris Nix (Geospatial Insight)

---

#### Team Members

* Chris Budd (University of Bath)
* Chris Nix (Geospatial Insight)
* Edward Pearce (University of Sheffield)
* Sachin Reddy (University of Bath)
* Lorna Wilson (University of Bath)
* Tina Zhou (University of Bath)

### Research Question to Address?

- Building height is calculated as the difference between roof height and ground height based on the DSM model. Are there ways to improve how roof height and ground height are calculated?
  - Max/min/mean/median of a selection of roof points has been trialled
  - Ground points are sampled from a 10m neighbourhood around the building footprint


### Task List

Can be viewed as a geometry/computer vision problem. Some research and software already available

### Resources

rasterIO - python - for coverting .tif to numpy array
QGIS - for viewing .tif

##### Software packages

- [OpenCV-Python Tutorials](https://docs.opencv.org/4.2.0/d6/d00/tutorial_py_root.html)
  - [Camera Calibration and 3D Reconstruction](https://docs.opencv.org/4.2.0/d9/db7/tutorial_py_table_of_contents_calib3d.html)
- [Rasterio](https://rasterio.readthedocs.io/en/latest/) can convert .tif files to numpy arrays
  > Geographic information systems use GeoTIFF and other formats to organize and store gridded raster datasets such as satellite imagery and terrain models. Rasterio reads and writes these formats and provides a Python API based on Numpy N-dimensional arrays and GeoJSON.
- [PyQGIS](https://docs.qgis.org/testing/en/docs/index.html) for analysing geospatial data, viewing .tif files
  - [Cookbook tutorial](https://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/intro.html)
  - [External tutorial](https://www.qgistutorials.com/en/docs/3/getting_started_with_pyqgis.html)

##### General background

- [Digital Elevation Model (DEM)](https://en.wikipedia.org/wiki/Digital_elevation_model): Distinction between Digital Terrain Model (DTM) representing bare ground and Digital Surface Model (DSM) including buildings, trees, other objects. ![DEM types](https://upload.wikimedia.org/wikipedia/commons/6/6c/DTM_DSM.svg)
- [Photogrammetry](https://en.wikipedia.org/wiki/Photogrammetry)
- NASA [Shuttle Radar Topography Mission (SRTM)](https://en.wikipedia.org/wiki/Shuttle_Radar_Topography_Mission): 1-arc second global digital elevation model (30 meters)

##### Mathematical techniques

###### [Pinhole Camera model](https://en.wikipedia.org/wiki/Pinhole_camera_model)
- [Triangulation (computer vision)](https://en.wikipedia.org/wiki/Triangulation_(computer_vision))
> In computer vision triangulation refers to the process of determining a point in 3D space given its projections onto two, or more, images. In order to solve this problem it is necessary to know the parameters of the camera projection function from 3D to 2D for the cameras involved
- [Epipolar geometry](https://en.wikipedia.org/wiki/Epipolar_geometry) Wikipedia page ![Epipolar geometry ideal case](https://upload.wikimedia.org/wikipedia/commons/1/14/Epipolar_geometry.svg)
- [Epipolar geometry lecture](http://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/OWENS/LECT10/node3.html)
- [Bundle adjustment](https://en.wikipedia.org/wiki/Bundle_adjustment)
- [Levenberg–Marquardt algorithm](https://en.wikipedia.org/wiki/Levenberg%E2%80%93Marquardt_algorithm)
###### [Pushbroom camera model](https://en.wikipedia.org/wiki/Push_broom_scanner) 
> A **push broom scanner**, also known as an **along-track scanner**, is a device for obtaining images with spectroscopic sensors.
The moving scanner line in a traditional photocopier (or a scanner or facsimile machine) is also a familiar, everyday example of a push broom scanner.
##### [Rational Polynomial coefficient (RPC) model](https://en.wikipedia.org/wiki/Rational_polynomial_coefficient)
> "The RPC model forms the co-ordinates of the image point as **ratios of cubic polynomials** in the co-ordinates of the world or object space or ground point. A set of images is given to determine the set of polynomial coefficients in the RPC model to minimise the error". - [Rational polynomial modelling for cartosat-1 data (PDF)](https://www.isprs.org/proceedings/XXXVII/congress/1_pdf/152.pdf)
  - [How are RPCs calculated?](https://gis.stackexchange.com/questions/180414/how-rational-polynomial-coefficientsrpcs-are-calculated-need-references) - GIS stack exchange
  - [On RPC model of satellite imagery](https://www.researchgate.net/publication/248137804_On_RPC_model_of_satellite_imagery)
  - [RPC-Based Orthorectification for Satellite Images Using FPGA](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6111837/) (nonessential)

The rational polynomial coefficient model is a rational function, described by ratios of cubic polynomials, which maps from object space to image space.
Mathematically, this can be formulated as 
$$q:[-1,1]^{3} \to [-1,1]^{2}, \, q(x,y,z)=(s,p)$$ $$s(x,y,z) = \frac{P_{1}(x,y,z)}{P_{2}(x,y,z)}, \, p(x,y,z) = \frac{P_{3}(x,y,z)}{P_{4}(x,y,z)}$$ $$P_{1},P_{2},P_{3},P_{4}\in\mathbb{R}[x,y,z]_{3}$$ where $x,y,z$ are the normalized object space coordinates, i.e normalized latitude, longitude and height respectively. $s$ and $p$ are the normalized scanline number and pixel number between $[-1,+1]$. 

The $P_{i}$ are cubic polynomials on three variables, and so each can be described by $20$ coefficients: $$\begin{equation}\begin{split}
P_{i}(x,y,z) & = a_{i,0} + a_{i,1}x + a_{i,2}y + a_{i,3}z \; + \\
 & a_{i,4}x^{2} + a_{i,5}xy + a_{i,6}xz + a_{i,7}y^{2} + a_{i,8}yz + a_{i,9}z^{2} \; + \\
 & a_{i,10}x^{3} + a_{i,11}x^{2}y + a_{i,12}x^{2}z +  a_{i,13}xy^{2} + a_{i,14}xyz \, + \\
 & a_{i,15}xz^{2} + a_{i,16}y^{3} + a_{i,17}y^{2}z + a_{i,18}yz^{2} + a_{i,19}z^{3}
\end{split}\end{equation}$$ and without loss of generality, we can assume the constant coefficients in the denomiator polynomials are equal to $1$. That is, $a_{2,0} = a_{4,0} = P_{2}(0,0,0) = P_{2}(0,0,0) = 1$

These relate to maps from $\mathbb{R}^{3}\to\mathbb{R}^{2}$ by an additional $10$ coefficients for normalization: For each coordinate, we subtract the mean value, then divide by a scaling factor equal to the maximum absolute value. For example, $X=\frac{\phi-\mu_{\phi}}{S_{\phi}}$ where $S_{\phi}=\max(|\phi_{\max} - \mu_{\phi}|, |\phi_{\min} - \mu_{\phi}|)$ where $\phi$ is the prenormalized latitude. We call $S_{\phi}, \mu_{\phi}$ the scale and offset parameters, respectively.

Such RPC models are typically fit to data by using an optimaization technique to minimize the (mean) squared error.

### Progress Against Tasks

- Sampling DSM heights across 1D line slices which pass through the target building to get a 1D height profile for analysis (repeat for multiple directions/incidence angles)
- New idea: Photogrammetry techniques and DSM generation techniques both introduce predictable forms of error relative to the true location of points in object space, and it should be possible to reverse this blurring using standard techniques from signal processing, such as deconvolution using Fast Fourier Transform (FFT)

### Next Steps

- Alternative approach: Shadow-length model to estimate building height, but may be more difficult in urban areas due to shadow occlusion and shadows falling onto other buildings
- [Digital Globe](https://www.digitalglobe.com/) is an American commercial vendor of space imagery and geospatial content, and operator of civilian remote sensing spacecraft. - [Wikipedia](https://en.wikipedia.org/wiki/DigitalGlobe). Sentinel-2 provides higher resolution publically available geospatial data in comparison to SRTM
