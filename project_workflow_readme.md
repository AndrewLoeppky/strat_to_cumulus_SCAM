# Andrew's Workflow for EOSC 595F -- Wildfire SCM Project

The goal of this project is to use the single column version of the Comminity Atmosphere Model (SCAM) to simulate the effects of intense wildfire smoke events in a radiative-convective scheme. The current workflow is to run the SCAM model on UBC's node07 via SSH, and do the necesarry post-processing in python on Andrew's local machine

## Steps

* connect to node07 as compstaff (required for write permission, this needs to be set up prior to executing these steps)
```
$ SSH n7comp
```

if at any time you wish to exit node07
```
Ctrl + d
```

* once connected, navigate to ```/newtera/phil/scam/work/```

* check if the container is running with
  `docker ps`, look for `scam_tutorial_v1.2`

* start the container (if it's not already running)
  `docker run -p 8445:8888 -v /newtera/phil/scam:/home/scam/work -d scam_tutorial_v1.2`

* confirm it's running
  `docker ps`

* log in from a terminal with:
  `docker exec -it friendly_goldwasser /bin/bash`

* path to cases inside the container is:
  `/opt/ncar/inputdata/atm/cam/scam/iop`

* create/edit the case you want to run in [jupyterlab](http://node07.eos.ubc.ca:8445/lab?)

* run the case with `/home/scam/create_scam6_iop`, replace `create_scam6_iop` with your own case

* if you encounter "permission denied" error, do the following:
  * ```$ ls -ltd *``` to see file permissions. ```x``` is probably missing
  * ```$ chmod +x <name of model run>``` to enable execute permission
  * check it worked with ```$ ls -ltd *```, there should be an ```x``` where there was once a ```-```
  * try running the case again

* currently, output is stored at `/home/scam/work/cases/` inside the container

* or, locate the output outside with `find . -iname "*<runname>*nc -ls`, it will look something like: `./cases/tutorial.FSCAM.cgilsS11/run/tutorial.FSCAM.cgilsS11.cam.h0.1997-07-15-00000.nc`

* for tips on exploring the output in python/xarray, copy the path into whatever version of `rscam.ipynb` and modify/run cells as needed


CGILS S11 reference paper:  https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2013MS000246

Use ```scopy``` (copy the work folder to local directory?)
```
$ scp -r n7comp:/newtera/phil/scam/work .
```



Connect to node07 or node07 compstaff from VScode -- *not currently working*

open vscode, ```ctrl + p``` and select ```SSH connect to host```

select ```n7``` or ```n7comp```

```file``` -> ```open``` -> ```/newtera/phil/scam/work```


## SCAM Tutorial

 https://www.dropbox.com/sh/c9lketehyo6ywdn/AACPkcDX-oF5yA5zYN-pb7aJa?dl=0

## TODO

* read and plot init files for S6, S11, S12

* convert netcdf to nparray or similar, read back in to directory

* read init files, write them back out, confirm they have the same results (xarray netcdfwriter)

* plot subsidence, wv flux, sensible latent heat flux, initial temp profile, radiative forcinfg, upstream advection

* from the init files and during the runs

* RESEARCH question: What causes the transistion from s11 to s6, what is deiving the stratocumulous to cumulous transition? are the dynamics accurately represented in the model (no, boundary layer is wrong)

* turn on and off drizzle, increase/decrease coelescence efficiency. equivalently, less cloud droplets so they get bigger

* modulate subsidence, klein line, see if cloud layer breaks up

* models are overpredicting cloud cover, we suspect the BL scheme is lacking 

* look at european centre lecture series on this subject
