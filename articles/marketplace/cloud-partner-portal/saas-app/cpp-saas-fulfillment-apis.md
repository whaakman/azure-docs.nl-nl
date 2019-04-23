---
title: SaaS vervulling API's - Azure Marketplace | Microsoft Docs
description: De versies van de API's waarmee u uw SaaS integreren met de Azure Marketplace biedt vervulling introduceert.
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
ms.date: 03/26/2019
ms.author: pbutlerm
ms.openlocfilehash: c7da46984d592abc6ed97d7490fde732bf26b0ba
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798725"
---
# <a name="saas-fulfillment-apis"></a>SaaS-uitvoerings-API's

De uitvoering van SaaS API's inschakelen independent software vendors (ISV) hun SaaS-toepassingen integreren met de Azure Marketplace. Deze API's kunnen ISV-toepassingen om deel te nemen in alle commerce ingeschakeld kanalen: direct, door partner geleide (reseller) en onder leiding van een veld.  Ze zijn vereist voor het aanbieden van transactable SaaS-aanbiedingen op Azure Marketplace.

> [!WARNING]
> De huidige versie van deze API is versie 2, die moet worden gebruikt voor alle nieuwe SaaS biedt.  Versie 1 van de API is afgeschaft en wordt onderhouden voor de ondersteuning van bestaande aanbiedingen.


## <a name="business-model-support"></a>Ondersteuning voor bedrijven-model

Deze API biedt ondersteuning voor de volgende zakelijke model mogelijkheden; U kunt:

* Geef meerdere abonnementen voor een aanbieding. Deze abonnementen hebben verschillende functionaliteit en kunnen verschillend zijn geprijsd.
* Geef een aanbieding op een per site of een per gebruiker facturering model uit te voeren.
* Maandelijkse en jaarlijkse (vooraf betaald) bieden opties voor facturering.
* Geef persoonlijke prijzen voor een klant op basis van een in de onderhandelde zakelijke overeenkomst.


## <a name="next-steps"></a>Volgende stappen

Als u hebt nog niet gedaan, registreert u uw SaaS-toepassing in de [Azure-portal](https://ms.portal.azure.com) zoals uitgelegd in [registreren van een Azure AD-toepassing](./cpp-saas-registration.md).  Daarna gebruikt u de meest recente versie van deze interface voor het ontwikkelen van: [SaaS vervulling API-versie 2](./cpp-saas-fulfillment-api-v2.md).
