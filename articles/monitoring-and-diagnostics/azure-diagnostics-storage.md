---
title: Diagnostische gegevens opslaan en weergeven in Azure Storage
description: Azure diagnostics-gegevens in Azure Storage kunt ophalen en weergeven
services: azure-monitor
author: jpconnock
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/01/2016
ms.author: jeconnoc
ms.component: diagnostic-extension
ms.openlocfilehash: 733c84ef9e6cee1a8ea488f0007ade1e72f39737
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033246"
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Diagnostische gegevens van Store en weergeven in Azure Storage
Diagnostische gegevens worden niet permanent opgeslagen, tenzij u deze naar de Microsoft Azure-opslagemulator of naar Azure storage overbrengen. Eenmaal in de opslag, kunnen deze worden bekeken met een van de verschillende beschikbare hulpprogramma's.

## <a name="specify-a-storage-account"></a>Geef een opslagaccount
U opgeven het opslagaccount dat u wilt gebruiken in het bestand ServiceConfiguration.cscfg. De accountgegevens die wordt gedefinieerd als een verbindingsreeks in een configuratie-instelling. Het volgende voorbeeld ziet u de standaard-verbindingsreeks die is gemaakt voor een nieuwe Cloud Service-project in Visual Studio:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

U kunt deze verbindingsreeks voor de accountgegevens voor Azure storage-account wijzigen.

Afhankelijk van het type diagnostische gegevens worden verzameld, gebruikt Azure Diagnostics de Blob-service of de Table-service. De volgende tabel toont de gegevensbronnen die worden behouden en de indeling.

| Gegevensbron | Opslagindeling |
| --- | --- |
| Logboeken in Azure |Tabel |
| IIS 7.0-Logboeken |Blob |
| Logboeken van Azure Diagnostics-infrastructuur |Tabel |
| Logboeken met traceringen aanvraag is mislukt |Blob |
| Windows-gebeurtenislogboeken |Tabel |
| Prestatiemeteritems |Tabel |
| Crashdumps |Blob |
| Aangepaste foutenlogboeken |Blob |

## <a name="transfer-diagnostic-data"></a>Diagnostische gegevens overdragen
Voor de SDK 2.5 of hoger, kan de aanvraag voor het overdragen van diagnostische gegevens via het configuratiebestand optreden. U kunt diagnostische gegevens overdragen met regelmatige tussenpozen zoals opgegeven in de configuratie.

Voor de SDK 2.4 en vorige kunt u vragen om over te dragen van de diagnostische gegevens via het configuratiebestand ook als via een programma. De programmatische aanpak kunt u de overdrachten op aanvraag.

> [!IMPORTANT]
> Wanneer u diagnostische gegevens naar Azure storage-account overdraagt, worden er kosten voor de storage-resources die gebruikmaakt van de diagnostische gegevens.
> 
> 

## <a name="store-diagnostic-data"></a>Diagnostische gegevens van Store
Logboekgegevens worden opgeslagen in Blob of Table storage met de volgende namen:

**Tabellen**

* **WadLogsTable** - logboeken die zijn geschreven in code met behulp van de traceringslistener.
* **WADDiagnosticInfrastructureLogsTable** -diagnostische monitor en configuratiewijzigingen.
* **WADDirectoriesTable** -mappen die wordt bewaakt door de diagnostische monitor.  Dit omvat de IIS-logboeken, IIS mislukte aanvraag logboeken en aangepaste mappen.  De locatie van het logboekbestand van de blob is opgegeven in het veld Container en de naam van de blob is in het veld RelativePath.  Het veld AbsolutePath geeft de locatie en naam van het bestand zoals deze waren aanwezig op de virtuele machine van Azure.
* **WADPerformanceCountersTable** – prestatiemeteritems.
* **WADWindowsEventLogsTable** – Windows-gebeurtenislogboeken.

**Blobs**

* **wad besturingselementcontainer** – (alleen voor SDK 2.4 en vorige) bevat de XML-configuratiebestanden die bepaalt van de Azure diagnostics.
* **wad-iis-failedreqlogfiles** : bevat gegevens uit logboeken mislukte IIS-aanvragen.
* **wad-iis-logboekbestanden** : bevat informatie over IIS-logboeken.
* **'aangepaste'** – op basis van een aangepaste container over het configureren van mappen die worden bewaakt door de diagnostische monitor.  De naam van deze blob-container wordt opgegeven in WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Hulpprogramma's om diagnostische gegevens weer te geven
Er zijn verschillende hulpprogramma's beschikbaar om weer te geven van de gegevens nadat deze zijn overgebracht naar de opslag. Bijvoorbeeld:

* Server Explorer in Visual Studio - als u de Azure-hulpprogramma's hebt geïnstalleerd voor Microsoft Visual Studio, kunt u het Azure Storage-knooppunt in Server Explorer om alleen-lezen-blob- en tabelgegevens van uw Azure storage-accounts weer te geven. U kunt gegevens weergeven vanuit uw lokale emulator van de opslagaccount en ook van storage-accounts u hebt gemaakt voor Azure. Zie voor meer informatie, [surfen en Storage-Resources beheren met de Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, OSX en Linux.
* [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) met inbegrip van Azure Diagnostics Manager zodat u kunt weergeven, downloaden en beheren van de diagnostische gegevens die worden verzameld door de toepassingen die worden uitgevoerd op Azure.

## <a name="next-steps"></a>Volgende stappen
[De stroom in een Cloud Services-toepassing met Azure Diagnostics traceren](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)

