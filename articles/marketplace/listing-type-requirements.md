---
title: Vereisten voor door vermeldingstype | Azure
description: In dit artikel beschrijft de in aanmerking komt criteria en publishing vereisten partners probeert te begrijpen hoe u apps publiceert naar Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 07a62dfa2d7e1c71daf547c5aa7c8c7d15830bfd
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309313"
---
# <a name="requirements-by-listing-type"></a>Vereisten voor door vermeldingstype  
De technische en marketing inhoud vereisten verschillen per winkel, het aanbiedingtype en type van de aanbieding. Bekijk de volgende specificaties om te controleren of de naleving.  
1. Via vereisten:  
    *   [AppSource](#storefront-requirements-appSource)  
    *   [Azure Marketplace](#storefront-requirements-azure-marketplace)  
2. Vermeldingstype en aanbieding Type vereisten:  
    *   Voor meer informatie over de aanbieding waardetypen en aanbiedingstypen, gaat u naar de Bepaal het Type aanbieding voor uw oplossing pagina op [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>Via vereisten: AppSource  
De volgende tabel beschrijft de vereisten voor publicatie op AppSource.  
| Vereiste | Details | Vereist of aanbevolen |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | Uw app moet Azure Active Directory voor federatieve eenmalige aanmelding (Azure AD federatieve SSO) toestaan met toestemming ingeschakeld.<ul> <li>Voor meer informatie over het inschakelen van Azure AD voor federatieve eenmalige aanmelding, gaat u naar de configureren eenmalige aanmelding tot toepassingen die zich niet in de Azure Active Directory application gallery pagina zich bevindt op [docs.microsoft.com/azure/active-directory/ Active-directory-saas-aangepaste-apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Vereist |   
| ***Integratie met Microsoft Cloud-Services*** | Uw app moet worden geïntegreerd met andere Microsoft-Cloud-services zoals Microsoft Power BI Cortana Intelligence of Microsoft Azure-services.<ul> <li>Een voorbeeld van een Microsoft-Cloud-service is Internet der dingen.</li> </ul> | Aanbevolen |  
| ***doelgroep*** | Uw app moet voor line-of-business-gebruikers en eigenaars van bedrijven. | Vereist | 
| ***Software als een service (SaaS)-app voor bedrijven*** | Uw app moet voldoen aan de volgende vereisten.<ul> <li>Een line-of-business-app voor SaaS</li> <li>Bedrijfsproces gericht</li> <li>Gericht op zakelijke klanten</li> <li>Gebruikers kunnen hun werkreferenties gebruiken zoals gebruikersnaam en wachtwoord aanmelden inschakelen</li> </ul> | Vereist |  
| ***Gratis proefperiode en evaluatiesoftware*** | Uw app moet bevatten een de volgende opties om een klant uw app gratis gebruiken voor een beperkte periode.<ul> <li>Geef een `try` methode, zodat klanten kunnen een proefversie van uw app binnen AppSource starten</li> <li>Geef een `request trial` optie in de AppSource, zodat klanten kunnen vragen om een evaluatieversie van uw app</li> </ul>De gratis proefversie die u opgeeft moet de klant bieden een vooraf ingestelde periode voor het uitproberen van uw app met zonder extra kosten. | Vereist |  
| ***Eenvoudig kunnen worden geconfigureerd, kant-en-klare oplossing*** | Uw app moet configureren en instellen met geen aanpassing vereist snel en eenvoudig. | Vereist |  
| ***Leads beheren*** | U moet uw CRM accepteren leadgegevens voordat u ontvangt leidt van de winkel inschakelen.<ul> <li>Voorbeelden van CRM's zijn Marketo, Microsoft Dynamics of Salesforce</li> </ul> | Vereist |  
| ***Het privacybeleid en gebruiksvoorwaarden*** | Uw app moet een koppeling naar de pagina van uw privacy-beleid met behulp van een openbare URL opgeven. De gebruiksvoorwaarden moeten worden opgegeven tijdens het publiceren als tekst. | Vereist |  
| ***Ondersteuning*** | Uw app moet een koppeling naar de ondersteuningspagina van uw klant met behulp van een openbare URL opgeven. Als uw app een proefversie is, moet klikt u ondersteunen zonder extra kosten tijdens de evaluatieperiode. | Vereist |  

## <a name="storefront-requirements-azure-marketplace"></a>Via vereisten: Azure Marketplace  
Hier volgen de vereisten voor het aanbieden van typen in Azure Marketplace.  
| Vereiste | Details | Vermeldingstype |  
|:--- |:--- |:--- |  
| ***Deelname-beleid*** | U de app moet voldoen aan het beleid van de deelname aan Azure Marketplace.<ul> <li>Voor meer informatie over het beleid voor deelname, gaat u naar de Azure Marketplace deelname beleidsregels pagina zich bevindt op [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | lijst<br />Transact<br />proefversie |  
| ***Integratie met Microsoft*** | Uw aanbieding moet gebruiken of breid typen van de Microsoft Azure service zoals compute, netwerk of opslag. Uw aanbieding moet uitgelijnd met een bestaande Azure Marketplace-categorie, zoals databases, beveiliging of netwerk.<ul> <li>Voor meer informatie over de Marketplace-aanbiedingen, gaat u naar de pagina zich bevindt op Marketplace-Apps [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | lijst<br />Transact<br />proefversie |  
| ***doelgroep*** | Uw aanbieding moet zijn voor IT-professionals, cloud ontwikkelaars of andere technische klant-functies. | lijst<br />Transact<br />proefversie |  
| ***Leads beheren*** | U moet uw CRM (Marketo, Microsoft Dynamics of Salesforce) te accepteren leadgegevens voordat u ontvangt leidt van de winkel inschakelen. | lijst<br />Transact<br />proefversie |  
| ***Het privacybeleid en gebruiksvoorwaarden*** | Uw app moet een koppeling naar de pagina van uw privacy-beleid met behulp van een openbare URL opgeven. De gebruiksvoorwaarden moeten worden opgegeven tijdens het publiceren als tekst. | lijst<br />Transact<br />proefversie |  
| ***Ondersteuning*** | Uw aanbieding moet een koppeling naar de ondersteuningspagina van uw klant met behulp van een openbare URL opgeven. Als uw aanbieding een proefversie is, moet klikt u ondersteunen zonder extra kosten tijdens de evaluatieperiode. | Transact<br />proefversie |    

## <a name="non-transact-listings"></a>Niet-transact aanbiedingen  
Deze sectie beschrijft alle aanbiedingstypen die de Transact vermeldingstype niet wordt gebruikt. 

### <a name="list"></a>Lijst  
De lijst vermeldingstype bevat de volgende typen van de aanbieding op de winkelobjecten in de marketplace.  

| Type aanbieding | Storefront | Details |  
|:---        |:---        |:---     |  
| Advies | AppSource | [Vereisten: AppSource: lijst: adviesdiensten](#requirements-appsource-list-consulting-services) |  
| Advies | Azure Marketplace | [Vereisten: Azure Marketplace: lijst: adviesdiensten](#requirements-azure-marketplace-list-consulting-services) |  
| Contact opnemen | AppSource | [](#) |  
| Contact opnemen | Azure Marketplace | [Vereisten: AppSource: lijst: contact met mij opnemen](#requirements-azure-marketplace-list-contact-me) |  

#### <a name="requirements-appsource-list-consulting-service"></a>Vereisten: AppSource: lijst: advies service  

| Vereisten | Details |  
|:--- |:--- |  
| Service aanbieding kenmerken | Uw advies service moet voldoen aan de volgende criteria.<ul> <li>Bieden een engagement afgebakende, vaste duur, vaste prijs (of gratis).</li> <li>Plaatsen volgens hoofdzakelijk voor voorverkoop.</li> <li>Beperkt tot één klant.</li> <li>Uitvoeren op de site.</li> </ul> |  
| Vereisten voor partners voor ondersteuningsdiensten | U voldoen aan de criteria in het desbetreffende gebied voor uw service.<table><tr><th>Oplossing gebied</th><th>Criteria</th></tr><tr><td>Dynamics 365 voor betrokkenheid van de klant</td><td>Zilver of goud Cloud Customer Relationship Management competentie hebben.</td></tr><tr><td>Dynamics 365 voor Financiën en bewerkingen, Enterprise edition</td><td>Zilver of Gold Enterprise Resource Planning competentie en omzet van uw cloud-bewerkingen in de afsluitende 12 maanden van $25.000 of meer hebben.</td></tr><tr><td>Dynamics 365 voor Financiën en bewerkingen, Business edition</td><td>Fungeren als Cloud Services-Provider (CSP) of digitale Partner van Record (DPOR) voor een of meer klanten.</td></tr><tr><td>Power BI</td><td>Voldoen aan de oplossing Partner.</td></tr><tr><td>PowerApps</td><td>De oplossing van een Partner-toepassingen hebt.</td></tr></table><ul> <li>Voor meer informatie over het beheer van de klant relatie gaat u naar de Cloud Customer Relationship Management pagina zich bevindt op [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Voor meer informatie over het plannen van de resource gaat u naar de resourceplanning pagina zich bevindt op [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>Voor meer informatie over de CSP, gaat u naar de Cloud Services-Provider pagina zich bevindt op [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>Voor meer informatie over DPOR, gaat u naar de digitale Partner of Record en Partner koppeling pagina zich bevindt op [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Voor meer informatie over de oplossing partner criteria, gaat u naar de oplossing partneroverzicht en stimulansen document zich bevindt op [www.microsoftpartnerserverandcloud.com/_layouts/download.aspx? SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf).</li> <li>Voor meer informatie over de partner-toepassingen, gaat u naar de Partner-toepassingen pagina zich bevindt op [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Vereisten: Azure Marketplace: lijst: advies service  

| Vereisten | Details |  
|:--- |:--- |  
| Service aanbieding kenmerken | Uw advies service moet voldoen aan de volgende criteria.<ul> <li>Bieden een engagement afgebakende, vaste duur, vaste prijs (of gratis).</li> <li>Plaatsen volgens hoofdzakelijk voor voorverkoop.</li> <li>Beperkt tot één klant.</li> <li>Uitvoeren op de site.</li> </ul> |  
| Vereisten voor partners voor ondersteuningsdiensten | U moet zilver of goud hebben in een van de volgende bevoegdheden in het desbetreffende gebied voor uw service. <table><tr><th>Oplossing gebied</th><th>Competentie</th></tr><td>Cloud-Platform en infrastructuur</td><td>Cloud-Platform<br />Datacentrum</td><tr><td>Toepassingsontwikkeling en ISV</td><td>Ontwikkeling van toepassingen<br />Integratie van toepassingen<br />DevOps</td></tr><tr><td>Gegevensbeheer en -analyse</td><td>Gegevensanalyse<br />Data Platform</td></tr></table><ul> <li>Voor meer informatie over bevoegdheden, gaat u naar de vaardigheden via Microsoft Partner Network pagina zich bevindt op [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Voor meer informatie over de aanbieding, gaat u naar de Azure Marketplace ondersteuningsdiensten pagina zich bevindt op [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---   

### <a name="trial"></a>Proefversie  

| Type aanbieding | Storefront | Details |  
|:---        |:---        |:---     |  
| Gratis / SaaS-evaluatieversie | AppSource | [Vereisten voor Type aanbieding: proefversie](#listing-type-requirements-trial) |  
| Gratis / SaaS-evaluatieversie | Azure Marketplace | [Vereisten: Azure Marketplace: proefversie: gratis proefversie / SaaS-evaluatieversie](#requirements-azure-marketplace-trial-free-trial-/-saas-trial) |  
| Interactieve demo | AppSource | [Vereisten voor Type aanbieding: proefversie](#listing-type-requirements-trial) |  
| Interactieve demo | Azure Marketplace | [Vereisten: Azure Marketplace: proefversie: interactieve demo](#requirements-azure-marketplace-trial-interactive-demo) |  
| Test drive | AppSource | [Vereisten voor Type aanbieding: proefversie](#listing-type-requirements-trial) |  
| Test drive | Azure Marketplace | [Vereisten: Azure Marketplace: proefversie: testen](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Vereisten: Azure Marketplace: proefversie  

| Vereiste | Details |  
|:--- |:--- |  
| Gratis proefperiode en evaluatiesoftware | De klant kan uw app gratis gebruiken voor een beperkte periode.<br /><br />De klant is niet vereist voor een licentie of abonnement betalen voor uw aanbieding of app. De klant is niet vereist om te betalen voor de onderliggende eigen Microsoft-product of service. Alle opties voor proefversie worden geïmplementeerd naar uw Azure-abonnement. U hebt enige besturingselement evaluatieversie van de kostenoptimalisatie en het beheer.<br /><br />U kunt ervoor kiezen een gratis proefversie, interactieve demo of uitprobeert. Ongeacht wat u kiest moet uw gratis proefabonnement bieden de klant een vooraf ingestelde periode om te proberen de app zonder extra kosten.<ul> <li>Om te beginnen met het maken van een station test sturen een e-mail naar [ amp-testdrive@microsoft.com ](mailto:amp-testdrive@microsoft.com).</li> </ul>Opmerking: Azure Marketplace SaaS proefversie ervaringen moeten klanten werkreferenties gebruiken om aan te melden toestaan.<ul> <li>Voor meer informatie gaat u naar de sectie proefversie ervaringen zich bevindt op AppSource [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Eenvoudig kunnen worden geconfigureerd, kant-en-klare oplossing | Uw app moet eenvoudig en snel te configureren en instellen. |  
| Beschikbaarheid / bedrijfstijd | Uw SaaS-app of platform moet een beschikbaarheid van minimaal 99,9% hebben. |  
| Azure Active Directory | Uw aanbieding moet toestaan dat Azure Active Directory (Azure AD) voor federatieve eenmalige aanmelding (SSO) (Azure AD federatieve SSO) met toestemming ingeschakeld. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Vereisten: Azure Marketplace: proefversie: gratis proefversie / SaaS-evaluatieversie  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Hiermee kunt een klant om te proberen het product voordat gebruik kopen bij een automatische methode voor het converteren naar een betaald. Bovendien kunnen bewijzen van concept voor de klant en gemeenschappelijke betrokkenheid bij sales-teams van Microsoft. | Uw oplossing is een virtuele machine of oplossingssjabloon.<br /><br />Uw oplossing is een aanbieding SaaS en aanbieden van een multitenant SaaS-product.<br /><br />U hebt een eerste sessie voor een klant en snel uitgevoerd.<br /><br />U hebt een één tenant maar klanten als gastgebruikers toevoegt. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Vereisten: Azure Marketplace: proefversie: interactieve demo  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Kunnen zien van uw oplossing in actie zonder de complexiteit van het instellen van uw klanten. | Uw oplossing vereist complexe instellingen die moeilijk te bewerkstelligen dat binnen de evaluatieperiode. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Vereisten: Azure Marketplace: proefversie: testen  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Hiermee kunt een klant om te proberen het product voordat ze kopen.<br /><br />Biedt een begeleide ervaring voor uw oplossing met behulp van vooraf geconfigureerde instellingen.<br /><br />De volgende zijn extra voordelen wanneer u een test-station.<ul> <li>27% van de gebruiker zoekopdrachten op marketplace worden verfijnd door gebruikers alleen weergeven aanbiedingen met test-stations.</li> <li>Aanbiedingen met test stations leads 38% meer dan aanbiedingen zonder genereren.</li> <li>36% van de nieuwe klant aankopen op marketplace afkomstig zijn van klanten dat een test-station in beslag nam.</li> <li>Test stations inschakelen Microsoft veld verkopers om uw product mede verkopen inspanningen beter te begrijpen.</li> </ul> | Uw oplossing is een virtuele machine, oplossingssjabloon of SaaS-app met een één tenant of is ingewikkeld om in te richten. <br /><br />U beschikt niet over een methode voor uw proefversie omzetten naar een betaald abonnement. |  

---

## <a name="transact-specific-listings"></a>Transact-specifieke aanbiedingen

### <a name="transact"></a>Handelen  

| Type aanbieding | Storefront | Details |   
|:---        |:---        | :--- |  
| Apps van Azure: beheerde app | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: apps van Azure: beheerde app](#requirements-azure-marketplace-transact-azure-apps-managed-app) |  
| Apps van Azure: oplossingssjabloon | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: apps van Azure: oplossingssjabloon](#requirements-azure-marketplace-transact-azure-apps-solution-template) |  
| Containers | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: Container](#requirements-azure-marketplace-transact-container) |  
| SaaS-app  | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: SaaS-app](#requirements-azure-marketplace-transact-saas-app) |  
| Virtuele machine | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: virtuele machine](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Vereisten: Azure Marketplace: Transact: Container  

| Vereiste | Details |  
|:--- |:--- |  
| Facturerings- en softwarelicentiecontrole | Ondersteuning voor beide de gratis of facturering BYOL-model. |  
| Docker-installatiekopie | Uw installatiekopie container moet worden gebaseerd op de Docker-bestandsindeling en moet worden opgehaald uit Azure Container registers. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Vereisten: Azure Marketplace: Transact: SaaS-app  

| Vereiste | Details |  
|:--- |:--- |  
| Facturerings- en softwarelicentiecontrole | Uw aanbieding is een maandelijkse vast tarief prijs. Gebruik gebaseerde prijscategorie en gebruik gebaseerde *true-up* opties worden niet ondersteund op dit moment. |  
| Annulering | Uw aanbieding is op elk gewenst moment Annuleerbare door de klant. |  
| Startpagina van de transactie | De startpagina van een Azure gezamenlijke transactie hosten. Uw startpagina kan uw klanten maken en beheren van uw SaaS-serviceaccount. |  
| SaaS-abonnement API | Geef een service die communiceert met de SaaS-abonnement maken, bijwerken en verwijderen van een gebruiker-account en service-plan. Alle kritieke API wijzigingen moeten binnen 24 uur worden ondersteund. Niet-kritieke API wijzigingen worden regelmatig bijgewerkt. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Vereisten: Azure Marketplace: Transact: virtuele machine  

| Vereiste | Details |  
|:--- |:--- | 
| Facturerings- en softwarelicentiecontrole | Uw virtuele machine moet ondersteuning voor facturering BYOL of betalen naar gebruik. |  
| Azure-compatibele virtuele harde schijf (VHD) | Virtuele machines moeten worden gebaseerd op Windows of Linux.<ul> <li>Voor meer informatie over het maken van een Linux-VHD, gaat u naar de maken een Azure-compatibele VHD sectie (op basis van Linux) zich bevindt op [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Ga voor meer informatie over het maken van een Windows-VHD naar de maken een Azure-compatibele VHD sectie (op Windows gebaseerd) zich bevindt op [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-compatible-VHD-Windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Volgende stappen
*   Ga naar de [AppSource Publisher handleiding en Azure Marketplace](./marketplace-publishers-guide.md) pagina.  
 
---  
