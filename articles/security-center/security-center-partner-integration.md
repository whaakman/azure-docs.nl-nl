---
title: Partnerintegratie in Azure Security Center | Microsoft Docs
description: "In dit document wordt uitgelegd hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging van uw Azure-resources te verbeteren."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 095b5c9d1a888a4061450234f80c52c5834fbf53
ms.lasthandoff: 12/02/2016


---
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegratie in Azure Security Center
In dit document wordt uitgelegd hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging te verbeteren en een geïntegreerde ervaring in Azure te bieden. Daarnaast kan Azure Marketplace worden gebruikt voor partnercertificering en facturering.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Waarom partneroplossingen met Security Center integreren?

Dit zijn de vier belangrijkste redenen voor partnerintegratie in Security Center:

- **Eenvoudige implementatie**: het is veel gemakkelijker om een partneroplossing te implementeren door de aanbeveling van Security Center te volgen. Het implementatieproces kan volledig worden geautomatiseerd met een standaardconfiguratie en -netwerktopologie. Klanten kunnen ook kiezen voor een semi-automatische optie die meer flexibiliteit en aanpasmogelijkheden bij de configuratie biedt.
- **Geïntegreerde detectie**: beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde statuscontrole en beheer**: met geïntegreerde statusgebeurtenissen kunnen klanten alle partneroplossingen in één oogopslag controleren. U beschikt over eenvoudige beheermogelijkheden en hebt toegang tot geavanceerde configuratieopties met behulp van de partneroplossing.
- **Exporteren naar SIEM**: klanten kunnen nu alle meldingen van Security Center en partners in CEF-indeling exporteren naar on-premises SIEM-systemen met behulp van Microsoft Azure-logboekintegratie (preview).


## <a name="what-partners-are-integrated-with-security-center"></a>Welke partners zijn geïntegreerd met Security Center?
Security Center is momenteel geïntegreerd met de volgende partners:

- Endpoint Protection (Trend Micro), 
- Web Application Firewall (Barracuda, F5 en Imperva, en binnenkort ook Microsoft WAF en Fortinet), 
- Next Generation Firewall (Check Point en Barracuda, en binnenkort ook Fortinet en Cisco). 
- Vulnerability Assessment (Qualys - preview). 

Het aantal partners in de bestaande categorieën in Security Center zal geleidelijk worden uitgebreid en er zullen nieuwe categorieën worden toegevoegd. 

## <a name="how-to-deploy-a-partner-solution"></a>Een partneroplossing implementeren

Security Center kan aanraden om een partneroplossing te implementeren op basis van de configuratie van uw Azure-omgeving en het gedefinieerde beveiligingsbeleid. In de aanbeveling wordt u door het selecteren en installeren van een partneroplossing geleid. Het implementatieproces kan op dit punt per soort oplossing en partner verschillen. Raadpleeg de volgende koppelingen voor meer informatie:

- [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md)
- [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md)
- [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md)
- [Beoordeling van beveiligingslekken is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md)

## <a name="how-to-manage-partner-solutions"></a>Hoe beheer ik partneroplossingen?

Zodra een partneroplossing is geïmplementeerd, kunt u informatie over de status van de oplossing bekijken en basisbeheertaken uitvoeren vanuit de tegel Partneroplossing in het hoofddashboard van Security Center. Lees [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md) voor meer informatie over het beheren van partneroplossingen in Security Center.

![Partnerintegratie](./media/security-center-partner-integration/security-center-partner-integration-fig1-new.png)


## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u een partneroplossing integreert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md)
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

