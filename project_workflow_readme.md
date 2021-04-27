# Andrew's Workflow for EOSC 595F -- Stratocumulus to Cumulus Transition with SCAM

This project uses the single column version of the Community Atmosphere Model (SCAM) explore stratocumulus to cumulus transitions with the well-studied CGILS experiments along a transect in the sub-tropical pacific. The real goal here is to get experience with SCAM, docker containers, and post processing with python and jupyter lab.

RESEARCH question: What causes the transition from s11 to s6, what is driving the stratocumulous to cumulous transition? are the dynamics accurately represented in the model (no, boundary layer is wrong)

## Helpful Links

[CGILS S11 reference paper](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2013MS000246)

[SCAM Tutorial](https://www.dropbox.com/sh/c9lketehyo6ywdn/AACPkcDX-oF5yA5zYN-pb7aJa?dl=0)

## Steps

### Get the Container Running

* connect to node07 as compstaff (required for write permission, this needs to be set up prior to executing these steps)
  
```bash
    SSH n7comp
```

* if at any time you wish to exit node07

```bash
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

* path to cases inside the container is: `/opt/ncar/inputdata/atm/cam/scam/iop`

* create/edit the case you want to run in [jupyterlab](http://node07.eos.ubc.ca:8445/lab?)

### Run the CGILS Control Cases (S6, S11, S12)

* In jupyterlab, locate the script called `create_scam6_iop`.Go to line 45, replace with `set IOP = cgilsS6`

* Open a terminal and run the case with `/home/scam/create_scam6_iop` Case should run in a few minutes
  
  * if you encounter "permission denied" error, do the following:
    * ```$ ls -ltd *``` to see file permissions. ```x``` is probably missing
    * ```$ chmod +x <name of model run>``` to enable execute permission
    * check it worked with ```$ ls -ltd *```, there should be an ```x``` where there was once a ```-```
    * try running the case again

* currently, output is stored at `/home/scam/work/cases/` inside the container

* repeat for `cgilsS11` and `cgilsS12`

Use ```scopy``` (copy the work folder to local directory?)

```bash
scp -r n7comp:/newtera/phil/scam/work .
```

## Experiment 1: Sweep of SST $\pm$ 5K for Each Case

SST is the main driver of stratocumulus to cumulus transition. Let's take each case, subtract 5K from the initial SST, and then run the model 10 times, adding 1K each iteration. At the end, restore the original IOP files.

### Make Backups of Unmodified IOPs, put them Somewhere

from the command line, do:

```python -c 'from SCT_fcn_library import *; backup_IOP()```

This will save a copy of the CGILS IOPs in

`/home/scam/andrew_2021/backup_iop/`

Note this should only be done once, and the function will raise an error if you try it after apr 25/21. If you really want to update the file backups, go into the function library and change the date in the assert statement. Now we can mess with the initial conditions and run more models. To revert to the original IOPs, do:

```python -c 'from SCT_fcn_library import *; restore_default_IOPs()```

### Proof of concept: Raise SST of CGILS12 by 10K

Raise SST of CGILS12 by 10K and see if clouds disappear. 

1) delete the old CGILS S12 output

  ```bash
  cd /home/scam/work/cases

  rm -r -f tutorial.FSCAM.cgilsS12
  ```

2) make sure IOPs are reset to defaults

```bash
python -c 'from SCT_fcn_library import *; restore_default_IOPs()
```

3) use `get_init_conds.ipynb` to modify `'Tg'` (temperature of ground?) variable in `S12_CTL_MixedLayerInit_reduced.nc`

4) run the model

   `/home/scam/create_scam6_iop.cgilsS12`

## TODO

* read and plot init files for S6, S11, S12 $\checkmark$

* convert netcdf to nparray or similar, read back in to directory $\checkmark$

* read init files, write them back out, confirm they have the same results (xarray netcdfwriter) $\checkmark$

* plot subsidence, wv flux, sensible latent heat flux, initial temp profile, radiative forcing, upstream advection

* from the init files and during the runs

* turn on and off drizzle, increase/decrease coelescence efficiency. equivalently, less cloud droplets so they get bigger -- tutorial exercise for messing with convective evaporation efficiency

* modulate subsidence, klein line, see if cloud layer breaks up

* models are overpredicting cloud cover, we suspect the BL scheme is lacking

* look at european centre lecture series on this subject

* the .nc files i want are at /opt/ncar/inputdata/atm/cam/scam/iop
