---
title: App-Service op Azure Stack-Update een | Microsoft Docs
description: Meer informatie over wat er in update voor App-Service op Azure-Stack de bekende problemen en waar u de update te downloaden.
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 0c33c8fdefbb27ba8414e58bed1b42ee7aaba88a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="app-service-on-azure-stack-update-one-release-notes"></a>App-Service op Azure Stack bijwerken een Release-opmerkingen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en oplossingen in Azure App Service op Azure Stack Update 1 en bekende problemen. Bekende problemen worden onderverdeeld in problemen met het rechtstreeks verband houden met de implementatie, bijwerken en problemen met de build (na de installatie).

> [!IMPORTANT]
> De update 1802 toepassen op uw Azure-Stack geïntegreerd systeem of de nieuwste Azure-Stack development kit implementeren voordat u Azure App Service implementeert.
>
>

## <a name="build-reference"></a>Verwijzing bouwen

De App op Azure Stack Update 1 build-nummer is **69.0.13698.9**

### <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Azure App Service op Azure-Stack is nu vereist een [drie onderwerp jokertekencertificaat](azure-stack-app-service-before-you-get-started.md#get-certificates) vanwege verbeteringen in de manier waarop SSO voor Kudu nu in Azure App Service wordt verwerkt.  Nieuw onderwerp is ** *. sso.appservice.<region>. <domainname>.<extension>**
>
>

Raadpleeg de [documentatie voordat u aan de slag](azure-stack-app-service-before-you-get-started.md) vóór de implementatie.

### <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen

Azure App Service op Azure Stack Update 1 bevat de volgende verbeteringen en oplossingen:

- **Hoge beschikbaarheid van Azure App Service** -fault-domeinen voor de Azure-Stack 1802 update ingeschakeld workloads die moeten worden geïmplementeerd.  App Service-infrastructuur is daarom kunnen worden fouttolerant zoals deze wordt geïmplementeerd in domeinen met fouten.  Standaard alle nieuwe implementaties van Azure App Service heeft deze mogelijkheid echter voor implementaties voltooien voordat Azure Stack 1802 update wordt toegepast naar verwijzen de [Foutdomein van App Service-documentatie](azure-stack-app-service-fault-domain-update.md)

- **Implementeren in een bestaand virtueel netwerk** -klanten kunnen nu op Azure-Stack-App Service implementeren in een bestaand virtueel netwerk.  Implementatie in een bestaand virtueel netwerk, kunnen klanten verbinding maken met de SQL Server en bestandsserver, vereist voor Azure App Service via particuliere poorten.  Tijdens de implementatie van klanten kunnen selecteren om te implementeren in een bestaand virtueel netwerk echter [moet maken voor gebruik door App Service-subnetten](azure-stack-app-service-before-you-get-started.md#virtual-network) voorafgaand aan de implementatie.

- Updates voor de **App Service-Tenant, Admin, portals van de functies en hulpprogramma's voor Kudu**.  Consistent zijn met Azure Stack Portal SDK-versie.

- **Updates voor de volgende toepassingsframeworks en hulpprogramma's**:
    - Toegevoegd **.Net Core 2.0** ondersteunen
    - Toegevoegd **Node.JS** versies:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Toegevoegd **NPM** versies:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Toegevoegd **PHP** Updates:
        - 5.6.32
        - 7.0.26 (x86- en x64)
        - 7.1.12 (x86- en x64)
    - Bijgewerkt **Git voor Windows** v 2.14.1
    - Bijgewerkt **volgt** naar v4.5.0

  - Ondersteuning toegevoegd voor **alleen HTTPS** functie binnen de functie van aangepaste domeinen in de Portal-App Service-Tenant. 

  - Toegevoegde validatie van de opslagverbinding in de kiezer voor aangepaste opslag voor Azure Functions 

#### <a name="fixes"></a>Oplossingen

- Bij het maken van een offline-implementatiepakket ontvangt klanten de toegang is geweigerd foutbericht bij het openen van de map van het App Service-installatieprogramma niet langer

- Problemen opgelost als u werkt in de functie aangepaste domeinen in de Portal-App Service-Tenant.

- Voorkomen dat klanten die gebruikmaken van de beheerder van de gereserveerde namen tijdens de installatie

- App Service-implementatie met ingeschakeld **verbonden met het domein** bestandsserver

- Verbeterde ophalen van Azure Stack hoofdmap van het certificaat in het script en nu het root-certificaat in het installatieprogramma van App Service te valideren.

- Vaste onjuiste status in Azure Resource Manager worden geretourneerd wanneer een abonnement is verwijderd opgenomen in de naamruimte Microsoft.Web resources.

### <a name="known-issues-with-the-deployment-process"></a>Bekende problemen met het implementatieproces

- Er zijn geen bekende problemen voor de implementatie van Azure App Service op Azure Stack Update 1.

### <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren

- Er zijn geen bekende problemen voor het bijwerken van Azure App Service op Azure Stack Update 1.

### <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

- Er zijn geen bekende problemen voor de installatie van Azure App Service op Azure Stack Update 1.

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekende problemen voor Cloud-beheerders werking van Azure App Service op Azure-Stack

Raadpleeg de documentatie in de [Azure Stack 1802 Release-opmerkingen](azure-stack-update-1802.md)

## <a name="see-also"></a>Zie ook

- Zie voor een overzicht van Azure App Service [Azure App Service op Azure-Stack overzicht](azure-stack-app-service-overview.md).
- Zie voor meer informatie over het voorbereiden voor het implementeren van App-Service op Azure-Stack [voordat u aan de slag met App-Service op Azure-Stack](azure-stack-app-service-before-you-get-started.md).
