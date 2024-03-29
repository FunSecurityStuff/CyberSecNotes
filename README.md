
<html>
  <head>
 <div data-target="readme-toc.content" class="Box-body px-5 pb-5">
   <h1></b>Awesome CyberSecurity Notes</a></h1>
            <p>     </p>
<p> Notes for IR (Windows and Linux commands).</p>
<h2><a id="user-content-table-of-contents" class="anchor" aria-hidden="true" href="#table-of-contents"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a>Table of Contents</h2>
<ul>
<li><a href="#-BlueTeam">Blue Team Resources</a></li>
<li><a href="#-RedTeam">Red Team Resources</a></li>
<li><a href="#-miscellaneous">Miscellaneous</a></li>
</ul>
 <h2><a id="user-content--datasets" class="anchor" aria-hidden="true" href="#-BlueTeam"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a><a href="#table-of-contents">↑</a> Blue Team Resources</h2>

  <H3> <p><b><a href="#table-of-contents">↑</a>CrowdStrike Threat Hunting Queries</b></p></h3>
<table>
    <tr>
      <td><b>Description</b></td>
      <td><b>Query</b></td></tr>
      <tr>
      <td>Commands run under psexec or psexecsv</td>
       <td> event_simpleName=ProcessRollup2 psexec OR psexecsv OR [search psexec OR psexecsv event_simpleName=processRollup2 | fields + TargetProcessId_decimal | rename TargetProcessId_decimal as ParentProcessId]
</tr>
<tr>
  <td>Network connections made from a file</td>
  <td>event_simpleName=NetworkConnectIP4 ComputerName=[ComputerName]
	|rename ContextProcessId_decimal as TargetProcessId_decimal
	|join TargetProcessId_decimal
	[ search event_simpleName=*ProcessRollup* ComputerName=[ComputerName] FileName=[FileName]]
	| stats values(RemoteIP) values(RemotePort_decimal) values(ImageFileName) values(FileName) values(CommandLine) values(UserName) by SHA256HashData</tr>   
    
<tr>
  <td>DNSRequest made from a file</td>
  <td> event_simpleName=DnsRequest ComputerName=[ComputerName]
	|rename ContextProcessId_decimal as TargetProcessId_decimal
	|join TargetProcessId_decimal
	[ search event_simpleName=*ProcessRollup* ComputerName=[ComputerName] FileName=[FileName]]
    | stats count by _time DomainName ComputerName CommandLine FileName UserName</td>
  </tr>
<tr>
  <td> Query to check if there were logins from the compromised system </td>
   <td>event_simpleName=UserLogon RemoteIP=[IPofCompromisedSystem]
| eval ProductTypeName=case(ProductType=1, "Workstation", ProductType=2, "Domain Controller", ProductType=3, "Server") 
| eval LogonType=case(LogonType_decimal="2", "Local Logon", LogonType_decimal="3", "Network", LogonType_decimal="4", "Batch", LogonType_decimal="5", "Service", LogonType_decimal="6", "Proxy", LogonType_decimal="7", "Unlock", LogonType_decimal="8", "Network Cleartext", LogonType_decimal="9", "New Credentials", LogonType_decimal="10", "RDP", LogonType_decimal="11", "Cached Credentials", LogonType_decimal="12", "Auditing", LogonType_decimal="13", "Unlock Workstation")
|table ProductTypeName LogonType _time UserName ClientComputerName ComputerName LocalAddressIP4 RemoteIP UserIsAdmin_decimal UserPrincipal</td>
  </tr>
<tr>
  <td>Check for files opened from outlook</td>
  <td> event_simpleName=*ProcessRollup* ParentBaseFileName=Outlook.exe ComputerName=[ComputerName] OR UserName=[UserName]
    | table _time CommandLine FileName FilePath UserName</td>

  </tr>
<tr>
<td>LOLBas Detection
<td> (event_simpleName=ProcessRollup2 OR event_simpleName=SyntheticProcessRollup2) (FileName=Atbroker.exe OR FileName=Bash.exe OR FileName=Bitsadmin.exe OR FileName=Certutil.exe OR FileName=Cmd.exe OR FileName=Cmstp.exe OR FileName=Control.exe OR FileName=Cscript.exe OR FileName=Csc.exe OR FileName=Dfsvc.exe OR FileName=Diskshadow.exe OR FileName=Dnscmd.exe OR FileName=Esentutl.exe OR FileName=Eventvwr.exe OR FileName=Expand.exe OR FileName=Extexport.exe OR FileName=Extrac32.exe OR FileName=Findstr.exe OR FileName=Forfiles.exe OR FileName=Ftp.exe OR FileName=Gpscript.exe OR FileName=Hh.exe OR FileName=Ie4uinit.exe OR FileName=Ieexec.exe OR FileName=Infdefaultinstall.exe OR FileName=Installutil.exe OR FileName=Jsc.exe OR FileName=Makecab.exe OR FileName=Mavinject.exe OR FileName=Mmc.exe OR FileName=Msconfig.exe OR FileName=Msdt.exe OR FileName=Mshta.exe OR FileName=Msiexec.exe OR FileName=Odbcconf.exe OR FileName=Pcalua.exe OR FileName=Pcwrun.exe OR FileName=Presentationhost.exe OR FileName=Print.exe OR FileName=Regasm.exe OR FileName=Regedit.exe OR FileName=Register-cimprovider.exe OR FileName=Regsvcs.exe OR FileName=Regsvr32.exe OR FileName=Reg.exe OR FileName=Replace.exe OR FileName=Rpcping.exe OR FileName=Rundll32.exe OR FileName=Runonce.exe OR FileName=Runscripthelper.exe OR FileName=Schtasks.exe OR FileName=Scriptrunner.exe OR FileName=Sc.exe OR FileName=SyncAppvPublishingServer.exe OR FileName=Verclsid.exe OR FileName=Wab.exe OR FileName=Wmic.exe OR FileName=Wscript.exe OR FileName=Wsreset.exe OR FileName=Xwizard.exe)
|stats values(CommandLine) count by ComputerName 
|sort -count </td>
  </tr>
  </table>
  
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
      
  <p><b><a href="#table-of-contents">↑</a> Show processes with networking functions</b></p>   
      
      tasklist /m WSock32.dll
      tasklist /m Ws2_32.dll
      tasklist /m Wininet.dll
      tasklist /m winhttp.dll

  <p><b><a href="#table-of-contents">↑</a>Show processes importing the Remote Access API</b></p>   
      
      tasklist /m RASAPI32.dll

  <p><b><a href="#table-of-contents">↑</a>Show processes importing the task scheduler API</b></p>   
      
      tasklist /m taskschd.dll
      tasklist /m mstask.dll
      
  <p><b><a href="#table-of-contents">↑</a>Show processes importing the Windows Media Instrumentation API</b></p>
      
      tasklist /m wbem*
      tasklist /m wmi*

      
                
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


           <p>Coming Soon</p>

 <h2><a id="user-content--datasets" class="anchor" aria-hidden="true" href="#-miscellaneous"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z"></path></svg></a><a href="#table-of-contents">↑</a> Miscellaneous</h2>
