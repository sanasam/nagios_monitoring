# Today we are going to talk about the Window system monitoring using NSclient++ 
# Here , i have install the nagios in my VM box which will act as a guest and i am going to monitor my window system using NSclient++ agent ( install in window system)
* Assumption
    1. Nagios has already installed in the VM servver with ubuntu/xenial64\
    2. Enabled the puplic network of nagios server from the notepad by uncommenting the respective line.\
        a. go to the vagrant file directory\
        b. open the vagrantfile using notepad\
        c. Go to the line where puplic ip address is mentioned.\
        d. uncommented that line.\
        e. save the file\
        f. Reload the vagarnt file or vagrant up if new bring up the server\



## Step1: NSclient++ latest version download and installing in the window system.
* Download the NSclient++ (latest verions ) from  the link [NSclient++](https://nsclient.org/download/thank-you/?file=https%3A%2F%2Fgithub.com%2Fmickem%2Fnscp%2Freleases%2Fdownload%2F0.5.2.35%2FNSCP-0.5.2.35-x64.msi)
* Do the normal window app installation
    In the intallation configuration setting , follow the below steps\
        1. Click all the option including  " allow all user to write config file" <img width="303" alt="Installation1" src="https://user-images.githubusercontent.com/48834323/111865063-c6e01b00-898a-11eb-9238-b7bd9567b452.png">)\ and then click Next\
        2. The next pop-up window will show to allow the host.\
            a. We can give the host ip address here. If  we provide the host IP address , then the NSclient only monitor that particular ip address
            b. We can leave it blank. The NSclient will monitor all the available id address\
            c. Leave the password blank as per now. We can add if required\
        Refer the below image for other ramaining  options.\ 
        ![image](https://user-images.githubusercontent.com/48834323/111865360-a44f0180-898c-11eb-9150-abc5d0799d50.png)\
        3. Then click Install\
### NOTE: If the window system create an Pop up for permission , just click Ok or allow to complete the installation.\


## Step 2: COnfiguration on the NSclinet 
    * GO to the C drive>program_file>NSclient>nsclient_configuration file and open it in the notepad by right mouse clicking.\
    Refer below image for ref.\
<img width="303" alt="image 4" src="https://user-images.githubusercontent.com/48834323/111865764-f1cc6e00-898e-11eb-968f-ad478fef475f.png">\
    * By default the below option in the nsclinet configuartion files are disabled. SO we need to **enabled**.\
            a. CheckDisk\
            b. CheckEventLog\
            c. CheckSystem\
        Changed from disble to enable in order the nagios able to monitor the window system. These are acting as a firewall.\
            d. Add the window system  ip address at here \
            [/settings/default]\
            allowed hosts = 127.0.0.1,***192.168.43.XXX*** ( this ip address you will get it by typing ipconfig on the window system cmd) \
    
## Step3: Restarting the NSclient from window service
* Go to the window service from the run box. Find the NSclient form the services and click right mouse and restart the service.\
Refer below image fore ref..\
<img width="303" alt="image 5" src="https://user-images.githubusercontent.com/48834323/111866803-06f8cb00-8996-11eb-972d-ac8e43698e8e.png">

## Step 4: Make sure the **Check_nt** is avalable in the nagios server.
* Go to the Nagios server command prompt.( i am using cmder). and navigate the plugins directory.\
*cd /usr/lib/nagios/plugins* and check whether "check_nt" lis there or not. If not, download the plugins. Normally this plugins are available in the nagios by default.\

* just type from the same directory to check whethe the nsclient is responding to nagios plugins\ 
*./check_nt -H "window ip address" -p 12489 (#NSclient port) -v UPTIME*\
 There are 2 opssible output
    1. Could not able to fetch from the server \
    2. Some output which shows the response\
* If the first option comes as output, try by hiiting *telnet "window ip address" 12489*. if it responces, it shows that the NSclient is up and running\


## Now we will do SOME monitoring things :-) 
1. We will monitor the CLIENTVERSION\
2. WE will monitor the Process_state\
3. We will monitor the CPU load\

#### For client version monitoring:
* Create a configuation file as below in the confd directory where all the .cfg files are resite:\
**define Host**
define host{
    use             generic-host
    host_name       window
    alias           window
    address         "your machine ip address:192.168.XX.XXX "

}

**define command**
define command{
    command_name        check_win
    command_line        /usr/lib/nagios/plugins/check_nt  -H "HOSTADDRESS" -p 12489 -v $ARG1$ $ARG2$
}

**define services**
define service{
    use                     generic-service
    host_name               window
    service_description     nsclient_version_monitoring
    check_command           check_win!NSCLIENTVERSION

}

define service{
    use                     generic-service
    host_name               window
    service_description     process_state_monitoring
    check_command           check_win!PROCSTATE! ' -d SHOWALL -l chrome.exe'

}

define service{
    use                     generic-service
    host_name               window
    service_description     cup_load_monitoring
    check_command           check_win!CPULOAD! '-l 5,70,90'

}

#### code is broken in .md file. Pl. refer the attached images for ref.

<img width="450" alt="image 5 coding" src="https://user-images.githubusercontent.com/48834323/111868449-e6357300-899f-11eb-9a46-b2554abfe049.png">\

<img width="492" alt="image 5 coding 2" src="https://user-images.githubusercontent.com/48834323/111868690-627c8600-89a1-11eb-911a-f8e8c2efd6d1.png">


## Step 5: Running the configuratin file
* After saving the file, come back to the directory where the *nagios.cfg> is reside.
* Run the below command:\
a. nagios3 -v nagios.cfg        # to validate the file\
b. service nagios3 reload       # to run the file\

* After running the ablove two command, go to the Nagios UI and check the monitoring.\
<img width="658" alt="image 5 coding 2 show" src="https://user-images.githubusercontent.com/48834323/111868834-39102a00-89a2-11eb-9bad-887bb50a7849.png">



## HAPPY LEARNING 

