---
title: Mutiple VIP's voor een cloudservice
description: Overzicht van multiVIP en het instellen van meerdere VIP's op een cloudservice
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b6b7b0b2d7a7f33facaf72bbd2d7937364770673
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Meerdere VIP's voor een cloudservice configureren

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

U kunt toegang tot Azure-cloudservices via het openbare Internet met behulp van een IP-adres opgegeven door Azure. Dit openbare IP-adres wordt aangeduid als een VIP-adres (virtuele IP) omdat deze is gekoppeld aan de Azure load balancer en geen virtuele Machine (VM) in de cloudservice-exemplaren. Met behulp van een enkele VIP kunt u een VM-instantie binnen een cloudservice openen.

Er zijn echter scenario's waarin u wellicht meer dan een VIP als een vermelding in het verwijzen naar dezelfde cloudservice. Uw cloudservice kan bijvoorbeeld meerdere websites die moeten SSL-verbindingen via de standaardpoort 443, zoals elke site wordt gehost voor een andere klant of tenant te hosten. In dit scenario moet u een andere openbare internetgerichte IP-adres voor elke website. Het onderstaande diagram ziet u een multitenant Standaardwebthema die als host fungeert voor meerdere SSL-certificaten op dezelfde openbare poort.

![Meerdere VIP SSL-scenario](./media/load-balancer-multivip/Figure1.png)

Alle VIP's gebruiken dezelfde openbare poort (443) in het bovenstaande voorbeeld en verkeer wordt omgeleid naar een of meer virtuele machines op een unieke particuliere poort voor het interne IP-adres van de cloudservice die als host fungeert voor alle websites gelijke taakverdeling.

> [!NOTE]
> Een andere situatie die het gebruik van de meerdere VIP's als host fungeert voor meerdere SQL AlwaysOn availability beschikbaarheidsgroeplisteners op dezelfde set van virtuele Machines.

VIP's zijn standaard dynamisch, wat betekent dat de IP-adres toegewezen aan de cloudservice na verloop van tijd kan worden gewijzigd. Om dat te voorkomen, kunt u een VIP-adres reserveren voor uw service. Zie voor meer informatie over gereserveerde VIP's, [gereserveerde openbare IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Zie [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/) voor informatie over de prijzen van VIP's en gereserveerde IP-adressen.

U kunt PowerShell gebruiken om te controleren of de VIP's die worden gebruikt door uw cloudservices, evenals toevoegen en verwijderen van VIP's, een VIP-adres naar een eindpunt koppelen en taakverdeling op een specifieke VIP-adres configureren.

## <a name="limitations"></a>Beperkingen

Op dit moment is meerdere VIP-functionaliteit beperkt tot de volgende scenario's:

* **Alleen IaaS**. U kunt meerdere VIP alleen inschakelen voor cloudservices met virtuele machines. U kunt meerdere VIP niet gebruiken voor PaaS-scenario's met rolinstanties.
* **Alleen PowerShell**. U kunt meerdere VIP alleen beheren met behulp van PowerShell.

Deze beperkingen zijn tijdelijk en kunnen op elk gewenst moment worden gewijzigd. Zorg ervoor dat deze pagina om te controleren of toekomstige wijzigingen aangepast.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Een VIP-adres toevoegen aan een cloudservice
Om een VIP-adres toevoegen aan uw service, voer de volgende PowerShell-opdracht:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Deze opdracht geeft u een resultaat vergelijkbaar met het volgende voorbeeld:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Een VIP-adres van een service in de cloud verwijderen
Voer de volgende PowerShell-opdracht voor het verwijderen van de VIP toegevoegd aan uw service in het bovenstaande voorbeeld:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> U kunt alleen een VIP-adres verwijderen als er geen eindpunten zijn gekoppeld.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Hoe VIP-gegevens ophalen uit een Cloud-Service
Voer de volgende PowerShell-script voor het ophalen van de VIP's die zijn gekoppeld aan een cloudservice:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Het script geeft een resultaat vergelijkbaar met het volgende voorbeeld:

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

In dit voorbeeld heeft de cloudservice 3 VIP's:

* **Vip1** is de standaard-VIP u weet dat omdat de waarde voor IsDnsProgrammedName is ingesteld op true.
* **Vip2** en **Vip3** niet als er geen IP-adressen worden gebruikt. Deze wordt alleen gebruikt als u een eindpunt naar het VIP koppelt.

> [!NOTE]
> Uw abonnement wordt alleen in rekening gebracht voor extra VIP's zodra ze gekoppeld aan een eindpunt zijn. Zie voor meer informatie over prijzen [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Het koppelen van een VIP-adres naar een eindpunt

Om te koppelen van een VIP-adres op een cloudservice om een eindpunt, voer de volgende PowerShell-opdracht:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

De opdracht maakt u een eindpunt dat is gekoppeld aan de VIP aangeroepen *Vip2* op poort *80*, gekoppeld aan de virtuele machine met de naam *myVM1* in een cloudservice met de naam *MijnService* met *TCP* op poort *8080*.

Om te controleren of de configuratie, moet u de volgende PowerShell-opdracht uitvoeren:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

De uitvoer lijkt op het volgende voorbeeld:

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

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Het inschakelen van taakverdeling op een specifieke VIP-adres

U kunt een enkel VIP koppelen aan meerdere virtuele machines voor load balancing-doeleinden. Bijvoorbeeld: u hebt een cloudservice met de naam *MijnService*, en twee virtuele machines met de naam *myVM1* en *myVM2*. En uw cloudservice heeft meerdere VIP's, een van deze met de naam *Vip2*. Als u zorgen wilt dat alle verkeer op poort *81* op *Vip2* wordt verdeeld tussen *myVM1* en *myVM2* op poort *8181* , voer het volgende PowerShell-script:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

U kunt ook de load balancer voor het gebruik van een andere VIP bijwerken. Als u de onderstaande PowerShell-opdracht uitvoert, wordt u bijvoorbeeld is ingesteld op het gebruik van een VIP Vip1 met de naam van de taakverdeling wijzigen:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Volgende stappen

[Taakverdeling van de Azure-logboekanalyse](load-balancer-monitor-log.md)

[Internet gerichte load balancer-overzicht](load-balancer-internet-overview.md)

[Aan de slag op Internet gerichte load balancer](load-balancer-get-started-internet-arm-ps.md)

[Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

[Gereserveerd IP-adres REST-API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)
