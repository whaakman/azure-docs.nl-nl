---
title: Problemen oplossen met Azure Automation-Runbooks
description: Meer informatie over het oplossen van problemen met Azure Automation-runbooks
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 01/17/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 231dd3789a20b649efd99a6b88f6e429e2626bd3
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391330"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Fouten met runbooks oplossen

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatiefouten bij het werken met Azure Automation-runbooks

### <a name="sign-in-failed"></a>Scenario: Aanmelden bij Azure-Account is mislukt

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met de `Add-AzureAccount` of `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op als de naam van de referentie-asset is niet geldig of als de gebruikersnaam en het wachtwoord die u hebt gebruikt voor het instellen van het Automation-referentieasset ongeldig zijn.

#### <a name="resolution"></a>Oplossing

Om te bepalen wat er mis is, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat u geen speciale tekens heeft, met inbegrip van de **@** teken in de naam van Automation-referentie asset die u gebruikt om te verbinden met Azure.  
2. Controleer of u kunt de gebruikersnaam en het wachtwoord die zijn opgeslagen in de Azure Automation-referentie in uw lokale PowerShell ISE-editor gebruiken. U kunt doen. Controleer de gebruikersnaam en het wachtwoord juist zijn door het uitvoeren van de volgende cmdlets in PowerShell ISE:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Als de verificatie lokaal is mislukt, betekent dit dat u uw Azure Active Directory-referenties juist nog niet hebt ingesteld. Raadpleeg [zich verifiëren bij Azure met behulp van Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogbericht om op te halen van de Azure Active Directory-account juist ingesteld.  

4. Als het ziet als een tijdelijke fout eruit, kunt u de logica voor opnieuw proberen toe te voegen aan uw verificatie routine maken verifiëren krachtiger.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Scenario: Kan het Azure-abonnement vinden

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met de `Select-AzureSubscription` of `Select-AzureRmSubscription` cmdlets:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Fout

Deze fout treedt op als naam van het abonnement is niet geldig of als de Azure Active Directory-gebruiker die is bij het ophalen van details van het abonnement is niet geconfigureerd als een beheerder van het abonnement.

#### <a name="resolution"></a>Oplossing

Om te bepalen als u hebt geverifieerd naar Azure en toegang tot het abonnement dat u probeert hebben te selecteren, moet u de volgende stappen uitvoeren:  

1. Het script buiten Azure Automation om te controleren of dat deze werkt zelfstandige testen.
2. Zorg ervoor dat u uitvoert het `Add-AzureAccount` cmdlet voordat u de `Select-AzureSubscription` cmdlet. 
3. Voeg `Disable-AzureRmContextAutosave –Scope Process` aan het begin van het runbook. Dit zorgt ervoor dat alle referenties alleen voor de uitvoering van het huidige runbook gelden.
4. Als u nog steeds deze foutmelding ziet, wijzigt u de code door toe te voegen de **AzureRmContext** parameter volgende de `Add-AzureAccount` cmdlet en vervolgens de code wordt uitgevoerd.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Scenario: Verificatie bij Azure is mislukt omdat de multi-factor authentication is ingeschakeld

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout wanneer verificatie bij Azure met uw Azure gebruikersnaam en wachtwoord:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Oorzaak

Als u MFA op uw Azure-account hebt, kunt u een Azure Active Directory-gebruiker niet gebruiken voor verificatie op Azure. In plaats daarvan moet u een certificaat of een service-principal gebruiken om te worden geverifieerd bij Azure.

#### <a name="resolution"></a>Oplossing

Raadpleeg voor het gebruik van een certificaat met de klassieke Azure-implementatie-cmdlets voor model, [maken en toevoegen van een certificaat voor het beheren van Azure-services.](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Voor het gebruik van een service-principal met Azure Resource Manager-cmdlets, raadpleegt u [service-principal met behulp van Azure portal maken](../../active-directory/develop/howto-create-service-principal-portal.md) en [verifiëren van een service-principal met Azure Resource Manager.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="common-errors-when-working-with-runbooks"></a>Veelvoorkomende fouten bij het werken met runbooks

###<a name="child-runbook-object"></a>Onderliggend runbook foutmelding wanneer de uitvoerstroom objecten in plaats van eenvoudige gegevenstypen bevat

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het aanroepen van een childrunbook met de `-Wait` switch en de uitvoerstroom-object bevat:

```
Object reference not set to an instance of an object
```

#### <a name="cause"></a>Oorzaak

Er is een bekend probleem waarbij de [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) niet de uitvoerstroom correct verwerkt als deze objecten bevat.

#### <a name="resolution"></a>Oplossing

Om op te lossen dit het is raadzaam dat u in plaats daarvan een polling-logica implementeren en gebruiken de [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) cmdlet voor het ophalen van de uitvoer. Een voorbeeld van deze logica wordt gedefinieerd in het volgende voorbeeld.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

### <a name="task-was-cancelled"></a>Scenario: Het runbook is mislukt met de fout: Een taak is geannuleerd

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met een fout die vergelijkbaar is met het volgende voorbeeld:

```
Exception: A task was canceled.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door verouderde Azure-modules.

#### <a name="resolution"></a>Oplossing

Deze fout kan worden opgelost door uw Azure-modules bijwerken naar de nieuwste versie.

Klik in uw Automation-Account op **Modules**, en klikt u op **Update Azure-modules**. De update duurt ongeveer 15 minuten, één keer voltooid opnieuw uitvoeren van het runbook is mislukt. Zie voor meer informatie over het bijwerken van de modules, [Update Azure-modules in Azure Automation](../automation-update-azure-modules.md).

### <a name="runbook-auth-failure"></a>Scenario: Runbooks mislukken wanneer er sprake is van meerdere abonnementen

#### <a name="issue"></a>Probleem

Bij het uitvoeren van runbooks met `Start-AzureRmAutomationRunbook`, het runbook is mislukt voor het beheren van Azure-resources.

#### <a name="cause"></a>Oorzaak

Het runbook is niet de juiste context gebruikt bij het uitvoeren van.

#### <a name="resolution"></a>Oplossing

Als u werkt met meerdere abonnementen, kan de context van het abonnement worden verbroken tijdens het aanroepen van runbooks. Om ervoor te zorgen dat de context van het abonnement wordt doorgegeven aan de runbooks, voeg de `AzureRmContext` parameter voor de cmdlet en geeft u de context toe. Het is ook raadzaam om te gebruiken de `Disable-AzureRmContextAutosave` cmdlet met de **proces** bereik om ervoor te zorgen dat de referenties die u gebruiken alleen worden gebruikt voor het huidige runbook.

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Scenario: Het runbook is mislukt vanwege een ontbrekende cmdlet

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met een fout die vergelijkbaar is met het volgende voorbeeld:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. De module met de cmdlet is niet geïmporteerd in het automation-account
2. De module met de cmdlet is geïmporteerd, maar is verouderd

#### <a name="resolution"></a>Oplossing

Deze fout kan worden opgelost door een van de volgende taken voltooien:

Als de module een Azure-module is, Zie [het bijwerken van Azure PowerShell-modules in Azure Automation](../automation-update-azure-modules.md) voor informatie over het bijwerken van de modules in uw automation-account.

Als er geen afzonderlijke module, zorg ervoor dat de module in geïmporteerd in uw Automation-Account.

### <a name="job-attempted-3-times"></a>Scenario: Het begin van de runbook-taak drie keer is uitgevoerd, maar deze kan niet worden gestart elke keer

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. Limiet voor geheugen. De voorgeschreven limiet op hoeveel geheugen is toegewezen aan een Sandbox is gevonden op [Automation Servicelimieten](../../azure-subscription-service-limits.md#automation-limits). Een taak kan mislukken als er meer dan 400 MB aan geheugen.

2. Netwerk-Sockets. Azure-sandboxes geladen zijn beperkt tot 1000 gelijktijdige netwerk sockets, zoals beschreven op [Automation Servicelimieten](../../azure-subscription-service-limits.md#automation-limits).

3. Module die incompatibel is. Deze fout kan optreden als de module-afhankelijkheden niet juist zijn en als ze niet, uw runbook retourneert doorgaans een '-opdracht niet gevonden' of 'Kan niet binden parameter'-bericht.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Voorgestelde methoden voor het werk binnen de geheugenlimiet zijn de werkbelasting tussen meerdere runbooks splitsen, niet zo veel gegevens in het geheugen, om onnodige uitvoer schrijven in uw runbooks niet te verwerken of houd rekening met het aantal controlepunten u in uw PowerShell-werkstroom schrijven runbooks. U kunt de methode clear zoals `$myVar.clear()` om te wissen van de variabele en gebruik `[GC]::Collect()` om uit te voeren garbagecollection onmiddellijk zo beperkt u het geheugengebruik van het runbook tijdens runtime.

* Uw Azure-modules bijwerken met de volgende stappen [het bijwerken van Azure PowerShell-modules in Azure Automation](../automation-update-azure-modules.md).  

* Een andere oplossing is om uit te voeren van het runbook op een [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Hybrid Workers zijn niet beperkt door de limieten voor geheugen en het netwerk die Azure-sandboxes geladen zijn.

### <a name="fails-deserialized-object"></a>Scenario: Runbook is mislukt vanwege gedeserialiseerde object

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Oorzaak

Als uw runbook een PowerShell-werkstroom is, slaat deze complexe objecten in een gedeserialiseerde indeling om vast te leggen de status van uw runbook als de werkstroom wordt onderbroken.

#### <a name="resolution"></a>Oplossing

Een van de volgende drie oplossingen kunt dit probleem oplossen:

1. Als u complexe objecten van een cmdlet naar een andere sluizen bent, moet u deze cmdlets verpakken in een InlineScript.
2. De naam of de waarde die u nodig hebt van het complexe object in plaats van het doorgeven van het gehele object doorgegeven.
3. Een PowerShell-runbook gebruiken in plaats van een PowerShell Workflow-runbook.

### <a name="quota-exceeded"></a>Scenario: Runbook-taak is mislukt omdat het toegewezen quotum overschreden

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de fout:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer de taakuitvoering is groter dan het gratis quotum van 500 minuten voor uw account. Dit quotum is van toepassing op alle soorten taken worden uitgevoerd, zoals een taak testen, starten van een taak vanuit de portal, een taak wordt uitgevoerd met behulp van webhooks en plannen van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Zie voor meer informatie over de prijzen voor automatisering, [Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Oplossing

Als u gebruiken van meer dan 500 minuten per maand wordt verwerkt wilt, moet u uw abonnement wijzigen van de gratis laag naar de Basic-laag. U kunt upgraden naar de Basic-laag door te nemen van de volgende stappen uit:  

1. Meld u aan bij uw Azure-abonnement  
2. Selecteer het Automation-account dat u wilt upgraden  
3. Klik op **instellingen** > **prijzen**.
4. Klik op **inschakelen** op de pagina van beneden naar uw account bijwerken naar de **Basic** laag.

### <a name="cmdlet-not-recognized"></a>Scenario: Cmdlet is niet herkend bij het uitvoeren van een runbook

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de fout:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt wanneer de PowerShell-engine de cmdlet die u in uw runbook niet kan vinden. Dit kan zijn omdat de module met de cmdlet in het account ontbreekt, er een naamconflict geeft met de naam van een runbook wordt of de cmdlet ook in een andere module bestaat en de naam kan niet worden omgezet in Automation.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:  

* Controleer of u de cmdletnaam van de juist hebt ingevoerd.  
* Zorg ervoor dat de cmdlet bestaat in uw Automation-account en er zijn geen conflicten. Om te controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkingsmodus en zoeken naar de cmdlet die u wilt zoeken in de bibliotheek of uitvoeren `Get-Command <CommandName>`. Zodra u hebt gevalideerd die de cmdlet voor het account beschikbaar is en dat er geen naam conflicteert met andere cmdlets of runbooks, toe te voegen aan het canvas en zorg ervoor dat u een geldige parameter ingesteld in uw runbook.  
* Als er een naamconflict en de cmdlet beschikbaar in twee verschillende modules is, kunt u deze kunt oplossen met behulp van de volledig gekwalificeerde naam van de cmdlet. Bijvoorbeeld, kunt u **ModuleName\CmdletName**.  
* Als u bent de runbook on-premises wordt uitgevoerd in een hybrid worker-groep en zorg ervoor dat is de module en de cmdlet geïnstalleerd op de computer die als host fungeert voor de hybrid worker.

### <a name="long-running-runbook"></a>Scenario: Een langlopende runbook kan niet worden voltooid

#### <a name="issue"></a>Probleem

Uw runbook bevat in een **gestopt** status nadat het is uitgevoerd voor drie uur. De fout kan ook optreden:

```
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Dit gedrag is inherent aan Azure-sandboxes geladen vanwege de bewaking van 'Geoorloofd delen' van de processen in Azure Automation, waarmee automatisch een runbook wordt gestopt als deze meer dan drie uur wordt uitgevoerd. De status van een runbook dat na de termijn eerlijke-share gaat verschilt per runbooktype. PowerShell en Python-runbooks zijn ingesteld op een **gestopt** status. PowerShell Workflow-runbooks zijn ingesteld op **mislukt**.

#### <a name="cause"></a>Oorzaak

Het runbook werd uitgevoerd dan de limiet 3 uur is toegestaan door evenredige deel in een Sandbox met Azure.

#### <a name="resolution"></a>Oplossing

Een aanbevolen oplossing is om uit te voeren van het runbook op een [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md).

Hybrid Workers zijn niet beperkt door de [evenredige deel](../automation-runbook-execution.md#fair-share) 3 uur runbook beperken die Azure-sandboxes geladen zijn. Terwijl de Hybrid Runbook Workers zijn niet beperkt door de limiet van de evenredige deel 3 uur, runbooks die worden uitgevoerd op Hybrid Runbook Workers moeten nog steeds worden ontwikkeld ter ondersteuning van gedrag voor opnieuw opstarten als er onverwachte lokale infrastructuur problemen zijn.

Een andere optie is het optimaliseren van het runbook met het maken van [onderliggende runbooks](../automation-child-runbooks.md). Als uw runbook dezelfde functie op een aantal resources, zoals een databasebewerking op verschillende databases doorloopt, kunt u deze functie kunt verplaatsen naar een onderliggend runbook. Elk van deze onderliggende runbooks gelijktijdig in afzonderlijke processen uitgevoerd. Dit gedrag vermindert de totale hoeveelheid tijd voor het bovenliggende runbook om te voltooien.

De PowerShell-cmdlets waarmee het onderliggende runbook scenario zijn:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) -met deze cmdlet kunt u een runbook te starten en parameters doorgeven aan het runbook

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) -met deze cmdlet kunt u de taak de status van alle onderliggende controleren als er bewerkingen die worden uitgevoerd moeten nadat het onderliggende runbook is voltooid.

### <a name="expired webhook"></a>Scenario: Status: 400-Ongeldige aanvraag bij het aanroepen van een webhook

#### <a name="issue"></a>Probleem

Als u een webhook voor een Azure Automation-runbook aanroepen probeert, krijgt u de volgende fout.

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Oorzaak

De webhook die u probeert om aan te roepen is uitgeschakeld of is verlopen.

#### <a name="resolution"></a>Oplossing

Als de webhook is uitgeschakeld, kunt u de webhook via Azure portal opnieuw inschakelen. Wanneer een webhook is verlopen, wordt de webhook moet worden verwijderd en opnieuw worden gemaakt. U kunt alleen [vernieuwen van een webhook](../automation-webhooks.md#renew-webhook) als deze niet al is verlopen.

### <a name="429"></a>Scenario: 429: Het aantal verwerkte aanvragen is momenteel te groot. Probeer het opnieuw

#### <a name="issue"></a>Probleem

U het volgende foutbericht ontvangt bij het uitvoeren van de `Get-AzureRmAutomationJobOutput` cmdlet:

```
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Oorzaak

Deze fout kan optreden bij het ophalen van de uitvoer van een runbook met veel [uitgebreide streams](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Oplossing

Er zijn twee manieren deze fout op te lossen:

* Het runbook bewerken en verminder het aantal taakstromen die deze verzendt.
* Verminder het aantal stromen moet worden opgehaald als de cmdlet wordt uitgevoerd. Als u wilt volgen dit gedrag, kunt u de `-Stream Output` parameter voor de `Get-AzureRmAutomationJobOutput` alleen uitvoerstromen cmdlet om op te halen. 

## <a name="common-errors-when-importing-modules"></a>Veelvoorkomende fouten bij het importeren van modules

### <a name="module-fails-to-import"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd na het importeren

#### <a name="issue"></a>Probleem

Een module te importeren is mislukt of is geïmporteerd, maar geen cmdlets worden geëxtraheerd.

#### <a name="cause"></a>Oorzaak

Er zijn enkele veelvoorkomende redenen waarom dat een module niet met succes in Azure Automation importeren kan:

* De structuur komt niet overeen met de structuur die het Automation nodig heeft om te worden.
* De module is afhankelijk van een andere module die nog niet geïmplementeerd is naar uw Automation-account.
* De module ontbreekt de bijbehorende afhankelijkheden in de map.
* De `New-AzureRmAutomationModule` cmdlet voor het uploaden van de module wordt gebruikt en u niet de volledige opslagpad toegestaan of de module nog niet hebt geladen met behulp van een openbaar toegankelijke URL.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Zorg ervoor dat de module de volgende indeling heeft: ModuleName.Zip **->** ModuleName of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)
* Open het .psd1-bestand en zien of de module afhankelijkheden heeft. Als dit het geval is, moet u deze modules uploaden naar het Automation-account.
* Zorg ervoor dat eventuele waarnaar wordt verwezen, dll's zijn aanwezig in de modulemap.

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.