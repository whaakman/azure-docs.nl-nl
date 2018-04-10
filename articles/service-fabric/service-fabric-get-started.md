---
title: Een Windows-ontwikkelomgeving voor Azure-microservices instellen | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma's en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen in Windows.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 4593f6f01759e41ee3cf4262b2eeb5295546e955
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="prepare-your-development-environment-on-windows"></a>Uw ontwikkelomgeving voorbereiden in Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

Als u [Azure Service Fabric-toepassingen][1] wilt bouwen en uitvoeren op een Windows-ontwikkelmachine, moet u de Service Fabric-runtime, SDK en hulpprogramma's installeren. U moet ook [de uitvoering van de Windows PowerShell-scripts inschakelen](#enable-powershell-script-execution) die in de SDK zijn opgenomen.

## <a name="prerequisites"></a>Vereisten
### <a name="supported-operating-system-versions"></a>Ondersteunde versies van besturingssystemen
De volgende versies van besturingssystemen worden ondersteund voor de ontwikkeling:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Ondersteuning voor Windows 7:
> - Windows 7 bevat standaard alleen Windows PowerShell 2.0. Voor Service Fabric PowerShell-cmdlets is PowerShell 3.0 of hoger vereist. U kunt [Windows PowerShell 5.0 downloaden][powershell5-download] via het Microsoft Downloadcentrum.
> - Er is geen omgekeerde proxy voor Service Fabric beschikbaar in Windows 7.
>

## <a name="install-the-sdk-and-tools"></a>De SDK en hulpprogramma's installeren
### <a name="to-use-visual-studio-2017"></a>Visual Studio 2017 gebruiken
De hulpprogramma's voor Service Fabric vormen een onderdeel van de Azure-workload voor ontwikkeling in Visual Studio 2017. Schakel deze workload in als onderdeel van de Visual Studio-installatie.
Bovendien moet u de Microsoft Azure Service Fabric SDK en runtime installeren met behulp van het webplatforminstallatieprogramma.

* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015 gebruiken (hiervoor is Visual Studio 2015 Update 2 of later vereist)
In Visual Studio 2015 worden de hulpprogramma's voor Service Fabric samen met de SDK en runtime geïnstalleerd met behulp van het webplatforminstallatieprogramma:

* [De SDK en hulpprogramma's voor Microsoft Azure Service Fabric installeren][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Alleen SDK-installatie
Als u alleen de SDK nodig hebt, kunt u dit pakket installeren:
* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

De huidige versies zijn:
* SDK en hulpprogramma's voor Service Fabric 3.0.480
* Service Fabric Runtime 6.1.480
* Service Fabric Tools for Visual Studio 2015 2.0.10124.2
* Visual Studio 2017 15.5.6 bevat Service Fabric Tools for Visual Studio 2.0.20180124.2  

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
* [Een Linux-ontwikkelomgeving voorbereiden in Windows](service-fabric-local-linux-cluster-windows.md)
* [Meer informatie over de programmeermodellen: Reliable Services en Reliable Actors](service-fabric-choose-framework.md)
* [Voorbeelden van Service Fabric-code op GitHub bekijken](https://aka.ms/servicefabricsamples)
* [Uw cluster visualiseren door gebruik te maken van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Het leertraject voor Service Fabric volgen voor een brede inleiding tot het platform](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)
* [Besturingssysteempatches automatiseren in uw cluster](service-fabric-patch-orchestration-application.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-campagnepagina"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-koppeling"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-koppeling"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-koppeling"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
