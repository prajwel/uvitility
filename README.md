# **UVITility**
> A python package with UVIT related utility functions.


You can install the UVITility Python package using the following command.

```bash
pip install uvitility --upgrade
``` 
	
> **IMPORTANT:** Even if you have UVITility already installed, make sure you use the latest version by running the above command. Current version of UVITility is shown on the badge below: <br> <a href="https://pypi.org/project/uvitility/"><img src="https://img.shields.io/pypi/v/uvitility?style=for-the-badge"/></a>

## Check for gaps in centroids of UVIT L1 data

The UVIT-Payload Operations Centre (UVIT-POC) has come across (in June 2021) the presence of gaps in the X-centroids and shifts of 3 pixels (~10") in the L1 data of UVIT. They are noticed for some orbits of a pointing. However, such gaps are not seen in the data of all OBSIDs. The presence of such gaps will lead to split images (separated by ~10") and shifted positions (by ~10") of sources in part of the field in the L2 images. This was found during visual examination of L2 images carried out as part of the quality checks done by the POC. It appears that ~20% of OBSIDs have this issue, and of those 20% of the OBSIDs, ~10%% of the orbits are affected. At the moment, the UVIT-POC suggests caution that this problem could be in any pointing during the whole history of observations, and individual PIs may like to look for it in their L1 data. The `check_centroid_gaps` function of UVITility can help to identify this defect.

After June 2021, in data sets where this issue is noticed, the POC has adopted the following approach 
- identify the orbit with centroid gaps, 
- if gaps are found, remove that particular orbit (hereinafter bad orbit), 
- create new merged L1 without the bad orbits and 
- run L2 on the new L1 and post the data back to ISSDC. 

This approach will lead to loss of L1 data, and thus creation of L2 images with less exposure time compared to the actual observations carried out. Those OBIDs affected by this problem, among others, will have the following statement in the DISCLAIMER.txt file which is part of the L2 bundle.

	"The data acquired at some orbits of the observation has 
	 gaps in X-centroids. In the L1 & L2 data downloadable 
     from ISSDC, those orbit data will be missing. In effect, 
     this has led to X% of loss in the acquired data."

The X% is calculated as follows,
X% = (UV_L1 - UV_L1_mod) * 100 / UV_L1

where,

UV_L1 = original L1 file 

UV_L1_mod = modified L1 file after removing corrupt data

**The checks have not been carried out on L1 datasets processed at the UVIT POC and sent to ISSDC before June 2021.** Therefore, we advise the users of UVIT data to check their L1 data for the defect using the `check_centroid_gaps` function of UVITility. It can be done as follows. 

1. Download the L1 dataset of your observation from ISSDC’s [AstroBrowse website](https://astrobrowse.issdc.gov.in/astro_archive/archive/Home.jsp). 
2. Extract the compressed L1 dataset into a directory. 
3. Run the `check_centroid_gaps` function on a Python command prompt or as a script. For example, 
```python
>>> import uvitility
>>> uvitility.check_centroid_gaps('path to the extracted L1 directory')
```
4. Finally, check the plots produced by the `check_centroid_gaps` function to find out whether your L1 dataset is affected by the gap problem. 

### Requirements

UVITility works with Python 3.6 or later. UVITility depends on the following packages:

* astropy
* matplotlib
* numpy

