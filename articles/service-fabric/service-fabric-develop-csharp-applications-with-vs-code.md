---
title: .NET Core Azure Service Fabric-toepassingen ontwikkelen met Visual Studio Code | Microsoft Docs
description: In dit artikel laat zien hoe bouwen, implementeren en fouten opsporen in .NET Core-Service Fabric-toepassingen met behulp van Visual Studio Code.
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
ms.openlocfilehash: 9da735f10063649222a38498af17e0404137a706
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315348"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Ontwikkel C# Service Fabric-toepassingen met Visual Studio Code

De [Service Fabric Reliable Services-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) gemakkelijk om .NET Core-Service Fabric-toepassingen op Windows, Linux en macOS-besturingssystemen te bouwen.

Dit artikel ziet u hoe u kunt bouwen, implementeren en fouten opsporen in een .NET Core-Service Fabric-toepassing met behulp van Visual Studio Code.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd VS Code, de Service Fabric Reliable Services-extensie voor VS Code en eventuele afhankelijkheden die nodig zijn voor uw ontwikkelomgeving. Zie voor meer informatie, [aan de slag](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Het voorbeeld downloaden
In dit artikel wordt de CounterService toepassing in de [Service Fabric .NET Core aan de slag-voorbeelden voor GitHub-opslagplaats](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Als u wilt klonen van de opslagplaats naar uw ontwikkelmachine, voer de volgende opdracht uit vanuit een terminalvenster (opdrachtvenster op Windows):

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Open de toepassing in VS Code

### <a name="windows"></a>Windows
Met de rechtermuisknop op het pictogram VS Code in het Menu Start en kies **als administrator uitvoeren**. Als u wilt het foutopsporingsprogramma koppelen aan uw services, moet u VS Code als administrator uitvoeren.

### <a name="linux"></a>Linux
Met behulp van de terminal, bladert u naar de /service-fabric-dotnet-core-getting-started/Services/CounterService pad vanuit de map die de toepassing lokaal is gekloond in.

Voer de volgende opdracht om te openen van VS Code als een hoofdgebruiker zodat het foutopsporingsprogramma aan uw services koppelen kunt.
```
sudo code . --user-data-dir='.'
```

De toepassing wordt nu weergegeven in de VS Code-werkruimte.

![Teller-servicetoepassing in de werkruimte](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>De toepassing bouwen
1. Druk op (Ctrl + Shift + p) te openen de **Command Palette** in VS Code.
2. Zoek en selecteer de **Service Fabric: -Toepassing ontwikkelen** opdracht. Uitvoer van de build wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS Code bouwen](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>De toepassing implementeren in het lokale cluster
Nadat u de toepassing hebt gebouwd, kunt u deze kunt implementeren met het lokale cluster. 

1. Uit de **Command Palette**, selecteer de **Service Fabric: Implementatie van toepassing (Localhost) opdracht**. De uitvoer van het installatieproces wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS Code implementeren](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Wanneer de implementatie voltooid is, start u een browser en opent u Service Fabric Explorer: http:\//localhost:19080 / Explorer. U ziet dat de toepassing wordt uitgevoerd. Dit kan enige tijd duren, dus zorg patiënt. 

   ![Teller-servicetoepassing in Service Fabric Explorer](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Nadat u hebt gecontroleerd dat de toepassing wordt uitgevoerd, start u een browser en opent u deze pagina: http:\//localhost:31002. Dit is de web-front-end van de toepassing. Vernieuw de pagina om te zien van de huidige waarde van de teller, zoals wordt opgehoogd.

   ![Teller-servicetoepassing in Browser](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Fouten in de toepassing opsporen
Wanneer foutopsporing in toepassingen in VS Code, moet de toepassing worden uitgevoerd op een lokaal cluster. Onderbrekingspunten kunnen vervolgens worden toegevoegd aan de code.

Om in te stellen een onderbrekingspunt en fouten opsporen, voert u de volgende stappen uit:
1. Openen in Explorer de */src/CounterServiceApplication/CounterService/CounterService.cs* bestands- en stel een onderbrekingspunt in op de regel 62 binnen de `RunAsync` methode.
3. Klik op het pictogram Debug in de **Activiteitenbalk** het foutopsporingsprogramma-weergave openen in VS Code. Klik op het tandwielpictogram aan de bovenkant van de weergave voor foutopsporing en selecteer **.NET Core** in de vervolgkeuzelijst omgeving. Het bestand launch.json wordt geopend. U kunt dit bestand kunt sluiten. U ziet nu de configuratie-opties in het menu voor het configureren van foutopsporing naast de knop uitvoeren (groene pijl).

   ![Fouten opsporen in pictogram in de VS Code-werkruimte](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Selecteer **.NET Core koppelen** in het menu debug-configuratie.

   ![Fouten opsporen in pictogram in de VS Code-werkruimte](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Open Service Fabric Explorer in een browser: http:\//localhost:19080 / Explorer. Klik op **toepassingen** en inzoomen naar beneden om te bepalen van het primaire knooppunt dat de CounterService op wordt uitgevoerd. In de afbeelding hieronder het primaire knooppunt voor de CounterService is 0-knooppunt.

   ![Primaire knooppunt voor CounterService](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. In VS Code, klikt u op het uitvoeren (groene pijl)-pictogram naast de **.NET Core koppelen** foutopsporing configuratie. Selecteer in het dialoogvenster voor het selecteren van proces het CounterService proces dat wordt uitgevoerd op het primaire knooppunt dat u in stap 4 hebt geïdentificeerd.

   ![Primaire proces](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. Het onderbrekingspunt in de *CounterService.cs* bestand zeer snel worden bereikt. U kunt vervolgens de waarden van de lokale variabelen verkennen. Gebruik de werkbalk Foutopsporing aan de bovenkant van VS Code wilt uitvoeren, stap over regels en-methoden, stap doorgaan of stap uit de huidige methode. 

   ![Fouten opsporen in waarden](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Als u de foutopsporingssessie beëindigen, klikt u op de plug-pictogram op de werkbalk Foutopsporing aan de bovenkant van VS Code...
   
   ![Foutopsporing voor verbreken](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Wanneer u klaar bent met het opsporen van fouten, kunt u de **Service Fabric: Toepassing verwijderen** opdracht aan de CounterService toepassing verwijderen uit uw lokale cluster. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [ontwikkelen en fouten opsporen in Service Fabric Java-toepassingen met VS Code](./service-fabric-develop-java-applications-with-vs-code.md).



