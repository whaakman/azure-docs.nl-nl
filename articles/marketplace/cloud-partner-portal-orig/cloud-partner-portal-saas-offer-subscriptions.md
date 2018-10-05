---
title: SaaS - verkopen via Azure | Microsoft Docs
description: Beschrijving van het abonnement facturering model voor SaaS-toepassingen en hoe u deze implementeert.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: f193138fbc5e779c3a6671757320d8918e08db46
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809975"
---
<a name="saas---sell-through-azure"></a>SaaS - verkopen via Azure
=========================

Dit artikel wordt beschreven voor het abonnement factureringsmodel voor SaaS-toepassingen en hoe u deze implementeert in de Cloud Partner-Portal.


<a name="overview"></a>Overzicht
--------

Azure marketplace kunt u om te publiceren en verkopen van uw SaaS-aanbiedingen via de Azure marketplace. Voor uw klanten ze kunnen nu worden in rekening gebracht via Azure direct de facturering en ze alleen ontvangt een factuur van Azure voor alle resources en services die ze hebben geactiveerd. Voor publicatie hebben SaaS-abonnementen de volgende voordelen:

-   **Consistente ervaring voor publiceren** --als u al SaaS-aanbiedingen publiceren of een andere aanbieding in Azure Marketplace, de publicatie-ervaring via de portal voor het publiceren van dezelfde is.
-   **Nieuwe winkel voor detectie** --alle aanbiedingen die gepubliceerd zijn zichtbaar in de externe Azure Marketplace-webwinkel, evenals de extensie Azure Marketplace in Azure portal.
-   **Geïntegreerde facturering** -u kunt nu verkopen in alle regio's waar Azure wordt verkocht en laat Azure regelt de facturering voor u.
-   **Genereren van leads geïntegreerd** --u kunt nu automatisch ontvangen potentiële klanten van Azure in de CRM van uw keuze, wanneer een Azure-gebruiker zich abonneert op uw SaaS-service met behulp van Azure marketplace.
-   **Gezamenlijke verkoop met Microsoft-verkopers** --u kunt een in aanmerking voor bi-directioneel delen van leads, geprioriteerde catalogusvermeldingen en de mogelijkheid om te oefenen en sluit gezamenlijke verkoop side-by-side met Microsoft-verkopers.

<a name="billing-models"></a>Facturering-modellen
--------------

Het enige ondersteunde factureringsmodel is een vaste maandelijkse bedrag per abonnement van uw SaaS-service.

**Opmerking:** extra factureringsmodellen mogelijk beschikbaar zijn op een later tijdstip.

Elke SaaS-aanbieding hebben een of meer abonnementen (bijvoorbeeld: Basic of Premium). Elk abonnement heeft enkele elementaire metagegevens die zijn gekoppeld aan dit, samen met de prijs die zijn gekoppeld aan het abonnement.

Hebt u volledige controle over de definitie van een plan. Wat een Basic-abonnement vormen bijvoorbeeld? Het plan wordt gedefinieerd door u en u de tekst die nodig zijn om te beschrijven als onderdeel van de publicatie van uw plan kunt opgeven.

De prijs die zijn gekoppeld aan het plan is de vaste maandelijkse kosten die gebruikers van Azure betaalt naar gebruik van de service.

### <a name="what-is-not-supported-today"></a>Wat is niet vandaag nog ondersteund?

-   Facturering op basis van aangepaste eenheden--bijvoorbeeld een vaste prijs op basis van het aantal aanvragen.
-   Facturering op basis van de toewijzing van seat--kunt bijvoorbeeld gebruikers van Azure om aan te schaffen licenties op basis van het aantal gebruikers.

<a name="end-to-end-flow"></a>End-to-end-stroom
---------------

Het abonnement SaaS bieden stroom vanuit het perspectief van een eindgebruiker eerst

**Opmerking:** deze beschrijving van de stroom wordt ervan uitgegaan dat u uw SaaS-aanbieding in Azure marketplace met de naam 'Contoso-demo SaaS' hebt gepubliceerd.

![Gebruikersstroom voor SaaS-abonnement](media/saas-offer-subscription/saas-subscription-user-flow.png)

Een Azure-gebruiker kunt beschikken over de volgende interacties met uw SaaS-service:

-   Detecteren van uw SaaS-service in Azure Marketplace
-   Abonneren op uw SaaS-service in Azure
-   Navigeer naar de SaaS-service in Azure portal
-   Een account in de SaaS-service maken en beheren (plan wijzigen/verwijderen) het account in SaaS
-   Afmelden met de SaaS-service voor Azure-portal

<a name="discover-your-saas-service-in-azure-marketplace"></a>Detecteren van uw SaaS-service in Azure Marketplace
-----------------------------------------------

Wanneer gebruikers Azure Marketplace in Azure portal starten, zien ze een categorie met de naam 'Software as a service (SaaS)'.

![Detecteren van SaaS met categorie-menu](media/saas-offer-subscription/saas-category-menu.png)

Gebruikers kunnen ook zoeken naar de SaaS-service.

![Detecteren van SaaS met behulp van search](media/saas-offer-subscription/saas-cpp-search.png)

Gebruikers kunnen vervolgens de details van uw service te bekijken en klik vervolgens op **maken**.

![Het maken van een SaaS-abonnement](media/saas-offer-subscription/saas-create-subscription.png)

### <a name="subscribe-to-saas-service-in-azure"></a>Abonneren op SaaS-service in Azure

De gebruiker kan zich vervolgens aanmelden met de SaaS-service van Azure.

![Abonneren op een SaaS-service](media/saas-offer-subscription/saas-subscribe-signup.png)

Wanneer de gebruiker zich op uw SaaS-service abonneert, wordt de gebruiker de volgende informatie

-   Naam van de naam die gebruikers kunnen ontdekken of dit exemplaar van de SaaS-abonnement in Azure beheren.
-   Abonnement--De context van abonnement die ze willen koppelen facturering voor de SaaS-service.
-   Abonnement: Het SaaS service-plan die moeten worden geabonneerd.

De gebruiksvoorwaarden van document geleverd als onderdeel van de publicatie van de aanbieding wordt ook weergegeven aan de gebruiker voordat de gebruiker zich op de SaaS-service abonneert.

De gebruiker kan nu abonneren op de service door te klikken op **abonneren**.
Azure verzendt een melding in de portal nadat het abonnement voltooid is.

### <a name="navigate-to-the-saas-service-from-azure-portal"></a>Navigeer naar de SaaS-Service in Azure Portal

Gebruikers, klik vervolgens op **naar de resource gaan** te bekijken of beheren van hun SaaS-exemplaar voor het abonnement.

![Bekijken of beheren van uw SaaS-exemplaar](media/saas-offer-subscription/saas-go-to-resource.png)

De gebruiker wordt geïnformeerd dat ze het account eerst in de SaaS-service configureren moeten. De facturering begint nadat de SaaS-service Azure facturering waarschuwt, wanneer de gebruiker maakt een account op de site van SaaS-service is gestart.

![Configureer uw SaaS-exemplaar](media/saas-offer-subscription/saas-configure-account.png)

Wanneer de gebruiker klikt op **Account configureren**, wordt deze doorverwezen naar het eindpunt van de SaaS-service. Tijdens deze omleiding, wordt een token doorgegeven als een queryparameter. Bijvoorbeeld:

![SaaS account token](media/saas-offer-subscription/saas-account-token.png)

Noteer deze waarde van het token. Dit token is een eenvoudige en moet worden omgezet naar een abonnements-id ophalen in Azure.

<a name="creating-a-saas-offer-subscription"></a>Het maken van een SaaS-aanbieding-abonnement
----------------------------------

Voor het bouwen van deze ervaring, zijn er twee onderdelen werk die vereist zijn:

-   Verbinding maken met de website van uw SaaS-service met Microsoft\'s SaaS APIs. Dit document [SaaS verkopen via Azure - API's](./cloud-partner-portal-saas-subscription-apis.md) wordt uitgelegd hoe u deze verbinding te maken.  
-   Schakel **verkopen via Azure** op Cloud Partner-Portal in de **technische informatie** uit en bieden alle van de configuratiegegevens van uw.

![Nieuwe SaaS bieden technische informatie](media/cpp-creating-saas-offers/saas-new-offer-technical-info-2.png)

Hieronder volgt een uitleg van alle vereiste velden voor de **technische Info** sectie:

|  **Velden van aanbod**                 |  **Beschrijving**                                   |
|  ----------------                 |  -------------------------------------             |
| Preview-versie abonnement-id 's          | Alle de Azure-abonnement-id's gebruikt voor het testen van uw aanbieding in preview, voordat deze algemeen beschikbaar is. |
| Aan de slag instructies      | Aanwijzingen voor het delen met uw klanten zodat ze verbinding maken met uw SaaS-app. Basisgebruik HTML is toegestaan, zoals tags als `<p>`, `<h1>`, `<li>`, enzovoort.  |
| URL landingspagina                  | De URL van de site die u worden zodat uw klanten land na het aanschaffen van Azure-portal. Deze URL worden ook het eindpunt dat de API's voor het vereenvoudigen van handel met Microsoft-verbinding wordt ontvangen.  |
| Verbinding Webhook                | Voor alle asynchrone gebeurtenissen die door Microsoft nodig heeft om u te sturen namens de klant (voorbeeld: Azure-abonnement is ongeldig geworden), moet je voor ons een webhook verbinding. Als u nog een webhook-systeem voldaan hebt, wordt de eenvoudigste configuratie is dat de logische App van een HTTP-eindpunt die luisteren naar gebeurtenissen worden gepubliceerd naar deze en klik vervolgens op de juiste manier verwerken.  Zie voor meer informatie, [aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten in logische apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint). |
| Azure AD-Tenant-ID en App-ID     | In Azure portal, vereist een Active Directory-App te maken, zodat de verbinding kan worden gevalideerd tussen de twee services zich achter een geverifieerde communicatie. Voor deze velden een AD-App maken en plak in de bijbehorende Tenant-Id en App-Id vereist. |
|  |  |


Ten slotte, als u selecteert **verkopen via Azure**, er is een extra sectie met de naam **plannen**. Abonnementen zijn alleen nodig als verkopen via Azure is geselecteerd. Deze sectie vindt u de specifieke schema's en hun bijbehorende prijzen die ondersteuning biedt voor uw SaaS-app. Vanaf vandaag kunnen we maandelijkse prijzen, de mogelijkheid om toe te staan voor 1 of 3-maanden gratis toegang. Deze abonnementen en prijzen moeten overeenkomen met de exacte plannen en prijzen die u op de site van uw eigen SaaS-app hebt.
