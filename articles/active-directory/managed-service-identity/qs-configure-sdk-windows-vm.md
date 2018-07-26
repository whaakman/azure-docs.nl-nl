---
title: Het configureren van een beheerde Service-identiteit ingeschakeld virtuele Azure-machine met een Azure-SDK
description: Voor stap door stap instructies voor het configureren en gebruiken van een beheerde Service-identiteit op een Azure-VM, met een Azure-SDK.
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
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 2763c78d309f5a90d68429caa46581e50f8b4303
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257657"
---
# <a name="configure-a-vm-managed-service-identity-using-an-azure-sdk"></a>De identiteit van een VM-Managed-Service met behulp van een Azure-SDK configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (AD). U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en verwijderen van de beheerde Service-identiteit voor een Azure-VM met een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-service-identity-support"></a>Azure SDK's met ondersteuning voor beheerde Service-identiteit 

Azure biedt ondersteuning voor meerdere programmeertalen platforms via een reeks [Azure-SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt voor de ondersteuning van beheerde Service-identiteit en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt wanneer aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET   | [Resource beheren vanaf een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Opslag beheren met een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Een virtuele machine met ingeschakelde MSI maken](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Een virtuele machine met ingeschakelde MSI maken](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Azure virtuele machine maken met een MSI-bestand](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder 'configureren beheerde Service-identiteit voor een Azure-VM", voor meer informatie over hoe u kunt de Azure portal, PowerShell, CLI en resource-sjablonen ook gebruiken.

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.
