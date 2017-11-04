---
title: Een Azure-synchronisatie van het bestand (preview) eindpunt van de Server toevoegen of verwijderen | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: 
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
ms.openlocfilehash: 92ac80953623a5a94d3104f30787c9636308c707
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="addremove-an-azure-file-sync-preview-server-endpoint"></a>Een Azure-synchronisatie van het bestand (preview) eindpunt van de Server toevoegen of verwijderen
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Een *servereindpunt* vertegenwoordigt een specifieke locatie op een *Server geregistreerd*, zoals een map op een volume van de server of in de hoofdmap van het volume. Meerdere Server-eindpunten kunnen bevinden zich op hetzelfde volume als hun naamruimten (bijvoorbeeld F:\sync1 en F:\sync2) niet overlappen. U kunt lagen beleidsregels cloud afzonderlijk configureren voor elk eindpunt van de Server. Als u de locatie van een server met een bestaande set van bestanden als een eindpunt van de Server aan een groep voor synchronisatie toevoegt, worden deze bestanden worden samengevoegd met andere bestanden al op de andere eindpunten in de groep voor synchronisatie.

Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure bestand Sync end-to-end.

## <a name="prerequisites"></a>Vereisten
Een Server om eindpunt te maken, moet u eerst controleren of de volgende criteria wordt voldaan: 
- De server de Azure-bestand Sync-agent is geïnstalleerd en is geregistreerd. Instructies voor het installeren van de Azure-bestand Sync-Agent kunnen worden gevonden in de [Register/unregister een server met het synchroniseren van Azure-bestand (preview)](storage-sync-files-server-registration.md) artikel. 
- Zorg ervoor dat een opslag-Sync-Service is geïmplementeerd. Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor meer informatie over het implementeren van een opslag-Sync-Service. 
- Zorg ervoor dat een groep voor synchronisatie is geïmplementeerd. Meer informatie over hoe [maken van een groep voor synchronisatie](storage-sync-files-deployment-guide.md#create-a-sync-group).
- Zorg ervoor dat de server is verbonden met internet en of Azure toegankelijk is.

## <a name="add-a-server-endpoint"></a>Het servereindpunt van een toevoegen
Als u wilt een eindpunt van de Server toevoegen, gaat u naar de gewenste groep voor synchronisatie en selecteer 'servereindpunt toevoegen'.

![Een nieuwe Server-eindpunt toevoegen in het deelvenster van de groep voor synchronisatie](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

De volgende informatie is vereist onder **servereindpunt toevoegen**:

- **Server geregistreerd**: de naam van de server of het cluster te maken van het eindpunt van de Server op.
- **Pad**: het pad op de Windows Server kunnen worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: een switch of cloud tiering uit te schakelen zodat zelden gebruikt of toegang tot bestanden naar op Azure-bestanden in tiers worden verdeeld.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume dat het eindpunt van de Server zich bevindt. Bijvoorbeeld, als de vrije ruimte op volume is ingesteld op 50% op een volume met een eindpunt met één Server, ongeveer de helft van de hoeveelheid gegevens wordt tiers worden verdeeld aan Azure-bestanden. Ongeacht of cloud tiering is ingeschakeld, worden uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer **maken** om toe te voegen van de Server het eindpunt. De bestanden in een naamruimte van een groep voor synchronisatie worden, nu synchroon bewaard. 

## <a name="remove-a-server-endpoint"></a>Een servereindpunt verwijderen
Wanneer ingeschakeld voor een eindpunt van de Server, cloud lagen wordt *laag* bestanden naar uw Azure-bestandsshares. Hierdoor kunnen lokale bestandsshares om te fungeren als een cache in plaats van een volledige kopie van de gegevensset om efficiënt gebruik van de ruimte op de bestandsserver te maken. Echter, als een eindpunt van de Server wordt verwijderd met gelaagde bestanden nog steeds lokaal op de server, die bestanden worden ontoegankelijk. Als u nog steeds toegang tot het bestand gewenst is, moet u alle bestanden van gelaagde van Azure Files daarom voordat u doorgaat met de uitschrijving intrekken. 

Dit kunt doen met de PowerShell-cmdlet zoals hieronder wordt weergegeven:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Als het lokale volume waarop de server niet voldoende vrije ruimte heeft voor het intrekken van de gelaagde gegevens, de `Invoke-StorageSyncFileRecall` cmdlet mislukt.  

Het eindpunt van de Server verwijderen:

1. Navigeer naar de opslag-synchronisatieservice waar uw server is geregistreerd.
2. Navigeer naar de gewenste groep voor synchronisatie.
3. Verwijder de Server-eindpunt dat u wenst in de groep voor synchronisatie in de Storage-Sync-Service. Dit kunt doen met de rechtermuisknop op de relevante Server-eindpunt in het deelvenster van de groep voor synchronisatie.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Volgende stappen
- [Een server met het synchroniseren van Azure-bestand (preview) register/unregister](storage-sync-files-server-registration.md)
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)