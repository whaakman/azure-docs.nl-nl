---
author: robinsh
ms.author: robin.shahan
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 163b479fdf2d6a4885604c52f36f707e558dc3a0
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011780"
---
## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Voorbereiden om Azure Resource Manager-aanvragen te verifiëren
U moet verifiëren dat de bewerkingen die u uitvoert op resources met behulp van de [Azure Resource Manager] [ lnk-authenticate-arm] met Azure Active Directory (AD). Er is de eenvoudigste manier om dit configureren met PowerShell of Azure CLI.

Installeer de [Azure PowerShell-cmdlets] [ lnk-powershell-install] voordat u doorgaat.

De volgende stappen laten zien hoe het instellen van wachtwoordverificatie voor een AD-toepassing met behulp van PowerShell. U kunt deze opdrachten uitvoeren in een standaard PowerShell-sessie.

1. Meld u aan uw Azure-abonnement met de volgende opdracht:

    ```powershell
    Connect-AzureRmAccount
    ```

1. Als u meerdere Azure-abonnementen hebt, u aanmelden bij Azure in, hebt u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht om de Azure-abonnementen beschikbaar voor gebruik weer te geven:

    ```powershell
    Get-AzureRMSubscription
    ```

    Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het beheren van uw IoT-hub te selecteren. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Maak een notitie van uw **TenantId** en **SubscriptionId**. U hebt deze later nog nodig.
3. Maak een nieuwe Azure Active Directory-toepassing met behulp van de volgende opdracht de tijdelijke aanduidingen vervangt:
   
   * **{Weergavenaam}:** een weergavenaam voor uw toepassing zoals **MySampleApp**
   * **{URL van startpagina}:** de URL van de startpagina van uw app, zoals **http://mysampleapp/home**. Deze URL heeft niet nodig om te verwijzen naar een echte toepassing.
   * **{Toepassings-id}:** Een unieke id zoals **http://mysampleapp**. Deze URL heeft niet nodig om te verwijzen naar een echte toepassing.
   * **{Password}:** Een wachtwoord dat u gebruikt voor verificatie met uw app.
     
     ```powershell
     $SecurePassword=ConvertTo-SecureString {password} –asplaintext –force
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password $SecurePassword
     ```
4. Noteer de **ApplicationId** van de toepassing die u hebt gemaakt. U nodig dit later.
5. Maak een nieuwe serviceprincipal met behulp van de volgende opdracht en vervangt **{MyApplicationId}** met de **ApplicationId** uit de vorige stap:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Instellen van een roltoewijzing met behulp van de volgende opdracht en vervangt **{MyApplicationId}** met uw **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

U bent nu klaar voor het maken van de Azure AD-toepassing waarmee u te verifiëren vanaf uw aangepaste C# toepassing. Verderop in deze zelfstudie moet u de volgende waarden:

* TenantId
* SubscriptionId
* ApplicationId
* Wachtwoord

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
