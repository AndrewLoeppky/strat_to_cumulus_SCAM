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

 * do problem set 3, exercise 2 - modulating radiative forcing, maybe stop the sun too

* run cgilsS11 (statoculumus), S12 (coastal stratus), S6 (trade cumulus)