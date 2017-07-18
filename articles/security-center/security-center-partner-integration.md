---
title: Partnerintegratie in Azure Security Center | Microsoft Docs
description: "Leer hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging van uw Azure-resources te verbeteren."
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
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 4d0909e926de14a0cbe9799b969ac7a1946d69d1
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegratie in Azure Security Center

In dit artikel wordt beschreven hoe Azure Security Center integreert met partners om u te helpen de algehele beveiliging te verbeteren. Security Center biedt een geïntegreerde ervaring in Azure en maakt gebruik van de voordelen van Azure Marketplace voor partnercertificering en -facturering.

> [!NOTE] 
> Vanaf juni 2017 gebruikt Security Center de Microsoft Monitoring Agent voor het verzamelen en opslaan van gegevens. Zie [Migratie van Azure Security Center-platform](security-center-platform-migration.md) voor meer informatie. De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Waarom partneroplossingen uit Security Center implementeren

Er zijn vier belangrijke redenen om partnerintegratie in Security Center te gebruiken:

- **Eenvoudige implementatie**. Het is veel gemakkelijker om een partneroplossing te implementeren door de aanbeveling van Security Center te volgen. Het implementatieproces kan volledig worden geautomatiseerd met behulp van de standaardinstallatie en netwerktopologie. Klanten kunnen ook kiezen voor een semi-geautomatiseerde optie voor meer flexibiliteit en aanpassing.
- **Geïntegreerde detectie**. Beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde status- en beheercontrole**. Klanten kunnen geïntegreerde statusgebeurtenissen gebruiken om in één oogopslag alle partneroplossingen te controleren. Er zijn basisbeheermogelijkheden beschikbaar, met eenvoudige toegang tot geavanceerde installatie met behulp van de partneroplossing.
- **Naar SIEM exporteren**. Klanten kunnen alle meldingen van Security Center en van de partner in CEF (Common Event Format) exporteren naar on-premises SIEM-systemen (Security Information and Event Management) met behulp van Azure-logboekintegratie (preview-versie).


## <a name="partners-that-integrate-with-security-center"></a>Partners die zijn geïntegreerd met Security Center

Security Center is momenteel geïntegreerd met deze oplossingen:

- Eindpuntbeveiliging ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec en [Microsoft Antimalware for Azure Cloud Services and Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Firewall voor webtoepassingen ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) en [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall van de volgende generatie ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) en [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Evaluatie van beveiligingsproblemen ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Het aantal partners in deze categorieën in Security Center zal geleidelijk worden uitgebreid en er zullen nieuwe categorieën worden toegevoegd. 

## <a name="deploy-a-partner-solution"></a>Een partneroplossing implementeren

Er kan een Security Center-aanbeveling worden weergegeven voor het implementeren van een partneroplossing op basis van de installatie van uw Azure-omgeving en het gedefinieerde beveiligingsbeleid. In de Security Center-aanbeveling wordt u door het selecteren en installeren van een partneroplossing geleid. Het implementatieproces kan verschillen, afhankelijk van het gebruikte type oplossing en van de partner. Raadpleeg voor meer informatie de volgende artikelen:

- [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md)
- [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md)
- [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md)
- [Evaluatie van beveiligingsproblemen is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Partneroplossingen beheren

Selecteer in de blade **Security Center** de tegel **Partneroplossingen** om na de implementatie informatie te bekijken over de status van de oplossing en om basisbeheertaken uit te voeren. Raadpleeg [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md) voor meer informatie over het beheren van partneroplossingen in Security Center.

![Partnerintegratie](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> De ondersteuning voor Symantec Endpoint Protection is beperkt tot detectie. Er zijn geen statuswaarschuwingen beschikbaar.
>

## <a name="see-also"></a>Zie ook

In dit artikel hebt u kunnen lezen hoe u partneroplossingen integreert in Azure Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Plannings- en bedieningsgids voor Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingswaarschuwingen per type in Security Center](security-center-alerts-type.md)
* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

