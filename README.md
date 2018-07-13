# docker-samples
Sample setup with a container running an OverOps remote collector and a container running an application monitored by an OverOps agent that reports to the remote collector container

## attach.sh 
Script that lets you jump into a shell session on any running container.  
Usage:  
grab the container ID from docker ps:  
$ docker ps  
CONTAINER ID        IMAGE               COMMAND                  CREATED                  STATUS              PORTS                    NAMES  
**063b9772a7e1**        agent               "/bin/sh -c 'java -a…"   Less than a second ago   Up 1 second                                  gifted_hugle  
d9cba594f3b7        collector           "/bin/sh -c '/opt/ta…"   4 seconds ago            Up 4 seconds        0.0.0.0:6060->6060/tcp   dreamy_lovelace  
$ ./attach.sh **063b9772a7e1**  
root@linuxkit-025000000001:/# ls  
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  scala-boom.jar  srv  sys  tmp  usr  var  
root@linuxkit-025000000001:/# ps -ef  
UID        PID  PPID  C STIME TTY          TIME CMD  
root         1     0  0 18:47 ?        00:00:00 /bin/sh -c java -agentlib:TakipiAgent -Dtakipi.log.file=/tmp/overops.log -jar scala-boom.jar  
root         6     1  2 18:47 ?        00:00:03 java -agentlib:TakipiAgent -Dtakipi.log.file=/tmp/overops.log -jar scala-boom.jar  
root        26     0  0 18:49 pts/0    00:00:00 /bin/bash  
root        35    26  0 18:49 pts/0    00:00:00 ps -ef  
root@linuxkit-025000000001:/#  
  
## How to Run
There are two folders: _agent_ and _collector_  
Each folder contains the following:  
* **Dockerfile** the dockerfile that has the commands to create the proper image
* **build.sh** a script that builds the Dockerfile into an image
* **run.sh** a script that runs the image for that folder as a detached conatiner  with the proper environmental variables and port mappings
* Any local files the docker image may need. E.g. agent has *takipi.deb* which is copied to the image during build and is used to install the agent files 
  
To get this sample running
* Modify collector/build.sh so that *--build-arg TAKIPI_SECRET_KEY=SAMPLE_KEY* uses your key in place of the value occupied by the *SAMPLE_KEY*
* Then run the scripts in this order: collector/build.sh ; collector/run.sh ; agent/build.sh ; agent/run.sh
