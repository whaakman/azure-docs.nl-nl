---
title: Vereisten van het Type aanbieding | Azure
description: In dit artikel beschrijft de criteria voor in aanmerking te komen en publiceren vereisten partners probeert te begrijpen hoe u apps publiceren naar de Azure Marketplace.
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
ms.date: 12/19/2018
ms.author: ellacroi
ms.openlocfilehash: ebe344d9f596f862fe5ffbfef083725e6527d0d3
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650203"
---
# <a name="requirements-by-listing-type"></a>Vereisten van het Type aanbieding  
De technische en marketingmateriaal inhoud vereisten variëren per winkel, aanbiedingtype en het type van de aanbieding. Bekijk de volgende specificaties om te controleren of de naleving.  
1. StoreFront vereisten:  
    *   [AppSource](#storefront-requirements-appsource)  
    *   [Azure Marketplace](#storefront-requirements-azure-marketplace)  
2. Type aanbieding en aanbieding Type vereisten:  
    *   Ga voor meer informatie over de typen van de aanbieding en typen naar het bepalen van het Type aanbieding voor de pagina met uw oplossing zich bevindt in [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>StoreFront vereisten: AppSource  
De volgende tabel beschrijft de vereisten voor het publiceren op AppSource.  

| Vereiste | Details | Vereist of aanbevolen |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | Uw app moet kunnen Azure Active Directory voor federatieve eenmalige aanmelding (Azure AD federatieve SSO) met toestemming ingeschakeld.<ul> <li>Voor meer informatie over het inschakelen van Azure AD voor federatieve eenmalige aanmelding, gaat u naar de configureren eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory application gallery-pagina te vinden op [docs.microsoft.com/azure/active-directory/ Active-directory-saas-aangepaste-apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Vereist |   
| ***Integratie met Microsoft Cloud Services*** | Uw app te integreren met andere Microsoft-Cloud-services zoals Microsoft Power BI, Cortana Intelligence of Microsoft Azure-services.<ul> <li>Een voorbeeld van een Microsoft Cloud-service is Internet of Things.</li> </ul> | Aanbevolen |  
| ***Doelgroep*** | Voor line-of-business-gebruikers en eigenaren van bedrijven moet uw app. | Vereist | 
| ***Software als een app service (SaaS) voor bedrijven*** | Uw app moet voldoen aan de volgende vereisten.<ul> <li>Een line-of-business SaaS-app</li> <li>Bedrijfsproces gericht</li> <li>Bedoeld voor zakelijke klanten</li> <li>Gebruikers met hun werkreferenties aan te melden bij, zoals gebruikersnaam en wachtwoord</li> </ul> | Vereist |  
| ***Gratis proefperiode is afgelopen en evaluatiesoftware*** | Uw app moet bevatten een de volgende opties in de order voor een klant uw app gedurende een beperkte periode gratis te gebruiken.<ul> <li>Geef een `try` methode, zodat klanten met een proefversie van uw app in AppSource beginnen kunnen</li> <li>Geef een `request trial` optie in de AppSource, zodat klanten kunnen vragen om een evaluatieversie van uw app</li> </ul>De gratis proefversie die u opgeeft moet de klant bieden een vooraf ingestelde periode voor het uitproberen van uw app zonder extra kosten. | Vereist |  
| ***Eenvoudig kunnen worden geconfigureerd, kant-en-klare oplossing*** | Uw app moet zijn eenvoudig en snel te configureren en instellen met geen aanpassingen vereist. | Vereist |  
| ***Leadbeheer*** | Schakel in uw CRM accepteren leadgegevens voordat u ontvangen van de webwinkel leidt.<ul> <li>Voorbeelden van referentiematerialen zijn Marketo, Microsoft Dynamics of Salesforce</li> </ul> | Vereist |  
| ***Privacybeleid en de gebruiksvoorwaarden*** | Uw app moet een koppeling naar de pagina van uw privacy-beleid met behulp van een openbare URL opgeven. De gebruiksrechtovereenkomst moeten worden opgegeven tijdens het publiceren als tekst. | Vereist |  
| ***Ondersteuning*** | Uw app moet een koppeling naar de ondersteuningspagina van uw klant via een openbare URL opgeven. Als uw app een proefversie, moet klikt u vervolgens u ondersteunen zonder extra kosten tijdens de proefperiode is afgelopen. | Vereist |  

## <a name="storefront-requirements-azure-marketplace"></a>StoreFront vereisten: Azure Marketplace  
Hier volgen de vereisten voor het aanbieden van typen in Azure Marketplace.  

| Vereiste | Details | Type aanbieding |  
|:--- |:--- |:--- |  
| ***Beleidsregels voor deelname aan*** | Uw app moet voldoen aan het deelnamebeleid voor Azure Marketplace.<ul> <li>Ga naar pagina te op het vinden Deelnamebeleid voor van de Azure Marketplace voor meer informatie over de beleidsregels voor deelname aan [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | list<br />Transact<br />trial |  
| ***Integratie met Microsoft*** | Uw aanbieding moet gebruiken of uitbreiden van Microsoft Azure-servicetypen zoals compute, netwerk of opslag. Uw aanbieding uit te lijnen aan een bestaande Azure Marketplace-categorie, zoals databases, beveiliging of netwerken.<ul> <li>Voor meer informatie over de Marketplace-aanbiedingen, gaat u naar de pagina te vinden op Marketplace-Apps [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | list<br />Transact<br />trial |  
| ***Doelgroep*** | Uw aanbieding moet voor IT-professionals, cloudontwikkelaars of andere technische klantrollen. | list<br />Transact<br />trial |  
| ***Leadbeheer*** | Schakel in uw CRM (Marketo, Microsoft Dynamics of Salesforce) om te accepteren leadgegevens voordat u ontvangen van de webwinkel leidt. | list<br />Transact<br />trial |  
| ***Privacybeleid en de gebruiksvoorwaarden*** | Uw app moet een koppeling naar de pagina van uw privacy-beleid met behulp van een openbare URL opgeven. De gebruiksrechtovereenkomst moeten worden opgegeven tijdens het publiceren als tekst. | list<br />Transact<br />trial |  
| ***Ondersteuning*** | Uw aanbieding moet een koppeling naar de ondersteuningspagina van uw klant via een openbare URL opgeven. Als uw aanbieding een proefversie is, moet klikt u vervolgens u ondersteunen zonder extra kosten tijdens de proefperiode is afgelopen. | Transact<br />trial |    

## <a name="non-transact-listings"></a>Niet-transact aanbiedingen  
Deze sectie beschrijft alle typen die geen gebruik maken van het type aanbieding Transact. 

### <a name="list"></a>Lijst  
De lijst met type aanbieding bevat de volgende typen op de storefronts in de marketplace.  

| Type aanbieding | Storefront | Details |  
|:---        |:---        |:---     |  
| Adviesdiensten | AppSource | Vereisten: AppSource: Lijst met: Adviesdiensten |  
| Adviesdiensten | Azure Marketplace | Vereisten: Azure Marketplace: Lijst met: Adviesdiensten |  
| Contact opnemen | AppSource | [](#) |  
| Contact opnemen | Azure Marketplace | Vereisten: AppSource: Lijst met: Contact opnemen |  

#### <a name="requirements-appsource-list-consulting-service"></a>Vereisten: AppSource: Lijst met: Consulting Services  

| Vereisten | Details |  
|:--- |:--- |  
| Kenmerken van de service-aanbieding | Uw advies-service moet voldoen aan de volgende criteria voldoen.<ul> <li>Lever een engagement vaste bereik, vaste duur, vaste prijs (of gratis).</li> <li>De plaatsen volgens voornamelijk voor voorverkoop.</li> <li>Beperkt tot één klant.</li> <li>Uitvoeren op de site.</li> </ul> |  
| Vereisten voor Consulting Services partner | U voldoet aan de criteria in het desbetreffende gebied voor uw service.<table><tr><th>Oplossingsgebied</th><th>Criteria</th></tr><tr><td>Dynamics 365 for Customer Engagement</td><td>Silver- of Gold Cloud Customer Relationship Management competentie hebben.</td></tr><tr><td>Dynamics 365 voor Finance and Operations, Enterprise edition</td><td>Silver- of Gold Enterprise Resource Planning competentie en opbrengst van uw cloudbewerkingen in de afsluitende 12 maanden van $25.000 of meer hebben.</td></tr><tr><td>Dynamics 365 for Finance and Operations, Business edition</td><td>Fungeren als Cloud Services-Provider (CSP) of digitale Partner van Record (DPOR) voor een of meer klanten.</td></tr><tr><td>Power BI</td><td>Voldoen aan de criteria Solution Partner.</td></tr><tr><td>PowerApps</td><td>Een oplossing Partnershowcase hebt.</td></tr></table><ul> <li>Voor meer informatie over klantrelatiebeheer, gaat u naar de Cloud Customer Relationship Management-pagina te vinden op [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Voor meer informatie over het plannen van resources, gaat u naar de Enterprise Resource Planning pagina zich bevindt op [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>Ga naar de pagina te vinden op Cloud Services-Provider voor meer informatie over CSP [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>Voor meer informatie over DPOR, gaat u naar de Digital Partner of Record en koppeling van de Partner-pagina te vinden op [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Voor meer informatie over solution partner criteria, gaat u naar het overzicht van de Partner van de oplossing en Incentives document dat zich bevindt in [www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf).</li> <li>Voor meer informatie over partnershowcase, gaat u naar de pagina te vinden op Partnershowcase [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Vereisten: Azure Marketplace: Lijst met: Consulting Services  

| Vereisten | Details |  
|:--- |:--- |  
| Kenmerken van de service-aanbieding | Uw advies-service moet voldoen aan de volgende criteria voldoen.<ul> <li>Lever een engagement vaste bereik, vaste duur, vaste prijs (of gratis).</li> <li>De plaatsen volgens voornamelijk voor voorverkoop.</li> <li>Beperkt tot één klant.</li> <li>Uitvoeren op de site.</li> </ul> |  
| Vereisten voor Consulting Services partner | U moet silver- of gold hebben in een van de volgende bevoegdheden in het desbetreffende gebied voor uw service. <table><tr><th>Oplossingsgebied</th><th>Competentie</th></tr><td>Cloud-Platform en infrastructuur</td><td>Cloud Platform<br />Datacentrum</td><tr><td>Ontwikkeling van toepassingen en ISV</td><td>Ontwikkeling van toepassingen<br />Integratie van toepassingen<br />DevOps</td></tr><tr><td>Gegevensbeheer van en analyses</td><td>Gegevensanalyse<br />Data Platform</td></tr></table><ul> <li>Voor meer informatie over de competenties die zijn, gaat u naar de competenties die via Microsoft Partner Network pagina zich bevindt op [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Voor meer informatie over de aanbieding, gaat u naar de Azure Marketplace Consulting Services pagina zich bevindt op [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---

### <a name="trial"></a>Proefversie  

| Type aanbieding | Storefront | Details |  
|:---        |:---        |:---     |  
| Gratis / SaaS-proef | AppSource | Vereisten voor het Type van aanbieding: Proefversie |  
| Gratis / SaaS-proef | Azure Marketplace | Vereisten: Azure Marketplace: Proefversie: Gratis proefversie / SaaS-proef |  
| Interactieve demo | AppSource | Vereisten voor het Type van aanbieding: Proefversie |  
| Interactieve demo | Azure Marketplace | [Vereisten: Azure Marketplace: Proefversie: Interactieve demo](#requirements-azure-marketplace-trial-interactive-demo) |  
| Test drive | AppSource | Vereisten voor het Type van aanbieding: Proefversie |  
| Test drive | Azure Marketplace | [Vereisten: Azure Marketplace: Proefversie: U kunt uitproberen](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Vereisten: Azure Marketplace: Proefversie  

| Vereiste | Details |  
|:--- |:--- |  
| Gratis proefperiode is afgelopen en evaluatiesoftware | De klant kan uw app gratis gebruiken voor een beperkte periode.<br /><br />Uw klant is niet vereist voor elke licentie of abonnement betalen voor de aanbieding of de app. De klant hoeft niet te betalen voor de onderliggende Microsoft eigen product of service. Alle opties voor proefversie worden geïmplementeerd naar uw Azure-abonnement. U hebt enige controle-proefversie van de kosten optimaliseren en het beheer.<br /><br />U kiest u een gratis proefversie, interactieve demo, of u kunt uitproberen. Ongeacht wat u kiest moet uw gratis proefversie bieden de klant een vooraf bepaalde hoeveelheid tijd om te proberen de app zonder extra kosten.<ul> <li>Om te beginnen met het proces voor het maken van een test uit, stuur een e-mail naar [ amp-testdrive@microsoft.com ](mailto:amp-testdrive@microsoft.com).</li> </ul>Opmerking: Azure Marketplace SaaS proefversie ervaringen moeten kunnen klanten werkreferenties gebruiken voor aanmelding bij.<ul> <li>Voor meer informatie gaat u naar de sectie van de proefversie ervaringen te vinden op AppSource [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Eenvoudig kunnen worden geconfigureerd, kant-en-klare oplossing | Uw app moet zijn eenvoudig en snel te configureren en instellen. |  
| Beschikbaarheid / uptime | Uw SaaS-app of -platform moet een uptime van ten minste 99,9% hebben. |  
| Azure Active Directory | Uw aanbieding moet toestaan dat Azure Active Directory (Azure AD) voor federatieve eenmalige aanmelding (SSO) (Azure AD federatieve SSO) met toestemming ingeschakeld. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Vereisten: Azure Marketplace: Proefversie: Gratis proefversie / SaaS-proef  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Hiermee kunt een klant om te proberen uw product voordat kopen via een geautomatiseerde methode voor de conversie naar betaald gebruik. Bovendien kunnen bewijzen van concept voor de klant en de gezamenlijke samenwerking met Microsoft-verkoopteam. | Uw oplossing is een virtuele machine of sjabloon-oplossing.<br /><br />Uw oplossing is een SaaS-aanbieding en bieden van een multitenant SaaS-product.<br /><br />U hebt een eerste sessie aan de slag van een klant snel te gaan.<br /><br />U hebt één tenant, maar klanten als gastgebruikers wilt toevoegen. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Vereisten: Azure Marketplace: Proefversie: Interactieve demo  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Kunnen uw klanten om te zien van uw oplossing in actie zonder de complexiteit van het instellen van. | Uw oplossing vereist complexe instellingen die moeilijk te bereiken binnen de proefperiode is afgelopen. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Vereisten: Azure Marketplace: Proefversie: Test drive  

| Voordeel | Vereiste |  
|:--- |:--- |  
| Hiermee kunt een klant om te proberen uw product voordat ze kopen.<br /><br />Biedt een begeleide ervaring van uw oplossing met behulp van vooraf geconfigureerde instellingen.<br /><br />Hieronder vindt u aanvullende voordelen bij het gebruik van een test uit.<ul> <li>27% van de zoekopdrachten van de gebruiker op de marketplace worden door gebruikers alleen show aanbiedingen met proeven verfijnd.</li> <li>Aanbiedingen met proeven leads van 38% meer dan biedt zonder te genereren.</li> <li>36% van de nieuwe klant aankopen op marketplace, zijn afkomstig van klanten die nodig was een test uit.</li> <li>Test drives inschakelen verkopers in het veld Microsoft voor meer informatie over uw product voor gezamenlijke verkoop inspanningen.</li> </ul> | Uw oplossing is een virtuele machine, oplossingssjabloon of SaaS-app met één tenant, of is ingewikkeld om in te richten. <br /><br />U hebt geen een manier om uw evaluatieversie converteren naar een betaald abonnement. |  

---

## <a name="transact-specific-listings"></a>Transact-specifieke aanbiedingen

### <a name="transact"></a>Handelen  

| Type aanbieding | Storefront | Details |   
|:---        |:---        | :--- |  
| Azure-apps: Beheerde app | Azure Marketplace | Vereisten: Azure Marketplace: Transact: Azure-apps: Beheerde app |  
| Azure-apps: Oplossingssjabloon | Azure Marketplace | Vereisten: Azure Marketplace: Transact: Azure-apps: Oplossingssjabloon |  
| Containers | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: Container](#requirements-azure-marketplace-transact-container) |  
| SaaS-app  | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: SaaS-app](#requirements-azure-marketplace-transact-saas-app) |  
| Virtuele machine | Azure Marketplace | [Vereisten: Azure Marketplace: Transact: Virtuele machine](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Vereisten: Azure Marketplace: Transact: Container  

| Vereiste | Details |  
|:--- |:--- |  
| Facturering en betaling naar gebruik | Ondersteuning voor een van beide de gratis of BYOL-model voor facturering. |  
| Docker-installatiekopie | Om de containerinstallatiekopie moet worden gebaseerd op de Docker-installatiekopieformaat en moet worden opgehaald uit Azure Container Registry. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Vereisten: Azure Marketplace: Transact: SaaS-app  

| Vereiste | Details |  
|:--- |:--- |  
| Facturering en betaling naar gebruik | De prijs van uw aanbieding geldt een maandelijkse vast tarief. Gebruik op basis van prijs- en op basis van gebruik *true-ups* opties worden niet ondersteund op dit moment. |  
| Annulering | Er is op elk gewenst moment uw aanbieding is annuleerbaar door de klant. |  
| Landingspagina van transactie | Een Azure gezamenlijke transactie landingspagina hosten. Uw startpagina kan uw klanten uw SaaS-service-account maken en beheren. |  
| SaaS-abonnement API | Geef een service die communiceert met de SaaS-abonnement te maken, bijwerken en verwijderen van een gebruiker account en service-plan. Alle kritieke API-wijzigingen moeten worden ondersteund binnen 24 uur. Niet-kritieke API wijzigingen worden regelmatig bijgewerkt. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Vereisten: Azure Marketplace: Transact: Virtuele machine  

| Vereiste | Details |  
|:--- |:--- | 
| Facturering en betaling naar gebruik | Uw virtuele machine moet ondersteunen BYOL of betalen per gebruik maandelijkse facturering. |  
| Azure-compatibele virtuele harde schijf (VHD) | Virtuele machines moeten worden gebouwd op Windows of Linux.<ul> <li>Zie voor meer informatie over het maken van een Linux VHD [op Azure onderschreven Linux-distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Zie voor meer informatie over het maken van een Windows VHD [maken van een Azure-compatibele VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |  

## <a name="next-steps"></a>Volgende stappen
*   Ga naar de [Azure Marketplace en AppSource Uitgevershandleiding voor](./marketplace-publishers-guide.md) pagina.  

