---
title: Het gebruik van een Azure VM beheerde Service-identiteit met Azure SDK 's
description: Codevoorbeelden voor het gebruik van Azure SDK's met een Azure VM-MSI-bestand.
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 222acd1624ce025985bf1a3eef40eb124ab7678e
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Het gebruik van een Azure VM beheerde Service identiteit (MSI) met Azure SDK 's 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Dit artikel bevat een lijst met de SDK-voorbeelden die gebruik van de ondersteuning van hun respectieve Azure SDK voor MSI aantonen.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Verbinding'-functie in de Azure portal op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 

## <a name="sdk-code-samples"></a>SDK-codevoorbeelden

| SDK             | Codevoorbeeld |
| --------------- | ----------- |
| .NET            | [Een Azure Resource Manager sjabloon implementeren vanuit een Windows-VM met behulp van de Service-identiteit beheerd](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure-services aanroepen vanuit een Linux-VM met behulp van de Service-identiteit beheerd](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Beheren van resources met behulp van de Service-identiteit beheerd](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [MSI gebruiken om te verifiëren gewoon uit in een virtuele machine](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Resources beheren van een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>Gerelateerde inhoud

- Zie [Azure SDK's](https://azure.microsoft.com/downloads/) voor een volledige lijst met Azure SDK-bronnen, met inbegrip van bibliotheek downloads, documentatie en meer.
- Zie voor het inschakelen van MSI op een Azure VM [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.








