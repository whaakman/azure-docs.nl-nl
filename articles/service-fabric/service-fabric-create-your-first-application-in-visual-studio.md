---
title: Een betrouwbare Azure Service Fabric-service maken met C#
description: Een betrouwbare Service-toepassing die is gebouwd op Azure Service Fabric met Visual Studio maken, implementeren en er foutopsporing op toepassen.
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
ms.date: 10/04/2017
ms.author: ryanwi
ms.openlocfilehash: a4bada3c099e348e2aa188fe08a67ef170ad50ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Uw eerste stateful betrouwbare Service Fabric-servicetoepassing maken met C#

Lees hoe u uw eerste Service Fabric-toepassing voor .NET in Windows in slechts een paar minuten kunt implementeren. Wanneer u klaar bent, hebt u een lokaal cluster uitgevoerd met een betrouwbare servicetoepassing.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u ervoor dat u [uw ontwikkelingsomgeving hebt ingesteld](service-fabric-get-started.md). Dit omvat het installeren van de Service Fabric SDK en Visual Studio 2017 of 2015.

## <a name="create-the-application"></a>De toepassing maken

Start Visual Studio als **beheerder**.

Maak een project met `CTRL`+`SHIFT`+`N`

Kies in het dialoogvenster **Nieuw Project** de optie **Cloud > Service Fabric-toepassing**.

Noem de toepassing **MyApplication** en druk op **OK**.

   
![Dialoogvenster voor nieuw project in Visual Studio][1]

In het volgende dialoogvenster kunt u elk type Service Fabric-toepassing maken. Kies voor deze snelstartgids **Stateful service**.

Noem de service **MyStatefulService** en druk op **OK**.

![Dialoogvenster voor nieuwe service in Visual Studio][2]


Visual Studio maakt het toepassingsproject en het stateful service-project en geeft deze weer in Solution Explorer.

![Solution Explorer na het maken van de stateful service-toepassing][3]

Het toepassingsproject (**MyApplication**) bevat geen directe code. Het verwijst naar een reeks serviceprojecten. Daarnaast bevat het project drie andere typen inhoud:

* **Profielen publiceren**  
Profielen voor het implementeren van verschillende omgevingen.

* **Scripts**  
PowerShell-script voor het implementeren/bijwerken van uw toepassing.

* **Toepassingsdefinitie**  
Bevat het bestand ApplicationManifest.xml onder *ApplicationPackageRoot* waarin de samenstelling van uw toepassing wordt beschreven. Bijbehorende toepassingsparameterbestanden bevinden zich onder *ApplicationParameters*, dat kan worden gebruikt om omgevingsspecifieke parameters op te geven. Visual Studio selecteert een toepassingsparameterbestand dat is opgegeven in het bijbehorende publicatieprofiel tijdens de implementatie in een specifieke omgeving.
    
Zie [Aan de slag met Reliable Services](service-fabric-reliable-services-quick-start.md) voor een overzicht van de inhoud van het serviceproject.

## <a name="deploy-and-debug-the-application"></a>De toepassing implementeren en fouten opsporen in de toepassing

Nu u een toepassing hebt, kunt u deze uitvoeren.

Druk op `F5` in Visual Studio om de toepassing voor foutopsporing te implementeren.

>[!NOTE]
>De eerste keer dat u de toepassing lokaal uitvoert en implementeert, wordt door Visual Studio een lokaal cluster voor foutopsporing gemaakt. Dit kan enige tijd duren. De status van het maken van het cluster wordt weergegeven in het Visual Studio-uitvoervenster.

Als het cluster gereed is, ontvangt u een melding van de lokale toepassing die het systeemvak voor het cluster beheert en die in de SDK is opgenomen.
   
![Melding van het systeemvak van het lokale cluster][4]

Als de toepassing wordt gestart, geeft Visual Studio automatisch de **diagnostische logboeken met gebeurtenissen** weer, waarin u de traceringsuitvoer van uw services kunt bekijken.
   
![Diagnostische logboeken][5]

De stateful servicesjabloon die wij hebben gebruikt, toont alleen de itemwaarde die wordt verhoogd in de methode `RunAsync` van **MyStatefulService.cs**.

Vouw een van de gebeurtenissen uit voor meer informatie, zoals het knooppunt waarop de code wordt uitgevoerd. In dit geval is het \_Node\_2. Dit kan anders zijn op uw machine.
   
![Details van de gebeurtenissenviewer][6]

Het lokale cluster bevat vijf knooppunten die worden gehost op een enkele computer. In een productieomgeving wordt elk knooppunt gehost op een afzonderlijke fysieke of virtuele machine. We bekijken een van de knooppunten op het lokale cluster om het verlies van gegevens van een machine te simuleren en tegelijkertijd te oefenen met de foutopsporingsfunctie.

Open in de **Solution Explorer** **MyStatefulService.cs**. 

Zoek de `RunAsync`-methode op en stel een onderbrekingspunt in op de eerste regel van de methode.

![Onderbrekingspunt in de stateful service RunAsync-methode][7]

Start **Service Fabric Explorer** door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken en kies **Lokaal cluster beheren**.

![Start Service Fabric Explorer vanuit Lokaal clusterbeheer][systray-launch-sfx]

[**Service Fabric Explorer** ](service-fabric-visualizing-your-cluster.md) biedt een visuele weergave van een cluster. Deze bevat de set toepassingen die erin zijn geïmplementeerd en de set van fysieke knooppunten waaruit het cluster bestaat.

Vouw in het linkerdeelvenster **Cluster > Knooppunten** uit en zoek het knooppunt waarop uw code wordt uitgevoerd.

Klik op **Acties > uitschakelen (opnieuw opstarten)** simuleren een machine opnieuw te starten.

![Een knooppunt in Service Fabric Explorer stoppen][sfx-stop-node]

U ziet uw onderbrekingspunt in Visual Studio, terwijl de berekening die u uitvoerde op één knooppunt naadloos wordt overgenomen door een ander.


Ga nu terug naar de details van de gebeurtenisviewer en bekijk de berichten. De teller blijft oplopen in stappen, zelfs als de gebeurtenissen daadwerkelijk afkomstig zijn uit een ander knooppunt.

![Details van de gebeurtenissenviewer na een failover][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a>Het lokale cluster opschonen (optioneel)

Vergeet niet dat dit lokale cluster echt bestaat. Als u het foutopsporingsprogramma stopt, worden uw toepassingsexemplaar en de registratie van het toepassingstype verwijderd. Het op de achtergrond uitvoeren van het cluster gaat echter gewoon door. Wanneer u klaar bent om het lokale cluster te stoppen, is er een aantal opties.

### <a name="keep-application-and-trace-data"></a>Toepassing behouden en gegevens traceren

Sluit het cluster af door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken en kies vervolgens **Lokaal cluster stoppen**.

### <a name="delete-the-cluster-and-all-data"></a>Het cluster en alle gegevens verwijderen

Verwijder het cluster door met de rechtermuisknop op het systeemvak **Lokaal clusterbeheer** te klikken en kies vervolgens **Lokaal cluster verwijderen**. 

Als u deze optie kiest, implementeert Visual Studio het cluster de volgende keer dat u de toepassing uitvoert opnieuw. Kies deze optie als u het lokale cluster een tijdje niet gaat gebruiken of als u resources moet vrijmaken.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [betrouwbare services](service-fabric-reliable-services-introduction.md).
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
