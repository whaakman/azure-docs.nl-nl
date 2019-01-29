---
title: Een Azure-SDK gebruiken voor het configureren van een virtuele machine met beheerde identiteiten voor Azure-resources
description: Stapsgewijze beheerde instructies voor het configureren en gebruiken van identiteiten voor een Azure-resources op een Azure-VM, met een Azure-SDK.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: priyamo
ms.openlocfilehash: dfe0b46411c5ed4cb37cdb2c95ddb1b57407f435
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193621"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>Een VM met beheerde identiteiten voor Azure-resources met behulp van een Azure-SDK configureren

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (AD). U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en verwijderen van beheerde identiteiten voor Azure-resources voor een Azure-VM met een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Ondersteuning voor Azure-SDK's met beheerde identiteiten voor Azure-resources 

Azure biedt ondersteuning voor meerdere programmeertalen platforms via een reeks [Azure-SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt voor de ondersteuning van beheerde identiteiten voor Azure-resources, en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt wanneer aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET   | [Resource beheren van een virtuele machine ingeschakeld met beheerde identiteiten voor Azure-resources ingeschakeld](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Opslag van een virtuele machine ingeschakeld met beheerde identiteiten voor Azure-resources beheren](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Een virtuele machine maken met het systeem toegewezen beheerde identiteit ingeschakeld](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Een virtuele machine maken met het systeem toegewezen beheerde identiteit ingeschakeld](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Azure virtuele machine maken met een door het systeem toegewezen identiteit die is ingeschakeld](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder **identiteit configureren voor een Azure-VM**, voor meer informatie over hoe u ook de Azure portal, PowerShell, CLI en resource-sjablonen kunt gebruiken.
