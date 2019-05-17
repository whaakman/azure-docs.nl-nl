---
title: 'Azure AD-toepassingsproxy: Versiegeschiedenis van release | Microsoft Docs'
description: In dit artikel geeft een lijst van alle versies van Azure AD-toepassingsproxy en een beschrijving van nieuwe functies en opgeloste problemen
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
ms.subservice: manage-apps
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7af05769861f98ec192789d90f8a61f5b6638d1d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65783289"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure AD-toepassingsproxy: Releasegeschiedenis van versie
In dit artikel bevat de versies en functies van de toepassingsproxy van Azure Active Directory (Azure AD) die zijn uitgebracht. Het team van Azure AD werkt Application Proxy regelmatig bij met nieuwe functies en functionaliteit. Toepassingsproxyconnectors worden automatisch bijgewerkt wanneer er een nieuwe versie wordt uitgebracht.

Hier volgt een lijst van gerelateerde resources:

Resource |  Details
--------- | --------- |
Toepassingsproxy inschakelen | Vereisten voor toepassingsproxy inschakelen, installeren en registreren van een connector worden beschreven in deze [zelfstudie](application-proxy-add-on-premises-application.md).
Meer informatie over Azure AD Application Proxy connectors | Meer informatie over [connector management](application-proxy-connectors.md) en hoe connectors [automatische clientupdate](application-proxy-connectors.md#automatic-updates).
Azure AD Application Proxy Connector downloaden |  [Download de meest recente connector](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download).

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Releasestatus

20 september 2018: Die zijn uitgebracht voor downloaden

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

- WebSocket-ondersteuning toegevoegd voor de toepassing QlikSense. Zie voor meer informatie over het integreren van QlikSense met Application Proxy, dit [scenario](application-proxy-qlik.md). 
- Verbeterde de installatiewizard om gemakkelijker een uitgaande proxy configureren. 
- TLS 1.2 ingesteld als het standaard-protocol voor connectors. 
- Een nieuwe gebruiksrechtovereenkomst (EULA) toegevoegd.  

### <a name="fixed-issues"></a>Problemen opgelost

- Een opgelost waardoor sommige geheugenlekken in de connector.
- De versie van het Azure Service Bus, waaronder een opgelost probleem voor de time-outproblemen connector bijgewerkt.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Releasestatus

19 januari 2018: Die zijn uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost

- Ondersteuning toegevoegd voor aangepaste domeinen dat domein vertaling in de cookie nodig.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Releasestatus 

25 mei 2017: Die zijn uitgebracht voor downloaden 

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen 

Verbeterde controle over de limieten voor de uitgaande verbindingen connectors. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Releasestatus

15 april 2017: Die zijn uitgebracht voor downloaden

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

- Vereenvoudigde onboarding- en -beheer met minder vereiste poorten. Application Proxy is het nu vereist slechts twee standaard uitgaande poorten openen: 443 en 80. Toepassingsproxy blijft alleen uitgaande verbindingen gebruiken, zodat u nog steeds niet alle onderdelen in een perimeternetwerk bevindt hoeft. Zie voor meer informatie onze [configuratiedocumentatie](application-proxy-add-on-premises-application.md).  
- Als dit wordt ondersteund door uw externe proxy of firewall, kunt u nu uw netwerk door DNS in plaats van IP-adresbereik openen. Proxy-toepassingsservices vereisen verbindingen met *. msappproxy.net en *. servicebus.windows.net alleen.


## <a name="earlier-versions"></a>Eerdere versies

Als u een Application Proxy connector-versie ouder is dan 1.5.36.0, bijwerken naar de nieuwste versie om ervoor te zorgen dat u de meest recente functies volledig wordt ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [extern toegang tot on-premises toepassingen via Azure AD-toepassingsproxy](application-proxy.md).
- Als u wilt gaan met Application Proxy, Zie [zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via toepassingsproxy](application-proxy-add-on-premises-application.md).
