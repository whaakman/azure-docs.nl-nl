---
title: Azure Service Fabric met Visual Studio Code aan de slag | Microsoft Docs
description: In dit artikel wordt een overzicht van het maken van Service Fabric-toepassingen met behulp van Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 1053bde5eb76a9e2731d9663dec97d51c321e907
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500216"
---
# <a name="service-fabric-for-visual-studio-code"></a>Service Fabric voor Visual Studio Code

De [Service Fabric Reliable Services-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) biedt de hulpprogramma's die nodig zijn om te maken, bouwen en fouten opsporen in Service Fabric-toepassingen op Windows, Linux en macOS-besturingssystemen.

Dit artikel bevat een overzicht van de vereisten en installatie van de extensie en het gebruik van de verschillende opdrachten die worden geleverd door de extensie. 

> [!IMPORTANT]
> Service Fabric Java-toepassingen op Windows-machines kunnen worden ontwikkeld, maar kunnen worden gedistribueerd naar alleen Azure Linux-clusters. Foutopsporing van Java-toepassingen wordt niet ondersteund op Windows.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden geïnstalleerd op alle omgevingen.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Service Fabric SDK](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman-generatoren--de juiste generatoren voor uw toepassing installeren

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

De volgende vereisten moeten worden geïnstalleerd voor Java-ontwikkeling:

* [Java SDK](https://aka.ms/azure-jdks) (versie 1.8)
* [Gradle](https://gradle.org/install/)
* [Foutopsporing voor Java VS Code-extensie](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) die nodig zijn om op te sporen Java-services. Foutopsporing van Java-services wordt ondersteund op Linux alleen. U kunt installeren door te klikken op het pictogram extensies in de **Activiteitenbalk** in VS Code en te zoeken of voor de extensie van de VS Code Marketplace.

De volgende vereisten moeten worden geïnstalleerd voor .NET Core /C# ontwikkeling:

* [.NET core](https://www.microsoft.com/net/learn/get-started) (versie 2.0.0 of hoger)
* [C#voor Visual Studio Code (van omnisharp) VS Code-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) die nodig zijn om op te sporen C# services. U kunt installeren door te klikken op het pictogram extensies in de **Activiteitenbalk** in VS Code en te zoeken of voor de extensie van de VS Code Marketplace.

## <a name="setup"></a>Instellen

1. Open Visual Studio Code.
2. Klik op het pictogram extensies in de **Activiteitenbalk** VS Code aan de linkerkant. Zoek 'Service Fabric'. Klik op **installeren** voor de Service Fabric Reliable Services-extensie.

## <a name="commands"></a>Opdrachten
De Service Fabric Reliable Services-extensie voor VS Code biedt veel opdrachten waarmee ontwikkelaars maken en implementeren van Service Fabric-projecten. U kunt aanroepen opdrachten van de **Command Palette** door te drukken `(Ctrl + Shift + p)`, typt u de naam van de opdracht in de invoer-balk en de gewenste opdracht in de lijst met vragen te selecteren. 

* Service Fabric: Toepassing maken 
* Service Fabric: Toepassing publiceren 
* Service Fabric: Toepassing implementeren 
* Service Fabric: Toepassing verwijderen  
* Service Fabric: -Toepassing ontwikkelen 
* Service Fabric: Toepassing opgeschoond 

### <a name="service-fabric-create-application"></a>Service Fabric: Toepassing maken

De **Service Fabric: Toepassing maken** opdracht maakt u een nieuwe Service Fabric-toepassing in de huidige werkruimte. Afhankelijk van welke yeoman-generatoren zijn geïnstalleerd op uw ontwikkelcomputer, kunt u verschillende soorten Service Fabric-toepassing, waaronder Java, C#, Container, en Gast-projecten. 

1.  Selecteer de **Service Fabric: Service toevoegen** opdracht
2.  Selecteer het type voor uw nieuwe Service Fabric-toepassing. 
3.  Voer de naam van de toepassing die u wilt maken
3.  Selecteer het type van de service die u wilt toevoegen aan uw Service Fabric-toepassing. 
4.  Volg de aanwijzingen om de naam van de service. 
5.  De nieuwe Service Fabric-toepassing wordt weergegeven in de werkruimte.
6.  Open de map van het nieuwe zodat dit de hoofdmap van de werkruimte wordt. U kunt doorgaan met het uitvoeren van opdrachten hier.

### <a name="service-fabric-add-service"></a>Service Fabric: Service toevoegen
De **Service Fabric: Service toevoegen** opdracht voegt een nieuwe service toe aan een bestaande Service Fabric-toepassing. De toepassing die de service wordt toegevoegd aan moet de hoofdmap van de werkruimte. 

1.  Selecteer de **Service Fabric: Service toevoegen** opdracht.
2.  Selecteer het type van uw huidige Service Fabric-toepassing. 
3.  Selecteer het type van de service die u wilt toevoegen aan uw Service Fabric-toepassing. 
4.  Volg de aanwijzingen om de naam van de service. 
5.  De nieuwe service wordt weergegeven in uw projectmap. 

### <a name="service-fabric-publish-application"></a>Service Fabric: Toepassing publiceren
De **Service Fabric: Toepassing publiceren** opdracht wordt uw Service Fabric-toepassing op een extern cluster geïmplementeerd. Het doelcluster kan een beveiligde of een onbeveiligd cluster zijn. Als parameters zijn niet ingesteld in Cloud.json, wordt de toepassing wordt geïmplementeerd op het lokale cluster.

1.  De eerste keer is dat de toepassing is gemaakt, wordt een bestand Cloud.json gegenereerd in de projectmap.
2.  Voer de waarden voor het cluster dat u verbinding maken wilt met in het bestand Cloud.json.
3.  Selecteer de **Service Fabric: Toepassing publiceren** opdracht.
4.  Bekijk het doelcluster met Service Fabric Explorer om te bevestigen dat de toepassing is geïnstalleerd. 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: (Localhost)-toepassing implementeren
De **Service Fabric: Toepassing implementeren** opdracht wordt uw Service Fabric-toepassing naar uw lokale cluster geïmplementeerd. Zorg ervoor dat uw lokale cluster voordat u de opdracht wordt uitgevoerd. 

1. Selecteer de **Service Fabric: Toepassing implementeren** opdracht
2. Het lokale cluster met Service Fabric Explorer weergeven (http:\//localhost:19080 / Explorer) om te bevestigen dat de toepassing is geïnstalleerd. Dit kan enige tijd duren, dus zorg patiënt.
3. U kunt ook **Service Fabric: Toepassing publiceren** opdracht zonder parameters die zijn ingesteld in het bestand Cloud.json om te implementeren op een lokaal cluster.

> [!NOTE]
> Implementeren van Java-toepassingen op het lokale cluster wordt niet ondersteund op Windows-machines.

### <a name="service-fabric-remove-application"></a>Service Fabric: Toepassing verwijderen
De **Service Fabric: Toepassing verwijderen** opdracht verwijdert u een Service Fabric-toepassing uit het cluster dat eerder is geïmplementeerd voor het gebruik van de VS Code-extensie. 

1.  Selecteer de **Service Fabric: Toepassing verwijderen** opdracht.
2.  Met Service Fabric Explorer om te bevestigen dat de toepassing is verwijderd van het cluster weergeven. Dit kan enige tijd duren, dus zorg patiënt.

### <a name="service-fabric-build-application"></a>Service Fabric: -Toepassing ontwikkelen
De **Service Fabric: Toepassing verwijderen** opdracht beide Java kunt bouwen of C# Service Fabric-toepassingen. 

1.  Zorg ervoor dat u zich in de hoofdmap van de toepassing voordat u deze opdracht uitvoert. De opdracht geeft het type van de toepassing (C# of Java) en bouwt u uw toepassing dienovereenkomstig.
2.  Selecteer de **Service Fabric: -Toepassing ontwikkelen** opdracht.
3.  De uitvoer van het bouwproces wordt geschreven naar de geïntegreerde terminal.

### <a name="service-fabric-clean-application"></a>Service Fabric: Toepassing opgeschoond
De **Service Fabric: Toepassing opgeschoond** opdracht verwijdert u alle jar-bestanden en eigen bibliotheken die zijn gegenereerd door de build. Geldig voor Java-toepassingen. 

1.  Zorg ervoor dat u zich in de hoofdmap van de toepassing voordat u deze opdracht uitvoert. 
2.  Selecteer de **Service Fabric: Toepassing opgeschoond** opdracht.
3.  De uitvoer van het nieuwe proces wordt geschreven naar de geïntegreerde terminal.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [ontwikkelt en debugt C# Service Fabric-toepassingen met VS Code](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Meer informatie over het [ontwikkelen en fouten opsporen in Service Fabric Java-toepassingen met VS Code](./service-fabric-develop-java-applications-with-vs-code.md).
