---
title: Diagnostische gegevens opslaan en weergeven in Azure Storage | Microsoft Docs
description: Azure diagnostics-gegevens ophalen voor Azure Storage en bekijken
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: 374cc179e13c00e439415e3df16e0c6d5ccba5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Diagnostische gegevens opslaan en weergeven in Azure Storage
Diagnostische gegevens is niet permanent opgeslagen tenzij u deze naar de Microsoft Azure-opslagemulator of naar Azure storage overdragen. Eenmaal in de opslag, kunnen deze worden bekeken met een van de verschillende beschikbare hulpprogramma's.

## <a name="specify-a-storage-account"></a>Storage-account opgeven
U opgeven het opslagaccount dat u wilt gebruiken in het bestand ServiceConfiguration.cscfg. De accountgegevens wordt gedefinieerd als een verbindingsreeks in een configuratie-instelling. Het volgende voorbeeld ziet u de standaardverbindingsreeks gemaakt voor een nieuwe Cloudservice-project in Visual Studio:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

U kunt deze verbindingsreeks met account-informatie voor een Azure storage-account wijzigen.

Afhankelijk van het type diagnostische gegevens worden verzameld, maakt gebruik van Azure Diagnostics de Blob-service of de tabel-service. De volgende tabel toont de gegevensbronnen die worden doorgevoerd en hun notatie.

| Gegevensbron | Opslagindeling |
| --- | --- |
| Azure-Logboeken |Tabel |
| IIS 7.0-Logboeken |Blob |
| Logboeken van Azure Diagnostics-infrastructuur |Tabel |
| Logboeken met traceringen aanvraag is mislukt |Blob |
| Windows-gebeurtenislogboeken |Tabel |
| Prestatiemeteritems |Tabel |
| Crashdumps |Blob |
| Aangepaste foutenlogboeken |Blob |

## <a name="transfer-diagnostic-data"></a>Diagnostische gegevens overbrengen
Voor de SDK 2.5 of hoger, kan de aanvraag voor diagnostische gegevens overdragen via het configuratiebestand optreden. U kunt diagnostische gegevens overbrengen met regelmatige tussenpozen zoals opgegeven in de configuratie.

Voor SDK-2.4 en vorige kunt u de diagnostische gegevens als programmatisch overdragen via het configuratiebestand ook vragen. De programmatische aanpak kunt u doen op aanvraag overdrachten.

> [!IMPORTANT]
> Wanneer u diagnostische gegevens naar Azure storage-account overdraagt, u kosten in rekening worden voor de storage-resources die gebruikmaakt van de diagnostische gegevens.
> 
> 

## <a name="store-diagnostic-data"></a>Diagnostische gegevens op te slaan
Logboekgegevens worden opgeslagen in Blob of Table storage met de volgende namen:

**Tabellen**

* **WadLogsTable** - Logboeken geschreven in code met behulp van de traceringslistener.
* **WADDiagnosticInfrastructureLogsTable** -diagnostische monitor en configuratie van wijzigingen.
* **WADDirectoriesTable** – mappen die wordt bewaakt door de diagnostische monitor.  Dit omvat IIS-logboeken, IIS kan niet logboeken aanvragen en aangepaste mappen.  De locatie van het logboekbestand van de blob is opgegeven in het veld Container en de naam van de blob is in het veld RelativePath.  Het veld AbsolutePath geeft de locatie en de naam van het bestand zoals deze waren aanwezig op de virtuele machine van Azure.
* **WADPerformanceCountersTable** – prestatiemeteritems.
* **WADWindowsEventLogsTable** – Windows-gebeurtenislogboeken.

**Blobs**

* **af-besturingselement-container** – (alleen voor SDK-2.4 en vorige) bevat de XML-configuratiebestanden die bepaalt van Azure diagnostics.
* **af-iis-failedreqlogfiles** – bevat gegevens uit logboeken IIS aanvragen is mislukt.
* **af-iis-logboekbestanden** : bevat informatie over IIS-logboeken.
* **'aangepaste'** – op basis van een aangepaste container over het configureren van mappen die worden bewaakt door de diagnostische monitor.  De naam van deze blob-container wordt in WADDirectoriesTable worden opgegeven.

## <a name="tools-to-view-diagnostic-data"></a>Hulpprogramma's om diagnostische gegevens weer te geven
Er zijn verschillende hulpprogramma's beschikbaar zijn voor het weergeven van de gegevens nadat deze zijn overgebracht naar de opslag. Bijvoorbeeld:

* Server Explorer in Visual Studio - als u de hulpprogramma's Azure hebt geïnstalleerd voor Microsoft Visual Studio, kunt u het Azure Storage-knooppunt in Server Explorer blob voor alleen-lezen en tabelgegevens weergeven van uw Azure storage-accounts. U kunt gegevens weergeven vanuit uw lokale opslagaccount voor de emulator en ook van storage-accounts u hebt gemaakt voor Azure. Zie voor meer informatie [surfen en het beheren van opslagbronnen met Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige app waardoor u eenvoudig werken met Azure Storage-gegevens op Windows, OSX en Linux.
* [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) bevat Azure Diagnostics Manager zodat u kunt bekijken, downloaden en beheren van de diagnostische gegevens verzameld door de toepassingen die worden uitgevoerd op Azure.

## <a name="next-steps"></a>Volgende stappen
[De stroom in een Cloud Services-toepassing met Azure Diagnostics traceren](cloud-services-dotnet-diagnostics-trace-flow.md)

