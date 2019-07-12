---
title: Galerie met installatiekopieën delen voor tenants in Azure | Microsoft Docs
description: Leer hoe u VM-installatiekopieën in Azure met behulp van gedeelde Afbeeldingsgalerieën-tenants delen.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: c26abe948fa415c780d543c615c34af2091cfbc7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709162"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Galerie met VM-installatiekopieën in Azure-tenants delen

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


> [!IMPORTANT]
> U kunt de portal niet gebruiken voor het implementeren van een virtuele machine uit een afbeelding in een andere azure-tenant. Voor het maken van een virtuele machine van een installatiekopie die wordt gedeeld tussen tenants, moet u de [Azure CLI](../linux/share-images-across-tenants.md) of Powershell.

## <a name="create-a-vm-using-powershell"></a>Maken van een virtuele machine met behulp van PowerShell


Meld u aan bij beide tenants met behulp van de toepassings-ID, secret en tenant-ID. 

```azurepowershell-interactive
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

De virtuele machine maken in de resourcegroep die gemachtigd op de app-registratie is. De informatie in dit voorbeeld vervangen door uw eigen.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $image `
   -Location "South Central US" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

## <a name="next-steps"></a>Volgende stappen

U kunt gedeelde installatiekopie ook maken galerie resources met behulp van de [Azure-portal](shared-images-portal.md).