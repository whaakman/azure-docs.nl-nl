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
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2018
ms.author: ryanwi
ms.openlocfilehash: f4ae164a9862a32b45fd5bd0ae7bc09a1180c344
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733481"
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
Webplatforminstallatieprogramma (WebPI) is de aanbevolen manier voor het installeren van de SDK en hulpprogramma's. Als u de runtime-fouten met WebPI ontvangt, kunt u ook directe koppelingen naar de installatieprogramma's vinden in de release-opmerkingen voor een specifieke release van Service Fabric. Opmerkingen bij de release kunnen u vinden in de verschillende aankondigingen op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/).

> [!NOTE]
> Lokale Service Fabric-ontwikkeling cluster upgrades worden niet ondersteund.

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
* Service Fabric SDK en hulpprogramma's 3.3.637
* Service Fabric-runtime 6.4.637
* Service Fabric Tools voor Visual Studio 2015 2.4.11116.1
* Visual Studio 2017 15,9 inch bevat Service Fabric Tools voor Visual Studio 2.4.11024.1 

Zie [Ondersteuning voor Service Fabric](service-fabric-support.md) voor een lijst met ondersteunde versies.

> [!NOTE]
> Upgrade van één machine clusters (OneBox) worden niet ondersteund voor toepassing of het Cluster; de OneBox-cluster verwijderen en opnieuw maken als u wilt een clusterupgrade uitvoeren of hebt problemen met de upgrade van een toepassing uit te voeren. 

## <a name="enable-powershell-script-execution"></a>Uitvoering van PowerShell-script inschakelen
Service Fabric gebruikt Windows PowerShell-scripts om een lokaal ontwikkelcluster te maken en om toepassingen vanuit Visual Studio te implementeren. Standaard worden deze scripts door Windows geblokkeerd zodat ze niet worden uitgevoerd. Als u ze wilt inschakelen, moet u het PowerShell-uitvoeringsbeleid wijzigen. Open PowerShell als een beheerder en voer de volgende opdracht in:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```
## <a name="install-docker-optional"></a>Docker (optioneel) installeren
[Service Fabric is een containerorchestrator](service-fabric-containers-overview.md) voor het implementeren van microservices in een computercluster. Voor het uitvoeren van toepassingen voor Windows-containers op uw lokale ontwikkelcluster, moet u eerst Docker voor Windows installeren. Ophalen [Docker CE voor Windows (stabiel)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Nadat u Docker hebt geïnstalleerd en gestart, klikt u met de rechtermuisknop op het systeemvakpictogram en selecteert u **Overschakelen naar Windows-containers**. Deze stap is vereist voor het uitvoeren van Docker-installatiekopieën onder Windows.

## <a name="next-steps"></a>Volgende stappen
Nu u uw ontwikkelingsomgeving hebt ingesteld, kunt u apps ontwikkelen en uitvoeren.

* [Meer informatie over het maken, implementeren en beheren van toepassingen](service-fabric-tutorial-create-dotnet-app.md)
* [Meer informatie over de programmeermodellen: Reliable Services en Reliable Actors](service-fabric-choose-framework.md)
* [Voorbeelden van Service Fabric-code op GitHub bekijken](https://aka.ms/servicefabricsamples)
* [Uw cluster visualiseren door gebruik te maken van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

[1]: https://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric-campagnepagina"
[2]: https://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-koppeling"
[full-bundle-dev15]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-koppeling"
[core-sdk]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-koppeling"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
