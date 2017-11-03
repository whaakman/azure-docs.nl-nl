---
title: Maken van een Service-Principal voor Azure Stack | Microsoft Docs
description: Beschrijft hoe u maakt een nieuwe service-principal die kan worden gebruikt met de op rollen gebaseerde toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen.
services: azure-resource-manager
documentationcenter: na
author: heathl17
manager: byronr
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 058b01a37e2858801895fd22cf73dd6bd342ca04
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provide-applications-access-to-azure-stack"></a>Toepassingen toegang bieden tot Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Wanneer een toepassing toegang moet tot het implementeren of via Azure Resource Manager-resources in Azure-Stack configureren, maakt u een service principal, een referentie voor uw toepassing is.  Vervolgens kunt u alleen de vereiste machtigingen om deze service-principal delegeren.  

Als u bijvoorbeeld mogelijk hebt u een configuration management-hulpprogramma dat gebruikmaakt van Azure Resource Manager voor het inventariseren van Azure-resources.  In dit scenario kunt u een service-principal maken, de lezersrol verlenen aan deze service-principal en beperken van het hulpprogramma configuration management naar alleen-lezen toegang. 

Service-principals zijn beter voor het uitvoeren van de app met uw eigen referenties, omdat:

* U kunt machtigingen toewijzen aan de service-principal die anders zijn dan de accountmachtigingen van uw eigen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* U hoeft niet te wijzigen van de app referenties als uw verantwoordelijkheden wijzigt.
* U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een onbewaakt script.  

## <a name="getting-started"></a>Aan de slag

Afhankelijk van hoe u Azure-Stack hebt geïmplementeerd, begint u met het maken van een service principal.  Dit document begeleidt u bij het maken van een service-principal voor beide [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) en [Active Directory Federation Services(AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Als u de service-principal hebt gemaakt, een reeks stappen algemene op AD FS en Azure Active Directory worden gebruikt om [machtigingen delegeren](azure-stack-create-service-principals.md#assign-role-to-service-principal) aan de rol.     

## <a name="create-service-principal-for-azure-ad"></a>Service-principal maken voor Azure AD

Als u Azure-Stack gebruikmaken van Azure AD als identiteit archief hebt geïmplementeerd, kunt u de service-principals, net zoals u zou voor Azure doen maken.  Deze sectie wordt beschreven hoe u de stappen uitvoert via de portal.  Controleer of u hebt de [Azure AD-machtigingen vereist](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) voordat u begint.

### <a name="create-service-principal"></a>Een service-principal maken
In deze sectie maakt maakt u een toepassing (service-principal) in Azure AD waarmee uw toepassing.

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **App registraties** > **toevoegen**   
3. Geef een naam en de URL voor de toepassing. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

U hebt een service-principal gemaakt voor uw toepassing.

### <a name="get-credentials"></a>Referenties ophalen
Wanneer u programmatisch zich aanmeldt, kunt u de ID gebruiken voor uw toepassing en een verificatiesleutel. Als u deze waarden, gebruikt u de volgende stappen uit:

1. Van **App registraties** in Active Directory, selecteer uw toepassing.

2. Kopieer de **toepassings-ID** en op te slaan in uw toepassingscode. De toepassingen in de [voorbeeldtoepassingen](#sample-applications) sectie verwijzen naar deze waarde als de client-id.

     ![client-id](./media/azure-stack-create-service-principal/image12.png)
3. Selecteer voor het genereren van een verificatiesleutel **sleutels**.

4. Geef een beschrijving van de sleutel en een duur voor de sleutel. Wanneer u klaar bent, selecteer **opslaan**.

Na het opslaan van de sleutel, wordt de waarde van de sleutel weergegeven. Deze waarde niet kopiëren omdat u niet kan ophalen van de sleutel later. U kunt de sleutelwaarde opgeven met de toepassings-ID aan te melden als de toepassing. Slaat de waarde van de sleutel waar uw toepassing deze kan worden opgehaald.

![sleutel wordt opgeslagen](./media/azure-stack-create-service-principal/image15.png)


Als u klaar is, gaat u verder met [uw toepassing een rol toewijst](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Service-principal maken voor AD FS
Als u Azure-Stack met AD FS hebt geïmplementeerd, kunt u PowerShell naar een service-principal maken, een rol voor toegang toe te wijzen en aan te melden met die identiteit PowerShell.

### <a name="before-you-begin"></a>Voordat u begint

[Download de hulpprogramma's voor het werken met Azure-Stack op uw lokale computer.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>De identiteit van PowerShell-module importeren
Nadat u de hulpprogramma's hebt gedownload, navigeer naar de map voor gedownloade en de identiteit van PowerShell-module importeren met behulp van de volgende opdracht:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

Wanneer u de module importeren, wordt een foutbericht 'AzureStack.Connect.psm1 niet digitaal is ondertekend. Het script wordt niet uitgevoerd op het systeem'. U lost dit probleem, kunt u instellen-uitvoeringsbeleid om toe te staan bij het uitvoeren van het script met de volgende opdracht in een PowerShell-sessie met verhoogde bevoegdheid:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>De service-principal maken
U kunt een Service-Principal maken met het uitvoeren van de volgende opdracht om ervoor te zorgen om bij te werken de *DisplayName* parameter:
```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose
```
### <a name="assign-a-role"></a>Een rol toewijzen
Zodra de Service-Principal is gemaakt, moet u [aan een rol toewijzen](azure-stack-create-service-principals.md#assign-role-to-service-principal)

### <a name="sign-in-through-powershell"></a>Meld u aan via PowerShell
Wanneer u hebt een rol is toegewezen, kunt u zich aanmeldt bij Azure Stack met behulp van de service-principal met de volgende opdracht:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>De rol toewijzen aan de service-principal
Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../../active-directory/role-based-access-built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van het bereik. Bijvoorbeeld, een toepassing met de rol Lezer voor een resourcegroep toe te voegen dat kan de resourcegroep en alle resources die deze bevat gelezen.

1. Ga naar het niveau van het bereik dat u wilt de toepassing toewijzen aan in de Stack van Azure-portal. Selecteer bijvoorbeeld als een rol bij het abonnementsbereik wilt toewijzen, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

2. Selecteer het bepaald abonnement (resourcegroep of resource) de toepassing toewijzen aan.

     ![Selecteer het abonnement voor de toewijzing](./media/azure-stack-create-service-principal/image16.png)

3. Selecteer **toegangsbeheer (IAM)**.

     ![Selecteer toegang](./media/azure-stack-create-service-principal/image17.png)

4. Selecteer **Toevoegen**.

5. Selecteer de rol die u wilt toewijzen aan de toepassing.

6. Zoeken naar uw toepassing en selecteer deze.

7. Selecteer **OK** voltooid de rol toe te wijzen. Ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Nu dat u hebt een service-principal gemaakt en een rol is toegewezen, kunt u beginnen met het gebruik van deze in uw toepassing te krijgen tot bronnen van de Azure-Stack.  

## <a name="next-steps"></a>Volgende stappen

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)