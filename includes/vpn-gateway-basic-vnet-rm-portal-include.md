---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 835f23f98ebe56e0b19081f07dc3302ef93b27b9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109218"
---
U hebt door deze stappen te volgen een VNet gemaakt met behulp van het Resource Manager-implementatiemodel en de Azure-portal. Zie [Overzicht van virtuele netwerken](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over virtuele netwerken.

>[!NOTE]
>Overleg met uw on-premises netwerkbeheerder om een IP-adresbereik te reserveren dat u specifiek voor dit virtuele netwerk kunt gebruiken, voordat dit VNet verbinding met een on-premises locatie kan maken. Als er een dubbel adresbereik bestaat aan beide zijden van de VPN-verbinding, wordt verkeer niet correct gerouteerd. Als u dit VNet met een ander VNet wilt verbinden, kan de adresruimte niet met het andere VNet overlappen. Houd hier rekening mee als u uw netwerkconfiguratie gaan plannen.
>
>

1. Meld u aan bij de [Azure-portal](http://portal.azure.com) en selecteer **Create a resource**. De pagina **Nieuw** wordt geopend.

2. Voer in het veld **Search the marketplace** *virtueel netwerk* in en selecteer **Virtueel netwerk** in de geretourneerde lijst. De pagina **Virtueel netwerk** wordt geopend.

   ![Pagina voor zoeken van Virtual Network](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Pagina voor zoeken van Virtual Network")

3. Selecteer in de lijst **Select a deployment model** onder aan de pagina de optie **Resource Manager** en selecteer vervolgens **Maken**. De pagina **Virtueel netwerk maken** wordt geopend.

   ![Pagina Virtueel netwerk maken](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet.png "Pagina Virtueel netwerk maken")

4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die u in het veld invoert, zijn gevalideerd. Sommige waarden worden automatisch ingevuld. Deze kunt u door uw eigen waarden vervangen:

   - **Naam**: Voer de naam in van het virtuele netwerk.

   - **Adresruimte**: voer de adresruimte in. Als er meerdere adresruimten moeten worden toegevoegd, voert u de eerste adresruimte hier toe. U kunt later, nadat u het VNet hebt gemaakt, extra adresruimten toevoegen.

   - **Abonnement**: controleer of het weergegeven abonnement het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.

   - **Resourcegroep**: selecteer een bestaande resourcegroep of maak een nieuwe door een naam voor de nieuwe resourcegroep in te voeren. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden als u een nieuwe groep aanmaakt. Zie [Overzicht van Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie over resourcegroepen.

   - **Locatie**: selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, zich bevinden.

   - **Subnet**: voeg de **naam** en het **adresbereik** van het subnet toe. U kunt later, nadat u het VNet hebt gemaakt, extra subnetten toevoegen. 
     
5. Selecteer **Maken**.
