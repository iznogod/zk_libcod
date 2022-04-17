# libcod for \*.ZK.\* Zombie Knights

Initial fork path:

<pre>
https://github.com/ibuddieat/zk_libcod (this repository)
└─> <a href="https://github.com/damage99/libcod">damage99/libcod</a>
    └─> <a href="https://github.com/voron00/libcod">voron00/libcod</a>
        └─> <a href="https://github.com/M-itch/libcod">M-itch/libcod</a>
            └─> <a href="https://github.com/kungfooman/libcod">kungfooman/libcod</a> (original libcod implementation)
</pre>

This extension ...
- provides interoperability between the Call of Duty&reg; 2 server and other software components and/or advanced game modifications (so-called "mods")
- intends to improve the overall game experience and security as the extended software (Call of Duty&reg; 2) is not maintained anymore since more than a decade
- has its focus on Call of Duty&reg; 2 in version 1.3, other versions are not fully supported (unless the respective offsets are added)
- is maintained for non-profit and educational purposes

List of high-level changes provided by this repository:
- Added cvars:
  * `sv_limitLocalRcon` to whitelist internal IPs at rcon rate limiting
  * `sv_logRcon` to disable (successful) rcon command logging
  * `sv_logHeartbeat` to disable logging of heartbeats that are sent to the master server
  * `sv_timeoutMessages` to disable player timeout messages being shown to other players
  * `sv_botKickMessages` to disable bot disconnect messages being shown to other players
  * `sv_kickMessages` to generally disable player kick messages being shown to other players
  * `g_debugEvents` to log events such as player footsteps, temporary entities etc.
  * `g_logPickup` to control logging of item pickup actions
  * `g_notifyPickup` to define whether to use the stock pickup logic or custom notify events
  * `g_debugStaticModels` to print info about static models on map load
- Added script code functions:
  * `<player> getClientHudElemCount()`
  * `<player> getGroundEntity()`
  * `<player> getInactivityTime()`
  * `<player> noclip("on|off|toggle")`
  * `<player> playFxForPlayer(<effect id>, <position of effect>, [<forward vector>], [<up vector>])`
  * `<player> playFxOnTagForPlayer(<effect id>, <entity>, <tag name>)`
  * `<player> runScriptAnimation()`
  * `<player> setEarthquakes("on|off|toggle")`
  * `<player> silent("on|off|toggle")`
  * `<weapon> getWeaponItemAmmo()`
  * `<weapon> setWeaponItemAmmo(<value>)`
  * `getWeaponFuseTime(<weapon name>)`
  * `setWeaponFuseTime(<weapon name>, <time in ms>)`
  * `getEntityCount()`
  * `setNextTestClientName(<name>)`
  * `resetTestClientNaming()`
  * `logPrintConsole(<message>)`
- Changed script code functions:
  * `<player> get_userinfo()` now returns strings only, instead of string or undefined
  * `obituary(<victim>, <attacker>, <weapon>, <meansOfDeath>, [<team>], [<origin>], [<max. distance>])`
- Added script callback functions:
  * `CodeCallback_Error`
  * `CodeCallback_ReloadButton`
  * `CodeCallback_LeanLeftButton`
  * `CodeCallback_LeanRightButton`
  * `CodeCallback_ProneButton`
  * `CodeCallback_CrouchButton`
  * `CodeCallback_JumpButton`
  * `CodeCallback_AdsButton`
  * `CodeCallback_MeleeBreathButton`
  * `CodeCallback_HoldBreathButton`
  * `CodeCallback_FragButton`
  * `CodeCallback_SmokeButton`
- Removed libcod cvars:
  * `con_coloredPrints` as it may break incoming rcon commands, thus causing issues with BigBrotherBot
- Reconstructed functions (see functions with `custom_` prefix in `libcod.cpp`):
  * `Touch_Item` to gain more control over item pickup actions
  * `SV_DropClient` to filter disconnect messages via various cvars
  * `SV_SendClientGameState` for miscellaneous game engine tests (e.g., connect configstrings)
  * `BG_AddPredictableEventToPlayerstate` et sequentes to be able to filter events
  * `G_AddEvent`
  * `MSG_WriteDeltaPlayerstate` et sequentes to be able to filter player/entity attributes
  * `MSG_WriteDeltaStruct`
  * `MSG_WriteDeltaField`
  * `MSG_WriteDeltaClient`
  * `MSG_WriteDeltaEntity`
  * `MSG_WriteDeltaArchivedEntity`
  * `SV_EmitPacketEntities`
  * `SV_EmitPacketClients`
  * `SV_ArchiveSnapshot`
  * `SV_BuildClientSnapshot`
  * `SV_WriteSnapshotToClient`
  * `Com_Error` et sequentes to be able to pass errors to gsc code
  * `Scr_ErrorInternal`
  * `RuntimeError_Debug`
  * `RuntimeError`
  * `CM_IsBadStaticModel` for `g_debugStaticModels` (this function name is a guess)
- Commented out several libcod functions that would make it easy to harm the server with malicious map scripts, thus currently breaking manymaps support
- Added/updated some missing/unknown declarations
- Fixed some minor bugs

Build requirements:
- gcc and g++ (with multilib on 64-bit x86 operating systems)
- libstdc++5
- MySQL client (if required by functionality)

Requirements installation (for Ubuntu 18.04.5 LTS):
```
dpkg --add-architecture i386
apt update
apt install gcc-multilib
apt install g++-multilib
apt install libstdc++5:i386
apt install libmysqlclient-dev:i386
```

Creating the binary (written to `./bin`):
```
./doit.sh
```