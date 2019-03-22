---
title: Pakketopname beheren met Azure Network Watcher - Azure portal | Microsoft Docs
description: Informatie over het beheren van de functie voor het vastleggen van pakketten van Network Watcher met behulp van de Azure portal.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: jdial
ms.openlocfilehash: 22bdd50f129a48ade97db323f904f7e652a00d39
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57889978"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Pakketopname beheren met Azure Network Watcher met behulp van de portal

Network Watcher packet-capture kunt u capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt bij het opsporen van afwijkingen, netwerk, zowel reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, foutopsporing, client-servercommunicatie en nog veel meer verzamelen. Kunnen op afstand activeren pakket wordt vastgelegd, vereenvoudigt de belasting van het uitvoeren van een pakketopname handmatig op een gewenste virtuele machine, die kostbare tijd bespaart.

In dit artikel leert u om te starten, stoppen, downloaden en een pakketopname verwijderen. 

## <a name="before-you-begin"></a>Voordat u begint

Pakketopname vereist de volgende verbinding:
* Uitgaande connectiviteit met een storage-account via poort 443.
* Binnenkomende en uitgaande connectiviteit met 169.254.169.254
* Binnenkomende en uitgaande connectiviteit met 168.63.129.16

Als een netwerkbeveiligingsgroep gekoppeld aan de netwerkinterface, of het subnet waarin de netwerkinterface zich is bevindt, zorgt u ervoor dat regels bestaan die de vorige poorten toestaat. 

## <a name="start-a-packet-capture"></a>Een pakketopname starten

1. Navigeer in uw browser naar de [Azure-portal](https://portal.azure.com) en selecteer **alle services**, en selecteer vervolgens **Network Watcher** in de **netwerksectie**.
2. Selecteer **pakketopname** onder **diagnostische hulpprogramma's voor het netwerk**. Eventuele bestaande pakketopnamen worden weergegeven, ongeacht hun status.
3. Selecteer **toevoegen** te maken van een pakketopname. U kunt waarden voor de volgende eigenschappen selecteren:
   - **Abonnement**: Het abonnement waarin de virtuele machine die u wilt maken van het pakket worden vastgelegd voor heeft.
   - **Resourcegroep**: De resourcegroep van de virtuele machine.
   - **Doel-virtuele-machine**: De virtuele machine die u wilt maken van de pakketopname voor.
   - **Naam van pakketopname**: Een naam voor de pakketopname.
   - **Storage-account of het bestand**: Selecteer **opslagaccount**, **bestand**, of beide. Als u selecteert **bestand**, het vastleggen wordt geschreven naar een pad vanuit de virtuele machine.
   - **Lokaal bestandspad**: Het lokale pad op de virtuele machine waar de pakketopname wordt opgeslagen (alleen geldig als *bestand* is geselecteerd). Het pad moet een geldig pad. Als u van een virtuele Linux-machine gebruikmaakt, het pad moet beginnen met */var/vastgelegd*.
   - **Storage-accounts**: Selecteer een bestaand opslagaccount, als u hebt geselecteerd *opslagaccount*. Deze optie is alleen beschikbaar als u hebt geselecteerd **opslag**.
   
     > [!NOTE]
     > Premium storage-accounts worden momenteel niet ondersteund voor het opslaan van pakket vastgelegd.

   - **Maximum aantal bytes per pakket**: Het aantal bytes van elk pakket die zijn vastgelegd. Als dit veld leeg blijft, worden alle bytes zijn vastgelegd.
   - **Maximum aantal bytes per sessie**: Het totale aantal bytes die worden vastgelegd. Wanneer de waarde is de packet capture stopt is bereikt.
   - **Tijdslimiet (seconden)**: De tijdslimiet voordat de pakketopname is gestopt. De standaardwaarde is 18.000 seconden.
   - Filteren (optioneel). Selecteer **+ filter toevoegen**
     - **Protocol**: Het protocol om te filteren op de pakketopname. De beschikbare waarden zijn TCP, UDP en alle.
     - **Lokaal IP-adres**: Filtert de pakketopname voor pakketten waarvan het lokale IP-adres overeenkomt met deze waarde.
     - **Lokale poort**: Filtert de pakketopname voor pakketten, waarbij de lokale poort komt overeen met deze waarde.
     - **Extern IP-adres**: Filtert de pakketopname voor pakketten die het externe IP-adres overeenkomt met deze waarde.
     - **Externe poort**: Filtert de pakketopname voor pakketten, waarbij de externe poort komt overeen met deze waarde.
    
     > [!NOTE]
     > Poort- en IP-adres-waarden mag bestaan uit één waarde, bereik van waarden of een bereik, zoals 80-1024, voor de poort. U kunt zoveel filters definiëren.

4. Selecteer **OK**.

Nadat de tijdslimiet ingesteld voor de pakketopname is verlopen, worden de pakketopname is gestopt, en kan worden gecontroleerd. U kunt ook handmatig een packet capture-sessie beëindigen.

> [!NOTE]
> De portal automatisch:
>  * Hiermee maakt u een network watcher in dezelfde regio als de regio voor de virtuele machine die u hebt geselecteerd, bestaat als de regio nog niet over een netwerk-watcher.
>  * Voegt de *AzureNetworkWatcherExtension* [Linux](../virtual-machines/linux/extensions-nwa.md) of [Windows](../virtual-machines/windows/extensions-nwa.md) extensie van de virtuele machine op de virtuele machine, als deze nog niet geïnstalleerd.

## <a name="delete-a-packet-capture"></a>Een pakketopname verwijderen

1. Selecteer in de weergave van het vastleggen van pakketten **...**  aan de rechterkant van het pakket vastleggen, of met de rechtermuisknop op een bestaande pakketopname en selecteer **verwijderen**.
2. U wordt gevraagd te bevestigen dat u wilt de pakketopname verwijderen. Selecteer **Ja**.

> [!NOTE]
> Een pakketopname verwijderen, verwijdert niet de capture-bestand in de storage-account of op de virtuele machine.

## <a name="stop-a-packet-capture"></a>Een pakketopname stoppen

Selecteer in de weergave van het vastleggen van pakketten **...**  aan de rechterkant van het pakket vastleggen, of met de rechtermuisknop op een bestaande pakketopname en selecteer **stoppen**.

## <a name="download-a-packet-capture"></a>Een pakketopname downloaden

Zodra uw pakket opnamesessie is voltooid, wordt de capture-bestand wordt geüpload naar blob-opslag of naar een lokaal bestand op de virtuele machine. De opslaglocatie van de pakketopname is gedefinieerd tijdens het maken van de pakketopname. Er is een handige hulpprogramma voor vastleggen van bestanden die zijn opgeslagen in een storage-account toegang tot Microsoft Azure Storage Explorer, die u kunt [downloaden](https://storageexplorer.com/).

Als een storage-account is opgegeven, worden pakketten vastleggen van bestanden worden opgeslagen in een storage-account op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

Als u hebt geselecteerd **bestand** tijdens het maken van het vastleggen, kunt u weergeven of downloaden van het bestand van het pad dat u hebt geconfigureerd op de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het automatiseren van pakketopnamen met waarschuwingen van de virtuele machine, [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md).
- Om te bepalen of er een specifiek verkeer is toegestaan in of uit een virtuele machine, Zie [een probleem VM-netwerk-verkeersfilter vaststellen](diagnose-vm-network-traffic-filtering-problem.md).
