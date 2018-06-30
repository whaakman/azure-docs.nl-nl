---
title: Java Azure Service Fabric-toepassingen met Visual Studio Code te ontwikkelen | Microsoft Docs
description: In dit artikel laat zien hoe maken, implementeren en foutopsporing Java Service Fabric-toepassingen met behulp van Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: JimacoMS
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 54c94c50f6292694e947d97a10fd6976c14e19df
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37115986"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Ontwikkelen van Java Service Fabric-toepassingen met Visual Studio Code

De [Reliable Services van Service Fabric-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) kunt u eenvoudig ontwikkelen van Java Service Fabric-toepassingen op de besturingssystemen Windows, Linux en Mac OS.

In dit artikel leest u hoe te bouwen, implementeren en fouten opsporen in een Java Service Fabric-toepassing met behulp van Visual Studio Code.

> [!IMPORTANT]
> Service Fabric-Java-toepassingen op Windows-machines kunnen worden ontwikkeld, maar kunnen worden gedistribueerd naar alleen Azure Linux-clusters. Java-toepassingen foutopsporing wordt niet ondersteund in Windows.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd tegenover Code, de Service Fabric Reliable Services-extensie voor VS-Code en eventuele afhankelijkheden die nodig zijn voor uw ontwikkelomgeving. Zie voor meer informatie, [aan de slag](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Het voorbeeld downloaden
Dit artikel wordt gebruikt voor de toepassing Voting in de [Service Fabric-Java-toepassing snel voorbeeld GitHub-opslagplaats](https://github.com/Azure-Samples/service-fabric-java-quickstart). 

Als u wilt klonen van de opslagplaats op uw ontwikkelcomputer, voer de volgende opdracht vanaf een terminalvenster (opdrachtvenster in Windows):

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Open de toepassing in VS-Code

Open VS-Code.  Klik op het pictogram Explorer in de **activiteit balk** en klik op **map openen**, of klik op **File -> map openen**. Navigeer naar de *./service-fabric-java-quickstart/Voting* directory in de map waar u de opslagplaats vervolgens gekloond, klikt u op **OK**. De werkruimte moet dezelfde bestanden die wordt weergegeven in de onderstaande schermafbeelding bevatten.

![Uw stem Java-toepassing in de werkruimte](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>De toepassing bouwen

1. Druk op (Ctrl + Shift + p) openen de **opdracht palet** in VS-Code.
2. Zoek en selecteer de **Service Fabric: toepassing bouwen** opdracht. De builduitvoer wordt verzonden naar de geïntegreerde terminal.

   ![Toepassingsopdracht in VS-Code maken](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>De toepassing op het lokale cluster implementeren
Nadat u de toepassing hebt gemaakt, kunt u deze kunt implementeren op het lokale cluster. 

> [!IMPORTANT]
> Java-toepassingen met het lokale cluster implementeren wordt niet ondersteund op Windows-machines.

1. Van de **opdracht palet**, selecteer de **Service Fabric: toepassing implementeren (Localhost) opdracht**. De uitvoer van het installatieproces wordt verzonden naar de geïntegreerde terminal.

   ![Toepassingsopdracht in VS-Code implementeren](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Wanneer de implementatie voltooid is, start u een browser en open Service Fabric Explorer: http://localhost:19080/Explorer. U ziet dat de toepassing wordt uitgevoerd. Dit kan enige tijd duren, dus wees geduld. 

   ![Stemtoepassing in Service Fabric Explorer](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Nadat u hebt geverifieerd dat de toepassing wordt uitgevoerd, een browser starten en open deze pagina: http://localhost:8080. Dit is de webfront-end van de toepassing. U kunt items toevoegen en klik op laten stemmen.

   ![Stemtoepassing in Browser](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Selecteer de toepassing te verwijderen uit het cluster, de **Service Fabric: toepassing verwijderen** opdracht van de **opdracht palet**. De uitvoer van het verwijderingsproces wordt verzonden naar de geïntegreerde terminal. U kunt Service Fabric Explorer gebruiken om te controleren of de toepassing is verwijderd uit het lokale cluster.

## <a name="debug-the-application"></a>Fouten opsporen in de toepassing
Als u fouten opspoort toepassingen in VS-Code, kan de toepassing moet worden uitgevoerd op een lokaal cluster. Onderbrekingspunten kunnen vervolgens worden toegevoegd aan de code.

> [!IMPORTANT]
> Java-toepassingen foutopsporing wordt niet ondersteund op Windows-machines.

Als u de VotingDataService en de toepassing Voting voor foutopsporing worden opgehaald, moet u de volgende stappen uitvoeren:

1. Update de *Voting/VotingApplication/VotingDataServicePkg/Code/entryPoint.sh* bestand.
De opdracht op regel 6 (gebruik '#') uitcommentariëren en de volgende opdracht toevoegen aan de onderkant van het bestand:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. Update de *Voting/VotingApplication/ApplicationManifest.xml* bestand. Stel de **MinReplicaSetSize** en de **TargetReplicaSetSize** kenmerken aan '1' in de **StatefulService** element:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. Klik op het pictogram voor foutopsporing in de **activiteit balk** het foutopsporingsprogramma weergave openen in VS-Code. Klik op het pictogram tandwielpictogram boven aan de weergave voor foutopsporing en selecteer **Java** in het vervolgkeuzemenu voor de omgeving. Het bestand launch.json wordt geopend. 

   ![Pictogram in de werkruimte voor VS-Code voor foutopsporing](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. Stel de waarde van de poort in de configuratie met de naam in het bestand launch.json **foutopsporing (toevoegen)** naar **8001**. Sla het bestand op.

   ![Configuratie voor de launch.json voor foutopsporing](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. De toepassing op het lokale cluster implementeren met behulp van de **Service Fabric: toepassing implementeren (Localhost)** opdracht. Controleren of de toepassing wordt uitgevoerd in Service Fabric Explorer. Uw toepassing is nu gereed voor fouten worden opgespoord.

Stel een onderbrekingspunt in door de volgende stappen uitvoeren:

1. Open in Verkenner de */Voting/VotingDataService/src/statefulservice/VotingDataService.java* bestand. Stel een onderbrekingspunt in op de eerste coderegel in de `try` blokkeren in de `addItem` methode (regel 80).
   
   ![Onderbrekingspunt in uw stem Data Service instellen](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Zorg ervoor dat u onderbrekingspunten instellen op uitvoerbare regels code. Bijvoorbeeld onderbrekingspunten instellen op methodedeclaraties, `try` -instructies of `catch` instructies wordt door het foutopsporingsprogramma worden overgeslagen.
2. Om te beginnen met foutopsporing, klikt u op het pictogram voor foutopsporing in de **activiteit balk**, selecteer de **foutopsporing (toevoegen)** configuratie in het menu Foutopsporing en klik op uitvoeren (groene pijl).

   ![Fouten opsporen (koppeling) configuratie](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Ga in een webbrowser naar http://localhost:8080. Typ een nieuw item in het tekstvak en klik op **+ toevoegen**. Uw onderbrekingspunt moet worden bereikt. U kunt de werkbalk Foutopsporing gebruiken aan de bovenkant van de VS Code wilt doorgaan met stap over regels, stap naar methoden, uitvoeren of stap buiten de huidige methode. 
   
   ![Onderbrekingspunt](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Als u wilt de foutopsporingssessie beëindigen, klikt u op het plug-pictogram op de werkbalk Foutopsporing boven aan het VS-Code.
   
   ![Verbreek de verbinding tussen foutopsporingsprogramma](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Wanneer u klaar bent met het opsporen van fouten, kunt u de **Service Fabric: toepassing verwijderen** opdracht de Voting toepassing verwijderen uit uw lokale cluster. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [ontwikkelen en testen van C# Service Fabric-toepassingen met tegenover Code](./service-fabric-develop-csharp-applications-with-vs-code.md).
