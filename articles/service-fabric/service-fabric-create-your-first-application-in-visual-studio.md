---
title: Een betrouwbare Azure Service Fabric-service maken met C#
description: Een Reliable Services-toepassing die is gebouwd op Azure Service Fabric met Visual Studio maken, implementeren en er foutopsporing op toepassen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 43f77a1a2e1bbe28bb646aa23c28c253c20e8dda
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Uw eerste stateful Reliable Service Fabric-servicetoepassing maken met C#

Lees hoe u uw eerste Azure Service Fabric-toepassing voor .NET in Windows in slechts een paar minuten kunt implementeren. Wanneer u klaar bent, hebt u een lokaal cluster dat wordt uitgevoerd met een Reliable Services-toepassing.

## <a name="prerequisites"></a>Vereisten

Zorg er voordat u begint voor dat u [uw ontwikkelingsomgeving hebt ingesteld](service-fabric-get-started.md). Dit proces omvat het installeren van de Service Fabric SDK en Visual Studio 2017 of 2015.

## <a name="create-the-application"></a>De toepassing maken

1. Start Visual Studio als beheerder.

2. Maak een project door Ctrl + Shift + N te selecteren.

3. Kies in het dialoogvenster **Nieuw Project** de optie **Cloud** > **Service Fabric-toepassing**.

4. Noem de toepassing **MyApplication**. Selecteer vervolgens **OK**.

   ![Dialoogvenster voor nieuw project in Visual Studio][1]

5. In het volgende dialoogvenster kunt u elk type Service Fabric-toepassing maken. Kies voor deze snelstart **Stateful service**.

6. Noem de service **MyStatefulService**. Selecteer vervolgens **OK**.

    ![Dialoogvenster voor nieuwe service in Visual Studio][2]

    Visual Studio maakt het toepassingsproject en het stateful service-project. Vervolgens worden deze in Solution Explorer weergegeven.

    ![Solution Explorer na het maken van de stateful service-toepassing][3]

    Het toepassingsproject (**MyApplication**) bevat geen code. Het verwijst naar een reeks serviceprojecten. Het heeft ook drie andere typen inhoud:

    * **Profielen publiceren**  
    Profielen voor het implementeren van verschillende omgevingen.

    * **Scripts**  
    PowerShell-scripts voor het implementeren of upgraden van uw toepassing.

    * **Toepassingsdefinitie**  
Bevat het bestand ApplicationManifest.xml onder *ApplicationPackageRoot* waarin de samenstelling van uw toepassing wordt beschreven. Bijbehorende toepassingsparameterbestanden bevinden zich onder *ApplicationParameters*, dat kan worden gebruikt om omgevingsspecifieke parameters op te geven. Visual Studio selecteert een toepassingsparameterbestand dat is opgegeven in het bijbehorende publicatieprofiel.
    
Zie [Aan de slag met Reliable Services](service-fabric-reliable-services-quick-start.md) voor een overzicht van de inhoud van het serviceproject.

## <a name="deploy-and-debug-the-application"></a>De toepassing implementeren en fouten opsporen in de toepassing

Nu u een toepassing hebt, kunt u deze via de volgende stappen uitvoeren, implementeren en er foutopsporing op toepassen.

1. Selecteer F5 in Visual Studio om de toepassing voor foutopsporing te implementeren.

    >[!NOTE]
    >De eerste keer dat u de toepassing lokaal uitvoert en implementeert, wordt door Visual Studio een lokaal cluster voor foutopsporing gemaakt. Dit kan enige tijd duren. De status van het maken van het cluster wordt weergegeven in het Visual Studio-uitvoervenster.

    Als het cluster gereed is, ontvangt u een melding van de lokale toepassing die het systeemvak voor het cluster beheert en die in de SDK is opgenomen.
    
    ![Melding van het systeemvak van het lokale cluster][4]

    Als de toepassing is gestart, geeft Visual Studio automatisch de diagnostische logboeken met gebeurtenissen weer, waarin u de traceringsuitvoer van uw services kunt bekijken.
    
    ![Diagnostische logboeken][5]

    >[!NOTE]
    >Tracering wordt automatisch gestart door gebeurtenissen in de viewer voor diagnostische gebeurtenissen. Als u de viewer voor diagnostische gebeurtenissen handmatig wilt configureren, opent u eerst het bestand `ServiceEventSource.cs`, dat zich in het project **MyStatefulService** bevindt. Kopieer de waarde van het kenmerk `EventSource` aan het begin van de klasse `ServiceEventSource`. In het volgende voorbeeld heeft de gebeurtenisbron de naam `"MyCompany-MyApplication-MyStatefulService"`, maar dit kan een andere naam zijn in uw situatie.
>
    >![Naam van bron van servicegebeurtenis zoeken][service-event-source-name]


2. Open vervolgens het dialoogvenster **ETW-providers**. Selecteer vervolgens het tandwielpictogram op het tabblad **Diagnostische gebeurtenissen**. Plak de naam van de gegevensbron die u hebt gekopieerd in het invoervak **ETW-providers**. Selecteer vervolgens de knop **Toepassen**. Gebeurtenissen worden vanaf nu automatisch getraceerd.

    ![Naam van bron van Diagnostische gebeurtenis instellen][setting-event-source-name]

    Er moeten nu gebeurtenissen worden weergegeven in het venster Diagnostische gebeurtenissen.

    De stateful-servicesjabloon toont alleen de itemwaarde die wordt verhoogd in de methode `RunAsync` van **MyStatefulService.cs**.

3. Vouw een van de gebeurtenissen uit voor meer informatie, zoals het knooppunt waarop de code wordt uitgevoerd. In dit geval is het **\_Node\_0,**. Dit kan anders zijn op uw machine.
   
    ![Details van de gebeurtenissenviewer][6]

4. Het lokale cluster bevat vijf knooppunten die worden gehost op een enkele computer. In een productieomgeving wordt elk knooppunt gehost op een afzonderlijke fysieke of virtuele machine. We bekijken een van de knooppunten op het lokale cluster om het verlies van gegevens van een machine te simuleren en tegelijkertijd te oefenen met de foutopsporingsfunctie van Visual Studio.

5. Open in de **Solution Explorer** **MyStatefulService.cs**. 

6. Zoek de `RunAsync`-methode op en stel een onderbrekingspunt in op de eerste regel van de methode.

    ![Onderbrekingspunt in de stateful service RunAsync-methode][7]

7. Start Service Fabric Explorer door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken en **Lokaal cluster beheren** te selecteren.

    ![Service Fabric Explorer starten vanuit lokaal clusterbeheer][systray-launch-sfx]

    [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) biedt een visuele weergave van een cluster. Deze bevat de set toepassingen die erin zijn geïmplementeerd en de set van fysieke knooppunten waaruit het cluster bestaat.

8. Vouw in het linkerdeelvenster **Cluster** > **Knooppunten** uit en zoek het knooppunt waarop uw code wordt uitgevoerd. Klik op **Acties** > **Uitschakelen (opnieuw opstarten)** om het opnieuw opstarten van een machine te simuleren.

    ![Een knooppunt in Service Fabric Explorer stoppen][sfx-stop-node]

    U ziet uw onderbrekingspunt in Visual Studio, terwijl de berekening die u uitvoerde op één knooppunt naadloos wordt overgenomen door een ander.

9. Ga nu terug naar de details van de gebeurtenisviewer en bekijk de berichten. De teller blijft oplopen in stappen, zelfs als de gebeurtenissen daadwerkelijk afkomstig zijn uit een ander knooppunt.

    ![Details van de gebeurtenissenviewer na een failover][diagnostic-events-viewer-detail-post-failover]

## <a name="clean-up-the-local-cluster-optional"></a>Het lokale cluster opschonen (optioneel)

Vergeet niet dat dit lokale cluster echt bestaat. Als u het foutopsporingsprogramma stopt, worden uw toepassingsexemplaar en de registratie van het toepassingstype verwijderd. Het op de achtergrond uitvoeren van het cluster gaat echter gewoon door. Wanneer u klaar bent om het lokale cluster te stoppen, is er een aantal opties.

### <a name="keep-application-and-trace-data"></a>Toepassing behouden en gegevens traceren

Sluit het cluster af door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken en selecteer vervolgens **Lokaal cluster stoppen**.

### <a name="delete-the-cluster-and-all-data"></a>Het cluster en alle gegevens verwijderen

Verwijder het cluster door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken. Kies vervolgens **Lokaal cluster verwijderen**. 

Als u deze optie kiest, implementeert Visual Studio het cluster de volgende keer dat u de toepassing opnieuw uitvoert. Kies deze optie als u het lokale cluster een tijdje niet gaat gebruiken of als u resources moet vrijmaken.

## <a name="next-steps"></a>Volgende stappen
Lees meer over [Reliable Services](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
