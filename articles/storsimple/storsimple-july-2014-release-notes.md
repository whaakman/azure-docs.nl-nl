---
title: Opmerkingen bij de Release van StorSimple 8000 versie release | Microsoft Docs
description: Beschrijft de nieuwe functies, open problemen en oplossingen beschikbaar voor de juli 2014 Microsoft Azure StorSimple versie.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 303cdffa15fdfe9b83d0612edecafc6943d218f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Releaseopmerkingen voor StorSimple 8000 Series versie - juli 2014
## <a name="overview"></a>Overzicht
De volgende release-opmerkingen problemen met de kritieke openen voor de StorSimple 8000 serie juli 2014 algemene beschikbaarheid (GA) versie van Microsoft Azure StorSimple. Deze release correspondeert met softwareversie 6.3.9600.17215.  

Tenzij anders vermeld, worden deze releaseopmerkingen zijn van toepassing op alle modellen van het StorSimple-apparaat. De release-opmerkingen worden voortdurend bijgewerkt; Wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Overweeg de volgende informatie voordat u uw Microsoft Azure StorSimple-oplossing implementeert.  

## <a name="known-issues-in-this-release"></a>Bekende problemen in deze release
De volgende tabel bevat een samenvatting van bekende problemen in deze release.  

| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrieksinstellingen terugzetten |In sommige gevallen, wanneer u de fabrieksinstellingen terug te zetten, uitvoert het StorSimple-apparaat kan blijven steken en dit bericht weergegeven: **terugzetten op fabrieksinstellingen wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt, terwijl de cmdlet uitgevoerd wordt. |Druk CTRL + C niet op na het initiëren van de fabrieksinstellingen terug te zetten. Als u al in deze staat, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf vervolgens de opslaggroep worden offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 3 |Cloud momentopname fouten |In zeldzame gevallen wordt een cloudmomentopname van een kan mislukken met de fout **back-up maximumlimiet bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat uit hetzelfde oorspronkelijke volume dat is verwijderd. | |Ja |Ja |
| 4 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 5 |Controle van grafieken van apparaten |In de StorSimple Manager-service de bewaking grafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. |Wijzig de configuratie van de web-proxy voor het apparaat geregistreerd bij uw StorSimple Manager-service zodanig dat de verificatie is ingesteld op NONE. U kunt dit doen het de Windows PowerShell voor StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 7 |Failback |Een failback binnen 24 uur van noodherstel (DR) wordt niet ondersteund. | |Ja |Nee |
| 8 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 9 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u opgeven van een apparaat IP-adres voor de installatie met succes te voltooien. | |Ja |Nee |
| 10 |Netwerkinterfaces |Netwerkinterfaces DATA 2 en DATA 3 zijn in de software gewisseld. |Neem contact op met Microsoft Support als u nodig hebt voor het configureren van deze interfaces. |Ja |Nee |

