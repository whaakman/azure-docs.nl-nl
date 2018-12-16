---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d35da4f1eaed91411c015ed7665944d886f9d79c
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444082"
---
Volg onderstaande stappen als u in het Resource Manager-implementatiemodel een VNet wilt maken met behulp van Azure Portal. Gebruik de [voorbeeldwaarden](#values) als u deze stappen uitvoert als onderdeel van een zelfstudie. Als u deze stappen niet uitvoert als onderdeel van een zelfstudie, vervangt u de waarden door die van uzelf. Bekijk het [Virtual Network-overzicht](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over het gebruik van virtuele netwerken.

>[!NOTE]
>U dient eerst met uw on-premises netwerkbeheerder een IP-adresbereik te reserveren dat u specifiek voor dit virtuele netwerk kunt gebruiken, voordat dit VNet verbinding met een on-premises locatie kan maken. Als er een dubbel adresbereik bestaat aan beide zijden van de VPN-verbinding, wordt verkeer mogelijk niet zoals verwacht gerouteerd. Als u dit VNet met een ander VNet wilt verbinden, kan de adresruimte niet met het andere VNet overlappen. Plan daarom uw netwerkconfiguratie zorgvuldig.
>
>

1. Open een browser, ga naar [Azure Portal](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Klik op **Een resource maken**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.
3. Ga onder aan de pagina Virtual Network naar de lijst **Selecteer een implementatiemodel**, selecteer de optie **Resource Manager** in de lijst en klik vervolgens op **Maken**. Hiermee opent u de pagina Virtueel netwerk maken.

   ![Pagina Virtueel netwerk maken](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network.png "De pagina Virtueel netwerk maken")
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn.

   - **Naam**: Voer de naam voor het virtuele netwerk. In dit voorbeeld wordt de naam VNet1 gebruikt.
   - **Adresruimte**: Voer de adresruimte. Als er meerdere adresruimten moeten worden toegevoegd, voegt u de eerste adresruimte toe. U kunt later, nadat u het VNet hebt gemaakt, extra adresruimten toevoegen. Zorg ervoor dat de adresruimte die u opgeeft, niet overlapt met de adresruimte voor uw on-premises locatie.
   - **Abonnement**: Controleer of het weergegeven abonnement het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
   - **Resourcegroep**: Selecteer een bestaande resourcegroep of maak een nieuwe door een naam voor uw nieuwe resourcegroep te typen. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe groep aanmaakt. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
   - **Locatie**: Selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.
   - **Subnet**: De subnetnaam van het eerste en het adresbereik toevoegen. U kunt extra subnetten en het gatewaysubnet later toevoegen, na het maken van dit VNet. 

5. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**. Nadat u op **Maken** klikt, ziet u een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.
