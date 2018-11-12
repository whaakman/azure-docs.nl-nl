---
title: Opmerkingen bij de release van de StorSimple 8000 serie Update 1.2 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 serie Update 1.2.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11138857e33eec0f854ddb61956ea24c858c49a5
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258983"
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>1.2 opmerkingen bij de release voor uw StorSimple 8000-apparaat bijwerken

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen identificeren voor StorSimple 8000 serie Update 1.2. Ze bevatten ook een lijst van de StorSimple-software, stuurprogramma en updates van de schijffirmware inbegrepen in deze release. 

Update 1.2 kan worden toegepast op alle StorSimple-apparaat met Release (GA), Update 0.1, 0.2-Update of Update 0.3-software. Update 1.2 is niet beschikbaar als uw apparaat met Update 1 of Update 1.1. Als uw apparaat wordt uitgevoerd Release (GA), kunt u [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) om u te helpen bij het installeren van deze update.

De volgende tabel bevat de versies van het apparaat software overeenkomt met update 1, 1.1 en 1.2.

| Als de update wordt uitgevoerd... | Dit is de versie van de apparaatsoftware. |
| --- | --- |
| Update 1.2 |6.3.9600.17584 |
| 1.1 bijwerken |6.3.9600.17521 |
| Update 1.0 |6.3.9600.17491 |

Lees de informatie in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert. Zie voor meer informatie over het [Update 1.2 installeren op uw StorSimple-apparaat](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Het duurt ongeveer 5 tot 10 uur om deze update (met inbegrip van de Windows-Updates) te installeren. 
> * Update 1.2 heeft software, LSI-stuurprogramma en updates van de schijffirmware. Als u wilt installeren, volg de instructies in [Update 1.2 installeren op uw StorSimple-apparaat](storsimple-install-update-1.md).
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Zoeken naar updates in een paar dagen opnieuw als deze snel weer beschikbaar is.
> 
> 

## <a name="whats-new-in-update-12"></a>Wat is er nieuw in Update 1.2
Deze functies zijn voor het eerst uitgebracht met Update 1 die beschikbaar zijn voor een beperkt aantal gebruikers is gemaakt. Met de release van Update 1.2 ziet de meeste van de StorSimple-gebruikers de volgende nieuwe functies en verbeteringen:

* **Migratie van 5000-7000 serie 8000-serie apparaten** : deze versie introduceert een nieuwe migratiefunctie waarmee de StorSimple 5000-7000 serie toestel gebruikers hun gegevens migreren naar een fysiek apparaat voor StorSimple 8000-serie of een virtuele apparaat. De migratiefunctie heeft twee belangrijke waardevoorstellen:                                                                  
  
  * **Zakelijke continuïteit**, door het inschakelen van de migratie van bestaande gegevens op de apparatuur uit de 5000-7000 serie aan de 8000-serie.
  * **Verbeterde functie voor aanbiedingen van apparatuur uit de 8000-serie**, zoals efficiënte gecentraliseerd beheer van meerdere apparaten via de StorSimple Manager-service, een betere klasse van hardware en firmware, virtuele apparaten, gegevensmobiliteit, bijgewerkt en functies in de toekomst roadmap.
    
    Raadpleeg de [Migratiehandleiding](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b) voor details over het migreren van een StorSimple 5000-7000-serie met een apparaat dat 8000-serie. 
* **Beschikbaarheid in de Azure Government-Portal** : StorSimple is nu beschikbaar in de Azure Government-portal. Zie hoe u [implementeren van een StorSimple-apparaat in de Azure Government-Portal](storsimple-deployment-walkthrough-gov.md).
* **Ondersteuning voor andere cloudserviceproviders** – andere cloudserviceproviders ondersteund Amazon S3, Amazon S3 met RRS, HP en OpenStack (bèta) zijn.
* **Update voor de meest recente Storage-API's** – met deze versie StorSimple is bijgewerkt naar de nieuwste Azure Storage-service API's. Apparaten van de StorSimple 8000-serie met software-versies vóór Update 1 (versie 0.1, 0.2 en 0,3) met een versie van de Azure Storage-Service-API's ouder zijn dan 17 juli 2009. Zoals vermeld in de bijgewerkte [aankondiging over het verwijderen van opslag-Serviceversies](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), door 1 augustus 2016 krijgen deze API's wordt afgeschaft. Het is noodzakelijk dat u de StorSimple 8000 serie Update 1 vóór 1 augustus 2016 van toepassing zijn. Als u niet om dit te doen, stopt StorSimple-apparaten correct werkt.
* **Ondersteuning voor Zone Redundant Storage (ZRS)** – met de upgrade naar de nieuwste versie van de Storage-API's, Zone Redundant Storage (ZRS) naast lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS biedt ondersteuning voor de StorSimple 8000-serie ). Verwijzen naar dit [artikel over Azure Storage-redundantieopties](../storage/common/storage-redundancy.md) voor ZRS meer informatie.
* **Verbeterde ervaring voor initiële implementatie en update** : In deze release de installatie- en update-processen zijn verbeterd. De installatie via de installatiewizard is verbeterd om feedback aan de gebruiker als de configuratie van het netwerk en firewall-instellingen onjuist zijn. Aanvullende diagnostische cmdlets is te vinden voor hulp bij het oplossen van netwerken van het apparaat. Zie de [implementatie probleemoplossingsartikel](storsimple-troubleshoot-deployment.md) voor meer informatie over de nieuwe diagnostische cmdlets gebruikt voor het oplossen van problemen.

## <a name="issues-fixed-in-update-12"></a>Problemen opgelost in Update 1.2
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Updates 1.2, 1.1 en 1.    

| Nee. | Functie | Probleem | Opgelost in Update | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Windows PowerShell voor StorSimple |Wanneer een gebruiker het StorSimple-apparaat op afstand toegang te krijgen met behulp van Windows PowerShell voor StorSimple en vervolgens de wizard setup worden gestart, wordt een crash zo snel Data 0 IP is ingevoerd is opgetreden. Deze fout is nu opgelost in Update 1. |Update 1 |Ja |Ja |
| 2 |Fabrieksinstellingen terugzetten |In sommige gevallen, tijdens de fabrieksinstellingen terug te zetten, het StorSimple-apparaat is geworden vastgelopen en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8)**. Dit is gebeurd als u op CTRL + C drukt, terwijl de cmdlet werd uitgevoerd. Deze fout is nu hersteld. |Update 1 |Ja |Nee |
| 3 |Fabrieksinstellingen terugzetten |Nadat een factory dual controller defect is opnieuw ingesteld, is het zijn toegestaan om door te gaan met device Registration service. Dit heeft geresulteerd in een niet-ondersteunde configuratie. In de Update 1, een foutbericht wordt weergegeven en registratie is geblokkeerd op een apparaat dat is een mislukte Fabrieksinstellingen terugzetten. |Update 1 |Ja |Nee |
| 4 |Fabrieksinstellingen terugzetten |In sommige gevallen, zijn ONWAAR positief incompatibel waarschuwingen gegenereerd. Op apparaten met Update 1 worden niet langer onjuiste incompatibel waarschuwingen gegenereerd. |Update 1 |Ja |Nee |
| 5 |Fabrieksinstellingen terugzetten |Als de fabrieksinstellingen terug te zetten werd onderbroken voordat is voltooid, wordt het apparaat de herstelmodus en staat niet toe dat u toegang krijgen tot Windows PowerShell voor StorSimple. Deze fout is nu hersteld. |Update 1 |Ja |Nee |
| 6 |Herstel na noodgeval |Een disaster recovery (DR)-fout is opgelost waarbij herstel na Noodgeval mislukken tijdens de detectie van back-ups op het doelapparaat. |Update 1 |Ja |Ja |
| 7 |Controle-LED 's |In bepaalde gevallen, controle-LED's aan het einde van apparaat heeft geen status aan te geven juist. De blauwe LED is uitgeschakeld. DATA 0 en 1-LED's van gegevens zijn knipperen, zelfs wanneer deze interfaces zijn niet geconfigureerd. Het probleem is opgelost en controle-LED's geven nu de juiste status. |Update 1 |Ja |Nee |
| 8 |Controle-LED 's |In bepaalde gevallen, na het toepassen van Update 1, de blauwe licht op de actieve controller uitgeschakeld waardoor het moeilijk te bepalen van de actieve controller. Dit probleem is opgelost in deze release van de patch. |Update 1.2 |Ja |Nee |
| 9 |Netwerkinterfaces |In eerdere versies van kan een StorSimple-apparaat dat is geconfigureerd met een niet-routeerbare gateway offline gaan. In deze release de routering metrische gegevens voor de Data 0 is de laagste; wordt gemaakt zelfs als andere netwerkinterfaces ingeschakeld voor de cloud zijn, wordt er daarom alle cloudverkeer van het apparaat worden gerouteerd via Data 0. |Update 1 |Ja |Ja |
| 10 |Back-ups |Een bug in Update 1, waardoor de back-ups mislukken na 24 dagen is, opgelost in de patch-versie 1.1 Update. |1.1 bijwerken |Ja |Ja |
| 11 |Back-ups |Een bug in eerdere versies leidde tot slechte prestaties voor cloudmomentopnamen met weinig wijzigingen tarieven. Deze fout is opgelost in deze release van de patch. |Update 1.2 |Ja |Ja |
| 12 |Updates |Een bug in Update 1 die een mislukte upgrade gemeld en de domeincontrollers om te gaan in de herstelmodus, veroorzaakt is, opgelost in deze release van de patch. |Update 1.2 |Ja |Ja |

## <a name="known-issues-in-update-12"></a>Bekende problemen in Update 1.2
De volgende tabel bevat een overzicht van de bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat zijn verbroken, wat resulteert in geen quorum schijf is vervolgens de opslaggroep offline. Dit wordt offline blijven, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, kunt u contact op met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Wanneer een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de installatiekopie wordt geladen vanuit het peer-knooppunt kan de besturings-ID weergegeven in eerste instantie als de peer-controller-ID. In zeldzame gevallen kan dit gedrag ook worden weergegeven na het systeem opnieuw is opgestart. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de netwerkcontroller te vervangen voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Met de Storage-service voor het verwijderen van het opslagaccount is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. |Ja |Ja | |
| 4 |Failover van het apparaat |Meerdere failovers van een volumecontainer van het bronapparaat dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van het apparaat vanaf een apparaat dat één dead op meerdere apparaten wordt de volumecontainers van het eerste apparaat een failover geen eigenaar van gegevens maken. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u ze in de klassieke Azure portal weergeven. | |Ja |Nee |
| 5 |Installatie |Tijdens de StorSimple-Adapter voor SharePoint-installatie moet u voor een IP-adres van het apparaat in de volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de apparaat-naar-servicecommunicatie worden beïnvloed en het apparaat gaat offline. Voor ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de URL van proxy HTTP als het opgegeven protocol heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureren en webproxy op een geregistreerd apparaat inschakelt, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de apparaatvolumes gaat u in een gedegradeerde status en de i/o's mislukken mogelijk vanwege een fout 'het apparaat is niet gereed'. |U moet handmatig opnieuw opstarten Apparaatcontrollers of uitvoeren van een failover van het apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Bij het gebruik van de StorSimple-cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - Wait** het eerste object selecteren, zodat u kunt een nieuwe maken **Upopslagaccount** object, de cmdlet retourneert alle objecten. |De cmdlet als volgt tussen haakjes verpakken: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - eerst 1 - Wait** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de verwachte Aankomsttijd voor de meest recente back-up nauwkeurige alleen voor de eerste volumecontainer. Bovendien begint parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer worden gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het herstel, zijn de volumes niet toegevoegd aan het back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet de 5000/7000-serie-apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijderen nadat de migratie voltooid en wordt doorgevoerd is. |Ja |Nee |
| 13 |Kloon en DR |Een StorSimple-apparaat met Update 1 kan klonen of uitvoeren van herstel na noodgevallen naar een apparaat met 1 software vóór het bijwerken. |U moet het doelapparaat bijwerken op 1 om toe te staan van deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Back-up van configuratie voor de migratie mislukken op een apparaat uit de 5000-7000 serie wanneer er volumegroepen met geen gekoppelde volumes. |Verwijderen van alle groepen van een leeg volume met geen gekoppelde volumes en voer vervolgens de configuratie van back-up opnieuw uit. |Ja |Nee |

## <a name="physical-device-updates-in-update-12"></a>Updates van fysiek apparaat in Update 1.2
Als patch update 1.2 wordt toegepast op een fysiek apparaat (versies vóór Update 1 wordt uitgevoerd), versie van de software wordt gewijzigd in 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller en firmware-updates in Update 1.2
Deze versie werkt het stuurprogramma en de schijffirmware op uw apparaat.

* Zie voor meer informatie over de update voor de controller SAS [Update 1 voor LSI SAS-controllers in Microsoft Azure StorSimple-apparaat](https://support.microsoft.com/kb/3043005). 
* Zie voor meer informatie over de update van de schijffirmware [schijffirmware Update 1 voor Microsoft Azure StorSimple-apparaat](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Updates van de virtuele apparaat in Update 1.2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-steps"></a>Volgende stappen
* [Update 1.2 installeren op uw apparaat](storsimple-install-update-1.md).

