---
title: Inleiding tot pakketopname in Azure Network Watcher | Microsoft Docs
description: Deze pagina biedt een overzicht van de mogelijkheden van Network Watcher packet capture
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 8ef9da86377ac6f1b012cb0ebfd9d6866bc0c620
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061585"
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Inleiding tot variabele pakketopname in Azure Network Watcher

Variabele pakketopname van Network Watcher kunt u packet capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Pakketopname helpt bij het opsporen van afwijkingen netwerk reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen.

Pakketopname is de extensie van een virtuele machine die op afstand via Network Watcher is gestart. Deze mogelijkheid vereenvoudigt het ongemak van het uitvoeren van een pakketopname handmatig op de gewenste virtuele machine, waardoor waardevolle tijd worden opgeslagen. Pakketopname kan worden geactiveerd via de portal, PowerShell, CLI of REST-API. Er is een voorbeeld van hoe pakketopname kan worden geactiveerd met waarschuwingen van de virtuele Machine. Filters zijn beschikbaar voor de opnamesessie om te controleren of vastleggen van verkeer die u wilt bewaken. Filters zijn gebaseerd op 5-tuple (protocol, lokale IP-adres, extern IP-adres, lokale poort en externe poort) informatie. De vastgelegde gegevens worden opgeslagen in de lokale schijf of een opslag-blob. Er is een limiet van 10 packet capture-sessies per regio per abonnement. Deze limiet geldt alleen voor de sessies en geldt niet voor de opgeslagen packet capture-bestanden lokaal op de virtuele machine of in een storage-account.

> [!IMPORTANT]
> Pakketopname is vereist voor een VM-extensie `AzureNetworkWatcherExtension`. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

Als u wilt de gegevens die u vastlegt op alleen de informatie die u wilt beperken, zijn de volgende opties beschikbaar voor een sessie voor het vastleggen van pakket:

**Configuratie vastleggen**

|Eigenschap|Description|
|---|---|
|**Maximum aantal bytes per pakket (bytes)** | Het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als dit veld leeg blijft. Het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als dit veld leeg blijft. Als u alleen de IPv4-header: 34 hier geven |
|**Maximum aantal bytes per sessie (bytes)** | Totaal aantal bytes in die zijn vastgelegd, zodra de sessie wordt beëindigd door de waarde is bereikt.|
|**Tijdslimiet (seconden)** | Stelt een tijdsbeperking van de voor het pakket vastleggen sessie. De standaardwaarde is 18000 seconden of vijf uur.|

**Filteren (optioneel)**

|Eigenschap|Description|
|---|---|
|**Protocol** | Het protocol om te filteren op de pakketopname. De beschikbare waarden zijn TCP, UDP en alle.|
|**Lokaal IP-adres** | Deze waarde filtert de pakketopname pakketten waarvan het lokale IP-adres overeenkomt met de waarde voor dit filter.|
|**Lokale poort** | Deze waarde filtert de pakketopname pakketten waar de lokale poort komt overeen met de waarde voor dit filter.|
|**Extern IP-adres** | Deze waarde filtert de pakketopname pakketten waarvan het externe IP-adres overeenkomt met de waarde voor dit filter.|
|**Externe poort** | Deze waarde filtert de pakketopname pakketten waar de externe poort komt overeen met de waarde voor dit filter.|

### <a name="next-steps"></a>Volgende stappen

Leer hoe u pakketopnamen via de portal kunt beheren door naar de pagina [pakketopname in Azure portal beheren](network-watcher-packet-capture-manage-portal.md) of met PowerShell, recentst [vastleggen van pakketten beheren met PowerShell](network-watcher-packet-capture-manage-powershell.md).

Informatie over het maken van proactieve pakketopnamen op basis van waarschuwingen van de virtuele machine, recentst [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













