# Custom Code Overview

## Introduction
Despite being the most complete networking wise, you'll notice that the code isn't much different from the last tutorial.
With the lobbyless approach, we want the game to be playing all the time. As soon as client opens game, we want to try to establish a connection to server. This way, once the client clicks play, they can be right in the game.

As player movement and spawning was discussed in the [Lobby tutorial](../LobbyTutorial/LobbyTut.md), I won't repeat it here.

## Client
```
#World.gd
puppet func remove_player(id):
	$Players.get_node(String(id)).queue_free()
```

This is the only new networked function on the client. It's again a `puppet`, and it's purpose is to let server delete players off the playing field. For example, when someone disconnects from game, they should no longer be in the game world. 

The code execution in the gamestate script is altered somewhat. The client now tries to connect to server as soon as game starts, but doesn't send any player info until the "Join" button is clicked.

## Server
```
#World.gd
puppetsync func remove_player(id):
	$Players.get_node(String(id)).queue_free()

#gamestate.gd
remote func populate_world():
	var caller_id = get_tree().get_rpc_sender_id()
	var world = get_node("/root/World")
	
	# Spawn all current players on new client
	for player in world.get_node("Players").get_children():
		world.rpc_id(caller_id, "spawn_player", player.position, player.get_network_master())
	
	# Spawn new player everywhere
	world.rpc("spawn_player", random_vector2(500, 500), caller_id)
```
The `remove_player` function is identical, except it's a `puppetsync` instead of `puppet` like on client. This is so that we can spawn new players on server and existing clients simultaneously. 

The old `post_start_game` has been renamed `populate_world`. This time when a client calls it, the client gets all current players spawned locally. Additionally, the new clients is also spawned all clients, including the new one.

Something you may notice, is that `World.tscn` is the default scene now. This is so game loads as soon as server is started.

## Conclusion
If you want, you can host this project as well, similar to last time. By now you have all the knowledge and code examples necessary to work on your own games. I'd love to see what you make, so please @MenipTweet when you post on Twitter. 

Thanks for going through all the tutorials so far! They took some effort and I'm glad someone is getting a use out of them. 

Cheers