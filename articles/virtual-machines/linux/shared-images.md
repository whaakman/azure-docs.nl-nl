---
title: Gedeelde VM-installatiekopieën maken met de Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u de Azure CLI gebruiken om te maken van een gedeelde installatiekopie van een virtuele machine in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: axayjo
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 08c7e84a27a4c8e9527083360dbd08296bd86775
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190051"
---
# <a name="preview-create-a-shared-image-gallery-with-the-azure-cli"></a>Preview: Een gedeelde afbeeldingengalerie maken met de Azure-CLI

Een [gedeelde afbeeldingengalerie](shared-image-galleries.md) vereenvoudigt de aangepaste installatiekopie voor uw organisatie te delen. Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor het opstarten van configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. 

De galerie met installatiekopieën gedeeld kunt u uw aangepaste VM-installatiekopieën delen met anderen in uw organisatie, binnen of tussen regio's binnen een AAD-tenant. Kies welke installatiekopieën die u wilt delen, welke regio's die u wilt maken ze beschikbaar zijn in, en die u wilt delen. U kunt meerdere galerieën kunt maken, zodat u kunt gedeelde afbeeldingen logisch groeperen. 

De galerie is een op het hoogste niveau resource die volledig op rollen gebaseerd toegangsbeheer (RBAC) biedt. Installatiekopieën kunnen worden bijgehouden, en u kunt kiezen voor het repliceren van elke versie van de installatiekopie naar een andere set Azure-regio's. De galerie werkt alleen met beheerde installatiekopieën.

De galerie met installatiekopieën van gedeelde functie heeft meerdere resourcetypen. Er wordt met behulp van of het bouwen van deze in dit artikel:

| Resource | Description|
|----------|------------|
| **Beheerde installatiekopie** | Dit is een basic-installatiekopie die kan worden alleen wordt gebruikt of worden gebruikt voor het maken van een **installatiekopieversie** in een galerie met installatiekopieën. Beheerde installatiekopieën zijn gemaakt op basis van gegeneraliseerde VM's. Een beheerde installatiekopie is een speciaal type VHD die kan worden gebruikt voor het maken van meerdere virtuele machines en kan nu worden gebruikt om gedeelde installatiekopie versies te maken. |
| **Galerie met installatiekopieën** | De Azure Marketplace, zoals een **afbeeldingengalerie** is een opslagplaats voor het beheren en delen van afbeeldingen, maar u bepaalt wie toegang heeft. |
| **De definitie van installatiekopie** | Afbeeldingen worden gedefinieerd in een galerie en informatie over de installatiekopie en de vereisten voor het gebruik van deze intern uitvoeren. Dit omvat of de installatiekopie Windows of Linux, release-opmerkingen en vereisten voor minimale en maximale hoeveelheid geheugen is. Het is een definitie van een type van de installatiekopie. |
| **De versie van installatiekopie** | Een **installatiekopieversie** is wat u gebruikt om een virtuele machine maken wanneer u een galerie. U kunt meerdere versies van een installatiekopie kunt hebben, indien nodig voor uw omgeving. Een beheerde installatiekopie, bijvoorbeeld wanneer u een **installatiekopieversie** voor het maken van een virtuele machine, de versie van de installatiekopie die wordt gebruikt voor het maken van nieuwe schijven voor de virtuele machine. Versies van een installatiekopie kunnen meerdere keren worden gebruikt. |



[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine maken van de installatiekopie versie met [az vm maken](/cli/azure/vm#az-vm-create).

```azurecli-interactive 
az vm create\
   -g myGalleryRG \
   -n myVM \
   --image "/subscriptions/<subscription-ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
   --generate-ssh-keys
```

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]



## <a name="next-steps"></a>Volgende stappen
U kunt ook de galerie met installatiekopieën van gedeelde resources met behulp van sjablonen maken. Er zijn verschillende Azure-Snelstartsjablonen beschikbaar: 

- [Een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [De definitie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [De versie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Een virtuele machine maken vanaf de versie van installatiekopie](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Zie voor meer informatie over gedeelde installatiekopie galerieën, de [overzicht](shared-image-galleries.md). Als u problemen ondervindt, raadpleegt u [probleemoplossing gedeeld afbeeldingsgalerieën](troubleshooting-shared-images.md).
