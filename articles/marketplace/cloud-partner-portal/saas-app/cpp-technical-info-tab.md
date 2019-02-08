---
title: Azure SaaS-aanbieding technische Toepassingsconfiguratie | Microsoft Docs
description: Configureer technische informatie voor SaaS-toepassing aanbieding op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: f0f6bbf5f235d6ae3e86114e583d55c502296470
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895394"
---
# <a name="saas-application-technical-info-tab"></a>Tabblad voor SaaS-toepassing technische gegevens

Het tabblad technische gegevens bevat het configuratieformulier voor technische. Gebruik dit formulier kiezen van het type SaaS-toepassing (app) u wilt maken en configureren hoe uw app wordt geleverd aan klanten.

![Technische configuratieformulier](./media/saas-techinfo-techconfig.png)

## <a name="technical-configuration-form"></a>Technische configuratieformulier

Dit formulier heeft 2 velden: Product en aanbieding.

### <a name="product-field"></a>Productveld

U kunt een SaaS-app opgeven voor beide van de volgende storefronts:
- Een zakelijke gebruiker door het selecteren van de **aanbieding** optie.
- Voor een IT-beheerder, door te selecteren **verkopen via Microsoft**.
Lezen om te bepalen welk type SaaS-app u bouwt, [begrijpen storefront selectie](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type#understand-storefront-selection).

#### <a name="sell-through-microsoft"></a>Verkopen via Microsoft
Als u wilt maken van deze ervaring die u wilt configureren van de volgende onderdelen:

- Verbinding maken met de website van uw SaaS-service met Microsofts SaaS APIs. De [SaaS verkopen via Azure-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-subscription-apis) artikel wordt uitgelegd hoe u deze verbinding te maken.
- Verkopen via Azure in de Cloud Partner-Portal inschakelen in het configuratieformulier voor technische en geef de vereiste gegevens. Zie voor meer informatie over dit factureringsmodel en hoe deze geïmplementeerd, [SaaS-verkopen via Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-offer-subscriptions).

 ![Verkopen via Microsoft-formulier](./media/saas-techinfo-sellthrough-ms.png)

De volgende tabel beschrijft de vereiste velden voor verkopen via Microsoft.

|  **Veldnaam**   |  **Beschrijving**  |
|  ---------------  |  ---------------  |
|    Preview-versie abonnement-id 's               |    Alle de Azure-abonnement-id's gebruikt voor het testen van uw aanbieding in preview, voordat deze algemeen beschikbaar is.               |
|     Aan de slag instructies              |   Aanwijzingen voor het delen met uw klanten zodat ze verbinding maken met uw SaaS-app. Eenvoudige HTML-codes zijn toegestaan, bijvoorbeeld: &lt;p&gt;, &lt;h1&gt;, &lt;li&gt;, enzovoort.                |
|    URL landingspagina  |   De URL van de site die u worden zodat uw klanten land na het aanschaffen van Azure-portal. Deze URL worden ook het eindpunt dat de API's voor het vereenvoudigen van handel met Microsoft-verbinding wordt ontvangen.                |
|  Verbinding Webhook    |  Voor alle asynchrone gebeurtenissen die door Microsoft nodig heeft om u te sturen namens de klant (voorbeeld: Azure-abonnement is geworden ongeldige), moet je voor ons een webhook verbinding. Als u nog een webhook-systeem voldaan hebt, wordt de eenvoudigste configuratie is dat de logische App van een HTTP-eindpunt die luisteren naar gebeurtenissen worden gepubliceerd naar deze en klik vervolgens op de juiste manier verwerken. Zie voor meer informatie, <a href="https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint">aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten in logische apps</a>                |
|  Azure AD-Tenant-ID en App-ID      |   In Azure portal, vereist een Active Directory-App te maken, zodat de verbinding kan worden gevalideerd tussen de twee services zich achter een geverifieerde communicatie. Voor deze velden een AD-App maken en plak in de bijbehorende Tenant-Id en App-Id vereist. Houd er rekening mee dat de App-id gekoppeld aan uw publisherID is. Daarom, zorg ervoor dat dezelfde App-ID zoals in alle aanbiedingen.             |


Ten slotte, als u selecteert **verkopen via Microsoft**, er is een andere nieuwe aanbieding tabblad met de naam **plannen**. 

De [tabblad abonnementen](./cpp-plans-tab.md) geeft een lijst van de specifieke schema's en hun bijbehorende prijzen die ondersteuning biedt voor uw SaaS-app. Vanaf vandaag kunnen we maandelijkse prijzen, de mogelijkheid om toe te staan voor 1 of 3-maanden gratis toegang. Deze abonnementen en prijzen moeten overeenkomen met de exacte plannen en prijzen die u op de site van uw eigen SaaS-app hebt.

>[!NOTE] 
>Abonnementen zijn alleen nodig als u ervoor waarvoor verkopen via Microsoft kiest.

### <a name="call-to-action-field"></a>Aanzetten tot actieveld

De aanroep van actieveld kunt u het bericht dat wordt weergegeven op van uw aanbieding overname knop kiezen. De volgende opties zijn beschikbaar:

- Gratis: als u deze optie is, kiest u wordt gevraagd om in te voeren van de URL van een proefversie waar klanten toegang tot uw SaaS-app krijgen kunnen. Bijvoorbeeld: https://contoso.com/trial
- Gratis proefversie – als u Kies deze optie, wordt u gevraagd om in te voeren van de URL van een proefversie waar klanten toegang tot uw SaaS-app krijgen kunnen. Bijvoorbeeld: https://contoso.com/trial
- Contact opnemen

Zie voor meer informatie over het aanroepen van actie-opties, kies een optie voor publiceren.

## <a name="next-steps"></a>Volgende stappen

- [Tabblad abonnementen (optioneel)](./cpp-plans-tab.md)
- [Tabblad Informatiekanaal](./cpp-channel-info-tab.md)
