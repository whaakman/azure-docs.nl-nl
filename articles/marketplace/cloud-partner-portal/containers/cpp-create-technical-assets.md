---
title: Technische activa Azure Containers-installatiekopie maken | Microsoft Docs
description: De technische assets voor een Azure-container maken.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 56de300a95d750957494865f9871127b2a9c8283
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979730"
---
# <a name="prepare-your-container-technical-assets"></a>Voorbereiden van uw container technische activa

Dit artikel wordt beschreven stappen en vereisten voor het configureren van een container aanbieding Azure Marketplace.

## <a name="before-you-begin"></a>Voordat u begint

Controleer de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances) documentatie, waarmee u snelstartgidsen, zelfstudies en voorbeelden.

## <a name="fundamental-technical-knowledge"></a>Fundamentele technische kennis

Het ontwerpen, bouwen en testen van deze elementen tijd in beslag nemen en technische kennis van het Azure-platform en de technologieën die wordt gebruikt voor het bouwen van de aanbieding is vereist.
 
Naast uw domein oplossing voor uw IT-team moet beschikken over kennis van de volgende Microsoft-technologieën:

-   Basiskennis van [Azure Services](https://azure.microsoft.com/services/) 
-   Hoe u [ontwerpen en bouwen van Azure-toepassingen](https://azure.microsoft.com/solutions/architecture/)
-   Praktische kennis van [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) en [Azure Networking](https://azure.microsoft.com/services/?filter=networking)
-   Praktische kennis van [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
-   Praktische kennis van [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Voorgesteld hulpprogramma 's

Kies een of beide van de volgende scripts omgevingen voor het beheer van uw containerinstallatiekopie:

-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure-CLI](https://docs.microsoft.com/cli/azure)

Bovendien is het raadzaam de volgende hulpprogramma's toe te voegen aan uw ontwikkelomgeving:

-   [Azure-opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Extensie: [Azure Resource Manager-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Extensie: [Maak](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Extensie: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

We raden ook controleren van de beschikbare hulpprogramma's in de [Azure-ontwikkelhulpprogramma's](https://azure.microsoft.com/tools/) pagina en, als u Visual Studio, de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>De containerinstallatiekopie maken

- Maak en configureer de virtuele harde schijf (VHD) voor uw container virtuele machine (VM). Deze VHD bevat het besturingssysteem (Windows, Linux of Ubuntu) voor de container. Extra gegevensschijven mogelijk vereist zijn.
- Configureren van het besturingssysteem van de virtuele machine, VM-grootte, poorten te openen en eventuele gekoppelde gegevensschijven.
- Installeer de toepassing en andere software die nodig voor uw aanbieding is. Bijvoorbeeld: database-software, software van derden of een aangepaste toepassing.

## <a name="next-steps"></a>Volgende stappen

[Uw aanbieding container maken](./cpp-create-offer.md)