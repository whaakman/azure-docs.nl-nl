---
title: Ontwikkel Java Azure Service Fabric-toepassingen met Visual Studio Code | Microsoft Docs
description: In dit artikel laat zien hoe bouwen, implementeren en fouten opsporen in Service Fabric Java-toepassingen met behulp van Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 7f60371fb533526ef5bdb154d0c08dface9c0d1f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667749"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Service Fabric Java-toepassingen ontwikkelen met Visual Studio Code

De [Service Fabric Reliable Services-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) maakt het eenvoudig ontwikkelen van Java Service Fabric-toepassingen op Windows, Linux en macOS-besturingssystemen.

Dit artikel ziet u hoe u kunt bouwen, implementeren en fouten opsporen in een Java Service Fabric-toepassing met behulp van Visual Studio Code.

> [!IMPORTANT]
> Service Fabric Java-toepassingen op Windows-machines kunnen worden ontwikkeld, maar kunnen worden gedistribueerd naar alleen Azure Linux-clusters. Foutopsporing van Java-toepassingen wordt niet ondersteund op Windows.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd VS Code, de Service Fabric Reliable Services-extensie voor VS Code en eventuele afhankelijkheden die nodig zijn voor uw ontwikkelomgeving. Zie voor meer informatie, [aan de slag](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Het voorbeeld downloaden
In dit artikel wordt gebruikgemaakt van de stemtoepassing in de [GitHub-opslagplaats voor Service Fabric Java-toepassing quickstart voorbeeld](https://github.com/Azure-Samples/service-fabric-java-quickstart). 

Als u wilt klonen van de opslagplaats naar uw ontwikkelmachine, voer de volgende opdracht uit vanuit een terminalvenster (opdrachtvenster op Windows):

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Open de toepassing in VS Code

Open Visual Studio Code.  Klik op het pictogram Explorer in de **Activiteitenbalk** en klikt u op **map openen**, of klik op **File -> map openen**. Navigeer naar de *./service-fabric-java-quickstart/Voting* directory in de map waar u de opslagplaats vervolgens gekloond, klikt u op **OK**. De werkruimte mag dezelfde bestanden die wordt weergegeven in de onderstaande schermafbeelding.

![Voting Java-toepassing in de werkruimte](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>De toepassing bouwen

1. Druk op (Ctrl + Shift + p) te openen de **Command Palette** in VS Code.
2. Zoek en selecteer de **Service Fabric: -Toepassing ontwikkelen** opdracht. Uitvoer van de build wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS Code bouwen](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>De toepassing implementeren in het lokale cluster
Nadat u de toepassing hebt gebouwd, kunt u deze kunt implementeren met het lokale cluster. 

> [!IMPORTANT]
> Implementeren van Java-toepassingen op het lokale cluster wordt niet ondersteund op Windows-machines.

1. Uit de **Command Palette**, selecteer de **Service Fabric: Implementatie van toepassing (Localhost) opdracht**. De uitvoer van het installatieproces wordt verzonden naar de geïntegreerde terminal.

   ![Opdracht van de toepassing in VS Code implementeren](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Wanneer de implementatie voltooid is, start u een browser en opent u Service Fabric Explorer: `http://localhost:19080/Explorer`. U ziet dat de toepassing wordt uitgevoerd. Dit kan enige tijd duren, dus zorg patiënt. 

   ![Stemtoepassing in Service Fabric Explorer](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Nadat u hebt gecontroleerd of de toepassing wordt uitgevoerd, start u een browser en opent u deze pagina: `http://localhost:8080`. Dit is de web-front-end van de toepassing. U kunt items toevoegen en klik op deze om te stemmen.

   ![Stemtoepassing in Browser](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. De toepassing te verwijderen uit het cluster, selecteer de **Service Fabric: Toepassing verwijderen** opdracht uit vanaf de **Command Palette**. De uitvoer van de verwijdering wordt verzonden naar de geïntegreerde terminal. U kunt Service Fabric Explorer gebruiken om te controleren dat de toepassing is verwijderd uit het lokale cluster.

## <a name="debug-the-application"></a>Fouten in de toepassing opsporen
Wanneer foutopsporing in toepassingen in VS Code, moet de toepassing worden uitgevoerd op een lokaal cluster. Onderbrekingspunten kunnen vervolgens worden toegevoegd aan de code.

> [!IMPORTANT]
> Foutopsporing van Java-toepassingen wordt niet ondersteund op Windows-machines.

Om voor te bereiden op de VotingDataService en de Voting-toepassing voor foutopsporing, voert u de volgende stappen uit:

1. Update de *Voting/VotingApplication/VotingDataServicePkg/Code/entryPoint.sh* bestand.
Opmerkingen bij de opdracht op regel 6 (gebruik '#') en voeg de volgende opdracht naar de onderkant van het bestand toe:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. Update de *Voting/VotingApplication/ApplicationManifest.xml* bestand. Stel de **MinReplicaSetSize** en de **TargetReplicaSetSize** kenmerken op '1' in de **StatefulService** element:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. Klik op het pictogram Debug in de **Activiteitenbalk** het foutopsporingsprogramma-weergave openen in VS Code. Klik op het tandwielpictogram aan de bovenkant van de weergave voor foutopsporing en selecteer **Java** in de vervolgkeuzelijst omgeving. Het bestand launch.json wordt geopend. 

   ![Fouten opsporen in pictogram in de VS Code-werkruimte](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. Stel de waarde van de poort in de configuratie met de naam in het bestand launch.json **foutopsporing (koppelen)** naar **8001**. Sla het bestand op.

   ![Fouten opsporen in de configuratie voor de launch.json](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. De toepassing met het lokale cluster implementeren met behulp van de **Service Fabric: (Localhost)-toepassing implementeren** opdracht. Controleren of de toepassing wordt uitgevoerd in Service Fabric Explorer. Uw toepassing is nu klaar om te worden opgespoord.

Als u wilt een onderbrekingspunt instellen, voert u de volgende stappen uit:

1. Openen in Explorer de */Voting/VotingDataService/src/statefulservice/VotingDataService.java* bestand. Stel een onderbrekingspunt in op de eerste regel met code in de `try` blokkeren de `addItem` methode (regel 80).
   
   ![Onderbrekingspunt instellen in uw stem Data Service](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Zorg ervoor dat u onderbrekingspunten instellen op uitvoerbare regels code. Bijvoorbeeld onderbrekingspunten instellen op methodedeclaraties, `try` -instructies of `catch` instructies zal worden gemist door het foutopsporingsprogramma.
2. Als u wilt beginnen met het opsporen van fouten, klikt u op het pictogram Debug in de **Activiteitenbalk**, selecteer de **foutopsporing (koppelen)** configuratie in het menu debug en klikt u op de knop uitvoeren (groene pijl).

   ![Foutopsporing (koppelen) configureren](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. In een webbrowser, gaat u naar `http://localhost:8080`. Typ een nieuw item in het tekstvak in en klikt u op **+ toevoegen**. Uw onderbrekingspunt moet worden bereikt. U kunt de werkbalk Foutopsporing aan de bovenkant van VS Code gebruiken om te gaan, stap over regels en stap methoden, of stap uit de huidige methode. 
   
   ![Onderbrekingspunt bereikt](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Als u de foutopsporingssessie beëindigen, klikt u op het pictogram plug op de werkbalk Foutopsporing aan de bovenkant van VS Code.
   
   ![Foutopsporing voor verbreken](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Wanneer u klaar bent met het opsporen van fouten, kunt u de **Service Fabric: Toepassing verwijderen** opdracht voor het verwijderen van de toepassing Voting uit uw lokale cluster. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [ontwikkelt en debugt C# Service Fabric-toepassingen met VS Code](./service-fabric-develop-csharp-applications-with-vs-code.md).
