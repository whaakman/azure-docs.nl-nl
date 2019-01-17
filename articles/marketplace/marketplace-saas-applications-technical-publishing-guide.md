---
title: Azure Marketplace SaaS-toepassingen technische Publicatiehandleiding voor
description: Stapsgewijze handleiding en publiceren controlelijsten voor het publiceren van SaaS-toepassingen op Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: keithcharlie
manager: nunoc
editor: keithcharlie
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: keithcharlie
ms.openlocfilehash: 19600ccbc2a424d210c686d7027b52bc6254de57
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352016"
---
# <a name="saas-applications-offer-publishing-guide"></a>SaaS-toepassingen bieden Publicatiehandleiding voor

SaaS-toepassingen kunnen worden gepubliceerd in de marketplace met drie verschillende aanroepen naar actie: 'Neem contact op met mij,' 'Nu uitproberen' en "Nu downloaden." Deze handleiding wordt uitgelegd dat deze drie opties, met inbegrip van de vereisten voor elk. 

## <a name="offer-overview"></a>Overzicht van de aanbieding  

SaaS-toepassingen zijn beschikbaar in zowel Azure Storefronts de volgende tabel beschrijft de huidige beschikbare opties:

| StoreFront optie | Aanbieding | Trial/Transact |  
| --- | --- | --- |  
| AppSource | Ja (contact met mij opnemen) | Ja (Power BI/Dynamics) |
| Azure Marketplace | Nee | Yes (SaaS Apps) |   

**Lijst met:**  De publicatieoptie vermelding bestaat uit een contactpersoon Me type aanbieding en wordt gebruikt wanneer de deelname van een evaluatieversie of transactieniveau op niet haalbaar is. Het voordeel van deze benadering is dat Hiermee uitgevers met een oplossing in de markt begint onmiddellijk met het ontvangen van leads die kunnen worden omgezet in deals kunnen te verhogen van uw bedrijf.  
**Proefversie/transactie:**  De klant beschikt over de optie voor het rechtstreeks kopen of vraag een proefversie voor uw oplossing. Leveren van een proefversie, verhoogt het niveau van de engagement aangeboden aan klanten en kan klanten voor het verkennen van uw oplossing voor het kopen. Met een evaluatie-ervaring, hebt u betere kans promotie in de storefronts en u kunt verwachten meer en uitgebreidere leads van interacties met klanten. Proefversies moeten ten minste gratis ondersteuning bevatten voor de duur van de proefperiode is afgelopen.  

| Aanbieding voor SaaS-Apps | Zakelijke vereisten | Technische vereisten |  
| --- | --- | --- |  
| **Contact opnemen** | Ja | Nee |  
| **PowerBI / Dynamics** | Ja | Ja (Azure AD-integratie) |  
| **SaaS-apps**| Ja | Ja (Azure AD-integratie) |     

## <a name="saas-list"></a>Lijst met SaaS

De aanbieding voor een SaaS-aanbieding met geen proefversie en er is geen facturering functionaliteit is "Neem contact op met me." 

U hoeft niet te configureren van Azure Active Directory om een SaaS-toepassing. 

|Vereisten  |Details  |
|---------|---------|
|Uw app is een SaaS-aanbieding  |   Uw oplossing is een SaaS-aanbieding en bieden van een multitenant SaaS-product.      |


## <a name="saas-trial"></a>SaaS Trial

U beschikt over een oplossing of de app met behulp van een gratis-naar-gebruik, software-as-a-service (SaaS)-op basis van de proefversie. Gratis proefversie aanbiedingen kunnen worden aangeboden als een proefaccount voor beperkt gebruik of beperkte duur. 


|Vereisten  |Details  |
|---------|---------|
|Uw app is een SaaS-aanbieding  |   Uw oplossing is een SaaS-aanbieding en bieden van een multitenant SaaS-product.      |
|Uw app is AAD ingeschakeld     |   De klant zich opnieuw gerichte naar uw domein en u rechtstreeks met de klant wordt transact       |


## <a name="saas-trial-technical-requirements"></a>SaaS proefversie technische vereisten

De technische vereisten voor SaaS-toepassingen kunnen eenvoudig. Uitgevers zijn alleen vereist voor het worden geïntegreerd met Azure Active Directory (Azure AD) om te worden gepubliceerd. Azure AD-integratie met toepassingen wordt goed gedocumenteerd en Microsoft biedt meerdere SDK's en resources om dit te realiseren.  

Als u wilt starten, is het raadzaam dat u een abonnement dat is bestemd voor uw Azure Marketplace publiceren hebt, zodat u kunt voor het isoleren van het werk van andere initiatieven. Als dit gebeurt kunt u beginnen met het implementeren van uw SaaS-toepassing in dit abonnement om te beginnen de projecten.  

De beste Azure Active Directory-documentatie, voorbeelden en richtlijnen zich bevinden op de volgende sites: 

* [Ontwikkelaarshandleiding voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure-Roadmap - beveiliging en identiteit](https://azure.microsoft.com/roadmap/?category=security-identity)

Zelfstudies, video, controleert u het volgende:

* [Azure Active Directory-verificatie met Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Azure Active Directory-identiteit technische Briefing - deel 1 van 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Azure Active Directory-identiteit technische Briefing - deel 2 van 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Het bouwen van Apps met Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Microsoft Azure-video's die zijn gericht op Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Gratis training voor Azure Active Directory is beschikbaar op  
* [Microsoft Azure for IT Pros Content-serie: Azure Active Directory](https://mva.microsoft.com/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Bovendien biedt Azure Active Directory een site om te controleren op Updates van de Service   
* [Azure AD-Service-updates](https://azure.microsoft.com/updates/?product=active-directory)

## <a name="using-azure-active-directory-to-enable-trials"></a>Met behulp van Azure Active Directory om in te schakelen proefversies  

Microsoft verifieert alle Marketplace-gebruikers met Azure AD, dus wanneer een geverifieerde gebruiker via de vermelding van uw proefversie in Marketplace klikt en wordt omgeleid naar de omgeving van uw proefversie, kunt u de gebruiker rechtstreeks in een proefversie inrichten zonder een Extra aanmeldingsstap. Het token dat uw app van Azure AD tijdens de verificatie ontvangt bevat waardevolle gebruikersgegevens die u gebruiken kunt om te maken van een gebruikersaccount in uw app, zodat u kunt voor het automatiseren van de inrichting ervaring en vergroot de kans van de conversie. Zie voor meer informatie over het token, [voorbeeld Tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims) .

Met behulp van Azure AD verificatie aan uw app of de proefversie van 1 en klik op inschakelen doet het volgende:  
* Stroomlijnt de klantervaring in Marketplace naar proefversie.  
* Het uiterlijk van een in-product ervaring onderhoudt, zelfs wanneer de gebruiker wordt omgeleid vanuit Marketplace aan uw domein of een evaluatieomgeving.  
* Vermindert de kans op stopzetting van omleiden omdat er een extra stap aanmelden.  
* Minder barrières voor implementatie voor het vullen van de grote van Azure AD-gebruikers.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Certificatie van uw Azure AD-integratie voor Marketplace  

U kunt certificeren van uw Azure AD-integratie in een aantal verschillende manieren, afhankelijk van of uw toepassing één tenant of meerdere tenants en of u nu nieuwe naar Azure AD federatieve eenmalige aanmelding (SSO), of al ondersteunen.  

**Voor toepassingen met meerdere tenants:**  

Als u al Azure AD ondersteunt, het volgende doen:
1.  Uw toepassing registreren in Azure portal
2.  De ondersteuning van multitenancy inschakelen in Azure AD een evaluatieversie 'met één muisklik'. Meer informatie vindt u [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Als u niet bekend bent met Azure AD voor federatieve eenmalige aanmelding, het volgende doen: 
1.  Uw toepassing registreren in Azure portal
2.  Ontwikkelen van eenmalige aanmelding met Azure AD via [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) of [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Ondersteuning voor multitenancy inschakelen functie in AAD om op te halen, met één muisklik' testervaring meer gedetailleerde informatie over vindt [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Voor één tenant-toepassing, moet u een van de volgende opties gebruiken:**  
* Gebruikers toevoegen aan uw directory als gastgebruikers met behulp van [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* Handmatig inrichten proefversies voor klanten met behulp van 'Contact opnemen'
* Een per-klant 'Test Drive' ontwikkelen
* Een voorbeeld van multitenant-demo bouwen met eenmalige aanmelding

## <a name="saas-subscriptions"></a>SaaS-abonnementen

Aanbiedingtype van SaaS-app gebruiken om in te schakelen van uw klant uw oplossing op basis van SaaS, technische als een abonnement kopen. De volgende vereisten wordt voldaan voor uw SaaS-app:
- Prijs en de factuur de service op een voorspelbaar, maandelijkse tarief.
- Bieden een methode voor het upgraden of de service op elk gewenst moment annuleren.
Microsoft als host fungeert voor de commercetransactie. Microsoft factureert de klant uit uw naam. Voor het gebruik van de factuur een SaaS-App als een abonnement, moet u de API voor eigen abonnement management-service inschakelen. Uw abonnement management service-API moet communiceren rechtstreeks met de Azure Resource Manager API's. Uw abonnement management service-API moet ondersteuning bieden voor service inrichten, upgraden en annuleren.

| Vereiste | Details |  
|:--- |:--- |  
|Facturering en betaling naar gebruik | De prijs van uw aanbieding geldt een maandelijkse vast tarief. Gebruik op basis van prijzen en mogelijkheden voor 'true '-up op basis van gebruik worden niet ondersteund op dit moment. |  
|Annulering | Er is op elk gewenst moment uw aanbieding is annuleerbaar door de klant. |  
|Landingspagina van transactie | U hosten een Azure gezamenlijke transactie landingspagina waar gebruikers kunnen maken en beheren van hun SaaS-serviceaccount. |   
| Abonnement API | U beschikbaar maakt voor een service die met de SaaS-abonnement communiceren kan te maken, bijwerken en verwijderen van een gebruiker account en service-plan. Kritieke API-wijzigingen moeten worden ondersteund binnen 24 uur. Niet-kritieke API-wijzigingen worden regelmatig uitgebracht. |  

## <a name="next-steps"></a>Volgende stappen
Als u dit nog niet hebt gedaan,

- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace.

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- [Meld u aan bij de Cloud Partner-Portal](https://cloudpartner.azure.com) maken of uitvoeren van uw aanbieding.
- Zie [aanbieding van Azure SaaS-toepassing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-offer) voor meer informatie.
