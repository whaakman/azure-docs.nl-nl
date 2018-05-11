---
title: Azure Automation Hybrid Runbook Workers
description: In dit artikel bevat informatie over het installeren en gebruiken van Hybrid Runbook Worker die is een functie van Azure Automation kunt u runbooks uitvoeren op machines in uw lokale datacentrum of de cloudprovider.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 26762a91ddad080f698368c1a74948a5678ef75a
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatiseren van bronnen in uw datacenter of de cloud met Hybrid Runbook Worker

Runbooks in Azure Automation kan wellicht geen toegang krijgen tot bronnen in andere clouds of in uw on-premises-omgeving, omdat ze worden uitgevoerd in de Azure-cloud. De Hybrid Runbook Worker-functie van Azure Automation kunt u direct op de computer die als host fungeert voor de rol en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen runbooks worden uitgevoerd. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd.

Deze functionaliteit wordt geïllustreerd in de volgende afbeelding:

![Overzicht van hybride Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

## <a name="hybrid-runbook-worker-groups"></a>Hybrid Runbook Worker-groepen

Elke Hybrid Runbook Worker is lid van een hybride Runbook Worker-groep die u opgeeft wanneer u de agent installeert. Een groep kan één agent bevatten, maar u kunt meerdere agents installeren in een groep voor hoge beschikbaarheid.

Wanneer u een runbook op een hybride Runbook Worker start, geeft u de groep die op wordt uitgevoerd. Elke werknemer in de groep worden opgevraagd Azure Automation om te zien of er geen taken beschikbaar zijn. Als er een taak beschikbaar vervolgens duurt de eerste worker is om de taak het. U kunt een bepaalde worker niet opgeven.

## <a name="installing-a-hybrid-runbook-worker"></a>Een hybride Runbook Worker installeren

Het proces voor het installeren van een hybride Runbook worker is verschillend, afhankelijk van het besturingssysteem. De volgende tabel bevat koppelingen naar de verschillende methoden die u gebruiken kunt voor het installeren van een hybride Runbook Worker. Als u wilt installeren en configureren van een hybride Runbook Worker van Windows, zijn er twee methoden beschikbaar. De aanbevolen methode gebruikt een Automation-runbook om het proces dat is vereist voor het configureren van een Windows-computer volledig te automatiseren. De tweede methode volgt een stapsgewijze procedure om handmatig te installeren en configureren van de rol. Voor Linux-machines uitvoeren u een pythonscript voor de installatie van de agent op de machine

|OS  |Implementatietypen  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Handmatig](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden de hybride Runbook Worker-rol met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten. Voor meer informatie over het voorbereiden voor beheer met DSC, Zie [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).
>
>Als u inschakelt de [Update beheeroplossing](automation-update-management.md), elke computer die is verbonden met uw werkruimte voor logboekanalyse is automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing. Het is echter niet geregistreerd bij de Hybrid Worker-groepen in uw Automation-account al gedefinieerd. De computer kan worden toegevoegd aan een hybride Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor de oplossing en de hybride Runbook Worker-groepslidmaatschap gebruikt. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.

Controleer de [informatie voor het plannen van uw netwerk](#network-planning) voordat u begint met het implementeren van een hybride Runbook Worker. Nadat u hebt een runbook worker is geïmplementeerd, controleren [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.

## <a name="removing-hybrid-runbook-worker"></a>Hybride Runbook Worker verwijderen

U kunt een of meer Hybrid Runbook Workers verwijderen uit een groep of kunt u de groep, afhankelijk van uw vereisten. Als u wilt een hybride Runbook Worker verwijderen uit een on-premises computer, moet u de volgende stappen uitvoeren:

1. Ga in de Azure-portal naar uw Automation-account.
2. Van de **instellingen** blade Selecteer **sleutels** en noteer de waarden voor veld **URL** en **primaire toegangssleutel**. Deze informatie moet u voor de volgende stap.

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
> Microsoft Monitoring Agent wordt niet verwijderd van de computer, alleen de functionaliteit en de configuratie van de Hybrid Runbook Worker-rol.

## <a name="remove-hybrid-worker-groups"></a>Hybrid Worker-groepen verwijderen

Voor een groep verwijdert, moet u eerst de hybride Runbook Worker verwijderen uit elke computer die lid is van de groep met de bovenstaande procedure en voer de volgende stappen uit om de groep te verwijderen.

1. Open het Automation-account in de Azure portal.
1. Onder **procesautomatisering**, selecteer **Hybrid worker-groepen**. Selecteer de groep die u wilt verwijderen. Na het selecteren van de specifieke groep de **Hybrid worker-groep** eigenschappenpagina wordt weergegeven.

   ![Pagina voor hybride Runbook Worker-groep](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Klik op de eigenschappenpagina voor de geselecteerde groep **verwijderen**. Selecteer een bericht weergegeven waarin u deze actie te bevestigen **Ja** als u zeker dat u wilt doorgaan.

   ![Dialoogvenster voor bevestiging groep verwijderen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu.

## <a name="network-planning"></a>Het netwerk configureren

### <a name="hybrid-worker-role"></a>Hybride-werkrol

Voor de hybride Runbook Worker verbinding maken met en registreren met logboekanalyse moet hebben toegang tot het poortnummer en de URL's die in deze sectie worden beschreven. Dit is in aanvulling op de [poorten en URL's die zijn vereist voor Microsoft Monitoring Agent](../log-analytics/log-analytics-agent-windows.md) verbinding maken met logboekanalyse.

Als u een proxyserver voor de communicatie tussen de agent en de Log Analytics-service gebruikt, moet u ervoor dat de juiste resources toegankelijk zijn. Als u een firewall gebruikt toegang tot het internet te beperken, moet u uw firewall om toegang te verlenen.

De volgende URL's en -poort zijn vereist voor de Hybrid Runbook Worker-rol om te communiceren met Automation:

* Poort: Alleen TCP 443 is vereist voor uitgaande toegang tot internet.
* Globale URL: *.azure-automation.net
* Globale URL van de VS Gov Virginia: *.azure automation.us
* Agent-Service: https://\<workspaceId\>.agentsvc.azure-automation.net

Als u een Automation-account dat gedefinieerd voor een bepaald gebied hebt, kunt u de communicatie met dat regionale datacenter kunt beperken. De volgende tabel bevat de DNS-record voor elke regio.

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
> Het Azure Datacenter IP-adres XML-bestand bevat de IP-adresbereiken die worden gebruikt in de Microsoft Azure-datacenters. COMPUTE, SQL en opslag bereiken zijn opgenomen in het bestand.
>
>Een bijgewerkt bestand is wekelijks geplaatst. Het bestand weerspiegelt de huidige geïmplementeerde bereiken en toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacentra voor ten minste één week.
>
> Er is een goed idee om te downloaden van het nieuwe XML-bestand per week. Werk vervolgens, uw site correct services identificeren die worden uitgevoerd in Azure. Azure ExpressRoute gebruikers Houd er rekening mee dat dit bestand moet worden gebruikt voor het bijwerken van de Border Gateway Protocol (BGP)-aankondiging van Azure ruimte de eerste week van elke maand.

### <a name="update-management"></a>Updatebeheer

Naast de standaard-adressen en poorten die de hybride Runbook Worker is vereist, zijn de volgende adressen vereist voor het beheer van updates. Communicatie met deze adressen wordt via poort 443 uitgevoerd.

* *.ods.opinsights.azure.com
* *.oms.opinsights.azure.com
* ods.systemcenteradvisor.com
* *.blob.core.windows.net/

## <a name="troubleshooting"></a>Problemen oplossen

De hybride Runbook Worker is afhankelijk van de Microsoft Monitoring Agent te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

1. De hybride worker zich achter een proxy of firewall.

   Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.

2. De computer die de hybride worker op wordt uitgevoerd heeft minder dan de minimale hardwarevereisten.

   Computers met de hybride Runbook Worker aan de minimale hardwarevereisten voldoen voordat u deze als host voor deze functie. Gebruik anders, afhankelijk van het Resourcegebruik van andere achtergrondprocessen en conflicten veroorzaakt door runbooks tijdens het uitvoeren van de computer wordt overbelast en ervoor zorgen dat de runbook-taak vertragingen of time-outs.

   Controleer de computer moeten worden uitgevoerd van de Hybrid Runbook Worker-functie voldoet aan de minimale hardwarevereisten. Zo ja, monitor CPU en geheugen gebruik om te bepalen een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en vensters. Als er geheugen of CPU-belasting, dit kan duiden op hoeft bij te werken of toevoegen van extra processors of geheugen voor het adres van de resource-knelpunt en los de fout te verhogen. Selecteer een andere berekeningsresource die de minimum vereisten en schaal ondersteunen kan wanneer werkbelasting eisen duiden op dat een verhoging is nodig.

3. De service Microsoft Monitoring Agent wordt niet uitgevoerd.

   Als de Microsoft Monitoring Agent Windows-service niet wordt uitgevoerd, dit voorkomt dat de hybride Runbook Worker communicatie met Azure Automation. Controleer of de agent wordt uitgevoerd met de volgende opdracht in PowerShell: `get-service healthservice`. Als de service wordt gestopt, voert u de volgende opdracht in PowerShell om de service te starten: `start-service healthservice`.

4. In de **toepassingen en Services Logs\Operations Manager** gebeurtenislogboek, ziet u gebeurtenis 4502 en EventMessage met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**met de volgende beschrijving: *het certificaat dat is doorgegeven door de service \<wsid\>. oms.opinsights.azure.com is niet uitgegeven door een certificeringsinstantie die wordt gebruikt voor Microsoft-services. Neem contact op met de netwerkbeheerder om te controleren of er een proxy die TLS/SSL-communicatie wordt onderschept worden uitgevoerd. Artikel KB3126513 bevat extra informatie over probleemoplossing voor problemen met de netwerkverbinding.*
    Dit kan worden veroorzaakt door uw proxy- of -firewall blokkeert communicatie met Microsoft Azure. Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.

Logboeken worden lokaal opgeslagen op elke worker hybride op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. U kunt controleren of er zijn waarschuwingen of fouten die zijn geschreven naar de **toepassingen en Services Logs\Microsoft-SMA\Operations** en **toepassingen en Services Logs\Operations Manager** in het gebeurtenislogboek die duidt dit op een verbindings- of andere probleem met betrekking tot de voorbereiding van de rol aan Azure Automation of probleem tijdens het normale bewerkingen uitvoeren.

Zie voor aanvullende stappen over het oplossen van problemen met updatebeheer [updatebeheer - probleemoplossing](automation-update-management.md#troubleshooting)

## <a name="next-steps"></a>Volgende stappen

Bekijk [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.
