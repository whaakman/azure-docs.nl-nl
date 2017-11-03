---
title: Inleiding tot pakketopname in Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de mogelijkheid van netwerk-Watcher pakket vastleggen
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 152cc8fb61aa6115c7b5863e4d798db9e7aa5b7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Inleiding tot variabele pakketopname in Azure-netwerk-Watcher

Netwerk-Watcher variabele pakketopname kunt u om pakket vastleggen sessies om bij te houden van verkeer van en naar een virtuele machine te maken. Helpt bij het vastleggen van pakket op te sporen netwerk afwijkingen beide reactief en proactivity. Andere toepassingen zijn onder andere het verzamelen van netwerkstatistieken, krijgt informatie over het netwerk beveiligingsrisico's voor foutopsporing van client-servercommunicaties en nog veel meer.

Pakketopname is een uitbreiding van virtuele machine die op afstand via het netwerk-Watcher wordt gestart. Deze mogelijkheid kan de werkbelasting van een pakketopname handmatig worden uitgevoerd op de gewenste virtuele machine, die kostbare tijd bespaart vergemakkelijken. Pakketopname kan worden geactiveerd via de portal, PowerShell, CLI of REST-API. Er is een voorbeeld van hoe pakketopname kan worden geactiveerd met waarschuwingen van de virtuele Machine. Filters zijn beschikbaar voor de opnamesessie om ervoor te zorgen vastleggen van verkeer dat u wilt bewaken. Filters zijn gebaseerd op 5-tuple (protocol, lokale IP-adres, extern IP-adres, poort lokale en externe poort) informatie. De vastgelegde gegevens wordt opgeslagen in de lokale schijf of een blob storage. Er is een limiet van 10 pakket vastleggen sessies per regio per abonnement. Deze beperking geldt alleen voor de sessies en niet van toepassing op de bestanden van de capture opgeslagen pakket lokaal op de virtuele machine of in een opslagaccount.

> [!IMPORTANT]
> Pakketopname vereist dat de extensie van een virtuele machine `AzureNetworkWatcherExtension`. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

Als u de informatie die u alleen de informatie die u wilt vastleggen, zijn de volgende opties beschikbaar voor een sessie voor het vastleggen van pakket:

**Configuratie vastleggen**

|Eigenschap|Beschrijving|
|---|---|
|**Maximum aantal bytes per pakket (in bytes)** | Het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als leeg is. Het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als leeg is. Als u alleen de IPv4-header – moet geven 34 hier |
|**Maximum aantal bytes per sessie (bytes)** | Totaal aantal bytes in die zijn vastgelegd, zodra de sessie wordt beëindigd door de waarde is bereikt.|
|**Tijdslimiet (seconden)** | Stelt een tijdsbeperking van de voor het pakket vastleggen sessie. De standaardwaarde is 18000 seconden of vijf uur.|

**Filteren (optioneel)**

|Eigenschap|Beschrijving|
|---|---|
|**Protocol** | Het protocol voor het filteren van voor het vastleggen van het pakket. De beschikbare waarden zijn TCP, UDP en alle.|
|**Lokaal IP-adres** | Deze waarde filtert de pakketopname voor de pakketten waar het lokale IP-adres overeenkomt met de filterwaarde van dit.|
|**Lokale poort** | Deze waarde filtert de pakketopname om pakketten van de lokale poort overeenkomt met de filterwaarde van dit.|
|**Extern IP-adres** | Deze waarde filtert de pakketopname voor de pakketten waar het externe IP-adres overeenkomt met de filterwaarde van dit.|
|**Externe poort** | Deze waarde filtert de pakketopname pakketten waarbij de externe poort overeenkomt met de waarde voor dit filter.|

### <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u het pakket mogelijk via de portal kunt beheren in via [pakketopname in de Azure portal beheren](network-watcher-packet-capture-manage-portal.md) of met PowerShell in via [pakket vastleggen beheren met PowerShell](network-watcher-packet-capture-manage-powershell.md).

Informatie over het maken van proactieve pakket opnamen op basis van waarschuwingen van de virtuele machine in via [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













