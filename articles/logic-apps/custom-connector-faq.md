---
title: Aangepaste connector FAQ - Azure Logic Apps | Microsoft Docs
description: Veelgestelde vragen over de vereisten, triggers, enzovoort over het maken van aangepaste connectors
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 225e56de3985acae871ddec447b763e7de61cb80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="faq-custom-connectors"></a>Veelgestelde vragen over: Aangepaste connectors

## <a name="requirements"></a>Vereisten

**V:** kan ik een connector zonder de REST-API's maken? </br>
**A:** Nee, als u wilt een connector maken, u moet ondersteunen stabiele HTTP REST-API's voor uw service. 

**V:** welke hulpmiddelen kan ik gebruiken om een connector te maken? </br>
**A:** Azure heeft mogelijkheden en services die u gebruiken kunt voor de weergave van alle services die als een API, zoals Azure App Service voor het hosten van API Management en meer.

**V:** welke authenticatietypen worden ondersteund? </br>
**A:** kunt u deze normen ondersteunde verificatie:

* [OAuth 2.0](https://oauth.net/2/), waaronder [Azure Active Directory](https://azure.microsoft.com/develop/identity/) of bepaalde services, zoals Dropbox, GitHub en SalesForce
* Algemene OAuth 2.0
* [Basisverificatie](https://swagger.io/docs/specification/authentication/basic-authentication/)
* [API-sleutel](https://swagger.io/docs/specification/authentication/api-keys/)

## <a name="triggers"></a>Triggers

**V:** kan ik triggers zonder webhooks maken? </br>
**A:** Nee, aangepaste connectors voor Azure Logic Apps en Microsoft-Flow ondersteuning alleen webhook op basis van triggers. Als u aanvragen van andere patronen voor implementatie wilt, neem dan contact op met [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com) met meer informatie over uw API.

## <a name="certification"></a>Certificering

**Q**: ik wil niet een Microsoft-partner of Independent Software Vendors (ISV's). Kan ik connectors nog steeds maken? </br>
**Een**: Ja, u kunt deze connectors voor intern gebruik registreren in uw organisatie, maar als u wilt certificeren en openbaar release een connector, moet u de eigenaar de onderliggende service of aanwezig expliciete rechten voor het gebruik van de API.

## <a name="other"></a>Overige

**V:** mijn API's gebruiken een dynamische host. Hoe ik ze implementeren met OpenAPI? </br>
**A:** aangepaste connectors bieden geen ondersteuning voor dynamische hosts. Gebruik in plaats daarvan een statische host voor ontwikkel- en testdoeleinden. Als u uw connector certificeren wilt, vraagt u uw Microsoft-contactpersoon over dynamische implementatie.

**V:** ondersteunen Postman verzameling V2? </br>
**A:** Nee, alleen Postman verzameling V1 wordt momenteel ondersteund.

**V:** ondersteunen OpenAPI 3.0? </br>
**A:** Nee, alleen OpenAPI 2.0 wordt ondersteund.