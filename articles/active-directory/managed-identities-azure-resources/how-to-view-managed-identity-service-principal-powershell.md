---
title: Om weer te geven van de service-principal van een beheerde identiteit met behulp van PowerShell
description: Stapsgewijze instructies voor het weergeven van de service-principal van een beheerde identiteit met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5aa5ab96ae85833e59ed40161cafcc1eb88379be
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485795"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>De service-principal van een beheerde identiteit met behulp van PowerShell weergeven

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u om weer te geven van de service-principal van een beheerde identiteit met behulp van PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/).
- Schakel [systeem toegewezen identiteit op een virtuele machine](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) of [toepassing](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- Installeer de nieuwste versie van [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="view-the-service-principal"></a>De service-principal weergeven

Deze volgende opdracht laat zien hoe u de service-principal van een virtuele machine of toepassing weergeven aan systeem toegewezen identiteit is ingeschakeld. Vervang `<VM or application name>` door uw eigen waarden.

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het weergeven van Azure AD-service-principals met behulp van PowerShell [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).


