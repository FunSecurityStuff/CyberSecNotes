
<html>
  <head>
 <div data-target="readme-toc.content" class="Box-body px-5 pb-5">
   <h1></b>Awesome CyberSecurity Notes</a></h1>
            <p>     </p>
<p> A curated list of amazingly  resources related cyber security.</p>
<h2><a id="user-content-table-of-contents" class="anchor" aria-hidden="true" href="#table-of-contents"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a>Table of Contents</h2>
<ul>
<li><a href="#-BlueTeam">Blue Team Resources</a></li>
<li><a href="#-RedTeam">Red Team Resources</a></li>
<li><a href="#-miscellaneous">Miscellaneous</a></li>
</ul>
 <h2><a id="user-content--datasets" class="anchor" aria-hidden="true" href="#-BlueTeam"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a><a href="#table-of-contents">↑</a> Blue Team Resources</h2>

   <h3><a href="#table-of-contents">↑</a> Windows Cheat Sheet</h3>
 
  <p> <h3><a href="#table-of-contents">↑</a> Triage on a live system</h3></p>
   <p><b><a href="#table-of-contents">↑</a> Gather System Information</b></p>   
   
       get-computerinfo
       echo %DATE% %TIME% 
       date
       time
       reg query "HKLM\System\CurrentControlSet\Control\TimeZoneInformation"
       systeminfo
       wmic computersystem list full
       wmic /node:localhost product list full /format:csv
       wmic softwarefeature get name,version /format:csv
       wmic softwareelement get name,version /format:csv
       reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /s
       echo %PATH%
       wmic bootconfig get /all /format:List
       wmic computersystem get name, domain, manufacturer, model, numberofprocessors,primaryownername,username,roles,totalphysicalmemory /format:list
       wmic timezone get Caption, Bias, DaylightBias, DaylightName, StandardName
       wmic recoveros get /all /format:List
       wmic os get /all /format:list
       wmic partition get /all /format:list
       wmic logicaldisk get /all /format:list
       wmic diskdrive get /all /format:list
       fsutil fsinfo drives
    
   <p><b><a href="#table-of-contents">↑</a> Process Info</b></p>   
       
       Get-CimInstance Win32_StartupCommand | Select-Object Name, command, Location, User | FL
       wmic startup list full
       tasklist
       wmic process list full /format:csv
       wmic process get name,commandline, parentprocessid,processid /format:csv
       wmic process get ExecutablePath,processid /format:csv
       wmic process get name,ExecutablePath,processid,parentprocessid /format:csv | findstr /I "appdata"
       wmic process where processid=[pid] get parentprocessid
       wmic process where processid=[pid] get commandline
       pslist
   
   
   
   <p><b><a href="#table-of-contents">↑</a> User Logon info</b></p>    
   
       qwinsta
       query user
       
   <p><b><a href="#table-of-contents">↑</a> Gather User Information</b></p>   
   
       whoami
       whoami /priv
       whoami /user
       net users
       net localgroup
       net localgroup administrators
       wmic useraccount get name, SID
       wmic useraccount list
  
  <p><b><a href="#table-of-contents">↑</a> Services</b></p>   
      
      wmic service list full
      net start
      sc query
      psservice
                
  </a><h3><a href="#table-of-contents">↑</a> Linux Cheat Sheet</h3>
 
 <p><b><a href="#table-of-contents">↑</a> show ALL users (includes LDAP)</b></p>   
      
     getent passwd
     
 <p><b><a href="#table-of-contents">↑</a> Lock a user account</b></p>   
     
     usermod -L <user>
     
 <p><b><a href="#table-of-contents">↑</a> Unlock a user account</b></p>   
     
     usermod -U <user>

 <p><b><a href="#table-of-contents">↑</a> Set a shell for a user</b></p>   
     
     usermod -s /bin/bash <user>
  
 <p><b><a href="#table-of-contents">↑</a> Collect running process list</b></p>   
     
     ps | awk -F" " '{print $1 " " $4}' > /tmp/processes.txt
     
     systemctl list-units
     
     service --status-all
     
<p><b><a href="#table-of-contents">↑</a>Check listening services</b></p>   
    
     netstat -tunap | grep LIST
     
     sudo netstat -t --listening -p
     
     sudo netstat -u --listening -p 
     
     ufw show listening
     
     ss -tnlp
     
     netstat -tnlp
     
 <p><b><a href="#table-of-contents">↑</a>Show active network connections to/from your services</b></p>    

    lsof -i
    
    netstat -plant
    
 <p><b><a href="#table-of-contents">↑</a>Show ARP table</b></p>    
 
    arp -a
    
  <p><b><a href="#table-of-contents">↑</a>Show routes</b></p>    
  
    route -n
    
  <p><b><a href="#table-of-contents">↑</a>Check scheduled tasks</b></p>    
    
    systemctl list-timers
    
    ls -larth /etc/cron*
    
    ls /var/spool/cron/*
    
  <p><b><a href="#table-of-contents">↑</a>SSH Keys and Authorised Users</b></p>
  
    cat /etc/ssh/sshd_config
    
    ls /home/*/.ssh/*

    cat /home/*/.ssh/id_rsa.pub

    cat /home/*/.ssh/authorized_keys
    
    
  <p><b><a href="#table-of-contents">↑</a>Sudoers File (who who can run commands as a different user)</b></p>
    
    cat /etc/sudoers

  <p><b><a href="#table-of-contents">↑</a>Search files recursively in directory for keyword</b></p>
    
    grep -H -i -r "password" /
    
  <p><b><a href="#table-of-contents">↑</a>Process Tree</b></p>
  
    ps -auxwf
  
  <p><b><a href="#table-of-contents">↑</a>Open Files and space usage</b></p>
  
    lsof
    du
   
 <p><b><a href="#table-of-contents">↑</a>Detailed Process Information</b></p>
  
    ls -al /proc/[PID]
  
 <p><b><a href="#table-of-contents">↑</a>SUID/SGID and Sticky Bit Special Permissions</b></p>
 
    find / -type f \( -perm -04000 -o -perm -02000 \) -exec ls -lg {} \;
  
 <p><b><a href="#table-of-contents">↑</a>Persistent Areas of Interest</b></p>
 
    /etc/rc.local
    /etc/initd
    /etc/rc*.d
    /etc/modules
    /etc/cron*
    /var/spool/cron/*
    /usr/lib/cron/
    /usr/lib/cron/tabs




    
  
    



    
 <h2><a id="user-content--datasets" class="anchor" aria-hidden="true" href="#-RedTeam"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a><a href="#table-of-contents">↑</a> Red Team Resources</h2>


           <p> Blank for now</p>

 <h2><a id="user-content--datasets" class="anchor" aria-hidden="true" href="#-miscellaneous"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a><a href="#table-of-contents">↑</a> Miscellaneous</h2>
