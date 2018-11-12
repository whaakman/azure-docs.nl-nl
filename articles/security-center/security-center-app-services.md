---
title: Beveiligen van App-Services in Azure Security Center | Microsoft Docs
description: Dit artikel helpt u om te beginnen met het beveiligen van uw App-Services in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 062d3ce75372cd09e617fb984208542a31cb8e4a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019125"
---
# <a name="protect-app-service-with-azure-security-center"></a>Beveiligen van App Service met Azure Security Center
Dit artikel helpt u Azure Security Center gebruiken om te controleren en beveiligen van uw toepassingen toevoegen aan App Service.

App Service kunt u bouwen en hosten van webtoepassingen in de programmeertaal van uw keuze zonder dat infrastructuur worden beheerd. App Service biedt automatisch schalen en hoge beschikbaarheid, ondersteuning voor zowel Windows en Linux, evenals geautomatiseerde implementaties vanuit GitHub, Visual Studio Team Services of een gitopslagplaats. 

Door beveiligingslekken in webtoepassingen zijn vaak misbruikt door aanvallers, omdat ze een gemeenschappelijke en dynamische interface voor bijna elke organisatie op het Internet hebben. Aanvragen voor toepassingen die worden uitgevoerd op App Service gaat via verschillende gateways die zijn geïmplementeerd in Azure-datacenters overal ter wereld, die verantwoordelijk is voor de routering van elke aanvraag naar de desbetreffende toepassing. 

Azure Security Center kunt evaluaties en aanbevelingen voor uw toepassingen uitvoeren in App Service in de sandboxes geladen in uw virtuele machine of on-demand instanties uitvoeren. Door gebruik te maken van de zichtbaarheid met Azure als cloudprovider, analyseert Security Center uw interne App Service-Logboeken om te controleren op veelvoorkomende aanvallen van de web-app die vaak worden uitgevoerd in meerdere doelen.

Security Center maakt gebruik van de schaal van de cloud om te bepalen van aanvallen op uw App Service-toepassingen en richt u op nieuwe aanvallen, terwijl aanvallers in de reconnaissancefase, scannen voor het identificeren van kwetsbaarheden in meerdere websites, op Azure gehost worden. Security Center maakt gebruik van analyses en machine learning-modellen voor alle interfaces zodat klanten om te communiceren met hun toepassingen, of via HTTP of via een beheermethoden. Bovendien, als een eigen service in Azure Security Center is ook in een unieke positie te bieden op basis van een host beveiligingsanalyses die betrekking hebben op de onderliggende rekenknooppunten voor deze PaaS, het inschakelen van Security Center voor het detecteren van aanvallen tegen de webtoepassingen die zijn al misbruikt.

## <a name="prerequisites"></a>Vereisten

Als u wilt controleren en beveiligen van uw App Service, moet u een App Service-plan dat is gekoppeld aan toegewezen machines hebben. Deze abonnementen zijn: Basic, Standard, Premium, geïsoleerd of Linux. Azure Security Center biedt geen ondersteuning voor de abonnementen gratis, gedeeld of verbruik. Zie voor meer informatie, [App Service-abonnementen](https://azure.microsoft.com/pricing/details/app-service/plans/).

## <a name="security-center-protection"></a>Security Center-beveiliging

Azure Security Center beveiligt de VM-exemplaar waarin uw App Service wordt uitgevoerd en de beheerinterface. Bewaakt ook aanvragen en antwoorden naar en van uw toepassingen die worden uitgevoerd in App Service.

Security Center is geïntegreerd met App Service, hoeft u voor de implementatie en het onboarding - integratie is volledig transparant.



## <a name="enabling-monitoring-and-protection-of-app-service"></a>Inschakelen van controle en beveiliging van App Service

1. Kies in Azure Security Center.
2. Ga naar **beveiligingsbeleid** en kies een abonnement.
3. Aan het einde van de rij van het abonnement, klikt u op **instellingen bewerken**.
4. Onder **prijscategorie**, in de **appservice** rij, uw wilt in-/ uitschakelen **ingeschakeld**.

![App service in-/ uitschakelen](./media/security-center-app-services/app-services-toggle.png)

>[!NOTE]
> Het aantal exemplaren die worden vermeld voor uw quantity Resource geeft het aantal relevante exemplaren van App-service actief is op het moment waarop u de prijzen laag-blade geopend. Omdat dit nummer kan worden gewijzigd op basis van de schaal opties die u hebt geselecteerd, wordt het aantal exemplaren dat in rekening worden gebracht dienovereenkomstig worden gewijzigd.

Herhaal dit proces en in-/ uitschakelen als wilt uitschakelen bewaking en aanbevelingen voor uw App Service, uw **App Service** wilt **uitgeschakelde**.



## <a name="see-also"></a>Zie ook
In dit artikel hebt u kunnen lezen hoe u de bewakingsmogelijkheden in Azure Security Center gebruikt. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsinstellingen configureert in Azure Security Center.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
