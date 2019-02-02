---
title: Runbooks uitvoeren op Hybrid Runbook Worker voor Azure Automation
description: In dit artikel bevat informatie over het uitvoeren van runbooks op virtuele machines in uw lokale datacenter of cloudprovider met de Hybrid Runbook Worker-rol.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f1700e124d1f572d0bf0ca76ea7c465f1ecf96c1
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657413"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Runbooks uitvoeren op een Hybrid Runbook Worker

Er is geen verschil in de structuur van runbooks die worden uitgevoerd in Azure Automation en runbooks die worden uitgevoerd op een Hybrid Runbook Worker. Runbooks die u met elke gebruikt waarschijnlijk aanzienlijk verschillen. Dit verschil is, omdat de runbooks die zijn gericht op een Hybrid Runbook Worker doorgaans beheren van resources op de lokale computer zelf of op basis van resources in de lokale omgeving waarop deze wordt geïmplementeerd. Runbooks in Azure Automation beheren meestal resources in de Azure-cloud.

U moet bij het ontwerpen van runbooks om uit te voeren op een Hybrid Runbook Worker, bewerken en testen van de runbooks in de computer die als host fungeert voor de Hybrid worker. De hostmachine heeft alle van de PowerShell-modules en toegang tot het netwerk die u wilt beheren en toegang tot de lokale bronnen. Wanneer een runbook wordt getest op de Hybrid worker-machine, kunt u deze vervolgens uploaden naar de Azure Automation-omgeving waar deze worden uitgevoerd in de Hybrid worker beschikbaar is. Het is belangrijk te weten dat taken die uitvoeren onder het lokale systeemaccount gebruikt voor Windows of een speciaal gebruikersaccount **nxautomation** op Linux. Dit gedrag kunt subtiele verschillen introduceren bij het ontwerpen van runbooks voor Hybrid Runbook Worker. Deze wijzigingen moeten worden gecontroleerd bij het schrijven van uw runbooks.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Een runbook starten op Hybrid Runbook Worker

[Een Runbook starten in Azure Automation](automation-starting-a-runbook.md) beschrijft de verschillende methoden voor het starten van een runbook. Hybrid Runbook Worker wordt toegevoegd een **RunOn** optie waarin u de naam van een Hybrid Runbook Worker-groep kunt opgeven. Wanneer een groep is opgegeven, is het runbook de opgehaald en uitgevoerd door een van de werknemers in die groep. Als deze optie niet is opgegeven, wordt deze uitgevoerd in Azure Automation als normaal.

Wanneer u een runbook in Azure portal start, krijgt u een **uitvoeren op** optie waarin u kunt selecteren **Azure** of **Hybrid Worker**. Als u selecteert **Hybrid Worker**, kunt u de groep in een vervolgkeuzelijst.

Gebruik de **RunOn** parameter. U kunt de volgende opdracht gebruiken om een runbook met de naam Test-Runbook op een Hybrid Runbook Worker-groep met de naam MyHybridGroup met behulp van Windows PowerShell te starten.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> De **RunOn** parameter is toegevoegd aan de **Start AzureAutomationRunbook** cmdlet in versie 0.9.1 tot en van Microsoft Azure PowerShell. U moet [download de nieuwste versie](https://azure.microsoft.com/downloads/) hebt u een eerdere versie is geïnstalleerd. U hoeft deze versie te installeren op een werkstation dat is waar u het runbook vanuit PowerShell begint. U hoeft niet te installeren op de worker-computer, tenzij u van plan bent om te starten van runbooks vanaf die computer"

## <a name="runbook-permissions"></a>Runbookmachtigingen

Runbooks die worden uitgevoerd op een Hybrid Runbook Worker kan niet dezelfde methode die meestal gebruikt wordt voor runbooks die worden geverifieerd met Azure-resources, omdat ze toegang met resources in Azure niet tot krijgen gebruiken. Het runbook kan een eigen verificatie van lokale bronnen geven, of kunt configureren met behulp van verificatie [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager
). U kunt ook een RunAs-account voor de context van een gebruiker voor alle runbooks opgeven.

### <a name="runbook-authentication"></a>Runbookverificatie

Standaard runbooks worden uitgevoerd in de context van het lokale systeemaccount gebruikt voor Windows en een speciaal gebruikersaccount **nxautomation** voor Linux op de lokale computer, zodat ze verificatiegegevens moeten opgeven hun eigen tot resources waartoe ze toegang hebben .

U kunt [referentie](automation-credentials.md) en [certificaat](automation-certificates.md) activa in uw runbook met cmdlets waarmee u referenties opgeven zodat u voor verschillende bronnen verifiëren kunt. Het volgende voorbeeld ziet u een deel van een runbook dat een computer wordt opnieuw opgestart. Deze referenties opgehaald uit een referentie-element en de naam van de computer van een variabele asset en gebruikt deze waarden vervolgens met de cmdlet Restart-Computer.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

U kunt ook [InlineScript](automation-powershell-workflow.md#inlinescript), waarmee u codeblokken worden uitgevoerd op een andere computer met de referenties die zijn opgegeven door de [PSCredential algemene parameter](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Uitvoeren als-account

De Hybrid Runbook Worker gebruikt standaard lokale systeem voor Windows en een speciaal gebruikersaccount **nxautomation** voor Linux voor het uitvoeren van runbooks. In plaats van dat runbooks bieden hun eigen verificatie van lokale bronnen, kunt u een **RunAs** account voor een Hybrid worker-groep. U geeft een [referentie-element](automation-credentials.md) die toegang heeft tot lokale bronnen en alle runbooks worden uitgevoerd onder deze referenties bij het uitvoeren van op een Hybrid Runbook Worker in de groep.

De naam van de gebruiker voor de referentie moet zich in een van de volgende indelingen:

* domein\gebruikersnaam
* username@domain
* gebruikersnaam (voor accounts die lokaal op de lokale computer)

Gebruik de volgende procedure om op te geven van een RunAs-account voor een Hybrid worker-groep:

1. Maak een [referentie-element](automation-credentials.md) met toegang tot lokale resources.
2. Open het Automation-account in Azure portal.
3. Selecteer de **Hybrid Worker-groepen** tegel en selecteer vervolgens de groep.
4. Selecteer **alle instellingen** en vervolgens **Hybrid worker-groepsinstellingen**.
5. Wijziging **Run As-** van **standaard** naar **aangepaste**.
6. Selecteer de referentie op en klik op **opslaan**.

### <a name="managed-identities-for-azure-resources"></a>Beheerde identiteiten voor Azure-Resources

Hybrid Runbook Workers die worden uitgevoerd op virtuele machines van Azure kunt u beheerde identiteiten voor Azure-resources gebruiken om Azure-resources te verifiëren. Er zijn veel voordelen voor het gebruik van beheerde identiteiten voor Azure-resources via Run As-accounts.

* Niet nodig voor het Run As-certificaat exporteren en vervolgens importeren in Hybrid Runbook Worker
* U hoeft te vernieuwen van het certificaat dat wordt gebruikt door het uitvoeren als-account
* Niet nodig voor het afhandelen van het verbindingsobject uitvoeren als in uw runbook-code

Voor het gebruik van een beheerde identiteit voor Azure-resources op een Hybrid Runbook worker, moet u de volgende stappen uit:

1. Een Azure-VM maken
2. [Beheerde identiteiten voor Azure-resources op de virtuele machine configureren](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)
3. [Uw VM-toegang verlenen tot een resourcegroep in Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)
4. [Een toegangstoken met behulp van de virtuele machine door het systeem toegewezen beheerde identiteit](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager)
5. [Installeer de Windows Hybrid Runbook Worker](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) op de virtuele machine.

Als de voorgaande stappen voltooid zijn, kunt u `Connect-AzureRmAccount -Identity` in het runbook om te verifiëren bij Azure-resources. Deze configuratie vermindert de noodzaak voor het uitvoeren als-Account gebruiken en beheren van het certificaat voor het uitvoeren als-account.

```powershell
# Connect to Azure using the Managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
Connect-AzureRmAccount -Identity

# Get all VM names from the subscription
Get-AzureRmVm | Select Name
```

### <a name="runas-script"></a>Automation uitvoeren als-account

Als onderdeel van uw geautomatiseerde build-proces voor het implementeren van resources in Azure, hebt u toegang tot on-premises systemen ter ondersteuning van een taak of een reeks stappen in de volgorde van de implementatie mogelijk nodig. Ter ondersteuning van verificatie op basis van Azure met behulp van de uitvoeren als-account, moet u het Run As-account-certificaat installeren.

De volgende PowerShell-runbook **Export-RunAsCertificateToHybridWorker**, exporteert u het Run As-certificaat van uw Azure Automation-account en downloadt en importeert deze in het certificaatarchief van lokale computer op een hybride worker, dat is verbonden met hetzelfde account. Als deze stap is voltooid, controleert u of dat de werknemer op Azure met behulp van de uitvoeren als-account kunt verifiëren.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
AUTHOR: Azure Automation Team
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

Sla de *Export-RunAsCertificateToHybridWorker* runbook op de computer met een `.ps1` extensie. Importeren in uw Automation-account en het bewerken van het runbook, wijzigen van de waarde van de variabele `$Password` met uw eigen wachtwoord. Publiceren en voert u het runbook. De Hybrid Worker-groep die worden uitgevoerd en verifiëren van runbooks met behulp van de uitvoeren als-account als doel. De taakstroom rapporteert de poging om het certificaat importeren in het archief van de lokale computer, en volgt met meerdere regels. Dit gedrag is afhankelijk van hoeveel Automation-accounts die u definieert in uw abonnement en als verificatie geslaagd is.

## <a name="job-behavior"></a>Gedrag van de taak

Taken worden verwerkt op Hybrid Runbook Workers iets anders dan ze wanneer ze worden uitgevoerd op Azure-sandboxes geladen. Één belangrijk verschil is dat er geen limiet voor de duur van de taak op Hybrid Runbook Workers. Runbooks in Azure uitgevoerd sandboxes zijn beperkt tot drie uur vanwege [evenredige deel](automation-runbook-execution.md#fair-share). Voor een runbook langlopende wilt u om ervoor te zorgen dat het bestand tegen mogelijk opnieuw opstarten is. Bijvoorbeeld, als de computer die als host fungeert voor de Hybrid worker opnieuw wordt opgestart. Als de machine van de Hybrid worker-host opnieuw is opgestart, klikt u vervolgens een actief runbooktaak opnieuw wordt opgestart vanaf het begin of vanaf het laatste controlepunt voor PowerShell Workflow-runbooks. Nadat een runbook taak opnieuw wordt gestart meer dan 3 keer, en vervolgens onderbroken.

## <a name="run-only-signed-runbooks"></a>Alleen ondertekende Runbooks worden uitgevoerd

Hybrid Runbook Workers kunnen worden geconfigureerd om alleen ondertekende runbooks worden uitgevoerd met bepaalde configuratie. De volgende sectie wordt beschreven hoe u het instellen van de Hybrid Runbook Workers om uit te voeren ondertekende [Windows Hybrid Runbook Worker](#windows-hybrid-runbook-worker) en [Hybrid Runbook Worker in Linux](#linux-hybrid-runbook-worker)

> [!NOTE]
> Nadat u een Hybrid Runbook Worker voor het uitvoeren van alleen ondertekende runbooks hebt geconfigureerd, worden runbooks hebt **niet** is worden ondertekend om uit te voeren op de worker mislukken.

### <a name="windows-hybrid-runbook-worker"></a>Windows Hybrid Runbook Worker

#### <a name="create-signing-certificate"></a>Certificaat voor ondertekening maken

Het volgende voorbeeld wordt een zelfondertekend certificaat dat kan worden gebruikt voor het ondertekenen van runbooks. Het voorbeeld maakt het certificaat en exporteert het. Het certificaat wordt geïmporteerd in de Hybrid Runbook Workers later opnieuw. De vingerafdruk wordt ook geretourneerd deze waarde wordt later gebruikt om te verwijzen naar het certificaat.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

#### <a name="configure-the-hybrid-runbook-workers"></a>De Hybrid Runbook Workers configureren

Kopieer het certificaat dat is gemaakt voor elke Hybrid Runbook Worker in een groep. Voer het volgende script om het certificaat importeren en configureren van de Hybrid Worker voor het gebruik van handtekeningvalidatie op runbooks.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

#### <a name="sign-your-runbooks-using-the-certificate"></a>Meld u aan uw Runbooks met behulp van het certificaat

Met de Hybrid Runbook workers geconfigureerd voor het gebruik van alleen runbooks ondertekend, moet u zich aanmelden met runbooks die moeten worden gebruikt op de Hybrid Runbook Worker. Gebruik het volgende voorbeeld PowerShell aan te melden van uw runbooks.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Wanneer het runbook is ondertekend, moet deze worden geïmporteerd in uw Automation-Account en gepubliceerd met het handtekeningblok. Zie voor informatie over het importeren van runbooks, [een runbook uit een bestand importeren in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).

### <a name="linux-hybrid-runbook-worker"></a>Hybrid Runbook Worker in Linux

Het tekenen van runbooks op een Hybrid Runbook Worker in Linux, uw Hybrid Runbook Worker moet beschikken over de [GPG](https://gnupg.org/index.html) uitvoerbare aanwezig op de machine.

#### <a name="create-a-gpg-keyring-and-keypair"></a>Een GPG-sleutelhanger en -sleutelpaar maken

De sleutelhanger en -sleutelpaar gemaakt, moet u gebruikmaken van de Hybrid Runbook Worker-account maken `nxautomation`.

Gebruik `sudo` aan te melden als de `nxautomation` account.

```bash
sudo su – nxautomation
```

Eenmaal met behulp van de `nxautomation` account, het gpg-sleutelpaar genereren.

```bash
sudo gpg --generate-key
```

GPG leidt u door de stappen voor het maken van het sleutelpaar. U moet een naam, een e-mailadres, de verlooptijd van, wachtwoordzin en wacht onvoldoende entropie opgeven op de computer voor de sleutel moet worden gegenereerd.

Omdat de GPG-map met sudo is gegenereerd, moet u de eigenaar wijzigen in nxautomation. 

Voer de volgende opdracht om de eigenaar niet wijzigen.

```bash
sudo chown -R nxautomation ~/.gnupg
```

#### <a name="make-the-keyring-available-the-hybrid-runbook-worker"></a>Beschikbaar maken voor de sleutelhanger de Hybrid Runbook Worker

Zodra de sleutelhanger is gemaakt, moet u het beschikbaar maken voor de Hybrid Runbook Worker. Wijzigen van het bestand met instellingen `/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf` om op te nemen in het volgende voorbeeld onder de sectie `[worker-optional]`

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

#### <a name="verify-signature-validation-is-on"></a>Controleer of handtekeningvalidatie is ingeschakeld

Als handtekeningvalidatie is uitgeschakeld op de machine, moet u deze in te schakelen. Voer de volgende opdracht om in te schakelen handtekeningvalidatie. Vervangen van `<LogAnalyticsworkspaceId>` met uw werkruimte-id.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

#### <a name="sign-a-runbook"></a>Meld u aan een runbook

Nadat de handtekeningvalidatie van de is geconfigureerd, kunt u de volgende opdracht uit om te ondertekenen van een runbook:

```bash
gpg –clear-sign <runbook name>
```

De ondertekende runbook heeft de naam `<runbook name>.asc`.

De ondertekende runbook kan nu worden geüpload naar Azure Automation en kan worden uitgevoerd als een reguliere runbook.

## <a name="troubleshoot"></a>Problemen oplossen

Als uw runbooks worden niet met succes is voltooid, raadpleegt u de gids voor probleemoplossing op [fouten bij de uitvoering van runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt om een runbook te starten, [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).
* Zie voor meer informatie over de verschillende manieren om te werken met PowerShell-runbooks in Azure Automation met behulp van de teksteditor, [bewerken van een Runbook in Azure Automation](automation-edit-textual-runbook.md)

