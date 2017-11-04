---
title: Een zoned openbare IP-adres maken met PowerShell | Microsoft Docs
description: Maak een openbare IP-adres in een zone beschikbaarheid met PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: bfeba57036338b4e325d2f122443f2cde0373eed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-powershell"></a>Maken van een openbaar IP-adres in een zone beschikbaarheid met PowerShell

U kunt een openbare IP-adres in een zone in Azure beschikbaarheid (preview) implementeren. Een zone beschikbaarheid is een fysiek afgescheiden zone in een Azure-regio. Leer hoe u het volgende doet:

> * Maken van een openbaar IP-adres in een zone beschikbaarheid
> * Verwante bronnen die zijn gemaakt in de zone beschikbaarheid identificeren
  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

In dit artikel is vereist dat u beschikt over versie 4.4.0 of hoger van de module AzureRM geïnstalleerd. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de module AzureRM op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```
## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroup* wordt gemaakt in de *westeurope* regio. *westeurope* is een van de Azure-regio's die ondersteuning biedt voor beschikbaarheid zones in preview.

```powershell
New-AzureRmResourceGroup -Name AzTest -Location westeurope
```

## <a name="create-a-zonal-public-ip-address"></a>Een zonal openbaar IP-adres maken

Maak een openbaar IP-adres in een beschikbaarheidsgroep zone met de volgende opdracht:

```powershell
    New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroup `
        -Name myPublicIp `
        -Location westeurope `
        -AllocationMethod Static `
        -Zone 2
```

> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

## <a name="get-zone-information-about-a-public-ip-address"></a>Zone-informatie over een openbaar IP-adres ophalen

Haal de gegevens van de zone van een openbaar IP-adres met de volgende opdracht:

```powershell
Get-AzureRmPublicIpAddress ` 
    -ResourceGroup myResourceGroup `
    -Name myPublicIp
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beschikbaarheid zones](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)
- Meer informatie over [openbare IP-adressen](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 