---
title: Een Windows-ontwikkelomgeving voor Azure-microservices instellen | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma's en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen in Windows.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 0691f26168feacf290b732afd7dfd680a2537179
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-your-development-environment-on-windows"></a>Uw ontwikkelomgeving voorbereiden in Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Als u [Azure Service Fabric-toepassingen][1] wilt bouwen en uitvoeren op een Windows-ontwikkelmachine, moet u de runtime, SDK en hulpprogramma's installeren. U moet er ook voor zorgen dat de Windows PowerShell-scripts die in de SDK zijn opgenomen, kunnen worden uitgevoerd.

## <a name="prerequisites"></a>Vereisten
### <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen
De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 bevat standaard alleen Windows PowerShell 2.0. Voor Service Fabric PowerShell-cmdlets is PowerShell 3.0 of hoger vereist. U kunt [Windows PowerShell 5.0 downloaden][powershell5-download] via het Microsoft Downloadcentrum.
> 
> 

## <a name="install-the-sdk-and-tools"></a>De SDK en hulpprogramma's installeren
### <a name="to-use-visual-studio-2017"></a>Visual Studio 2017 gebruiken
De hulpprogramma's voor Service Fabric vormen een onderdeel van de Azure-workload voor ontwikkeling en beheer in Visual Studio 2017. Schakel deze workload in als onderdeel van de Visual Studio-installatie.
Bovendien moet u de Microsoft Azure Service Fabric SDK installeren met behulp van het webplatforminstallatieprogramma.

* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015 gebruiken (hiervoor is Visual Studio 2015 Update 2 of later vereist)
Hulpprogramma's voor Service Fabric worden in Visual Studio 2015 geïnstalleerd samen met de SDK met behulp van het webplatforminstallatieprogramma:

* [De SDK en hulpprogramma's voor Microsoft Azure Service Fabric installeren][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Alleen SDK-installatie
Als u alleen de SDK nodig hebt, kunt u dit pakket installeren:
* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

De huidige versies zijn:
* Service Fabric SDK 2.8.211
* Service Fabric Runtime 6.0.211
* Service Fabric Tools voor Visual Studio 2015 1.7.50721
* Visual Studio 2017 Update 3 bevat Service Fabric Tools voor Visual Studio 1.7.20170817
* Visual Studio 2017 Update 4 Preview 1 (15.4.0 Preview 1.0) bevat Service Fabric Tools voor Visual Studio 1.7.20170721

Zie [Ondersteuning voor Service Fabric](service-fabric-support.md) voor een lijst met ondersteunde versies.

## <a name="enable-powershell-script-execution"></a>Uitvoering van PowerShell-script inschakelen
Service Fabric gebruikt Windows PowerShell-scripts om een lokaal ontwikkelcluster te maken en om toepassingen vanuit Visual Studio te implementeren. Standaard worden deze scripts door Windows geblokkeerd zodat ze niet worden uitgevoerd. Als u ze wilt inschakelen, moet u het PowerShell-uitvoeringsbeleid wijzigen. Open PowerShell als een beheerder en voer de volgende opdracht in:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Volgende stappen
Nu u uw ontwikkelingsomgeving hebt ingesteld, kunt u apps ontwikkelen en uitvoeren.

* [Uw eerste Service Fabric-toepassing in Visual Studio maken](service-fabric-create-your-first-application-in-visual-studio.md)
* [Meer informatie over het implementeren en beheren van toepassingen op uw lokale cluster](service-fabric-get-started-with-a-local-cluster.md)
* [Meer informatie over de programmeermodellen: Reliable Services en Reliable Actors](service-fabric-choose-framework.md)
* [Voorbeelden van Service Fabric-code op GitHub bekijken](https://aka.ms/servicefabricsamples)
* [Uw cluster visualiseren door gebruik te maken van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Het leertraject voor Service Fabric volgen voor een brede inleiding tot het platform](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-campagnepagina"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-koppeling"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-koppeling"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-koppeling"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
