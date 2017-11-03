---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: c179c39a43da0aa35e945acceacbf27fe8e328f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="release-notes"></a>Releaseopmerkingen

## <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Herstel van een crash die zelden optreden kan bij het aanroepen van `EngagementAgentUtils.isInDedicatedEngagementProcess`, die ook wordt gebruikt door de `EngagementApplication` klasse.

## <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8-ondersteuning (vorige versies van de SDK werken niet in Android 8).
* Er is geen meer afhankelijkheid van ondersteuningsbibliotheek.
* Verwijder `EngagementFragmentActivity` klasse.
* Vanwege [achtergrond uitvoering limieten](https://developer.android.com/preview/features/background.html) op Android 8 logboeken op de achtergrond kunnen worden uitgesteld totdat de gebruiker werkt met het apparaat, wordt dit een invloed hebben op campagne Push **geleverd** en **Systeemmelding weergegeven** statistieken wordt uitgesteld als de slaapstand van het apparaat (de melding wordt nog steeds worden weergegeven, wordt ring en Trillen in realtime zonder problemen).
* Vanwege [achtergrond locatie limieten](https://developer.android.com/preview/features/background-location-limits.html), de locatie op de achtergrond wordt niet regelmatig bijgewerkt op Android 8 realtime.

## <a name="424-03302017"></a>4.2.4 (03/30/2017)
* Los in app-melding tekstkleur op Android 7 moet hetzelfde zijn als oudere versies van Android.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Er is geen meer Wi-Fi-vergrendeling.
* Herstel een impasse aangetroffen bij het aanroepen van getDeviceId voordat init (fout die is geïntroduceerd in 4.2.0).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)
* Verbeteringen in stabiliteit.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)
* Beveiliging: uitschakelen webtoegang weergave lokaal bestand.
* Beveiliging: Verwijder `EngagementPreferenceActivity` klasse die uitgebreider is verouderd en niet-beveiligde `PreferenceActivity` klasse.
* Beveiliging: reach activiteiten nu beschreven worden voor het gebruik van `exported="false"`, deze vlag kan ook worden gebruikt in eerdere versies van de SDK.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* De SDK is nu een licentie verleend onder MIT.
* Toestaan dat een aangepast apparaat-id opgeven bij het initialiseren SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)
* Verbeteringen in stabiliteit.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)
* Verbeteringen in stabiliteit.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)
* Verbeteringen in stabiliteit.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)
* Verbeteringen in stabiliteit.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)
* Verbeteringen in stabiliteit.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)
* Nieuw model voor machtiging verwerken voor Android M.
* Kunnen nu functies configureren voor locatie tijdens runtime in plaats van `AndroidManifest.xml`.
* Een machtiging fout oplossen: als u `ACCESS_FINE_LOCATION`, klikt u vervolgens `ACCESS_COARSE_LOCATION` niet meer nodig is.
* Verbeteringen in stabiliteit.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)
* Interne protocol wijzigingen aanbrengen analyses en pushmeldingen betrouwbaarder.
* Native pushberichten (GCM/ADM) is nu ook gebruikt voor in de app-meldingen dus moet u de referenties van native pushberichten voor elk type pushcampagne configureren.
* Grote afbeelding melding oplossen: ze zijn weergegeven alleen per 10 na wordt gepusht.
* Herstel van een fout in de weergave: het standaard actie-URL is ook uitvoeren op een koppeling te klikken.
* Herstel een zeldzame crash die betrekking hebben op het beheer van lokale opslag.
* Los de dynamische configuratie tekenreeks management.
* Bijwerken van de gebruiksrechtovereenkomst.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)
* Initiële versie van Azure Mobile Engagement
* appId-configuratie wordt vervangen door de configuratie van een verbinding-tekenreeks.
* API's voor het verzenden en ontvangen van berichten van willekeurige XMPP van willekeurige XMPP entiteiten verwijderd.
* API voor het verzenden en ontvangen van berichten tussen apparaten verwijderd.
* Verbeterde beveiliging.
* Google Play en SmartAd bijhouden verwijderd.

