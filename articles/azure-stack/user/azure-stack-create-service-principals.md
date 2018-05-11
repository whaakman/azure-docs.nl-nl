---
title: Maken van een Service-Principal voor Azure Stack | Microsoft Docs
description: Beschrijft het maken van een service-principal die kan worden gebruikt met de op rollen gebaseerde toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2018
ms.author: mabrigg
ms.openlocfilehash: de5712fd7b48a759b366f5b9808bbbefc6e305cd
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Toepassingen toegang geven tot bronnen van de Azure-Stack door het service-principals maken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een toepassing toegang geven tot Azure-Stack bronnen door het maken van een service principal die gebruikmaakt van Azure Resource Manager. Een service-principal kunt u de specifieke machtigingen delegeren met behulp van [toegangsbeheer op basis van rollen](azure-stack-manage-permissions.md).

Als een best practice moet u de service-principals gebruiken voor uw toepassingen. Service-principals zijn beter voor het uitvoeren van een app met uw eigen referenties voor de volgende redenen:

* U kunt machtigingen toewijzen aan de service-principal die anders zijn dan de accountmachtigingen van uw eigen. Een service-principal machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* U hoeft niet te wijzigen van de app referenties als uw rol of verantwoordelijkheden wijzigt.
* U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een onbewaakt script.

## <a name="example-scenario"></a>Voorbeeldscenario

U hebt een configuration management-app die nodig voor het inventariseren van Azure-resources met Azure Resource Manager. U kunt een service-principal maken en toewijzen aan de rol Lezer toe. Deze rol geeft de app alleen-lezen toegang tot Azure-resources.

## <a name="getting-started"></a>Aan de slag

De stappen in dit artikel als richtlijn te gebruiken:

* Maken van een service-principal voor uw app.
* Uw app registreren en een verificatiesleutel maken.
* Uw app aan een rol toewijzen.

De manier waarop u Active Directory hebt geconfigureerd voor Azure-Stack bepaalt hoe u een service-principal maken. Kies een van de volgende opties:

* Maken van een service principal voor [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Maken van een service principal voor [Active Directory Federation Services (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

De stappen voor het toewijzen van een service-principal aan een rol dezelfde voor Azure AD en AD FS. Nadat u de service-principal gemaakt, kunt u [machtigingen delegeren](azure-stack-create-service-principals.md#assign-role-to-service-principal) toewijzen aan een rol.

## <a name="create-a-service-principal-for-azure-ad"></a>Een service-principal maken voor Azure AD

Als uw Azure-Stack Azure AD als de identity-store gebruikt, kunt u een service principal met dezelfde stappen als in Azure met behulp van de Azure portal maken.

>[!NOTE]
Controleer dat u hebt de [Azure AD-machtigingen vereist](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) voordat u een service-principal maken.

### <a name="create-service-principal"></a>Een service-principal maken

Maken van een service-principal voor uw toepassing:

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **App registraties** > **toevoegen**.
3. Geef een naam en URL voor de toepassing op. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

### <a name="get-credentials"></a>Referenties ophalen

Wanneer u zich aanmeldt via een programma, kunt u de ID gebruiken voor uw toepassing en een verificatiesleutel. Deze waarden ophalen:

1. Van **App registraties** in Active Directory, selecteer uw toepassing.

2. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. De toepassingen in de [voorbeeldtoepassingen](#sample-applications) gebruiken **client-id** met betrekking tot de **toepassings-ID**.

     ![Toepassings-ID voor de toepassing](./media/azure-stack-create-service-principal/image12.png)
3. Selecteer **Sleutels** om een verificatiesleutel te genereren.

4. Geef een beschrijving van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

>[!IMPORTANT]
Nadat u de sleutel opslaan **waarde** wordt weergegeven. Noteer deze waarde omdat u de sleutel later niet ophalen. Sla de sleutelwaarde op waar uw toepassing deze kan ophalen.

![Waarschuwing: sleutelwaarde voor opgeslagen sleutel.](./media/azure-stack-create-service-principal/image15.png)

De laatste stap is [uw toepassing een rol toewijst](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Service-principal maken voor AD FS

Als u Azure-Stack AD FS gebruikt als de identity-archief hebt geïmplementeerd, kunt u PowerShell gebruiken voor de volgende taken:

* Een service-principal maken.
* Service-principal toewijzen aan een rol.
* Meld u met de service-principal-identiteit.

### <a name="before-you-begin"></a>Voordat u begint

[Download de vereiste Azure Stack-hulpprogramma's op uw lokale computer.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>De identiteit van PowerShell-module importeren

Navigeer naar de downloadmap voor de Azure-Stack-hulpprogramma's en de identiteit van PowerShell-module importeren met behulp van de volgende opdracht:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

Als u de Identity-module importeren, krijgt u mogelijk dit foutbericht wordt weergegeven: 'AzureStack.Connect.psm1 niet digitaal is ondertekend. Het script wordt niet uitgevoerd op het systeem'.

U kunt dit probleem oplossen, moet u het uitvoeringsbeleid zodat het uitvoeren van het script configureren. Als u wilt het uitvoeringsbeleid instellen, voer de volgende opdracht in een PowerShell-sessie met verhoogde bevoegdheid:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>De service-principal maken

U kunt een service-principal maken met de volgende opdracht om ervoor te zorgen voor het bijwerken van de **DisplayName** parameter:

```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose

```

### <a name="assign-a-role"></a>Een rol toewijzen

Nadat u de service-principal gemaakt, moet u [toewijzen aan een rol](azure-stack-create-service-principals.md#assign-role-to-service-principal).

### <a name="sign-in-using-powershell"></a>Meld u aan met behulp van PowerShell

U kunt zich aanmelden naar Azure-Stack door de volgende opdracht uit te voeren, om ervoor te zorgen om bij te werken de **EnvironmentName** parameter met de naam van uw app:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId `
 -TenantId $directoryTenantId
```

## <a name="assign-the-service-principal-to-a-role"></a>De service-principal toewijzen aan een rol

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
Op het niveau van een abonnement, resourcegroep of een bron, kunt u een rol bereik instellen. Machtigingen worden overgenomen op lagere niveaus van het bereik. Een app met de rol Lezer voor een resourcegroep betekent bijvoorbeeld dat de app de bronnen in de resourcegroep kan lezen.

Gebruik de volgende stappen uit als richtlijn voor een rol toewijzen aan een service-principal.

1. Ga naar het niveau van het bereik dat u wilt dat de toepassing toewijzen aan in de Stack van Azure-portal. Selecteer bijvoorbeeld als een rol bij het abonnementsbereik wilt toewijzen, **abonnementen**.

2. Selecteer het abonnement de toepassing toewijzen aan. In dit voorbeeld is het abonnement Visual Studio Enterprise.

     ![Selecteer Visual Studio Enterprise-abonnement voor toewijzing](./media/azure-stack-create-service-principal/image16.png)

3. Selecteer **Access Control (IAM)** voor het abonnement.

     ![Selecteer toegangsbeheer](./media/azure-stack-create-service-principal/image17.png)

4. Selecteer **Toevoegen**.

5. Selecteer de rol die u wilt toewijzen aan de toepassing.

6. Zoeken naar uw toepassing en selecteer deze.

7. Selecteer **OK** voltooid de rol toe te wijzen. U kunt uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope zien.

Nu dat u hebt een service-principal gemaakt en een rol is toegewezen, kan uw toepassing toegang krijgen tot bronnen van de Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
