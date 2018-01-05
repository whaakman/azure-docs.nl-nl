---
title: Relatieve latenties naar Azure-regio's weergeven op een specifieke locatie | Microsoft Docs
description: Informatie over het relatieve latenties overal in internetproviders weergeven voor Azure-regio's op een specifieke locatie.
services: network-watcher
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: a6c2ffa619eeff8b455df8a8b2157525af12c640
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Weergave relatieve latentie naar Azure-regio's op een specifieke locatie

Informatie over het gebruik van de Azure in deze zelfstudie [netwerk-Watcher](network-watcher-monitoring-overview.md) service om te helpen kiezen welke Azure-regio voor het implementeren van uw toepassing of service in, op basis van uw demografische gebruiker. Daarnaast kunt u deze gebruiken om u te helpen bij het evalueren van de serviceproviders verbindingen naar Azure.  
        
## <a name="create-a-network-watcher"></a>maken van een netwerk-watcher

Als u al een netwerk-watcher in ten minste één Azure [regio](https://azure.microsoft.com/regions), kunt u de taken in deze sectie overslaan. Maak een resourcegroep voor de netwerk-watcher. In dit voorbeeld wordt de resourcegroep wordt gemaakt in de regio VS-Oost, maar u kunt de resourcegroep maken in Azure-regio.

```powershell
New-AzureRmResourceGroup -Name NetworkWatcherRG -Location eastus
```

Maak een netwerk-watcher. U moet een netwerk-watcher gemaakt in ten minste één Azure-regio hebben. In dit voorbeeld wordt een netwerk-watcher gemaakt in de Oost ons Azure-regio.

```powershell
New-AzureRmNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Relatieve netwerklatenties vergelijken met één Azure-regio vanaf een bepaalde locatie

Serviceproviders evalueren of het oplossen van een gebruiker een probleem melden, zoals 'de site is traag,' van een specifieke locatie naar de azure-regio waar een service wordt geïmplementeerd. De volgende opdracht geeft bijvoorbeeld de gemiddelde relatieve Internet serviceprovider latenties tussen de staat Washington in de Verenigde Staten en de regio West ons 2 Azure tussen December 13-15 2017:

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
> De regio die u in de vorige opdracht opgeeft hoeft niet dezelfde zijn als de regio die u hebt opgegeven als u de netwerk-watcher opgehaald. De vorige opdracht gewoon vereist dat u een bestaande netwerk-watcher opgeeft. De netwerk-watcher kan zijn in elke regio. Als u waarden opgeven voor `-Country` en `-State`, moeten ze geldig zijn. De waarden zijn hoofdlettergevoelig. Gegevens zijn beschikbaar voor een beperkt aantal landen, provincies en plaatsen. Voer de opdrachten [beschikbare landen, statussen, plaatsen en providers weergeven](#view-available) om een lijst met beschikbare landen, plaatsen en statussen voor gebruik met de vorige opdracht weer te geven. 

> [!WARNING]
> U moet een datum na 14 November 2017 voor opgeven `-StartTime` en `-EndTime`. Een datum vóór 14 November 2017 geven geen gegevens geretourneerd. 

De uitvoer van de vorige opdracht volgt:

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

In de resulterende uitvoer is de waarde voor **Score** wordt de relatieve latentie in regio's en providers. Een score van 1 is de slechtste (hoogste) latentie terwijl 100 de laagste latentie. De relatieve latenties gemiddelde voor de dag. In het vorige voorbeeld, terwijl deze heeft wissen die de latenties zijn dezelfde beide dagen en dat er is een klein verschil tussen de latentie van de twee providers, het is ook wissen die de latenties voor beide providers weinig de schaal van 1-100 zijn. Terwijl dit wordt verwacht, omdat de staat Washington in de Verenigde Staten fysiek dicht bij de regio West ons 2 Azure, soms resultaten zijn niet zoals verwacht. Hoe groter het datumbereik dat u opgeeft, hoe meer u gemiddelde latentie gedurende een bepaalde periode.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Relatieve netwerkvertragingen van Azure-regio's vanuit een specifieke locatie met elkaar vergelijken

Als in plaats van de relatieve latenties tussen een specifieke locatie en een specifieke Azure-regio met `-Location`, u wilt bepalen van de relatieve latenties tot alle Azure-regio's vanuit een specifieke fysieke locatie, kunt u dat te doen. De volgende opdracht kunt u evalueren welke azure-regio voor het implementeren van een service in als uw primaire gebruikers Comcast-gebruikers die zich in de staat Washington zijn:

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
In tegenstelling tot moet wanneer u een enkele locatie opgeeft, als u niet een locatie opgeven of meerdere locaties opgeven, zoals 'West US2', 'West US', u een Internet-provider als de opdracht uit te voeren. 

## <a name="view-available"></a>Beschikbare landen, statussen, plaatsen en providers weergeven

Gegevens zijn beschikbaar voor specifieke internetproviders, landen, statussen en plaatsen. Voer de volgende opdracht voor een lijst met alle beschikbare Internet serviceproviders, landen, provincies en steden, die u kunt gegevens bekijken:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Gegevens is alleen beschikbaar voor de landen, provincies en plaatsen die zijn geretourneerd door de vorige opdracht. De vorige opdracht moet u een bestaande netwerk-watcher opgeven. Het opgegeven voorbeeld de *NetworkWatcher_eastus* netwerk-watcher in een resourcegroep met de naam *NetworkWatcherRG*, maar u kunt een bestaande netwerk-watcher opgeven. Als u een bestaande netwerk-watcher niet hebt, maakt u een door de taken in te voeren [maken van een netwerk-watcher](#create-a-network-watcher). 

Nadat de vorige opdracht is uitgevoerd, kunt u de uitvoer die wordt geretourneerd door te geven van geldige waarden voor filteren **land**, **status**, en **stad**, indien gewenst.  Bijvoorbeeld, als u wilt weergeven van de lijst met Internet serviceproviders beschikbaar in Amsterdam, in de Verenigde Staten, voer de volgende opdracht:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> De opgegeven waarde voor **land** moet hoofdletters en kleine letters. De opgegeven waarden voor **status** en **stad** moet een kleine letter. De waarden moeten worden vermeld in de uitvoer die wordt geretourneerd na het uitvoeren van de opdracht zonder waarden voor **land**, **status**, en **stad**. Als u wilt opgeven of onjuist of geef een waarde op voor **land**, **status**, of **stad** die zich niet in de uitvoer die wordt geretourneerd na het uitvoeren van de opdracht zonder waarden voor deze Eigenschappen van de uitvoer van de geretourneerde is leeg.
