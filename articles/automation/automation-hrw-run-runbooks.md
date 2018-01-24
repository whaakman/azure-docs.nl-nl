---
title: Runbooks worden uitgevoerd op Azure Automation Hybrid Runbook Worker | Microsoft Docs
description: Dit artikel bevat informatie over het uitvoeren van runbooks op computers in uw lokale datacentrum of cloudprovider met de hybride Runbook Worker-rol.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: magoedte
ms.openlocfilehash: d73bb33b4b330df803e140145ed63319af4a6733
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Actieve runbooks in een hybride Runbook Worker 
Er is geen verschil in de structuur van runbooks die worden uitgevoerd in Azure Automation en toepassingen die worden uitgevoerd op een hybride Runbook Worker. Runbooks die u met elke gebruikt waarschijnlijk aanzienlijk verschillen echter omdat runbooks die gericht is op een hybride Runbook Worker doorgaans bronnen op de lokale computer zelf of op basis van bronnen in de lokale omgeving waarop deze wordt geïmplementeerd, terwijl runbooks in beheren Azure Automation beheren meestal bronnen in de Azure-cloud.

U kunt een runbook bewerken voor hybride Runbook Worker in Azure Automation, maar misschien hebt u problemen als u probeert het runbook testen in de editor.  De PowerShell-modules die toegang hebben tot de lokale bronnen kunnen niet worden geïnstalleerd in uw Azure Automation-omgeving in dat geval de test mislukt.  Als u de vereiste modules installeert, klikt u vervolgens het runbook wordt uitgevoerd, maar wordt niet mogelijk is de toegang tot lokale bronnen voor een volledige test.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Een runbook wordt gestart op hybride Runbook Worker
[Een Runbook starten in Azure Automation](automation-starting-a-runbook.md) beschrijft de verschillende methoden voor het starten van een runbook.  Hybride Runbook Worker wordt toegevoegd een **RunOn** optie waarin u de naam van een hybride Runbook Worker-groep kunt opgeven.  Als u een groep opgeeft, wordt het runbook de opgehaald en uitgevoerd door de werknemers in die groep.  Als deze optie niet is opgegeven, wordt deze uitgevoerd in Azure Automation als normaal.

Wanneer u een runbook in de Azure portal start, krijgt u een **uitvoeren op** optie waarin u kunt selecteren **Azure** of **Hybrid Worker**.  Als u selecteert **Hybrid Worker**, kunt u de groep uit een vervolgkeuzelijst.

Gebruik de **RunOn** parameter.  U kunt de volgende opdracht gebruiken om een runbook met de naam Test-Runbook op een Hybrid Runbook Worker-groep met de naam MyHybridGroup met Windows PowerShell te starten.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> De **RunOn** parameter is toegevoegd aan de **Start AzureAutomationRunbook** cmdlet in versie 0.9.1 van Microsoft Azure PowerShell.  U moet [de nieuwste versie downloaden](https://azure.microsoft.com/downloads/) als er een eerdere versie is geïnstalleerd.  U hoeft alleen te installeren van deze versie op een werkstation waarop het runbook wordt gestart vanuit Windows PowerShell.  U hoeft niet te installeren op de computer van de werknemer, tenzij u van plan bent runbooks starten vanaf die computer.  U kunt geen momenteel een runbook starten op een hybride Runbook Worker vanuit een ander runbook omdat dit de meest recente versie van Azure Powershell worden geïnstalleerd in uw Automation-account vereist.  De nieuwste versie wordt automatisch bijgewerkt in Azure Automation en automatisch vanaf de werknemers snel.
>
>

## <a name="runbook-permissions"></a>Runbook-machtigingen
Runbooks die worden uitgevoerd op een hybride Runbook Worker niet dezelfde methode die doorgaans gebruikt wordt voor runbooks die worden geverifieerd voor Azure-resources, omdat ze bij het openen van bronnen buiten Azure gebruiken.  Het runbook kan ofwel de eigen verificatie tot lokale bronnen bieden of kunt u een RunAs-account om een gebruikerscontext voor alle runbooks.

### <a name="runbook-authentication"></a>Runbook-verificatie
Standaard wordt runbooks uitgevoerd in de context van het lokale systeemaccount op de lokale computer, zodat ze hun eigen verificatiegegevens aan resources die ze toegang hebben tot moeten opgeven.  

U kunt [referentie](http://msdn.microsoft.com/library/dn940015.aspx) en [certificaat](http://msdn.microsoft.com/library/dn940013.aspx) activa in uw runbook met cmdlets waarmee u referenties opgeven zodat kan worden geverifieerd voor verschillende bronnen.  Het volgende voorbeeld ziet u een deel van een runbook dat een computer wordt opnieuw opgestart.  Deze referenties opgehaald uit een referentie-element en de naam van de computer van een variabele asset en gebruikt vervolgens deze waarden met de cmdlet Restart-Computer.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

U kunt ook gebruikmaken van [InlineScript](automation-powershell-workflow.md#inlinescript), waarmee u blokken code uitvoeren op een andere computer met de referenties die zijn opgegeven door de [PSCredential algemene parameter](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="runas-account"></a>RunAs-account
In plaats van runbooks bieden hun eigen verificatie van lokale bronnen, kunt u een **RunAs** account voor een Hybrid worker-groep.  U geeft een [referentieasset](automation-credentials.md) die toegang heeft tot de lokale bronnen en alle runbooks worden uitgevoerd onder deze referenties wanneer u gebruikmaakt van een hybride Runbook Worker in de groep.  

De gebruikersnaam op voor de referentie moet worden gebruikt in een van de volgende indelingen:

* domain\username
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
Als onderdeel van uw automatische proces voor het implementeren van resources in Azure, hebt u toegang tot on-premises systemen ter ondersteuning van een taak of een reeks stappen in de volgorde van de implementatie mogelijk nodig.  Ter ondersteuning van verificatie op basis van Azure met behulp van het Run As-account, moet u het Run As-account-certificaat installeren.  

De volgende PowerShell-runbook *Export RunAsCertificateToHybridWorker*, exporteert u het Run As-certificaat van uw Azure Automation-account en downloadt en in het certificaatarchief van de lokale computer op een hybride worden geïmporteerd werknemer is verbonden met hetzelfde account.  Zodra deze stap is voltooid, controleert u of dat het werkproces kan worden geverifieerd in Azure maken met het Run As-account.

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

    [OutputType([string])] 

    # Set the password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

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
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts to confirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select-Object AutomationAccountName

Sla de *Export RunAsCertificateToHybridWorker* runbook op uw computer met een `.ps1` extensie.  Importeren in uw Automation-account en het bewerken van het runbook, als u de waarde van de variabele `$Password` met uw eigen wachtwoord.  Publiceren en voer vervolgens het runbook die gericht is op de Hybrid Worker-groep die worden uitgevoerd en verifiëren van runbooks met het Run As-account.  De taakstroom rapporteert de poging om het certificaat importeren in het archief van de lokale computer en volgt met meerdere regels, afhankelijk van hoeveel Automation-accounts zijn gedefinieerd in uw abonnement en als verificatie geslaagd is.  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Het oplossen van runbooks op hybride Runbook Worker
Logboeken worden lokaal opgeslagen op elke worker hybride op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  Hybride worker registreert ook fouten en gebeurtenissen in het Windows-gebeurtenislogboek onder **toepassingen en Services Logs\Microsoft-SMA\Operational**.  Gebeurtenissen met betrekking tot runbooks die worden uitgevoerd op de worker worden geschreven naar **toepassingen en Services Logs\Microsoft-Automation\Operational**.  De **Microsoft SMA** logboek bevat veel meer gebeurtenissen met betrekking tot de runbooktaak die naar de werknemer en de verwerking van het runbook gepusht.  Terwijl de **Microsoft Automation** gebeurtenislogboek niet veel gebeurtenissen met details hulp bij het oplossen van problemen met de uitvoering van runbook, vindt u ten minste de resultaten van de runbooktaak.  

[Runbook uitvoer en berichten](automation-runbook-output-and-messages.md) worden verzonden naar Azure Automation van hybrid workers net als runbooktaken uitvoeren in de cloud.  Ook kunt u de stromen uitgebreid en voortgang van de dezelfde manier als voor andere runbooks.  

Als uw runbooks zijn niet voltooid, waardoor de taak overzicht de status van toont **onderbroken**, lees het artikel over probleemoplossing [Hybrid Runbook Worker: een runbooktaak wordt beëindigd met de status onderbroken ](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).   

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt om een runbook te starten, [een Runbook starten in Azure Automation](automation-starting-a-runbook.md).  
* Zie voor informatie over de verschillende procedures voor het werken met PowerShell en PowerShell Workflow-runbooks in Azure Automation met behulp van de teksteditor, [bewerken van een Runbook in Azure Automation](automation-edit-textual-runbook.md)
