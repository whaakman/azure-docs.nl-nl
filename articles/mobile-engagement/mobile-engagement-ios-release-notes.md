---
title: Azure Mobile Engagement iOS SDK-releaseopmerkingen | Microsoft Docs
description: Meest recente updates en procedures voor iOS SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 9bdaa57f9902373ccf796ff109332b64c66bf9e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Mobile Engagement iOS SDK-releaseopmerkingen

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Vaste badges uitgeschakeld op de achtergrond.
* Vaste waarschuwingen op 9 over API's niet aangeroepen in hoofdwachtrij XCode.
* Een geheugenlek vast Reach polls.
* Ondersteuning voor iOS verwijderd 6.X. Vanaf deze versie van het implementatiedoel van uw toepassing moet ten minste iOS 7.

## <a name="401-12132016"></a>4.0.1 (12/13/2016)
* Verbeterde levering van het logboek op de achtergrond.

## <a name="400-09122016"></a>4.0.0 (09/12/2016)
* Vaste melding niet worden ondernomen op iOS-10-apparaten.
* Afschaffen XCode 7.

## <a name="324-06302016"></a>3.2.4 (06/30/2016)
* Vaste aggregatie tussen technische logboeken en andere logboeken.

## <a name="323-06072016"></a>3.2.3 (06/07/2016)
* De fout vast waar de leveringsfeedback is niet gemeld wanneer de app op de achtergrond.
* Geoptimaliseerd voor het verzenden van technische Logboeken.

## <a name="322-04072016"></a>3.2.2 (04/07/2016)
* Vaste bug op HTTP-aanvraag annulering wat soms tot vastlopen leidt.

## <a name="321-12112015"></a>3.2.1 (12/11/2015)
* De vertraging opgelost als een nieuw exemplaar van de app wordt gegenereerd door een melding met dieptekoppelingen

## <a name="320-10082015"></a>3.2.0 (10/08/2015)
* Bitcode in de SDK zodat deze samenwerkt ingeschakeld **Xcode 7**.
* Verholpen fouten gerelateerd aan in-app-meldingen.
* De in-app-meldingen betrouwbaarder in geval van een laag energieniveau en andere dergelijke scenario's gemaakt.
* Extra console-logboeken die worden gegenereerd door 3e partij bibliotheek verwijderd.

## <a name="310-08262015"></a>3.1.0 (08/26/2015)
* Oplossingen voor iOS 9-compatibiliteit met een bibliotheek van derden oplossen. Het is crashes veroorzaakt tijdens het verzenden van resultaten, toepassingsinformatie of extra gegevens worden opgevraagd.

## <a name="300-06192015"></a>3.0.0 (06/19/2015)
* Mobile Engagement gebruikt achtergrond-Pushmeldingen.
* Ondersteuning voor iOS verwijderd 4.X. Vanaf deze versie van het implementatiedoel van uw toepassing moet ten minste iOS 6.

## <a name="220-05212015"></a>2.2.0 (05/21/2015)
* De Mobile Engagement apparaat-id voor apparaten < iOS 6 is nu gebaseerd op een GUID die wordt gegenereerd tijdens de installatie.

## <a name="210-04242015"></a>2.1.0 (04/24/2015)
* Toegevoegde Swift compatibiliteit.
* Wanneer de op een melding klikt, wordt door de actie-URL nu is rechts uitgevoerd nadat de toepassing is geopend.
* Toegevoegde ontbrekende headerbestand in de SDK-pakket.
* Een probleem opgelost wanneer de Mobile Engagement crash Rapportagefout is uitgeschakeld.

## <a name="200-02172015"></a>2.0.0 (02/17/2015)
* Initiële versie van Azure Mobile Engagement
* appId/sdkKey configuratie wordt vervangen door de configuratie van een verbinding-tekenreeks.
* API's voor het verzenden en ontvangen van berichten van willekeurige XMPP van willekeurige XMPP entiteiten verwijderd.
* API voor het verzenden en ontvangen van berichten tussen apparaten verwijderd.
* Verbeterde beveiliging.
* SmartAd bijhouden verwijderd.
