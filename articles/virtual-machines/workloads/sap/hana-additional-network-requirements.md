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
ms.openlocfilehash: 2f4c74076f2763addc1f2b971d8ea0ca29ce6fd0
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392995"
---
# <a name="additional-network-requirements-for-large-instances"></a>Aanvullende vereisten voor grote instanties

Mogelijk hebt u aanvullende vereisten als onderdeel van een implementatie van grote instanties van SAP HANA op Azure.

## <a name="add-more-ip-addresses-or-subnets"></a>Meer IP-adressen of subnetten toevoegen

De Azure-portal, PowerShell of Azure CLI gebruiken wanneer u meer IP-adressen of subnetten toevoegen.

Het nieuwe IP-adresbereik toevoegen als een nieuw bereik aan de adresruimte van virtueel netwerk, in plaats van een nieuwe, samengevoegde bereik genereren. Deze wijziging naar Microsoft verzenden. Hiermee kunt u verbinding maken tussen die nieuwe IP-adresbereik en de HANA grote instantie eenheden in uw client. Een Azure-ondersteuning-aanvraag voor het ophalen van de adresruimte nieuwe virtuele netwerk is toegevoegd, kunt u openen. Nadat u de bevestiging hebt ontvangen, moet u de volgende stappen uitvoeren.

Zie voor informatie over het maken van een ander subnet van de Azure-portal [een virtueel netwerk maken met de Azure-portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). Als u wilt maken vanuit PowerShell, Zie [maken van een virtueel netwerk met behulp van PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="add-virtual-networks"></a>Virtuele netwerken toevoegen

Na de verbinding in eerste instantie met een of meer virtuele netwerken van Azure, is het raadzaam om nieuwe zijn in die toegang hebben tot SAP HANA op Azure (grote instanties) verbinding te maken. Eerst een Azure-ondersteuningsaanvraag indienen. In deze aanvraag zijn de specifieke gegevens voor de identificatie van de specifieke Azure-implementatie. Ook de IP-adresbereik ruimte of bereiken van de adresruimte van Azure-netwerk. SAP HANA op Microsoft-Service Management geeft vervolgens de benodigde informatie die u wilt verbinding maken met de aanvullende virtuele netwerken en Azure ExpressRoute. Voor elk virtueel netwerk moet u een unieke autorisatiesleutel verbinding maken met de ExpressRoute-circuit tot HANA grote instanties.

## <a name="increase-expressroute-circuit-bandwidth"></a>Bandbreedte van ExpressRoute-circuit vergroten

Neem contact op met SAP HANA op Microsoft-Service Management. Als ze u raden aan de bandbreedte van de SAP HANA op Azure (grote instanties) ExpressRoute-circuit vergroten, maakt u een ondersteuningsaanvraag indienen voor Azure. (U kunt een verhoging voor een enkele bandbreedte tot maximaal 10 Gbps.) U ontvangt vervolgens een melding nadat de bewerking voltooid is. u hoeft te doen niets om in te schakelen deze hogere snelheid in Azure.

## <a name="add-an-additional-expressroute-circuit"></a>Voeg een extra ExpressRoute-circuit

Neem contact op met SAP HANA op Microsoft-Service Management. Als ze u een extra ExpressRoute-circuit toevoegen adviseren, maakt u een ondersteuningsaanvraag indienen voor Azure (met inbegrip van een aanvraag voor het ophalen van de autorisatie-informatie voor verbinding met het nieuwe circuit). Voordat u de aanvraag, definieert u de adresruimte die wordt gebruikt op de virtuele netwerken. SAP HANA op Microsoft-Service Management kunnen vervolgens autorisatie bieden.

Wanneer het nieuwe circuit is gemaakt en de SAP HANA op Microsoft-Service Management-configuratie voltooid is, ontvangt u een melding met de informatie die u nodig hebt om door te gaan. U bent niet kunnen verbinding maken met virtuele netwerken van Azure dit extra circuit als ze al zijn verbonden met een andere SAP HANA op Azure (grote instantie) ExpressRoute-circuit in dezelfde Azure-regio.

## <a name="delete-a-subnet"></a>Een subnet verwijderen

Als u wilt verwijderen van een subnet van een virtueel netwerk, kunt u de Azure-portal, PowerShell of Azure CLI. Als uw Azure-netwerk IP-adresbereik of adres adresruimte een samengevoegde adresbereik is, is er geen volgen voor u met Microsoft. (Merk echter op het virtuele netwerk is nog steeds de doorgegeven de adresruimte van de BGP-route met het subnet verwijderd.) Mogelijk hebt u de Azure-netwerk-bereik of het adres adresruimte gedefinieerd als meerdere IP-adressen, waarvan een is toegewezen aan uw subnet verwijderd. Zorg dat u die verwijderen uit de adresruimte van uw virtuele netwerk. Vervolgens informeert SAP HANA op Microsoft-Service Management van de bereiken die SAP HANA op Azure (grote instanties) is toegestaan om te communiceren met verwijderen.

Zie voor meer informatie, [verwijderen van een subnet](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet).

## <a name="delete-a-virtual-network"></a>Een virtueel netwerk verwijderen

Zie voor meer informatie, [verwijderen van een virtueel netwerk](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network).

SAP HANA op Microsoft-Management-Service verwijdert de bestaande machtigingen op de SAP HANA op Azure (grote instanties) ExpressRoute-circuit. U verwijdert ook de Azure-netwerk IP-adresbereik of adres adresruimte voor de communicatie met HANA grote instanties.

Nadat u het virtuele netwerk verwijdert, opent u een Azure-ondersteuningsaanvraag voor het IP-adresbereik ruimte of de adresbereiken op die moeten worden verwijderd.

Om te controleren of dat u Alles verwijderen, verwijdert u de ExpressRoute-verbinding, gateway van het virtuele netwerk, het virtuele netwerk gateway openbare IP-adres en het virtuele netwerk.

## <a name="delete-an-expressroute-circuit"></a>Een ExpressRoute-circuit verwijderen

Als u wilt verwijderen een extra SAP HANA op Azure (grote instanties) ExpressRoute-circuit, opent u een Azure-ondersteuningsaanvraag met SAP HANA op Microsoft-Service Management. De aanvraag is dat het circuit worden verwijderd. U kunt binnen de Azure-abonnement, verwijderen of behouden van het virtuele netwerk, indien nodig. Echter, moet u de verbinding tussen de HANA grote instanties ExpressRoute-circuit en de gekoppelde virtuele netwerkgateway verwijderen.

## <a name="next-steps"></a>Volgende stappen

- [SAP HANA (Large Instances) installeren en configureren in Azure](hana-installation.md)
