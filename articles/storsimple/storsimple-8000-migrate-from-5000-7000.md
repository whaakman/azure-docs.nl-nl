---
title: Migreren van gegevens op StorSimple 5000 7000-serie naar 8000 series apparaat | Microsoft Docs
description: Biedt een overzicht en de vereisten van de functie voor migratie.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/11/2017
ms.author: alkohli
ms.openlocfilehash: 36df62c4b01c623702707d39c6af59f4752ee6e0
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>Gegevens migreren van StorSimple 5000 7000-serie naar 8000 series apparaat

> [!IMPORTANT]
> - Migratie wordt momenteel een bewerking voor assistentie. Als u van plan bent om gegevens te migreren van uw StorSimple-apparaat 5000 7000-serie op een apparaat 8000-serie, moet u de migratie met Microsoft Support plannen. Microsoft Support kunnen vervolgens uw abonnement voor de migratie. Zie voor meer informatie hoe [een ondersteuningsticket opent](storsimple-8000-contact-microsoft-support.md).
> - Nadat u de serviceaanvraag-bestand, duurt het enkele weken voor de migratieplan uitvoering en om de migratie daadwerkelijk wordt gestart.
> - Voordat u contact opneemt met Microsoft Support, zijn moet bekijken en voltooid de [migratie vereisten](#migration-prerequisites) aangegeven in het artikel.

## <a name="overview"></a>Overzicht

Dit artikel bevat de migratiefunctie waarmee de StorSimple 5000 7000-serie klanten voor het migreren van hun gegevens naar de StorSimple 8000 series fysiek apparaat of een apparaat 8010/8020 cloud. In dit artikel bevat ook koppelingen naar een downloadbare stapsgewijze introductie van de stappen die nodig zijn om gegevens te migreren vanaf een 5000 7000 reeks verouderde apparaat aan een fysieke 8000 reeks of cloud-apparaat.

Dit artikel is van toepassing op zowel de on-premises 8000 series-apparaat als het toestel StorSimple-Cloud.


## <a name="migration-feature-versus-host-side-migration"></a>Migratiefunctie versus host-side '-migratie

U kunt uw gegevens dankzij de migratiefunctie verplaatsen of door het uitvoeren van een host-side '-migratie. Deze sectie beschrijft de details van elke methode met inbegrip van de voor- en nadelen. Gebruik deze informatie om te achterhalen welke methode u wilt uitvoeren om uw gegevens te migreren.

De migratiefunctie simuleert een (DR) noodherstelproces van 7000/5000-reeks op 8000-serie. Deze functie kunt u de gegevens migreren van 5000/7000-serie indeling naar 8000 series indeling op Azure. Het migratieproces wordt gestart met behulp van het hulpprogramma voor migratie van StorSimple. Het hulpprogramma begint met het downloaden en de conversie van back-upmetagegevens op het apparaat van de reeks 8000 en gebruikt vervolgens de meest recente back-up om de volumes op het apparaat weer te geven.

|      | Professionals                                                                                                                                     |Nadelen                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Het migratieproces bewaart de geschiedenis van back-ups die zijn uitgevoerd op 5000/7000-serie.                                               | Wanneer gebruikers proberen toegang tot de gegevens, worden de gegevens in deze migratie gedownload van Azure waardoor steeds kosten voor het downloaden van gegevens.                                     |
| 2.   | Er zijn geen gegevens worden aan de kant van de host gemigreerd.                                                                                                     | Het proces moet uitvaltijd tussen het begin van de en recente back-ups wordt opgehaald op de 8000-serie (kan worden geschat met behulp van het hulpprogramma voor migratie). |
| 3.   | Dit proces behoudt alle beleidsregels, bandbreedte sjablonen, versleuteling en andere instellingen op apparaten 8000 serie.                      | Gebruikerstoegang verschijnt terug alleen de gegevens die worden gebruikt door de gebruikers en wordt de volledige gegevensset niet rehydrate.                                                  |
| 4.   | Dit proces vereist meer tijd om te converteren van alle de oudere back-ups in Azure die asynchroon wordt uitgevoerd zonder enige impact op productie | Migratie kan alleen worden uitgevoerd op een cloud-configuratie-niveau.  Afzonderlijke volumes in een cloud-configuratie kunnen niet afzonderlijk worden gemigreerd.                       |

Een host-side '-migratie kan onafhankelijk van 8000 serie instellen en de gegevens van 5000/7000-serie apparaat kopiëren naar de 8000 series apparaat. Dit komt overeen met gegevens uit een opslagapparaat migreren naar een andere. Tal van hulpprogramma's zoals Diskboss, robocopy worden gebruikt om de gegevens te kopiëren.

|      | Professionals                                                                                                                      |Nadelen                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Migratie kan op basis van de volume per volume op een gefaseerde manier worden gerealiseerd.                                               | Eerdere back-ups (taken op 5000/7000-serie) worden niet beschikbaar op het apparaat 8000-serie.                                                                                                       |
| 2.   | Consolidatie van gegevens in één opslagaccount in Azure maakt.                                                       | Eerste back-up naar de cloud op 8000 series duurt langer als de gegevens op 8000 series moet een back-up naar Azure.                                                                     |
| 3.   | Na een geslaagde migratie, alle gegevens die lokaal op het apparaat is. Er zijn geen latenties bij het openen van de gegevens. | Azure-opslag-verbruik wordt verhoogd tot de gegevens worden verwijderd van het apparaat 5000/7000.                                                                                                        |
| 4.   |                                                                                                                           | Als het apparaat 7000/5000-reeks een grote hoeveelheid gegevens heeft, tijdens de migratie van deze gegevens moeten worden gedownload van azure die worden, kosten en latenties gerelateerd aan het downloaden van gegevens van Azure |

In dit artikel is alleen gericht op het onderdeel van de migratie vanuit 5000/7000 naar 8000 series apparaat. Voor meer informatie over de host-side '-migratie, gaat u naar [migratie van andere opslagapparaten](http://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating Data to StorSimple Volumes_09-02-15.pdf).

## <a name="migration-prerequisites"></a>Vereisten voor migratie

Hier volgen de vereisten voor migratie voor uw oude 5000 of 7000 reeks apparaat en het apparaat van de StorSimple 8000 serie.

> [!IMPORTANT]
> Controleer en voltooi de migratie-vereisten voordat u een serviceaanvraag met Microsoft Support indienen.

### <a name="for-the-50007000-series-device-source"></a>Voor het apparaat 5000/7000-serie (bron)

Voordat u de migratie begint, zorg ervoor dat:

* U hebt uw 5000 of 7000-serie bron apparaat; het apparaat kan worden live of omlaag.

    > [!IMPORTANT]
    > Het is raadzaam dat u seriële toegang tot dit apparaat gedurende het migratieproces hebben. Moet er problemen apparaat, kan seriële toegang helpen bij het oplossen van problemen.

* Uw Bronapparaat 5000 of 7000 reeks v2.1.1.518 voor software-versie wordt uitgevoerd of hoger. Eerdere versies worden niet ondersteund.
* Kijken om te controleren of de versie die uw 5000 of 7000-serie wordt uitgevoerd, de rechterbovenhoek van uw Web-UI. Dit moet worden weergegeven de softwareversie die uw apparaat wordt uitgevoerd. Voor de migratie, moet uw 5000 of 7000-serie v2.1.1.518 worden uitgevoerd.

    ![Controleer de softwareversie op de oudere apparaten](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Als uw live-apparaat wordt niet uitgevoerd v2.1.1.518 of hoger, werk uw systeem naar de vereiste minimale versie. Raadpleeg voor gedetailleerde instructies [Upgrade van uw systeem naar v2.1.1.518](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
    * Als u v2.1.1.518 uitvoert, gaat u naar de webgebruikersinterface om te zien of er geen meldingen voor register restore fouten. Als het register herstellen is mislukt, voert u register restore. Voor meer informatie gaat u naar het [uitvoeren register restore](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry).
    * Als er een omlaag apparaat die v2.1.1.518 niet werd uitgevoerd, moet u een failover naar een vervangende apparaat waarop v2.1.1.518 uitvoeren. Raadpleeg voor gedetailleerde instructies DR van uw 5000/7000-serie StorSimple-apparaat.
    * Back-up van de gegevens van uw apparaat door het maken van een cloudmomentopname.
    * Controleer of er andere actieve back-uptaken die worden uitgevoerd op het bronapparaat. Dit omvat de taken op de host StorSimple Data Protection Console. Wacht totdat de huidige taken te voltooien.


### <a name="for-the-8000-series-physical-device-target"></a>Voor het 8000 series fysieke apparaat (doel)

Voordat u de migratie begint, zorg ervoor dat:

* 8000-serie doelapparaat is ingeschreven en wordt uitgevoerd. Zie voor meer informatie hoe [StorSimple-apparaat met de StorSimple Manager-service implementeren](storsimple-8000-deployment-walkthrough-u2.md).
* Uw apparaat 8000 series heeft de meest recente StorSimple 8000 Series Update 5 geïnstalleerd en 6.3.9600.17845 of hoger wordt uitgevoerd. Als uw apparaat niet de nieuwste updates zijn geïnstalleerd heeft, moet u de meest recente updates installeren voordat u kunt doorgaan met de migratie. Zie voor meer informatie hoe [meest recente update installeren op uw apparaat 8000 series](storsimple-8000-install-update-5.md).
* Uw Azure-abonnement is ingeschakeld voor de migratie. Als uw abonnement is niet ingeschakeld, moet u contact op met Microsoft Support zodat uw abonnement voor de migratie.

### <a name="for-the-80108020-cloud-appliance-target"></a>Voor het toestel 8010/8020 cloud (doel)

Voordat u de migratie begint, moet u controleren:

* Uw doel cloud-apparaat is ingeschreven en worden uitgevoerd. Zie voor meer informatie hoe [implementeren en beheren van StorSimple Cloud toestel](storsimple-8000-cloud-appliance-u2.md).
* Uw toestel cloud wordt de meest recente StorSimple 8000 Series Update 5 softwareversie 6.3.9600.17845 uitgevoerd. Als uw toestel cloud Update 5 niet wordt uitgevoerd, maakt u een nieuwe Update 5 cloud toestel voordat u met de migratie verdergaat. Zie voor meer informatie hoe [maken van een apparaat 8010/8020 cloud](storsimple-8000-cloud-appliance-u2.md).

### <a name="for-the-computer-running-storsimple-migration-tool"></a>Voor de computer met hulpprogramma voor migratie van StorSimple

Hulpprogramma voor migratie van StorSimple is een gebruikersinterface gebaseerde hulpprogramma waarmee u gegevens migreren van een StorSimple 5000 7000-serie op een apparaat 8000-serie. Gebruik een computer die voldoet aan de volgende vereisten voor het installeren van het hulpprogramma voor migratie van StorSimple.

De computer verbinding heeft met Internet en:

* De volgende besturingssystemen wordt uitgevoerd
    * Windows 10.
    * Windows Server 2012 R2 (of hoger) voor het installeren van hulpprogramma voor migratie van StorSimple.
* Met .NET 4.5.2 geïnstalleerd.
* Minimaal 5 GB vrije ruimte installeren en gebruiken van het hulpprogramma heeft.

> [!TIP]
> Als uw StorSimple-apparaat is verbonden met een Windows Server-host, kunt u het hulpprogramma voor migratie installeren op de Windows Server-hostcomputer.

#### <a name="to-install-storsimple-migration-tool"></a>Hulpprogramma voor migratie van de StorSimple installeren

De volgende stappen voor het hulpprogramma voor migratie van de StorSimple installeren op uw computer uitvoeren.

1. Kopieer de map _StorSimple8000SeriesMigrationTool_ naar uw Windows-computer. Zorg ervoor dat het station waar de software is gekopieerd voldoende ruimte heeft.

    Open het hulpprogramma configuratiebestand _StorSimple8000SeriesMigrationTool.exe.config_ in de map. Hier volgt het fragment van het bestand.
    
    ```
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Bewerk de waarden die overeenkomen met de sleutels en vervangen door:

    * `UserName`– De gebruikersnaam voor aanmelding bij Azure-portal.
    * `SubscriptionName and SubscriptionId`– Geef een naam en -ID voor uw Azure-abonnement. In uw StorSimple-apparaat Manager service landingspagina, onder **algemene**, klikt u op **eigenschappen**. Kopieer de naam van het abonnement en abonnements-ID die is gekoppeld aan uw service.
    * `ResourceName`– Naam van uw StorSimple-apparaat Manager-service in de Azure portal. Ook die wordt weergegeven onder het service-eigenschappen.
    * `ResourceGroup`– Naam van de resourcegroep die zijn gekoppeld aan uw StorSimple-apparaat Manager-service in de Azure portal. Ook die wordt weergegeven onder het service-eigenschappen.
    ![Controleer de eigenschappen van de service voor doelapparaat](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId`– Azure Active Directory-Tenant-ID in Azure-portal. Meld u aan bij Microsoft Azure als beheerder. Klik in de Microsoft Azure-portal op **Azure Active Directory**. Onder **beheren**, klikt u op **eigenschappen**. De tenant ID wordt weergegeven in de **map-ID** vak.
    ![Tenant-ID voor Azure Active Directory controleren](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png)

3.  Sla de wijzigingen in het configuratiebestand.
4.  Voer de _StorSimple8000SeriesMigrationTool.exe_ starten van het hulpprogramma. Als u wordt gevraagd om referenties, geef de referenties die zijn gekoppeld aan uw abonnement in Azure-portal. 
5.  Het hulpprogramma voor migratie van StorSimple UI wordt weergegeven.
  

## <a name="next-steps"></a>Volgende stappen
De stapsgewijze instructies voor het downloaden [gegevens migreren van een StorSimple 5000 7000-serie naar een apparaat 8000 series](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b).
