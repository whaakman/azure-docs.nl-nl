---
title: App Service in Azure Stack update 2 opmerkingen bij de release | Microsoft Docs
description: Meer informatie over wat er in de update 2 voor App Service in Azure Stack, de bekende problemen en het downloaden van de update.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: be09773a1ce3e80547d9e5f0e9de2a2d9e093c60
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970914"
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>App Service op de opmerkingen bij de release van de Azure Stack update 2

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 2 en bekende problemen. Bekende problemen zijn onderverdeeld in problemen direct verband houden met de implementatie, het updateproces en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1804 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.2 implementeert.
>
>

## <a name="build-reference"></a>Naslaginformatie over bouwen

De App Service op Azure Stack Update 2 build-nummer is **72.0.13698.10**

### <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Nieuwe implementaties van Azure App Service in Azure Stack nu vereisen een [drie-onderwerp jokertekencertificaat](azure-stack-app-service-before-you-get-started.md#get-certificates) door verbeteringen in de manier waarop SSO voor Kudu nu wordt uitgevoerd in Azure App Service. Nieuw onderwerp is  **\*. sso.appservice.\< regio\>.\< domeinnaam\>.\< extensie\>**
>
>

Raadpleeg de [voordat u aan de slag-documentatie](azure-stack-app-service-before-you-get-started.md) voordat u begint met implementeren.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Azure App Service op Azure Stack Update 2 omvat de volgende verbeteringen en oplossingen:

- Updates voor **App Service-Tenant, Admin, portals van functies en hulpprogramma's voor Kudu**. Consistent zijn met Azure Stack-Portal SDK-versie.

- Updates voor core-service om de betrouwbaarheid en foutberichten eenvoudiger diagnose van algemene problemen waardoor te verbeteren.

- **Updates voor de volgende App-frameworks en hulpmiddelen**:
  - Toegevoegde .net Framework 4.7.1
  - Toegevoegd **Node.JS** versies:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - Toegevoegd **NPM** versies:
    - 5.6.0
  - Bijgewerkt .net Core-onderdelen moet consistent zijn met Azure App Service in de openbare cloud.
  - Bijgewerkte Kudu

- Automatisch wisselen van de implementatie van de functie is ingeschakeld - sleuven [configureren voor automatisch wisselen](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing#configure-auto-swap)

- Testen in productie-functie ingeschakeld - [Inleiding tot testen in productie](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

- Azure Functions-proxy's ingeschakeld - [werken met Azure Functions-proxy's](https://docs.microsoft.com/azure/azure-functions/functions-proxies)

- App Service-beheerextensie UX-ondersteuning toegevoegd voor:
  - Geheime draaien
  - Certificaatrotatie van
  - Rotatie van systeem-referentie
  - Tekenreeks verbindingsrotatie

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

- Werknemers kunnen geen bestandsserver bereiken wanneer App Service is geïmplementeerd in een bestaand virtueel netwerk en de bestandsserver alleen beschikbaar op het particuliere netwerk is.

Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen de worker-subnet en de bestandsserver aan te geven. Om dit te doen, gaat u naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen toe:
 * Bron:
 * Poortbereik van bron: *
 * Bestemming: IP-adressen
 * Doel-IP-adresbereik: bereik van IP-adressen voor uw bestandsserver
 * Poortbereik van doel: 445
 * Protocol: TCP
 * Actie: toestaan
 * Prioriteit: 700
 * Naam: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure App Service in Azure Stack

Raadpleeg de documentatie in de [opmerkingen bij de Release van Azure Stack 1804](azure-stack-update-1804.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure App Service, [Azure App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden van het App Service implementeren in Azure Stack [voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
