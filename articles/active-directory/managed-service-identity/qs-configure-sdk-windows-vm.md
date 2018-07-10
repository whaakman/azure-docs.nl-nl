---
title: Een MSI-functionaliteit Azure-VM met een Azure-SDK configureren
description: Voor stap door stap instructies voor het configureren en gebruiken van een Managed Service Identity (MSI) op een Azure-VM, met een Azure-SDK.
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
ms.openlocfilehash: dee4a3e27623150ce3fa648d73542db0cbb23e93
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901439"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Een VM-Managed Service Identity (MSI) met behulp van een Azure-SDK configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (AD). U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en verwijderen van MSI-bestand voor een Azure-VM met een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure SDK's met MSI-ondersteuning 

Azure biedt ondersteuning voor meerdere programmeertalen platforms via een reeks [Azure-SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt voor het ondersteunen van MSI-bestand en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt wanneer aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET   | [Resource beheren vanaf een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Opslag beheren met een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Een virtuele machine met ingeschakelde MSI maken](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Een virtuele machine met ingeschakelde MSI maken](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Azure virtuele machine maken met een MSI-bestand](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder 'configureren MSI-bestand voor een Azure-VM", voor meer informatie over hoe u ook de Azure portal, PowerShell, CLI en resource-sjablonen kunt gebruiken.

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.
