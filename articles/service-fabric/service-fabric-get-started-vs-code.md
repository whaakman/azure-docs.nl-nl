---
title: Azure Service Fabric met VS-Code aan de slag | Microsoft Docs
description: Dit artikel bevat een overzicht van Service Fabric-toepassingen met behulp van Visual Studio Code maken.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 367829c269bd1d96e6aa5fab1be008483a4ab5ab
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37115994"
---
# <a name="service-fabric-for-visual-studio-code"></a>Service Fabric voor Visual Studio Code

De [Reliable Services van Service Fabric-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) biedt de hulpprogramma's die nodig zijn om te maken, bouwen en fouten opsporen in Service Fabric-toepassingen op Windows, Linux en Mac OS-besturingssystemen.

In dit artikel biedt een overzicht van de vereisten en installatie van de extensie en het gebruik van de verschillende opdrachten die beschikbaar zijn voor de uitbreiding. 

> [!IMPORTANT]
> Service Fabric-Java-toepassingen op Windows-machines kunnen worden ontwikkeld, maar kunnen worden gedistribueerd naar alleen Azure Linux-clusters. Java-toepassingen foutopsporing wordt niet ondersteund in Windows.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden geïnstalleerd op alle omgevingen.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Service Fabric SDK](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Genereren van yeoman--de juiste genereren voor uw toepassing installeren

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

De volgende vereisten moeten worden geïnstalleerd voor het ontwikkelen van Java:

* [Java SDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (versie 1.8)
* [Gradle](https://gradle.org/install/)
* [Foutopsporing voor Java-Code voor VS-extensie](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) die nodig zijn om op te sporen Java-services. Foutopsporing Java-services wordt ondersteund op Linux alleen. U kunt installeren door te klikken op het pictogram uitbreidingen in de **activiteit balk** in VS-Code en zoeken voor de extensie, of een van de VS Code Marketplace.

De volgende vereisten moeten worden geïnstalleerd voor .NET Core / C#-ontwikkeling:

* [.NET core](https://www.microsoft.com/net/learn/get-started) (versie 2.0.0 of hoger)
* [C# voor Visual Studio Code (via OmniSharp) tegenover Code extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) nodig fouten opsporen in C#-services. U kunt installeren door te klikken op het pictogram uitbreidingen in de **activiteit balk** in VS-Code en zoeken voor de extensie, of een van de VS Code Marketplace.

## <a name="setup"></a>Instellen

1. Open VS-Code.
2. Klik op het pictogram uitbreidingen in de **activiteit balk** tegenover Code aan de linkerkant. Zoek naar 'Service Fabric'. Klik op **installeren** voor de extensie voor Service Fabric Reliable Services.

## <a name="commands"></a>Opdrachten
De Service Fabric Reliable Services-extensie voor VS-Code bevat veel opdrachten waarmee ontwikkelaars maken en implementeren van Service Fabric-projecten. U kunt aanroepen opdrachten uit de **opdracht palet** door te drukken `(Ctrl + Shift + p)`, de opdrachtnaam te typen in de invoer-balk en de gewenste opdracht in de prompt lijst te selecteren. 

* Service Fabric: Toepassing maken 
* Service Fabric: Toepassing publiceren 
* Service Fabric: Toepassing implementeren 
* Service Fabric: Toepassing verwijderen  
* Service Fabric: Toepassing bouwen 
* Service Fabric: Schone-toepassing 

### <a name="service-fabric-create-application"></a>Service Fabric: Toepassing maken

De **Service Fabric: toepassing maken** opdracht maakt u een nieuwe Service Fabric-toepassing in uw huidige werkruimte. Afhankelijk van welke genereren yeoman zijn geïnstalleerd op uw ontwikkelcomputer, kunt u verschillende soorten Service Fabric-toepassing, met inbegrip van Java, C#-Container en Gast projecten. 

1.  Selecteer de **Service Fabric: Service toevoegen** opdracht
2.  Selecteer het type voor de nieuwe Service Fabric-toepassing. 
3.  Voer de naam van de toepassing die u wilt maken
3.  Selecteer het type van de service die u wilt toevoegen aan uw Service Fabric-toepassing. 
4.  Volg de aanwijzingen voor de naam van de service. 
5.  De nieuwe Service Fabric-toepassing wordt weergegeven in de werkruimte.
6.  Open de nieuwe toepassingsmap zodat dit de hoofdmap in de werkruimte wordt. U kunt doorgaan met het uitvoeren van opdrachten via deze locatie.

### <a name="service-fabric-add-service"></a>Service Fabric: Service toevoegen
De **Service Fabric: Service toevoegen** opdracht voegt een nieuwe service aan een bestaande Service Fabric-toepassing. De toepassing die de service worden toegevoegd aan moet de hoofdmap van de werkruimte. 

1.  Selecteer de **Service Fabric: Service toevoegen** opdracht.
2.  Selecteer het type van uw huidige Service Fabric-toepassing. 
3.  Selecteer het type van de service die u wilt toevoegen aan uw Service Fabric-toepassing. 
4.  Volg de aanwijzingen voor de naam van de service. 
5.  De nieuwe service die wordt weergegeven in uw projectmap. 

### <a name="service-fabric-publish-application"></a>Service Fabric: Toepassing publiceren
De **Service Fabric: toepassing publiceren** opdracht uw Service Fabric-toepassing op een externe cluster implementeert. Het doelcluster kan een beveiligde of een niet-beveiligde cluster zijn. Als parameters zijn niet ingesteld in Cloud.json, wordt de toepassing wordt geïmplementeerd op het lokale cluster.

1.  De eerste keer dat de toepassing is gebouwd, wordt een bestand Cloud.json gegenereerd in de projectmap.
2.  Voer de waarden voor het cluster dat u verbinding maken wilt met in het bestand Cloud.json.
3.  Selecteer de **Service Fabric: toepassing publiceren** opdracht.
4.  Bekijk het doelcluster met Service Fabric Explorer om te bevestigen dat de toepassing is geïnstalleerd. 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: (Localhost)-toepassing implementeren
De **Service Fabric: toepassing implementeren** opdracht implementeert uw Service Fabric-toepassing naar uw lokale cluster. Zorg ervoor dat uw lokale cluster voordat u de opdracht wordt uitgevoerd. 

1.  Selecteer de **Service Fabric: toepassing implementeren** opdracht
2.  Het lokale cluster met Service Fabric Explorer weergeven (http://localhost:19080/Explorer) om te bevestigen dat de toepassing is geïnstalleerd. Dit kan enige tijd duren, dus wees geduld.
3.  U kunt ook **Service Fabric: toepassing publiceren** opdracht zonder parameters ingesteld in het bestand Cloud.json om te implementeren op een lokaal cluster.

> [!NOTE]
> Java-toepassingen met het lokale cluster implementeren wordt niet ondersteund op Windows-machines.

### <a name="service-fabric-remove-application"></a>Service Fabric: Toepassing verwijderen
De **Service Fabric: toepassing verwijderen** opdracht verwijdert u een Service Fabric-toepassing uit het cluster al eerder is geïmplementeerd voor het gebruik van de Code van de VS-extensie. 

1.  Selecteer de **Service Fabric: toepassing verwijderen** opdracht.
2.  Met Service Fabric Explorer om te bevestigen dat de toepassing is verwijderd van het cluster weergeven. Dit kan enige tijd duren, dus wees geduld.

### <a name="service-fabric-build-application"></a>Service Fabric: Toepassing bouwen
De **Service Fabric: toepassing verwijderen** opdracht Java of C# Service Fabric-toepassingen kunt maken. 

1.  Zorg ervoor dat u zich in de hoofdmap van de toepassing voordat deze opdracht wordt uitgevoerd. De opdracht geeft het type van de toepassing (C# of Java) en dienovereenkomstig builds van uw toepassing.
2.  Selecteer de **Service Fabric: toepassing bouwen** opdracht.
3.  De uitvoer van deze procedure wordt geschreven naar de geïntegreerde terminal.

### <a name="service-fabric-clean-application"></a>Service Fabric: Schone-toepassing
De **Service Fabric: schone toepassing** opdracht verwijdert u alle jar-bestanden en systeemeigen bibliotheken die zijn gegenereerd door de build. Geldig voor Java-toepassingen. 

1.  Zorg ervoor dat u zich in de hoofdmap van de toepassing voordat deze opdracht wordt uitgevoerd. 
2.  Selecteer de **Service Fabric: schone toepassing** opdracht.
3.  De uitvoer van het nieuwe proces wordt geschreven naar de geïntegreerde terminal.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [ontwikkelen en testen van C# Service Fabric-toepassingen met tegenover Code](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Meer informatie over hoe [ontwikkelen en testen van Java Service Fabric-toepassingen met tegenover Code](./service-fabric-develop-java-applications-with-vs-code.md).
