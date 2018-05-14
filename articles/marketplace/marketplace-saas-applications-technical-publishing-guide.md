---
title: Azure Marketplace SaaS Applications Technical Guide publiceren
description: Stapsgewijze handleiding en publishing controlelijsten voor het publiceren van SaaS-toepassingen in Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: BrentL-Collabera
manager: ''
editor: BrentL-Collabera
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.author: pabutler
ms.openlocfilehash: 48b0b4177dad6262105bf30be2b8714f6ea1228f
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>Technische publishing handleiding van SaaS-toepassingen

Welkom bij de Azure Marketplace SaaS-toepassingen technische handleiding publiceren. Deze handleiding is ontworpen om u te helpen candidate en bestaande uitgevers in de lijst van hun toepassingen en services in Azure Marketplace met de SaaS-toepassingen aanbieden. 

Deze handleiding is om beter te begrijpen hoe u een SaaS-aanbieding publiceert, onderverdeeld in de volgende secties:
* Overzicht van aanbieding
* Zakelijke vereisten
* Technische vereisten
* Publicatieproces
* Met behulp van Azure Active Directory proefversies inschakelen
* Gecertificeerd door uw Azure AD-integratie voor Marketplace

## <a name="offer-overview"></a>Overzicht van aanbieding  

SaaS-toepassingen zijn beschikbaar in beide winkelobjecten Azure de volgende tabel beschrijft de huidige beschikbare opties:

| Via optie | Aanbieding | Proefversie/Transact |  
| --- | --- | --- |  
| AppSource | Ja (contact met mij opnemen) | Ja (Power BI/Dynamics) |
| Azure Marketplace | Nee | Ja (SaaS-Apps) |   

**Aanbieding:** de publicatieoptie aanbieding bestaat uit een contactpersoon Me aanbiedingstype en wordt gebruikt als een evaluatie - of transactieniveau deelname niet haalbaar is. Het voordeel van deze benadering is dat het mogelijk uitgevers met een oplossing in markt maakt naar onmiddellijk te ontvangen van leads die kunnen worden omgezet in aanbiedingen te verhogen van uw bedrijf.  
**Proefversie/Transact:** de klant heeft de optie voor het rechtstreeks kopen of vraag een proefversie voor uw oplossing. Een proefversie-ervaring bieden, verhoogt het niveau van de engagement aangeboden aan klanten en kan klanten verkennen van uw oplossing voordat u koopt. Een proefversie-ervaring bieden, hebt u betere kans verkleind dat bij promotie in de winkelobjecten en houd rekening met meer en uitgebreidere leads van betrokkenheid van de klant. Proefversies moeten ten minste gratis ondersteuning bevatten voor de duur van de proefperiode.  

| Aanbieding voor SaaS-Apps | Zakelijke vereisten | Technische vereisten |  
| --- | --- | --- |  
| **Contact met ons opnemen** | Ja | Nee |  
| **Power BI / Dynamics** | Ja | Ja (Azure AD-integratie) |  
| **SaaS-Apps**| Ja | Ja (Azure AD-integratie) |     

Zie voor meer informatie over de Marketplace-winkelobjecten en een beschrijving van elke optie publishing de [Marketplace Publisher handleiding](https://aka.ms/sellerguide) en [publicatieopties](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#select-a-publishing-option).

## <a name="business-requirements"></a>Zakelijke vereisten
De SaaS bieden bedrijfsvereisten in combinatie met de technische vereisten kunnen worden voltooid. De meeste zakelijke vereisten en gegevens worden verzameld tijdens het bouwen van de SaaS-aanbieding in de Cloud Partner-Portal. De zakelijke vereisten zijn de volgende: 
* Akkoord gaat met de beleidsregels voor deelname
* Integratie met Microsoft 
* De aanbieding doelgroep identificeren
* Definieer en bepalen de lead management moet worden gebruikt
* Instellen van het privacybeleid en gebruiksvoorwaarden
* Ondersteuning voor contactpersonen definiëren  

Voor meer informatie kunt u vinden in het onderwerp [vereisten voor het publiceren van de marketplace](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#prerequisites-for-marketplace-publishing)

## <a name="technical-requirements"></a>Technische vereisten

De technische vereisten voor SaaS-toepassingen zijn eenvoudige. Uitgevers hoeven alleen te worden geïntegreerd met Azure Active Directory (Azure AD) worden gepubliceerd. Azure AD-integratie met toepassingen goed wordt gedocumenteerd en Microsoft biedt meerdere SDK's en bronnen om dit te realiseren.  

Als u wilt starten, is het raadzaam dat u een abonnement specifiek voor uw Azure Marketplace publiceren hebt, zodat u kunt het isoleren van de andere initiatieven te werken. Wanneer dit wordt gedaan kunt u beginnen met het implementeren van uw SaaS-toepassing in dit abonnement starten van de projecten.  

De beste Azure Active Directory-documentatie, voorbeelden en richtlijnen zich bevinden op de volgende sites: 

* [Ontwikkelaarshandleiding Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

* [Integreren met Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-how-to-integrate)

* [Toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

* [Overzicht van Azure - beveiliging en identiteit](https://azure.microsoft.com/roadmap/?category=security-identity)

Video's voor zelfstudies, controleert u het volgende:

* [Azure Active Directory-verificatie met Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Azure Active Directory identiteit technische gegevens meest - deel 1 van 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Azure Active Directory identiteit technische gegevens meest - deel 2 van 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Apps bouwen met Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Microsoft Azure-video's die zijn gericht op het Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Gratis Azure Active Directory-training is beschikbaar op  
* [Microsoft Azure voor IT-professionals inhoud reeks: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Bovendien biedt Azure Active Directory een site om te controleren op Updates van de Service   
* [Azure AD-Service-updates](https://azure.microsoft.com/updates/?product=active-directory)

Voor ondersteuning, kunt u de volgende bronnen:
* [MSDN-forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="publishing-process"></a>Publicatieproces

Technische en zakelijke stappen voor het publicatieproces SaaS zijn.  De meeste van het werk dat is gedaan ontwikkelen en integratie van Azure Active Directory kan worden gedaan in combinatie met het werk dat nodig is om te voldoen aan de zakelijke vereisten van de aanbieding. Het merendeel van de bedrijfsvereisten deel uitmaken van de configuratie van de SaaS-App Offer het Cloud Partner-Portal.  
Het volgende diagram toont dat de belangrijkste publishing stappen voor de proefversie/Transact bieden:  

![SaaS publishing stappen](./media/marketplace-saas-applications-technical-publishing-guide/saaspublishingsteps.png)  

De volgende tabel beschrijft de belangrijkste stappen publishing beschreven:  

| Publishing stap | Beschrijving |   
| --- | --- |  
| **De SaaS-toepassing maken** | Meld u aan de Cloud Partner-Portal, selecteer bij **nieuw**, en selecteer vervolgens de **SaaS-apps** bieden. |  
| **Maken van de integratie met Azure AD** | Volg de technische vereisten die worden beschreven in de vorige sectie voor het integreren van uw SaaS aanbieding met Azure AD. |  
| **De instellingen van aanbieding**| Voer alle SaaS aanbieding initiële gegevens. De ID bieden en bieden naam die u wilt gebruiken. |     
| **Instellen van de technische informatie** | Geef de technische informatie over de aanbieding. De URI en het type van de aanbieding overname knop (vrije, spoor of neem contact op met mij) van de oplossing zijn vereist voor SaaS-toepassingen. |  
| **Drive(Optional) testen** | Dit is een optionele type proefversie, die nodig zijn voornamelijk voor andere typen van Marketplace biedt. Hiermee kunt u de evaluatieversie geïmplementeerd in de Publisher abonnementen versus eindgebruikers hebben. |  
| **De aanbieding via materialen instellen**| In dit gedeelte wordt de uitgever koppelen en logo's, marketingmateriaal, juridische documenten uploaden en configureren van het beheersysteem voor potentiële klanten. |
| **Instellen van de contactpersonen van aanbieding** | Voer de Engineering-contactpersonen en de contactgegevens voor ondersteuning voor de SaaS-aanbieding. |  
| **Controleer of de SaaS-App Azure AD-integratie** | Voordat u uw SaaS-app voor publicatie verzendt, moet u controleren dat de app is geïntegreerd met Azure AD |  
| **De aanbieding publiceren**| Nadat de aanbieding en de technische activa zijn voltooid, kunt u de aanbieding kunt verzenden. Hiermee start u het publicatieproces waarin de oplossingssjabloon is getest, gevalideerd, gecertificeerd en goedgekeurd voor publicatie. |

## <a name="using-azure-active-directory-to-enable-trials"></a>Met behulp van Azure Active Directory proefversies inschakelen  

Microsoft verifieert alle Marketplace-gebruikers met Azure AD, daarom wanneer een geverifieerde gebruiker klikt op via uw proefversie aanbieding in Marketplace en wordt omgeleid naar uw omgeving proefversie, kunt u de gebruiker rechtstreeks in een proefversie inrichten zonder een aanvullende aanmelden stap. Het token dat uw app van Azure AD tijdens verificatie ontvangt bevat waardevolle gebruikersgegevens die u gebruiken kunt voor het maken van een gebruikersaccount in uw app zodat u kunt de ervaring voor de inrichting automatiseren en vergroot de kans van de conversie. Zie voor meer informatie over het token [voorbeeld Tokens](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims) .

Met behulp van Azure AD 1 Klik verificatie aan uw app of de evaluatieversie in te schakelen, gebeurt het volgende:  
* Stroomlijnt de gebruikerservaring van Marketplace evaluatieversie.  
* De werking van een product ervaring onderhoudt zelfs wanneer de gebruiker is omgeleid uit Marketplace aan uw domein of de proefversie omgeving.  
* Verkleint de kans op stopzetting op omleiding omdat er geen een aanvullende stap aanmelden.  
* Vermindert implementatie barrières voor het vullen van de grote van Azure AD-gebruikers.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Gecertificeerd door uw Azure AD-integratie voor Marketplace  

U kunt uw Azure AD-integratie in een aantal verschillende manieren, afhankelijk van uw toepassing enkel- of meerdere tenant certificeren en u niet bekend bent met Azure AD federatieve afzonderlijke eenmalige aanmelding (SSO) of bieden al ondersteuning.  

**Voor toepassingen met meerdere tenants:**  

Als u al Azure AD ondersteunen, het volgende doen:
1.  Uw toepassing registreren in de Azure-portal
2.  De multi-tenancymodus ondersteuning inschakelen in Azure AD om op te halen van een evaluatieversie van een enkele klik. Meer informatie vindt u [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).  

Als u niet bekend met Azure AD federatieve SSO bent, het volgende doen: 
1.  Uw toepassing registreren in de Azure-portal
2.  Eenmalige aanmelding ontwikkelen met Azure AD via [OpenID Connect](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) of [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Ondersteuning voor multitenancy functie in AAD ophalen van een enkele klik evaluatieversie meer specifieke informatie vindt u [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Voor één tenant toepassing, moet u een van de volgende opties gebruiken:**  
* Gebruikers toevoegen aan uw directory als gastgebruikers met [Azure B2B](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* Handmatig inrichten proefversies voor klanten met behulp van 'Contact Me'
* Een per-klant Test-station ontwikkelen
* Een voorbeeld van multitenant-demo-app met eenmalige aanmelding bouwen

