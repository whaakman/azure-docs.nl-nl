---
title: Een externe cache gebruiken in Azure API Management | Microsoft Docs
description: Informatie over het configureren en gebruiken van een externe cache in Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: f57b6b35ffff85aad4d970cf9aa908d2a80eadf1
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620898"
---
# <a name="use-an-external-redis-cache-in-azure-api-management"></a>Een externe Redis-cache gebruiken in Azure API Management

Behalve dat u de ingebouwde cache kunt gebruiken, biedt Azure API Management ook de mogelijkheid om antwoorden op te slaan in een externe Redis-cache.

Het gebruik van een externe cache heeft een aantal voordelen ten opzichte van de ingebouwde cache. Het is vooral nuttig in de volgende gevallen:

* Als u wilt vermijden dat uw cache periodiek wordt gewist tijdens het bijwerken van API Management
* Als u meer controle wilt over de configuratie van uw cache
* Als u meer gegevens in de cache wilt opslaan dan is toegestaan door uw API Management-laag
* Als u opslag in de cache wilt gebruiken met de laag Verbruik van API Management

Zie [De cachebeleidsregels van API Management](api-management-caching-policies.md) en [Aangepaste opslaan in de cache in Azure API Management](api-management-sample-cache-by-key.md) voor meer informatie over opslaan in de cache.

![Uw eigen cache gebruiken in API Management](media/api-management-howto-cache-external/overview.png)

Wat u leert:

> [!div class="checklist"]
> * Een externe cache toevoegen in API Management

## <a name="availability"></a>Beschikbaarheid

> [!NOTE]
> Deze functie is momenteel alleen beschikbaar in de laag **Verbruik** van Azure API Management.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

+ [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md)
+ Inzicht in [Aangepast opslaan in cache in Azure API Management](api-management-howto-cache.md)

## <a name="create-cache"> </a>Een Azure Redis-cache maken

In deze sectie wordt uitgelegd hoe u een Redis-cache maakt in Azure. Als u al een Redis-cache hebt, binnen of buiten Azure, kunt u <a href="#add-external-cache">verder gaan</a> met de volgende sectie.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Een externe cache toevoegen

Volg onderstaande stappen als u een externe Redis-cache wilt toevoegen in Azure API Management.

![Uw eigen cache gebruiken in API Management](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> De instelling **Vanuit** bepaalt welke regionale implementatie van API Management zal communiceren met de geconfigureerde cache in het geval van een configuratie met meerdere regio's van API Management. De cache die als **Standaard** is opgegeven, wordt overschreven door caches met een regionale waarde.
>
> Bijvoorbeeld, als API Management wordt gehost in de regio's US - oost, Azië - zuidoost en Europa - west, en er twee caches zijn geconfigureerd, één voor **Default** en één voor **Azië - zuidoost**, dan zal API Management in **Azië - zuidoost** een eigen cache gebruiken, terwijl de andere twee regio's de cachevermelding **Standaard** gebruiken.

### <a name="add-an-azure-redis-cache-from-the-same-subscription"></a>Een Azure Redis-cache toevoegen uit hetzelfde abonnement

1. Blader naar uw API Management-exemplaar in de Azure-portal.
2. Selecteer het tabblad **Externe cache** in het menu aan de linkerkant.
3. Klik op de knop **+ Toevoegen**.
4. Selecteer uw cache in de vervolgkeuzelijst **Cache-instantie**.
5. Selecteer **Standaard** of geef de gewenste regio op in het veld **Vanuit**.
6. Klik op **Opslaan**.

### <a name="add-a-redis-cache-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Een Redis-cache toevoegen die wordt gehost buiten het huidige Azure-abonnement of Azure in het algemeen

1. Blader naar uw API Management-exemplaar in de Azure-portal.
2. Selecteer het tabblad **Externe cache** in het menu aan de linkerkant.
3. Klik op de knop **+ Toevoegen**.
4. Selecteer **Aangepast** in de vervolgkeuzelijst **Cache-instantie**.
5. Selecteer **Standaard** of geef de gewenste regio op in het veld **Vanuit**.
6. Geef de verbindingsreeks van uw Redis-cache op in het veld **Verbindingsreeks**.
7. Klik op **Opslaan**.

## <a name="use-the-external-cache"></a>De externe cache gebruiken

Wanneer de externe cache in Azure API Management is geconfigureerd, kan deze worden gebruikt via cachebeleidsregels. Zie [Opslaan in cache toevoegen om de prestaties in Azure API Management te verbeteren](api-management-howto-cache.md) voor gedetailleerde stappen.

## <a name="next-steps"> </a>Volgende stappen
* Zie [Cachebeleidsregels][Caching policies] in [Naslaginformatie over beleid voor API Management][API Management policy reference] voor meer informatie over cachebeleidsregels.
* Zie [Aangepast opslaan in cache in Azure API Management](api-management-sample-cache-by-key.md) voor informatie over het opslaan van items in de cache per sleutel met behulp van beleidsexpressies.

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx