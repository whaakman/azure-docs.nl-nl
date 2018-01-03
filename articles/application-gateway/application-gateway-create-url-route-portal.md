---
title: Maak een regel op basis van het pad voor een toepassingsgateway - Azure-portal | Microsoft Docs
description: Informatie over het maken van een regel op basis van het pad voor een toepassingsgateway met behulp van de Azure-portal.
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: b207e7e7bd83e56db68288190c7bedafa8b5b7fa
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-azure-portal"></a>Een regel op basis van het pad voor een toepassingsgateway maken met behulp van de Azure-portal

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Met URL-pad gebaseerde routering koppelt u de routes op basis van het URL-pad van de HTTP-aanvragen. Er wordt gecontroleerd of er een route naar een back-endserverpool geconfigureerd voor de URL in de toepassingsgateway is en vervolgens het netwerkverkeer worden verzonden naar de gedefinieerde groep. Vaak gebruikt voor het URL-pad gebaseerde routering is van aanvragen verdelen over voor andere typen inhoud aan andere back-endserver groepen.

Toepassingsgateways zijn twee typen: basic en regels voor URL-pad is gebaseerd. Het basic regeltype biedt round robin-service voor de back-end-adresgroepen. Het patroon van het pad van de aanvraag-URL-pad gebaseerde regels naast round-robin distributie ook gebruiken bij het kiezen van de juiste back-end-pool.

## <a name="scenario"></a>Scenario

Het volgende scenario maakt een regel op basis van het pad in een bestaande application gateway.
Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [een toepassingsgateway maken met de portal](application-gateway-create-gateway-portal.md).

![URL-route][scenario]

## <a name="createrule"></a>De regel op basis van een pad maken

Een regel op basis van het pad moet een eigen listener. Voordat u de regel maakt, moet u controleren of er een listener beschikbaar om te gebruiken.

### <a name="step-1"></a>Stap 1

Ga naar de [Azure-portal](http://portal.azure.com) en selecteer een bestaande toepassing. Klik op **regels**.

![Overzicht van Application Gateway][1]

### <a name="step-2"></a>Stap 2

Klik op de **op basis van het pad** knop een nieuwe regel op basis van het pad toevoegen.

### <a name="step-3"></a>Stap 3

De **toevoegen op basis van een pad regel** blade bevat twee secties. De eerste sectie is waar u de listener, de naam van de regel en de standaardinstellingen van het pad gedefinieerd. De standaardinstellingen van het pad zijn voor routes die niet onder de aangepaste route op basis van het pad vallen. Het tweede gedeelte van de **toevoegen op basis van een pad regel** blade is waar u de regels op basis van een pad zelf definiëren.

**Basisinstellingen**

* **Naam**: een beschrijvende naam voor de regel die toegankelijk is in de portal.
* **Listener**: de listener die wordt gebruikt voor de regel.
* **Standaard back-endpool**: moet worden gebruikt voor de standaardregel voor de back-end.
* **Standaardinstellingen voor HTTP-**: de HTTP-instellingen voor de standaardregel moet worden gebruikt.

**Instellingen voor een regel op basis van het pad**

* **Naam**: een beschrijvende naam voor de regel op basis van het pad.
* **Paden**: het pad naar de regel eruitziet voor doorsturen van verkeer.
* **Back-endpool**: de back-end voor de regel moet worden gebruikt.
* **HTTP-instelling**: de HTTP-instellingen voor de regel moet worden gebruikt.

> [!IMPORTANT]
> De **paden** instelling is de lijst met patronen pad moet worden gezocht. Elk patroon moet beginnen met een slash en een sterretje is alleen toegestaan aan het einde. Voorbeelden van geldige: / xyz, / XYZ*, en /xyz/*.  

![Een regel op basis van een pad blade met informatie is ingevuld toevoegen][2]

Een regel op basis van het pad toevoegen aan een bestaande application gateway is een eenvoudig proces via de Azure portal. Nadat u een regel op basis van een pad hebt gemaakt, kunt u deze zodanig dat extra regels kunt bewerken. 

![Extra regels op basis van het pad toevoegen][3]

Deze stap configureert u een route op basis van het pad. Het is belangrijk te weten dat aanvragen niet opnieuw worden geschreven. Wanneer aanvragen worden geleverd in, de toepassingsgateway inspecteert de aanvraag en, op basis van het URL-patroon, stuurt de aanvraag naar de juiste back-end-pool.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van SSL-offloading met Azure Application Gateway, [een toepassingsgateway voor SSL-offload configureren met behulp van de Azure-portal](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
