---
title: SaaS-API's voor vervulling | Azure Marketplace
description: De versies van de API's waarmee u uw SaaS integreren met de Azure Marketplace biedt vervulling introduceert.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: 7896ed77d9dbb3358ddb1c809ca342828280f66a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258922"
---
# <a name="saas-fulfillment-apis"></a>SaaS-uitvoerings-API's

De uitvoering van SaaS API's inschakelen independent software vendors (ISV) hun SaaS-toepassingen integreren met de Azure Marketplace. Deze API's kunnen ISV-toepassingen om deel te nemen in alle commerce ingeschakeld kanalen: direct, door partner geleide (reseller) en onder leiding van een veld.  Ze zijn vereist voor het aanbieden van transactable SaaS-aanbiedingen op Azure Marketplace.

> [!WARNING]
> De huidige versie van deze API is versie 2, die moet worden gebruikt voor alle nieuwe SaaS-aanbiedingen.  Versie 1 van de API is afgeschaft en wordt onderhouden voor de ondersteuning van bestaande aanbiedingen.


## <a name="business-model-support"></a>Ondersteuning voor bedrijven-model

Deze API biedt ondersteuning voor de volgende zakelijke model mogelijkheden; U kunt:

* Geef meerdere abonnementen voor een aanbieding. Deze abonnementen hebben verschillende functionaliteit en kunnen verschillend zijn geprijsd.
* Geef een aanbieding op een per site of een per gebruiker facturering model uit te voeren.
* Maandelijkse en jaarlijkse (vooraf betaald) bieden opties voor facturering.
* Geef persoonlijke prijzen voor een klant op basis van een in de onderhandelde zakelijke overeenkomst.


## <a name="next-steps"></a>Volgende stappen

Als u hebt nog niet gedaan, registreert u uw SaaS-toepassing in de [Azure-portal](https://ms.portal.azure.com) zoals uitgelegd in [registreren van een Azure AD-toepassing](./pc-saas-registration.md).  Daarna gebruikt u de meest recente versie van deze interface voor het ontwikkelen van: [SaaS vervulling API-versie 2](./pc-saas-fulfillment-api-v2.md).
