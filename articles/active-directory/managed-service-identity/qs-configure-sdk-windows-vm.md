---
title: Het configureren van een MSI-functionaliteit Azure virtuele machine met een Azure-SDK
description: Stap door stap instructies voor het configureren en gebruiken van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met een Azure-SDK.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 781f332b2892d9af536bf9a6f81642842285927b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Een VM beheerde Service identiteit (MSI) met een Azure-SDK configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (AD). U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor een virtuele machine van Azure, met een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure-SDK's met MSI-ondersteuning 

Azure ondersteunt meerdere programming platforms door een reeks [Azure SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt voor het ondersteunen van MSI en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt als aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET   | [Beheer van de bron van een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Opslag beheren vanaf een virtuele machine van MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Een virtuele machine maken met MSI ingeschakeld](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Een virtuele machine maken met MSI ingeschakeld](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Azure virtuele machine maken met een MSI-bestand](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder 'Configureren MSI voor een virtuele machine van Azure', voor meer informatie over hoe u kunt de Azure portal, PowerShell, CLI en resource-sjablonen ook gebruiken.

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
