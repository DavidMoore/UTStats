# Unreal Tournament Stats

## Project Goals

Provide Unreal Tournament players with the best stats **recording**, **reporting** and **ranking** possible.

## History

### Unreal Tournament (1999)

The original Unreal Tournament 1999 included integration with NetGamesUSA's
ngStats, a stats provider that also supported MechWarrior and Quake III.

This provided a global record of games, including detailed match stats, and a weekly and quarterly
player leaderboard for the core games types (including Deathmatch and CTF).

NetGamesUSA was acquired by Microsoft in 2000 (http://www.microsoft.com/en-us/news/press/2000/jul00/netgamespr.aspx);
while UT's ngStats remained live, 
the website did degrade over time, and was eventually shut down entirely around about 2002.

### Unreal Tournament 2003 and 2004

For 2003, stats were available at http://ut2003stats.epicgames.com.

Unreal Tournament 2004 stats are available from Epic themselves, and are still live @ http://ut2004stats.epicgames.com/

These emulate the original ngStats functionality.

## Unreal Tournament

The new Unreal Tournament is currently actively being developed by Epic Games and the community.

This particular project is focused on ensuring that a rich, extensible framework for collecting stats is available at game release.

# Architecture & Design

The first focus is being able to consume statistics from Unreal Tournament games.

There is already analytics support built in to the Unreal Engine; we can potentially leverage this.

* At a high level, the game engine should send a particular match's statistics to a web endpoint using JSON.
* Configure the server to push game statistics to 1 or more web endpoints.
* Support for formats other than JSON, such as Google Protocol Buffers?
* Real-time and/or post-game push

## Hardcore stats

Ideally, we should be able to playback and analyse an entire game through an uploaded server demo.

This would allow us to change and improve our algorithms, and re-analyze past and present games, 
as well as support future mods and mutators. In essence, we will always have access to all the data
we could possibly need, rather than what was sent and available at the time.

Right now, there is no demo recording support in Unreal Tournament.

## Stats to record

Every stat would have a timestamp recorded, with sub-second precision.

An initial prospective list:

Event Name|Description|Parameters
----------|-----------|----------
LoggedIn|A player logged in to the match|Player
LoggedOut|A player left a match|Player
ItemGet|An item is picked up by a player (e.g. ammo, weapon, health, power up etc)|Player,Item,Quantity
ChangeTeam|A player changed team|Player
Suicide|A player suicided|Player,DamageType
Kill|A player killed another player|Killer,Victim,DamageType,KillerWeapon,VictimWeapon
FirstBlood|Someone got first blood|Killer,Victim,DamageType,KillerWeapon,VictimWeapon
TeamScore|Team scored e.g. flag capture|Player
Assist|Player assisted (e.g. assisted flag capture)|Player


### Stats concerns

* Find the balance between meaningful events, and too much noise and size in the log.
* What can we rebuild from the stats, reliably? Because the server is the authority, we can be sure that first blood is first blood.
* Another example is that do we re-construct a multi kill scenario by looking at the kill timings, or log a special event
at game time? If we reconstruct game events, do we have enough information of the game rules, timings and other data to be
reliable here?

## Project overlap

The statistics can overlap with other parts of the game, particularly real-time.

Matchmaking|Matching players together for a compeitive match.
Team balancing|Similar to above, but related to team games like TDM and CTF where team balance is important (and in CTF, roles are important. Player ratings might match up, but having no seasoned attackers on one team could skew the balance.)
Tournament play|Enhancements to sanctioned competitions with organised teams and tournament structures (round robin, ladders, elimination etc)
Live casting|How and what stats would be made available when casting? Are stats and ranking two-way, with servers being able to pull from web services instead of just push to them?

## Ranking Algorithms

The Elo rating system has proven a dominant algorithm for rankings in diverse games and organizations such as Chess, Tennis, Football and related to eSports and UT: ngStats, UT2004Stats, Quake Live.

The Glicko rating system is an improvement on Elo also, and has been extended further in the Microsoft TrueSkill algorithm.

Whatever algorithm(s) are looked at, this should be customized to address concerns such as:

* High-ranking players not playing to protect their ranking


## Performance Concerns

* Logging stats should have as low CPU and memory footprint on the server and client as possible
* Uploading logs from server to the service shouldn't impact the server, even if the service is down or slow
* What kind of throughput will be needed for the services to consume and report on incoming logs?