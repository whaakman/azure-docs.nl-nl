---
title: Meerdere VIP's voor een cloudservice
description: Overzicht van multiVIP en over het instellen van meerdere VIP's op een cloudservice
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 03ecb44f3d561ff720939d0605d342b299194ef2
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218794"
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Meerdere VIP's voor een cloudservice configureren

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

U kunt toegang tot Azure-cloud-services via het openbare Internet met behulp van een IP-adres dat is verstrekt door Azure. Dit openbare IP-adres wordt een VIP-adres (virtuele IP) genoemd omdat deze is gekoppeld aan de Azure load balancer en niet de virtuele-Machine (VM) in de cloudservice-exemplaren. U kunt toegang tot een VM-exemplaar binnen een cloudservice met behulp van een enkele VIP.

Er zijn echter scenario's waarin u mogelijk meer dan één VIP omdat er geen vermelding verwijzen naar dezelfde cloudservice. Uw cloudservice kan bijvoorbeeld hosten van meerdere websites waarvoor SSL-connectiviteit met behulp van de standaardpoort 443 is vereist als elke site wordt gehost voor een andere klant of tenant. In dit scenario moet u een andere openbare internetgerichte IP-adres voor elke website. Het volgende diagram ziet u een typische multitenant-website die als host fungeert voor meerdere SSL-certificaten op de dezelfde openbare poort.

![Meerdere VIP SSL-scenario](./media/load-balancer-multivip/Figure1.png)

Alle VIP's gebruiken dezelfde openbare poort (443) en verkeer wordt omgeleid naar een of meer load balance VM's op een unieke particuliere poort voor het interne IP-adres van de cloudservice die als host fungeert voor alle websites in het bovenstaande voorbeeld.

> [!NOTE]
> Een andere situatie vereisen het gebruik van de meerdere VIP's als host fungeert voor meerdere listeners voor SQL AlwaysOn beschikbaarheid op dezelfde set van virtuele Machines.

VIP's zijn standaard dynamisch, wat betekent dat het IP-adres toegewezen aan de cloudservice na verloop van tijd kan worden gewijzigd. Om te voorkomen dat gebeurt, kunt u een VIP-adres reserveren voor uw service. Zie voor meer informatie over gereserveerde VIP's, [gereserveerde openbare IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Raadpleeg [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/) voor informatie over prijzen voor VIP's en gereserveerde IP-adressen.

U kunt PowerShell gebruiken om te controleren of de VIP's die worden gebruikt door uw cloud-services, evenals toevoegen en verwijderen van VIP's, een VIP-adres naar een eindpunt koppelen en configureren van taakverdeling op een specifieke VIP-adres.

## <a name="limitations"></a>Beperkingen

Op dit moment is de Multi-VIP-functionaliteit beperkt tot de volgende scenario's:

* **Alleen IaaS**. U kunt alleen Multi-VIP inschakelen voor cloud-services die virtuele machines bevatten. U kunt in PaaS-scenario's met instanties van de Multi-VIP gebruiken.
* **Alleen PowerShell**. U kunt alleen Multi-VIP beheren met behulp van PowerShell.

Deze beperkingen zijn tijdelijk en kunnen op elk gewenst moment worden gewijzigd. Zorg ervoor dat u terug naar deze pagina om te controleren of toekomstige wijzigingen.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Een VIP-adres toevoegen aan een cloudservice
Een VIP-adres toevoegen aan uw service, moet u de volgende PowerShell-opdracht uitvoeren:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Deze opdracht wordt een resultaat vergelijkbaar met het volgende voorbeeld weergegeven:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Het verwijderen van een VIP-adres van een service in de cloud
Als u wilt verwijderen van het VIP-adres toevoegen aan uw service in het bovenstaande voorbeeld, voer de volgende PowerShell-opdracht:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> U kunt alleen een VIP-adres verwijderen als er geen eindpunten zijn gekoppeld.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Hoe VIP-gegevens ophalen uit een Service in de Cloud
Om op te halen van de VIP's die zijn gekoppeld aan een cloudservice, voer de volgende PowerShell-script:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Het script wordt een resultaat vergelijkbaar met het volgende voorbeeld weergegeven:

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

* **Vip1** is de standaard-VIP, u weet dat omdat de waarde voor IsDnsProgrammedName is ingesteld op true.
* **Vip2** en **Vip3** niet als ze niet alle IP-adressen hebben worden gebruikt. Ze zullen alleen worden gebruikt als u een eindpunt naar het VIP koppelt.

> [!NOTE]
> Uw abonnement wordt alleen in rekening gebracht voor extra VIP's zodra ze gekoppeld aan een eindpunt zijn. Zie voor meer informatie over prijzen [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Een VIP-adres naar een eindpunt koppelen

Om te koppelen van een VIP-adres op een service in de cloud naar een eindpunt, voer de volgende PowerShell-opdracht:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

De opdracht maakt u een eindpunt dat is gekoppeld aan het VIP-adres met de naam *Vip2* op poort *80*, en koppelt dat aan de virtuele machine met de naam *myVM1* in een cloudservice met de naam *MijnService* met behulp van *TCP* op poort *8080*.

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

U kunt een enkel VIP-adres koppelen aan meerdere virtuele machines voor taakverdeling van toepassing. Bijvoorbeeld, u hebt een cloudservice met de naam *MijnService*, en twee virtuele machines met de naam *myVM1* en *myVM2*. En uw cloudservice heeft meerdere VIP's, een van deze met de naam *Vip2*. Als u ervoor zorgen wilt dat alle verkeer naar poort *81* op *Vip2* wordt verdeeld tussen *myVM1* en *myVM2* op poort *8181* , voer het volgende PowerShell-script:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

U kunt ook de load balancer voor het gebruik van een andere VIP-adres bijwerken. Als u de onderstaande PowerShell-opdracht uitvoert, wordt u bijvoorbeeld de load balancer-set te gebruiken van een VIP-adres met de naam Vip1 wijzigen:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Volgende stappen

[Logboekanalyse voor Azure-taakverdeling](load-balancer-monitor-log.md)

[Overzicht van Internet internetgerichte load balancer](load-balancer-internet-overview.md)

[Aan de slag op de internetgerichte load balancer](load-balancer-get-started-internet-arm-ps.md)

[Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

[Gereserveerde IP-adres REST-API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)
