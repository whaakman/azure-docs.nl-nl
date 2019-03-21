---
title: Beveiligen van App-Services in Azure Security Center | Microsoft Docs
description: Dit artikel helpt u om te beginnen met het beveiligen van uw App-Services in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2019
ms.author: monhaber
ms.openlocfilehash: ea738535ae9326109a7c3fdd0b5d0c4f4691fdf0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57878627"
---
# <a name="protect-app-service-with-azure-security-center"></a>Beveiligen van App Service met Azure Security Center
Dit artikel helpt u Azure Security Center gebruiken om te controleren en beveiligen van uw toepassingen toevoegen aan App Service.

App Service kunt u bouwen en hosten van webtoepassingen in de programmeertaal van uw keuze zonder dat infrastructuur worden beheerd. App Service biedt automatisch schalen en hoge beschikbaarheid, ondersteuning voor zowel Windows en Linux, evenals geautomatiseerde implementaties vanuit GitHub, Azure DevOps of een gitopslagplaats. 

Door beveiligingslekken in webtoepassingen zijn vaak misbruikt door aanvallers, omdat ze een gemeenschappelijke en dynamische interface voor bijna elke organisatie op het Internet hebben. Aanvragen voor toepassingen die worden uitgevoerd op App Service gaat via verschillende gateways die zijn geïmplementeerd in Azure-datacenters overal ter wereld, die verantwoordelijk is voor de routering van elke aanvraag naar de desbetreffende toepassing. 

Azure Security Center kunt evaluaties en aanbevelingen voor uw toepassingen uitvoeren in App Service in de sandboxes geladen in uw virtuele machine of on-demand instanties uitvoeren. Door gebruik te maken van de zichtbaarheid met Azure als cloudprovider, analyseert Security Center uw interne App Service-Logboeken om te controleren op veelvoorkomende aanvallen van de web-app die vaak worden uitgevoerd in meerdere doelen.

Security Center maakt gebruik van de schaal van de cloud om te bepalen van aanvallen op uw App Service-toepassingen en richt u op nieuwe aanvallen, terwijl aanvallers in de reconnaissancefase, scannen voor het identificeren van kwetsbaarheden in meerdere websites, op Azure gehost worden. Security Center maakt gebruik van analyses en machine learning-modellen voor alle interfaces zodat klanten om te communiceren met hun toepassingen, of via HTTP of via een beheermethoden. Bovendien, als een eigen service in Azure Security Center is ook in een unieke positie te bieden op basis van een host beveiligingsanalyses die betrekking hebben op de onderliggende rekenknooppunten voor deze PaaS, het inschakelen van Security Center voor het detecteren van aanvallen tegen de webtoepassingen die zijn al misbruikt.

## <a name="prerequisites"></a>Vereisten

Als u wilt controleren en beveiligen van uw App Service, moet u een App Service-plan dat is gekoppeld aan toegewezen machines hebben. Deze abonnementen zijn: Basic, Standard, Premium, geïsoleerd, of Linux. Azure Security Center biedt geen ondersteuning voor de abonnementen gratis, gedeeld of verbruik. Zie voor meer informatie, [App Service-abonnementen](https://azure.microsoft.com/pricing/details/app-service/plans/).

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

* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md): Leer hoe u beveiligingsinstellingen configureert in Azure Security Center.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [App-services](security-center-virtual-machine-protection.md#app-services):  Een lijst van uw App service-omgevingen met samenvattingen van de status weergeven.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.
