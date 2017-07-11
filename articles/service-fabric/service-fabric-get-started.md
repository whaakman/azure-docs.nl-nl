---
title: Een ontwikkelomgeving voor Azure-microservices instellen | Microsoft Docs
description: Installeer de runtime, SDK en hulpprogramma's en maak een lokaal ontwikkelcluster. Zodra u dit hebt gedaan, kunt u toepassingen bouwen.
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
ms.date: 06/20/2017
ms.author: ryanwi, mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 926dfe3de0715f855e6d5b57f10c2366cda8583b
ms.contentlocale: nl-nl
ms.lasthandoff: 06/21/2017


---
<a id="prepare-your-development-environment" class="xliff"></a>

# Uw ontwikkelomgeving voorbereiden
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Als u [Azure Service Fabric-toepassingen][1] op uw ontwikkelmachine wilt bouwen en uitvoeren, moet u de runtime, de SDK en de hulpprogramma's installeren. U moet er ook voor zorgen dat de Windows PowerShell-scripts die in de SDK zijn opgenomen, kunnen worden uitgevoerd.

<a id="prerequisites" class="xliff"></a>

## Vereisten
<a id="supported-operating-system-versions" class="xliff"></a>

### Ondersteunde versies van besturingssystemen
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

<a id="install-the-sdk-and-tools" class="xliff"></a>

## De SDK en hulpprogramma's installeren
<a id="to-use-visual-studio-2017" class="xliff"></a>

### Visual Studio 2017 gebruiken
De hulpprogramma's voor Service Fabric vormen een onderdeel van de Azure-workload voor ontwikkeling en beheer in Visual Studio 2017. Schakel deze workload in als onderdeel van de Visual Studio-installatie.
Bovendien moet u de Microsoft Azure Service Fabric SDK installeren met behulp van het webplatforminstallatieprogramma.

* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

<a id="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later" class="xliff"></a>

### Visual Studio 2015 gebruiken (hiervoor is Visual Studio 2015 Update 2 of later vereist)
Hulpprogramma's voor Service Fabric worden in Visual Studio 2015 geïnstalleerd samen met de SDK met behulp van het webplatforminstallatieprogramma:

* [De SDK en hulpprogramma's voor Microsoft Azure Service Fabric installeren][full-bundle-vs2015]

<a id="sdk-installation-only" class="xliff"></a>

### Alleen SDK-installatie
Als u alleen de SDK nodig hebt, kunt u dit pakket installeren:
* [Microsoft Azure Service Fabric SDK installeren][core-sdk]

De huidige versies zijn:
* Service Fabric SDK 2.6.220
* Service Fabric-runtime 5.6.220
* Visual Studio 2015-hulpprogramma's 1.6.50508.2
* Visual Studio 2017 update 2

De huidige previewversies zijn:
* Service Fabric SDK 255.255.2718.255
* Service Fabric-runtime 255.255.5718.255
* Visual Studio 2015-hulpprogramma's 1.6.50509.5
* Visual Studio 2017 update 3 preview 1

Zie [Ondersteuning voor Service Fabric](service-fabric-support.md) voor een lijst met ondersteunde versies.

<a id="enable-powershell-script-execution" class="xliff"></a>

## Uitvoering van PowerShell-script inschakelen
Service Fabric gebruikt Windows PowerShell-scripts om een lokaal ontwikkelcluster te maken en om toepassingen vanuit Visual Studio te implementeren. Standaard worden deze scripts door Windows geblokkeerd zodat ze niet worden uitgevoerd. Als u ze wilt inschakelen, moet u het PowerShell-uitvoeringsbeleid wijzigen. Open PowerShell als een beheerder en voer de volgende opdracht in:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen
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

