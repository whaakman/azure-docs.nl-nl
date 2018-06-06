---
title: Een eindpunt van de server synchroniseren met Azure-bestand (preview) toevoegen of verwijderen | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: 93331dd936a6d7b30ca18743d2079900421b2620
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738476"
---
# <a name="addremove-an-azure-file-sync-preview-server-endpoint"></a>Een eindpunt van de server synchroniseren met Azure-bestand (preview) toevoegen of verwijderen
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door het omzetten van de Windows-Servers in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

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
- **Cloud Tiering**: overschakelen naar de in- of uitschakelen, cloud tiering. Wanneer dit is ingeschakeld, cloud lagen wordt *laag* bestanden naar uw Azure-bestandsshares. Dit converteert lokale bestandsshares naar een cache in plaats van een volledige kopie van de gegevensset om u te helpen bij het beheren van de efficiëntie van de ruimte op de server.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume dat het servereindpunt zich bevindt. Bijvoorbeeld, als de vrije ruimte op volume is ingesteld op 50% op een volume met een eindpunt met één server, ongeveer de helft van de hoeveelheid gegevens wordt tiers worden verdeeld aan Azure-bestanden. Ongeacht of cloud tiering is ingeschakeld, worden uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer **maken** om toe te voegen van de server het eindpunt. De bestanden in een naamruimte van een groep voor synchronisatie worden, nu synchroon bewaard. 

## <a name="remove-a-server-endpoint"></a>Een servereindpunt verwijderen
Als u wenst te zetten met behulp van Azure File-synchronisatie voor het eindpunt van een bepaalde server, kunt u het servereindpunt verwijderen. 

> [!Warning]  
> Probeer niet aan het oplossen van problemen met synchronisatie, cloud tiering of een andere aspect van Azure bestand Sync door te verwijderen en opnieuw maken van het servereindpunt, tenzij expliciet naar een engineer van Microsoft. Verwijderen van een servereindpunt is een destructieve bewerking en gelaagde bestanden in de server het eindpunt wordt niet opnieuw worden 'verbonden' naar de locatie op de Azure-bestandsshare nadat het servereindpunt opnieuw gemaakt wordt, waardoor de synchroon fouten. Ook merk gelaagde bestanden die aanwezig zijn buiten de server eindpunt naamruimte mogelijk permanent verloren. Gelaagde bestaan mogelijk binnen uw server eindpunt zelfs als cloud tiering nooit is ingeschakeld.

Om ervoor te zorgen dat alle gelaagde bestanden zijn ingetrokken voordat u het servereindpunt verwijdert, cloud tiering op het servereindpunt uitschakelen en vervolgens de volgende PowerShell-cmdlet om in te trekken van alle gelaagde bestanden in de naamruimte van uw server eindpunt wordt uitgevoerd:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Note]  
> Als het lokale volume waarop de server niet voldoende vrije ruimte heeft voor het intrekken van de gelaagde gegevens, de `Invoke-StorageSyncFileRecall` cmdlet mislukt.  

Het servereindpunt verwijderen:

1. Navigeer naar de opslag-synchronisatieservice waar uw server is geregistreerd.
2. Navigeer naar de gewenste sync-groep.
3. Verwijder de server-eindpunt dat u wenst in de groep voor synchronisatie in de Storage-Sync-Service. Dit kunt doen met de rechtermuisknop op de relevante server-eindpunt in het deelvenster van de groep synchronisatie.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Volgende stappen
- [Een server met het synchroniseren van Azure-bestand (preview) register/unregister](storage-sync-files-server-registration.md)
- [Planning voor de implementatie van een Azure-bestand synchronisatie](storage-sync-files-planning.md)