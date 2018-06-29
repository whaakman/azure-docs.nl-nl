---
title: Het oplossen van problemen met Azure Automation Desired State Configuration (DSC)
description: Dit artikel bevat informatie over het oplossen van Desired State Configuration (DSC)
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5d2eae67fcff74a7016f7f6125e31a9c8c2bda97
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063889"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Problemen met Desired State Configuration (DSC)

In dit artikel bevat informatie over het oplossen van problemen met Desired State Configuration (DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Veelvoorkomende fouten bij het werken met Desired State Configuration (DSC)

### <a name="failed-not-found"></a>Scenario: Het knooppunt bevindt zich in de status mislukt met een 'Niet gevonden'-fout

#### <a name="issue"></a>Probleem

Het knooppunt heeft een rapport met **mislukt** status en met de fout:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt meestal op wanneer het knooppunt is toegewezen aan een configuratienaam (bijvoorbeeld ABC) in plaats van een knooppunt configuratienaam (bijvoorbeeld ABC. WebServer).

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u het knooppunt met 'knooppunt configuratienaam' en niet de 'configuratienaam' toewijst.
* U kunt een knooppuntconfiguratie toewijzen aan een knooppunt met Azure portal of met een PowerShell-cmdlet.

  * Om de configuratie van een knooppunt toewijzen aan een knooppunt met Azure portal, opent u de **DSC-knooppunten** pagina, selecteer vervolgens een knooppunt en klik op **toewijzen knooppuntconfiguratie** knop.  
  * Gebruiken om de configuratie van een knooppunt aan een knooppunt met behulp van PowerShell-cmdlet toewijst, **Set AzureRmAutomationDscNode** cmdlet

### <a name="no-mof-files"></a>Scenario: Er is geen knooppuntconfiguraties (MOF-bestanden) zijn gemaakt tijdens het compileren van een configuratie

#### <a name="issue"></a>Probleem

DSC-Compilatietaak wordt onderbroken met de fout:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Oorzaak

Wanneer de volgende expressie de **knooppunt** sleutelwoord in de DSC-configuratie wordt geëvalueerd naar `$null`, en vervolgens geen knooppuntconfiguraties worden geproduceerd.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen oplossen het probleem:

* Zorg ervoor dat de expressie naast de **knooppunt** -sleutelwoord in de definitie van de configuratie is niet naar $null evalueren.
* Als u ConfigurationData doorgeeft bij het compileren van de configuratie, zorgt u ervoor dat u de verwachte waarden die vereist dat de configuratie van doorgeeft [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Scenario: Het rapport DSC-knooppunt wordt de status 'wordt uitgevoerd' vastgelopen

#### <a name="issue"></a>Probleem

De uitvoer van de DSC-agent:

```
No instance found with given property values
```

#### <a name="cause"></a>Oorzaak

U hebt uw versie van WMF bijgewerkt en WMI hebt beschadigd.

#### <a name="resolution"></a>Oplossing

Om op te lossen de probleem Volg de instructies in de [DSC bekende problemen en beperkingen](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) artikel.

### <a name="issue-using-credential"></a>Scenario: Een kan referenties niet gebruiken in een DSC-configuratie

#### <a name="issue"></a>Probleem

DSC-Compilatietaak is onderbroken met de fout:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Oorzaak

U hebt een referentie gebruikt in een configuratie maar geen juiste **ConfigurationData** instellen **PSDscAllowPlainTextPassword** op true voor de configuratie van elk knooppunt.

#### <a name="resolution"></a>Oplossing

* Zorg ervoor dat u in de juiste doorgeven **ConfigurationData** instellen **PSDscAllowPlainTextPassword** op true voor de configuratie van elk knooppunt vermeld in de configuratie. Zie voor meer informatie [activa in Azure Automation DSC](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Volgende stappen

Als u hebt uw probleem niet wordt weergegeven of niet uw probleem kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een incident voor ondersteuning van Azure kunt opslaan. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.