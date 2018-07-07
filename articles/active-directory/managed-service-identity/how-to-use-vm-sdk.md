---
title: Een Azure VM Managed Service Identity gebruiken met Azure-SDK 's
description: Codevoorbeelden voor het gebruik van Azure-SDK's met een Azure VM-MSI-bestand.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 1c7a0d10f8ff005d90bb77f33bf40a00f97e078f
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901749"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Een Azure VM Managed Service Identity (MSI) gebruiken met Azure-SDK 's 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
In dit artikel bevat een lijst van de SDK-voorbeelden die laten gebruik van hun respectieve Azure SDK ondersteuning voor MSI-bestand zien.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Connect'-functie in Azure portal, op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI-bestand op een virtuele machine [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 

## <a name="sdk-code-samples"></a>SDK-codevoorbeelden

| SDK             | Codevoorbeeld |
| --------------- | ----------- |
| .NET            | [Een Azure Resource Manager sjabloon implementeren vanuit een Windows-VM met behulp van de beheerde Service-identiteit](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure-services aanroepen vanuit een Linux-VM met beheerde Service-identiteit](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Beheren van resources met behulp van de beheerde Service-identiteit](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [MSI gebruiken voor het verifiëren gewoon vanaf binnen een virtuele machine](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Resources beheren vanuit een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>Gerelateerde inhoud

- Zie [Azure-SDK's](https://azure.microsoft.com/downloads/) voor de volledige lijst van de Azure SDK-bronnen, met inbegrip van bibliotheek downloads, documentatie en meer.
- Zie voor het inschakelen van MSI-bestand op een Azure VM [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.








