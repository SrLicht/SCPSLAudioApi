# SCPSLAudioAPi

This is an API Library that does not depend on any Plugin frameworks for its function.
This API provides a implementation of an Audio player in a SCP:SL Server with options to extend

## Installation
Download the latest release from the [Releases](https://github.com/CedModV2/SCPSLAudioApi/releases/) page and place the SCPSLAudioAPi.dll file in your plugin dependencies folder

## Usage
Note: This is **NOT** an audioplayer plugin, this is a library to make creating a plugin that can play audio easier. To be able to play audio you will need to find a plugin that does that using this api, or a custom solution.

## Developer Usage
The Library mainly consists of the AudioPlayerBase class, which can be inherited in combination with overriding Methods to create custom logic while still having the heavy lifting done by the Api.

### Without a custom AudioPlayer class
Simply Create a Dummyplayer (Not included in the library) and Call AudioPlayerBase.Get() on the ReferenceHub of this DummyPlayer.

To play Audio, you must first, insert a Path to the audio file into the Queue, This can be done by directly modifying AudioToPlay, but it is recommend to use the Enqueue Method. \
When audio is in the queue, call the Play method and after the file has been loaded, it should start playing your audio.

### With a Custom AudioPlayer class
Simply Create a Dummyplayer (Not included in the library) and Call AudioPlayerBase.Get() on the ReferenceHub of this DummyPlayer.

Custom AudioPlayer classes work the same way as a normal AudioPlayerBase, the developer can change whatever they feel like in the methods for playing and broadcasting.
Refer to the source of AudioPlayerBase to see what each method does.

You can get your AudioPlayer class in multiple ways
You can try to find it in AudioPlayerBase.AudioPlayers, but it is recommended to create a helper Method.

```csharp
public static CustomAudioPlayer Get(ReferenceHub hub)
{
    if (AudioPlayers.TryGetValue(hub, out AudioPlayerBase player))
    {
        if (player is CustomAudioPlayer cplayer1)
            return cplayer1;
    }

    var cplayer = hub.gameObject.AddComponent<CustomAudioPlayer>();
    cplayer.Owner = hub;

    AudioPlayers.Add(hub, cplayer);
    return cplayer;
}
```

**Note:** For Urls to work you must set AllowUrl to true on the AudioPlayerBase instance.

### Features:
Volume control - `AudioPlayerBase::Volume` \
Queue Looping - `AudioPlayerBase::Loop` \
Queue Shuffle - `AudioPlayerBase::Shuffle` \
Autoplay - `AudioPlayerBase::Continue` \
Pausing - `AudioPlayerBase::ShouldPlay` \
Play From URL - `AudioPlayerBase::AllowUrl` \
Play to specific players only (Will play to everyone if the list is Empty) - `AudioPlayerBase::BroadcastTo` \
Debug logs (can cause spam) - `AudioPlayerBase::LogDebug`
