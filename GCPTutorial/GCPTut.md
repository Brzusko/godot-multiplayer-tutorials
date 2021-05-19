# Hosting Godot server via Google Cloud Platform

### Note
This is a barebones tutorial whose purpose is to show how to host via GCP.

### Requirements
* Godot 3.1.1, the abilities to read English and follow basic instructions.
* Being able to search the web for answers is a strongly recommended skill, though not required.

# Tutorial
## Introduction
Hosting via GCP is fairly cheap and straightforward. They provide a $300 12 month credit for new users, making getting started completely free. Note you will need to connect a credit/debit card as a measure of protection against abuse. [Go here](https://cloud.google.com/free/docs/gcp-free-tier) to read about their free tier, and when ready click the "Try" button in top right of screen. Let's get started.

## Setup
You'll want to download this repo and take a look at both server and client projects. You can try out the server locally: export the client project, and run several instances. Also run the server project in editor. After server is up, click "Join" button on clients and you should see clients are able to see each other.

Once done experimenting, open the server project and head to export screen. Create a Linux/x11 export template, and click "Export PCK/ZIP”. I chose my filename as `Server.pck`. We will use this file later on to run on our server.

## Create and Setup Google Cloud Platform VM
Go to the [Compute Engine VM instances page](https://console.cloud.google.com/compute/instances). It will take some minutes for it to initialize. If you see a popup about the free trial, click activate.

#### Creation
Here we will create and setup our VM. Click on "Create Instance" button near top of page, and change name and region to one that suits you. I will call mine "tutorial". Under "Machine Type", change from "1 vCPU" to "micro (1 shared vCPU)" option. This is their lightest VM, and as such, they provide some free instance usage. In my case, I have "Your first 744 hours of f1-micro instance usage are free this month" which is more than enough for any prototyping. Finally, under "Firewall" select both "Allow HTTP traffic" and "Allow HTTPS traffic". Click “Create”. Here is what mine looks like: 

![alt text](images/CreateInstance.png?raw=true "Create Instance")

#### External IP
After a short while, your VM will be created and you can connect to it. We need to do two things. First, copy your "External IP" and paste it in Client project in gamestate.gd as `const ip = "ip"`. My ip is `34.83.224.107`, so in my script I have `const ip = "34.83.224.107"`. This will let our Client try to connect to the server at that ip. 

#### Firewall Rules
Next we need to add some firewall rules to allow traffic in and out of the VM. I have DEFAULT_PORT defined in Server and Client as "44444" so we will add incoming and outgoing rules for those ports. Google calls them ingress and egress rules respectively. Head on over to [VPC network - Firewall rules](https://console.cloud.google.com/networking/firewalls/list) to add our rules. Click on "Create a firewall rule" at top of page and make rule as follows: set name to "client", "Direction of traffic" should be left as "Ingress", “Targets" to "All instances in the network", "Source IP ranges" to "0.0.0.0/0" (to allow all IPs). Under “Protocols and ports”, enable tcp and udp, and type the desired port, in our case "44444". Finally click "Create". Our server can now hear incoming messages, but can't send yet. Lets create another firewall rule. Click "Create firewall rule" and set name as "server". This time set "Direction of traffic" to "Egress", and all other options same as before:  "Targets" to "All instances in the network", "Source IP ranges" to "0.0.0.0/0" (to allow all IPs). Under “Protocols and ports”, enable tcp and udp, and type the desired port, in our case "44444". Click "Create" again. Here are my client and server rules:


| Client Rule | Server Rule |
| ----------- | -----------:|
| ![alt text](images/ClientRule.png?raw=true "Client Rule") | ![alt text](images/ServerRule.png?raw=true "Server Rule") |

#### Server Binary
Great, our firewall is setup! Next up is actually hosting our game. Go to [Compute Engine-VM instances](https://console.cloud.google.com/compute/instances). Under "Connect" click "SSH". A window should popup that connects to your VM. If you don't see progress after 30 seconds, close the window and click "SSH" again. We now just need to get the Godot Server binary and our server pck file onto the VM and we can host. Download the 3.1.1 server binary by executing in ssh terminal 
`wget https://downloads.tuxfamily.org/godotengine/3.1.1/Godot_v3.1.1-stable_linux_server.64.zip`.
Note: I got this link by going to https://godotengine.org/download/server and copying link from "Server" button. Now we need to extract the zip. Run 
`sudo apt install unzip` 
to install the unzip program, then unzip the server via 
`unzip Godot_v3.1.1-stable_linux_server.64.zip`.

#### Server pck and Running
Next we need to upload our server pck file. Click on the cog in the upper right and select "Upload File". Here is where button is:

![alt text](images/UploadFile.png?raw=true "Create Instance")

Browse to and select the server pck. In my case it's at "GCPTutorial/Server/Server.pck". Once uploaded, start server via 
`./Godot_v3.1.1-stable_linux_server.64 --main-pack Server.pck`.

## Conclusion
Congrats, you've got a Godot server running! Once done player around, turn off VM on the [Compute Engine-VM instances](https://console.cloud.google.com/compute/instances) page. 

After looking through this project source, you have some idea of how a multiplayer setup works in Godot, as well as how to host via GCP.. 

If you want some explanations of the custom functions in this project's server and client, [click here](InDepth.md)

If you've done the above and are interested in more, check out the [Lobby tutorial](../LobbyTutorial/LobbyTut.md) and [Lobbyless Server tutorial](../LobbylessTutorial/LobbylessTut.md).

Cheers
