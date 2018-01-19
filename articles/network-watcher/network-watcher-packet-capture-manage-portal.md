---
title: Pakket opnamen met Azure-netwerk-Watcher - Azure-portal beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de functie voor het vastleggen van pakket van netwerk-Watcher met Azure portal beheren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c283fb4d1a390de88b425ce51a312b5404503f0d
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Pakket opnamen beheren met Azure met behulp van de portal netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Netwerk-Watcher pakketopname kunt u om sessies vastleggen om bij te houden van verkeer van en naar een virtuele machine te maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt op te sporen netwerk afwijkingen reactief en proactief. Andere toepassingen zijn onder andere het verzamelen van netwerkstatistieken, krijgt informatie over het netwerk beveiligingsrisico's voor foutopsporing van client-servercommunicaties en nog veel meer. Door op afstand activeren pakket opnamen, deze mogelijkheid kan vergemakkelijken de last van een pakketopname handmatig en op de gewenste machine, die kostbare tijd bespaart worden uitgevoerd.

In dit artikel leert u de verschillende beheertaken die momenteel beschikbaar voor pakketopname zijn.

- [**Start een pakketopname**](#start-a-packet-capture)
- [**Stop de pakketopname van een**](#stop-a-packet-capture)
- [**Het vastleggen van een pakket verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u de volgende bronnen hebt:

- Een exemplaar van netwerk-Watcher in de regio die u wilt maken van een pakketopname
- Een virtuele machine met de pakket-capture-extensie is ingeschakeld.

> [!IMPORTANT]
> Pakketopname vereist dat de extensie van een virtuele machine `AzureNetworkWatcherExtension`. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Pakket vastleggen agent uitbreiding via de portal

Voor het installeren van het pakket vastleggen VM-agent via de portal, gaat u naar de virtuele machine, klikt u op **extensies** > **toevoegen** en zoek naar **netwerk-Watcher-Agent voor Windows**

![Agent weergeven][agent]

## <a name="packet-capture-overview"></a>Overzicht van pakket vastleggen

Navigeer naar de [Azure-portal](https://portal.azure.com) en klik op **Networking** > **netwerk-Watcher** > **pakket vastleggen**

De overzichtspagina ziet u een lijst met alle pakketten die bestaan ongeacht de status vastlegt.

> [!NOTE]
> Pakketopname vereist connectiviteit met de storage-account via poort 443.

![scherm overzicht van pakket vastleggen][1]

## <a name="start-a-packet-capture"></a>Start een pakketopname

Klik op **toevoegen** een pakketopname maken.

De eigenschappen die kunnen worden gedefinieerd op een pakketopname zijn:

**Belangrijkste instellingen**

- **Abonnement** -deze waarde is het abonnement dat wordt gebruikt, een exemplaar van netwerk-watcher nodig is in elk abonnement.
- **Resourcegroep** -de resourcegroep van de virtuele machine die wordt benaderd.
- **Virtuele machine als doel** -de virtuele machine die wordt uitgevoerd de pakketopname
- **Pakketnaam vastleggen** -deze waarde is de naam van het pakket vastleggen.

**Configuratie vastleggen**

- **Storage-Account** -bepaalt als pakketopname wordt opgeslagen in een opslagaccount.
- **Bestand** -bepaalt als een pakketopname lokaal wordt opgeslagen op de virtuele machine.
- **Storage-Accounts** : de storage-account op te slaan van het pakket vastleggen in geselecteerd. Standaardlocatie is https://{storage-account-id name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription} /resourcegroups/ {name}/providers/microsoft.compute/virtualmachines/{virtual machine Resourcegroepnaam} / {jj} / {MM} / {DD} / {HH} packetcapture__{MM}_{SS} _ {XXX} Cap. (Alleen beschikbaar als **opslag** is geselecteerd)
- **Lokale bestandspad** -het lokale pad op een virtuele machine op te slaan van het vastleggen van het pakket. (Alleen beschikbaar als **bestand** is ingeschakeld). Een geldig pad moet worden opgegeven. Voor een virtuele Linux-machine, het pad moet beginnen met */var/vastgelegd*.
- **Maximum aantal bytes per pakket** - het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als leeg is.
- **Maximum aantal bytes per sessie** - totaal aantal bytes die zijn vastgelegd, zodra het pakket vastleggen gestopt door de waarde is bereikt.
- **Tijdslimiet (seconden)** -bepaalt de tijdslimiet voor de pakketopname te stoppen. Standaard is 18000 seconden.

> [!NOTE]
> Premium storage-accounts worden momenteel niet ondersteund voor opslag van pakket worden vastgelegd.

**Filteren (optioneel)**

- **Protocol** -het protocol voor het filteren van voor het vastleggen van het pakket. De beschikbare waarden zijn TCP, UDP en alle.
- **Lokaal IP-adres** -deze waarde filtert de pakketopname voor de pakketten waar het lokale IP-adres overeenkomt met de filterwaarde van dit.
- **Lokale poort** -deze waarde filtert de pakketopname om pakketten van de lokale poort overeenkomt met de filterwaarde van dit.
- **Extern IP-adres** -deze waarde filtert de pakketopname voor de pakketten waar het externe IP-adres overeenkomt met de filterwaarde van dit.
- **Externe poort** -deze waarde filtert de pakketopname pakketten waarbij de externe poort overeenkomt met de waarde voor dit filter.

> [!NOTE]
> Poort en IP-adres-waarden mag geen enkele waarde, bereik van waarden of een set. (dat wil zeggen, 80-1024 voor poort) U kunt filters die u wilt definiëren.

Nadat u de waarden zijn ingevuld, klikt u op **OK** de pakketopname maken.

![maken van een pakketopname][2]

Nadat de tijdslimiet ingesteld voor het vastleggen van het pakket is verlopen, wordt de vastlegging van het pakket wordt gestopt en kan worden gecontroleerd. U kunt ook handmatig de opnamen pakket sessies stoppen.

## <a name="delete-a-packet-capture"></a>Het vastleggen van een pakket verwijderen

Klik in de weergave van de capture pakket op de **contextmenu** (...) of klik met de rechtermuisknop en klikt u op **verwijderen** de pakketopname stoppen

![Het vastleggen van een pakket verwijderen][3]

> [!NOTE]
> Als u het vastleggen van een pakket verwijdert, verwijdert niet het bestand in het opslagaccount.

U wordt gevraagd om te bevestigen dat u wilt de pakketopname verwijderen, klikt u op **Ja**

![Bevestiging][4]

## <a name="stop-a-packet-capture"></a>Stop de pakketopname van een

Klik in de weergave van de capture pakket op de **contextmenu** (...) of klik met de rechtermuisknop en klikt u op **stoppen** de pakketopname stoppen

## <a name="download-a-packet-capture"></a>Een pakketopname downloaden

Zodra de sessie van uw pakket vastleggen is voltooid, wordt de capture-bestand wordt geüpload naar blob-opslag of naar een lokaal bestand op de virtuele machine. De opslaglocatie van de pakketopname is gedefinieerd bij het maken van de sessie. Een handig hulpmiddel voor toegang tot deze capture-bestanden opgeslagen in een opslagaccount is Microsoft Azure Storage Explorer, die u kunt hier downloaden: http://storageexplorer.com/

Als een opslagaccount is opgegeven, worden bestanden voor pakket worden opgeslagen in een opslagaccount op de volgende locatie:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakket opnamen met waarschuwingen van de virtuele machine met weer te geven [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Als bepaalde verkeer is toegestaan in of buiten uw virtuele machine in via vinden [controleren IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













