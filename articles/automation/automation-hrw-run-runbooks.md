---
title: Runbooks uitvoeren op Hybrid Runbook Worker voor Azure Automation
description: In dit artikel bevat informatie over het uitvoeren van runbooks op virtuele machines in uw lokale datacenter of cloudprovider met de Hybrid Runbook Worker-rol.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 58ff2b5bbf338f3af78b693aef57cf6293dc08b7
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436505"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Runbooks uitvoeren op een Hybrid Runbook Worker

Er is geen verschil in de structuur van runbooks die worden uitgevoerd in Azure Automation en bronnen die worden uitgevoerd op een Hybrid Runbook Worker. Runbooks die u met elke gebruikt waarschijnlijk aanzienlijk verschillen echter omdat de runbooks die gewoonlijk zijn gericht op een Hybrid Runbook Worker-resources op de lokale computer zelf of op basis van resources in de lokale omgeving waarin deze is geïmplementeerd, terwijl runbooks in beheren Azure Automation beheren meestal resources in de Azure-cloud.

U moet bij het ontwerpen van runbooks om uit te voeren op een Hybrid Runbook Worker, bewerken en testen van de runbooks in de computer die als host fungeert voor de Hybrid worker. De hostmachine heeft alle van de PowerShell-modules en toegang tot het netwerk die u wilt beheren en toegang tot de lokale bronnen. Zodra een runbook is bewerkt en getest op de Hybrid worker-machine, kunt u deze vervolgens uploaden naar de Azure Automation-omgeving waar deze worden uitgevoerd in de Hybrid worker beschikbaar is. Het is belangrijk te weten dat taken worden uitgevoerd onder het lokale systeemaccount voor windows of een speciaal gebruikersaccount **nxautomation** op Linux, die kan leiden tot subtiele verschillen bij het ontwerpen van runbooks voor Hybrid Runbook Worker dit moet zijn in aanmerking genomen.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Een runbook starten op Hybrid Runbook Worker

[Een Runbook starten in Azure Automation](automation-starting-a-runbook.md) beschrijft de verschillende methoden voor het starten van een runbook. Hybrid Runbook Worker wordt toegevoegd een **RunOn** optie waarin u de naam van een Hybrid Runbook Worker-groep kunt opgeven. Als een groep is opgegeven, is het runbook de opgehaald en uitgevoerd door een van de werknemers in die groep. Als deze optie niet opgegeven is, wordt deze uitgevoerd in Azure Automation als normaal.

Wanneer u een runbook in Azure portal start, krijgt u een **uitvoeren op** optie waarin u kunt selecteren **Azure** of **Hybrid Worker**. Als u selecteert **Hybrid Worker**, kunt u de groep in een vervolgkeuzelijst.

Gebruik de **RunOn** parameter. U kunt de volgende opdracht gebruiken om een runbook met de naam Test-Runbook op een Hybrid Runbook Worker-groep met de naam MyHybridGroup met behulp van Windows PowerShell te starten.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> De **RunOn** parameter is toegevoegd aan de **Start AzureAutomationRunbook** cmdlet in versie 0.9.1 tot en van Microsoft Azure PowerShell. U moet [download de nieuwste versie](https://azure.microsoft.com/downloads/) hebt u een eerdere versie is geïnstalleerd. U hoeft deze versie te installeren op een werkstation dat is waar u het runbook vanuit PowerShell begint. U hoeft niet te installeren op de worker-computer, tenzij u van plan bent om te starten van runbooks vanaf die computer"

## <a name="runbook-permissions"></a>Runbookmachtigingen

Runbooks die worden uitgevoerd op een Hybrid Runbook Worker kan niet dezelfde methode die meestal gebruikt wordt voor runbooks die worden geverifieerd met Azure-resources, omdat ze toegang resources buiten Azure tot zijn gebruiken. Geef het runbook kan een eigen verificatie van lokale bronnen, of kunt u een RunAs-account voor de context van een gebruiker voor alle runbooks.

### <a name="runbook-authentication"></a>Runbookverificatie

Standaard runbooks worden uitgevoerd in de context van het lokale systeemaccount gebruikt voor Windows en een speciaal gebruikersaccount **nxautomation** voor Linux op de lokale computer, zodat ze verificatiegegevens moeten opgeven hun eigen tot resources waartoe ze toegang hebben .

U kunt [referentie](automation-credentials.md) en [certificaat](automation-certificates.md) activa in uw runbook met cmdlets waarmee u referenties opgeven zodat u voor verschillende bronnen verifiëren kunt. Het volgende voorbeeld ziet u een deel van een runbook dat een computer wordt opnieuw opgestart. Deze referenties opgehaald uit een referentie-element en de naam van de computer van een variabele asset en gebruikt deze waarden vervolgens met de cmdlet Restart-Computer.

```azurepowershell-interactive
$Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
$Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

U kunt ook gebruikmaken van [InlineScript](automation-powershell-workflow.md#inlinescript), waarmee u blokken code uitvoeren op een andere computer met referenties die zijn opgegeven door de [PSCredential algemene parameter](/powershell/module/psworkflow/about/about_workflowcommonparameters).

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

### <a name="automation-run-as-account"></a>Automation uitvoeren als-account

Als onderdeel van uw geautomatiseerde build-proces voor het implementeren van resources in Azure, hebt u toegang tot on-premises systemen ter ondersteuning van een taak of een reeks stappen in de volgorde van de implementatie mogelijk nodig. Ter ondersteuning van verificatie op basis van Azure met behulp van de uitvoeren als-account, moet u het Run As-account-certificaat installeren.

De volgende PowerShell-runbook *Export-RunAsCertificateToHybridWorker*, exporteert u het Run As-certificaat van uw Azure Automation-account en downloadt en importeert deze in het certificaatarchief van lokale computer op een hybride werknemer verbonden met hetzelfde account. Als deze stap is voltooid, controleert u of dat de werknemer op Azure met behulp van de uitvoeren als-account kunt verifiëren.

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

Sla de *Export-RunAsCertificateToHybridWorker* runbook op de computer met een `.ps1` extensie. Importeren in uw Automation-account en het bewerken van het runbook, wijzigen van de waarde van de variabele `$Password` met uw eigen wachtwoord. Publiceren en voert u het runbook die gericht is op de Hybrid Worker-groep die worden uitgevoerd en verifiëren van runbooks met behulp van de uitvoeren als-account. De taakstroom rapporteert de poging om het certificaat importeren in het archief van de lokale computer, en volgt met meerdere regels, afhankelijk van hoeveel Automation-accounts zijn gedefinieerd in uw abonnement en als verificatie geslaagd is.

## <a name="job-behavior"></a>Gedrag van de taak

Taken worden verwerkt op Hybrid Runbook Workers iets anders dan wanneer ze worden uitgevoerd op Azure-sandboxes geladen. Één belangrijk verschil is dat er geen limiet voor de duur van de taak op Hybrid Runbook Workers. Als u een runbook langlopende hebt die u wilt ervoor te zorgen dat het bestand tegen mogelijk opnieuw wordt opgestart, bijvoorbeeld is als de computer die als host fungeert voor de Hybrid worker opnieuw wordt opgestart. Als de machine van de Hybrid worker-host opnieuw is opgestart, klikt u vervolgens een actief runbooktaak opnieuw wordt opgestart vanaf het begin of vanaf het laatste controlepunt voor PowerShell Workflow-runbooks. Als een runbook-taak is meer dan 3 keer opnieuw wordt opgestart, wordt deze onderbroken.

## <a name="troubleshoot"></a>Problemen oplossen

Als uw runbooks niet zijn voltooid en het taakoverzicht de status geeft **onderbroken**, bekijk de gids voor probleemoplossing in [fouten bij de uitvoering van runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt om een runbook te starten, [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).
* Zie voor meer informatie over de verschillende procedures voor het werken met PowerShell en PowerShell-werkstroom runbooks in Azure Automation met behulp van de teksteditor, [bewerken van een Runbook in Azure Automation](automation-edit-textual-runbook.md)
