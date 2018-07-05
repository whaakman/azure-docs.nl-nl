---
title: Controleer metrische gegevens voor Azure Analysis Services-server | Microsoft Docs
description: Informatie over het bewaken van metrische gegevens Analysis Services-server een in Azure portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 608323c467e0106af816c3432dec24090a9a9599
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442930"
---
# <a name="monitor-server-metrics"></a>Metrische servergegevens bewaken

Analyseservices biedt metrische gegevens om u te helpen bij het controleren van de prestaties en status van uw servers. Controleer bijvoorbeeld geheugen en CPU-gebruik, het aantal clientverbindingen en het verbruik van de resource. Dezelfde controle framework van de analyseservices wordt gebruikt als de meeste andere Azure-services. Zie voor meer informatie, [metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Als u meer gedetailleerde diagnostische gegevens, prestaties, bijhouden en trends te identificeren voor meerdere serviceresources in een resourcegroep of abonnement, gebruikt u [Azure Monitor](https://azure.microsoft.com/services/monitor/). Azure Monitor (service) kan leiden tot een factureerbare service.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Voor het bewaken van metrische gegevens voor een Analysis Services-server

1. Selecteer in Azure portal, **metrische gegevens**.

    ![Bewaken in Azure Portal](./media/analysis-services-monitor/aas-monitor-portal.png)

2. In **beschikbare metrische gegevens**, selecteert u de metrische gegevens om op te nemen in de grafiek. 

    ![Monitor-grafiek](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>
## <a name="server-metrics"></a>Metrische servergegevens
Deze tabel gebruiken om te bepalen welke metrische gegevens voor uw bewaking scenario het meest geschikt zijn. Metrische gegevens van dezelfde eenheid kan worden weergegeven op de dezelfde grafiek.

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Type samenvoeging geselecteerd|Beschrijving|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Lengte van taakwachtrij Pool opdracht|Count|Gemiddeld|Het aantal taken in de wachtrij van de opdrachtthreadpool.|
|CurrentConnections|Verbinding: Huidige verbindingen|Count|Gemiddeld|Huidig aantal clientverbindingen dat tot stand gebracht.|
|CurrentUserSessions|Huidige gebruikerssessies|Count|Gemiddeld|Huidig aantal vastgestelde gebruikerssessies.|
|mashup_engine_memory_metric|M-Enginegeheugen|Bytes|Gemiddeld|Geheugengebruik door mashup-engineprocessen|
|mashup_engine_qpu_metric|M-Engine QPU|Count|Gemiddeld|QPU-gebruik door mashupengineprocessen|
|memory_metric|Geheugen|Bytes|Gemiddeld|Geheugen. Bereik van 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|
|memory_thrashing_metric|Geheugenthrashing|Procent|Gemiddeld|Gemiddelde geheugenthrashing.|
|CleanerCurrentPrice|Geheugen: Schoner, huidige prijs|Count|Gemiddeld|Huidige prijs van geheugen, wat $/ byte/tijd, genormaliseerd voor 1000.|
|CleanerMemoryNonshrinkable|Geheugen: Schoner geheugen niet verkleinbaar|Bytes|Gemiddeld|Hoeveelheid geheugen in bytes, niet kan worden leeggemaakt Opruimprogramma op de achtergrond.|
|CleanerMemoryShrinkable|Geheugen: Schoner geheugen verkleinbaar|Bytes|Gemiddeld|Hoeveelheid geheugen in bytes, leeggemaakt Opruimprogramma op de achtergrond.|
|MemoryLimitHard|: Geheugen vaste geheugenlimiet|Bytes|Gemiddeld|Vaste geheugenlimiet, van configuratiebestand.|
|Toegepaste memorylimithigh-waarde|Geheugen: Hoge geheugenlimiet|Bytes|Gemiddeld|Hoge geheugenlimiet, van configuratiebestand.|
|MemoryLimitLow|Geheugen: Lage geheugenlimiet|Bytes|Gemiddeld|Lage geheugenlimiet, van configuratiebestand.|
|MemoryLimitVertiPaq|Geheugen: VertiPaq-geheugenlimiet|Bytes|Gemiddeld|In-memory limiet, van configuratiebestand.|
|MemoryUsage|Geheugen: Geheugengebruik|Bytes|Gemiddeld|Het geheugengebruik van het serverproces zoals gebruikt bij het berekenen van de prijs van schoner geheugen. Gelijk aan de teller Process\PrivateBytes plus de grootte van het geheugen toegewezen gegevens, wordt een geheugen, dat is toegewezen of door de engine voor in-memory analyse (VertiPaq) boven de geheugenlimiet-engine genegeerd.|
|Quota|Geheugen: quotum|Bytes|Gemiddeld|Het huidige geheugenquotum, in bytes. Geheugenquotum wordt ook wel bekend als een geheugenreservering grant of geheugen.|
|QuotaBlocked|Geheugen: Geblokkeerd quotum|Count|Gemiddeld|Huidig aantal quotumaanvragen dat is geblokkeerd totdat andere geheugenquotums worden vrijgemaakt.|
|VertiPaqNonpaged|Geheugen: VertiPaq niet verwisselbaar|Bytes|Gemiddeld|Bytes van het geheugen in de werkset voor gebruik door de in-memory engine vergrendeld.|
|VertiPaqPaged|Geheugen: VertiPaq verwisselbaar|Bytes|Gemiddeld|Bytes wisselbaar geheugen in gebruik voor in-memory-gegevens.|
|ProcessingPoolJobQueueLength|Lengte van taakwachtrij van toepassingen verwerken|Count|Gemiddeld|Aantal niet-I/O-taken in de wachtrij van de verwerkende ThreadPool.|
|RowsConvertedPerSec|Verwerken: Geconverteerde rijen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen dat tijdens verwerking is geconverteerd.|
|RowsReadPerSec|Verwerken: Gelezen rijen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen lezen van alle relationele databases.|
|RowsWrittenPerSec|Verwerken: Geschreven rijen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen dat tijdens de verwerking is geschreven.|
|qpu_metric|QPU|Count|Gemiddeld|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|
|QueryPoolBusyThreads|Query Pool-Jobs onderzoeken|Count|Gemiddeld|Het aantal actieve threads in de querythreadpool.|
|SuccessfullConnectionsPerSec|Geslaagde verbindingen Per seconde|CountPerSecond|Gemiddeld|Snelheid van aanvullen met de verbinding is geslaagd.|
|CommandPoolBusyThreads|Threads: Opdracht pool-Jobs onderzoeken|Count|Gemiddeld|Het aantal actieve threads in de opdrachtthreadpool.|
|CommandPoolIdleThreads|Threads: Niet-actieve threads van groep van de opdracht|Count|Gemiddeld|Het aantal niet-actieve threads in de opdrachtthreadpool.|
|LongParsingBusyThreads|Threads: Threads voor lang parseren bezet|Count|Gemiddeld|Het aantal actieve threads in de ThreadPool voor lang parseren.|
|LongParsingIdleThreads|Threads: Parseren van niet-actieve threads voor lang|Count|Gemiddeld|Het aantal niet-actieve threads in de ThreadPool voor lang parseren.|
|LongParsingJobQueueLength|Threads: Lengte van taakwachtrij lang parseren|Count|Gemiddeld|Het aantal taken in de wachtrij van de ThreadPool voor lang parseren.|
|ProcessingPoolIOJobQueueLength|Threads: Lengte van taakwachtrij i/o van toepassingen verwerken|Count|Gemiddeld|Het aantal i/o-taken in de wachtrij van de verwerkende ThreadPool.|
|ProcessingPoolBusyIOJobThreads|Threads: Threads van verwerkende pool bezet i/o-taak|Count|Gemiddeld|Het aantal threads waarmee i/o-taken worden uitgevoerd in de verwerkende ThreadPool.|
|ProcessingPoolBusyNonIOThreads|Threads: Actieve niet-I/O-threads van toepassingen verwerken|Count|Gemiddeld|Het aantal threads waarmee niet-I/O-taken worden uitgevoerd in de verwerkende ThreadPool.|
|ProcessingPoolIdleIOJobThreads|Threads: Groep niet-actieve i/o-taakthreads van verwerkende|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de verwerkende ThreadPool.|
|ProcessingPoolIdleNonIOThreads|Threads: Threads van verwerkende pool niet-actieve niet-I/O|Count|Gemiddeld|Het aantal niet-actieve threads in de verwerkende ThreadPool toegewezen aan niet-I/O-taken.|
|QueryPoolIdleThreads|Threads: Niet-actieve threads van groep van toepassingen opvragen|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de verwerkende ThreadPool.|
|QueryPoolJobQueueLength|Threads: Lengte van taakwachtrij pool Query|Count|Gemiddeld|Het aantal taken in de wachtrij van de querythreadpool.|
|ShortParsingBusyThreads|Threads: Threads voor kort parseren bezet|Count|Gemiddeld|Het aantal actieve threads in de ThreadPool voor kort parseren.|
|ShortParsingIdleThreads|Threads: Parseren van niet-actieve threads voor kort|Count|Gemiddeld|Het aantal niet-actieve threads in de ThreadPool voor kort parseren.|
|ShortParsingJobQueueLength|Threads: Parseren van de lengte van taakwachtrij voor kort|Count|Gemiddeld|Het aantal taken in de wachtrij van de ThreadPool voor kort parseren.|
|TotalConnectionFailures|Totaal aantal verbindingsfouten|Count|Gemiddeld|Totaal aantal mislukte verbindingspogingen.|
|TotalConnectionRequests|Totaal aantal verbindingsaanvragen|Count|Gemiddeld|Totaal aantal verbindingsaanvragen. |

## <a name="next-steps"></a>Volgende stappen
[Bewaking in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md)   
[Metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)   
[Metrische gegevens in Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx)