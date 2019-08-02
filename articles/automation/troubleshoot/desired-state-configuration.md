---
title: Problemen oplossen met Azure Automation desired state Configuration (DSC)
description: In dit artikel vindt u informatie over het oplossen van problemen met desired state Configuration (DSC)
services: automation
ms.service: automation
ms.subservice: ''
author: bobbytreed
ms.author: robreed
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6de348a19081eba685deafebd8a7c9b9d6556444
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688116"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Problemen met desired state Configuration (DSC) oplossen

In dit artikel vindt u informatie over het oplossen van problemen met desired state Configuration (DSC).

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Stappen voor het oplossen van problemen met desired state Configuration (DSC)

Wanneer er fouten zijn opgetreden bij het compileren of implementeren van configuraties in de Azure-status configuratie, zijn hier enkele stappen beschikbaar om u te helpen bij het vaststellen van het probleem.

1. **Zorg ervoor dat uw configuratie compileert op uw lokale computer:**  De Azure-status configuratie is gebaseerd op Power shell DSC. U kunt de documentatie voor de DSC-taal en syntaxis vinden in de [Power shell DSC-documenten](/powershell/dsc/overview/overview).

   Door de DSC-configuratie op uw lokale machine te compileren, kunt u veelvoorkomende fouten detecteren en oplossen, zoals:

   - **Ontbrekende modules**
   - **Syntaxis fouten**
   - **Logische fouten**
2. **DSC-logboeken weer geven op uw knoop punt:** Als uw configuratie is gecompileerd, maar mislukt als deze wordt toegepast op een knoop punt, kunt u gedetailleerde informatie vinden in de logboeken. Zie voor informatie over waar DSC-logboeken kunnen [worden gevonden de DSC-gebeurtenis logboeken](/powershell/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

   Futhermore, de [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) kan u helpen bij het parseren van gedetailleerde informatie uit de DSC-Logboeken. Als u contact opneemt met ondersteuning, moeten deze logboeken uw probleem dianose.

   U kunt **xDscDiagnostics** installeren op uw lokale computer met behulp van de instructies in [de module stabiele versie installeren](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

   Als u **xDscDiagnostics** wilt installeren op uw Azure-machine, kunt u [AZ VM Run-Command](/cli/azure/vm/run-command) of [invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)gebruiken. U kunt ook de **opdracht optie uitvoeren** vanuit de portal gebruiken door de stappen uit te voeren die zijn gevonden in [Power shell-scripts uitvoeren in uw Windows-VM met de opdracht uitvoeren](../../virtual-machines/windows/run-command.md).

   Voor informatie over het gebruik van **xDscDiagnostics**raadpleegt u [xDscDiagnostics gebruiken voor het analyseren van DSC](/powershell/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs)-logboeken en [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets)-cmdlets.
3. **Zorg ervoor dat uw knoop punten en Automation-werk ruimte de vereiste modules hebben:** De gewenste status configuratie is afhankelijk van de modules die op het knoop punt zijn geïnstalleerd.  Wanneer u Azure Automation status configuratie gebruikt, importeert u de benodigde modules in uw Automation-account met behulp van de stappen die worden beschreven in [modules importeren](../shared-resources/modules.md#import-modules). Configuraties kunnen ook afhankelijk zijn van specifieke versies van modules.  Zie [problemen met modules oplossen](shared-resources.md#modules)voor meer informatie.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Veelvoorkomende fouten bij het werken met de desired state Configuration (DSC)

### <a name="unsupported-characters"></a>Omstandigheden Een configuratie met speciale tekens kan niet worden verwijderd uit de portal

#### <a name="issue"></a>Probleem

Wanneer u probeert een DSC-configuratie te verwijderen uit de portal, ziet u de volgende fout:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Oorzaak

Deze fout is een tijdelijk probleem dat is gepland om te worden opgelost.

#### <a name="resolution"></a>Oplossing

* Gebruik de AZ-cmdlet Remove-AzAutomationDscConfiguration om de configuratie te verwijderen.
* De documentatie voor deze cmdlet is nog niet bijgewerkt.  Tot slot raadpleegt u de documentatie voor de module AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Omstandigheden Kan de DSC-agent niet registreren

#### <a name="issue"></a>Probleem

Wanneer u probeert een `Set-DscLocalConfigurationManager` of andere DSC-cmdlet uit te voeren, wordt de volgende fout weer gegeven:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt meestal veroorzaakt door een firewall, de computer zich achter een proxy server bevindt of andere netwerk fouten.

#### <a name="resolution"></a>Oplossing

Controleer of de computer toegang heeft tot de juiste eind punten voor Azure Automation DSC en probeer het opnieuw. Zie [netwerk planning](../automation-dsc-overview.md#network-planning) voor een lijst met poorten en adressen.

### <a name="failed-not-found"></a>Omstandigheden Het knoop punt heeft de status mislukt met de fout ' niet gevonden '

#### <a name="issue"></a>Probleem

Het knoop punt heeft een rapport met de status **mislukt** en bevat de volgende fout:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt doorgaans op wanneer het knoop punt is toegewezen aan een configuratie naam (bijvoorbeeld ABC) in plaats van een knooppunt configuratie naam (bijvoorbeeld ABC. Webserver).

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u het knoop punt met de naam ' knooppunt configuratie ' en niet de ' configuratie naam ' toewijst.
* U kunt een knooppunt configuratie toewijzen aan een knoop punt met behulp van Azure Portal of met een Power shell-cmdlet.

  * Als u een knooppunt configuratie wilt toewijzen aan een knoop punt met behulp van Azure Portal, opent u de pagina **DSC-knoop punten** , selecteert u een knoop punt en klikt u op de knop **knooppunt configuratie toewijzen** .
  * Met de cmdlet **set-AzureRmAutomationDscNode** kunt u een knooppunt configuratie toewijzen aan een knoop punt met behulp van de Power shell-cmdlet.

### <a name="no-mof-files"></a>Omstandigheden Er zijn geen knooppunt configuraties (MOF-bestanden) geproduceerd tijdens het compileren van een configuratie

#### <a name="issue"></a>Probleem

Uw DSC-compilatie taak wordt onderbroken met de volgende fout:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Oorzaak

Wanneer de expressie die volgt op `$null`het sleutel woord van het **knoop punt** in de DSC-configuratie, wordt geëvalueerd in, worden er geen knooppunt configuraties geproduceerd.

#### <a name="resolution"></a>Oplossing

Het probleem wordt opgelost met een van de volgende oplossingen:

* Zorg ervoor dat de expressie naast het sleutel woord van het **knoop punt** in de configuratie definitie niet evalueert naar $null.
* Als u ConfigurationData door geven tijdens het compileren van de configuratie, moet u ervoor zorgen dat u de verwachte waarden doorgeeft die de configuratie vereist van [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Omstandigheden De status van het DSC-knoop punt wordt vastgelopen

#### <a name="issue"></a>Probleem

De DSC-agent uitvoer:

```error
No instance found with given property values
```

#### <a name="cause"></a>Oorzaak

U hebt uw WMF-versie bijgewerkt en hebt WMI beschadigd.

#### <a name="resolution"></a>Oplossing

Volg de instructies in het artikel [bekende problemen en beperkingen van DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) om het probleem op te lossen.

### <a name="issue-using-credential"></a>Omstandigheden Kan geen referentie gebruiken in een DSC-configuratie

#### <a name="issue"></a>Probleem

Uw DSC-compilatie taak is onderbroken met de volgende fout:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Oorzaak

U hebt een referentie in een configuratie gebruikt, maar u hebt niet de juiste **ConfigurationData** om **PSDscAllowPlainTextPassword** in te stellen op True voor elke knooppunt configuratie.

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u de juiste **ConfigurationData** geeft om **PSDscAllowPlainTextPassword** in te stellen op True voor elke knooppunt configuratie die wordt vermeld in de configuratie. Zie [assets in azure Automation DSC](../automation-dsc-compile.md#assets)voor meer informatie.

### <a name="failure-processing-extension"></a>Omstandigheden Onboarding van de DSC-extensie, fout verwerkings uitbreiding

#### <a name="issue"></a>Probleem

Bij onboarding met behulp van de DSC-extensie treedt er een fout op met de volgende fout:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt doorgaans op wanneer aan het knoop punt een knooppunt configuratie naam wordt toegewezen die niet voor komt in de service.

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u het knoop punt toewijst met een knooppunt configuratie naam die exact overeenkomt met de naam in de service.
* U kunt ervoor kiezen om de naam van de configuratie van het knoop punt niet op te stellen, wat resulteert in het voorbereiden van het knoop punt, maar geen knooppunt configuratie toewijzen

### <a name="failure-linux-temp-noexec"></a>Omstandigheden Het Toep assen van een configuratie in Linux, treedt er een fout op met een algemene fout

#### <a name="issue"></a>Probleem

Bij het Toep assen van een configuratie in Linux treedt er een fout op met de volgende fout:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Oorzaak

Klanten hebben vastgesteld dat als de map/tmp-locatie is ingesteld op exec, de huidige versie van DSC geen configuraties kan Toep assen.

#### <a name="resolution"></a>Oplossing

* Verwijder de optie voor uitvoeren van de map/tmp-locatie.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een ondersteunings incident voor Azure opslaan. Ga naar de [ondersteunings site van Azure](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning verkrijgen**.
