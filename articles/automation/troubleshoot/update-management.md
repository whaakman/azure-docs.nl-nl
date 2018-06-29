---
title: Fouten met updatebeheer oplossen
description: Meer informatie over het oplossen van problemen met updatebeheer
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b77d1210ff48a4bd30834fcbad64173bf77b1290
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063881"
---
# <a name="troubleshooting-issues-with-update-management"></a>Het oplossen van problemen met updatebeheer

Dit artikel wordt beschreven oplossingen voor het oplossen van problemen die optreden kunnen wanneer u updatebeheer

## <a name="windows"></a>Windows

Als u problemen ondervindt tijdens een poging om vrij te geven van de oplossing op een virtuele machine, controleert u de **Operations Manager** gebeurtenislogboek onder **toepassings- en servicelogboeken** op de lokale computer voor gebeurtenissen met de gebeurtenis-ID **4502** en gebeurtenis-bericht met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

De volgende sectie worden specifieke foutberichten en een mogelijke oplossing voor elk gemarkeerd. Zie voor andere onboarding problemen, [oplossing onboarding oplossen](onboarding.md).

### <a name="machine-already-registered"></a>Scenario: Machine is al geregistreerd bij een ander account

#### <a name="issue"></a>Probleem

U ontvangt het volgende foutbericht weergegeven:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Oorzaak

De machine is al vrijgegeven aan een andere werkruimte om updates te beheren.

#### <a name="resolution"></a>Oplossing

Voer het opruimen van de oude artefacten op de machine door [verwijderen van de hybrid runbook groep](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) en probeer het opnieuw.

### <a name="machine-unable-to-communicate"></a>Scenario: De computer is kan niet communiceren met de service

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten weergegeven:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Oorzaak

Mogelijk zijn er een proxy, de gateway of de firewall blokkeert netwerkcommunicatie.

#### <a name="resolution"></a>Oplossing

Bekijk uw netwerken en zorg ervoor dat de juiste poorten en adressen zijn toegestaan. Zie [vereisten](../automation-hybrid-runbook-worker.md#network-planning), voor een lijst met poorten en adressen die zijn vereist voor het beheer van updates en hybride Runbook Workers.

### <a name="unable-to-create-selfsigned-cert"></a>Scenario: Kan geen zelfondertekend certificaat maken

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten weergegeven:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Oorzaak

De hybride Runbook Worker kon niet worden een zelfondertekend certificaat genereren

#### <a name="resolution"></a>Oplossing

Controleer of het systeem-account leestoegang heeft tot de map **C:\ProgramData\Microsoft\Crypto\RSA** en probeer het opnieuw.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Scenario: De Update-uitvoering niet kan worden gestart

#### <a name="issue"></a>Probleem

Een update wordt uitgevoerd niet starten op een Linux-computer.

#### <a name="cause"></a>Oorzaak

De Linux hybride Worker is beschadigd.

#### <a name="resolution"></a>Oplossing

Maak een kopie van de volgende logboekbestanden en voor het oplossen van problemen worden bewaard:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Scenario: Update-uitvoering wordt gestart, maar er een fout optreedt

#### <a name="issue"></a>Probleem

Een update-uitvoering wordt gestart, maar er een fout optreedt tijdens de uitvoering.

#### <a name="cause"></a>Oorzaak

Mogelijke oorzaken zijn:

* Pakketbeheer is niet in orde
* Specifieke pakketten kunnen verstoren patchen in de cloud
* Andere redenen

#### <a name="resolution"></a>Oplossing

Als er fouten optreden tijdens een update uitgevoerd nadat deze is gestart met succes op Linux, Controleer de uitvoer van de geïnfecteerde computer in de taak. Misschien vindt u specifieke foutberichten van uw machine Pakketbeheer die u kunt onderzoeken en actie ondernemen. Updatebeheer is vereist voor de package manager gezond voor geslaagde update-implementaties.

In sommige gevallen kunnen updates pakket bijwerken Management zo wordt voorkomen dat een update-implementatie voltooid verstoren. Als dat wordt weergegeven, hebt u deze pakketten uitsluiten van toekomstige update wordt uitgevoerd of ze handmatig installeren zelf.

Als u een probleem met een patch niet kunt oplossen, maakt u een kopie van het volgende logboekbestand en handhaven **voordat** de implementatie van de volgende update voor het oplossen van problemen wordt gestart:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Volgende stappen

Als u hebt uw probleem niet wordt weergegeven of niet uw probleem kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een incident voor ondersteuning van Azure kunt opslaan. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.