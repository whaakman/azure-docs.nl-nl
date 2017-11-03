---
title: Projecten bijwerken naar de huidige versie van de Azure-hulpprogramma's | Microsoft Docs
description: Meer informatie over het bijwerken van een Azure-project in Visual Studio naar de huidige versie van de Azure-hulpprogramma 's
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1d64070a-078d-468a-87f4-e6715de6475f
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 9a35de7ca0e7161468181b21709e1bd9915d566f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projecten bijwerken naar de huidige versie van de Azure-hulpprogramma's voor Visual Studio
## <a name="overview"></a>Overzicht
Nadat u de huidige release van de hulpprogramma's van Azure (of een eerdere versie die nieuwer is dan 1.6) hebt geïnstalleerd, laat u de projecten die zijn gemaakt met een Azure-hulpprogramma's voordat 1.6 (November 2011) zullen automatisch worden bijgewerkt als u deze openen. Als u projecten gemaakt met behulp van de versie 1.6 (November 2011) versie van deze hulpprogramma's en nog steeds dat versie geïnstalleerd, kunt u deze projecten openen in de oudere versie en later besluit of bijwerken.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hoe uw project verandert wanneer u een upgrade uitvoeren
Als een project automatisch bijgewerkt wordt of u opgeven dat u wilt upgraden, uw project om te werken met de huidige versies van bepaalde assembly's wordt gewijzigd en sommige eigenschappen zijn ook gewijzigd zoals in deze sectie wordt beschreven. Als uw project andere wijzigingen compatibel zijn met de nieuwere versie van de hulpprogramma's vereist, moet u deze wijzigingen handmatig aanbrengen.

* Het bestand web.config voor web-rollen en het bestand app.config voor werkrollen worden bijgewerkt om te verwijzen naar de nieuwere versie van Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* De assembly's Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll en Microsoft.WindowsAzure.ServiceRuntime.dll worden bijgewerkt naar de nieuwe versies.
* Profielen publiceren die zijn opgeslagen in de Azure-project-bestand (.ccproj) worden verplaatst naar een afzonderlijk bestand, met de .azurePubXml uitbreiding in de **publiceren** submap.
* Sommige eigenschappen in het publicatieprofiel worden ter ondersteuning van nieuwe en gewijzigde functies bijgewerkt. **AllowUpgrade** wordt vervangen door **DeploymentReplacementMethod** omdat kunt u een geïmplementeerde cloudservice tegelijkertijd of incrementeel bijwerken.
* De eigenschap **UseIISExpressByDefault** wordt toegevoegd en ingesteld op false, zodat de webserver dat wordt gebruikt voor het opsporen van fouten niet automatisch gewijzigd van Internet Information Services (IIS) in IIS Express. IIS Express is de standaard-webserver voor projecten die zijn gemaakt met de nieuwere versie van de hulpprogramma's.
* Als Azure opslaan in cache wordt gehost in een of meer van uw project rollen, worden sommige eigenschappen in de serviceconfiguratie (.cscfg-bestand) en de servicedefinitie (csdef-bestand) gewijzigd wanneer de upgrade van een project wordt uitgevoerd. Als de Azure Caching NuGet-pakket gebruikt in het project, wordt het project bijgewerkt naar de meest recente versie van het pakket. U moet het bestand web.config te openen en controleren of de configuratie van de client correct is behouden tijdens de upgrade. Als u de verwijzingen naar Azure Caching clientassembly zonder gebruik van het NuGet-pakket hebt toegevoegd, kunt u deze assembly's niet bijgewerkt; Deze verwijzingen naar de nieuwe versies, moet u handmatig bijwerken.

> [!IMPORTANT]
> F # projecten, moet u verwijzingen naar Azure-assembly's handmatig bijwerken zodat deze verwijzen naar de nieuwere versies van deze assembly's.
> 
> 

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Een Azure-project bijwerken naar de huidige release
1. De huidige versie van de Azure-hulpprogramma's worden geïnstalleerd op de installatie van Visual Studio die u wilt gebruiken voor het bijgewerkte project en open vervolgens het project dat u wilt upgraden. Als het project is gemaakt met een extra Azure release voordat 1.6 (November 2011), het project wordt automatisch bijgewerkt naar de huidige versie. Als het project is gemaakt met de November 2011 release en deze release is nog steeds geïnstalleerd, het project wordt geopend in deze release.
2. Open het snelmenu voor het projectknooppunt in Solution Explorer, kies **eigenschappen**, en kies vervolgens de **toepassing** tabblad van het dialoogvenster dat wordt weergegeven.
   
    De **toepassing** tabblad ziet u de hulpprogramma's-versie die is gekoppeld aan het project. Als de huidige versie van de hulpprogramma's van Azure wordt weergegeven, is het project al bijgewerkt. Als u een nieuwere versie van de hulpprogramma's dan wat het tabblad toont, hebt geïnstalleerd een **Upgrade** knop wordt weergegeven.
3. Kies de **Upgrade** knop bijwerken van een project naar de huidige versie van de hulpprogramma's.
4. Bouw het project en vervolgens los eventuele fouten die het gevolg zijn van wijzigingen van de API. Zie de documentatie voor de specifieke API voor informatie over het wijzigen van uw code voor de nieuwe versie.

