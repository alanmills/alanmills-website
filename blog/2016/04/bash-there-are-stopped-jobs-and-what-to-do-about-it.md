Bash warning 'there are stopped jobs' and what to do about it
=============================================================
**Author:** Alan Mills  
**Date:** [14 April 2016 22:11](/blog/history/2016-04.md)  
**Tags:** [bash](/blog/categories/bash.md), [OS X El Capitan](/blog/categories/osx-10-11.md)   
**Status**: Publish

When exiting a Bash shell using the `exit` Bash command you may get the output
``` bash
$ exit
logout
There are stopped jobs.
```

A stopped job is a process that has temporarily been put into the background and therefore, is no longer running.  Processes are put into this stopped state by sending <kbd>Ctrl</kbd>+<kbd>Z</kbd> to the Terminal while the job is running.

What jobs are running?
----------------------

So what to do about these stopped jobs?  First of all lets find out what jobs are running by using the Bash `jobs` command?
``` bash
$ jobs
[1]  Stopped        ./install.sh
[2]- Stopped        ping alanmills.info
[3]+ Stopped        ping www.cadamus.com
```

From this output we can see three stopped jobs.  From here there are a number of options:
* [Resume the stopped job](resuming-the-job),
  * [And then killing the resumed job](killing-a-running-job),
* [Kill the stopped job](killing-a-stopped-job).

Resuming the job
----------------
Use the Bash `fg #` command to resume a background job.  So for instance if we run `fg 2`, the `ping alanmills.info` job will resume and the system will output:
```bash
$ fg 2
ping alanmills.info
64 bytes from 216.239.34.21: icmp_seq=1 ttl=41 time=976.006 ms
64 bytes from 216.239.34.21: icmp_seq=2 ttl=41 time=514.086 ms
64 bytes from 216.239.34.21: icmp_seq=3 ttl=41 time=433.043 ms
```
### Killing a running job
For this particular job, ping will continue pinging alanmills.info until we kill the job.  We kill the job by sending <kbd>Ctrl</kbd>+<kbd>C</kbd> to the Terminal while the job is running.
```bash
64 bytes from 216.239.34.21: icmp_seq=2 ttl=41 time=514.086 ms
64 bytes from 216.239.34.21: icmp_seq=3 ttl=41 time=433.043 ms
^C
--- alanmills.info ping statistics ---
21 packets transmitted, 21 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 177.689/608.437/1023.167/239.954 ms

```
Killing a stopped job
---------------------
There are two ways to kill a stopped job without resuming it first:
1. [Kill the process using the Process ID](Kill-the-process-using-the-Process-ID),
2. [Kill the process using the background process number](kill-the-process-using-the-background-process-number).

### Kill the process using the Process ID
The first thing we need to get is the PID for the stopped job.  Use the Bash `ps` command.
```bash
$ ps
PID TTY           TIME CMD
70479 ttys000    0:00.12 -bash
89455 ttys000    0:00.01 /bin/bash ./install.sh
90711 ttys000    0:00.01 ping www.cadamus.com
```

Looking at the list, the stopped command `ping www.cadamus.com` has `PID 90711`.  Therefore the stopped job can be killed using the Bash `kill PID` command, for this example `kill 90711`.
``` bash
$ kill 90711
[3]+  Terminated: 15        ping www.cadamus.com
```

### Kill the process using the background process number
The final option is also the quickest way to kill an individual stopped job.  Use the Bash `kill %#` command, in this example `kill %1` will kill the `./install.sh` stopped job.
```bash
$ kill %1
[1]+  Terminated: 15        ./install.sh
```
