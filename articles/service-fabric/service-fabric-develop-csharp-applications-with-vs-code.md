---
title: .NET Core Azure Service Fabric-toepassingen met Visual Studio Code te ontwikkelen | Microsoft Docs
description: In dit artikel laat zien hoe maken, implementeren en foutopsporing .NET Core Service Fabric-toepassingen met behulp van Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 27c7c62125f3f559fb1764292729cbbfdc1c4e5f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116023"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>C# Service Fabric-toepassingen met Visual Studio Code te ontwikkelen

De [Reliable Services van Service Fabric-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) kunt u eenvoudig ontwikkelen van .NET Core Service Fabric-toepassingen op Windows, Linux en Mac OS-besturingssystemen.

In dit artikel leest u hoe te bouwen, implementeren en foutopsporing van een .NET Core Service Fabric-toepassing met behulp van Visual Studio Code.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd tegenover Code, de Service Fabric Reliable Services-extensie voor VS-Code en eventuele afhankelijkheden die nodig zijn voor uw ontwikkelomgeving. Zie voor meer informatie, [aan de slag](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Het voorbeeld downloaden
Dit artikel wordt de CounterService toepassing in de [Service Fabric .NET Core aan de slag voorbeelden GitHub-opslagplaats](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Als u wilt klonen van de opslagplaats op uw ontwikkelcomputer, voer de volgende opdracht vanaf een terminalvenster (opdrachtvenster in Windows):

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Open de toepassing in VS-Code

### <a name="windows"></a>Windows
Met de rechtermuisknop op het pictogram VS-Code in het Menu Start en kies **als administrator uitvoeren**. Als u wilt het foutopsporingsprogramma koppelen aan uw services, moet u tegenover Code als administrator uitvoeren.

### <a name="linux"></a>Linux
Met behulp van de terminal, gaat u naar het pad /service-fabric-dotnet-core-getting-started/Services/CounterService vanuit de map die de toepassing lokaal is gekloond in.

Voer de volgende opdracht om te openen tegenover Code als een hoofdgebruiker zodat het foutopsporingsprogramma aan uw services koppelen kunt.
```
sudo code . --user-data-dir='.'
```

De toepassing wordt nu weergegeven in uw werkruimte VS-Code.

![Prestatiemeteritem-servicetoepassing in de werkruimte](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>De toepassing bouwen
1. Druk op (Ctrl + Shift + p) openen de **opdracht palet** in VS-Code.
2. Zoek en selecteer de **Service Fabric: toepassing bouwen** opdracht. De builduitvoer wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS-Code maken](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>De toepassing op het lokale cluster implementeren
Nadat u de toepassing hebt gemaakt, kunt u deze kunt implementeren op het lokale cluster. 

1. Van de **opdracht palet**, selecteer de **Service Fabric: toepassing implementeren (Localhost) opdracht**. De uitvoer van het installatieproces wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS-Code implementeren](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Wanneer de implementatie voltooid is, start u een browser en open Service Fabric Explorer: http://localhost:19080/Explorer. U ziet dat de toepassing wordt uitgevoerd. Dit kan enige tijd duren, dus wees geduld. 

   ![Prestatiemeteritem-servicetoepassing in Service Fabric Explorer](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Nadat u hebt geverifieerd dat de toepassing wordt uitgevoerd, een browser starten en open deze pagina: http://localhost:31002. Dit is de webfront-end van de toepassing. Vernieuw de pagina om te zien van de huidige waarde van het item wordt verhoogd.

   ![Prestatiemeteritem-servicetoepassing in Browser](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Fouten opsporen in de toepassing
Als u fouten opspoort toepassingen in VS-Code, kan de toepassing moet worden uitgevoerd op een lokaal cluster. Onderbrekingspunten kunnen vervolgens worden toegevoegd aan de code.

Stel een onderbrekingspunt en foutopsporing in door de volgende stappen uitvoeren:
1. Open in Verkenner de */src/CounterServiceApplication/CounterService/CounterService.cs* bestands- en stel een onderbrekingspunt in op de regel 62 binnen de `RunAsync` methode.
3. Klik op het pictogram voor foutopsporing in de **activiteit balk** het foutopsporingsprogramma weergave openen in VS-Code. Klik op het pictogram tandwielpictogram boven aan de weergave voor foutopsporing en selecteer **.NET Core** in het vervolgkeuzemenu voor de omgeving. Het bestand launch.json wordt geopend. U kunt dit bestand sluiten. U ziet nu de configuratie-opties in het menu van foutopsporing configuration naast de uitgevoerde knop (groene pijl).

   ![Pictogram in de werkruimte voor VS-Code voor foutopsporing](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Selecteer **.NET Core koppelen** in het menu van de configuratie van foutopsporing.

   ![Pictogram in de werkruimte voor VS-Code voor foutopsporing](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Service Fabric Explorer openen in een browser: http://localhost:19080/Explorer. Klik op **toepassingen** en omlaag dowwn om te bepalen van het primaire knooppunt dat de CounterService op wordt uitgevoerd. In de afbeelding hieronder het primaire knooppunt voor de CounterService is 0-knooppunt.

   ![Primaire knooppunt voor CounterService](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. In VS-Code, klikt u op de uitvoering pictogram (groene pijl) naast de **.NET Core koppelen** foutopsporing configuratie. Selecteer het CounterService proces dat wordt uitgevoerd op het primaire knooppunt dat u in stap 4 hebt geïdentificeerd in het dialoogvenster van de selectie proces.

   ![Primaire proces](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. Het onderbrekingspunt in de *CounterService.cs* bestand zeer snel worden bereikt. U kunt vervolgens de waarden van de lokale variabelen verkennen. Gebruik de werkbalk Foutopsporing aan de bovenkant van de VS Code wilt doorgaan met stap over regels, stap naar methoden, uitvoeren of stap buiten de huidige methode. 

   ![Waarden voor foutopsporing](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Als u wilt de foutopsporingssessie beëindigen, klikt u op het plug-pictogram op de werkbalk Foutopsporing aan de bovenkant van de VS Code...
   
   ![Verbreek de verbinding tussen foutopsporingsprogramma](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Wanneer u klaar bent met het opsporen van fouten, kunt u de **Service Fabric: toepassing verwijderen** opdracht de CounterService toepassing verwijderen uit uw lokale cluster. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [ontwikkelen en testen van Java Service Fabric-toepassingen met tegenover Code](./service-fabric-develop-java-applications-with-vs-code.md).



