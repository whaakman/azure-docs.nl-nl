---
title: Windows virtuele bureaublad Preview-service-principals en roltoewijzingen maken met behulp van PowerShell - Azure
description: Over het maken van service-principals en rollen toewijzen met behulp van PowerShell in Windows Virtual Desktop Preview.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: 44c823653ecbad1c4dd1fd35b676c8a6d8bd1620
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206665"
---
# <a name="tutorial-create-service-principals-and-role-assignments-by-using-powershell"></a>Zelfstudie: Service-principals en roltoewijzingen maken met behulp van PowerShell

Service-principals zijn identiteiten die u in Azure Active Directory maken kunt voor het toewijzen van rollen en machtigingen voor een specifiek doel. In Windows Virtual Desktop Preview, kunt u een service principal te maken:

- Specifieke virtuele Windows-bureaublad-beheertaken automatiseren.
- Als de referenties in plaats van gebruikers MFA vereist gebruiken bij het uitvoeren van een Azure Resource Manager-sjabloon voor virtuele Windows-bureaublad.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een serviceprincipal maken in Azure Active Directory.
> * Maak een roltoewijzing in virtuele Windows-bureaublad.
> * Meld u aan virtuele Windows-bureaublad met behulp van de service-principal.

## <a name="prerequisites"></a>Vereisten

Voordat u service-principals en roltoewijzingen maken kunt, moet u drie dingen doen:

1. Installeer de module AzureAD. Voor het installeren van de module PowerShell ook uitvoeren als beheerder en voer de volgende cmdlet uit:

    ```powershell
    Install-Module AzureAD
    ```

2. Voer de volgende cmdlets met de waarden in de aanhalingstekens vervangen door de waarden die relevant zijn voor uw sessie.

    ```powershell
    $myTenantName = "<my-tenant-name>"
    ```

3. Volg alle instructies in dit artikel in dezelfde PowerShell-sessie. U werkt deze mogelijk niet als u het venster sluiten en terug naar het later opnieuw.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Een service-principal maken in Azure Active Directory

Nadat u de vereisten hebt voldaan in de PowerShell-sessie, voert u de volgende PowerShell-cmdlets voor het maken van een multitenant service principal in Azure.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Een roltoewijzing maken in Windows Virtual Desktop Preview

Nu dat u een service principal gemaakt hebt, kunt u het aanmelden bij virtuele Windows-bureaublad. Zorg ervoor dat u zich aanmelden met een account met machtigingen voor het maken van de roltoewijzing.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voer de volgende PowerShell-cmdlets voor de verbinding met virtuele Windows-bureaublad en een roltoewijzing voor de service-principal maken.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Meld u aan met de service-principal

Nadat u een roltoewijzing voor de service principal gemaakt, zorg er dan voor dat de service-principal kan zich aanmelden bij virtuele Windows-bureaublad door de volgende cmdlet:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Nadat u zich hebt aangemeld, zorg er dan voor dat alles werkt door een paar Windows virtuele bureaublad PowerShell-cmdlets met de service-principal te testen.

## <a name="view-your-credentials-in-powershell"></a>Uw referenties in PowerShell weergeven

Voordat u de PowerShell-sessie beëindigen, weergeven van uw referenties en schrijf ze op voor toekomstig gebruik. Het wachtwoord is vooral belangrijk omdat u niet meer ophalen nadat u deze PowerShell-sessie sluit.

Dit zijn de drie referenties die te noteren en de cmdlets die u moet uitvoeren om op te halen ze:

- Wachtwoord:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Tenant-ID:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Toepassings-ID:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt gemaakt van de service-principal en een rol in uw tenant virtuele Windows-bureaublad toegewezen, kunt u het maken van een groep host. Voor meer informatie over pools host, verder met de zelfstudie voor het maken van een host van toepassingen in virtuele Windows-bureaublad.

 > [!div class="nextstepaction"]
 > [Zelfstudie voor virtuele Windows-bureaublad host pool](./create-host-pools-azure-marketplace.md)
