---
title: Het oplossen van problemen met Azure Automation Desired State Configuration (DSC)
description: Dit artikel bevat informatie over het oplossen van Desired State Configuration (DSC)
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a2ec36a99b2940fa662b0d9bd16b06777684db2f
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448055"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Troubleshoot Desired State Configuration (DSC)

In dit artikel bevat informatie over het oplossen van problemen met Desired State Configuration (DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Veelvoorkomende fouten bij het werken met Desired State Configuration (DSC)

### <a name="unsupported-characters"></a>Scenario: Een configuratie met speciale tekens kan niet worden verwijderd uit de portal

#### <a name="issue"></a>Probleem

Wanneer u probeert een DSC-configuratie verwijderen uit de portal, ziet u de volgende fout:

```
An error occured while deleteing the DSC configuration '<name>'.  Error-details: The arguement configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Oorzaak

Dit is een tijdelijk probleem dat is gepland om te worden omgezet.

#### <a name="resolution"></a>Oplossing

* Gebruik de Az-Cmdlet "Remove-AzAutomationDscConfiguration" te verwijderen van de configuratie.
* De documentatie voor deze cmdlet is nog niet bijgewerkt.  Raadpleeg de documentatie voor de AzureRM-module tot die tijd.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-not-found"></a>Scenario: Knooppunt zich in de status mislukt met een 'Niet gevonden'-fout

#### <a name="issue"></a>Probleem

Het knooppunt heeft een rapport met **mislukt** status en met de fout:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt meestal op wanneer het knooppunt is toegewezen aan een configuratienaam (bijvoorbeeld ABC) in plaats van een naam van de knooppuntconfiguratie (bijvoorbeeld ABC. WebServer).

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u het knooppunt met 'naam van het configuratieknooppunt' en niet de 'configuratienaam' toewijst.
* U kunt een knooppuntconfiguratie toewijzen aan een knooppunt met behulp van Azure portal of met een PowerShell-cmdlet.

  * Als u wilt een knooppuntconfiguratie toewijzen aan een knooppunt met behulp van Azure portal, opent u de **DSC-knooppunten** pagina, selecteer vervolgens een knooppunt en klik op **knooppuntconfiguratie toewijzen** knop.  
  * Als u wilt een knooppuntconfiguratie toewijzen aan een knooppunt met behulp van PowerShell-cmdlet, gebruiken **Set AzureRmAutomationDscNode** cmdlet

### <a name="no-mof-files"></a>Scenario: Geen knooppuntconfiguraties (MOF-bestanden) zijn gemaakt tijdens het compileren van een configuratie

#### <a name="issue"></a>Probleem

Uw DSC-Compilatietaak onderbreekt met de fout:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Oorzaak

Wanneer de volgende expressie de **knooppunt** sleutelwoord in de DSC-configuratie wordt geëvalueerd als `$null`, en vervolgens geen knooppuntconfiguraties worden geproduceerd.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Zorg ervoor dat de expressie naast de **knooppunt** sleutelwoord in de definitie van de configuratie is niet naar $null evalueren.
* Als u ConfigurationData doorgeeft bij het compileren van de configuratie, zorgt u ervoor dat u de verwachte waarden die de configuratie is vereist van doorgeeft [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Scenario: Het rapport DSC-knooppunt wordt de status 'wordt uitgevoerd' vastgelopen

#### <a name="issue"></a>Probleem

De uitvoer van de DSC-agent:

```
No instance found with given property values
```

#### <a name="cause"></a>Oorzaak

U uw versie van WMF een upgrade hebt uitgevoerd en WMI beschadigd zijn.

#### <a name="resolution"></a>Oplossing

Om op te lossen de probleem-Volg de instructies in de [DSC bekende problemen en beperkingen](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) artikel.

### <a name="issue-using-credential"></a>Scenario: Kan niet een referentie gebruiken in een DSC-configuratie

#### <a name="issue"></a>Probleem

Uw DSC-Compilatietaak is onderbroken met de fout:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Oorzaak

U hebt een referentie gebruikt in een configuratie maar is niet juiste **ConfigurationData** om in te stellen **PSDscAllowPlainTextPassword** op ' True ' voor de knooppuntconfiguratie van elk.

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u in de juiste doorgeven **ConfigurationData** om in te stellen **PSDscAllowPlainTextPassword** op ' True ' voor de configuratie van elk knooppunt die worden vermeld in de configuratie. Zie voor meer informatie, [assets in Azure Automation DSC](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet wordt gezien of kan niet aan het oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
