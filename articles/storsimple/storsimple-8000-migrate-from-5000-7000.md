---
title: Migreren van gegevens op StorSimple 5000-7000 serie 8000-serie-apparaat | Microsoft Docs
description: Biedt een overzicht en de vereisten van de functie voor migratie.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/11/2017
ms.author: alkohli
ms.openlocfilehash: 378eaea4c0cf19c48f47067e4f548f2d7f530377
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856685"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>Gegevens migreren van de StorSimple 5000-7000-serie 8000-serie-apparaat

> [!IMPORTANT]
> - Migratie wordt momenteel een bewerking voor assistentie. Als u van plan bent om gegevens te migreren van uw StorSimple 5000-7000-serie-apparaat op een apparaat uit de 8000-serie, moet u voor het plannen van de migratie met Microsoft Support. Microsoft Support kunnen vervolgens uw abonnement voor de migratie. Zie voor meer informatie over het [Open een ondersteuningsticket](storsimple-8000-contact-microsoft-support.md).
> - Nadat u de serviceaanvraag-bestand, duurt het enkele weken uit te voeren van het migratieplan en de migratie daadwerkelijk wordt gestart.
> - Voordat u contact opneemt met Microsoft Support, moet u ervoor dat u de sla en volledige de [migratie vereisten](#migration-prerequisites) aangegeven in het artikel.

## <a name="overview"></a>Overzicht

Dit artikel bevat de migratiefunctie waarmee de StorSimple 5000-7000-serie klanten om hun gegevens naar StorSimple 8000-serie fysiek apparaat of een 8010/8020-cloudapparaat te migreren. In dit artikel bevat ook koppelingen naar een downloadbare stapsgewijze uitleg van de stappen die nodig zijn voor het migreren van gegevens uit een 5000-7000 serie oudere apparaten op een fysieke 8000 serie of cloudapparaat.

In dit artikel is van toepassing voor zowel de on-premises 8000-serie-apparaat, evenals de StorSimple-Cloudapparaat.


## <a name="migration-feature-versus-host-side-migration"></a>Migratiefunctie ten opzichte van de host-side '-migratie

U kunt uw gegevens met behulp van de migratiefunctie voor verplaatsen of door het uitvoeren van een host-side '-migratie. Deze sectie beschrijft de details van elke methode met inbegrip van de voor- en nadelen. Gebruik deze informatie om te achterhalen welke methode u wilt verrichten om uw gegevens te migreren.

De migratiefunctie simuleert een disaster recovery (DR) proces uit 7000/5000 serie 8000-serie. Deze functie kunt u de gegevens van de indeling van 5000/7000-serie migreren naar de 8000-serie-indeling op Azure. Het migratieproces wordt gestart met behulp van het hulpprogramma voor migratie van StorSimple. Het hulpprogramma voor het downloaden en de conversie van back-upmetagegevens begint op het apparaat uit de 8000-serie en gebruikt vervolgens de meest recente back-up om beschikbaar te stellen van de volumes op het apparaat.

|      | Professionals                                                                                                                                     |Nadelen                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Het migratieproces bewaart de geschiedenis van back-ups die zijn uitgevoerd voor de 5000/7000-serie.                                               | Wanneer gebruikers proberen toegang tot de gegevens, worden de gegevens in deze migratie gedownload van Azure waardoor er gegevens downloaden kosten in rekening.                                     |
| 2.   | Er zijn geen gegevens worden gemigreerd aan de host.                                                                                                     | Het proces moet downtime tussen het begin van de back-up en de meest recente back-up wordt opgehaald op de 8000-serie (kan worden geschat met behulp van het hulpprogramma voor migratie). |
| 3.   | Dit proces bewaart alle beleidsregels, bandbreedtesjablonen, versleuteling en andere instellingen op apparaten uit de 8000-serie.                      | Gebruikerstoegang gaat terug alleen de gegevens die worden gebruikt door de gebruikers en wordt de volledige gegevensset niet rehydrate.                                                  |
| 4.   | Dit proces vereist extra tijd om te converteren van alle de oudere back-ups in Azure die asynchroon wordt uitgevoerd zonder gevolgen voor productie | Migratie kan alleen worden uitgevoerd op het niveau van een cloud-configuratie.  Afzonderlijke volumes in een cloudconfiguratie kunnen niet afzonderlijk worden gemigreerd.                       |

Een host-side '-migratie kan onafhankelijk van de 8000-serie instellen en het kopiëren van de gegevens van het apparaat uit de 5000/7000-serie 8000-serie-apparaat. Dit is gelijk aan gegevens van één opslagapparaat migreert naar een andere. Tal van hulpprogramma's zoals Diskboss, robocopy worden gebruikt om de gegevens te kopiëren.

|      | Professionals                                                                                                                      |Nadelen                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Migratie kan op een volume door volume uit te voeren op een gefaseerde manier worden gerealiseerd.                                               | Vorige back-ups (die 5000/7000-serie) wordt niet beschikbaar zijn op het apparaat 8000-serie.                                                                                                       |
| 2.   | Kan voor de consolidatie van gegevens naar een opslagaccount in Azure.                                                       | Eerste back-up naar de cloud op 8000-serie duurt langer als alle gegevens op de behoeften van de 8000-serie aan de back-up naar Azure.                                                                     |
| 3.   | Na een geslaagde migratie, alle gegevens lokaal op het apparaat is. Er zijn geen latentie bij het openen van de gegevens. | Gebruik van Azure-opslag wordt verhoogd tot de gegevens worden verwijderd uit de 5000/7000-apparaat.                                                                                                        |
| 4.   |                                                                                                                           | Als het apparaat uit de 7000/5000-serie een grote hoeveelheid gegevens heeft, tijdens de migratie van moeten deze gegevens worden gedownload van azure die in rekening gebracht wordt kosten en latentie met betrekking tot het downloaden van gegevens van Azure |

In dit artikel richt zich alleen met de migratiefunctie uit de 5000/7000 apparaat uit de 8000-serie. Voor meer informatie over de host-side '-migratie, gaat u naar [migratie vanaf andere opslagapparaten](http://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating%20Data%20to%20StorSimple%20Volumes_09-02-15.pdf).

## <a name="migration-prerequisites"></a>Vereisten voor migratie

Hier volgen de vereisten voor migratie voor uw verouderde 5000 of 7000 serie apparaat en de storsimple 8000-serie.

> [!IMPORTANT]
> Controleer en voltooi de migratie-vereisten voordat u een serviceaanvraag met Microsoft Support.

### <a name="for-the-50007000-series-device-source"></a>Voor het apparaat 5000/7000-serie (bron)

Voordat u de migratie begint, zorg ervoor dat:

* U hebt uw 5000 of 7000-serie bron-apparaat. het apparaat kan worden live id of de pijl-omlaag.

    > [!IMPORTANT]
    > Het is raadzaam dat u seriële toegang tot dit apparaat tijdens het migratieproces hebben. Moet er problemen met het apparaat, kan seriële toegang helpen bij het oplossen van problemen.

* Uw Bronapparaat 5000 of 7000 serie v2.1.1.518 voor software-versie wordt uitgevoerd of hoger. Eerdere versies worden niet ondersteund.
* Om te controleren of de versie die uw 5000 of 7000-serie wordt uitgevoerd, bekijkt u de rechterbovenhoek van uw Web-UI. Dit moet de versie van de software die uw apparaat wordt uitgevoerd worden weergegeven. Voor de migratie, moet uw 5000 of 7000-serie v2.1.1.518 worden uitgevoerd.

    ![Softwareversie op oudere apparaten controleren](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Als uw apparaat actief is niet actief v2.1.1.518 of later, Waardeer uw systeem naar de vereiste minimale versie. Raadpleeg voor gedetailleerde instructies [upgraden van uw systeem naar v2.1.1.518](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
    * Als u v2.1.1.518 uitvoert, gaat u naar de web-UI te zien of er geen meldingen voor fouten bij het terugzetten van register. Als het register herstellen is mislukt, voert u register herstellen. Voor meer informatie gaat u naar het [register herstellen uitvoeren](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry).
    * Hebt u een omlaag apparaat die v2.1.1.518 niet werd uitgevoerd, moet u een failover naar een vervangend apparaat waarop v2.1.1.518 uitvoeren. Raadpleeg voor gedetailleerde instructies voor herstel na Noodgevallen van het apparaat van de StorSimple 5000/7000-serie.
    * Back-up van de gegevens voor uw apparaat door het maken van een cloudmomentopname.
    * Controleer voor alle andere actieve back-uptaken die worden uitgevoerd op het bronapparaat. Dit omvat de taken op de host van de Console van het StorSimple-bescherming van gegevens. Wachten op de huidige taken om te voltooien.


### <a name="for-the-8000-series-physical-device-target"></a>Voor de fysieke 8000-serie-apparaat (doel)

Voordat u de migratie begint, zorg ervoor dat:

* Het doel 8000-serie-apparaat is ingeschreven en wordt uitgevoerd. Zie voor meer informatie over het [StorSimple-apparaat bij StorSimple Manager-service implementeren](storsimple-8000-deployment-walkthrough-u2.md).
* Uw apparaat 8000-serie heeft de meest recente StorSimple 8000 serie Update 5 geïnstalleerd en 6.3.9600.17845 of hoger wordt uitgevoerd. Als uw apparaat beschikt niet over de meest recente updates zijn geïnstalleerd, moet u de meest recente updates installeren voordat u verder kunt gaan met de migratie. Zie voor meer informatie over het [meest recente update installeren op uw apparaat 8000-serie](storsimple-8000-install-update-5.md).
* Uw Azure-abonnement is ingeschakeld voor migratie. Als uw abonnement niet is ingeschakeld, moet u contact op met Microsoft Support om in te schakelen van uw abonnement voor de migratie.

### <a name="for-the-80108020-cloud-appliance-target"></a>Voor de 8010/8020-cloudapparaat (doel)

Voordat u de migratie begint, controleert u of:

* Uw cloudapparaat doel is geregistreerd en wordt uitgevoerd. Zie voor meer informatie over het [implementeren en beheren van StorSimple-Cloudapparaat](storsimple-8000-cloud-appliance-u2.md).
* Uw cloudapparaat wordt de meest recente StorSimple 8000 serie Update 5 softwareversie 6.3.9600.17845 uitgevoerd. Als uw cloudapparaat Update 5 niet wordt uitgevoerd, maakt u een nieuwe Update 5-cloudapparaat voordat u met de migratie doorgaat. Zie voor meer informatie over het [een 8010/8020-cloudapparaat maken](storsimple-8000-cloud-appliance-u2.md).

### <a name="for-the-computer-running-storsimple-migration-tool"></a>Voor de computer met hulpprogramma voor migratie van StorSimple

Hulpprogramma voor migratie van StorSimple is een gebruikersinterface gebaseerde hulpprogramma waarmee u gegevens migreren van een StorSimple 5000-7000-serie op een apparaat uit de 8000-serie. Voor het installeren van het hulpprogramma voor migratie van StorSimple, een computer die voldoet aan de volgende vereisten te gebruiken.

De computer heeft verbinding met Internet en:

* De volgende besturingssystemen wordt uitgevoerd
    * Windows 10.
    * Windows Server 2012 R2 (of hoger) voor het installeren van hulpprogramma voor migratie van StorSimple.
* Met .NET 4.5.2 geïnstalleerd.
* Minimaal 5 GB vrije ruimte om te installeren en gebruik het hulpprogramma heeft.

> [!TIP]
> Als uw StorSimple-apparaat is verbonden met een Windows Server-host, kunt u het hulpprogramma voor migratie installeren op de Windows Server-hostcomputer.

#### <a name="to-install-storsimple-migration-tool"></a>Voor het installeren van hulpprogramma voor migratie van StorSimple

Voer de volgende stappen uit voor het installeren van hulpprogramma voor migratie van StorSimple op uw computer.

1. Kopieer de map _StorSimple8000SeriesMigrationTool_ op uw Windows-computer. Zorg ervoor dat het station waarop de software is gekopieerd voldoende ruimte heeft.

    Open het configuratiebestand van het hulpprogramma _StorSimple8000SeriesMigrationTool.exe.config_ in de map. Hier is het fragment van het bestand.
    
    ```
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Bewerk de waarden die overeenkomen met de sleutels en vervang:

    * `UserName` – De gebruikersnaam voor aanmelding bij Azure portal.
    * `SubscriptionName and SubscriptionId` – Geef een naam en ID voor uw Azure-abonnement. In uw StorSimple Device Manager-service startpagina onder **algemene**, klikt u op **eigenschappen**. Kopieer de naam van het abonnement en abonnements-ID die is gekoppeld aan uw service.
    * `ResourceName` : De naam van uw StorSimple Device Manager-service in Azure portal. Ook die wordt weergegeven onder de eigenschappen van de service.
    * `ResourceGroup` : De naam van de resourcegroep die is gekoppeld aan uw StorSimple Device Manager-service in Azure portal. Ook die wordt weergegeven onder de eigenschappen van de service.
    ![Controleer de eigenschappen van de service voor doelapparaat](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId` – Azure Active Directory Tenant-ID in Azure portal. Meld u aan bij Microsoft Azure aan als beheerder. Klik in de Microsoft Azure-portal op **Azure Active Directory**. Onder **beheren**, klikt u op **eigenschappen**. De tenant ID wordt weergegeven in de **map-ID** vak.
    ![Tenant-ID voor Azure Active Directory controleren](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png)

3.  Sla de wijzigingen aangebracht in het configuratiebestand.
4.  Voer de _StorSimple8000SeriesMigrationTool.exe_ om het hulpprogramma te starten. Wanneer u hierom wordt gevraagd om referenties, geef de referenties die zijn gekoppeld aan uw abonnement in Azure portal. 
5.  Het hulpprogramma voor migratie van StorSimple gebruikersinterface wordt weergegeven.
  

## <a name="next-steps"></a>Volgende stappen
De stapsgewijze instructies voor het downloaden [gegevens migreren van een StorSimple 5000-7000 serie naar een apparaat 8000-serie](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b).
