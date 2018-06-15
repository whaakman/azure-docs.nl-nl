---
title: Runbooks worden uitgevoerd op Azure Automation Hybrid Runbook Worker
description: Dit artikel bevat informatie over het uitvoeren van runbooks op computers in uw lokale datacentrum of cloudprovider met de hybride Runbook Worker-rol.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a4cf32ea7b77db3fc78a404063b8a4d69ecebf58
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195706"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Actieve runbooks in een hybride Runbook Worker

Er is geen verschil in de structuur van runbooks die worden uitgevoerd in Azure Automation en toepassingen die worden uitgevoerd op een hybride Runbook Worker. Runbooks die u met elke gebruikt waarschijnlijk aanzienlijk verschillen echter omdat runbooks die gericht is op een hybride Runbook Worker doorgaans bronnen op de lokale computer zelf of op basis van bronnen in de lokale omgeving waarop deze wordt geïmplementeerd, terwijl runbooks in beheren Azure Automation beheren meestal bronnen in de Azure-cloud.

Wanneer u runbooks uit te voeren op een hybride Runbook Worker ontwerpt, moet u bewerken en testen van de runbooks in de computer die als host fungeert voor de hybride worker. De hostmachine heeft alle PowerShell-modules en toegang tot het netwerk te beheren en toegang tot de lokale bronnen. Wanneer een runbook is bewerkt en getest op de Hybrid worker-machine, kunt u deze uploaden naar de Azure Automation-omgeving waarin deze worden uitgevoerd in de hybride worker beschikbaar is. Het is belangrijk te weten dat taken worden uitgevoerd onder het lokale systeemaccount voor windows of een speciaal gebruikersaccount **nxautomation** op Linux, die subtiele verschillen kunnen opleveren bij het ontwerpen van runbooks voor een hybride Runbook Worker-dit moet zijn in aanmerking genomen.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Een runbook wordt gestart op hybride Runbook Worker

[Een Runbook starten in Azure Automation](automation-starting-a-runbook.md) beschrijft de verschillende methoden voor het starten van een runbook. Hybride Runbook Worker wordt toegevoegd een **RunOn** optie waarin u de naam van een hybride Runbook Worker-groep kunt opgeven. Als u een groep opgeeft, wordt het runbook de opgehaald en uitgevoerd door een van de werknemers in die groep. Als deze optie niet is opgegeven, wordt deze uitgevoerd in Azure Automation als normaal.

Wanneer u een runbook in de Azure portal start, krijgt u een **uitvoeren op** optie waarin u kunt selecteren **Azure** of **Hybrid Worker**. Als u selecteert **Hybrid Worker**, kunt u de groep uit een vervolgkeuzelijst.

Gebruik de **RunOn** parameter. U kunt de volgende opdracht gebruiken om een runbook met de naam Test-Runbook op een Hybrid Runbook Worker-groep met de naam MyHybridGroup met Windows PowerShell te starten.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> De **RunOn** parameter is toegevoegd aan de **Start AzureAutomationRunbook** cmdlet in versie 0.9.1 van Microsoft Azure PowerShell. U moet [de nieuwste versie downloaden](https://azure.microsoft.com/downloads/) als er een eerdere versie is geïnstalleerd. U hoeft alleen te installeren van deze versie op een werkstation waar u het runbook met PowerShell begint. U hoeft niet te installeren op de computer van de werknemer, tenzij u van plan bent runbooks starten vanaf die computer'

## <a name="runbook-permissions"></a>Runbook-machtigingen

Runbooks die worden uitgevoerd op een hybride Runbook Worker niet dezelfde methode die doorgaans gebruikt wordt voor runbooks die worden geverifieerd voor Azure-resources, omdat ze bij het openen van bronnen buiten Azure gebruiken. Het runbook kan ofwel de eigen verificatie tot lokale bronnen bieden of kunt u een RunAs-account om een gebruikerscontext voor alle runbooks.

### <a name="runbook-authentication"></a>Runbook-verificatie

Standaard runbooks worden uitgevoerd in de context van het lokale systeemaccount voor Windows en een speciaal gebruikersaccount **nxautomation** voor Linux op de lokale computer, zodat ze verificatiegegevens moeten opgeven hun eigen aan resources die ze toegang krijgen tot .

U kunt [referentie](automation-credentials.md) en [certificaat](automation-certificates.md) activa in uw runbook met cmdlets waarmee u referenties opgeven zodat kan worden geverifieerd voor verschillende bronnen. Het volgende voorbeeld ziet u een deel van een runbook dat een computer wordt opnieuw opgestart. Deze referenties opgehaald uit een referentie-element en de naam van de computer van een variabele asset en gebruikt vervolgens deze waarden met de cmdlet Restart-Computer.

```azurepowershell-interactive
$Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
$Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

U kunt ook gebruikmaken van [InlineScript](automation-powershell-workflow.md#inlinescript), waarmee u blokken code uitvoeren op een andere computer met de referenties die zijn opgegeven door de [PSCredential algemene parameter](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>RunAs-account

De hybride Runbook Worker gebruikt standaard lokale systeem voor Windows en een speciaal gebruikersaccount **nxautomation** voor Linux runbooks uitvoeren. In plaats van runbooks bieden hun eigen verificatie van lokale bronnen, kunt u een **RunAs** account voor een Hybrid worker-groep. U geeft een [referentieasset](automation-credentials.md) die toegang heeft tot de lokale bronnen en alle runbooks worden uitgevoerd onder deze referenties wanneer u gebruikmaakt van een hybride Runbook Worker in de groep.

De gebruikersnaam op voor de referentie moet worden gebruikt in een van de volgende indelingen:

* domein\gebruikersnaam
* username@domain
* gebruikersnaam (voor accounts die lokaal op de lokale computer)

Gebruik de volgende procedure om op te geven van een RunAs-account voor een Hybrid worker-groep:

1. Maak een [referentieasset](automation-credentials.md) met toegang tot lokale bronnen.
2. Open het Automation-account in de Azure portal.
3. Selecteer de **Hybrid Worker-groepen** tegel en selecteer vervolgens de groep.
4. Selecteer **alle instellingen** en vervolgens **hybride worker groepsinstellingen**.
5. Wijziging **Run As-** van **standaard** naar **aangepaste**.
6. Selecteer de referentie en klik op **opslaan**.

### <a name="automation-run-as-account"></a>Automation-Run As-account

Als onderdeel van uw automatische proces voor het implementeren van resources in Azure, hebt u toegang tot on-premises systemen ter ondersteuning van een taak of een reeks stappen in de volgorde van de implementatie mogelijk nodig. Ter ondersteuning van verificatie op basis van Azure met behulp van het Run As-account, moet u het Run As-account-certificaat installeren.

De volgende PowerShell-runbook *Export RunAsCertificateToHybridWorker*, exporteert u het Run As-certificaat van uw Azure Automation-account en downloadt en in het certificaatarchief van de lokale computer op een hybride worden geïmporteerd werknemer is verbonden met hetzelfde account. Zodra deze stap is voltooid, controleert u of dat het werkproces kan worden geverifieerd in Azure maken met het Run As-account.

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

Sla de *Export RunAsCertificateToHybridWorker* runbook op uw computer met een `.ps1` extensie. Importeren in uw Automation-account en het bewerken van het runbook, als u de waarde van de variabele `$Password` met uw eigen wachtwoord. Publiceren en voer vervolgens het runbook die gericht is op de Hybrid Worker-groep die worden uitgevoerd en verifiëren van runbooks met het Run As-account. De taakstroom rapporteert de poging om het certificaat importeren in het archief van de lokale computer en volgt met meerdere regels, afhankelijk van hoeveel Automation-accounts zijn gedefinieerd in uw abonnement en als verificatie geslaagd is.

## <a name="job-behavior"></a>Gedrag van de taak

Taken worden verwerkt op hybride Runbook Workers iets anders dan wanneer ze worden uitgevoerd op Azure sandboxes. Één belangrijk verschil is dat er geen limiet voor de duur van de taak op hybride Runbook Workers. Als u een runbook langlopende hebt die u wilt ervoor te zorgen dat het bestand tegen mogelijk opnieuw wordt opgestart, bijvoorbeeld is als de computer die als host fungeert voor de hybride worker opnieuw is opgestart. Als de Hybrid worker-hostmachine opnieuw is opgestart, vervolgens opnieuw een actief runbooktaak vanaf het begin of vanaf het laatste controlepunt voor PowerShell Workflow-runbooks. Als een runbooktaak is meer dan 3 keer opnieuw wordt opgestart, wordt deze onderbroken.

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Het oplossen van runbooks op hybride Runbook Worker

Logboeken worden lokaal opgeslagen op elke worker hybride op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Hybride werknemers registreren ook fouten en gebeurtenissen in het Windows-gebeurtenislogboek onder **toepassingen en Services Logs\Microsoft-SMA\Operational**. Gebeurtenissen met betrekking tot runbooks die worden uitgevoerd op de worker worden geschreven naar **toepassingen en Services Logs\Microsoft-Automation\Operational**. De **Microsoft SMA** logboek bevat veel meer gebeurtenissen met betrekking tot de runbooktaak die naar de werknemer en de verwerking van het runbook gepusht. Terwijl de **Microsoft Automation** gebeurtenislogboek heeft geen veel gebeurtenissen met details hulp bij het oplossen van problemen met de uitvoering van runbook, deze bevat de resultaten van de runbooktaak.

[Runbook uitvoer en berichten](automation-runbook-output-and-messages.md) worden verzonden naar Azure Automation van hybrid workers net als runbooktaken uitvoeren in de cloud. Ook kunt u de stromen uitgebreid en voortgang van de dezelfde manier als voor andere runbooks.

Als uw runbooks zijn niet voltooid, waardoor de taak overzicht de status van toont **onderbroken**, raadpleegt u het artikel over probleemoplossing [Hybrid Runbook Worker: een runbooktaak wordt beëindigd met de status onderbroken](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt om een runbook te starten, [een Runbook starten in Azure Automation](automation-starting-a-runbook.md).
* Zie voor informatie over de verschillende procedures voor het werken met PowerShell en PowerShell Workflow-runbooks in Azure Automation met behulp van de teksteditor, [bewerken van een Runbook in Azure Automation](automation-edit-textual-runbook.md)
