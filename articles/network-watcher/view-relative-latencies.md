---
title: Relatieve latentie aan Azure-regio's van specifieke locaties bekijken | Microsoft Docs
description: Informatie over het weergeven van relatieve latentie voor internetproviders aan Azure-regio's vanaf specifieke locaties.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 6ac37c3a53b0cc71bdab85fb86e0e85d998867aa
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300605"
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>De relatieve latentie weergeven op Azure-regio's op specifieke locaties

In deze zelfstudie leert u hoe u het gebruik van de Azure [Network Watcher](network-watcher-monitoring-overview.md) service waarmee u kunt bepalen welke Azure-regio naar uw toepassing implementeren of -service in, op basis van uw demografische gebruiker. U kunt deze ook gebruiken om u te helpen bij het evalueren van serviceproviders verbindingen met Azure.  
        
## <a name="create-a-network-watcher"></a>Maken van een network watcher

Als u al een network watcher in ten minste één Azure [regio](https://azure.microsoft.com/regions), kunt u de taken in deze sectie overslaan. Maak een resourcegroep voor de netwerk-watcher. In dit voorbeeld wordt de resourcegroep wordt gemaakt in de regio VS-Oost, maar u kunt de resourcegroep in een Azure-regio maken.

```powershell
New-AzureRmResourceGroup -Name NetworkWatcherRG -Location eastus
```

Maak een network watcher. Hebt u een network watcher in ten minste één Azure-regio gemaakt. In dit voorbeeld wordt een network watcher in de oostelijke VS Azure-regio gemaakt.

```powershell
New-AzureRmNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Relatieve netwerklatenties vergelijken met één Azure-regio op een bepaalde locatie

Serviceproviders evalueren of een gebruiker een probleem melden zoals 'de site is traag,' problemen oplossen op een bepaalde locatie naar de azure-regio waarin een service wordt geïmplementeerd. De volgende opdracht retourneert bijvoorbeeld het gemiddelde relatieve Internet serviceprovider latentie tussen de staat Washington in de Verenigde Staten en de regio West US 2 Azure tussen 13-15 December 2017:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Location "West US 2" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> De regio die u in de vorige opdracht opgeeft hoeft niet dezelfde zijn als de regio die u hebt opgegeven toen u de netwerk-watcher opgehaald. De vorige opdracht is alleen vereist dat u een bestaande netwerk-watcher opgeeft. De netwerk-watcher kan zich in andere regio's. Als u waarden opgeven voor `-Country` en `-State`, deze moet geldig zijn. De waarden zijn hoofdlettergevoelig. Gegevens zijn beschikbaar voor een beperkt aantal landen, Staten en plaatsen. Voer de opdrachten [weer beschikbaar landen, provincies, steden en providers](#view-available) om een lijst met beschikbare landen en steden statussen voor gebruik met de vorige opdracht weer te geven. 

> [!WARNING]
> Moet u een datum in de afgelopen 30 dagen voor `-StartTime` en `-EndTime`. Opgeven van een eerdere datum, leidt dit er zijn geen gegevens worden geretourneerd.

De uitvoer van de vorige opdracht met de volgende:

```powershell
AggregationLevel   : State
ProviderLocation   : {
                       "Country": "United States",
                       "State": "washington"
                     }
ReachabilityReport : [
                       {
                         "Provider": "Qwest Communications Company, LLC - ASN 209",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 92
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 92
                           }
                         ]
                       },
                       {
                         "Provider": "Comcast Cable Communications, LLC - ASN 7922",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 96
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 96
                           }
                         ]
                       }
                     ]
```

In de resulterende uitvoer, de waarde voor **Score** wordt de relatieve latentie tussen regio's en -providers. Een score van 1 is de slechtste (hoogste) latentie, terwijl 100 de laagste latentie is. De relatieve latenties zijn gemiddeld voor die dag. In het vorige voorbeeld, terwijl deze heeft wissen die de latenties zijn hetzelfde op beide dagen en dat er is een klein verschil tussen de latentie van de twee providers, het is ook wissen die de latenties voor beide providers lage op de schaal van 1-100 zijn. Terwijl deze wordt verwacht, omdat de staat Washington in de Verenigde Staten bevinden zich fysiek dicht bij de West US 2 Azure-regio, soms resultaten zijn niet zoals verwacht. Hoe groter het datumbereik dat u opgeeft, hoe meer u gemiddelde latentie na verloop van tijd.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Relatieve netwerkvertragingen van Azure-regio's vanaf een specifieke locatie met elkaar vergelijken

Als, in plaats van op te geven de relatieve latentie tussen een specifieke locatie en een specifieke Azure-regio met `-Location`, u wilt bepalen van de relatieve latentie voor alle Azure-regio's op een bepaalde fysieke locatie, kunt u dat te doen. Bijvoorbeeld, de volgende opdracht helpt u te evalueren welke azure-regio voor het implementeren van een service in als uw primaire gebruikers Comcast gebruikers zich in de staat Washington:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Provider "Comcast Cable Communications, LLC - ASN 7922" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

>[!NOTE]
In tegenstelling tot moet wanneer u een enkele locatie opgeeft, als u niet een locatie opgeven, of meerdere locaties, zoals 'West vs2', 'VS-West, u een Internet-provider wanneer u de opdracht uitvoert. 

## <a name="view-available"></a>Beschikbare landen, provincies, steden en providers weergeven

Gegevens zijn beschikbaar voor bepaalde internetproviders, landen, Staten en plaatsen. Voer de volgende opdracht om een lijst weergeven met alle beschikbare internetserviceproviders, landen, Staten en plaatsen, die u kunt gegevens bekijken:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Gegevens is alleen beschikbaar voor de landen, Staten en plaatsen die zijn geretourneerd door de vorige opdracht. De vorige opdracht moet u om op te geven van een bestaande netwerk-watcher. Het opgegeven voorbeeld de *NetworkWatcher_eastus* network watcher in een resourcegroep met de naam *NetworkWatcherRG*, maar kunt u eventuele bestaande netwerk-watcher. Als u een bestaande netwerk-watcher niet hebt, maakt u er een door het uitvoeren van de taken in [maken van een network watcher](#create-a-network-watcher). 

Nadat de vorige opdracht is uitgevoerd, kunt u de uitvoer die wordt geretourneerd door het opgeven van geldige waarden voor filteren **land**, **status**, en **plaats**, indien gewenst.  Bijvoorbeeld, als u wilt weergeven van de lijst met internetproviders in Seattle, Washington, beschikbaar in de Verenigde Staten, voer de volgende opdracht:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> De opgegeven waarde voor **land** moet hoofdletters en kleine letters. De waarden die zijn opgegeven voor **status** en **plaats** moet een kleine letter. De waarden moeten worden vermeld in de uitvoer die wordt geretourneerd na het uitvoeren van de opdracht zonder waarden voor **land**, **status**, en **plaats**. Als u de onjuiste aanvraag opgeven of een waarde opgeven voor **land**, **status**, of **plaats** die zich niet in de uitvoer die wordt geretourneerd na het uitvoeren van de opdracht zonder waarden voor deze Eigenschappen van de resulterende uitvoer is leeg.
