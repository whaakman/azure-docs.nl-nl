---
title: Aanvullende vereisten voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Aanvullende vereisten voor SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ee89553846309bd96f3cad3648e337488cf07d3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164708"
---
# <a name="additional-network-requirements"></a>Aanvullende vereisten

U mogelijk soms extra vereisten als onderdeel van de implementatie van HANA grote instanties. In dit artikel ziet u de volgende vereisten:
- [Meer IP-adressen of subnet toe te voegen ](#adding-more-ip-addresses-or-subnets)
- [Virtuele vetworks toevoegen](#adding-vnets)
- [Vergroot de bandbreedte van het express route-circuit](#increasing-expressroute-circuit-bandwidth)
- [Een extra express route-circuit toevoegen](#adding-an-additional-expressroute-circuit)
- [Een subnet verwijderen](#deleting-a-subnet)
- [Een virtueel netwerk verwijderen](#deleting-a-vnet)
- [Een express-router-circuit verwijderen](#deleting-an-expressroute-circuit)


## <a name="adding-more-ip-addresses-or-subnets"></a>Meer IP-adressen of subnetten toe te voegen

Gebruik de Azure portal, PowerShell of CLI bij het toevoegen van meer IP-adressen of subnetten.

In dit geval wordt de aanbeveling is het nieuwe IP-adresbereik toevoegen als nieuwe bereik aan de VNet-adresruimte in plaats van een nieuwe, samengevoegde bereik genereren. In beide gevallen moet u deze wijziging naar Microsoft om de connectiviteit buiten die nieuwe IP-adresbereik aan de HANA grote instantie-eenheden in uw client toestaan indienen. U kunt een aanvraag Azure-ondersteuning voor het ophalen van de nieuwe VNet-adresruimte van toegevoegd openen. Nadat u de bevestiging hebt ontvangen, moet u de volgende stappen uitvoeren.

Voor het maken van een ander subnet van de Azure-portal, Zie het artikel [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network), en als u wilt maken vanuit PowerShell, Zie [maken van een virtueel netwerk met behulp van PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Vnet's toe te voegen

Na het in eerste instantie een of meer Azure VNets verbinden, is het raadzaam om toe te voegen nieuwe zijn in die toegang hebben tot SAP HANA op Azure (grote instanties). Eerst een Azure-ondersteuningsaanvraag indienen, in die aanvraag bevat zowel de specifieke gegevens identificeren van de specifieke Azure-implementatie en het IP-adresbereiken ruimte van de Azure-VNet-adresruimte. SAP HANA op Azure Service Management geeft vervolgens de benodigde informatie die u wilt verbinding maken met de extra VNets en ExpressRoute. Voor elke VNet moet u een unieke sleutel voor autorisatie verbinding maken met de ExpressRoute-Circuit tot HANA grote instanties.

Stappen voor het toevoegen van een nieuw Azure-VNet:

1. Voltooid de eerste stap bij het onboarding-proces, Zie de _het maken van Azure VNet_ sectie.
2. Voltooid de tweede stap in het onboarding-proces, Zie de _gatewaysubnet maakt_ sectie.
3. Als u wilt de aanvullende VNets verbinden met het HANA grote instantie ExpressRoute-circuit, opent u een Azure-ondersteuningsaanvraag met informatie over de nieuwe VNet en een nieuwe autorisatie-sleutel aanvragen.
4. Wanneer een melding dat de autorisatie is voltooid, gebruik de Microsoft geleverde autorisatie-informatie voor het voltooien van de derde stap in het onboarding-proces, Zie de _VNets koppelen_ sectie.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Steeds meer bandbreedte van ExpressRoute-circuit

Neem contact op met SAP HANA op Azure Service Management. Als u wordt aangeraden de bandbreedte van de SAP HANA op Azure (grote instanties) ExpressRoute-circuit vergroten, maakt u een ondersteuningsaanvraag indienen voor Azure. (U kunt een verhoging voor een enkele bandbreedte tot maximaal 10 Gbps.) U ontvangt vervolgens een melding nadat de bewerking voltooid is. Er is geen verdere actie nodig om in te schakelen deze hogere snelheid in Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Toevoegen van een extra ExpressRoute-circuit

Neem contact op met SAP HANA op Azure Service Management, als u wordt aangeraden dat een extra ExpressRoute-circuit is vereist, maken van een Azure ondersteuningsaanvraag te maken van een nieuwe ExpressRoute-circuit (en verbinding maken met het autorisatie-informatie ophalen). De adresruimte die gaat worden gebruikt voor de VNets moeten worden gedefinieerd voordat u de aanvraag, in volgorde voor SAP HANA op Azure-servicebeheer voor autorisatie.

Zodra het nieuwe circuit is gemaakt en de SAP HANA op Azure Service Management-configuratie voltooid is, gaat u voor het ontvangen van meldingen met de informatie die u nodig hebt om door te gaan. Volg de stappen voor het maken en de nieuwe VNet verbinden met dit extra circuit hierboven. U bent niet kunnen Azure VNets verbinden met deze aanvullende circuit als ze al met een andere SAP HANA op Azure (grote instantie) ExpressRoute-circuit in dezelfde Azure-regio verbonden.

## <a name="deleting-a-subnet"></a>Een subnet verwijderen

Als u wilt verwijderen van een VNet-subnet, kan de Azure portal, PowerShell of CLI worden gebruikt. Als uw Azure-VNet-IP-adres bereik/Azure VNet-adresruimte een samengevoegde adresbereik is, is er geen volgen voor u met Microsoft. Behalve dat het VNet is nog steeds de adresruimte van het BGP-route met het verwijderde subnet wordt doorgegeven. Als u de Azure-VNet-IP-adres bereik/Azure VNet-adresruimte gedefinieerd als meerdere IP-adresbereiken waarvan een is toegewezen aan uw subnet verwijderd, moet u die buiten de VNet-adresruimte te verwijderen en vervolgens informeren SAP HANA op Azure Service Management deze verwijderen uit het bereik dat is toegestaan om te communiceren met SAP HANA op Azure (grote instanties).

Als u wilt verwijderen van een subnet, Zie [verwijderen van een subnet](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) voor meer informatie over het maken van subnets.

## <a name="deleting-a-vnet"></a>Verwijderen van een VNet

Als u wilt verwijderen van een virtueel netwerk, Zie [verwijderen van een virtueel netwerk](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). SAP HANA op Azure Service Management verwijdert de bestaande machtigingen op de SAP HANA op Azure (grote instanties) ExpressRoute-circuit en verwijderen van de Azure-VNet-IP-adres bereik/Azure VNet-adresruimte voor de communicatie met HANA grote instanties.

Wanneer het VNet is verwijderd, opent u een Azure-ondersteuningsaanvraag voor de IP-adresbereiken ruimte moet worden verwijderd.

Om te controleren of dat alles is verwijderd, verwijdert u de volgende items:

- De ExpressRoute-verbinding, VNet-Gateway, openbare IP-VNet-Gateway en VNet

## <a name="deleting-an-expressroute-circuit"></a>Een ExpressRoute-circuit verwijderen

Als u wilt verwijderen een extra SAP HANA op Azure (grote instanties) ExpressRoute-circuit, opent u een Azure-ondersteuningsaanvraag met SAP HANA op Azure Service Management en aanvragen dat het circuit moet worden verwijderd. U kunt binnen de Azure-abonnement, verwijderen of behouden het VNet indien nodig. Echter, moet u de verbinding tussen het HANA grote instanties ExpressRoute-circuit en de gekoppelde VNet-gateway verwijderen.

Als u ook wilt verwijderen van een VNet, volgt u de instructies over het verwijderen van een VNet in de sectie hierboven.

**Volgende stappen**

- Raadpleeg [installeren en configureren van SAP HANA (grote instanties) op Azure](hana-installation.md).
