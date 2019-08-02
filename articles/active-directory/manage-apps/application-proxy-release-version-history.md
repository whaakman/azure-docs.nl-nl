---
title: 'Azure AD-toepassingsproxy: Release geschiedenis van versie | Microsoft Docs'
description: In dit artikel wordt een overzicht gegeven van alle versies van Azure AD-toepassingsproxy en worden nieuwe functies en opgeloste problemen beschreven
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.subservice: app-mgmt
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9ecd193282ed9b7333df44689530b4d057ad7f8
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693904"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure AD-toepassingsproxy: Releasegeschiedenis van versie
In dit artikel vindt u een overzicht van de versies en functies van de toepassings proxy van Azure Active Directory (Azure AD) die is uitgebracht. Het Azure AD-team werkt de toepassings proxy regel matig bij met nieuwe functies en functionaliteit. Application proxy-connectors worden automatisch bijgewerkt wanneer een nieuwe versie wordt uitgebracht. 

We raden u aan om ervoor te zorgen dat automatische updates zijn ingeschakeld voor uw connectors om ervoor te zorgen dat u beschikt over de nieuwste functies en oplossingen voor fouten. Micro soft biedt direct ondersteuning voor de recentste connector versie en één versie eerder.

Hier volgt een lijst met verwante resources:

Resource |  Details
--------- | --------- |
Toepassings proxy inschakelen | De vereisten voor het inschakelen van toepassings proxy en het installeren en registreren van een connector worden beschreven in deze [zelf studie](application-proxy-add-on-premises-application.md).
Meer informatie over Azure AD Application Proxy connectors | Meer informatie over [connector beheer](application-proxy-connectors.md) en hoe connectors [automatisch worden bijgewerkt](application-proxy-connectors.md#automatic-updates).
Downloaden van Azure AD-toepassingsproxy-connector |  [Down load de nieuwste connector](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download).

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Release status

20 september 2018: Uitgebracht voor downloaden

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

- De ondersteuning voor websockets voor de QlikSense-toepassing is toegevoegd. Zie dit [overzicht](application-proxy-qlik.md)voor meer informatie over het integreren van QlikSense met toepassings proxy. 
- De installatie wizard is verbeterd om het configureren van een uitgaande proxy eenvoudiger te maken. 
- Stel TLS 1,2 in als het standaard protocol voor connectors. 
- Er is een nieuwe gebruiksrecht overeenkomst voor eind gebruikers toegevoegd.  

### <a name="fixed-issues"></a>Problemen opgelost

- Er is een fout opgelost die een aantal geheugen lekkages in de connector heeft veroorzaakt.
- De Azure Service Bus versie is bijgewerkt, die een fout oplossing bevat voor problemen met de time-out van de connector.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Release status

19 januari 2018: Uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost

- Er is ondersteuning toegevoegd voor aangepaste domeinen die domein vertaling in de cookie nodig hebben.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Release status 

25 mei 2017: Uitgebracht voor downloaden 

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen 

Verbeterde controle over de uitgaande verbindings limieten van connectors. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Release status

15 april 2017: Uitgebracht voor downloaden

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

- Vereenvoudigd onboarding en beheer met minder vereiste poorten. Toepassings proxy vereist nu het openen van slechts twee standaard uitgaande poorten: 443 en 80. Toepassings proxy blijft alleen uitgaande verbindingen gebruiken, dus u hebt nog steeds geen onderdelen in een DMZ nodig. Zie onze [configuratie documentatie](application-proxy-add-on-premises-application.md)voor meer informatie.  
- Als dit wordt ondersteund door uw externe proxy of firewall, kunt u uw netwerk nu openen met DNS in plaats van met een IP-bereik. Voor Application proxy-services zijn alleen verbindingen met *. msappproxy.net en *. servicebus.windows.net vereist.


## <a name="earlier-versions"></a>Eerdere versies

Als u een versie van een toepassings proxy connector hebt die ouder is dan 1.5.36.0, werkt u bij naar de nieuwste versie om ervoor te zorgen dat u over de meest recente volledig ondersteunde functies beschikt.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [externe toegang tot on-premises toepassingen via Azure AD-toepassingsproxy](application-proxy.md).
- Zie [zelf studie: Voeg een on-premises toepassing toe voor externe toegang via](application-proxy-add-on-premises-application.md)toepassings proxy.
