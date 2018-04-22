# oversampling
Oversampling irregular satellite pixels to a regular grid. 

# Input data
The input data format is .mat, following the heritage of original matlab code. Python can readily handle .mat files through scipy.io.loadmat. These input data are called L2g files (level 2 data for gridding). The variables in these files are:

1. Latitude and Longitude of satellite pixel center.
2. Pixel geoometry. These are coordinates of the four pixel corners (rectangular pixels for OMI, OMPS) or the two elliptical axes and the rotational angle (oval pixels for IASI, CrIS).
3. Parameters for further filtering, e.g., cloud fraction, solar zenith angle, thermal contrast, across-track positions, etc.
4. Parameters for further grouping/categorization, e.g., wind, temperature, time (utc,  number of days from January 0, 0000), weekdays/weekends.
5. Volumn density and uncertainty for each satellite pixel.

# Key functions
## Loading function
This function loads L2g files, unfies the names and format of variables, does the intital quality check. The output will be variables ready for oversampling.

## Set up oversampling domain
This function sets up a mesh grid to which the L2g data are oversampled. This can be a simple lat-lon mesh, or projected mesh. In the latter case, the satellite pixel coordinates need to be transformed to the same projection.

## Super Gaussian spatial response function
This function takes in the pixel center, pixel geometry, and the mesh grid and generates a 2-D super Gaussian spatial response function (SRF) accordingly

## Weighted sum
This function loops over all useful L2g pixels, generate super Gaussian SRF for each pixel, and cumulatively aggregate three matrices, A, B, and D. D is a simple accumulation of all super Gaussian SRF; A is the accumulation of SRF divided by the sum of SRF and the uncertainty, then multiplied by the column density; B is only the accumulation of SRF divided by the sum of SRF and the uncertainty.

## Post processing
This function calculates the oversampled column density as A./B and plots/saves the results.
