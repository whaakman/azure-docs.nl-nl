---
title: Het configureren van een gebruiker toegewezen MSI voor een Azure-VM met een Azure-SDK
description: Stap door stapsgewijze instructies voor het configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine van Azure, met een Azure-SDK.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 19b6179803b8de4102818c1522b00e6b4881d0f0
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-sdk"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) configureren voor een virtuele machine met een Azure-SDK

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u het inschakelen en het verwijderen van een gebruiker toegewezen MSI voor een virtuele machine van Azure, met een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure-SDK's met MSI-ondersteuning 

Azure ondersteunt meerdere programming platforms door een reeks [Azure SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt voor het ondersteunen van MSI en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt als aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| Python | [Een virtuele machine maken met MSI ingeschakeld](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Azure virtuele machine maken met een MSI-bestand](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder 'Configureren MSI voor een virtuele machine van Azure', voor meer informatie over hoe u een gebruiker toegewezen MSI kunt configureren op een Azure VM.

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
