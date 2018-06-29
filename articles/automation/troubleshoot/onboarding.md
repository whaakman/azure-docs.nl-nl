---
title: Problemen oplossen met fouten voorbereiding, updatebeheer, bijhouden en inventarisatie
description: Meer informatie over het voorbereiden op fouten met de Update Management, bijhouden en oplossingen voor inventarisatie oplossen
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 044cb56b8991a1eb2dd6a1d35be621f2ffab3250
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063810"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Fouten oplossen wanneer onboarding oplossingen

Er kan fouten optreden bij onboarding oplossingen zoals updatebeheer of bijhouden en inventaris. In dit artikel beschrijft de verschillende fouten die optreden en hoe deze op te lossen.

## <a name="general-errors"></a>Algemene fouten

### <a name="computer-grou-query-format-error"></a>Scenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de opgeslagen zoekopdracht computer groepsquery gebruikt voor het doel van de oplossing is niet de juiste indeling. 

#### <a name="cause"></a>Oorzaak

U hebt de query gewijzigd, of door het systeem is gewijzigd.

#### <a name="resolution"></a>Oplossing

U kunt de query voor deze oplossing en de oplossing wordt de query reonboard verwijderen. De query kan worden gevonden in uw werkruimte onder **opgeslagen zoekopdrachten**. De naam van de query is **MicrosoftDefaultComputerGroup**, en de categorie van de query is de naam van de oplossing die is gekoppeld aan deze query. Als meerdere oplossingen zijn ingeschakeld, de **MicrosoftDefaultComputerGroup** ziet u meerdere keren onder **opgeslagen zoekacties**.

### <a name="policy-violation"></a>Scenario: PolicyViolation

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de implementatie is mislukt vanwege schending van een of meer beleidsregels.

#### <a name="cause"></a>Oorzaak 

Een beleid is dat wordt geblokkeerd door de bewerking niet voltooien.

#### <a name="resolution"></a>Oplossing

Om de oplossing implementeren, moet u rekening houden met het opgegeven beleid wijzigen. De specifieke wijzigingen die zijn vereist als er veel verschillende typen beleidsregels die kunnen worden gedefinieerd zijn, zijn afhankelijk van het beleid dat is geschonden. Bijvoorbeeld als een beleid is gedefinieerd voor een resourcegroep die geen toegang tot de inhoud van bepaalde soorten resources in die resourcegroep wijzigen, kunt u bijvoorbeeld doen het volgende:

* Het beleid helemaal verwijderen.
* Probeer om vrij te geven naar een andere resourcegroep.
* Herzien het beleid door, bijvoorbeeld:
  * Opnieuw als doel het beleid op een specifieke bron (bijvoorbeeld een specifieke Automation-account).
  * Wijzigen van de set van resources die het beleid is geconfigureerd om te weigeren.

Controleer de meldingen in de rechterbovenhoek van de Azure portal of navigeer naar de resourcegroep waarin uw automation-account en selecteer **implementaties** onder **instellingen** om de mislukte weer te geven de implementatie. Voor meer informatie over Azure beleid bezoek: [overzicht van Azure beleid](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>De extensie MMA fouten

Bij het implementeren van een oplossing voor tal van gerelateerde resources worden geïmplementeerd. Een van deze resources is de extensie van Microsoft Monitoring Agent of de OMS-Agent voor Linux. Dit zijn extensies van virtuele Machine is geïnstalleerd door de virtuele machine Guest-Agent die verantwoordelijk is voor communicatie met de geconfigureerde werkruimte Operations Management Suite (OMS) omwille van de latere coördinatie van het downloaden van de binaire bestanden en andere bestanden die de oplossing zijn van onboarding afhankelijk eenmaal wordt uitgevoerd.
U doorgaans waarnemen eerst MMA of OMS-Agent voor Linux-installatiefouten vanaf een melding weergegeven in de Hub meldingen. Te klikken op deze kennisgeving biedt meer informatie over de specifieke fout. Navigatie naar de resourcegroepen resource en vervolgens naar het element implementaties binnen het biedt ook informatie over de fouten bij de implementatie die is opgetreden.
Installatie van de MMA of OMS-Agent voor Linux kan om diverse redenen mislukken en de stappen voor het oplossen van deze oorzaken variëren, afhankelijk van het probleem. Specifieke stappen volgt.

De volgende sectie worden verschillende problemen die optreden kunnen bij het voorbereiden die ertoe leiden dat een fout opgetreden bij de implementatie van de extensie MMA.

### <a name="webclient-exception"></a>Scenario: Een uitzondering opgetreden tijdens een WebClient-aanvraag

De extensie MMA op de virtuele machine is kan niet communiceren met externe bronnen en de implementatie mislukt.

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten die worden geretourneerd:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Oorzaak

Enkele mogelijke oorzaken voor deze fout zijn:

* Er is een proxy is geconfigureerd in de virtuele machine, waarmee alleen bepaalde poorten.

* Een firewallinstelling is toegang tot de vereiste poorten en adressen geblokkeerd.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat u de juiste poorten hebt en adressen voor communicatie open. Zie voor een lijst met poorten en adressen [plannen van uw netwerk](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Scenario: De installatie is mislukt vanwege omgevingsproblemen met de tijdelijke

De installatie van de uitbreiding voor Microsoft Monitoring Agent is mislukt tijdens de implementatie als gevolg van een andere installatie of blokkeren van de installatie van de actie

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten kunnen worden geretourneerd:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Oorzaak

Enkele mogelijke oorzaken voor deze fout zijn:

* Een andere installatie wordt uitgevoerd
* Het systeem opnieuw opstarten tijdens de sjabloonimplementatie van de is geactiveerd

#### <a name="resolution"></a>Oplossing

Deze fout is een tijdelijke fout in aard. Probeer de implementatie voor het installeren van de extensie.

### <a name="installation-timeout"></a>Scenario: Installatie time-out

De installatie van de MMA-extensie is niet voltooid vanwege een time-out.

#### <a name="issue"></a>Probleem

Hier volgt een voorbeeld van een foutmelding die kan worden geretourneerd:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Oorzaak

Dit is een fout omdat de virtuele machine wordt onder een zware belasting tijdens de installatie.

### <a name="resolution"></a>Oplossing

Probeert te installeren van de extensie MMA wanneer de virtuele machine een lagere belast is.

## <a name="next-steps"></a>Volgende stappen

Als u hebt uw probleem niet wordt weergegeven of niet uw probleem kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een incident voor ondersteuning van Azure kunt opslaan. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
