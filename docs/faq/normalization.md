# Technical Information

This section contains more indepth technical information regarding Music Assistant's workings for those interested.

## Volume Normalization

The normalization value to totally avoid clipping should be -23 LUFS based on this official documentation https://tech.ebu.ch/docs/r/r128.pdf but values differ per plattform so it is complicated to get the right value.

Here is a good document from the mastering plugin developer izotope comparing different plattforms like YouTube, Spotify, Deezer, Tidal and many more. [This page explains the whole topic very well](https://www.izotope.com/en/learn/mastering-for-streaming-platforms.html#loudness-specifications-by-streaming-platform)

Noting the above, the default value is -17 which should be a good compromise in 99% of cases. MA originally had it set the to EBU's standard value of -23 but then there were complaints from people finding the music too silent.

The equalizer is bypassed when the value is set to 0 but in general, it is not recommended to turn off the volume normalization because there are so many different loudness levels, especially if music is played from different sources. MA uses an integrated loudness level based on the EBU-R128 standard and only adjusts gain of the entire track up or down so there is no compression of dynamic range. For that same reason of not losing dynamic range MA does not use a limiter and it's the user's responsibility to use sane values for the target level of the volume normalization.

If audio is only played from one single source (e.g. Deezer) and that audio source already has normalized its audio files, then its safe to disable normalization in MA. If audio is played from different sources or audio is not normalized at the source, it is highly recommended to leave normalization enabled for the best experience.

**More technical details**

All tracks are processed as raw pcm by Music Assistant internally. So everything that is played will be first converted to raw pcm in the sample rate and bit depth of the origin (unless flow mode is enabled) and the gain adjustment is done while extracting the raw source media so the pcm chunks passed into the streaming engine have the gain adjustment applied. In this way there should be enough headroom within the 16 or 24 bits bit depth. Converting to 32 or 64 bits floating point is something that has been considered but it was decided that it was not worth the overhead and comes at the risk of dithering when going back from 64 bits to 16 or 24 bits so instead the MA design settled on applying the correction gain while extracting the raw audio from its base codec.

All further processing in MA is done at PCM raw audio level, such as the optional equalizer (in the future maybe some room correction or other filters) - if "flow mode" is enabled crossfading is also done on the raw pcm chunks but that will upsample all audio into one static sample rate and bit depth to create one "flow" of audio to send to the player.

The final part in the chain is that MA sends either the raw pcm data to the player or encodes it into FLAC due to it being lossless while keeping a reasonable file size.

## Track Queueing

MA has 3 ways of enqueuing tracks to players:

**BASIC/FALLBACK**
send tracks from the queue one by one. We just watch the player and if we see that it stopped playing at the end of a track, we tell it to play the next. This is the most basic form of enqueuing and completely handled by the MA queue controller and thus works with ALL players. This will NOT support gapless or crossfade. This setting is the default if the player has no native enqueue support or flow mode enabled. By default DLNA players use this option as well as the upcoming Home Assistant based players. Full metadata is sent to the player of what is playing.

**NATIVE ENQUEUE SUPPORT**
The player has native enqueue support so we tell it what the next track will be right before the current one ends. This is a special feature that not all players support. Its supported (and by default enabled) for Slimproto, Google Cast and Sonos players. Some DLNA players support it but others don't so we offer a player setting to enable it so a user can try himself if the player accepts it. benefit is full support gapless playback and in case of Slimproto and Sonos also crossfade. Also full metadata will be sent to the player what is currently playing.

**FLOW MODE**
As an alternative to native enqueuing we can also send one stream of audio to the player and let Music Assistant stitch the songs together (with gapless or crossfade). This mode is used by default for Airplay and snapcast and will be used by DLNA and Cast if crossfade gets enabled. Downside of this approach is that Google cast (and most DLNA players) looses metadata display (on the speaker itself). Some DLNA players support "icy metadata" which is what we send to inform what is playing. 

## Player Perfect Sync

Audio sync between speakers is only possible if there is accurate knowledge of the exact (millisecond-precise) timestamp a player played each audio frame. In practice there are 2 common strategies used to sync audio:

1) Sending timestamped frames to each player - there is one master clock and audio frames are sent to each receiving device in a sync group including the timestamp. So each player will have knowledge when it should play each frame and it can drop or inject frames to keep in sync. This strategy is often used in the proprietary protocols such as Google Cast, Sonos, Roon RAAT, BluOS and even Apple Airplay (RAOP).
This is by far the best (most precise) sync method because each player is responsible for sending the correct audio frames to the DAC (or digital transport).

2) Server-side corrected sync. Each player is sent the audio stream from the centralized source and then the server keeps track of each player's latency and corrects it by pausing/forwarding, playing faster/slower or injecting frames. This is easier to implement because the only thing needed client-side is accurate progress reporting of the client (how many milliseconds it played) and the server contains all the centralized logic to keep the sync. This is used by, for example, slimproto (squeezebox) and snapcast.

How well the time synchronisation works depends on multiple factors but often you will find that strategy 1 is superior to strategy 2 and/or strategy 2 needs tweaking to get it right. For instance on snapcast you often hear skipping (small disturbances to the sound while its adjusting) and slimproto works very poorly with wifi based devices due to changing player latency.

RECOMMENDATION: To have the best synced audio experiences of players, try to stick with one ecosystem and, if possible, choose one of the options that implement strategy 1 (proprietary protocol).
Airplay is favoured because it a very good sync protocol that originally was proprietary but has been reverse engineered. You can now have airplay targets in both commercially available audio gear (even high end equipment) and DIY devices. It outperforms all the strategy 2 stream protocols by a wide margin. The same applies to a $35 chromecast audio puck with the google cast protocol. It works really well out of the box.
