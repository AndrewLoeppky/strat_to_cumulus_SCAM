# Andrew's Workflow for EOSC 595F -- Wildfire SCM Project

The goal of this project is to use the single column version of the Comminity Atmosphere Model (SCAM) to simulate the effects of intense wildfire smoke events in a radiative-convective scheme. The current workflow is to run the SCAM model on UBC's node07 via SSH, and do the necesarry post-processing in python on Andrew's local machine

## Cheat sheet

open a windows terminal

Connect to node07
```
$ SSH n7
```

Connect to node07 as compstaff (required for write permission)
```
$ SSH n7comp
```

Exit node07
```
Ctrl + d
```

Once connected, navigate to ```/newtera/phil/scam/work/```


Use ```scopy``` (copy the work folder to local directory?)
```
$ scp -r n7comp:/newtera/phil/scam/work .
```

Connect to node07 or node07 compstaff from VScode

open vscode, ```ctrl + p``` and select ```SSH connect to host```

select ```n7``` or ```n7comp```

```file``` -> ```open``` -> ```/newtera/phil/scam/work```


## SCAM Tutorial

 https://www.dropbox.com/sh/c9lketehyo6ywdn/AACPkcDX-oF5yA5zYN-pb7aJa?dl=0