---
title: Partner- en oplossingenintegratie in Azure Security Center | Microsoft Docs
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
ms.date: 09/12/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8cc44da0f61362018d2757da58ca4fb3a9a43764
ms.contentlocale: nl-nl
ms.lasthandoff: 09/14/2017

---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Partner- en oplossingenintegratie in Azure Security Center

In dit artikel wordt beschreven hoe Azure Security Center integreert met partners om u te helpen de algehele beveiliging te verbeteren. Security Center biedt een geïntegreerde ervaring in Azure en maakt gebruik van de voordelen van Azure Marketplace voor partnercertificering en -facturering.

## <a name="why-deploy-partner-solutions-from-security-center"></a>Waarom partneroplossingen uit Security Center implementeren

Er zijn vier belangrijke redenen om partnerintegratie in Security Center te gebruiken:

- **Eenvoudige implementatie**. Het is veel gemakkelijker om een partneroplossing te implementeren door de aanbeveling van Security Center te volgen. Het implementatieproces kan volledig worden geautomatiseerd met behulp van de standaardinstallatie en netwerktopologie. Klanten kunnen ook kiezen voor een semi-geautomatiseerde optie voor meer flexibiliteit en aanpassing.
- **Geïntegreerde detectie**. Beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde status- en beheercontrole**. Klanten kunnen geïntegreerde statusgebeurtenissen gebruiken om in één oogopslag alle partneroplossingen te controleren. Er zijn basisbeheermogelijkheden beschikbaar, met eenvoudige toegang tot geavanceerde installatie met behulp van de partneroplossing.
- **Naar SIEM exporteren**. Klanten kunnen alle meldingen van Security Center en van de partner in CEF (Common Event Format) exporteren naar on-premises SIEM-systemen (Security Information and Event Management) met behulp van Azure-logboekintegratie (preview-versie).


## <a name="partners-that-integrate-with-security-center"></a>Partners die zijn geïntegreerd met Security Center

Op dit moment is de volgende systeemeigen integratie van de partneroplossingen beschikbaar in Azure Marketplace met Security Center:

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

Selecteer op het dashboard **Security Center** de optie **Partneroplossingen** om na de implementatie informatie te bekijken over de status van de oplossing en om basisbeheertaken uit te voeren.

![Integratie van partneroplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

De inhoud die u ziet wanneer u Beveiligingsoplossingen opent, is afhankelijk van uw infrastructuur. Als we de vorige afbeelding als voorbeeld gebruiken, heeft deze pagina drie secties:

- **Verbonden oplossingen**: hier ziet u oplossingen die zijn verbonden met Security Center.
- **Gedetecteerde oplossingen**: hier ziet u oplossingen die niet zijn verbonden met Security Center. U kunt deze oplossingen verbinden, waarna ze onder verbonden oplossingen worden weergegeven.  Als in Security Center geen niet-verbonden oplossingen worden gedetecteerd, is deze sectie verborgen.
- **Gegevensbronnen toevoegen**: hier ziet u Azure- en niet-Azure-gegevensbronnen die u aan Security Center kunt toevoegen.

### <a name="connected-solutions"></a>Verbonden oplossingen

In de sectie **Verbonden oplossingen** ziet u alle beveiligingsoplossingen die op dat moment zijn verbonden met Security Center. 

![Verbonden oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Welke informatie wordt weergegeven, is afhankelijk van de oplossing. De volgende informatie kan op elke tegel worden weergegeven:

- Bedrijfspictogram voor de partner.  Als Security Center het bedrijfspictogram niet heeft, worden de eerste tekens van de naam van de partner weergegeven.
- Oplossingstype.
- De computernaam wordt mogelijk weergegeven.
- Integriteitsstatus.  Als er geen integriteitsindicator is verzonden, worden in Security Center de datum en tijd weergegeven van de laatst ontvangen gebeurtenis. Hiermee wordt aangegeven of het apparaat al dan niet rapporteert. Als Security Center de integriteitsindicator van een bepaalde oplossing niet ontvangt, wordt de tegel van die oplossing niet in deze sectie weergegeven.

> [!NOTE]
> In Watch Security Center worden de datum en tijd weergegeven van de laatst ontvangen gebeurtenis. Hiermee wordt aangegeven of het apparaat al dan niet rapporteert. Oplossingen die geen integriteitsindicator verzenden, worden weergegeven als Verbonden als in de afgelopen 14 dagen een waarschuwing of gebeurtenis is verzonden.
>  

Sommige van deze oplossingen zijn mogelijk volledig geïntegreerd in Azure; andere kunnen on-premises zijn. Aangezien Security Center [CEF (Common Event Format)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef) ondersteunt, kan verbinding worden gemaakt met oplossingen die CEF gebruiken, zoals een Firewall die CEF ondersteunt. Vanaf het moment dat deze oplossing is toegevoegd aan Security Center, worden Firewall-logboeken in CEF-indeling naar Security Center verzonden, waar ze worden doorgegeven aan [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Firewall is een niet-Azure-resource die wel gebeurtenissen, maar geen integriteitsindicator verzendt.  De enige informatie die Security Center over de status heeft, is het laatste tijdstip waarop dit apparaat een gebeurtenis heeft verzonden.  Voor alle niet-Azure-resources worden in Security Center in het statusgebied van de tegel de datum en tijd weergegeven waarop de laatste gebeurtenis is ontvangen; dit geeft aan dat de niet-Azure-resource nog steeds rapporteert.

### <a name="discovered-solutions"></a>Gedetecteerde oplossingen

In de sectie **Gedetecteerde oplossingen** ziet u alle oplossingen die zijn toegevoegd via Azure en door Security Center worden voorgesteld om verbinding mee te maken.

![Gedetecteerde oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center kan worden geïntegreerd met ingebouwde Azure-oplossingen, zoals [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection). Als u een licentie hebt voor Azure AD Identity Protection, maar deze niet is verbonden met Security Center, wordt Azure AD Identity Protection vermeld onder **Gedetecteerde oplossingen**. Als u deze oplossing wilt integreren met Security Center, klikt u op de tegel **Azure AD Identity Protection** op **VERBINDING MAKEN**, waarna de volgende pagina wordt weergegeven:

![Azure AD-identiteitsbeveiliging](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

U voltooit het verbinden van Azure AD Identity Protection door een werkruimte te selecteren waarin de gegevens moeten worden opgeslagen. Alle gegevens van Azure AD Identity Protection stromen vanuit het werkruimtegebied dat in deze stap is geselecteerd.  Via de werkruimteselector selecteert u de werkruimte, waarna de gegevens daarvandaan beginnen te stromen.

U moet een globale beheerder of beveiligingsbeheerder zijn om verbinding te kunnen maken met Security Center.  De knop **Verbinding maken** is uitgeschakeld als u niet gemachtigd bent. In dat geval wordt er een bericht weergegeven waarin wordt uitgelegd waarom de knop is uitgeschakeld.

Waarschuwingen van Azure AD Identity Protection lopen via de detectiepijp van Security Center, zodat u waarschuwingen ontvangt van Security Center en Azure Active Directory Identity Protection. In Security Center worden alle waarschuwingen samengevoegd die relevant lijken voor het maken van een [veiligheidsincident](https://docs.microsoft.com/azure/security-center/security-center-incident). De beschrijving van het beveiligingsincident geeft u meer inzicht in verdachte activiteiten.

### <a name="add-data-sources"></a>Gegevensbronnen toevoegen

U kunt Azure- en niet-Azure-computers toevoegen om te integreren met Security Center.  Het toevoegen van niet-Azure-computers betekent dat u een on-premises computer kunt toevoegen, of een apparaat dat ondersteuning biedt voor CEF. 

![Gegevensbronnen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Zie ook

In dit artikel hebt u kunnen lezen hoe u partneroplossingen integreert in Azure Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Plannings- en bedieningsgids voor Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingswaarschuwingen per type in Security Center](security-center-alerts-type.md)
* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

