---
title: Maken van een Service-Principal voor Azure Stack | Microsoft Docs
description: Beschrijft hoe u een nieuwe service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen te maken.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2018
ms.author: sethm
ms.openlocfilehash: 65fa9593b35af45ee9b8568bac5e4886909314e1
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092540"
---
# <a name="provide-applications-access-to-azure-stack"></a>Toepassingen toegang geven tot Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Wanneer een toepassing toegang hebben moet tot het implementeren of via Azure Resource Manager-resources configureren in Azure Stack, maakt u een service-principal, dit een referentie voor uw toepassing is. Vervolgens kunt u alleen de benodigde machtigingen om deze service-principal te delegeren.  

Een voorbeeld: als u mogelijk een configuratiebeheerprogramma die gebruikmaakt van Azure Resource Manager het inventariseren van Azure-resources. In dit scenario kunt u een service-principal maken, de rol van lezer verlenen aan deze service-principal en het hulpprogramma configuration management naar alleen-lezen toegang beperken. 

Service-principals zijn het geschiktst is voor het uitvoeren van de app met uw eigen referenties omdat:

* U kunt machtigingen toewijzen aan de service-principal die anders is dan de accountmachtigingen van uw eigen zijn. Deze machtigingen worden meestal beperkt tot de functies die met de app moeten kunnen worden uitgevoerd.
* U hoeft niet te wijzigen van de referenties van de app als uw verantwoordelijkheden wijzigt.
* U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een script zonder toezicht.  

## <a name="getting-started"></a>Aan de slag

Afhankelijk van hoe u Azure Stack hebt geïmplementeerd, begint u met een service-principal maken. Dit document beschrijft het maken van een service-principal voor zowel [Azure Active Directory (Azure AD)](#create-service-principal-for-azure-ad) en [Active Directory Federation Services(AD FS)](#create-service-principal-for-ad-fs). Als u de service-principal hebt gemaakt, een reeks stappen die gemeenschappelijk zijn voor zowel AD FS en Azure Active Directory worden gebruikt om [overdragen van machtigingen](#assign-role-to-service-principal) aan de rol.     

## <a name="create-service-principal-for-azure-ad"></a>Service-principal maken voor Azure AD

Als u Azure Stack met Azure AD als de identiteitsarchief hebt geïmplementeerd, kunt u service-principals net zoals u zou voor Azure doen maken. Deze sectie leest u hoe u de stappen via de portal uitvoert. Controleer of u hebt de [vereist Azure AD-machtigingen](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) voordat u begint.

### <a name="create-service-principal"></a>Een service-principal maken
In deze sectie maakt u een toepassing (service-principal) in Azure AD die staat voor uw toepassing.

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **App-registraties** > **nieuwe toepassing registreren**   
3. Geef een naam en URL op voor de toepassing. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

U kunt een service-principal voor uw toepassing hebt gemaakt.

### <a name="get-credentials"></a>Referenties ophalen
Wanneer u zich programmatisch aanmeldt, u de ID gebruiken voor uw toepassing, en voor een Web-app / API, een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Van **App-registraties** in Active Directory, selecteer uw toepassing.

2. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. De toepassingen in de [voorbeeldtoepassingen](#sample-applications) sectie verwijzen naar deze waarde als de client-ID.

     ![client-id](./media/azure-stack-create-service-principal/image12.png)
3. Voor het genereren van een verificatiesleutel voor een Web-app / API, selecteer **instellingen** > **sleutels**. 

4. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde in Kladblok of een andere tijdelijke locatie, omdat u de sleutel later niet ophalen. U geeft de sleutelwaarde samen met de toepassings-ID aan te melden als de toepassing. De waarde van de Store op een locatie waar uw toepassing kunt ophalen.

![opgeslagen sleutel](./media/azure-stack-create-service-principal/image15.png)

Als u klaar bent, gaat u verder met [toewijzen van uw toepassing een rol](#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Service-principal maken voor AD FS
Als u Azure Stack met AD FS hebt geïmplementeerd, kunt u PowerShell gebruiken om te maken van een service-principal en zich aanmelden vanaf PowerShell die identiteit met een rol voor toegang toewijzen.

Het script wordt uitgevoerd vanaf het eindpunt van de bevoegde op een virtuele machine van ERCS.

Vereisten:
- Er is een certificaat vereist.

#### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:


|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|Naam|Naam van de SPN-account|Mijntoep|
|ClientCertificates|Matrix met objecten van certificaat|X509 certificaat|
|ClientRedirectUris<br>(Optioneel)|Toepassing omleidings-URI|-|

#### <a name="example"></a>Voorbeeld

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdrachten uit:

   > [!NOTE]
   > In dit voorbeeld wordt een zelfondertekend certificaat gemaakt. Wanneer u deze opdrachten in een productie-implementatie uitvoeren, gebruikt u [Get-certificaat](/powershell/module/pkiclient/get-certificate) om op te halen van het certificaatobject voor het certificaat dat u wilt gebruiken.

   ```PowerShell  
    # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
    $creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $creds

    # This produces a self signed cert for testing purposes. It is prefered to use a managed certificate for this.
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange

    $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name '<yourappname>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }
    $session|remove-pssession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
    $TenantID = $AzureStackInfo.AADTenantID

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

    # Set the GraphEndpointResourceId value
    Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

    Add-AzureRmAccount -EnvironmentName "azurestackuser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID

    # Output the SPN details
    $ServicePrincipal

   ```

2. Nadat de automatisering is voltooid, wordt de vereiste gegevens op het gebruik van de SPN-naam weergegeven. 

   Bijvoorbeeld:

   ```shell
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="assign-a-role"></a>Een rol toewijzen
Zodra de Service-Principal is gemaakt, moet u [toewijzen aan een rol](#assign-role-to-service-principal).

### <a name="sign-in-through-powershell"></a>Meld u aan via PowerShell
Wanneer u een rol hebt toegewezen, kunt u zich aanmeldt bij Azure Stack met behulp van de service-principal met de volgende opdracht:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ClientId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Rol toewijzen aan service-principal
Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol staat voor de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het vindt de resourcegroep en alle resources die deze bevat.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing in de Azure Stack-portal. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

2. Selecteer het specifieke abonnement (resourcegroep of resource) om de toepassing te wijzen aan.

     ![abonnement voor toewijzing selecteren](./media/azure-stack-create-service-principal/image16.png)

3. Selecteer **toegangsbeheer (IAM)**.

     ![Selecteer toegang](./media/azure-stack-create-service-principal/image17.png)

4. Selecteer **Toevoegen**.

5. Selecteer de rol die u wilt toewijzen aan de toepassing.

6. Uw toepassing zoekt en selecteert u deze.

7. Selecteer **OK** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Nu dat u hebt gemaakt van een service-principal en een rol is toegewezen, kunt u beginnen met dit voor toegang tot Azure Stack-resources in uw toepassing.  

## <a name="next-steps"></a>Volgende stappen

[Gebruikers toevoegen voor AD FS](azure-stack-add-users-adfs.md)
[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
