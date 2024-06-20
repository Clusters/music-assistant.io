---
title: Player Support
description: Information Relevant to all Player Providers 
---

# Player Providers

For specific player provider information refer to the relevant section in this document. Most players are enabled by default and will be automatically discovered by Music Assistant. The following table summarises player capabilities

![Preview image](../assets/player-provider-summary.png){ width = 600 }

If a device supports multiple protocols then multiple players for the device will be seen. In the player provider settings you can disable or hide any players you do not use. Deleting a player is useful if there is a problem with it. Deleted players will get rediscovered and will return to the list on MA restart or player provider reload.

![Preview image](../assets/screenshots/player-disable.png){ width = 600 } 

!!! note
    If any player is not transitioning between songs then check if the player has the option QUEUE FLOW MODE. Try enabling it if it does.

## Audio Quality

Audio quality is the principal reason why native MA players are developed. These players provide the highest quality playback experience. HA players should work and may work well but they may also have been written with a basic objective such as enabling text to speech. Therefore, if there is a MA player available and a HA integration then you should always choose the MA player. 

48kHz / 24 bits and above is considered High Resolution (Hi Res)
