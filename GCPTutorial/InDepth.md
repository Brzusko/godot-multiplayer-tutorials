# Custom Code Overview

## Introduction
The server and client projects in this folder are pretty simple, and the built-in signals and methods have been described in the [Networking Basics](../NetworkingBasics.md) tutorial. With that knowledge, I believe much if the code is self explanatory, so we will just look at the custom functions.

## Client
```
#gamestate.gd
# Players dict stored as id:name
var players = {}

puppet func register_player(id, new_player_data):
	players[id] = new_player_data
	emit_signal("players_updated")


puppet func unregister_player(id):
	players.erase(id)
	emit_signal("players_updated")
```

The purpose of this code for the client to be informed about who the players are. As mention in [Networking Basics](../NetworkingBasics.md), each node has a default network master of 1. This includes this gamestate script: it is a puppet to the server. You can see that the two function utilize the `puppet` keyword. This means that only the server can call them remotely: other clients can't. It's a nice was to prevent griefing from malicious users. 

## Server
```
# Players dict stored as id:name
var players = {}

# Player management functions
remote func register_player(new_player_name):
	# We get id this way instead of as parameter, to prevent users from pretending to be other users
	var caller_id = get_tree().get_rpc_sender_id()

	# Add him to our list
	players[caller_id] = new_player_name
	
	# Add everyone to new player:
	for p_id in players:
		rpc_id(caller_id, "register_player", p_id, players[p_id]) # Send each player to new dude
	
	rpc("register_player", caller_id, players[caller_id]) # Send new dude to all players
	# NOTE: this means new player's register gets called twice, but fine as same info sent both times
	
	print("Client ", caller_id, " registered as ", new_player_name)


puppetsync func unregister_player(id):
	players.erase(id)
	
	print("Client ", id, " was unregistered")
```

This time we have `register_player` set as `remote` and `unregister_player` set as `puppetsync`. This mean that while anyone can remotely call `register_player`, only the server can run `unregister_player` via an rpc. Note you don't have to have `unregister_player` as a `puppetsync`: you can instead leave off any networking keywords. Then, you would call it once "normally" to execute locally on server, and once via rpc to execute on clients. 

## Conclusion
After looking through this project source and file, you have some idea of how a multiplayer setup works in Godot. 

If you haven't yet, go through the [Google Cloud Platform Hosting](GCPTut.md) tutorial to learn how to host a Godot server via GCP.

If you've done the above and are interested in more, check out the [Lobby tutorial](../LobbyTutorial/LobbyTut.md) and [Lobbyless Server tutorial](../LobbylessTutorial/LobbylessTut.md).

Cheers