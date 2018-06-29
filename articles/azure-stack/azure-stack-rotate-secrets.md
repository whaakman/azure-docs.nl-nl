---
title: Geheimen in Azure-Stack draaien | Microsoft Docs
description: Ontdek hoe u uw geheimen in Azure-Stack draaien.
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
ms.date: 05/15/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 8ac151a70a81f78dab5ed1f30df51a1121a42cbd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029013"
---
# <a name="rotate-secrets-in-azure-stack"></a>Geheimen in Azure-Stack draaien

*Deze instructies gelden alleen voor Azure Stack geïntegreerde systemen versie 1803 en Later. Probeer geen geheime rotatie op pre-1802 Azure Stack-versies*

Verschillende geheimen Azure Stack gebruikt voor het onderhouden van veilige communicatie tussen de Azure-Stack-infrastructuur en -services.

- **Interne geheimen**  
Alle certificaten, wachtwoorden, beveiligde tekenreeksen, en sleutels die worden gebruikt door de Azure-Stack-infrastructuur zonder tussenkomst van de Azure-Stack-Operator. 

- **Externe geheimen**  
Certificaten van de infrastructuur-service voor extern gerichte services die worden geleverd door de Azure-Stack-Operator. Dit omvat de certificaten voor de volgende services: 
    - Beheerdersportal 
    - Openbare-Portal 
    - Beheerder Azure resourcemanager 
    - Globale Azure Resource Manager 
    - Beheerder Keyvault 
    - KeyVault 
    - ACS (inclusief blob, table en queue storage) 
    - ADFS<sup>*</sup>
    - Grafiek<sup>*</sup>

    > <sup>*</sup> Alleen toepasbaar als de omgeving identiteitsprovider Active Directory Federated Services (AD FS).

> [!NOTE]
> Alle andere beveiligde sleutels en tekenreeksen, met inbegrip van de BMC en overschakelen van wachtwoorden, wachtwoorden van gebruikers- en nog steeds handmatig worden bijgewerkt door de beheerder. 

Operators moeten om de integriteit van de Azure-Stack-infrastructuur te behouden, de mogelijkheid om terug te draaien regelmatig hun infrastructuur geheimen op frequenties die consistent met de beveiligingsvereisten van hun organisatie zijn.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Geheimen met externe certificaten van een nieuwe certificeringsinstantie draaien

Azure-Stack ondersteunt geheime wisselen met externe certificaten van een nieuwe certificeringsinstantie (CA) in de volgende situaties:

|Geïnstalleerde certificaat CA|CA om terug te draaien|Ondersteund|Azure Stack-versies die worden ondersteund|
|-----|-----|-----|-----|-----|
|Van zelf-ondertekend|Naar de onderneming|Niet ondersteund||
|Van zelf-ondertekend|Voor zelf-ondertekend|Niet ondersteund||
|Van zelf-ondertekend|Naar openbaar<sup>*</sup>|Ondersteund|1803 & hoger|
|Van Enterprise|Naar de onderneming|Ondersteund zolang klanten de dezelfde ondernemings-CA gebruiken zoals gebruikt bij de implementatie|1803 & hoger|
|Van Enterprise|Voor zelf-ondertekend|Niet ondersteund||
|Van Enterprise|Naar openbaar<sup>*</sup>|Ondersteund|1803 & hoger|
|Van publiek<sup>*</sup>|Naar de onderneming|Niet ondersteund|1803 & hoger|
|Van publiek<sup>*</sup>|Voor zelf-ondertekend|Niet ondersteund||
|Van publiek<sup>*</sup>|Naar openbaar<sup>*</sup>|Ondersteund|1803 & hoger|

<sup>*</sup> Hier volgen openbare certificeringsinstanties die deel van de vertrouwde basis-programma van Windows uitmaken. U vindt de volledige lijst [Microsoft Trusted Root Certificate Program: deelnemers (vanaf 27 juni 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Waarschuwing herstel

Wanneer geheimen binnen 30 dagen na de vervaldatum zijn, worden de volgende waarschuwingen gegenereerd in de Beheerdersportal: 

- Wachtwoordverlooptijd in behandeling zijnde service-account 
- In behandeling intern certificaat verloopt 
- In behandeling extern certificaat verloopt 

Geheime rotatie van de onderstaande instructies wordt uitgevoerd, wordt deze waarschuwingen oplossen.

## <a name="pre-steps-for-secret-rotation"></a>Vooraf stappen voor het geheime draaien

1.  Waarschuw de gebruikers van elke onderhoudsbewerkingen. Normale onderhoudsvensters die zo veel mogelijk, tijdens buiten kantooruren plannen. Onderhoud kunnen zowel de werkbelastingen van de gebruiker en de portal bewerkingen beïnvloeden.

    > [!note]  
    > De volgende stappen gelden alleen wanneer de externe Azure-Stack-geheimen draaien.

2. Zorg ervoor dat geheime rotatie nog niet is uitgevoerd op uw omgeving binnen de afgelopen maand. Op dit moment ondersteunt Azure Stack alleen geheime rotatie één keer per maand. 
3. Bereid een nieuwe reeks vervanging externe certificaten. De nieuwe set overeenkomt met de certificaat-specificaties die worden beschreven in de [Azure Stack PKI-certificaatvereisten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
4.  Een back-up naar de certificaten gebruikt voor rotatie op een veilige back-locatie opslaan. Als uw rotatie wordt uitgevoerd en mislukt, vervangt u de certificaten in de bestandsshare met de back-ups voordat u de draaihoek opnieuw uitvoeren. Houd er rekening mee, back-ups behouden in de veilige back-uplocatie.
5.  Een bestandsshare die u vanaf de ERCS virtuele machines openen kunt maken. De bestandsshare moet leesbaar en beschrijfbaar zijn voor de **CloudAdmin** identiteit.
6.  Open een PowerShell ISE-console vanaf een computer waarop u toegang tot de bestandsshare hebt. Navigeer naar de bestandsshare. 
7.  Voer **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** om de vereiste mappen voor uw externe certificaten te maken.

## <a name="rotating-external-and-internal-secrets"></a>Externe en interne geheimen draaien

Beide externe een interne geheim draaien:

1. Binnen het zojuist gemaakte **certificaten** map gemaakt in de stappen vóór de nieuwe set van externe certificaten vervangen in de mapstructuur volgens de notatie die wordt beschreven in de sectie verplichte certificaten plaatsen van de [Azure Stack PKI-certificaatvereisten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Maken van een PowerShell-sessie met de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) met behulp van de **CloudAdmin** account en de sessies opslaan als een variabele. Als de parameter in de volgende stap gebruikt u deze variabele.

    > [!IMPORTANT]  
    > Voer de sessie, slaat u de sessie als een variabele niet.
    
3. Voer  **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Geeft de sessievariabele bevoorrechte eindpunt PowerShell als de **sessie** parameter. 
4. Voer **Start SecretRotation** met de volgende parameters:
    - **PfxFilesPath**  
    Geef het netwerkpad naar de directory van uw certificaten eerder hebt gemaakt.  
    - **PathAccessCredential**  
    Een PSCredential-object om referenties voor de share. 
    - **CertificatePassword**  
    Een beveiligde tekenreeks van het wachtwoord dat wordt gebruikt voor alle de pfx-certificaatbestanden gemaakt.
4. Een ogenblik geduld terwijl uw geheimen draaien.  
Wanneer deze geheime wisselen is voltooid, de console weergegeven **algemene actiestatus: geslaagd**. 
5. Uw certificaten verwijderen uit de share gemaakt in de vooraf stap na geheime wisselen is voltooid, en deze opslaan op hun beveiligde back-uplocatie. 

## <a name="walkthrough-of-secret-rotation"></a>Overzicht van de geheime draaihoek

Het volgende PowerShell-voorbeeld toont de cmdlets en -parameters worden uitgevoerd om uw geheimen draaien.

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
## <a name="rotating-only-internal-secrets"></a>Alleen intern geheimen draaien

Alleen intern geheimen met Azure-Stack draaien:

1. Maken van een PowerShell-sessie met de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Uitvoeren in de sessie bevoorrechte eindpunt **Start SecretRotation** zonder argumenten.

## <a name="start-secretrotation-reference"></a>Start SecretRotation verwijzing

De geheimen van een Azure-Stack-systeem worden gedraaid. Alleen uitgevoerd voor het eindpunt van Azure-Stack-bevoegdheden.

### <a name="syntax"></a>Syntaxis

Pad (standaard)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Beschrijving

De cmdlet Start-SecretRotation draait geheimen van de infrastructuur van een Azure-Stack-systeem. Standaard die alle geheimen blootgesteld aan het infrastructuurnetwerk in de interne gedraaid met invoer van gebruiker gedraaid en dus de certificaten van alle eindpunten voor extern netwerk-infrastructuur. Wanneer het roteren van de infrastructuur van externe netwerkeindpunten moet Start SecretRotation via een Invoke-Command-scriptblok worden uitgevoerd met de Azure-Stack-omgeving bevoorrechte eindpunt sessie doorgegeven als de sessieparameter.
 
### <a name="parameters"></a>Parameters

| Parameter | Type | Vereist | Positie | Standaard | Beschrijving |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Reeks  | False  | Met de naam  | Geen  | Het fileshare-pad naar de **\Certificates** map met alle externe eindpunt certificaten het netwerk. Alleen vereist wanneer de interne en externe geheimen draaien. Einde map moet **\Certificates**. |
| CertificatePassword | SecureString | False  | Met de naam  | Geen  | Het wachtwoord voor alle certificaten die zijn opgegeven in de PfXFilesPath. Waarde is vereist als PfxFilesPath wordt verstrekt wanneer zowel interne als externe geheimen worden gedraaid. |
|

### <a name="examples"></a>Voorbeelden
 
**Alleen interne infrastructuur geheimen draaien**

```powershell  
PS C:\> Start-SecretRotation  
```

Met deze opdracht worden alle van de infrastructuur geheimen blootgesteld aan Azure-Stack interne netwerk. Start SecretRotation alle stack gegenereerde geheimen draait, maar omdat er geen certificaten zijn opgegeven, niet extern eindpunt certificaten wordt gedraaid.  

**Interne en externe infrastructuur geheimen draaien**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Met deze opdracht worden alle van de infrastructuur geheimen blootgesteld aan Azure-Stack interne netwerk, evenals de TLS-certificaten gebruikt voor externe netwerkeindpunten infrastructuur Azure-Stack. Start SecretRotation draait alle stack gegenereerde geheimen en omdat er certificaten worden aangeboden, ook certificaten extern eindpunt wordt gedraaid.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Het wachtwoord baseboard management controller (BMC) bijwerken

De BMC (baseboard management controller) bewaakt de fysieke status van uw servers. De specificaties en instructies voor het bijwerken van het wachtwoord van de BMC variëren, afhankelijk van de leverancier van uw oorspronkelijke leveranciers (OEM)-hardware. De wachtwoorden voor Azure-Stack-onderdelen op een reguliere uitgebracht, moet u bijwerken.

1. Werk de BMC op fysieke servers van de Azure-Stack door de OEM-instructies te volgen. Het wachtwoord voor elke BMC in uw omgeving moet hetzelfde zijn.
2. Open een bevoorrechte eindpunt in Azure Stack-sessies. Zie voor instructies [met behulp van de bevoegde eindpunt in Azure-Stack](azure-stack-privileged-endpoint.md).
3. Nadat uw PowerShell-prompt is gewijzigd in **[IP-adres of ERCS VM name]: PS >** of **[azs ercs01]: PS >**, afhankelijk van de omgeving, voeren `Set-BmcPassword` door het uitvoeren van `invoke-command`. Uw sessievariabele bevoorrechte eindpunt als parameter doorgeven. Bijvoorbeeld:

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

[Meer informatie over Azure-Stack-beveiliging](azure-stack-security-foundations.md)
