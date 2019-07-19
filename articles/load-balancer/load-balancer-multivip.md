---
title: Meerdere Vip's voor een Cloud service
titlesuffix: Azure Load Balancer
description: Overzicht van multiVIP en hoe u meerdere Vip's kunt instellen voor een Cloud service
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 3e97bea85d4d97b159168b21b4a6e932e655ccfb
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274693"
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Meerdere Vip's configureren voor een Cloud service

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

U kunt Azure Cloud Services via het open bare Internet openen met behulp van een IP-adres dat door Azure wordt verschaft. Dit open bare IP-adres wordt een VIP (Virtual IP) genoemd, omdat deze is gekoppeld aan de Azure-load balancer en niet op de virtuele machine (VM)-exemplaren in de Cloud service. U kunt elk VM-exemplaar in een Cloud service openen met behulp van één VIP.

Er zijn echter scenario's waarin u mogelijk meer dan één VIP nodig hebt als een ingangs punt voor dezelfde Cloud service. Uw Cloud service kan bijvoorbeeld meerdere websites hosten waarvoor SSL-connectiviteit is vereist met behulp van de standaard poort 443, omdat elke site wordt gehost voor een andere klant of Tenant. In dit scenario moet u voor elke website een ander openbaar gericht IP-adres hebben. In het onderstaande diagram ziet u een typische webhosting met meerdere tenants met een behoefte aan meerdere SSL-certificaten op dezelfde open bare poort.

![SSL-scenario voor meerdere VIP](./media/load-balancer-multivip/Figure1.png)

In het bovenstaande voor beeld gebruiken alle Vip's dezelfde open bare poort (443) en wordt het verkeer omgeleid naar een of meer virtuele machines met gelijke taak verdeling op een unieke particuliere poort voor het interne IP-adres van de Cloud service die als host fungeert voor alle websites.

> [!NOTE]
> Een andere situatie waarbij het gebruik van meerdere Vip's vereist is, is het hosten van meerdere SQL AlwaysOn-beschikbaarheids groep-listeners op dezelfde set Virtual Machines.

Vip's zijn standaard dynamisch, wat betekent dat het daad werkelijke IP-adres dat is toegewezen aan de Cloud service na verloop van tijd kan veranderen. Als u wilt voor komen dat dit gebeurt, kunt u een VIP voor uw service reserveren. Zie [gereserveerde open bare IP](../virtual-network/virtual-networks-reserved-public-ip.md)voor meer informatie over gereserveerde vip's.

> [!NOTE]
> Zie de [prijzen voor IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/) voor meer informatie over de prijzen op vip's en gereserveerde ip's.

U kunt Power shell gebruiken om te controleren welke Vip's door uw Cloud Services worden gebruikt, en Vip's toevoegen en verwijderen, een VIP aan een eind punt koppelen en taak verdeling configureren voor een specifieke VIP.

## <a name="limitations"></a>Beperkingen

Op dit moment is de functionaliteit voor meerdere VIP'S beperkt tot de volgende scenario's:

* **Alleen IaaS**. U kunt multi VIP alleen inschakelen voor Cloud Services die virtuele machines bevatten. U kunt multi VIP niet gebruiken in PaaS-scenario's met rolinstanties.
* **Alleen Power shell**. U kunt multi VIP alleen beheren met behulp van Power shell.

Deze beperkingen zijn tijdelijk en kunnen op elk gewenst moment worden gewijzigd. Ga naar deze pagina om toekomstige wijzigingen te verifiëren.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Een VIP toevoegen aan een Cloud service
Voer de volgende Power shell-opdracht uit om een VIP toe te voegen aan uw service:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Met deze opdracht wordt een resultaat weer gegeven dat vergelijkbaar is met het volgende voor beeld:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Een VIP verwijderen uit een Cloud service
Voer de volgende Power shell-opdracht uit om de VIP toe te voegen die is toegevoegd aan uw service in het bovenstaande voor beeld:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> U kunt een VIP alleen verwijderen als er geen eind punten aan zijn gekoppeld.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Hoe kan ik VIP-gegevens ophalen uit een Cloud service?
Voer het volgende Power shell-script uit om de Vip's op te halen die zijn gekoppeld aan een Cloud service:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

In het script wordt een resultaat weer gegeven dat vergelijkbaar is met het volgende voor beeld:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

In dit voor beeld heeft de Cloud service drie Vip's:

* **Vip1** is de standaard-VIP. u weet dat omdat de waarde voor IsDnsProgrammedName is ingesteld op True.
* **Vip2** en **Vip3** worden niet gebruikt omdat ze geen IP-adressen hebben. Ze worden alleen gebruikt als u een eind punt aan het VIP koppelt.

> [!NOTE]
> Voor extra Vip's worden er alleen kosten in rekening gebracht als deze zijn gekoppeld aan een eind punt. Zie [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/)voor meer informatie over prijzen.

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Een VIP koppelen aan een eind punt

Voer de volgende Power shell-opdracht uit om een VIP in een Cloud service te koppelen aan een eind punt:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

De opdracht maakt een eind punt dat is gekoppeld aan het VIP met de naam *Vip2* op poort *80*, en koppelt het aan de virtuele machine met de naam *myVM1* in een Cloud service met de naam *myService* met *TCP* op poort *8080*.

Voer de volgende Power shell-opdracht uit om de configuratie te controleren:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

De uitvoer ziet er ongeveer uit als in het volgende voor beeld:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Taak verdeling inschakelen voor een specifieke VIP

U kunt één VIP met meerdere virtuele machines koppelen voor taak verdeling. Stel dat u een Cloud service hebt met de naam *myService*en twee virtuele machines met de naam *myVM1* en *myVM2*. En uw Cloud service heeft meerdere Vip's, een ervan met de naam *Vip2*. Voer het volgende Power shell-script uit als u ervoor wilt zorgen dat alle verkeer naar poort *81* op *Vip2* wordt verdeeld over *myVM1* en *myVM2* op poort *8181*:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

U kunt uw load balancer ook bijwerken om een ander VIP te gebruiken. Als u bijvoorbeeld de Power shell-opdracht hieronder uitvoert, wijzigt u de set met taak verdeling voor het gebruik van een VIP met de naam Vip1:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Volgende stappen

[Azure Monitor logboeken voor Azure-taak verdeling](load-balancer-monitor-log.md)

[Overzicht van Internet gerichte load balancer](load-balancer-internet-overview.md)

[Aan de slag met de Internet gerichte load balancer](load-balancer-get-started-internet-arm-ps.md)

[Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

[Gereserveerd IP REST-Api's](https://msdn.microsoft.com/library/azure/dn722420.aspx)
