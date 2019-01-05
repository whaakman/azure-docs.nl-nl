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
ms.openlocfilehash: 1f7c2bde631e42ac94ce4c0394d13e369f6ea23f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54049755"
---
# <a name="subscriptions-in-azure-api-management"></a>Abonnementen in Azure API Management

Abonnementen zijn een belangrijk concept in Azure API Management. Ze zijn de meest voorkomende manier voor consumenten van de API voor toegang tot API's die zijn gepubliceerd via een API Management-exemplaar. Dit artikel bevat een overzicht van het concept.

## <a name="what-are-subscriptions"></a>Welke abonnementen zijn?

Wanneer u API's via API Management publiceert, is het eenvoudig en voor beveiligde toegang tot deze API's met behulp van abonnementssleutels. Ontwikkelaars die gebruikmaken van de gepubliceerde API's willen moeten een geldig abonnement-sleutel opnemen in de HTTP-aanvragen wanneer ze deze API's aanroepen. Anders wordt worden de aanroepen onmiddellijk geweigerd door de API Management-gateway. Ze worden niet doorgestuurd naar de back-end-services.

Als u een abonnementssleutel voor toegang tot API's, is een abonnement vereist. Een abonnement is in feite een benoemde container voor een combinatie van sleutel abonnement. Ontwikkelaars die gebruikmaken van de gepubliceerde API's willen krijgt abonnementen. En ze hoeft goedkeuring van de API-uitgevers. API-uitgevers kunnen ook abonnementen direct voor de API-consumenten maken.

> [!TIP]
> API Management biedt ook ondersteuning voor andere methoden voor het beveiligen van toegang tot API's, met inbegrip van de volgende voorbeelden:
> - [OAuth 2.0](api-management-howto-protect-backend-with-aad.md)
> - [Clientcertificaten](api-management-howto-mutual-certificates-for-clients.md)
> - [IP-whitelists](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Bereik van abonnementen

Abonnementen kunnen worden gekoppeld aan verschillende bereiken: product, alle API's of een afzonderlijke API.

### <a name="subscriptions-for-a-product"></a>Abonnementen voor een product

Traditioneel abonnementen in API Management zijn altijd gekoppeld aan één [API-product](api-management-terminology.md) bereik. De lijst met producten ontwikkelaars gevonden op de Portal voor ontwikkelaars. Verzend dan ze abonnementaanvragen voor de producten die ze wilden gebruiken. Nadat een aanvraag is goedgekeurd, automatisch of door de API-uitgevers, kan de ontwikkelaar de sleutels in het gebruiken voor toegang tot alle API's in het product.

![Product-abonnementen](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> In bepaalde scenario's wilt API-uitgevers publiceren van een API-product voor het publiek zonder de noodzaak van abonnementen. Schakelt ze uit de **-abonnement vereist** kiezen op de **instellingen** pagina van het product in de Azure-portal. Als gevolg hiervan zijn alle API's onder het product toegankelijk zonder API-sleutel.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Abonnementen voor alle API's of een afzonderlijke API

> [!NOTE]
> Deze functie is momenteel beschikbaar in de laag van API Management-verbruik alleen.

Wanneer we geïntroduceerd de [verbruik](https://aka.ms/apimconsumptionblog) laag van API Management kunnen we enkele wijzigingen in het Sleutelbeheer stroomlijnen gemaakt:
- Eerst hebben we twee meer abonnement bereiken toegevoegd: alle API's en één API. Het bereik van abonnementen is niet langer beperkt tot een API-product. Het is nu mogelijk te maken van sleutels die toegang aan een API of alle API's binnen een exemplaar van API Management, verlenen zonder een product maken en de API's eerst aan toe te voegen. Bovendien elk exemplaar van API Management wordt nu geleverd met een onveranderbare, alle-API's-abonnement. Dit abonnement kunt u gemakkelijker en eenvoudiger om te testen en fouten opsporen in API's in de testconsole.

- Ten tweede API Management nu kunnen **zelfstandige** abonnementen. Abonnementen zijn niet langer vereist om te worden gekoppeld aan een developer-account. Deze functie is handig in scenario's, zoals wanneer meerdere ontwikkelaars of teams een abonnement deelt.

- Ten slotte, API-uitgevers kunnen nu [abonnementen maken](api-management-howto-create-subscriptions.md) rechtstreeks in Azure portal:

    ![Flexibele abonnementen](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over API Management:

+ Informatie over andere [concepten](api-management-terminology.md) in API Management.
+ Volg onze [zelfstudies](import-and-publish.md) voor meer informatie over API Management.
+ Controleer onze [pagina met veelgestelde vragen](api-management-faq.md) voor veelgestelde vragen.