# Installed stack

## Ambari 
  Version: 2.1.0 from the latest Ambari repo at the time:

    http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.1.0/ambari.repo


## JDK
   Version: OpenJDK 1.8 select at Ambari setup 
    
    java -version
        openjdk version "1.8.0_77"
        OpenJDK Runtime Environment (build 1.8.0_77-b03)
        OpenJDK 64-Bit Server VM (build 25.77-b03, mixed mode)

## HDP
   Version: HDP-2.3.4.0-3485 select from Ambari
   
# Issues

## After configuring Namenode HA
   The Knox service cannot be started. Quits with a message:
   
    resource_management.core.exceptions.Fail: Execution of '/usr/hdp/current/knox-server/bin/knoxcli.sh create-master 
        --master [PROTECTED]' returned 1. Master secret is already present on disk. 
        Please be aware that overwriting it will require updating other security artifacts.  
        Use --force to overwrite the existing master secret.
        ERROR: Invalid Command
            Unrecognized option:create-master
            A fatal exception has occurred. Program will exit.
            
This seems to be a bug in Ambari:

    https://community.hortonworks.com/questions/7750/knox-gateway-start-fail.html?page=2&pageSize=10&sort=votes
    
The bug is fixed in Ambari 2.1.2, which was not in the latest Ambari repo at the time
of the installation. 
To fix it in the current Ambari version remove this file on ths host running the Knox gateway:

    sudo rm /usr/hdp/current/knox-server/data/security/master
    
 