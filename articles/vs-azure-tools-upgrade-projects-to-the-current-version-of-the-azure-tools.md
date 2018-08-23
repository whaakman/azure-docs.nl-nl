---
title: Projecten bijwerken naar de huidige versie van de Azure-hulpprogramma's | Microsoft Docs
description: Meer informatie over het upgraden van een Azure-project in Visual Studio naar de huidige versie van de Azure-hulpprogramma 's
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1d64070a-078d-468a-87f4-e6715de6475f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 19d6edd9f5aa4fd33611a25143f36c6365c26761
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060788"
---
# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projecten bijwerken naar de huidige versie van de Azure Tools voor Visual Studio
## <a name="overview"></a>Overzicht
Nadat u de huidige release van de Azure-hulpprogramma's (of een vorige versie die nieuwer is dan 1.6) hebt geïnstalleerd, alle projecten die zijn gemaakt met behulp van een Azure-hulpprogramma's voor 1.6 release (November 2011) wordt automatisch worden bijgewerkt als u ze opent. Als u projecten gemaakt met behulp van de versie 1.6 (November 2011) versie van deze hulpprogramma's en u nog steeds deze release die is geïnstalleerd hebt, kunt u deze projecten openen in de oudere versie en later besluit of u wilt upgraden.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hoe uw project wordt gewijzigd wanneer u een upgrade uitvoert
Als een project automatisch bijgewerkt wordt of als u opgeeft dat u wilt bijwerken, uw project is gewijzigd om te werken met de huidige versies van bepaalde assembly's en sommige eigenschappen zijn ook gewijzigd zoals in deze sectie wordt beschreven. Als uw project is vereist voor andere wijzigingen compatibel zijn met de nieuwere versie van de hulpprogramma's, moet u deze wijzigingen handmatig aanbrengen.

* Het bestand web.config voor web-rollen en het bestand app.config voor werkrollen worden bijgewerkt om te verwijzen naar de nieuwere versie van Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* De Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll en Microsoft.WindowsAzure.ServiceRuntime.dll-assembly's zijn bijgewerkt naar de nieuwe versies.
* Profielen publiceren die zijn opgeslagen in de Azure-project-bestand (.ccproj) worden verplaatst naar een afzonderlijk bestand, met de extensie .azurePubXml, in de **publiceren** submap.
* Sommige eigenschappen in het publicatieprofiel worden bijgewerkt ter ondersteuning van nieuwe en gewijzigde functies. **AllowUpgrade** wordt vervangen door **DeploymentReplacementMethod** omdat kunt u een geïmplementeerde cloudservice tegelijkertijd of incrementeel bijwerken.
* De eigenschap **UseIISExpressByDefault** is toegevoegd en ingesteld op false, zodat de webserver die wordt gebruikt voor het opsporen van fouten niet automatisch gewijzigd van Internet Information Services (IIS) in IIS Express. IIS Express is de standaard-webserver voor projecten die zijn gemaakt met de nieuwere versies van de hulpprogramma's.
* Als Azure Caching wordt gehost in een of meer van de rollen van uw project, worden bepaalde eigenschappen in de serviceconfiguratie (.cscfg-bestand) en de definitie van de service (.csdef-bestand) worden gewijzigd wanneer een project wordt bijgewerkt. Als het project gebruikmaakt van de Azure cache NuGet-pakket, wordt het project is bijgewerkt naar de meest recente versie van het pakket. U moet open het bestand web.config en controleer of de configuratie van de client correct is onderhouden tijdens het upgradeproces. Als u de verwijzingen naar Azure Caching client assembly's zonder gebruik van het NuGet-pakket hebt toegevoegd, kunt u deze assembly's wordt niet bijgewerkt; Deze verwijzingen naar de nieuwe versies, moet u handmatig bijwerken.

> [!IMPORTANT]
> Voor F #-projecten, moet u verwijzingen naar Azure-assembly's handmatig bijwerken zodat ze verwijzen naar de nieuwere versies van deze assembly's.
> 
> 

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Een Azure-project bijwerken naar de huidige versie
1. De huidige versie van de Azure-hulpprogramma's installeren in de installatie van Visual Studio die u wilt gebruiken voor het bijgewerkte project en open vervolgens het project dat u wilt upgraden. Als het project is gemaakt met een Azure-hulpprogramma's vrijgeven voordat 1.6 (November 2011), het project wordt automatisch bijgewerkt naar de huidige versie. Als het project is gemaakt met de November 2011 vrijgeven en deze release nog steeds is geïnstalleerd, het project wordt geopend in deze release.
2. Open in Solution Explorer het snelmenu voor het projectknooppunt, kies **eigenschappen**, en kies vervolgens de **toepassing** tabblad van het dialoogvenster dat wordt weergegeven.
   
    De **toepassing** tabblad bevat de hulpprogramma's voor de versie die is gekoppeld aan het project. Als de huidige versie van Azure-hulpprogramma's wordt weergegeven, is al bijgewerkt van het project. Als u een nieuwere versie van de hulpprogramma's dan welke het tabblad bevat, hebt geïnstalleerd een **Upgrade** knop wordt weergegeven.
3. Kies de **Upgrade** knop een project bijwerken naar de huidige versie van de hulpprogramma's.
4. Bouw het project en klik vervolgens los eventuele fouten die het resultaat van API-wijzigingen. Zie voor meer informatie over het wijzigen van uw code voor de nieuwe versie, de documentatie voor de specifieke API.

