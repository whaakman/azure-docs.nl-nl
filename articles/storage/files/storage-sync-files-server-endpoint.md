---
title: Een Azure File Sync-servereindpunt toevoegen of verwijderen | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van een implementatie van Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 00ce83a7ec52e24ce858c720e834b8396c2f808c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523962"
---
# <a name="addremove-an-azure-file-sync-server-endpoint"></a>Een Azure File Sync-servereindpunt toevoegen/verwijderen
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door het transformeren van uw Windows-Servers in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Een *servereindpunt* staat voor een specifieke locatie op een *geregistreerde server*, zoals een map op een volume van de server of in de hoofdmap van het volume. Meerdere servereindpunten kunnen bevinden zich op hetzelfde volume als de naamruimten niet (bijvoorbeeld F:\sync1 en F:\sync2 overlappen). U kunt cloud cloudlagen beleidsregels voor het eindpunt voor elke server afzonderlijk configureren. Als u de locatie van een server met een bestaande set van bestanden als een servereindpunt aan een groep voor synchronisatie toevoegen, zullen deze bestanden worden samengevoegd met andere bestanden al op de andere eindpunten in de groep voor synchronisatie.

Zie [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure File Sync end-to-end.

## <a name="prerequisites"></a>Vereisten
Voor het maken van een servereindpunt, moet u eerst ervoor zorgen dat de volgende criteria wordt voldaan: 
- De server heeft de Azure File Sync-agent is geïnstalleerd en is geregistreerd. Instructies voor het installeren van de Azure File Sync-Agent kunnen worden gevonden de [registreren/registratie van een server met Azure File Sync](storage-sync-files-server-registration.md) artikel. 
- Zorg ervoor dat een Opslagsynchronisatieservice is geïmplementeerd. Zie [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md) voor meer informatie over het implementeren van een Opslagsynchronisatieservice. 
- Zorg ervoor dat een synchronisatiegroep is geïmplementeerd. Meer informatie over het [maken van een synchronisatiegroep](storage-sync-files-deployment-guide.md#create-a sync-group-and-a-cloud-endpoint).
- Zorg ervoor dat de server is verbonden met internet en dat Azure toegankelijk is. We gebruiken poort 443 voor alle communicatie tussen de server en onze service.

## <a name="add-a-server-endpoint"></a>Een servereindpunt toevoegen
Een servereindpunt toevoegen, gaat u naar de gewenste synchronisatiegroep, en selecteer 'Servereindpunt toevoegen'.

![Een nieuw servereindpunt toevoegen in het deelvenster van de groep synchroniseren](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

De volgende informatie is vereist onder **servereindpunt toevoegen**:

- **Geregistreerde server**: de naam van de server of het cluster te maken van het servereindpunt op.
- **Pad**: het pad op de Windows-Server moeten worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: overschakelen naar de in- of uitschakelen van cloud tiering. Wanneer dit is ingeschakeld, cloud trapsgewijs worden *laag* bestanden naar uw Azure-bestandsshares. Dit converteert on-premises bestandsshares naar een cache in plaats van een volledige kopie van de gegevensset, om u te helpen bij het beheren van de ruimte-efficiëntie op de server.
- **Beschikbare volumeruimte**: de hoeveelheid vrije ruimte te reserveren op het volume dat het servereindpunt zich bevindt. Bijvoorbeeld, als de vrije ruimte op volume is ingesteld op 50% op een volume met een eindpunt met één server, ongeveer de helft van de hoeveelheid gegevens wordt in tiers worden verdeeld naar Azure Files. Ongeacht of cloud tiering is ingeschakeld, wordt uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer **maken** het servereindpunt toevoegen. De bestanden in een naamruimte uit een groep voor synchronisatie wordt nu synchroon worden gehouden. 

## <a name="remove-a-server-endpoint"></a>Een servereindpunt verwijderen
Als u wenst op te zetten met behulp van Azure File Sync voor het eindpunt van een bepaalde server, kunt u het servereindpunt verwijderen. 

> [!Warning]  
> Probeer niet aan het oplossen van problemen met synchronisatie, cloud-opslaglagen of een andere aspect van Azure File Sync door te verwijderen en opnieuw maken van het servereindpunt tenzij expliciet naar een Microsoft-technicus. Verwijderen van een servereindpunt is een destructieve bewerking en gelaagde bestanden in het servereindpunt wordt niet opnieuw worden 'verbonden' op de locaties van de Azure-bestandsshare nadat het servereindpunt opnieuw gemaakt wordt, waardoor synchroon fouten. Let ook op, gelaagde bestanden die buiten de server eindpunt-naamruimte bestaat mogelijk permanent verloren. Gelaagde bestanden kunnen bestaan binnen uw server, zelfs als eindpunt cloud tiering nooit is ingeschakeld.

Om ervoor te zorgen dat alle gelaagde bestanden zijn ingetrokken voordat u het servereindpunt verwijdert, cloud-opslaglagen op het servereindpunt uitschakelen en vervolgens de volgende PowerShell-cmdlet om in te trekken van alle gelaagde bestanden in de naamruimte van uw server-eindpunt wordt uitgevoerd:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Note]  
> Als het lokale volume waarop de server niet voldoende vrije ruimte heeft voor het intrekken van alle gelaagde gegevens, de `Invoke-StorageSyncFileRecall` cmdlet mislukt.  

Het servereindpunt verwijdert:

1. Navigeer naar de Opslagsynchronisatieservice waar de server is geregistreerd.
2. Navigeer naar de gewenste synchronisatiegroep.
3. Het servereindpunt die u wilt verwijderen in de groep voor synchronisatie in de Opslagsynchronisatieservice. Dit kan worden bereikt met de rechtermuisknop op het eindpunt van de relevante server in het deelvenster van de groep synchroniseren.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Volgende stappen
- [Een server met Azure File Sync registreren/registratie](storage-sync-files-server-registration.md)
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)