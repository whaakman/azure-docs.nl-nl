---
title: Oplossen van fouten onboarding updatebeheer, wijzigingen bijhouden en inventaris
description: Meer informatie over het oplossen van fouten met de updatebeheer, wijzigingen bijhouden en inventaris oplossingen onboarding
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: c11013c926e77447a69ce0dfe697fdda1ecc2b8c
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284843"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Problemen oplossen bij het onboarding-oplossingen

Fouten kunnen optreden tijdens het onboarding-oplossingen zoals updatebeheer of wijzigingen bijhouden en inventaris. In dit artikel beschrijft de verschillende fouten die optreden en hoe ze op te lossen.

## <a name="general-errors"></a>Algemene fouten

### <a name="computer-grou-query-format-error"></a>Scenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de opgeslagen zoekopdracht computer groepsquery gebruikt om u te richten op de oplossing is niet juist opgemaakt. 

#### <a name="cause"></a>Oorzaak

U hebt de query gewijzigd, of door het systeem zijn gewijzigd.

#### <a name="resolution"></a>Oplossing

U kunt de query voor deze oplossing en de oplossing, wordt de query reonboard verwijderen. De query worden gevonden in uw werkruimte onder **opgeslagen zoekopdrachten**. De naam van de query is **MicrosoftDefaultComputerGroup**, en de categorie van de query is de naam van de oplossing die is gekoppeld aan deze query. Als meerdere oplossingen zijn ingeschakeld, de **MicrosoftDefaultComputerGroup** meerdere keren weergegeven onder **opgeslagen zoekopdrachten**.

### <a name="policy-violation"></a>Scenario: PolicyViolation

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de implementatie is mislukt vanwege schending van een of meer beleidsregels.

#### <a name="cause"></a>Oorzaak 

Een beleid is dat wordt geblokkeerd door de bewerking niet voltooien.

#### <a name="resolution"></a>Oplossing

De oplossing implementeren, moet u rekening houden met het wijzigen van het opgegeven beleid. Er zijn veel verschillende soorten beleid die kunnen worden gedefinieerd, is de specifieke wijzigingen nodig zijn afhankelijk van het beleid dat is geschonden. Bijvoorbeeld, als er een beleid is gedefinieerd in een resourcegroep die geen toestemming hebben voor het wijzigen van de inhoud van bepaalde typen resources in die resourcegroep, kunt u bijvoorbeeld doen van de volgende:

* Verwijder het beleid kan worden overgeslagen.
* Probeer te onboarden naar een andere resourcegroep.
* Wijzig het beleid door, bijvoorbeeld:
  * Opnieuw die gericht is op het beleid op een specifieke bron (bijvoorbeeld bij een specifieke Automation-account).
  * Wijzigen van de set van resources dat beleid is geconfigureerd om te weigeren.

Controleer de meldingen in de rechterbovenhoek van de Azure-portal of Ga naar de resourcegroep waarin uw automation-account en selecteer **implementaties** onder **instellingen** om de mislukte weer te geven de implementatie. Voor meer informatie over Azure Policy bezoek: [overzicht van Azure Policy](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>MMA-extensies oplossen

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bij het implementeren van een oplossing voor een verscheidenheid aan gerelateerde resources worden geïmplementeerd. Een van deze resources is de Microsoft Monitoring Agent-extensie of Log Analytics-agent voor Linux. Dit zijn geïnstalleerd door de virtuele machine-Gastagent die verantwoordelijk is voor communicatie met de geconfigureerde Log Analytics-werkruimte, ten behoeve van hoger coördinatie van het downloaden van de binaire bestanden extensies voor virtuele machines en andere bestanden die de oplossing voor u onboarding zijn afhankelijk van nadat deze uitvoering is begonnen.
U doorgaans eerst op de hoogte van de MMA of Log Analytics-agent voor Linux-installatiefouten vanaf een melding wordt weergegeven in Notification hubs. Te klikken op de melding dat biedt meer informatie over de specifieke fout. Navigatie naar de resource resourcegroepen, en vervolgens naar het element implementaties binnen het bevat ook informatie over de fouten bij de implementatie die zijn opgetreden.
Installatie van de MMA of Log Analytics-agent voor Linux kan om verschillende redenen mislukken en de stappen voor het oplossen van deze fouten zijn afhankelijk van het probleem. Specifieke stappen volgt.

Het volgende gedeelte bevat verschillende problemen die optreden kunnen wanneer onboarding die leiden een fout opgetreden bij de implementatie van de MMA-extensie tot.

### <a name="webclient-exception"></a>Scenario: Een uitzondering is opgetreden tijdens een WebClient-aanvraag

De MMA-extensie op de virtuele machine is kan niet communiceren met externe bronnen en de implementatie mislukt.

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten die worden geretourneerd:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Oorzaak

Er zijn enkele mogelijke oorzaken voor deze fout:

* Er is een proxy is geconfigureerd in de virtuele machine, waarmee alleen bepaalde poorten.

* Een firewallinstelling is toegang tot de vereiste poorten en -adressen geblokkeerd.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat u de juiste poorten hebt en adressen voor communicatie openen. Zie voor een lijst met poorten en -adressen, [plannen van uw netwerk](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Scenario: De installatie is mislukt vanwege problemen met de tijdelijke omgeving

De installatie van de Microsoft Monitoring Agent-extensie is mislukt tijdens de implementatie vanwege een andere installatie of blokkeren van de installatie van de actie

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten kunnen worden geretourneerd:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Oorzaak

Er zijn enkele mogelijke oorzaken voor deze fout:

* Een andere installatie wordt uitgevoerd
* Het systeem opnieuw opstarten tijdens de sjabloonimplementatie is geactiveerd

#### <a name="resolution"></a>Oplossing

Deze fout is een tijdelijke fout in de natuur. De implementatie voor het installeren van de extensie opnieuw probeert.

### <a name="installation-timeout"></a>Scenario: Installatie van time-out

De installatie van de MMA-extensie is niet voltooid vanwege een time-out.

#### <a name="issue"></a>Probleem

Hier volgt een voorbeeld van een foutbericht die kan worden geretourneerd:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Oorzaak

Deze fout is omdat de virtuele machine wordt zwaar belast tijdens de installatie.

### <a name="resolution"></a>Oplossing

Poging tot de MMA-extensie installeren wanneer de virtuele machine minder belast wordt.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet wordt gezien of kan niet aan het oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
