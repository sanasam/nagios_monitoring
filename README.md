# Today we are going to talk about the Window system monitoring using NSclient++ 
# Here , i have install the nagios in my VM box which will act as a guest and i am goining to monitor my window system using NSclient++ agent ( install in window system)
## Step1: NSclient++ latest version doenload and installing in the window system.
* Download the NSclient++ (latest verions ) fro the link [NSclient++](https://nsclient.org/download/thank-you/?file=https%3A%2F%2Fgithub.com%2Fmickem%2Fnscp%2Freleases%2Fdownload%2F0.5.2.35%2FNSCP-0.5.2.35-x64.msi)
* Do the normal window app installation
    In the intallatin configuration setting , follow the below steps
        1. Click all the option including  " allow all user to write config file" <img width="334" alt="Installation1" src="https://user-images.githubusercontent.com/48834323/111865063-c6e01b00-898a-11eb-9238-b7bd9567b452.png">) and then click Next
        2. The next pop up windos will show to allow the host.
            a. We can give the host ip address here. If  we provide the host IP address , then the NSvclient only monitor that pparticular ip address
            b. We can leave it blank. The NSclient will monitor all the available id address
            c. Leave the password blank as per now. We can add if required
        Refer the below image for other raminaing option. ![image](https://user-images.githubusercontent.com/48834323/111865360-a44f0180-898c-11eb-9150-abc5d0799d50.png)