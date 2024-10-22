---
title: Frequently Asked Questions - Groups
description: Player Grouping Functionality in Music Assistant
---

# Groups

!!! warning "Note"
    Do not delete or disable a player which is in a group while the group is playing
    
## Manual Sync

These are temporary group players that are configured via the [Player List](ui.md#player-list) or HA action. They provide a very flexible way to create and destroy groups as the players can be added or removed at any time. 

### Features

- Players can be dynamically added or removed before or during playback
- Players in the group will play in perfect sync
- The HA actions of [media_player.join](https://www.home-assistant.io/integrations/media_player/#action-media_playerjoin) and [media_player.unjoin](https://www.home-assistant.io/integrations/media_player/#action-media_playerunjoin) can be used to manage this type of group

### Known Issues / Notes

- Players can be linked via the checkbox in the [Player List](../ui.md#player-list)
- Only like player types that support synced playback are allowed to be joined
- When players are grouped in this way only the main player holds the queue. For example, player A has an existing queue and players B and C are joined to player A. If player A is turned off players B and C will stop
- Some player types may pause momentarily as a new device is joined
- HA automations could be used to quickly create an often used group of players 

## Sync Groups

These are permanent group players that are configured via the MA settings. These should be used when the make up of the group will rarely change and it is desired that the players will typically all continuously play music. A player cannot be removed from the group if the group is playing although it can be muted (or volume reduced to 0). If there is a need to add or remove additional players then use the dynamic members option or switch to using a manual sync group. At power on/off of the syncgroup the original set of members will be restored.

![image](../assets/screenshots/syncgroup.png)

After selecting ADD GROUP PLAYER as shown above, the group configuration screen will appear where the group type must be selected. At least one player must be added to the group before the group can be saved. It is recommended that a custom name be used as the automatically generated name is unique but not memorable.

!!! tip
    Remember Chromecast devices must be grouped in the Google Home app for perfect sync

### Features

- The native player types shown (i.e. not the Universal Group option) support perfect sync (this is a requirement to be shown in the list)
- In addition to playing in sync the group will hold the queue regardless of which player(s) in the group are powered off
  
### Known Issues / Notes

- As mentioned above the group player holds the queue rather than any individual player. However, if the group leader is lost then playback will stop but the queue can be restarted
- The group will not power on if one of the child players is synced to another group
- If, in the group settings, the player friendly name has been replaced by an unusual ID then likely the player was seen by the MA logic as unavailable. Check the individual player status

## Universal Groups

Music Assistant has support for grouping dissimilar playback devices. Configuration is done in the same manner as described in the Sync Groups section with UNIVERSAL selected as the Group Type. 

### Features

All devices can be grouped but not all will play in sync. See the individual player providers page for any specific limitations.

### Known Issues / Notes

- This type of group should be avoided if a [Sync Group or Native Group](../ui.md#grouping-players) can be used
- Universal Groups can include sync groups but not other universal groups
- The group will not power on if one of the child players is synced to another group
- The group will power on if one or more of the child players is unavailable
- If, in the group settings, the player friendly name has been replaced by an unusual ID then likely the player was seen by the MA logic as unavailable. Check the individual player status
- Universal Groups are on a best-effort basis and might not work correctly with all player models or give unexpected results
