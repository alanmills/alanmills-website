Writing and executing a Bash script
===================================
**Author:** Alan Mills  
**Date:** [13 April 2016 09:29](/blog/history/2016-04.md)  
**Tags:** [OS X El Capitan](/blog/categories/osx-10-10.md), [Bash](/blog/categories/bash.md), [Vim](/blog/categories/vim.md)   
**Status**: Publish


1. Using your text editor of choice create a bash script.  
  ``` bash
  $ vim myscript.sh
  ```
  Go into insert mode <kbd>i</kbd> and enter the bash script text
  ``` bash
  #!/bin/bash
  ping -c 1 alanmills.info
  ```
  Exit the insert mode <kbd>esc</kbd> and save and quit Vim <kbd>:wq</kbd>

2. Change the file permissions to enable the script to execute
  ``` bash
  $ chmod u+x myscript.sh
  ```

3. Execute the script
  ``` bash
  $ ./myscript.sh
  ```
  This will execute the script and output:
  ``` bash
  PING alanmills.info (216.239.34.21): 56 data bytes
  64 bytes from 216.239.34.21: icmp_seq=0 ttl=42 time=327.096 ms

  --- alanmills.info ping statistics ---
  1 packets transmitted, 1 packets received, 0.0% packet loss
  round-trip min/avg/max/stddev = 327.096/327.096/327.096/0.000 ms
  ```
