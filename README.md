# Tuto-TokoVoip-FiveM
Voici le Tuto d'installation de Toko Voip sur FiveM
Tout droits réservés Alexis Brossard


**Etape 1 :**

Déstart esx voice -> ```#esx_voice``` dans le server.cfg

*Désinstaller mumble artefact*

Aller dans *alpine\opt\cfx-server*

Renommer -> ```libvoip-server-mumble.json``` en -> ```ihaverename-mumble.json```

Ensuite dans :
components.json  -> Supprimer =

```  "voip-server:mumble",```

**Etape 2 :** 

Installer TokoVoip + Ls Radio plus SR_TokoRadioswitch et les **ensure**

Toko TS3 -> https://github.com/Itokoyamato/TokoVOIP_TS3/releases/download/v1.2.5-v1.3.5/tokovoip_1_2_5.ts3_plugin
LS-Radio -> https://github.com/alcapone-dev/ls-radio
SR_TokoRadioswitch -> https://github.com/TheStonedRaider/SR_TokoRadioswitch

```ensure tokovoip_script
ensure ls-radio
ensure SR_TokoRadioswitch```

**Etape 3 :** 

Créer les canaux TS + Add plugin Toko

**Etape 4 :** 

Config le C_config dans tokovoip_script

```		TSChannel = "Server",
		TSPassword = "motdepasse", -- TeamSpeak channel password (can be empty)

		-- Optional: TeamSpeak waiting channel name, players wait in this channel and will be moved to the TSChannel automatically
		-- If the TSChannel is public and people can join directly, you can leave this empty and not use the auto-move
		TSChannelWait = "Acceuil",

		-- Blocking screen informations
		TSServer = "94.250.223.6:15136", -- TeamSpeak server address to be displayed on blocking screen
		TSChannelSupport = "Support 1", -- TeamSpeak support channel name displayed on blocking screen
		TSDownload = "http://papybrossard.fr", -- Download link displayed on blocking screen
		TSChannelWhitelist = { -- Black screen will not be displayed when users are in those TS channels
			"Support 1",
			"Support 2",```
Les mettre au même nom que les canaux vocaux du TS !!

**Etape 5 :**

Dans  [gcphone/client/client.lua]

1 / Ajouter ceci -> local TokoVoipID = nil  (ligne 19)

2/ Remplacer ceci :

```NetworkSetVoiceChannel(infoCall.id + 1)
NetworkSetTalkerProximity(0.0)```

Par ceci : 

```exports.tokovoip_script:addPlayerToRadio(infoCall.id + 120)
TokoVoipID = infoCall.id + 120```

3/ Maintenant trouver : 

```NetworkSetTalkerProximity(2.5)```

Et remplacer ceci par ceci : 

```exports.tokovoip_script:removePlayerFromRadio(TokoVoipID)
TokoVoipID = nil```

4/ Ensuite trouver : 

```RegisterNUICallback(‘notififyUseRTC’, function (use, cb)```

Et ajouter :

```exports.tokovoip_script:removePlayerFromRadio(TokoVoipID)
TokoVoipID = nil```

Sous : 

```Citizen.InvokeNative(0xE036A705F989E049)```

5/ Ensuite supprimer :
 
```NetworkSetTalkerProximity(2.5)```


Copyright PapyBrossard 2019 | Alexis Brossard
