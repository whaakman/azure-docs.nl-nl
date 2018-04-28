---
title: Een eindpunt van de server synchroniseren met Azure-bestand (preview) toevoegen of verwijderen | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: ''
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 26e4af814bad988da02d4e0cf36f17e1beec872e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="addremove-an-azure-file-sync-preview-server-endpoint"></a>Een eindpunt van de server synchroniseren met Azure-bestand (preview) toevoegen of verwijderen
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Een *servereindpunt* vertegenwoordigt een specifieke locatie op een *geregistreerde server*, zoals een map op een volume van de server of in de hoofdmap van het volume. Meerdere server-eindpunten kunnen bevinden zich op hetzelfde volume als hun naamruimten (bijvoorbeeld F:\sync1 en F:\sync2) niet overlappen. U kunt de lagen beleid cloud afzonderlijk voor elk servereindpunt kunt configureren. Als u de locatie van een server met een bestaande set van bestanden als een servereindpunt aan een groep voor synchronisatie toevoegt, worden deze bestanden worden samengevoegd met andere bestanden al op de andere eindpunten in de groep voor synchronisatie.

Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure bestand Sync end-to-end.

## <a name="prerequisites"></a>Vereisten
Een om servereindpunt te maken, moet u eerst controleren of de volgende criteria wordt voldaan: 
- De server de Azure-bestand Sync-agent is geïnstalleerd en is geregistreerd. Instructies voor het installeren van de Azure-bestand Sync-Agent kunnen worden gevonden in de [Register/unregister een server met het synchroniseren van Azure-bestand (preview)](storage-sync-files-server-registration.md) artikel. 
- Zorg ervoor dat een opslag-Sync-Service is geïmplementeerd. Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor meer informatie over het implementeren van een opslag-Sync-Service. 
- Zorg ervoor dat een groep voor synchronisatie is geïmplementeerd. Meer informatie over hoe [maken van een groep voor synchronisatie](storage-sync-files-deployment-guide.md#create-a-sync-group).
- Zorg ervoor dat de server is verbonden met internet en of Azure toegankelijk is. We gebruiken poort 443 voor alle communicatie tussen de server en onze service.

## <a name="add-a-server-endpoint"></a>Het servereindpunt van een toevoegen
Als u wilt een servereindpunt toevoegt, gaat u naar de gewenste sync-groep en selecteer 'servereindpunt toevoegen'.

![Een nieuw servereindpunt in het deelvenster van de groep sync toevoegen](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

De volgende informatie is vereist onder **servereindpunt toevoegen**:

- **Geregistreerde server**: de naam van de server of het cluster te maken van het servereindpunt op.
- **Pad**: het pad op de Windows Server kunnen worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: een switch of cloud tiering uit te schakelen zodat zelden gebruikt of toegang tot bestanden naar op Azure-bestanden in tiers worden verdeeld.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume dat het servereindpunt zich bevindt. Bijvoorbeeld, als de vrije ruimte op volume is ingesteld op 50% op een volume met een eindpunt met één server, ongeveer de helft van de hoeveelheid gegevens wordt tiers worden verdeeld aan Azure-bestanden. Ongeacht of cloud tiering is ingeschakeld, worden uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer **maken** om toe te voegen van de server het eindpunt. De bestanden in een naamruimte van een groep voor synchronisatie worden, nu synchroon bewaard. 

## <a name="remove-a-server-endpoint"></a>Een servereindpunt verwijderen
Wanneer ingeschakeld voor een servereindpunt, cloud lagen wordt *laag* bestanden naar uw Azure-bestandsshares. Hierdoor kunnen lokale bestandsshares om te fungeren als een cache in plaats van een volledige kopie van de gegevensset om efficiënt gebruik van de ruimte op de bestandsserver te maken. Echter, **als een servereindpunt wordt verwijderd met gelaagde bestanden nog steeds lokaal op de server, die bestanden ontoegankelijk**. Als u nog steeds toegang tot het bestand is vereist op de lokale bestandsshares, moet u alle bestanden van gelaagde van Azure-bestanden daarom intrekken voordat u doorgaat met het verwijderen van de server het eindpunt. 

Dit kunt doen met de PowerShell-cmdlet zoals hieronder wordt weergegeven:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Als het lokale volume waarop de server niet voldoende vrije ruimte heeft voor het intrekken van de gelaagde gegevens, de `Invoke-StorageSyncFileRecall` cmdlet mislukt.  

Het servereindpunt verwijderen:

1. Navigeer naar de opslag-synchronisatieservice waar uw server is geregistreerd.
2. Navigeer naar de gewenste sync-groep.
3. Verwijder de server-eindpunt dat u wenst in de groep voor synchronisatie in de Storage-Sync-Service. Dit kunt doen met de rechtermuisknop op de relevante server-eindpunt in het deelvenster van de groep synchronisatie.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Volgende stappen
- [Een server met het synchroniseren van Azure-bestand (preview) register/unregister](storage-sync-files-server-registration.md)
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)
