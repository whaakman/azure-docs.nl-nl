---
title: Partnerintegratie in Azure Security Center | Microsoft Docs
description: "In dit document wordt uitgelegd hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging van uw Azure-resources te verbeteren."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 85460ea37d251b53074de9d832610d253700f2c1
ms.contentlocale: nl-nl
ms.lasthandoff: 06/24/2017


---
<a id="partner-integration-in-azure-security-center" class="xliff"></a>

# Partnerintegratie in Azure Security Center
In dit document wordt uitgelegd hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging te verbeteren en een geïntegreerde ervaring in Azure te bieden. Daarnaast kan Azure Marketplace worden gebruikt voor partnercertificering en facturering.

>[!NOTE] 
>Vanaf begin juni 2017 zal Security Center de Microsoft Monitoring Agent gebruiken voor het verzamelen en opslaan van gegevens. Zie [Migratie van Azure Security Center-platform](security-center-platform-migration.md) voor meer informatie. De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>

<a id="why-deploy-partners-solutions-from-security-center" class="xliff"></a>

## Waarom partneroplossingen met Security Center integreren?

Dit zijn de vier belangrijkste redenen voor partnerintegratie in Security Center:

- **Eenvoudige implementatie**: het is veel gemakkelijker om een partneroplossing te implementeren door de aanbeveling van Security Center te volgen. Het implementatieproces kan volledig worden geautomatiseerd met een standaardconfiguratie en -netwerktopologie. Klanten kunnen ook kiezen voor een semi-automatische optie die meer flexibiliteit en aanpasmogelijkheden bij de configuratie biedt.
- **Geïntegreerde detectie**: beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde statuscontrole en beheer**: met geïntegreerde statusgebeurtenissen kunnen klanten alle partneroplossingen in één oogopslag controleren. U beschikt over eenvoudige beheermogelijkheden en hebt toegang tot geavanceerde configuratieopties met behulp van de partneroplossing.
- **Exporteren naar SIEM**: klanten kunnen nu alle meldingen van Security Center en partners in CEF-indeling exporteren naar on-premises SIEM-systemen met behulp van Microsoft Azure-logboekintegratie (preview).


<a id="what-partners-are-integrated-with-security-center" class="xliff"></a>

## Welke partners zijn geïntegreerd met Security Center?
Security Center is momenteel geïntegreerd met deze oplossingen:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Microsoft Antimalware voor Azure](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Web Application Firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), [App Gateway WAF](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Next Generation Firewall ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) en [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Vulnerability Assessment ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Het aantal partners in de bestaande categorieën in Security Center zal geleidelijk worden uitgebreid en er zullen nieuwe categorieën worden toegevoegd. 

<a id="how-to-deploy-a-partner-solution" class="xliff"></a>

## Een partneroplossing implementeren

Security Center kan aanraden om een partneroplossing te implementeren op basis van de configuratie van uw Azure-omgeving en het gedefinieerde beveiligingsbeleid. In de aanbeveling wordt u door het selecteren en installeren van een partneroplossing geleid. Het implementatieproces kan op dit punt per soort oplossing en partner verschillen. Raadpleeg de volgende koppelingen voor meer informatie:

- [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md)
- [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md)
- [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md)
- [Beoordeling van beveiligingslekken is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md)

<a id="how-to-manage-partner-solutions" class="xliff"></a>

## Hoe beheer ik partneroplossingen?

Zodra een partneroplossing is geïmplementeerd, kunt u informatie over de status van de oplossing bekijken en basisbeheertaken uitvoeren vanuit de tegel Partneroplossing in het hoofddashboard van Security Center. Lees [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md) voor meer informatie over het beheren van partneroplossingen in Security Center.

![Partnerintegratie](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> De ondersteuning voor Symantec Endpoint Protection is beperkt tot detectie, maar er zijn geen statuswaarschuwingen beschikbaar.
>

<a id="see-also" class="xliff"></a>

## Zie ook
In dit document hebt u kunnen lezen hoe u een partneroplossing integreert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md)
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

