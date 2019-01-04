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
ms.date: 12/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 27fd02503881ef1f0587ccb0301f8490101d5bcb
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720642"
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
  - Beheerdersuitbreiding Host  
  - ACS (met inbegrip van de blob, table en queue storage)  
  - AD FS *  
  - Grafiek *  

\* Alleen van toepassing als id-provider van de omgeving Active Directory Federated Services (AD FS is).

> [!Note]  
> Alle andere beveiligde sleutels en tekenreeksen, met inbegrip van de BMC en schakelt u over wachtwoorden, gebruikers en beheerders wachtwoorden worden nog steeds handmatig bijgewerkt door de beheerder. 

> [!Note]  
> Beginnen met de Azure Stack 1811 release, is geheime rotatie gescheiden voor interne en externe certificaten. 

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

<sup>*</sup>Geeft aan dat de openbare certificeringsinstanties die deel van het Windows Trusted Root-programma uitmaken. U vindt de volledige lijst in het artikel [Microsoft Trusted Root Certificate Program: Deelnemers aan (vanaf 27 juni 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Herstel van waarschuwing

Wanneer de geheimen zijn binnen 30 dagen na verlopen, worden de volgende waarschuwingen gegenereerd in de Beheerdersportal: 

- Wachtende service-account wachtwoord verloopt 
- In behandeling interne certificaat verloopt 
- In behandeling extern certificaat verlopen 

Geheime rotatie met behulp van de onderstaande instructies uitgevoerd, wordt deze waarschuwingen oplossen.

> [!Note]  
> Azure Stack-omgevingen in de pre-1811 versies mogelijk waarschuwingen voor in behandeling zijnde intern certificaat of geheime accountwachtwoorden ziet. Deze waarschuwingen niet juist zijn en moeten worden genegeerd zonder uit te voeren van interne geheime draaien. Onjuiste vervaldatum van de interne clientgeheim waarschuwingen zijn een bekend probleem dat is opgelost in 1811 – interne geheimen verloopt niet, tenzij de omgeving actief is geweest gedurende twee jaar. 

## <a name="pre-steps-for-secret-rotation"></a>Stappen voor geheime rotatie vóór

   > [!IMPORTANT]  
   > Als de geheime rotatie is al uitgevoerd op uw Azure Stack-omgeving moet u het systeem bijwerken naar versie 1811 of hoger voordat u geheime rotatie opnieuw uitvoert. Geheime rotatie moet worden uitgevoerd door de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) en Azure Stack-operators referenties vereist. Als uw omgeving Azure Stack Operator(s) niet weet of geheime rotatie is uitgevoerd op uw omgeving, moet u bijwerken naar 1811 alvorens geheime rotatie opnieuw.

1. Het is raadzaam dat u uw Azure Stack-exemplaar naar versie 1811 bijwerken.

    > [!Note] 
    > U hoeft voor pre-1811 versies niet geheimen om toe te voegen host uitbreidingscertificaten draaien. U moet de instructies in het artikel [voorbereiden voor de host van de extensie voor Azure Stack](azure-stack-extension-host-prepare.md) extensie host certificaten toevoegen.

2.  Operators mogelijk waarschuwingen openen en sluiten van automatisch tijdens het draaien van geheimen met Azure Stack.  Dit is verwacht gedrag en de waarschuwingen kunnen worden genegeerd.  Operators kunnen de geldigheid van deze waarschuwingen controleren door te voeren **Test AzureStack**.  Voor operators SCOM gebruiken voor het bewaken van Azure Stack-systemen, plaatsen van een systeem in de onderhoudsmodus wordt voorkomen dat deze waarschuwingen hun ITSM-systemen is bereikt, maar blijft het een waarschuwing als door het Azure Stack-systeem niet meer bereikbaar is. 
3. Informeer de gebruikers van elke onderhoudsbewerkingen. Normale onderhoudsvensters, zo veel mogelijk tijdens de kantooruren plannen. Onderhoudsbewerkingen mogelijk van invloed op zowel de werkbelastingen van de gebruiker en de portal bewerkingen.

    > [!Note]  
    > De volgende stappen zijn alleen van toepassing wanneer u externe geheimen voor Azure Stack.

4. Voer **[Test AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** en controleer of alle test uitvoer in orde zijn voordat het draaien van geheimen.
5. Bereid een nieuwe set vervanging externe certificaten. De nieuwe set komt overeen met de certificaat-specificaties die worden beschreven in de [Azure Stack PKI-certificaatvereisten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs). U kunt een CSR-request (CSR) genereren voor de aanschaf of het maken van nieuwe certificaten met behulp van de stappen in [PKI-certificaten genereren](https://docs.microsoft.com/azure/azure-stack/azure-stack-get-pki-certs) en bereid ze voor gebruik in uw Azure Stack-omgeving met de stappen in [ Bereid Azure Stack PKI-certificaten](https://docs.microsoft.com/azure/azure-stack/azure-stack-prepare-pki-certs). Zorg ervoor dat u het valideren van de certificaten die u met de stappen voorbereiden in [PKI-certificaten valideren](https://docs.microsoft.com/azure/azure-stack/azure-stack-validate-pki-certs). 
6.  Een back-up naar de certificaten gebruikt voor draaien op een veilige locatie van de back-Store. Als uw rotatie wordt uitgevoerd en mislukt, vervangt u de certificaten in de bestandsshare met de back-ups voordat u de draaihoek van het opnieuw uitvoeren. Houd er rekening mee, back-ups behouden in de veilige back-uplocatie.
7.  Maak een bestandsshare die u vanaf de ERCS virtuele machines openen kunt. De bestandsshare moet leesbaar en beschrijfbaar zijn voor de **CloudAdmin** identiteit.
8.  Open een PowerShell ISE-console op een computer waar u toegang tot de bestandsshare hebt. Navigeer naar de bestandsshare. 
9.  Voer **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** om de vereiste mappen voor uw externe certificaten te maken.

## <a name="rotating-external-secrets"></a>Externe geheimen draaien

Externe geheimen draaien:

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
4. Een ogenblik geduld terwijl uw geheimen draaien. Externe geheime rotatie duurt normaal gesproken ongeveer één uur. Wanneer deze geheime rotatie is voltooid, de console weergegeven **algemene actiestatus: Geslaagde**. 
    > [!Note]  
    > Als de geheime rotatie is mislukt, volg de instructies in het foutbericht en opnieuw uitvoeren **start secretrotation** met de **-opnieuw uitvoeren** Parameter. 

    ```PowerShell  
    Start-SecretRotation -Rerun
    ```
    Neem contact op met ondersteuning als u ondervindt herhaald geheime rotatie fouten.
5. Na voltooiing van de geheime rotatie, uw certificaten verwijderen uit de share in de vooraf stap hebt gemaakt en op te slaan in de veilige back-uplocatie. 

## <a name="walkthrough-of-secret-rotation"></a>Overzicht van het geheim draaien

De volgende PowerShell-voorbeeld ziet u de cmdlets en -parameters om uit te voeren om uw geheimen draaien.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>Alleen interne geheimen draaien

> [!Note]  
> Interne geheim rotatie moet alleen worden uitgevoerd als u vermoedt dat een interne geheim is aangetast door een kwaadwillende entiteit, of als u hebt ontvangen een waarschuwing (op build 1811 of hoger) die aangeeft interne certificaten bijna zijn verlopen. Azure Stack-omgevingen in de pre-1811 versies mogelijk waarschuwingen voor in behandeling zijnde intern certificaat of geheime accountwachtwoorden ziet. Deze waarschuwingen niet juist zijn en moeten worden genegeerd zonder uit te voeren van interne geheime draaien. Onjuiste vervaldatum van de interne clientgeheim waarschuwingen zijn een bekend probleem dat is opgelost in 1811 – interne geheimen verloopt niet, tenzij de omgeving actief is geweest gedurende twee jaar.

Alleen interne geheimen met Azure-Stack draaien:

1. Maken van een PowerShell-sessie met de [bevoegde eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Uitvoeren in de sessie bevoegde eindpunt **Start SecretRotation-interne**.

    > [!Note]  
    > Azure Stack-omgevingen in de pre-1811 versies is niet vereist de **-interne** vlag. **Start-SecretRotation** alleen interne geheimen draait.

3. Een ogenblik geduld terwijl uw geheimen draaien.  
Wanneer deze geheime rotatie is voltooid, de console weergegeven **algemene actiestatus: Geslaagde**. 
    > [!Note]  
    > Als geheim rotatie is mislukt, volg de instructies in het foutbericht wordt weergegeven en wordt opnieuw uitgevoerd **start secretrotation** met de **– intern** en **-opnieuw** parameters.  

    ```PowerShell  
    Start-SecretRotation -Internal -Rerun
    ```
    Neem contact op met ondersteuning als u ondervindt herhaald geheime rotatie fouten.

## <a name="start-secretrotation-reference"></a>Start-SecretRotation verwijzing

De geheimen van een Azure Stack-systeem draait. Alleen uitgevoerd voor de bevoegde eindpunt voor Azure Stack.

### <a name="syntax"></a>Syntaxis

#### <a name="for-external-secret-rotation"></a>Voor externe geheime rotatie

```Powershell  
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>Voor interne geheime rotatie 

```Powershell  
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>Voor externe geheime rotatie opnieuw uitvoeren 

```Powershell  
Start-SecretRotation [-Rerun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>Voor interne geheime rotatie opnieuw uitvoeren 

```Powershell  
Start-SecretRotation [-Rerun] [-Internal]
```
### <a name="description"></a>Description

De **Start SecretRotation** cmdlet draait de geheimen van de infrastructuur van een Azure Stack-systeem. Standaard worden deze alleen de certificaten van alle externe network-infrastructuur-eindpunten. Als u gebruikt met de - interne vlag interne worden infrastructuur geheimen gedraaid. Wanneer u externe network-infrastructuur-eindpunten, **Start SecretRotation** moet worden uitgevoerd met een **Invoke-Command** scriptblok met de Azure Stack-omgeving in de beschermde modus eindpunt sessie Als de sessieparameter doorgegeven.
 
### <a name="parameters"></a>Parameters

| Parameter | Type | Vereist | Positie | Standaard | Description |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Reeks  | False  | met de naam  | Geen  | Het fileshare-pad naar de **\Certificates** map met alle externe eindpunt certificaten het netwerk. Alleen vereist wanneer u externe geheimen of alle geheime gegevens. Einde map moet **\Certificates**. |
| CertificatePassword | SecureString | False  | met de naam  | Geen  | Het wachtwoord voor alle certificaten die zijn opgegeven in de - PfXFilesPath. Vereiste waarde als PfxFilesPath is opgegeven als zowel interne als externe geheimen worden gedraaid. |
| Intern | Reeks | False | met de naam | Geen | Interne markering moet worden gebruikt op elk moment Azure Stack-operators wil de interne infrastructuur geheimen draaien. |
| PathAccessCredential | PSCredential | False  | met de naam  | Geen  | De PowerShell-referentie voor de bestandsshare van de **\Certificates** map met alle externe eindpunt certificaten het netwerk. Alleen vereist wanneer u externe geheimen of alle geheime gegevens.  |
| Opnieuw uitvoeren | SwitchParameter | False  | met de naam  | Geen  | Opnieuw uitvoeren moet worden gebruikt op elk moment geheime rotatie is nieuwe na een mislukte poging. |

### <a name="examples"></a>Voorbeelden
 
#### <a name="rotate-only-internal-infrastructure-secrets"></a>Alleen interne infrastructuur geheimen draaien

Dit moet worden uitgevoerd via uw Azure Stack [omgeving in de beschermde modus eindpunt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).

```powershell  
PS C:\> Start-SecretRotation  -Internal
```

Met deze opdracht worden alle van de infrastructuur-geheimen blootgesteld aan interne netwerk van Azure Stack. 

#### <a name="rotate-only-external-infrastructure-secrets"></a>Alleen externe infrastructuur geheimen draaien  

```powershell  
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock {  

Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }  

Remove-PSSession -Session $PEPSession
```

Met deze opdracht worden de TLS-certificaten gebruikt voor het externe netwerk infrastructuur-eindpunten van Azure Stack.   

**Interne en externe infrastructuur geheimen draaien**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Met deze opdracht worden alle van de infrastructuur-geheimen blootgesteld aan interne netwerk van Azure Stack, evenals de TLS-certificaten gebruikt voor het externe netwerk infrastructuur-eindpunten van Azure Stack. Start-SecretRotation draait alle geheimen stack gegenereerd en omdat er certificaten worden verleend, Extern eindpunt certificaten ook worden gedraaid.  


## <a name="update-the-baseboard-management-controller-bmc-credential"></a>Bijwerken van de baseboard management controller (BMC)-referentie

De BMC (baseboard management controller) wordt de fysieke status van uw servers. De specificaties en instructies over het bijwerken van de accountnaam en het wachtwoord van de BMC variëren op basis van de leverancier van de oorspronkelijke leveranciers (OEM)-hardware. U moet uw wachtwoorden voor Azure Stack-onderdelen op gezette tijden bijwerken.

1. De BMC op de Azure Stack fysieke servers bijwerken door de OEM-instructies te volgen. De accountnaam en het wachtwoord voor elke BMC in uw omgeving moeten hetzelfde zijn.
2. Een bevoegde eindpunt geopend in Azure Stack-sessies. Zie voor instructies [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).
3. Nadat uw PowerShell-prompt is gewijzigd in **[IP-adres of ERCS VM name]: PS >** of **[azs-ercs01]: PS >**, afhankelijk van de omgeving, voeren `Set-BmcCredemtial` door uit te voeren `invoke-command`. Uw sessievariabele bevoegde eindpunt als een parameter doorgeven. Bijvoorbeeld:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBMCuser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BMCPassword is mandatory, while the BMCUser parameter is optional.
        Set-Bmccredential -bmcpassword $using:NewBMCpwd -bmcuser $using:NewBMCuser
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    U kunt ook de statische PowerShell-versie met de wachtwoorden gebruiken als coderegels:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -Force
    $NewBMCuser = "<New BMC User name>" 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmccredential -bmcpassword $using:NewBMCpwd -bmcuser $using:NewBMCuser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure Stack-beveiliging](azure-stack-security-foundations.md)
