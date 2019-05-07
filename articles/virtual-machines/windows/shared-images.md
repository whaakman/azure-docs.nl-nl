---
title: Gedeelde VM-installatiekopieën maken met Azure PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell voor het maken van de installatiekopie van een gedeelde virtuele machine in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: a3c035715de736b816027b08a1e242e6a65d1f13
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148747"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>Een gedeelde afbeeldingengalerie maken met Azure PowerShell 

Een [gedeelde afbeeldingengalerie](shared-image-galleries.md) vereenvoudigt de aangepaste installatiekopie voor uw organisatie te delen. Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt om de bootstrap-implementatietaken zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. 

De galerie met installatiekopieën gedeeld kunt u uw aangepaste VM-installatiekopieën delen met anderen in uw organisatie, binnen of tussen regio's binnen een AAD-tenant. Kies welke installatiekopieën die u wilt delen, welke regio's die u wilt maken ze beschikbaar zijn in, en die u wilt delen. U kunt meerdere galerieën kunt maken, zodat u kunt gedeelde afbeeldingen logisch groeperen. 

De galerie is een op het hoogste niveau resource die volledig op rollen gebaseerd toegangsbeheer (RBAC) biedt. Installatiekopieën kunnen worden bijgehouden, en u kunt kiezen voor het repliceren van elke versie van de installatiekopie naar een andere set Azure-regio's. De galerie werkt alleen met beheerde installatiekopieën.

De galerie met installatiekopieën van gedeelde functie heeft meerdere resourcetypen. Er wordt met behulp van of het bouwen van deze in dit artikel:

| Resource | Description|
|----------|------------|
| **Beheerde installatiekopie** | Dit is een basic-installatiekopie die kan worden alleen wordt gebruikt of worden gebruikt voor het maken van een **installatiekopieversie** in een galerie met installatiekopieën. Beheerde installatiekopieën zijn gemaakt op basis van gegeneraliseerde VM's. Een beheerde installatiekopie is een speciaal type VHD die kan worden gebruikt voor het maken van meerdere virtuele machines en kan nu worden gebruikt om gedeelde installatiekopie versies te maken. |
| **Galerie met installatiekopieën** | De Azure Marketplace, zoals een **afbeeldingengalerie** is een opslagplaats voor het beheren en delen van afbeeldingen, maar u bepaalt wie toegang heeft. |
| **De definitie van installatiekopie** | Afbeeldingen worden gedefinieerd in een galerie en informatie over de installatiekopie en de vereisten voor het gebruik van deze intern uitvoeren. Dit omvat of de installatiekopie Windows of Linux, release-opmerkingen en vereisten voor minimale en maximale hoeveelheid geheugen is. Het is een definitie van een type van de installatiekopie. |
| **De versie van installatiekopie** | Een **installatiekopieversie** is wat u gebruikt om een virtuele machine maken wanneer u een galerie. U kunt meerdere versies van een installatiekopie kunt hebben, indien nodig voor uw omgeving. Een beheerde installatiekopie, bijvoorbeeld wanneer u een **installatiekopieversie** voor het maken van een virtuele machine, de versie van de installatiekopie die wordt gebruikt voor het maken van nieuwe schijven voor de virtuele machine. Versies van een installatiekopie kunnen meerdere keren worden gebruikt. |

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt het voorbeeld in dit artikel hebt voltooid, moet u een bestaande beheerde installatiekopie hebben. U kunt volgen [zelfstudie: Een aangepaste installatiekopie van een Azure-VM maken met Azure PowerShell](tutorial-custom-images.md) maken indien nodig. Wanneer het uitvoeren van dit artikel vervangen door de resourcegroep en VM-namen van waar nodig.

[!INCLUDE [virtual-machines-common-shared-images-powershell](../../../includes/virtual-machines-common-shared-images-powershell.md)]
 
## <a name="create-vms-from-an-image"></a>Virtuele machines maken van een installatiekopie

Als de versie van de installatiekopie is voltooid, kunt u een of meer nieuwe virtuele machines maken. Met behulp van de vereenvoudigde parameter ingesteld voor de [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet, hoeft u alleen te bieden van afbeeldings-ID van de versie van de installatiekopie. 

Dit voorbeeld maakt u een virtuele machine met de naam *myVMfromImage*, in de *myResourceGroup* in de *VS-Oost* datacenter.

```azurepowershell-interactive
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $imageVersion.Id `
   -Location "South Central US" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

[!INCLUDE [virtual-machines-common-gallery-list-ps](../../../includes/virtual-machines-common-gallery-list-ps.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-ps](../../../includes/virtual-machines-common-shared-images-update-delete-ps.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt ook de galerie met installatiekopieën van gedeelde resources met behulp van sjablonen maken. Er zijn verschillende Azure-Snelstartsjablonen beschikbaar: 

- [Een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [De definitie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [De versie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Een virtuele machine maken vanaf de versie van installatiekopie](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Zie voor meer informatie over gedeelde installatiekopie galerieën, de [overzicht](shared-image-galleries.md). Als u problemen ondervindt, raadpleegt u [probleemoplossing gedeeld afbeeldingsgalerieën](troubleshooting-shared-images.md).

