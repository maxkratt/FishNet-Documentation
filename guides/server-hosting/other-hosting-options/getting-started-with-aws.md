# Getting Started with AWS

This tutorial was kindly provided by haseebzahid413#2226.

## Verify Permissions for AWS Account

1. Go to AWS Management Console
2. Launch a Virtual Machine
3. Select any free-tier Linux machine
4. Select any free-tier instance type
5. Edit Security Groups
6.
   1. Add Rule
   2. Custom TCP Rule - port 7770 (or whatever you want to choose for your game)
   3. set Source to anywhere
   4. Add Rule
   5. UDP rule - port 7770 (or whatever you want to choose for your game)
   6. set Source to anywhere
   7. Press “Review and Launch”
7. Create a new Key Pair
8.
   1. Give it a name
   2. download keypair
   3. Press “Launch Instance”
9. Download Putty
10. Download WinSCP
11. Open PuttyGen
12.
    1. Conversions > Import Key
    2. Select the downloaded key
    3. Set the right Key Type as you set while creating the key above
    4. Save Private Key
    5. Close Window
    6. This file of type ppk will be used later
13. Open Putty
14.
    1. Copy IPV4 DNS address for the instance we created above from AWS EC2 Instances window
    2. Putty Configuration > Session > Host Name set this hostname to follows
    3.
       1. ec2-user@ followed by the DNS address copied above
    4. Keep port at 22
    5. Connection Type SSH
    6. Go to Connection > SSH > Auth
    7.
       1. Provide the ppk generated above here in “Browse”
    8. Go back to Session
    9.
       1. Provide a name in Saved Sessions to be save this session
       2. Press Save
    10. Now double click this name in session to open this session
    11. Our Putty terminal is up and running on the Linux machine
15. Open WinSCP
16.
    1. When asked opt for single window with 2 panels option
    2. Login > New Site
    3.
       1. File Protocol > SCP
       2. Paste the same hostname pasted in Putty, or copied from AWS EC2 Instances Console
       3. Username > ec2-user
       4. Advanced > SSH > Authentication > Private Key File
       5.
          1. Provide the private key file here
          2. Press Okay
       6. Press Save
       7. Provide a name to this new Login
    4. Double click on the login created
    5. It will connect to our Linux Server
    6. We will use this Window to transfer replace or delete build on our Linux server

## Configuring Fish-Networking

1. Add TugBoat component on Network Manager
2.
   1. Set IPV4 server bind address to 127.0.0.1, or leave **empty**.
   2. Set port to 7770 (or whatever you want to specify but should be allowed in EC2 instance network incoming traffic conditions)
   3. Set client address to EC2 instance public IPV4 DNS address
   4. Save scene
3. In Build Settings
4.
   1. Set target platform to Linux
   2. Keep architecture to x86\_64
   3. Enable ServerBuild (to keep headless)
   4. Make sure the server scene is added and enabled in build settings
5. Build and save in a folder
6. Wait for the build process to complete

## Deploying

1. WinSCP
   1. Navigate left panel to the unity server build folder
   2. Right click and upload that to the linux server
   3. It will take some time to upload
   4. Once upload complete
2. Putty terminal
   1. Go the the Putty terminal window
   2. Using standard commands navigate inside the build folder
   3. Run commands
   4.
      1. chmod +x “executable file name of type .x86\_64”
      2. ./“executable file name of type .x86\_64”
      3. Server is up and running
   5. Go back to Unity for Client editor testing or build

## Connecting A Client

1. Run in unity
2. Press Client
3. It should run perfectly fine
