---
title: 'Maken en een ExpressRoute-circuit aanpassen: Azure portal | Microsoft Docs'
description: In dit artikel wordt beschreven hoe maken, richten, controleren, bijwerken, verwijderen en een ExpressRoute-circuit inrichting ervan ongedaan.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: a21fdfbc4396f2b7aff50fae4ca796d8ea6a733b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Maken en een ExpressRoute-circuit wijzigen
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
>

In dit artikel wordt beschreven hoe een Azure ExpressRoute-circuit maken met behulp van de Azure portal en het Azure Resource Manager-implementatiemodel. De volgende stappen ziet u ook het Controleer de status van het circuit, bijwerken of verwijderen en deze inrichting ervan ongedaan.


## <a name="before-you-begin"></a>Voordat u begint
* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* Zorg ervoor dat u toegang tot hebt de [Azure-portal](https://portal.azure.com).
* Zorg ervoor dat u machtigingen voor het maken van nieuwe netwerkresources. Neem contact op met uw accountbeheerder als u niet de juiste machtigingen hebt.
* U kunt [Bekijk een video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) voordat u begint om de stappen beter te begrijpen.

## <a name="create"></a>Maken en een ExpressRoute-circuit inrichten
### <a name="1-sign-in-to-the-azure-portal"></a>1. Aanmelden bij Azure Portal
Open een browser, ga naar [Azure Portal](http://portal.azure.com) en meld u aan met uw Azure-account.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Een nieuwe ExpressRoute-circuit maken
> [!IMPORTANT]
> Uw ExpressRoute-circuit wordt gefactureerd vanaf het moment dat de sleutel van een service wordt verleend. Zorg ervoor dat u deze bewerking niet uitvoeren wanneer de connectiviteitsprovider gereed is voor het inrichten van het circuit.
> 
> 

1. U kunt een ExpressRoute-circuit maken door de optie voor het maken van een nieuwe resource. Klik op **nieuw** > **Networking** > **ExpressRoute**, zoals wordt weergegeven in de volgende afbeelding:

  ![Een ExpressRoute-circuit maken](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Nadat u op **ExpressRoute**, ziet u de **maken ExpressRoute-circuit** pagina. Wanneer u de waarden op deze pagina hebt ingevuld, zorg ervoor dat u de juiste SKU-laag (Standard of Premium) en de gegevens softwarelicentiecontrole factureringsmodel (onbeperkt of Metered) opgeeft.

  ![De SKU-laag en gegevensmeters configureren](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit.png)

  * **Laag** bepaalt of een standaard ExpressRoute of een premium-invoegtoepassing voor ExpressRoute is ingeschakeld. U kunt opgeven **standaard** standaard SKU ophalen of **Premium** voor de premium-invoegtoepassing.
  * **Gegevensmeters** bepaalt het type facturering. U kunt opgeven **Metered** voor een plan datalimiet en **onbeperkt** voor een onbeperkte gegevens-plan. Houd er rekening mee dat kunt u het type van de facturering van **Metered** naar **onbeperkt**, maar niet het type van **onbeperkt** naar **Metered**.
  * **Peeringlocatie** is de fysieke locatie waar u peering met Microsoft.

    > [!IMPORTANT]
    > De Peeringlocatie geeft de [fysieke locatie](expressroute-locations.md) waar u peering met Microsoft. Dit is **niet** gekoppeld aan 'Locatie'-eigenschap die verwijst naar de Geografie waar de Resource-Provider van het Azure-netwerk bevindt. Het is raadzaam om te kiezen een Netwerkresourceprovider geografisch dicht bij de Peeringlocatie van het circuit terwijl ze niet zijn gerelateerd.
    >
    >

### <a name="3-view-the-circuits-and-properties"></a>3. De circuits en -eigenschappen weergeven
**Alle circuits weergeven**

U vindt de circuits die u hebt gemaakt door te selecteren **alle resources** in het menu aan de linkerkant.

![Weergave circuits](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**De eigenschappen weergeven**

U kunt de eigenschappen van het circuit weergeven door deze te selecteren. Op de **overzicht** pagina voor uw circuit de servicesleutel wordt weergegeven in het sleutelveld van de service. U moet de servicesleutel kopiëren voor uw circuit en geef dit door naar de serviceprovider om het inrichtingsproces te voltooien. De circuitservicesleutel is specifiek voor uw circuit.

![Eigenschappen weergeven](./media/expressroute-howto-circuit-portal-resource-manager/servicekey1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. De servicesleutel verzenden naar uw connectiviteitsprovider voor inrichting
Op deze pagina **providerstatus** bevat informatie over de huidige status van inrichting aan de kant van de serviceprovider. **Status circuit** biedt u de status aan de kant van Microsoft. Zie voor meer informatie over het circuit inrichtingstatuswaarden de [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.

Wanneer u een nieuwe ExpressRoute-circuit maakt, wordt het circuit in de volgende status is:

Providerstatus: niet ingericht<BR>
Circuit status: ingeschakeld

![Inrichting te initiëren](./media/expressroute-howto-circuit-portal-resource-manager/status.png)

Het circuit wordt gewijzigd in de volgende status wanneer de connectiviteitsprovider wordt deze voor u inschakelen:

Providerstatus: inrichting<BR>
Circuit status: ingeschakeld

Voordat u kunt een ExpressRoute-circuit gebruiken, moet deze de status van de volgende zijn:

Providerstatus: ingericht<BR>
Circuit status: ingeschakeld

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Controleer regelmatig de status en de status van de sleutel van het circuit
U kunt de eigenschappen van het circuit die u bent geïnteresseerd zijn in door deze te selecteren kunt weergeven. Controleer de **providerstatus** en zorg ervoor dat deze is verplaatst naar **ingericht** voordat u doorgaat.

![Status van circuit en -provider](./media/expressroute-howto-circuit-portal-resource-manager/provisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Maken van uw configuratie van de routering
Voor stapsgewijze instructies raadpleegt u de [ExpressRoute-circuit routeringsconfiguratie](expressroute-howto-routing-portal-resource-manager.md) artikel maken en wijzigen van circuit peerings.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services op laag 2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IP VPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u.
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. Een virtueel netwerk koppelen aan een ExpressRoute-circuit
Vervolgens moet u een virtueel netwerk koppelen aan uw ExpressRoute-circuit. Gebruik de [virtuele netwerken koppelen aan ExpressRoute-circuits](expressroute-howto-linkvnet-arm.md) artikel als u met het implementatiemodel van Resource Manager werkt.

## <a name="status"></a>Opvragen van de status van een ExpressRoute-circuit
U kunt de status van een circuit weergeven door te selecteren en de pagina overzicht weer te geven. 

## <a name="modify"></a>Wijzigen van een ExpressRoute-circuit
U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder enige impact op connectiviteit. U kunt het facturering model voor bandbreedte, SKU, wijzigen en klassieke bewerkingen op de **configuratie** pagina. Zie voor informatie over limieten en beperkingen, de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md). 

U kunt de volgende taken zonder uitvaltijd uitvoeren:

* In- of uitschakelen van een ExpressRoute Premium-invoegtoepassing voor ExpressRoute-circuit.
* De bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. De bandbreedte van een circuit downgraden wordt niet ondersteund. 
* Wijzigen van de softwarelicentiecontrole plan van *Datalimiet gegevens* naar *onbeperkt*. Het plan softwarelicentiecontrole wijzigen van onbeperkt naar Datalimiet gegevens wordt niet ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

> [!IMPORTANT]
> U moet de ExpressRoute-circuit opnieuw maken als er onvoldoende capaciteit op de bestaande poort. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Downgraden bandbreedte, moet u het ExpressRoute-circuit inrichting ervan ongedaan en vervolgens opnieuw inrichten van nieuwe ExpressRoute-circuit.
> 
> De Premium-invoegtoepassing voor bewerking uit te schakelen kan mislukken als u resources die groter zijn dan wat voor het standaard circuit is toegestaan.
> 
> 

Als u wilt een ExpressRoute-circuit wijzigen, klikt u op **configuratie**.

![Circuit wijzigen](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)




## <a name="delete"></a>Opheffen van inrichting en een ExpressRoute-circuit verwijderen
U kunt uw ExpressRoute-circuit verwijderen door het selecteren van de **verwijderen** pictogram. Houd rekening met de volgende informatie:

* U moet alle virtuele netwerken van het ExpressRoute-circuit ontkoppelen. Als deze bewerking is mislukt, controleert u of er geen virtuele netwerken zijn gekoppeld aan het circuit.
* Als de ExpressRoute-circuit serviceprovider Inrichtingsstatus **inrichten** of **ingericht** moet u werken met uw serviceprovider voor inrichting ervan ongedaan maakt het circuit op hun kant. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid opheffen van inrichting het circuit en waarschuwt ons.
* Als de provider heeft het circuit deprovisioned (de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**), kunt u het circuit verwijderen. Hierdoor wordt voorkomen dat facturering voor het circuit.

## <a name="next-steps"></a>Volgende stappen
Nadat u het circuit hebt gemaakt, kunt u doorgaan met de volgende stappen uitvoeren:

* [Maken en aanpassen van routering voor ExpressRoute-circuit](expressroute-howto-routing-portal-resource-manager.md)
* [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)