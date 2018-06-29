---
title: Azure Automation Hybrid Runbook Worker
description: In dit artikel bevat informatie over het installeren en gebruiken van Hybrid Runbook Worker, is een functie van Azure Automation kunt u runbooks worden uitgevoerd op computers in uw lokale datacentrum of de cloudprovider.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: be79f0111cb569509cb05b24c99f86d4ca9534b0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063858"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Het automatiseren van bronnen in uw datacenter of de cloud met behulp van de Hybrid Runbook Worker

Runbooks in Azure Automation is mogelijk geen toegang krijgen tot bronnen in andere clouds of in uw on-premises omgeving omdat ze worden uitgevoerd op het Azure-cloud-platform. U kunt de hybride Runbook Worker-functie van Azure Automation voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen gebruiken. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd.

De volgende afbeelding ziet u deze functionaliteit:

![Overzicht van Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Elke Hybrid Runbook Worker is lid van een hybride Runbook Worker-groep die u opgeeft wanneer u de agent installeert. Een groep kan één agent bevatten, maar u kunt meerdere agents installeren in een groep voor hoge beschikbaarheid.

Wanneer u een runbook op een hybride Runbook Worker start, geeft u de groep die op wordt uitgevoerd. Elke werknemer in de groep worden opgevraagd Azure Automation om te zien of er geen taken beschikbaar zijn. Als een taak beschikbaar is, wordt het door de eerste worker is om de taak duurt. U kunt een bepaalde worker niet opgeven.

## <a name="install-a-hybrid-runbook-worker"></a>Een hybride Runbook Worker installeren

Het proces voor het installeren van een hybride Runbook Worker is afhankelijk van het besturingssysteem. De volgende tabel bevat koppelingen naar de methoden die u voor de installatie gebruiken kunt. 

Als u wilt installeren en configureren van een hybride Runbook Worker van Windows, kunt u twee methoden. De aanbevolen methode gebruikt een Automation-runbook om het proces van het configureren van een Windows-computer volledig automatiseren. De tweede methode volgt een stapsgewijze procedure om handmatig te installeren en configureren van de rol. Voor Linux-machines, moet u een pythonscript voor de installatie van de agent op de machine uitvoeren.

|OS  |Implementatietypen  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Handmatig](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden voor de Hybrid Runbook Worker-rol met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten. Voor meer informatie over het voorbereiden voor beheer met DSC, Zie [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).
>
>Als u inschakelt de [Update beheeroplossing](automation-update-management.md), elke computer die verbonden met uw Azure-logboekanalyse-werkruimte wordt automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing. De computer is echter niet geregistreerd bij de Hybrid Worker-groepen in uw Automation-account al gedefinieerd. De computer kan worden toegevoegd aan een hybride Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor de oplossing en het lidmaatschap van de Hybrid Runbook Worker. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van Hybrid Runbook Worker.

Controleer de [informatie voor het plannen van uw netwerk](#network-planning) voordat u begint met het implementeren van een hybride Runbook Worker. Nadat u de werknemer implementeren, controleren [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.

## <a name="remove-a-hybrid-runbook-worker"></a>Een hybride Runbook Worker verwijderen

U kunt een of meer Hybrid Runbook Workers verwijderen uit een groep of kunt u de groep, afhankelijk van uw vereisten. Als u wilt een hybride Runbook Worker verwijderen uit een on-premises computer, moet u de volgende stappen uitvoeren:

1. Ga in de Azure-portal naar uw Automation-account.
2. Onder **instellingen**, selecteer **sleutels** en noteer de waarden voor **URL** en **primaire toegangssleutel**. Deze informatie moet u voor de volgende stap.

### <a name="windows"></a>Windows

Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdracht. Gebruik de **-uitgebreide** overschakelen voor een gedetailleerd logboek van het verwijderingsproces.

```powershell
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Als verlopen machines uit de Hybrid Worker-groep verwijderen, gebruik het optionele `machineName` parameter.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Deze code verwijdert geen Microsoft Monitoring Agent van de computer, alleen de functionaliteit en de configuratie van de Hybrid Runbook Worker-rol.

## <a name="remove-a-hybrid-worker-group"></a>Een hybride Worker-groep verwijderen

Als u wilt een groep verwijdert, moet u eerst de hybride Runbook Worker verwijderen uit elke computer die lid is van de groep met de bovenstaande procedure. Voer de volgende stappen uit om de groep te verwijderen:

1. Open het Automation-account in de Azure portal.
1. Onder **procesautomatisering**, selecteer **Hybrid worker-groepen**. Selecteer de groep die u wilt verwijderen. De eigenschappenpagina voor de groep wordt weergegeven.

   ![De pagina Eigenschappen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Selecteer op de eigenschappenpagina voor de geselecteerde groep **verwijderen**. Een bericht vraagt u deze actie te bevestigen. Selecteer **Ja** als u zeker dat u wilt doorgaan.

   ![Bevestigingsbericht](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Dit kan enkele seconden duren. U kunt de voortgang bijhouden onder **Meldingen** in het menu.

## <a name="network-planning"></a>Het netwerk configureren

### <a name="hybrid-worker-role"></a>Hybride-werkrol

Voor de hybride Runbook Worker verbinding maken met en registreren met logboekanalyse moet hebben toegang tot het poortnummer en de URL's die in deze sectie worden beschreven. Deze toegang is in aanvulling op de [poorten en URL's die zijn vereist voor Microsoft Monitoring Agent](../log-analytics/log-analytics-agent-windows.md) verbinding maken met logboekanalyse.

Als u een proxyserver voor de communicatie tussen de agent en de Log Analytics-service gebruikt, moet u ervoor dat de juiste resources toegankelijk zijn. Als u een firewall gebruikt toegang tot het internet te beperken, moet u uw firewall om toegang te verlenen.

De volgende URL's en -poort zijn vereist voor de Hybrid Runbook Worker-rol om te communiceren met Automation:

* Poort: Alleen TCP 443 is vereist voor uitgaande toegang tot internet.
* Globale URL: *.azure-automation.net
* Globale URL van de VS Gov Virginia: *.azure automation.us
* Agent-service: https://\<workspaceId\>.agentsvc.azure-automation.net

Als u een Automation-account dat gedefinieerd voor een bepaald gebied hebt, kunt u de communicatie met dat regionale datacenter kunt beperken. De volgende tabel bevat de DNS-record voor elke regio:

| **Regio** | **DNS-record** |
| --- | --- |
| West-centraal VS | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Zuid-centraal VS |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| VS - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Canada - midden |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| West-Europa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Noord-Europa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Zuidoost-Azië |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Centraal-India |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan - oost |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australië - zuidoost |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Verenigd Koninkrijk Zuid | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| VS (overheid) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Voor een lijst met regio IP-adressen in plaats van de regionamen, downloadt u de [Azure Datacenter IP-adres](https://www.microsoft.com/download/details.aspx?id=41653) XML-bestand van het Microsoft Download Center.

> [!NOTE]
> Het Azure Datacenter IP-adres XML-bestand bevat de IP-adresbereiken die worden gebruikt in de Microsoft Azure-datacenters. Het bestand bevat compute, SQL en opslag bereiken.
>
>Een bijgewerkt bestand is wekelijks geplaatst. Het bestand weerspiegelt de huidige geïmplementeerde bereiken en toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacentra voor ten minste één week.
>
> Er is een goed idee om te downloaden van het nieuwe XML-bestand per week. Werk vervolgens, uw site correct services identificeren die worden uitgevoerd in Azure. Azure ExpressRoute gebruikers Houd er rekening mee dat dit bestand wordt gebruikt voor het bijwerken van de Border Gateway Protocol (BGP)-aankondiging van Azure ruimte in de eerste week van elke maand.

### <a name="update-management"></a>Updatebeheer

Naast de standaard-adressen en poorten die de hybride Runbook Worker is vereist, zijn de volgende adressen vereist voor het beheer van updates. Communicatie met deze adressen wordt via poort 443 uitgevoerd.

|Openbare Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

## <a name="troubleshoot"></a>Problemen oplossen

Zie voor meer informatie over het oplossen van uw hybride Runbook Workers, [probleemoplossing Hybrid Runbook Workers](troubleshoot/hybrid-runbook-worker.md#general)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving, [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md).