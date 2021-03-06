# Wakanda Deployment Guide

This guide will walk you through the process of deploying your Wakanda app in the Cloud. We will be installing Windows 2008 Server and Ubuntu 12.04 Linux server operating systems on Windows Azure and Amazon EC2 infrastructure. It should take no more than 20 minutes to get your app running, including signing up to Windows Azure or Amazon accounts.

We would love to see if you could share the URL of your app on our forum at http://forum.wakanda.org. Or drop us an email about your experience at wakanda@wakanda.org. Looking forward to hearing from you and happy deploying!

The guide was written by [Juergen Fesslmeier](wakanda@wakanda.org).

Special thanks to [David Robbins](http://wakanda.org) for sharing his [vacation management solution (PTO)](https://github.com/davidrobbins/pto).

# Setup on Windows Azure

## Setup Windows 2008 Server

Go ahead and sign up to Windows Azure's 90-day free trial (you will need a valid credit card, a phone or mobile phone) at:

    http://www.windowsazure.com/en-us/pricing/free-trial/

After you sign in to your new Azure account, you will need to register for the "Virtual Machines & Virtual Networks" section in the Preview Features in your account. Sign up by navigating to:

    https://account.windowsazure.com/PreviewFeatures/. 

You are approved instantaneously after rolling in and are ready to go to start your first virtual server.

Let's do that and navigate to your Azure Portal, bring up the Azure manager at

    http://manage.windowsazure.com/

select the "Virtual Machines" tab on the left, and click on "+ New" on the  bottom left menu, select Virtual Machine again and proceed with "From Gallery" in the selection.

From the list, choose 

    "Windows Server 2008 R2 SP1 July 2012"

and hit Next.

You are then brought to the "VM Configuration" page of wizard. Choose a virtual machine name, e.g. Win8Server1, along with a password (confirm password) and select 

    "Medium (2 cores, 3.5 GB Memory)"

and hit Next.

Note: At this point, Wakanda requires a two core CPU (or higher) on Windows to run properly. A smaller, e.g. 1 GB memory, or higher, configuration not enough right now.

At VM Mode, select 

    "Standalone Virtual Machine"

and a DNS name, e.g. wakobar.cloudapp.net, the URL under which your app will be available at. For the Storage Account" keep the "Automatically Generated Storage Account", and pick your region, e.g. "West US", or whatever is closest to you as latency time may vary. Hit Next.

On VM Options, under "Availability Set" keep "(None)" and hit Next. 

You will now see a list of your virutal machines with the one you just added. Your newly created machine is starting up and should be ready within a few minutes. 

Once the VM is provisioned, you may need to start the server manually. So select the virtual machine in the list, e.g. "Win8Server1" and click on "Restart" in the menu (bottom).

Let's configure the endpoints of the VM. With endpoints you define outbound communication by opening additional ports and protocols. So on the VM detail page, click on Endpoints (in the top bar just below the server name). There is one endpoint configured ("Remote Desktop", tcp, port 3389).

Select "Add Endpoint" (menu bar in the bottom), and click on Add Endpoint again in the radio buttons, now hit Next. Enter "http" as the name of the new endpoint, select protocol TCP, enter 80 for public port, enter 8081 for private port, now hit Next. 

Note: We assume that the Wakanda app we want to install on the server is running on port 8081. Otherwise, change your private port to whatever your Wakanda solution is running on.

Repeat the previous step for each of the following ports/apps:

    PUBLIC PORT  DESCRIPTION                                PRIVATE PORT
    80           "http", standard http port                 8081
    8080         "wak-admin", Wakanda Admin                 8080
    4443         "wak-debug", Wakanda remote debugging      4443

Note: It may take a few minutes before each endpoint becomes active.

Now, on the VM detail page, click on "Connect". A remote desktop protocol configuration file is generated and downloaded to your machine (make sure you hold on to it). Go ahead and open it (given, you must have Remote Desktop Connection installed on your machine). When prompted for the login credentials provide your user:Administrator, password:<password you chose when creating the VM>, and optionally, the domain e.g. wakobar.cloudapp.com. If prompted for a certificate, just confirm and continue. You should now get connected to the server instance.

Optional: 

Attach storage to the VM. Go to the list of virtual machines and select yours. Select "Attach", keep the default selection and enter 5 GB for the drive size and hit Next. It will take some time to attach the storage. Connect to the virtual machine again. After you log on to the virtual machine, open Server Manager (icon in the status bar). In the left pane, expand "Storage" (bottom), and then click "Disk Management" (bottom). Right-click Disk 2, and then select "New Simple Volume...". Keep on clicking next on the wizard until done.

You can install your Software on this storage and reuse the storage from each VM you install.

## Setup Ubuntu Server 12.04

Note: If you are on Windows you may want to download [Cygwin](http://www.cygwin.com) for Linux look and feel environment.

Go to your Azure manager https://manage.windowsazure.com/, click on Virtual Machines and hit "New". Select Virtual Machine and "From Gallery".

In the Wizard, select:

    "Ubuntu Server 12.04 LTS"

and hit Next.

On VM Configuration choose a virtual machine name, e.g. Ubuntu12Server1, select a new user, e.g. "deploy", add password, select 

    "Small (1 core, 1.75 GB Memory)"

You can leave "Upload SSH key for authentication" unchecked at this point and hit Next.

On VM Mode select Standalone Virtual Machine, DNS Name, e.g. wakofoo.cloudapp.net, storage account "Use Automatic generated storage account", keep region "West Coast" or change, hit Next.

VM Options, no additions, next to finish the wizard and create the VM (takes a few minutes). Note: if the virtual machine shows "Stopped" in Status, try to restart it from the menu (below).

Before we can access the server admin, we need to configure endpoints to make your app publicly available. Navigate to:

    https://manage.windowsazure.com

Click on your Ubuntu VM, click on "ENDPOINTS" (top, under your server name), select "Add endpoint" and hit next, enter a name, e.g. "wak-manager", protocol, e.g. TCP, public port 8080, and private port 8080, and hit Next/Finish (this may take a few minutes).

Create endpoints for each of the following ports: 

    PUBLIC PORT  DESCRIPTION                                PRIVATE PORT
    80           "http", standard http port                 8081
    8080         "wak-admin", Wakanda Admin                 8080
    4443         "wak-debug", Wakanda remote debugging      4443

Click on your Ubuntu virtual machine https://manage.windowsazure.com to get to the detail page. On the machine's dashboard page navigate to "SSH Details" and get copy the location of your machines public IP address or DNS.

To login to your VM, install Putty first (if you are on Windows and need a SSH client) or use the command line on OS X/Linux. 

    $ ssh <user>@<server>

E.g. remember you can paste the public server IP/DNS into the terminal:

    $ ssh -p 22 deploy@wakofoo.cloudapp.net 

or, in the terminal (on your local machine) connect to your Ubuntu server, deploy account, e.g.

    $ ssh deploy@wakofoo.cloudapp.net
    Password: <enter password>
    deploy@ubuntu:~$ 
    
You will be in the `deploy` user's home folder. When you enter `pwd` at the prompt, you should see `/home/deploy`. Otherwise, just enter `cd`, which brings you back to your home folder.

# Setup on Amazon EC2

## Setup of Windows Server 2008

Note: This section needs more work.

Signup or sign in to your to Amazon AWS. When you sign in, make sure you will have a telephone number and your credit card ready. Amazon will only charge your card if you using computing time. You can always shut-down your server.

Go to the Management Console at https://console.aws.amazon.com in your browser and click on EC2.

Select Launch Instance. It brings up a setup wizard, sinde choose the first option Classic Wizzard.

On the Choose an AMI page, click on the "Community AMIs" tab, set search filter to "All Images" and look for the following "Windows Server with Wakanda" images:

    (ami-14ce687d)
    -> ami-cbc87da2  Default Wakanda Server 2008 R2 64-bit

Continue by clicking "Select" next to the matching AMI in the list.

On the "Instance Details" step of the wizard, select 

    * Number of instances: 1
    * Instance Type: Micro (t1.micro, 613 MiB)
    * Select Launch Instances, as EC2, Availability Zone: "No Preference" (or us-east-1a, etc. if you prefer)

Hit Next onto the Advanced options.

    * Kernel ID: Use Default
    * RAM Disk ID: Use Default
    * Monitoring: off
    * Keep everything else as default

Hit Continue to the next step. 

On the Storage Device Configuration you should see the following default storage mountpoint in your list:

    Type    Device       Snapshot ID      Size     Volume Type   Delete on Termination
    Root    /dev/sda1    snap-946b85eb    30GiB    standard      true

Hit the Edit button next to the storage device and change Deleteon Terminate to false, select Save, and then hit Next to continue.

On the adding tags to your instance hit Next to continue.

On the Create Key Pair step of the Wizard select "Create a new Key Pair", 1. Enter a name for your key pair, e.g. "wakowin" and 2. Click to create your key pair: Hit Create & Download your Key Pair. It will then bring you to the next step of the Wizard.

On the Configure Firewall, choose the default item in the Security Groups. Hit Next to Continue to the Review page and hit Launch to launch the instance. Click "View your instance on the Instance page" or Close and select Instances.

To connect to your newly created instance, open "Remote Desktop Connection" either on Windows or your Mac.

## Setup of Ubuntu 12.04 Server

Login to AWS and navigate to Instances on http://console.aws.amazon.com/ or by clicking on Instances on Navigation.

Select "Launch Instance", on in the "Create new Instance" screen select "Classic Wizard" and hit Continue.

On the "Quick Start" tab choose:

    Ubuntu Server 12.04 LTS, 64-bit

and then Select.

On the "Instance Detail" step, choose 1 instance of `Small (m1.small, 1.7 GiB)` keep the rest of the default setting and select Continue.

On Advanced Instance Options keep defaults and hit Continue.

On Storage Device Configuration keep defaults and hit Continue.

On Tag Instance Settings keep defaults and hit Continue.

Now on the "Create Key Pair" step of the wizard choose existing (wakowin) or create a new on key pair that you download. Hit Continue.

On the "Configure Firewall" in select the "Create a new Security Group" radio buttons, then enter a group name and description, e.g. wakrules, and on "Inbound Rules" add the following Port numbers and services.

    TCP
    Port(s)      Service    Source     Description
    80           HTTP       0.0.0.0/0  Web server
    443          HTTPS      0.0.0.0/0  SSL
    22           SSH        0.0.0.0/0  ssh
    8080                    0.0.0.0/0  Admin
    4433                    0.0.0.0/0  Secure Admin
    8081 - 8100             0.0.0.0/0  Apps
    1919 - 1921             0.0.0.0/0  Remote debugger

Hit Continue.

On the "Review" page of the wizard hit Launch to launch the instance, then Close to close the Wizard.

Now login to your running instance using the downloaded key pair, e.g. `wakowin.pem`. You must change the file permissions of the key pair file, given it is located in `~/.ssh/wakowin.pem`, so do the following:

    chmod 400 ~/.ssh/wakowin.pem
    ls ~/.ssh/wakowin.pem
    -r--------@ 1 user  staff  1696 Apr  7 10:20 /Users/j/.ssh/wakuntu.pem

Note: You want to keep the .pem file in a safe place. Once downloaded it cannot be re-downloaded again.

    ssh -i <pem-file> <user>@<public-DNS>

E.g.

    ssh -i ~/.ssh/wakowin.pem ubuntu@ec2-23-21-16-67.compute-1.amazonaws.com

You get the public DNS when you go http://console.aws.amazon.com/ec2/ click on Instances and select your running instance and copy the DNS.

Check the file system disk space usage:

    df -Th
    
    Filesystem     Type      Size  Used Avail Use% Mounted on
    /dev/xvda1     ext4      8.0G  867M  6.8G  12% /
    udev           devtmpfs  819M   12K  819M   1% /dev
    tmpfs          tmpfs     331M  168K  331M   1% /run
    none           tmpfs     5.0M     0  5.0M   0% /run/lock
    none           tmpfs     827M     0  827M   0% /run/shm
    /dev/xvdb      ext3      147G  188M  140G   1% /mnt

Use `netstat` to check if all configured ports are open in the firewall, you should see the following:

    $ sudo netstat -anltp | grep "LISTEN"
    
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
    tcp6       0      0 :::8080                 :::*                    LISTEN      1599/Wakanda    
    tcp6       0      0 :::4433                 :::*                    LISTEN      1599/Wakanda    
    tcp6       0      0 :::8081                 :::*                    LISTEN      1599/Wakanda    
    tcp6       0      0 :::22                   :::*                    LISTEN      -               
    tcp6       0      0 :::1919                 :::*                    LISTEN      1599/Wakanda

# Installing Wakanda

## Installing Wakanda on Windows 2008 Server

Login to your server instance using Remote Desktop Client.

* On Windows Azure: double-click on the generated Remote Desktop Client file you have previously downloaded, e.g. Win8Server1.rdp

* On Amazon EC2: Copy & paste the public server location into the Remote Desktop Client location, using username: Administrator, password: <decrypted password, see Amazon section>

Open IE on the Server machine and direct to the downloads area of Wakanda:

    http://www.wakanda.org/downloads

Go to the Wakanda for Windows section and download:

    "Server 64-bit"

Open the archive and copy the contents to Desktop. You should now see a folder:

    /Users/Administrator/Desktop/Wakanda\ Server

We need to configure the Firewall to let incoming traffic through the Wakanda Server. In Control Panel navigate to "Configure Windows Firewall". In the top-left corner, click on "Allow a program or feature through Windows Firewall". Click on "Allow another program..." and browse to "/Users/Administrator/Wakanda\ Server\Wakanda\ Server.exe", click on "Add" and OK.

Launch the Wakanda Server in "/Users/Administrator/Wakanda\ Server\Wakanda\ Server.exe". Click on "Run" if you should see a security warning.

You should see the following message in a console windows:

    Welcome to Wakanda Server!
    
    Publishing solution "DefaultSolution"
        Project "ServerAdmin" published at 127.0.0.1 on port 8080

Note: If you don't see the full text output (avove), you may have configured your server incorrectly. Remember, your server instance needs to run 2 cores, or more.

To check if you server is publicly accessible, open up a browser (Chrome, Safari, Firefox) and enter the location (you get the IP address or DNS of your public server from the Azure/Amazon console.), e.g.:

    http://168.62.204.176:8080

You should see Wakanda admin interface launch inside the browser.

Back in the RDC open an IE to download an already written Wakanda solution. Navigate to 

    https://github.com/davidrobbins/pto

Click on the ZIP button next to the repository address and download the archive (select open). Copy the files and rename the folder:

    /Users/Administrator/apps/pto

Finally, you want to add the application to the Wakanda app server. Navigate to the sever admin, either on the Windows server or on your local browser, e.g. http://168.62.204.176:8080/. 

Click on "Browse" and enter the following location of your app:

    C:/Users/Administrator/apps/pto/PTOb201.waSolution

Checking back on your server in the Wakanda Server window, you should see something like the following:

    Welcome to Wakanda Server!

    Publishing solution "DefaultSolution"
        Project "ServerAdmin" published at 127.0.0.1 on port 8080
    
    Solution closed
    
    Published solution "PTOb201"
        Project "PTOb201" published at 127.0.0.1 on port 8081

Access the app on your public IP/DNS, like for example:

    http://168.62.7.116

Or in case you are not re-routing port 80 to your private 8081 port you can access it like this:

    http://168.62.7.116:8081

You should see the wonderful PTO application in action. Enjoy!

## Installing Wakanda on Ubuntu Linux

Download the latest (production) release. Outside your terminal, browse to

    http://download.wakanda.org/ProductionChannel

and pick a build the you want to download and install. Right click on the downloadable package and copy the link. Then return to the terminal, at the prompt (the `$` indicates a command line prompt) enter the following (paste the link):

    $ wget http://download.wakanda.org/.../Wakanda-Server-x64-v2.tar.gz

Unzip the downloaded `.tar.gz` file into the user's root folder, e.g. like this:

    $ tar xzvf Wakanda-Server-x64-v2.tar.gz

Launch the Server like this (don't omit the ampersand, it will make Wakdanda run in the background):

    $ ~/Wakanda\ Server/Wakanda &
    Welcome to Wakanda Server!
    Publishing solution "DefaultSolution"
    Project "ServerAdmin" published at 127.0.0.1 on port 8080

Access the server manager from your public browser like (you get the public IP address from your Azure managing console):

    http://168.62.7.116:8080
    
Install a solution on the machine, e.g.:

    mkdir ~/apps
    cd apps
    sudo apt-get install git
    git clone git://github.com/davidrobbins/PTO.git

Inside the server manager, add the PTO solution. Hit the "Browse" button in the status bar. As Solution path point to the solution location on the server and hit "Launch", such as: 

E.g.

    /home/deploy/apps/PTO/PTOb201.waSolution

or on AWS

    /home/ubuntu/apps/PTO/PTOb201.waSolution

On the server terminal you should get a log message like

    Publishing solution "PTOb201"
    Project "PTOb201" published at 127.0.0.1 on port 8081 

Before you can access the application, add another endpoint for port 8081.

Access the app here:

    http://168.62.7.116

In case you are not re-routing port 80 to your private 8081 port you can access it here:

    http://168.62.7.116:8081

You should see the wonderful PTO application in action. Enjoy!

### Differences in Installation from Dev Channel

Go to the Ubuntu console, and install `unzip` like this:

    $ sudo apt-get install unzip

Then point your local browser to `http://download.wakanda.org/DevChannel`, go to the Linux folder and pick a development build. Right click on `Wakanda-Server-x64.zip` and copy the link.

On the Ubuntu console download the Wakanda Server and unzip the files, like this:

    $ cd
    $ wget wget http://download.wakanda.org/.../Wakanda-Server-x64.zip
    $ unzip Wakanda-Server-x64.zip

# Remote Debugging

Given that Wakanda Server and the PTO applications are installed and running on the server, and all TCP ports are configured correctly (see Server installation).

Open a browser and launch the secure admin interface using port `4433` like this:

    https://ec2-174-129-188-68.compute-1.amazonaws.com:4433

Essentially, this is the admin interface over a secure connection.

Launch the Studio on your local machine.

Go to "Run" menu and choose "Connect to Other Server", enter the remote server's IP address or DNS name in the IP entry field, e.g. 

    ec2-174-129-188-68.compute-1.amazonaws.com

and in the port field enter `4433` then hit Connect.

After the Studio connects to the remote server, you will see a green light on Debugger inside the Studio status bar. 

Now set a breakpoint anywhere in your code or add a language breakpoint using the `debugger` keyword.

Try to hit that breakpoint from your app running on a hosted environment. When hitting the breakpoint, you should see the Wakanda Debugger window popup on your local machine on the exact breakpoint. Go ahead and step-into/through or setup watches, etc.

# SSL Security and Certiciates

http://www.akadia.com/services/ssh_test_certificate.html

Change into a folder where you want to have the keys to be stored in, e.g.:

    cd ~/.ssh

1. Generate a Private Key

The openssl toolkit is used to generate an RSA Private Key and CSR (Certificate Signing Request). It can also be used to generate self-signed certificates which can be used for testing purposes or internal usage.
Generate a private key:

    openssl genrsa -des3 -out server.key 1024

2. Generate a CSR (Certificate Signing Request)

Once the private key is generated a Certificate Signing Request can be generated. The CSR is then used in one of two ways. Ideally, the CSR will be sent to a Certificate Authority, such as Thawte or Verisign who will verify the identity of the requestor and issue a signed certificate. The second option is to self-sign the CSR, which will be demonstrated in the next section.

    openssl req -new -key server.key -out server.csr

3. Remove Passphrase from Key

One unfortunate side-effect of the pass-phrased private key is that Wakanda will ask for the pass-phrase each time the web server is started.

Use the following command to remove the pass-phrase from the key:

    cp server.key server.key.org
    openssl rsa -in server.key.org -out server.key

4. Generating a Self-Signed Certificate

At this point you will need to generate a self-signed certificate because you either don't plan on having your certificate signed by a CA, or you wish to test your new SSL implementation while the CA is signing your certificate. This temporary certificate will generate an error in the client browser to the effect that the signing certificate authority is unknown and not trusted.

To generate a temporary certificate which is good for 365 days, issue the 
following command:

    openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

5. Copy keys to the solutions's project folder

   cd PTO/PTOb201
   mkdir certificates
   cp ~/.ssl/server.key certificates/key.pem
   cp ~/.ssl/server.crt certificates/cert.pem

6. Configure the certificate in your Wakanda project

Open the `PTOb201.waSettings` and in the Secure Connections (SSL - TLS). Check "Enable secure connections". Choose port number `8082`, and browse to your "Certificate Path" to where your `server.crt` resides. Save settings and start server. Go to:

    https://localhost:8082

Note: You should receive a browser warning when loading the page as you don't as the certificate is not signed by a Certificate Authority.

