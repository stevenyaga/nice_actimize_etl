=======================
Control-m Common Tasks
=======================

Show list of running jobs
-------------------------
* Run cmd as Administrator then run the following commands

* Show all running jobs

```bash
ctmpsm -LISTALL
```

* Show all running jobs for a specific folder

```bash
ctmpsm -LISTAJFFLD <FOLDER NAME> e.g ctmpsm -LISTAJFFLD KEN_SSD_AML
```

Kill a specific job
-------------------

```bash
ctmkilljob -ORDERID <ORDERID>
```

Show a list of all Hosts
------------------------

```bash
ctmhostgrp -LIST
```

Show a list of all hosts on a specific host groups
--------------------------------------------------

```bash
ctmhostgrp -EDIT -HOSTGRP <HOST_GROUP> -APPLTYPE OS -VIEW e.g ctmhostgrp -EDIT -HOSTGRP SAM_HOST -APPLTYPE OS -VIEW 
```

Kill all jobs on a specific host
--------------------------------

```bash
ctmkilljob -HOSTID <HOST_ID> e.g  ctmkilljob -HOSTID nice-samudmbatch.ke.sbicdirectory.com
```
