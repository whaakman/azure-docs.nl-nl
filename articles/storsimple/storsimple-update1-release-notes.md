---
title: StorSimple 8000 Series Update 1.2 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 1.2.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2856cda1fde04ab61b4cf15ad0dcc3db2a9df68
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>1.2 release-opmerkingen voor uw StorSimple 8000 serie-apparaat bijwerken
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 1.2. Ze bevatten ook een lijst van de StorSimple-software, stuurprogramma en schijf firmware-updates is opgenomen in deze release. 

Update 1.2 kan worden toegepast op een StorSimple-apparaat met Release (GA), Update 0.1, Update 0.2 of Update 0.3-software. Update 1.2 is niet beschikbaar als uw apparaat met Update 1 of Update 1.1. Als u uw apparaat wordt uitgevoerd Release (GA), neemt u [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om u te helpen bij het installeren van deze update.

De volgende tabel bevat de versies van het apparaat software overeenkomt met update 1, 1.1 en 1.2.

| Als de update wordt uitgevoerd... | Dit is de versie van uw apparaat software. |
| --- | --- |
| 1.2 bijwerken |6.3.9600.17584 |
| 1.1 bijwerken |6.3.9600.17521 |
| Update 1.0 |6.3.9600.17491 |

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert. Zie voor meer informatie hoe [Update 1.2 installeren op uw StorSimple-apparaat](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Het duurt ongeveer 5 tot 10 uur deze update (met inbegrip van de Windows-Updates) te installeren. 
> * 1.2-update heeft software, LSI stuurprogramma en schijf firmware-updates. Als u wilt installeren, volg de instructies in [Update 1.2 installeren op uw StorSimple-apparaat](storsimple-install-update-1.md).
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Zoeken naar updates in een paar dagen opnieuw als deze is binnenkort beschikbaar.
> 
> 

## <a name="whats-new-in-update-12"></a>Wat is er nieuw in Update 1.2
Deze functies zijn voor het eerst uitgebracht met Update 1, die beschikbaar zijn voor een beperkt aantal gebruikers is gemaakt. Met de release van Update 1.2 ziet de meeste van de StorSimple-gebruikers de volgende nieuwe functies en verbeteringen:

* **Migratie van 5000 7000-serie naar apparaten 8000 serie** – deze release introduceert een nieuwe migratiefunctie waarmee de StorSimple 5000 7000 series toestel gebruikers hun gegevens migreren naar een fysiek StorSimple 8000 serie-apparaat of een virtueel apparaat. De migratiefunctie heeft twee belangrijkste waardevoorstellen:                                                                  
  
  * **Zakelijke continuïteit**, doordat de migratie van bestaande gegevens op 5000 7000-serie toestellen op 8000 series apparaten.
  * **De offerings functie van de toestellen 8000 series verbeterd**, zoals efficiënt gecentraliseerd beheer van meerdere apparaten via de StorSimple Manager-service voor een betere klasse van hardware en firmware, virtuele apparaten gegevensmobiliteit en functies in de toekomst roadmap bijgewerkt.
    
    Raadpleeg de [Migratiehandleiding](http://www.microsoft.com/download/details.aspx?id=47322) voor meer informatie over het migreren van een StorSimple 5000 7000-serie op een apparaat 8000-serie. 
* **Beschikbaarheid in de Portal voor Azure Government** – StorSimple is nu beschikbaar in de portal voor Azure Government. Zie hoe [implementeren van een StorSimple-apparaat in de Portal voor Azure Government](storsimple-deployment-walkthrough-gov.md).
* **Ondersteuning voor andere cloudserviceproviders** – andere cloudserviceproviders ondersteund Amazon S3, Amazon S3 met RRS, HP en OpenStack (bèta) zijn.
* **Update van de meest recente Storage-API's** – met deze versie StorSimple is bijgewerkt naar de nieuwste Azure Storage-service API's. StorSimple 8000 series apparaten die worden uitgevoerd vóór het bijwerken 1 softwareversies (Release 0,1 0,2 en 0,3) met een versie van de Azure Storage-Service API-ouder is dan 17 juli 2009. Zoals vermeld in de bijgewerkte [aankondiging over het verwijderen van opslag-versies](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), door 1 augustus 2016 krijgen deze API wordt afgeschaft. Het is noodzakelijk StorSimple 8000 Series Update 1 vóór 1 augustus 2016 toe te passen. Als u dit niet doet, meer StorSimple-apparaten correct werkt.
* **Ondersteuning voor Zone redundante opslag (ZRS)** – met de upgrade naar de nieuwste versie van de Storage-API's, de serie StorSimple 8000 Zone redundante opslag (ZRS) naast lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS) ondersteunt. Neem deze [artikel op Azure redundantie opslagopties](../storage/common/storage-redundancy.md) voor ZRS meer informatie.
* **Verbeterde ervaring voor initiële implementeren en bijwerken** : In deze release, de installatie en update processen zijn verbeterd. De installatie via de wizard setup is verbeterd om feedback geven aan de gebruiker als de netwerkconfiguratie en firewall-instellingen onjuist zijn. Aanvullende diagnostische cmdlets er om u te helpen bij het oplossen van netwerken van het apparaat zijn opgegeven. Zie de [probleemoplossing implementatie artikel](storsimple-troubleshoot-deployment.md) voor meer informatie over de nieuwe diagnostische cmdlets gebruikt voor het oplossen van problemen.

## <a name="issues-fixed-in-update-12"></a>Problemen in Update 1.2 opgelost
De volgende tabel bevat een samenvatting van problemen die Updates 1.2, 1.1 en 1 zijn opgelost.    

| Nee. | Functie | Probleem | Opgelost in Update | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Windows PowerShell voor StorSimple |Wanneer een gebruiker het StorSimple-apparaat op afstand toegang te krijgen met Windows PowerShell voor StorSimple en vervolgens de wizard setup worden gestart, wordt een crash zo snel Data 0 IP is ingevoerd is opgetreden. Deze fout is nu opgelost in Update 1. |Update 1 |Ja |Ja |
| 2 |Fabrieksinstellingen terugzetten |In sommige gevallen, tijdens de fabrieksinstellingen terug te zetten, het StorSimple-apparaat is vastgelopen en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8)**. Dit het geval als u CTRL + C ingedrukt terwijl de cmdlet uitgevoerd is. Deze fout is nu hersteld. |Update 1 |Ja |Nee |
| 3 |Fabrieksinstellingen terugzetten |Nadat een factory is mislukt, dubbele domeincontroller opnieuw hebt ingesteld, is het zijn toegestaan om door te gaan met de apparaatregistratie van het. Dit resulteert in een niet-ondersteunde systeemconfiguratie. Een foutbericht wordt weergegeven in Update 1 en registratie is geblokkeerd op een apparaat dat een mislukte op Fabrieksinstellingen terugzetten is. |Update 1 |Ja |Nee |
| 4 |Fabrieksinstellingen terugzetten |In sommige gevallen zijn false positief verschil waarschuwingen gegenereerd. Op apparaten met Update 1 wordt niet langer onjuist verschil waarschuwingen gegenereerd. |Update 1 |Ja |Nee |
| 5 |Fabrieksinstellingen terugzetten |Als een fabrieksinstellingen is onderbroken voordat deze zijn voltooid, wordt het apparaat de herstelmodus en staat niet toe dat u toegang tot Windows PowerShell voor StorSimple. Deze fout is nu hersteld. |Update 1 |Ja |Nee |
| 6 |Herstel na noodgeval |Een disaster recovery (DR)-fout is verholpen waarin DR mislukken tijdens de detectie van back-ups op het doelapparaat. |Update 1 |Ja |Ja |
| 7 |Bewaking LED 's |In bepaalde gevallen LED's aan het einde van toestel bewaking niet gespecificeerd juiste status. De blauwe LED is uitgeschakeld. DATA 0 en 1 LED's van gegevens zijn knippert zelfs wanneer deze interfaces niet zijn geconfigureerd. Het probleem is opgelost en bewaking LED's nu geven aan de juiste status. |Update 1 |Ja |Nee |
| 8 |Bewaking LED 's |In bepaalde gevallen wordt na het toepassen van Update 1 de blauw licht op de actieve controller uitgeschakeld waardoor het moeilijk te identificeren van de actieve controller. Dit probleem is verholpen in deze release van de patch. |1.2 bijwerken |Ja |Nee |
| 9 |Netwerkinterfaces |In eerdere versies van kan een StorSimple-apparaat dat is geconfigureerd met een niet-routeerbare gateway offline gaan. In deze release de routering metriek voor Data 0 is gedaan de laagste; zelfs als andere netwerkinterfaces ingeschakeld voor de cloud zijn, wordt er daarom het cloud-verkeer van het apparaat worden gerouteerd via Data 0. |Update 1 |Ja |Ja |
| 10 |Back-ups |In de patch-versie 1.1 van de Update is een fout in Update 1 waardoor back-ups mislukken na 24 dagen opgelost. |1.1 bijwerken |Ja |Ja |
| 11 |Back-ups |Een bug in eerdere versies heeft geleid tot slechte prestaties voor cloudmomentopnamen van met lage wijziging tarieven. Deze fout is verholpen in deze release van de patch. |1.2 bijwerken |Ja |Ja |
| 12 |Updates |Een fout in Update 1, die een mislukte upgrade gerapporteerd, waardoor de controllers om te gaan in de herstelmodus overgaat is opgelost in deze release van de patch. |1.2 bijwerken |Ja |Ja |

## <a name="known-issues-in-update-12"></a>Bekende problemen in Update 1.2
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf vervolgens de opslaggroep worden offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. |Ja |Ja | |
| 4 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. Apparaat failover van één apparaat inactief naar meerdere apparaten brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 5 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Wanneer u de cmdlet StorSimple gebruiken **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - wacht** het eerste object selecteren, zodat u kunt een nieuwe maken **VolumeContainer** object, de cmdlet retourneert alle objecten. |Inpakken van de cmdlet tussen haakjes als volgt: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - eerst 1 - wachttijd** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de ETA voor de meest recente back-up nauwkeurig is alleen voor de eerste volumecontainer. Bovendien start parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer zijn gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het terugzetten zijn volumes niet toegevoegd aan de back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet het 5000/7000-serie apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijdert nadat de migratie voltooid en doorgevoerd is. |Ja |Nee |
| 13 |Kloon en Noodherstel |Een StorSimple-apparaat met Update 1 kan niet klonen of herstel na noodgevallen bij een apparaat met 1 software vóór het bijwerken uitvoeren. |U moet het doelapparaat op 1 bijwerken zodat deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Configuratieback-up voor de migratie kan op een apparaat 5000 7000-serie mislukken wanneer er groepen met geen gekoppelde volumes volume zijn. |Alle lege volume-groepen die geen gekoppelde volumes verwijderen en probeer vervolgens de configuratieback-up. |Ja |Nee |

## <a name="physical-device-updates-in-update-12"></a>Fysiek apparaat-updates in de Update 1.2
Als het bijwerken van de patch 1.2 wordt toegepast op een fysiek apparaat (met eerdere versies dan Update 1), versie van de software wordt gewijzigd in 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller en firmware-updates in de Update 1.2
Deze release werkt het stuurprogramma en de firmware van de schijf op uw apparaat.

* Zie voor meer informatie over de update van de domeincontroller SAS [Update 1 voor LSI SAS-controllers in Microsoft Azure StorSimple toestel](https://support.microsoft.com/kb/3043005). 
* Zie voor meer informatie over de schijf firmware-update [schijf firmware Update 1 voor Microsoft Azure StorSimple-apparaat](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Virtueel apparaat-updates in de Update 1.2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-steps"></a>Volgende stappen
* [Update 1.2 op uw apparaat installeert](storsimple-install-update-1.md).

