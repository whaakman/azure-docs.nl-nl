---
title: Abonnementen in Azure API Management | Microsoft Docs
description: Meer informatie over het concept van abonnementen in Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 45a65c7a94f3e6917b2882f27c9ad7d764ef97d7
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621813"
---
# <a name="subscriptions-in-azure-api-management"></a>Abonnementen in Azure API Management

Abonnementen is een belangrijk concept in Azure API Management (APIM). Dit is de meest voorkomende manier voor consumenten van de API voor toegang tot API's die zijn gepubliceerd via een APIM-instantie. Dit artikel bevat een overzicht van het concept.

## <a name="what-is-subscriptions"></a>Wat is er abonnementen

Bij het publiceren van API's met APIM, is de eenvoudigste en meest voorkomende manier voor het beveiligen van toegang tot deze API's met behulp van Abonnementssleutels. Met andere woorden, moeten ontwikkelaars die gebruikmaken van de gepubliceerde API's moeten een geldige Subscription-Key opnemen in HTTP-aanvragen bij het maken van deze API's aanroepen. Anders de aanroepen onmiddellijk worden geweigerd door de APIM-gateway en niet worden doorgestuurd naar de back-endservices.

Als u wilt een Abonnementssleutel voor toegang tot API's, is een abonnement vereist. Abonnement is in feite een benoemde container voor een combinatie van sleutel-abonnement. Abonnementen kunnen worden verkregen door ontwikkelaars die gebruikmaken van de gepubliceerde API's, met of zonder toestemming van de API-uitgevers willen. API-uitgevers kunnen ook abonnementen maken direct namens API consumenten.

> [!TIP]
> APIM biedt ook ondersteuning voor andere methoden voor het beveiligen van toegang tot API's waaronder [OAuth2.0](api-management-howto-protect-backend-with-aad.md), [clientcertificaten](api-management-howto-mutual-certificates-for-clients.md), en [IP-whitelists](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Bereik van abonnementen

Abonnementen kunnen worden gekoppeld aan verschillende bereiken: Product, alle API's of een afzonderlijke API.

### <a name="subscriptions-for-a-product"></a>Abonnementen voor een product

Traditioneel abonnementen in APIM zijn altijd gekoppeld aan één [API-product](api-management-terminology.md) bereik. Ontwikkelaars zou de lijst met producten vinden op de Portal voor ontwikkelaars en het Abonnementaanvragen verzenden voor de producten die ze graag willen gebruiken. Wanneer een aanvraag voor een abonnement is goedgekeurd (automatisch of door de API-uitgevers), kan de ontwikkelaar de sleutels in het gebruiken voor toegang tot alle API's in het product.

![Product-abonnementen](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> In bepaalde scenario's kunt API-uitgevers publiceren van een API-product voor het publiek zonder de noodzaak van abonnementen. Schakel kunnen de **-abonnement vereist** optie in de **instellingen** pagina van het product in de Azure-portal. Als gevolg hiervan zijn alle API's onder het product toegankelijk zonder API-sleutel.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Abonnementen voor alle API's of een afzonderlijke API

> [!NOTE]
> Deze functie is momenteel beschikbaar in de laag van API Management-verbruik alleen.

Wanneer we geïntroduceerd de [verbruik](https://aka.ms/apimconsumptionblog) laag van APIM, we enkele wijzigingen in het Sleutelbeheer stroomlijnen hebt gemaakt. Eerst hebben we twee meer abonnement bereiken - toegevoegd alle API's en één API. Het bereik van abonnementen is niet langer beperkt tot een API-product. Het is nu mogelijk te maken van sleutels toegang verlenen tot een API (of alle API's binnen een APIM-instantie), zonder een product maken en de API's eerst aan toe te voegen. Bovendien elk APIM-instantie wordt nu geleverd met een onveranderbare, alle-API's-abonnement, waardoor het eenvoudiger om te testen en fouten opsporen in API's in de Test-console en gemakkelijker.

Ten tweede kunt APIM nu "zelfstandige" abonnementen. Abonnementen zijn niet langer vereist om te worden gekoppeld aan een developer-account. Dit is handig in scenario's zoals wanneer een abonnement wordt gedeeld door meerdere ontwikkelaars of teams.

Ten slotte, API-uitgevers kunnen nu [abonnementen maken](api-management-howto-create-subscriptions.md) rechtstreeks in de Azure-Portal:

![Flexibele abonnementen](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over API Management:

+ Informatie over andere [concepten](api-management-terminology.md) in API Management
+ Volg onze [zelfstudies](import-and-publish.md) voor meer informatie over API Management
+ Controleer onze [pagina met veelgestelde vragen](api-management-faq.md) voor veelgestelde vragen