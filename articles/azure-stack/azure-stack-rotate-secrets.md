---
title: Geheimen in Azure Stack draaien | Microsoft Docs
description: Informatie over het draaien van uw geheimen in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dacfa738a99eb2d580d825957d09b2b1a3111e93
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051388"
---
# <a name="rotate-secrets-in-azure-stack"></a>Geheimen in Azure Stack draaien

*Deze instructies gelden alleen voor Azure Stack geïntegreerde systemen versie 1803 en Later. Probeer niet geheime rotatie in de pre-1802 Azure Stack-versies*

Azure Stack gebruikt verschillende geheimen voor het onderhouden van veilige communicatie tussen de resources voor Azure Stack-infrastructuur en services.

- **Interne geheimen**  
Alle certificaten, wachtwoorden, beveiligde tekenreeksen, en sleutels die worden gebruikt door de Azure Stack-infrastructuur zonder tussenkomst van de Azure Stack-Operator. 

- **Externe geheimen**  
Infrastructuur voor service-certificaten voor extern gerichte-services die worden geleverd door de Azure Stack-Operator. Dit omvat de certificaten voor de volgende services: 
    - Beheerdersportal 
    - Openbare-Portal 
    - Beheerder van Azure resourcemanager 
    - Globale Azure Resource Manager 
    - Beheerder Key Vault 
    - KeyVault 
    - ACS (met inbegrip van de blob, table en queue storage) 
    - ADFS<sup>*</sup>
    - Grafiek<sup>*</sup>

   <sup>*</sup> Alleen van toepassing als id-provider van de omgeving Active Directory Federated Services (AD FS is).

> [!NOTE]
> Alle andere beveiligde sleutels en tekenreeksen, met inbegrip van de BMC en schakelt u over wachtwoorden, gebruikers en beheerders wachtwoorden worden nog steeds handmatig bijgewerkt door de beheerder. 

Operators moeten de integriteit van de Azure Stack-infrastructuur te behouden, de mogelijkheid om te roteren periodiek de geheimen van de infrastructuur bij frequenties die consistent met de beveiligingsvereisten van hun organisatie zijn.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Geheimen met externe certificaten van een nieuwe certificeringsinstantie draaien

Azure Stack ondersteunt geheime rotatie van externe certificaten van een nieuwe certificeringsinstantie (CA) in de volgende situaties:

|Geïnstalleerde certificaat CA|Certificeringsinstantie voor het draaien|Ondersteund|Ondersteunde versies van Azure Stack|
|-----|-----|-----|-----|
|Van zelf-ondertekend|Naar de onderneming|Niet ondersteund||
|Van zelf-ondertekend|Voor zelf-ondertekend|Niet ondersteund||
|Van zelf-ondertekend|Voor publiek<sup>*</sup>|Ondersteund|1803 en hoger|
|Van onderneming|Naar de onderneming|Als klanten de dezelfde ondernemings-CA gebruiken zoals gebruikt bij de implementatie wordt ondersteund|1803 en hoger|
|Van onderneming|Voor zelf-ondertekend|Niet ondersteund||
|Van onderneming|Voor publiek<sup>*</sup>|Ondersteund|1803 en hoger|
|Van openbare<sup>*</sup>|Naar de onderneming|Niet ondersteund|1803 en hoger|
|Van openbare<sup>*</sup>|Voor zelf-ondertekend|Niet ondersteund||
|Van openbare<sup>*</sup>|Voor publiek<sup>*</sup>|Ondersteund|1803 en hoger|

<sup>*</sup> Dit zijn openbare certificeringsinstanties die deel van het Windows Trusted Root-programma uitmaken. U vindt de volledige lijst [Microsoft Trusted Root Certificate Program: deelnemers in (vanaf 27 juni 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Herstel van waarschuwing

Wanneer de geheimen zijn binnen 30 dagen na verlopen, worden de volgende waarschuwingen gegenereerd in de Beheerdersportal: 

- Wachtende service-account wachtwoord verloopt 
- In behandeling interne certificaat verloopt 
- In behandeling extern certificaat verlopen 

Geheime rotatie met behulp van de onderstaande instructies uitgevoerd, wordt deze waarschuwingen oplossen.

## <a name="pre-steps-for-secret-rotation"></a>Stappen voor geheime rotatie vóór

   > [!IMPORTANT]  
   > Zorg ervoor dat de geheime rotatie is nog niet is uitgevoerd op uw omgeving. Als de geheime rotatie is al uitgevoerd, moet u Azure Stack bijwerken naar versie 1807 of hoger voordat u geheime rotatie uitvoert. 
1.  Informeer de gebruikers van elke onderhoudsbewerkingen. Normale onderhoudsvensters, zo veel mogelijk tijdens de kantooruren plannen. Onderhoudsbewerkingen mogelijk van invloed op zowel de werkbelastingen van de gebruiker en de portal bewerkingen.
    > [!note]  
    > De volgende stappen zijn alleen van toepassing wanneer u externe geheimen voor Azure Stack.
3. Bereid een nieuwe set vervanging externe certificaten. De nieuwe set komt overeen met de certificaat-specificaties die worden beschreven in de [Azure Stack PKI-certificaatvereisten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
4.  Een back-up naar de certificaten gebruikt voor draaien op een veilige locatie van de back-Store. Als uw rotatie wordt uitgevoerd en mislukt, vervangt u de certificaten in de bestandsshare met de back-ups voordat u de draaihoek van het opnieuw uitvoeren. Houd er rekening mee, back-ups behouden in de veilige back-uplocatie.
5.  Maak een bestandsshare die u vanaf de ERCS virtuele machines openen kunt. De bestandsshare moet leesbaar en beschrijfbaar zijn voor de **CloudAdmin** identiteit.
6.  Open een PowerShell ISE-console op een computer waar u toegang tot de bestandsshare hebt. Navigeer naar de bestandsshare. 
7.  Voer **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** om de vereiste mappen voor uw externe certificaten te maken.

## <a name="rotating-external-and-internal-secrets"></a>Externe en interne geheimen draaien

Externe een interne geheim draaien:

1. In het zojuist gemaakte **certificaten** directory hebt gemaakt in de stappen vóór de nieuwe set van externe certificaten vervangen in de mapstructuur op basis van de indeling die worden beschreven in de sectie verplichte certificaten plaatsen van de [Azure Stack PKI-certificaatvereisten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Maken van een PowerShell-sessie met de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) met behulp van de **CloudAdmin** account en de sessies opslaan als een variabele. U gebruikt deze variabele als de parameter in de volgende stap.

    > [!IMPORTANT]  
    > De sessie, slaat u de sessie als een variabele niet.
    
3. Voer  **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Doorgeven van uw variabele bevoegde eindpunt PowerShell-sessie als de **sessie** parameter. 
4. Voer **Start SecretRotation** met de volgende parameters:
    - **PfxFilesPath**  
    Geef het netwerkpad naar de map van uw certificaten eerder hebt gemaakt.  
    - **PathAccessCredential**  
    Een PSCredential-object om referenties voor de share. 
    - **CertificatePassword**  
    Een beveiligde tekenreeks van het wachtwoord gebruikt voor alle van de pfx-certificaatbestanden gemaakt.
4. Een ogenblik geduld terwijl uw geheimen draaien.  
Wanneer deze geheime rotatie is voltooid, de console weergegeven **algemene actiestatus: geslaagd**. 
5. Na voltooiing van de geheime rotatie, uw certificaten verwijderen uit de share in de vooraf stap hebt gemaakt en op te slaan in de veilige back-uplocatie. 

## <a name="walkthrough-of-secret-rotation"></a>Overzicht van het geheim draaien

De volgende PowerShell-voorbeeld ziet u de cmdlets en -parameters om uit te voeren om uw geheimen draaien.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -AsPlainText -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>Alleen interne geheimen draaien

Alleen interne geheimen met Azure-Stack draaien:

1. Maken van een PowerShell-sessie met de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Uitvoeren in de sessie bevoegde eindpunt **Start SecretRotation** zonder argumenten.

## <a name="start-secretrotation-reference"></a>Start-SecretRotation verwijzing

De geheimen van een Azure Stack-systeem draait. Alleen uitgevoerd voor de bevoegde eindpunt voor Azure Stack.

### <a name="syntax"></a>Syntaxis

Pad (standaard)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Beschrijving

De cmdlet Start-SecretRotation draait de geheimen van de infrastructuur van een Azure Stack-systeem. Standaard gedraaid alle geheimen blootgesteld aan de interne infrastructuur-netwerk met de invoer van de gebruiker ook gedraaid de certificaten van alle externe network-infrastructuur-eindpunten. Wanneer u de infrastructuur-eindpunten voor externe netwerken, moet Start SecretRotation via een Invoke-Command-scriptblok worden uitgevoerd met de Azure Stack-omgeving bevoegde eindpunt sessie ingevoerd als de sessieparameter.
 
### <a name="parameters"></a>Parameters

| Parameter | Type | Vereist | Positie | Standaard | Beschrijving |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Reeks  | False  | met de naam  | Geen  | Het fileshare-pad naar de **\Certificates** map met alle externe eindpunt certificaten het netwerk. Alleen vereist wanneer u externe geheimen of alle geheime gegevens. Einde map moet **\Certificates**. |
| CertificatePassword | SecureString | False  | met de naam  | Geen  | Het wachtwoord voor alle certificaten die zijn opgegeven in de - PfXFilesPath. Vereiste waarde als PfxFilesPath is opgegeven als zowel interne als externe geheimen worden gedraaid. |
| PathAccessCredential | PSCredential | False  | met de naam  | Geen  | De PowerShell-referentie voor de bestandsshare van de **\Certificates** map met alle externe eindpunt certificaten het netwerk. Alleen vereist wanneer u externe geheimen of alle geheime gegevens.  |
| Opnieuw uitvoeren | SwitchParameter | False  | met de naam  | Geen  | Opnieuw uitvoeren moet worden gebruikt op elk moment geheime rotatie opnieuw geprobeerd na een mislukte poging wordt. |

### <a name="examples"></a>Voorbeelden
 
**Alleen interne infrastructuur geheimen draaien**

```powershell  
PS C:\> Start-SecretRotation  
```

Met deze opdracht worden alle van de infrastructuur-geheimen blootgesteld aan interne netwerk van Azure Stack. Start-SecretRotation draait alle geheimen van de stack is gegenereerd, maar omdat er geen certificaten zijn opgegeven, niet extern eindpunt certificaten wordt gedraaid.  

**Interne en externe infrastructuur geheimen draaien**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Met deze opdracht worden alle van de infrastructuur-geheimen blootgesteld aan interne netwerk van Azure Stack, evenals de TLS-certificaten gebruikt voor het externe netwerk infrastructuur-eindpunten van Azure Stack. Start-SecretRotation draait alle geheimen stack gegenereerd en omdat er certificaten worden verleend, Extern eindpunt certificaten ook worden gedraaid.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>De baseboard management controller (BMC)-wachtwoord bijwerken

De BMC (baseboard management controller) wordt de fysieke status van uw servers. De specificaties en instructies over het bijwerken van het wachtwoord van de BMC variëren op basis van de leverancier van de oorspronkelijke leveranciers (OEM)-hardware. U moet uw wachtwoorden voor Azure Stack-onderdelen op een regelmatiger bijwerken.

1. Bijwerken van de BMC op fysieke servers van de Azure Stack door de OEM-instructies te volgen. Het wachtwoord voor elke BMC in uw omgeving, moet hetzelfde zijn.
2. Een bevoegde eindpunt geopend in Azure Stack-sessies. Zie voor instructies, [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).
3. Nadat uw PowerShell-prompt is gewijzigd in **[IP-adres of ERCS VM name]: PS >** of **[azs-ercs01]: PS >**, afhankelijk van de omgeving, voeren `Set-BmcPassword` door uit te voeren `invoke-command`. Uw sessievariabele bevoegde eindpunt als een parameter doorgeven. Bijvoorbeeld:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    U kunt ook de statische PowerShell-versie met de wachtwoorden gebruiken als coderegels:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure Stack-beveiliging](azure-stack-security-foundations.md)
