---
title: Gegevens migreren van StorSimple 5000-7000 serie naar Azure File Sync | Microsoft Docs
description: Beschrijft hoe u gegevens migreren van de StorSimple 5000/7000-serie voor Azure File Sync (AFS).
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2018
ms.author: alkohli
ms.openlocfilehash: d54394a8dc74d04f1922a78826b10a4a421d193b
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42814587"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-azure-file-sync"></a>Gegevens van de StorSimple 5000-7000-serie migreren naar Azure File Sync

> [!IMPORTANT]
> Op 31 juli 2019 bereiken de StorSimple 5000/7000-serie het einde van de ondersteuningstatus (EOS). U wordt aangeraden dat de StorSimple 5000/7000-serie klanten naar een van de alternatieven zoals beschreven in het document migreren.

Migratie van gegevens is het proces van het verplaatsen van gegevens van de ene opslaglocatie naar de andere. Dit omvat een exacte kopie van de huidige gegevens van een organisatie van het ene apparaat naar een ander apparaat te maken, bij voorkeur zonder onderbreken of uitschakelen van actieve toepassingen, en vervolgens alle invoer/uitvoer (I/O) activiteit wordt omgeleid naar het nieuwe apparaat. 

StorSimple 5000- en 7000-serie opslagapparaten bereiken het einde van de service in juli 2019. Dit betekent dat Microsoft wordt niet langer ter ondersteuning van de hardware en software voor de StorSimple 5000/7000-serie na juli 2019. Klanten die gebruik van deze apparaten maken moeten hun StorSimple-gegevens migreren naar andere hybride-opslagoplossingen op Azure. In dit artikel bevat informatie over de migratie van gegevens van een apparaat uit de StorSimple 5000/7000-serie voor Azure File Sync (AFS).

## <a name="intended-audience"></a>Doelgroep

In dit artikel is bedoeld voor IT-professionals (IT) en informatiewerkers die verantwoordelijk is voor het implementeren en beheren van apparaten van de StorSimple 5000/7000-serie in het datacenter. De klanten met behulp van de StorSimple-apparaten voor server-workloads van het bestand (met Windows Server) kunnen dit migratiepad met name aantrekkelijke vinden. Als u van mening dat zijn de functies van Azure File Sync-werkt goed voor uw organisatie, wordt in dit artikel krijgt u inzicht in hoe u deze oplossingen van StorSimple.

## <a name="migration-considerations"></a>Overwegingen bij migraties

Dit proces werkt voor klanten die een Windows-bestandsshare met behulp van een StorSimple-volume voor de opslag hebt geconfigureerd. Gegevens uit de StorSimple 5000/7000 migreren naar Azure File Sync omvat het converteren van die locatie van de bestandsshare aan een [servereindpunt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) en klik vervolgens met behulp van lokaal stations gekoppeld als een ander eindpunt dit vervolgens de nieuwe locatie wordt. 

Tijdens het verplaatsen van AFS, de volgende punten overwegen:

1. Azure Files is momenteel een beperking van 5 TB/delen. Deze beperking, met behulp van Azure File Sync met gegevens die zijn verdeeld over meerdere Azure-bestandsshares kan worden ondervangen. Raadpleeg voor meer informatie de [gegevenspatroon groei van de implementatie van Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-planning).
2. Deze migratie wordt de volledige set met primaire gegevens gedownload naar een on-premises apparaat, zoals het kopiëren van gegevens wordt gedaan via de on-premises-apparaat. Zorg ervoor dat er voldoende bandbreedte voor deze overdracht.
3. Dit proces behoudt de momentopnamen die al zijn gemaakt niet. Deze alleen de primaire gegevens gemigreerd. Het proces ook behouden niet de bijbehorende bandbreedtesjablonen of back-upbeleid. [Gebruik Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-files) voor het instellen van de back-upbeleid nadat de gegevens worden gemigreerd op het Azure-bestand delen.
4. StorSimple biedt eerste hardware van derden. Met Azure bestanden/Azure-bestandssynchronisatie u echter uw eigen lokale Windows-serverhardware gebruiken als de lokale cache. U moet ervoor zorgen dat er voldoende opslagcapaciteit aan de set gegevens van uw keuze lokaal te houden. Raadpleeg voor meer informatie over opslaglagen en doel-instelling vereiste vrije ruimte het [een servereindpunt maken bij het implementeren van Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal). 
5. Controleer de [prijzen voor Azure File Sync](https://azure.microsoft.com/pricing/details/storage/files/) zoals dit van StorSimple varieert. AFS beschikt niet over deduplicatie en compressie, zoals StorSimple.

## <a name="migration-prerequisites"></a>Vereisten voor migratie

Hier vindt u de vereisten voor uw verouderde 5000 of 7000 serie apparaat van de migratie naar Azure File Sync. Voordat u begint, zorg ervoor dat u hebt:

- Toegang tot een StorSimple 5000/7000 serie apparaat actief software-versie v2.1.1.518 of later. Eerdere versies worden niet ondersteund. De versie van de software die wordt uitgevoerd moet worden weergegeven in de rechterbovenhoek van de webgebruikersinterface van uw StorSimple-apparaat.  
    Als uw apparaat wordt niet uitgevoerd voor v2.1.1.518, zet u uw systeem aan de vereiste minimale versie. Raadpleeg voor gedetailleerde instructies [upgraden van uw systeem naar v2.1.1.518](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
- Controleer voor alle actieve back-uptaken die worden uitgevoerd op het bronapparaat. Dit omvat de taken op de host van de Console van het StorSimple-bescherming van gegevens. Wachten op de huidige taken om te voltooien. 
- Toegang tot een Windows Server-host is verbonden met uw StorSimple 5000-7000 serie apparaat. De host moet een ondersteunde versie van Windows Server worden uitgevoerd zoals beschreven in [Azure File Sync-interoperabiliteit](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning).
- StorSimple-volumes op de host zijn gekoppeld en bestandsshares bevatten.
- De host heeft onvoldoende lokale opslag om uw lokaal in cache opgeslagen gegevens te bevatten.
- De eigenaar van niveau toegang tot de Azure-abonnement dat u gebruiken wilt voor Azure File Sync implementeren. U ondervindt mogelijk problemen bij het maken van een cloudeindpunt om daar de synchronisatiegroep als u geen eigenaar of beheerdersmachtigingen.
- Toegang tot een [algemeen gebruik v2-opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-account-options) met een Azure-bestandsshare die u wilt synchroniseren. Ga voor meer informatie naar 
 - Hoe u [maken van een opslagaccount voor algemeen gebruik v2](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal#create-a-general-purpose-storage-account).
 - Hoe u [maken van een Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share#create-file-share-through-the-azure-portal).

## <a name="migration-process"></a>Migratieproces

Migreren van gegevens van de StorSimple is 5000-7000 naar AFS een proces in twee stappen:
1.  Het repliceren van de gegevens van de lokale bestandsserver waar de StorSimple-volumes zijn gekoppeld aan een Azure-bestanden delen.  Replicatie wordt uitgevoerd via een AFS-agent die u installeert.
2.  Het StorSimple-apparaat loskoppelt. De lokale schijven fungeert als de lokale cache.

### <a name="migration-steps"></a>Migratiestappen

Voer de volgende stappen uit voor het migreren van de Windows-bestandsshare die is geconfigureerd op het StorSimple-volumes naar een Azure File Sync-share. 
1.  Deze stappen uitvoeren op de dezelfde Windows Server-host waar de StorSimple-volumes zijn gekoppeld of gebruik een ander systeem. 
    - [Voorbereiden van Windows Server voor gebruik met Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#prepare-windows-server-to-use-with-azure-file-sync).
    - [Installeer de Azure File Sync-agent](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#install-the-azure-file-sync-agent).
    - [De Opslagsynchronisatieservice-service implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#deploy-the-storage-sync-service). 
    - [Windows-Server registreren met opslag-Sync-service](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#register-windows-server-with-storage-sync-service). 
    - [Maak een groep voor synchronisatie en een cloudeindpunt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#create-a-sync-group-and-a-cloud-endpoint). Synchronisatiegroepen moeten worden gemaakt voor elke Windows-bestandsshare die moet worden gemigreerd van de host.
    - [Maken van een servereindpunt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal#create-a-server-endpoint). Geef het pad op als het pad van het StorSimple-volume met de gegevens van uw bestand delen. Bijvoorbeeld, als het StorSimple-volume station `J`, en uw gegevens zich bevinden `J:/<myafsshare>`, klikt u vervolgens dit pad toevoegen als een servereindpunt. Laat de **Opslaglagen** als **uitgeschakelde**.
2.  Wacht totdat de synchronisatie van de server bestand voltooid is. Voor elke server in een bepaalde synchronisatiegroep, zorg ervoor dat:
    - De tijdstempels voor de laatste synchronisatie heeft geprobeerd voor zowel uploaden en downloaden zijn recente.
    - De status is groen voor zowel uploaden en downloaden.
    - De **synchronisatieactiviteiten** bevat te weinig of geen bestanden om te synchroniseren.
    - De **bestanden niet synchroniseren** is 0 voor zowel uploaden en downloaden.
    Voor meer informatie over wanneer de serversynchronisatie voltooid is, gaat u naar [problemen met Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#how-do-i-know-if-my-servers-are-in-sync-with-each-other). De synchronisatie kan enkele uren duren, afhankelijk van uw gegevensgrootte en bandbreedte, dagen. Nadat de synchronisatie is voltooid, is al uw gegevens veilig in de Azure-bestandsshare. 
3.  Ga naar de shares op de StorSimple-volumes. Selecteer een share, klik met de rechtermuisknop en selecteer **eigenschappen**. Houd er rekening mee de sharemachtigingen onder **Security**. Deze machtigingen moet handmatig worden toegepast op de nieuwe share in de volgende stap.
4.  Afhankelijk van of u de dezelfde Windows Server-host of een ander account gebruiken, zijn de volgende stappen verschillend.

    Sla deze stap over en gaat u naar de volgende stap als u een andere Windows Server-host. Als u de dezelfde Windows-bestandsserver voor AFS, wordt u nu een paar minuten uitvaltijd ondervindt. 
    - **Downtime Start** -verwijderen van het eindpunt van de server die u hebt gemaakt in *1F stap*. 
    - Maak een nieuw servereindpunt met het pad waar u de gegevens zich bevinden vooruit gaan.
    - Wanneer het servereindpunt wordt weergegeven als in orde (dit kan enkele minuten duren), ziet u de gegevens in deze nieuwe locatie. U kunt nu uw Windows Server-host om van dienst van bestanden van dit nieuwe locatie te configureren. - **Downtime eindigt**.
5.  Als u een andere Windows-bestandsserver voor Azure File Sync gebruikt, wordt u niet uitvaltijd ondervindt. 
    - Een andere servereindpunt met het pad van de lokale opslag die u bereid bent te gebruiken als een cache in plaats van het StorSimple-apparaat toevoegen. 
    - U kunt zich de bestanden in de nieuwe server te zien in enkele minuten. U bent om de overschakeling van uw StorSimple-apparaat naar deze nieuwe locatie op de host op elk gewenst moment.

    > [!TIP] 
    > Overweeg de configuratie van deze nieuwe bestandsshare met dezelfde naam en hetzelfde pad als één it is vervangen, om te minimaliseren wordt onderbroken. Als u van DFS-N, mogelijk u wijzigingen kunt aanbrengen in de configuratie.
6.  Configureren van de machtigingen voor het delen als zijn aangegeven in *stap 3*.

Als u problemen ondervindt tijdens de gegevensmigratie, kunt u [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md). 



## <a name="next-steps"></a>Volgende stappen

Als AFS niet de juiste oplossing voor u is, krijgt u informatie over het [gegevens migreren van een StorSimple 5000-7000 serie naar een apparaat 8000-serie](storsimple-8000-migrate-from-5000-7000.md).

