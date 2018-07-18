---
title: Pakketopname beheren met Azure Network Watcher - Azure portal | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van de functie voor het vastleggen van pakketten van Network Watcher met behulp van Azure portal
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 18e5f8eda51f8834f6346eef3d7ad31bc556290a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090194"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Pakketopname beheren met Azure Network Watcher met behulp van de portal

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure-CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Network Watcher packet-capture kunt u capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt bij het opsporen van afwijkingen netwerk reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen. Door vereenvoudigt deze mogelijkheid voor het extern activeren pakketopnamen, de belasting van het uitvoeren van een pakketopname handmatig en op de gewenste computer, wat kostbare tijd bespaart.

In dit artikel doorloopt u de verschillende beheertaken die momenteel beschikbaar voor de pakketopname zijn.

- [**Een pakketopname starten**](#start-a-packet-capture)
- [**Een pakketopname stoppen**](#stop-a-packet-capture)
- [**Een pakketopname verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u de volgende bronnen hebt:

- Een exemplaar van Network Watcher in de regio die u wilt maken van een pakketopname
- Een virtuele machine met de packet capture-uitbreiding is ingeschakeld.

> [!IMPORTANT]
> Pakketopname is vereist voor een VM-extensie `AzureNetworkWatcherExtension`. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Packet Capture-agentextensie via de portal

Voor het installeren van de packet capture VM-agent via de portal, gaat u naar uw virtuele machine, klikt u op **extensies** > **toevoegen** en zoek naar de **Network Watcher-Agent voor Windows**

![Agent weergeven][agent]

## <a name="packet-capture-overview"></a>Overzicht van pakketopname

Navigeer naar de [Azure-portal](https://portal.azure.com) en klikt u op **netwerken** > **Network Watcher** > **vastleggen van pakketten**

De overzichtspagina ziet u een lijst met alle pakketten die aanwezig zijn, ongeacht de status vastlegt.

> [!NOTE]
> Pakketopname is vereist na de connectiviteit.
> * Uitgaande connectiviteit met de storage-account via poort 443.
> * Binnenkomende en uitgaande connectiviteit met 169.254.169.254
> * Binnenkomende en uitgaande connectiviteit met 168.63.129.16

![scherm Packet capture-overzicht][1]

## <a name="start-a-packet-capture"></a>Een pakketopname starten

Klik op **toevoegen** te maken van een pakketopname.

De eigenschappen die kunnen worden gedefinieerd op een pakketopname zijn:

**Belangrijkste instellingen**

- **Abonnement** -deze waarde is het abonnement dat wordt gebruikt, wordt een exemplaar van network watcher is vereist in elk abonnement.
- **Resourcegroep** -de resourcegroep van de virtuele machine die wordt benaderd.
- **Doel van de virtuele machine** -de virtuele machine die wordt uitgevoerd de pakketopname
- **Naam van pakketopname** -deze waarde is de naam van de pakketopname.

**Configuratie vastleggen**

- **Lokaal bestandspad** -lokaal pad op de virtuele machine waar de pakketopname is opgeslagen (alleen geldig als **[bestand]** is geselecteerd). U moet een geldig pad opgeven. Als u van een virtuele Linux-machine gebruikmaakt, het pad moet beginnen met / var / worden vastgelegd.
- **Storage-Account** -bepaalt als pakketopname is opgeslagen in een storage-account.
- **Bestand** -bepaalt als een pakketopname lokaal wordt opgeslagen op de virtuele machine.
- **Storage-Accounts** : de storage-account om op te slaan van de pakketopname in geselecteerd. Standaardlocatie is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription-id} /resourcegroups/ {name}/providers/microsoft.compute/virtualmachines/{virtual machine groepsnaam voor accountresources} / {jj} / {MM} / {yyyy} / {HH} packetcapture__{MM}_{SS} _ {XXX} .cap. (Alleen ingeschakeld als **opslag** is geselecteerd)
- **Lokaal bestandspad** -het lokale pad op een virtuele machine op te slaan van de pakketopname. (Alleen ingeschakeld als **bestand** is geselecteerd). Een geldig pad moet worden opgegeven. Voor een virtuele Linux-machine, het pad moet beginnen met */var/vastgelegd*.
- **Maximum aantal bytes per pakket** : het aantal bytes van elk pakket die zijn vastgelegd, alle bytes zijn vastgelegd als dit veld leeg blijft.
- **Maximum aantal bytes per sessie** : het totale aantal bytes dat wordt vastgelegd, zodra het pakket vastleggen gestopt door de waarde is bereikt.
- **Tijdslimiet (seconden)** -Hiermee stelt u een tijdslimiet voor de pakketopname stoppen. De standaardwaarde is 18000 seconden.

> [!NOTE]
> Premium storage-accounts worden momenteel niet ondersteund voor het opslaan van pakket vastgelegd.

**Filteren (optioneel)**

- **Protocol** -het-protocol om te filteren op de pakketopname. De beschikbare waarden zijn TCP, UDP en alle.
- **Lokaal IP-adres** -deze waarde filtert de pakketopname pakketten waarvan het lokale IP-adres overeenkomt met de waarde voor dit filter.
- **Lokale poort** -deze waarde filtert de pakketopname pakketten waar de lokale poort komt overeen met de waarde voor dit filter.
- **Extern IP-adres** -deze waarde filtert de pakketopname pakketten waarvan het externe IP-adres overeenkomt met de waarde voor dit filter.
- **Externe poort** -deze waarde filtert de pakketopname pakketten waar de externe poort komt overeen met de waarde voor dit filter.

> [!NOTE]
> Poort- en IP-adres-waarden mag bestaan uit één waarde, bereik van waarden of een set. (dat wil zeggen, 80-1024 voor poort) U kunt zoveel filters als u wilt definiëren.

Nadat u de waarden zijn ingevuld, klikt u op **OK** te maken van de pakketopname.

![een pakketopname maken][2]

Nadat de tijdslimiet ingesteld voor de pakketopname is verlopen, kan de pakketopname stopt en kan worden gecontroleerd. U kunt ook handmatig de sessies van de opnamen pakket stoppen.

## <a name="delete-a-packet-capture"></a>Een pakketopname verwijderen

Klik in de weergave van het vastleggen van pakketten op de **contextmenu** (...) of klik met de rechtermuisknop en klikt u op **verwijderen** de pakketopname stoppen

![Een pakketopname verwijderen][3]

> [!NOTE]
> Een pakketopname verwijderen, wordt het bestand in het opslagaccount niet verwijderen.

U wordt gevraagd te bevestigen dat u wilt de pakketopname verwijderen, klikt u op **Ja**

![Bevestiging][4]

## <a name="stop-a-packet-capture"></a>Een pakketopname stoppen

Klik in de weergave van het vastleggen van pakketten op de **contextmenu** (...) of klik met de rechtermuisknop en klikt u op **stoppen** de pakketopname stoppen

## <a name="download-a-packet-capture"></a>Een pakketopname downloaden

Zodra uw pakket opnamesessie is voltooid, wordt de capture-bestand wordt geüpload naar blob-opslag of naar een lokaal bestand op de virtuele machine. De opslaglocatie van de pakketopname is gedefinieerd tijdens het maken van de sessie. Een handig hulpmiddel voor toegang tot deze bestanden opgeslagen in een storage-account is Microsoft Azure Storage Explorer, die hier kan worden gedownload:  http://storageexplorer.com/

Als een storage-account is opgegeven, worden pakketten vastleggen van bestanden worden opgeslagen in een storage-account op de volgende locatie:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakketopnamen met waarschuwingen van de virtuele machine via [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Zoeken of bepaalde verkeer is toegestaan in of buiten uw virtuele machine door naar de pagina [controleren IP-stroom controleren](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













