---
title: Om weer te geven van de service-principal van een beheerde identiteit met behulp van PowerShell
description: Stapsgewijze instructies voor het weergeven van de service-principal van een beheerde identiteit met behulp van PowerShell.
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
ms.date: 11/29/2018
ms.author: daveba
ms.openlocfilehash: 0e8558d96803e15da355f083c6d1988d5ee6cb68
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882678"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>De service-principal van een beheerde identiteit met behulp van PowerShell weergeven

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u om weer te geven van de service-principal van een beheerde identiteit met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/).
- Schakel [systeem toegewezen identiteit op een virtuele machine](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) of [toepassing](/azure/app-service/app-service-managed-service-identity#adding-a-system-assigned-identity).
- Als u ervoor kiest om PowerShell lokaal te installeren en gebruiken, is voor deze zelfstudie versie 5.7.0 of hoger van de Azure PowerShell-module vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 
- Als u PowerShell lokaal uitvoert, moet u ook het volgende doen: 
    - Voer `Login-AzureRmAccount` uit om een verbinding op te zetten met Azure.
    - Installeer de [nieuwste versie van PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Voer `Install-Module -Name PowerShellGet -AllowPrerelease` uit om de voorlopige versie van de `PowerShellGet`-module op te halen (mogelijk moet u met `Exit` de huidige PowerShell-sessie afsluiten nadat u met deze opdracht de `AzureRM.ManagedServiceIdentity`-module hebt geïnstalleerd).
    - Voer `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` voor het installeren van de voorlopige versie van de `AzureRM.ManagedServiceIdentity` module om uit te voeren van de gebruiker toegewezen identiteit bewerkingen in dit artikel worden beheerd.

## <a name="view-the-service-principal"></a>De service-principal weergeven

Deze volgende opdracht laat zien hoe u de service-principal van een virtuele machine of toepassing weergeven aan systeem toegewezen identiteit is ingeschakeld. Vervang `<VM or application name>` door uw eigen waarden.

```PowerShell
Get-AzureRmADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het weergeven van Azure AD-service-principals met behulp van PowerShell [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal).


