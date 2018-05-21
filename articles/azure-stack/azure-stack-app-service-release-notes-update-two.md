---
title: App-Service op Azure-Stack update 2 release-opmerkingen | Microsoft Docs
description: Meer informatie over wat er in de update twee voor App-Service op Azure-Stack, de bekende problemen en waar u de update te downloaden.
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
ms.openlocfilehash: 8e1790b7d0b3a210a9142fc8580ff8ed4d64311c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>App-Service op de opmerkingen bij de release van de Azure-Stack-update 2

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 2 en bekende problemen. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met de implementatie, bijwerken en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1804 toepassen op uw Azure-Stack geïntegreerd systeem of de nieuwste Azure-Stack development kit implementeren voordat u Azure App Service 1.2 implementeert.
>
>

## <a name="build-reference"></a>Verwijzing bouwen

De App op Azure Stack Update 2 build-nummer is **72.0.13698.10**

### <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Nieuwe implementaties van Azure App Service op Azure-Stack moet nu een [drie onderwerp jokertekencertificaat](azure-stack-app-service-before-you-get-started.md#get-certificates) vanwege verbeteringen in de manier waarop SSO voor Kudu nu in Azure App Service wordt verwerkt. Nieuw onderwerp is  **\*. sso.appservice.\< regio\>.\< DOMAINNAME\>.\< de extensie\>**
>
>

Raadpleeg de [documentatie voordat u aan de slag](azure-stack-app-service-before-you-get-started.md) vóór de implementatie.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Op Azure Stack Update 2-Azure App Service bevat de volgende verbeteringen en oplossingen:

- Updates voor de **App Service-Tenant, Admin, portals van de functies en hulpprogramma's voor Kudu**. Consistent zijn met Azure Stack Portal SDK-versie.

- Updates voor de Kernservice fout messaging inschakelen eenvoudiger diagnose van veelvoorkomende problemen en de betrouwbaarheid te verbeteren.

- **Updates voor de volgende toepassingsframeworks en hulpprogramma's**:
  - Toegevoegde .net Framework 4.7.1
  - Toegevoegd **Node.JS** versies:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - Toegevoegd **NPM** versies:
    - 5.6.0
  - .Net bijgewerkt kernonderdelen consistent zijn met Azure App Service in de openbare cloud.
  - Bijgewerkte Kudu

- Automatisch wisselen van de implementatie van de functie ingeschakeld - sleuven [configureren voor automatisch wisselen](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing#configure-auto-swap)

- Testen in productie-functie ingeschakeld - [Inleiding tot testen in productie](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

- Azure Functions-proxy's ingeschakeld - [werken met Azure Functions-proxy's](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies)

- App Service-beheerder extensie UX ondersteuning toegevoegd voor:
  - Geheime draaien
  - Certificaat draaien
  - Systeem referentie draaien
  - Verbinding tekenreeks draaien

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

- Werknemers hebben geen toegang heeft tot bestandsserver wanneer de App Service is geïmplementeerd in een bestaand virtueel netwerk en de bestandsserver is alleen beschikbaar op het particuliere netwerk.

Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met uw bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen het subnet van de werknemer en de bestandsserver aan te geven. Hiervoor gaat u naar de WorkersNsg in het beheerportal en een uitgaande beveiligingsregel met de volgende eigenschappen toevoegen:
 * Bron:
 * Poortbereik van bron: *
 * Bestemming: IP-adressen
 * Doel-IP-adresbereik: bereik van IP-adressen voor uw bestandsserver
 * Poortbereik van doel: 445
 * Protocol: TCP
 * Actie: toestaan
 * Prioriteit: 700
 * Naam: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders werking van Azure App Service op Azure-Stack

Raadpleeg de documentatie in de [Azure Stack 1804 Release-opmerkingen](azure-stack-update-1804.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure App Service [Azure App Service op Azure-Stack overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden voor het implementeren van App-Service op Azure-Stack [voordat u aan de slag met App-Service op Azure-Stack](azure-stack-app-service-before-you-get-started.md).
