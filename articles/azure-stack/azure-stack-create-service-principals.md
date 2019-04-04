---
title: Beheren van een Service-Principal voor Azure Stack | Microsoft Docs
description: Beschrijft hoe u een nieuwe service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot resources beheren.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: sethm
ms.lastreviewed: 12/18/2018
ms.openlocfilehash: 0f5a4dc76830740d69547a01ce40b5e10cf4a74b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499405"
---
# <a name="provide-applications-access-to-azure-stack"></a>Toepassingen toegang geven tot Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Wanneer een toepassing toegang hebben moet tot het implementeren of via Azure Resource Manager-resources configureren in Azure Stack, maakt u een service-principal, dit een referentie voor uw toepassing is. Vervolgens kunt u alleen de benodigde machtigingen om deze service-principal te delegeren.  

Een voorbeeld: als u mogelijk een configuratiebeheerprogramma die gebruikmaakt van Azure Resource Manager het inventariseren van Azure-resources. In dit scenario kunt u een service-principal maken, de rol van lezer verlenen aan deze service-principal en het hulpprogramma configuration management naar alleen-lezen toegang beperken. 

Service-principals zijn het geschiktst is voor het uitvoeren van de app met uw eigen referenties omdat:

 - U kunt machtigingen toewijzen aan de service-principal die anders is dan de accountmachtigingen van uw eigen zijn. Deze machtigingen worden meestal beperkt tot de functies die met de app moeten kunnen worden uitgevoerd.
 - U hoeft niet te wijzigen van de referenties van de app als uw verantwoordelijkheden wijzigt.
 - U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een script zonder toezicht.  

## <a name="getting-started"></a>Aan de slag

Afhankelijk van hoe u Azure Stack hebt geïmplementeerd, begint u met een service-principal maken. Dit document beschrijft het maken van een service principal voor:

- Azure Active Directory (Azure AD). Azure AD is een multitenant, cloudgebaseerde directory, en identity management-service. U kunt Azure AD gebruiken met een gekoppelde Azure-Stack.
- Active Directory Federation Services (AD FS). AD FS biedt vereenvoudigde, beveiligde identiteitsfederatie en mogelijkheden voor eenmalige aanmelding (SSO) Web. U kunt AD FS gebruiken met zowel verbonden als niet-verbonden Azure Stack-exemplaren.

Als u de service-principal hebt gemaakt, wordt een reeks stappen die gemeenschappelijk zijn voor zowel AD FS en Azure Active Directory worden gebruikt voor het overdragen van machtigingen aan de rol.

## <a name="manage-service-principal-for-azure-ad"></a>Service-principal voor Azure AD beheren

Als u Azure Stack met Azure Active Directory (Azure AD) als uw identity management-service hebt geïmplementeerd, kunt u service-principals net zoals u zou voor Azure doen maken. Deze sectie leest u hoe u de stappen via de portal uitvoert. Controleer of u hebt de [vereist Azure AD-machtigingen](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) voordat u begint.

### <a name="create-service-principal"></a>Een service-principal maken

In deze sectie maakt u een toepassing (service-principal) in Azure AD die staat voor uw toepassing.

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **App-registraties** > **nieuwe toepassing registreren**
3. Geef een naam en URL op voor de toepassing. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

U kunt een service-principal voor uw toepassing hebt gemaakt.

### <a name="get-credentials"></a>Referenties ophalen

Wanneer u zich programmatisch aanmeldt, u de ID gebruiken voor uw toepassing, en voor een Web-app / API, een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Van **App-registraties** in Active Directory, selecteer uw toepassing.

2. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. De toepassingen in de sectie Voorbeeld toepassingen verwijzen naar deze waarde als de client-ID.

     ![Client-id](./media/azure-stack-create-service-principal/image12.png)
3. Voor het genereren van een verificatiesleutel voor een Web-app / API, selecteer **instellingen** > **sleutels**. 

4. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde in Kladblok of een andere tijdelijke locatie, omdat u de sleutel later niet ophalen. U geeft de sleutelwaarde samen met de toepassings-ID aan te melden als de toepassing. De waarde van de Store op een locatie waar uw toepassing kunt ophalen.

![opgeslagen sleutel](./media/azure-stack-create-service-principal/image15.png)

Als u klaar bent, kunt u uw toepassing een rol toewijzen.

## <a name="manage-service-principal-for-ad-fs"></a>Service-principal voor AD FS beheren

Als u Azure Stack met Active Directory Federation Services (AD FS) als uw identity management-service hebt geïmplementeerd, moet u PowerShell gebruiken voor het maken van een service-principal en meld u aan dat de identiteit met een rol voor toegang toewijzen.

U kunt een van twee methoden aan uw service-principal maken met AD FS. U kunt:
 - [Een service-principal met behulp van een certificaat maken](azure-stack-create-service-principals.md#create-a-service-principal-using-a-certificate)
 - [Een service-principal met behulp van een clientgeheim maken](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret)

Taken voor het beheren van AD FS service-principals.

| Type | Bewerking |
| --- | --- |
| AD FS-certificaat | [Maken](azure-stack-create-service-principals.md#create-a-service-principal-using-a-certificate) |
| AD FS-certificaat | [Update](azure-stack-create-service-principals.md#update-certificate-for-service-principal-for-ad-fs) |
| AD FS-certificaat | [Remove](azure-stack-create-service-principals.md#remove-a-service-principal-for-ad-fs) |
| AD FS-Client-geheim | [Maken](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret) |
| AD FS-Client-geheim | [Update](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret) |
| AD FS-Client-geheim | [Remove](azure-stack-create-service-principals.md#remove-a-service-principal-for-ad-fs) |

### <a name="create-a-service-principal-using-a-certificate"></a>Een service-principal met behulp van een certificaat maken

Bij het maken van een service-principal tijdens het gebruik van AD FS voor identiteit, kunt u een certificaat.

#### <a name="certificate"></a>Certificaat

Er is een certificaat vereist.

**Vereisten voor certificaten**

 - De cryptografieprovider (CSP) moet verouderde sleutel-provider.
 - Opmaak van het certificaat moet zich in PFX-bestand, zoals de openbare en persoonlijke sleutels vereist zijn. Windows-servers gebruiken pfx-bestanden met het openbare sleutelbestand (SSL-certificaatbestand) en de bijbehorende persoonlijke sleutelbestand.
 - Voor de productie, moet het certificaat van een interne certificeringsinstantie of een openbare certificeringsinstantie worden uitgegeven. Als u een openbare certificeringsinstantie gebruikt, moet u de instantie opgenomen in het operationele systeem basisinstallatiekopie als onderdeel van het Microsoft-programma vertrouwde basis-CA. U vindt de volledige lijst met [Microsoft Trusted Root Certificate Program: Deelnemers](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
 - Uw Azure Stack-infrastructuur moet toegang tot het netwerk hebben tot de locatie van de certificeringsinstantie certificaat certificaatintrekkingslijst (CRL) gepubliceerd in het certificaat. Dit CRL moet een HTTP-eindpunt.

#### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:

|Parameter|Description|Voorbeeld|
|---------|---------|---------|
|Name|Naam van de SPN-account|MyAPP|
|ClientCertificates|Matrix met objecten van certificaat|X509 certificaat|
|ClientRedirectUris<br>(Optioneel)|Toepassing omleidings-URI|-|

#### <a name="use-powershell-to-create-a-service-principal"></a>PowerShell gebruiken voor het maken van een service-principal

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende cmdlets:

   ```powershell  
    # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
    $Creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

    # If you have a managed certificate use the Get-Item command to retrieve your certificate from your certificate location.
    # If you don't want to use a managed certificate, you can produce a self signed cert for testing purposes: 
    # $Cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange
    $Cert = Get-Item "<YourCertificateLocation>"
    
    $ServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name '<YourAppName>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
    $Session | Remove-PSSession

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

    Add-AzureRmAccount -EnvironmentName "AzureStackUser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID

    # Output the SPN details
    $ServicePrincipal

   ```
   > [!Note]  
   > Voor validatie gebruikt een zelfondertekend certificaat kan worden gemaakt met het onderstaande voorbeeld:

   ```powershell  
   $Cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange
   ```


2. Nadat de automatisering is voltooid, wordt de vereiste gegevens op het gebruik van de SPN-naam weergegeven. Het verdient aanbeveling voor het opslaan van de uitvoer voor later gebruik.

   Bijvoorbeeld:

   ```shell
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="update-certificate-for-service-principal-for-ad-fs"></a>Certificaat voor service-principal bijwerken voor AD FS

Als u Azure Stack met AD FS hebt geïmplementeerd, kunt u PowerShell gebruiken om bij te werken van het geheim voor een service-principal.

Het script wordt uitgevoerd vanaf het eindpunt van de bevoegde op een virtuele machine van ERCS.

#### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:

|Parameter|Description|Voorbeeld|
|---------|---------|---------|
|Name|Naam van de SPN-account|MyAPP|
|ApplicationIdentifier|Unieke id|S-1-5-21-1634563105-1224503876-2692824315-2119|
|ClientCertificate|Matrix met objecten van certificaat|X509 certificaat|

#### <a name="example-of-updating-service-principal-for-ad-fs"></a>Voorbeeld van het bijwerken van service-principal voor AD FS

Het voorbeeld wordt een zelfondertekend certificaat gemaakt. Wanneer u de cmdlets in een productie-implementatie uitvoert, gebruikt u [Get-Item](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Management/Get-Item) om op te halen van het certificaatobject voor het certificaat dat u wilt gebruiken.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende cmdlets:

     ```powershell
          # Creating a PSSession to the ERCS PrivilegedEndpoint
          $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

          # This produces a self signed cert for testing purposes. It is preferred to use a managed certificate for this.
          $NewCert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange

          $RemoveServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier  S-1-5-21-1634563105-1224503876-2692824315-2120 -ClientCertificates $NewCert}

          $Session | Remove-PSSession
     ```

2. Nadat de automatisering is voltooid, wordt de bijgewerkte vingerafdrukwaarde is vereist voor verificatie van de SPN-naam weergegeven.

     ```Shell  
          ClientId              : 
          Thumbprint            : AF22EE716909041055A01FE6C6F5C5CDE78948E9
          ApplicationName       : Azurestack-ThomasAPP-3e5dc4d2-d286-481c-89ba-57aa290a4818
          ClientSecret          : 
          RunspaceId            : a580f894-8f9b-40ee-aa10-77d4d142b4e5
     ```

### <a name="create-a-service-principal-using-a-client-secret"></a>Een service-principal met behulp van een clientgeheim maken

Bij het maken van een service-principal tijdens het gebruik van AD FS voor identiteit, kunt u een certificaat. U gebruikt de bevoegde eindpunt om uit te voeren van de cmdlets.

Deze scripts worden uitgevoerd vanaf het eindpunt van de bevoegdheden op een ERCS virtuele machine. Zie voor meer informatie over het bevoegde eindpunt [met behulp van het eindpunt van de bevoegde in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).

#### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:

| Parameter | Description | Voorbeeld |
|----------------------|--------------------------|---------|
| Name | Naam van de SPN-account | MyAPP |
| GenerateClientSecret | Geheim maken |  |

#### <a name="use-the-ercs-privilegedendpoint-to-create-the-service-principal"></a>De PrivilegedEndpoint ERCS gebruiken voor het maken van de service-principal

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende cmdlets:

     ```powershell  
      # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
     $Creds = Get-Credential

     # Creating a PSSession to the ERCS PrivilegedEndpoint
     $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Creating a SPN with a secre
     $ServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name '<YourAppName>' -GenerateClientSecret}
     $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
     $Session | Remove-PSSession

     # Output the SPN details
     $ServicePrincipal
     ```

2. Na het uitvoeren van cmdlets wordt de vereiste gegevens op het gebruik van de SPN-naam weergegeven in de shell. Zorg ervoor dat u het clientgeheim opslaan.

     ```powershell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : 6RUZLRoBw3EebMDgaWGiowCkoko5_j_ujIPjA8dS
     PSComputerName        : 192.168.200.224
     RunspaceId            : 286daaa1-c9a6-4176-a1a8-03f543f90998
     ```

#### <a name="update-client-secret-for-a-service-principal-for-ad-fs"></a>Clientgeheim voor een service-principal bijwerken voor AD FS

Een nieuwe clientgeheim is automatisch gegenereerd door de PowerShell-cmdlet.

Het script wordt uitgevoerd vanaf het eindpunt van de bevoegde op een virtuele machine van ERCS.

##### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:

| Parameter | Description | Voorbeeld |
|-----------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------|
| ApplicationIdentifier | De unieke id. | S-1-5-21-1634563105-1224503876-2692824315-2119 |
| ChangeClientSecret | Hiermee wijzigt u het clientgeheim met een punt rollover van 2880 minuten waarin het oude geheim nog geldig is. |  |
| ResetClientSecret | Wijzig het clientgeheim direct |  |

##### <a name="example-of-updating-a-client-secret-for-ad-fs"></a>Voorbeeld van het bijwerken van een clientgeheim voor AD FS

Het voorbeeld wordt de **ResetClientSecret** parameter, die het clientgeheim onmiddellijk wordt gewijzigd.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende cmdlets:

     ```powershell  
          # Creating a PSSession to the ERCS PrivilegedEndpoint
          $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

          # This produces a self signed cert for testing purposes. It is preferred to use a managed certificate for this.
          $NewCert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange

          $UpdateServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier  S-1-5-21-1634563105-1224503876-2692824315-2120 -ResetClientSecret}

          $Session | Remove-PSSession
     ```

2. Nadat de automatisering is voltooid, wordt het zojuist gegenereerde geheim dat is vereist voor verificatie van de SPN-naam. Zorg ervoor dat u het nieuwe clientgeheim opslaan.

     ```powershell  
          ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2120
          ClientId              :  
          Thumbprint            : 
          ApplicationName       : Azurestack-Yourapp-6967581b-497e-4f5a-87b5-0c8d01a9f146
          ClientSecret          : MKUNzeL6PwmlhWdHB59c25WDDZlJ1A6IWzwgv_Kn
          RunspaceId            : 6ed9f903-f1be-44e3-9fef-e7e0e3f48564
     ```

### <a name="remove-a-service-principal-for-ad-fs"></a>Verwijderen van een service-principal voor AD FS

Als u Azure Stack met AD FS hebt geïmplementeerd, kunt u PowerShell gebruiken om te verwijderen van een service-principal.

Het script wordt uitgevoerd vanaf het eindpunt van de bevoegde op een virtuele machine van ERCS.

#### <a name="parameters"></a>Parameters

De volgende informatie is vereist als invoer voor de automation-parameters:

|Parameter|Description|Voorbeeld|
|---------|---------|---------|
| Parameter | Beschrijving | Voorbeeld |
| ApplicationIdentifier | Unieke id | S-1-5-21-1634563105-1224503876-2692824315-2119 |

> [!Note]  
> Als u wilt weergeven in een lijst van alle bestaande service-principals en hun toepassings-id, kan de opdracht get-graphapplication worden gebruikt.

#### <a name="example-of-removing-the-service-principal-for-ad-fs"></a>Voorbeeld van het verwijderen van de service-principal voor AD FS

```powershell  
     Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
     $Creds = Get-Credential

     # Creating a PSSession to the ERCS PrivilegedEndpoint
     $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

     $UpdateServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Remove-GraphApplication -ApplicationIdentifier S-1-5-21-1634563105-1224503876-2692824315-2119}

     $Session | Remove-PSSession
```

## <a name="assign-a-role"></a>Een rol toewijzen

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol staat voor de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het vindt de resourcegroep en alle resources die deze bevat.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing in de Azure Stack-portal. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

2. Selecteer het specifieke abonnement (resourcegroep of resource) om de toepassing te wijzen aan.

     ![abonnement voor toewijzing selecteren](./media/azure-stack-create-service-principal/image16.png)

3. Selecteer **toegangsbeheer (IAM)**.

     ![Selecteer toegang](./media/azure-stack-create-service-principal/image17.png)

4. Selecteer **roltoewijzing toevoegen**.

5. Selecteer de rol die u wilt toewijzen aan de toepassing.

6. Uw toepassing zoekt en selecteert u deze.

7. Selecteer **OK** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Nu dat u hebt gemaakt van een service-principal en een rol is toegewezen, kunt u beginnen met dit voor toegang tot Azure Stack-resources in uw toepassing.  

## <a name="next-steps"></a>Volgende stappen

[Gebruikers toevoegen voor AD FS](azure-stack-add-users-adfs.md)  
[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)  
[Documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory)  
[Active Directory Federation Services](https://docs.microsoft.com/windows-server/identity/active-directory-federation-services)
