# Godot Multiplayer Tutorials
###### Security note: Code in this repo is not always written with security in mind and may be susceptible to malicious users. 

This repo is a collection of tutorials intended to get someone starting with making multiplayer games. Server/client architecture is used here, if you are looking for P2P try going through relevant demos [on the Godot github](https://github.com/godotengine/godot-demo-projects).

## Getting Started
###### Note: Please always take a look at the source code. It will increase understanding much more than just reading a tutorial
Start with the [NetworkingBasics](NetworkingBasics.md) tutorial. It gives an overview of how to approach server/client in Godot and covers many of the functions, properties, and signals that will be useful.

Next, learn to host with the [Google Compute Platform](GCPTutorial/GCPTut.md) tutorial. It contains a barebones client and server projects, and goes over how to get a Godot server hosted via Google. If you want to understand some of the custom functions better, also take a look at [this code overview](GCPTutorial/InDepth.md) of the GCPTutorial project.

After looking through basics, check out how to have servers with lobbies in the [Lobby Tutorial](LobbyTutorial/LobbyTut.md). This one also covers player movement.

Finally to round out your knowledge, learn about [Lobbyless Server](LobbylessTutorial/LobbylessTut.md). This one is the culmination of the previous tutorials and contains all the networking code you need.



### Notes
If you find that you need more info, there is also [this tutorial](https://mrminimal.gitlab.io/2018/07/26/godot-dedicated-server-tutorial.html) that describes how to approach multiplayer using server/client in Godot.

Note that currently HTML5 games will not be able to use the high level networking described in this repo. It's being worked on, but in the meantime you can [take a look at this tutorial](https://old.reddit.com/r/godot/comments/bux2hs/how_to_use_godots_high_level_multiplayer_api_with/).

### Twitter
I would love to hear about someone going through these tutorials and making games utilizing their concepts. Tweet @MenipTweet!
You can also follow me http://twitter.com/MenipTweet to get updates about these multiplayer tutorials and the projects I'm working on.