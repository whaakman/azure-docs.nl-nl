---
title: StorSimple 8000 Update 0,2 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies en oplossingen, open problemen en oplossingen beschikbaar voor de januari 2015 Microsoft Azure StorSimple versie (Update 0.2).
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 2fc50f7faddb4b61ea5e7d328469fc3cc0eb6f3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 serie Update 0,2 release-opmerkingen - januari 2015
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen problemen met de kritieke openen voor de januari 2015-release van Microsoft Azure StorSimple. Ze bevatten ook een lijst van de StorSimple-software en -firmware-updates opgenomen in deze release. Dit is de tweede versie nadat de StorSimple 8000 Series versie algemeen beschikbaar is in juli 2014 is gedaan.

De versie van de software fysiek apparaat van de update van oktober wordt niet door deze update worden gewijzigd. Deze blijft versie 6.3.9600.17312. De afbeelding die wordt gebruikt door de installatiekopie van het virtuele apparaat is gewijzigd in deze release. Daarom alle nieuwe virtuele apparaten die zijn gemaakt nadat 1/20/2015 wordt de versie als 6.3.9600.17361 weergegeven.  

Controleer de volgende gegevens in de release-opmerkingen voor de update van januari 2015.

> [!IMPORTANT]
> * Deze update is niet beschikbaar via Windows Update en net als bij andere updates kan niet worden geïnstalleerd. Uw apparaat ontvangt geen deze update, zelfs als u de updates hebt toegepast met behulp van de klassieke Azure portal. Deze update is alleen van toepassing op virtuele apparaten die zijn gemaakt na 20 januari 2015. 
> * De release januari van StorSimple bevat geen updates op het fysieke StorSimple-apparaat. U kunt nog steeds beschikbare Windows-updates toepassen op het virtuele apparaat, met inbegrip van recente beveiligingsproblemen, maar niet ziet u een wijziging in de versie voor het fysieke StorSimple-apparaat.
> 
> 

## <a name="whats-new-in-the-january-release"></a>Wat is er nieuw in versie januari
Deze update bevat een oplossing die betrekking hebben op de volumes gaat offline op het virtuele apparaat. (Zie [problemen opgelost in deze release](#issues-fixed-in-the-january-release).)  

De update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-january-release"></a>Problemen in de release januari opgelost
De volgende tabel worden beschreven het probleem dat met deze update is verholpen.

| Nee. | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Volumes gaat offline |Als hoge cloud latenties zich blijven enkele minuten duren voordoen, gaat de StorSimple-volumes virtueel apparaat offline op de hosts. Deze oplossing verhoogt de drempelwaarde voor cloud latenties, waardoor het minimaliseert de situaties waardoor de volumes offline te gaan op hosts. |Nee |Ja |

## <a name="known-issues-in-the-january-release"></a>Bekende problemen in de release januari
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrieksinstellingen terugzetten |In sommige gevallen, wanneer u de fabrieksinstellingen terug te zetten, uitvoert het StorSimple-apparaat kan blijven steken en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8).** Dit gebeurt als u op CTRL + C drukt, terwijl de cmdlet uitgevoerd wordt. |Druk CTRL + C niet op na het initiëren van de fabrieksinstellingen terug te zetten. Als u al in deze staat, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf vervolgens de opslaggroep worden offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 3 |Cloud momentopname fouten |In zeldzame gevallen wordt een cloudmomentopname van een kan mislukken met de fout **back-up maximumlimiet bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | |Ja |Ja |
| 4 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID.  In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 5 |Controle van grafieken van apparaten |In de StorSimple Manager-service de bewaking grafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. |Wijzig de configuratie van de web-proxy voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat de verificatie is ingesteld op NONE. U kunt dit doen het de Windows PowerShell voor StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 7 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. |Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. |Ja |Nee |
| 8 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 9 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Zie voor meer informatie over het [webproxy voor uw apparaat configureren](storsimple-configure-web-proxy.md). |Ja |Nee |
| 10 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 11 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |

## <a name="physical-device-updates-in-the-january-release"></a>Fysiek apparaatupdates in de januari release
Deze update bevat geen andere wijzigingen aan het StorSimple-apparaat.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Serial attached SCSI (SAS)-controller en firmware-updates in de januari release
Deze release bevat geen updates op de controller serial attached SCSI (SAS) of de firmware. De stuurprogramma-update is in de oktober 2014-release. 

## <a name="virtual-device-updates-in-the-january-release"></a>Virtueel apparaatupdates in de januari release
Deze release bevat een bijgewerkte installatiekopie voor het virtuele apparaat. Alle virtuele apparaten die zijn gemaakt na 20 januari 2015 wordt de softwareversie weergegeven als 6.3.9600.17361.

