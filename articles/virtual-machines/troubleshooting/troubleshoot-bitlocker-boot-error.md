---
title: Het oplossen van fouten van BitLocker opstarten op een Azure-VM | Microsoft Docs
description: Informatie over het oplossen van BitLocker-opstartfouten in een Azure VM
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 13a45593509deb0cb9578c5ea6ed83aab3e008a4
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216557"
---
# <a name="bitlocker-boot-errors-on-an-azure-vm"></a>Opstartfouten BitLocker op een Azure VM

 Dit artikel beschrijft de BitLocker-fouten die optreden kunnen bij het starten van een Windows virtuele machine (VM) in Microsoft Azure.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="symptom"></a>Symptoom

 Een Windows-VM niet wordt gestart. Wanneer u de schermafbeeldingen controleren in de [diagnostische gegevens over opstarten](../windows/boot-diagnostics.md) venster ziet u een van de volgende foutberichten:

- Sluit het USB-stuurprogramma dat beschikt over de BitLocker-sleutel

- U bent vergrendeld! Geef de herstelsleutel opnieuw aan de slag (indeling: Verenigde Staten) de verkeerde aanmelden informatie is te vaak ingevoerd, zodat uw PC is vergrendeld om uw privacy te beschermen. Als u wilt ophalen van de herstelsleutel, gaat u naar http://windows.microsoft.com/recoverykeyfaq van een andere PC of mobiel apparaat. Als u deze nodig hebt, de sleutel-ID is XXXXXXX. Of u kunt de PC opnieuw instellen.

- Voer het wachtwoord voor het ontgrendelen van deze schijf [] Druk op de sleutel invoegen om te zien van het wachtwoord, terwijl u typt.
- Voer uw herstelsleutel de herstelsleutel laden vanaf een USB-apparaat.

## <a name="cause"></a>Oorzaak

Dit probleem kan optreden als de virtuele machine het BitLocker-herstel sleutel (BEK)-bestand voor het ontsleutelen van de versleutelde schijf niet vinden.

## <a name="solution"></a>Oplossing

U lost dit probleem, stoppen en wijs de virtuele machine en vervolgens opnieuw te starten. Met deze bewerking zorgt ervoor dat de virtuele machine het bestand BEK ophalen uit de Azure Key Vault en klik vervolgens op de versleutelde schijf plaatsen. 

Als deze methode niet de los het probleem is, volg deze stappen voor het herstellen van het bestand BEK handmatig:

1. Een momentopname van de schijf van de betrokken virtuele machine als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).
2. [De schijf koppelen aan een virtuele machine voor herstel](troubleshoot-recovery-disks-portal-windows.md) die is versleuteld met BitLocker. Dit is vereist om uit te voeren de [beheren bde](https://docs.microsoft.com/windows-server/administration/windows-commands/manage-bde) opdracht die is alleen beschikbaar op de VM BitLocker is versleuteld.

    Wanneer u een beheerde schijf koppelt, ontvangt u wellicht een foutmelding 'bevat instellingen voor codering en kan daarom niet worden gebruikt als een gegevensschijf'. In dit geval, voer het volgende script opnieuw te proberen om de schijf te koppelen:

    ```Powershell
    $rgName = "myResourceGroup"
    $osDiskName = "ProblemOsDisk"

    New-AzDiskUpdateConfig -EncryptionSettingsEnabled $false |Update-AzDisk -diskName $osDiskName -ResourceGroupName $rgName

    $recoveryVMName = "myRecoveryVM" 
    $recoveryVMRG = "RecoveryVMRG" 
    $OSDisk = Get-AzDisk -ResourceGroupName $rgName -DiskName $osDiskName;

    $vm = get-AzVM -ResourceGroupName $recoveryVMRG -Name $recoveryVMName 

    Add-AzVMDataDisk -VM $vm -Name $osDiskName -ManagedDiskId $osDisk.Id -Caching None -Lun 3 -CreateOption Attach 

    Update-AzVM -VM $vm -ResourceGroupName $recoveryVMRG
    ```
     U kunt een beheerde schijf niet koppelen aan een virtuele machine die is hersteld van een blob-installatiekopie.

3. Nadat de schijf is gekoppeld, moet u een extern bureaublad verbinding met de virtuele machine voor herstel zodat u enkele Azure PowerShell-scripts kunt uitvoeren. Zorg ervoor dat u hebt de [meest recente versie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) geïnstalleerd op de virtuele machine voor herstel.

4. Open een verhoogde Azure PowerShell-sessie (als administrator uitvoeren). Voer de volgende opdrachten om aan te melden bij Azure-abonnement:

    ```Powershell
    Add-AzAccount -SubscriptionID [SubscriptionID]
    ```

5. Voer het volgende script om te controleren of de naam van de BEK-bestand:

    ```powershell
    $vmName = "myVM"
    $vault = "myKeyVault"
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.ContentType -match 'BEK')} `
            | Sort-Object -Property Created `
            | ft  Created, `
                @{Label="Content Type";Expression={$_.ContentType}}, `
                @{Label ="Volume"; Expression = {$_.Tags.VolumeLetter}}, `
                @{Label ="DiskEncryptionKeyFileName"; Expression = {$_.Tags.DiskEncryptionKeyFileName}}
    ```

    Hier volgt een voorbeeld van de uitvoer. Ga naar de BEK bestandsnaam op voor de gekoppelde schijf. In dit geval veronderstellen we dat de stationsletter van de gekoppelde schijf F, en het bestand BEK EF7B2F5A - 50C 6-4637-9F13-7F599C12F85C. BEK.

    ```
    Created             Content Type Volume DiskEncryptionKeyFileName               
    -------             ------------ ------ -------------------------               
    4/5/2018 7:14:59 PM Wrapped BEK  C:\    B4B3E070-836C-4AF5-AC5B-66F6FDE6A971.BEK
    4/7/2018 7:21:16 PM Wrapped BEK  F:\    EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    4/7/2018 7:26:23 PM Wrapped BEK  G:\    70148178-6FAE-41EC-A05B-3431E6252539.BEK
    4/7/2018 7:26:26 PM Wrapped BEK  H:\    5745719F-4886-4940-9B51-C98AFABE5305.BEK
    ```

    Als u twee dubbele volumes ziet, is het volume dat het nieuwere tijdstempel heeft de huidige BEK-bestand dat wordt gebruikt door de virtuele machine voor herstel.

    Als de **inhoudstype** waarde **verpakt BEK**, gaat u naar de [sleutel versleuteling sleutel (KEK) scenario's](#key-encryption-key-scenario).

    Nu dat u de naam van het bestand BEK voor het station hebt, moet u de naam voor het bestand van een geheim maken. BEK-bestand voor het ontgrendelen van het station. 

6.  De BEK-bestand downloaden naar de recovery-schijf. Het volgende voorbeeld wordt de BEK-bestand naar de map C:\BEK opgeslagen. Zorg ervoor dat de `C:\BEK\` pad bestaat voordat u de scripts uitvoert.

    ```powershell
    $vault = "myKeyVault"
    $bek = " EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK"
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $bek
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = "C:\BEK\DiskEncryptionKeyFileName.BEK"
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7.  Als u wilt de gekoppelde schijf met behulp van de BEK-bestand hebt ontgrendeld, kunt u de volgende opdracht uitvoeren:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    In dit voorbeeld is de gekoppelde besturingssysteemschijf station f kan zijn. Zorg ervoor dat u de juiste stationsletter. 

    - Als de schijf is ontgrendeld met behulp van de BEK-sleutel. Wij kunnen beschouwen als de BItLocker-probleem worden opgelost. 

    - Als met de BEK-sleutel, de schijf niet ontgrendelen wordt, kunt u beveiliging tijdelijk BitLocker om uit te schakelen door het uitvoeren van de volgende opdracht onderbreken
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Als u opnieuw opbouwen van de virtuele machine wilt met behulp van de schijf dytem, moet u het station volledig decoderen. Voer hiervoor de volgende opdracht uit:

        ```powershell
        manage-bde -off F:
        ```
8.  De schijf loskoppelen van de virtuele machine voor herstel, en voeg vervolgens de schijf met de betreffende virtuele machine als een schijf opnieuw. Zie voor meer informatie, [een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine voor herstel](troubleshoot-recovery-disks-windows.md).

### <a name="key-encryption-key-scenario"></a>Coderingssleutel Key-scenario

Volg deze stappen voor een scenario Key-versleutelingssleutel:

1. Zorg ervoor dat het aangemelde gebruikersaccount dat is vereist voor de machtiging 'niet-ingepakte' in het toegangsbeleid van Key Vault in de **gebruiker | Sleutelmachtigingen | Cryptografische bewerkingen | Sleutel uitpakken**.
2. Opslaan van de volgende scripts in een. Ps1-bestand:

    ```powershell
    #Set the Parameters for the script
    param (
            [Parameter(Mandatory=$true)]
            [string] 
            $keyVaultName,
            [Parameter(Mandatory=$true)]
            [string] 
            $kekName,
            [Parameter(Mandatory=$true)]
            [string]
            $secretName,
            [Parameter(Mandatory=$true)]
            [string]
            $bekFilePath,
            [Parameter(Mandatory=$true)]
            [string] 
            $adTenant
            )
    # Load ADAL Assemblies
    $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    [System.Reflection.Assembly]::LoadFrom($adal)
    [System.Reflection.Assembly]::LoadFrom($adalforms)

    # Set well-known client ID for AzurePowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2" 
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURI = "https://vault.azure.net"
    # Set Authority to Azure AD Tenant
    $authority = "https://login.windows.net/$adtenant"
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $authResult = $authContext.AcquireToken($resourceAppIdURI, $clientId, $redirectUri, "Auto")
    # Generate auth header 
    $authHeader = $authResult.CreateAuthorizationHeader()
    # Set HTTP request headers to include Authorization header
    $headers = @{'x-ms-version'='2014-08-01';"Authorization" = $authHeader}

    ########################################################################################################################
    # 1. Retrieve wrapped BEK
    # 2. Make KeyVault REST API call to unwrap the BEK
    # 3. Convert the Base64Url string returned by KeyVault unwrap to Base64 string 
    # 4. Convert Base64 string to bytes and write to the BEK file
    ########################################################################################################################

    #Get wrapped BEK and place it in JSON object to send to KeyVault REST API
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $secretName
    $wrappedBekSecretBase64 = $keyVaultSecret.SecretValueText
    $jsonObject = @"
    {
    "alg": "RSA-OAEP",
    "value" : "$wrappedBekSecretBase64"
    }
    "@

    #Get KEK Url
    $kekUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name $kekName).Key.Kid;
    $unwrapKeyRequestUrl = $kekUrl+ "/unwrapkey?api-version=2015-06-01";

    #Call KeyVault REST API to Unwrap 
    $result = Invoke-RestMethod -Method POST -Uri $unwrapKeyRequestUrl -Headers $headers -Body $jsonObject -ContentType "application/json" -Debug

    #Convert Base64Url string returned by KeyVault unwrap to Base64 string
    $base64UrlBek = $result.value;
    $base64Bek = $base64UrlBek.Replace('-', '+');
    $base64Bek = $base64Bek.Replace('_', '/');
    if($base64Bek.Length %4 -eq 2)
    {
        $base64Bek+= '==';
    }
    elseif($base64Bek.Length %4 -eq 3)
    {
        $base64Bek+= '=';
    }

    #Convert base64 string to bytes and write to BEK file
    $bekFileBytes = [System.Convert]::FromBase64String($base64Bek);
    [System.IO.File]::WriteAllBytes($bekFilePath,$bekFileBytes)
    ```
3. De parameters instellen. Het script het geheim KEK-sleutel voor het maken van de sleutel BEK verwerken, en sla het op een lokale map op de virtuele machine voor herstel.

4. Wanneer wordt begonnen met het script ziet u de volgende uitvoer:

        GAC    Version        Location                                                                              
        ---    -------        --------                                                                              
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\AzureRM.profile\4.0.0\Microsoft.Identity...
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\AzureRM.profile\4.0.0\Microsoft.Identity...

    Als het script is voltooid, ziet u de volgende uitvoer:

        VERBOSE: POST https://myvault.vault.azure.net/keys/rondomkey/<KEY-ID>/unwrapkey?api-
        version=2015-06-01 with -1-byte payload
        VERBOSE: received 360-byte response of content type application/json; charset=utf-8


5. Als u wilt de gekoppelde schijf met behulp van de BEK-bestand hebt ontgrendeld, kunt u de volgende opdracht uitvoeren:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    In dit voorbeeld is de gekoppelde besturingssysteemschijf station f kan zijn. Zorg ervoor dat u de juiste stationsletter. 

    - Als de schijf is ontgrendeld met behulp van de BEK-sleutel. Wij kunnen beschouwen als de BItLocker-probleem worden opgelost. 

    - Als met de BEK-sleutel, de schijf niet ontgrendelen wordt, kunt u beveiliging tijdelijk BitLocker om uit te schakelen door het uitvoeren van de volgende opdracht onderbreken
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Als u opnieuw opbouwen van de virtuele machine wilt met behulp van de schijf dytem, moet u het station volledig decoderen. Voer hiervoor de volgende opdracht uit:

        ```powershell
        manage-bde -off F:
        ```

6. De schijf loskoppelen van de virtuele machine voor herstel, en voeg vervolgens de schijf met de betreffende virtuele machine als een schijf opnieuw. Zie voor meer informatie, [een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine voor herstel](troubleshoot-recovery-disks-windows.md).
