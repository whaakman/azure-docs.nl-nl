---
title: Microservices van Azure lokaal implementeren en upgraden | Microsoft Docs
description: Ontdek hoe u een lokaal Service Fabric-cluster instelt, een bestaande toepassing implementeert en de toepassing upgradet.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: 359677972c7e1fa3f7435052021ddfae5b1ed85e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Aan de slag met het implementeren en bijwerken van toepassingen op uw lokale cluster
De Azure Service Fabric SDK bevat een volledig lokale ontwikkelingsomgeving  waarmee u snel uw toepassingen kunt implementeren en beheren op een lokaal cluster. In dit artikel maakt u een lokaal cluster, implementeert u een bestaande toepassing en werkt u de toepassing bij naar een nieuwe versie vanuit Windows PowerShell.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u [uw ontwikkelingsomgeving](service-fabric-get-started.md) al hebt ingesteld.
> 
> 

## <a name="create-a-local-cluster"></a>Een lokaal cluster maken
Een Service Fabric-cluster vertegenwoordigt een aantal hardwareresources waarvoor u toepassingen kunt implementeren. Een cluster bestaat meestal uit vijf tot vele duizenden computers. De Service Fabric SDK bevat echter een clusterconfiguratie die op één machine kan worden uitgevoerd.

Het is belangrijk om te weten of het lokale Fabric Service-cluster geen emulator of simulator is. Dit cluster wordt uitgevoerd op dezelfde platformcode als de code op clusters voor meerdere machines. Het enige verschil is dat er platformbewerkingen worden uitgevoerd die doorgaans worden verdeeld over vijf machines op één computer.

Met behulp van de SDK kunt u op twee manieren een lokaal cluster instellen: via een  Windows PowerShell-script of via de app Local Cluster Manager. In deze zelfstudie gebruiken we het PowerShell-script.

> [!NOTE]
> Als u al een lokaal cluster hebt gemaakt door een toepassing te implementeren vanuit Visual Studio hebt gemaakt, kunt u dit gedeelte overslaan.
> 
> 

1. Start een nieuw PowerShell-venster als beheerder.
2. Voer het installatiescript van het  cluster uit wat in de SDK-map staat:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    Het installeren van een cluster duurt even. Nadat Setup is voltooid, ziet u soortgelijke uitvoer als deze:
   
    ![Cluster-installatieuitvoer][cluster-setup-success]
   
    U kunt nu een toepassing implementeren in uw cluster.

## <a name="deploy-an-application"></a>Een app implementeren
De Service Fabric SDK bevat een uitgebreide reeks frameworks en ontwikkelaarstools voor het maken van toepassingen. Zie [uw eerste Service Fabric-toepassing maken in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) als u meer wilt weten over  het maken van toepassingen in Visual Studio.

In deze zelfstudie gebruiken we een bestaande voorbeeldtoepassing (WordCount genoemd), zodat we ons kunnen richten op de beheeraspecten van het platform: implementatie, bewaking en upgrade.

1. Start een nieuw PowerShell-venster als beheerder.
2. Importeer de Service Fabric SDK PowerShell -module,
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. Maak een map waarin u de toepassing opslaat die u downloadt en implementeert, bijvoorbeeld C:\ServiceFabric.
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. [Download de toepassing WordCount](http://aka.ms/servicefabric-wordcountapp) naar de locatie die u hebt gemaakt.  Opmerking: de browser Microsoft Edge slaat het bestand met de extensie *.zip* op.  Wijzig de bestandsextensie in *.sfpkg*.
5. Verbinding maken met het lokale cluster:
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. Maak een nieuwe toepassing door de SDK-implementatieopdracht te gebruiken met een naam en pad naar het toepassingspakket.
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    Als het goed is, wordt de volgende uitvoer weergegeven:
   
    ![Een toepassing implementeren in het lokale cluster][deploy-app-to-local-cluster]
7. Ga naar [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html) om de toepassing te zien. U ziet het volgende:
   
    ![De UI van de geïmplementeerde toepassing][deployed-app-ui]
   
    De toepassing WordCount is eenvoudig. Het bevat JavaScript-code aan de clientzijde voor het genereren van willekeurige "woorden" van vijf tekens, die vervolgens worden doorgegeven aan de toepassing via ASP.NET Web API. Een stateful service houdt het aantal getelde woorden bij. Ze worden gepartitioneerd op basis van het eerste teken van het woord. U vindt de broncode voor de app WordCount in de [klassieke voorbeelden waarmee u aan de slag kunt gaan](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).
   
    De toepassing die we hebben geïmplementeerd, bevat vier partities. Woorden die beginnen met A tot G worden in de eerste partitie opgeslagen, woorden die beginnen met H tot N worden in de tweede partitie opgeslagen, enzovoort.

## <a name="view-application-details-and-status"></a>Details en de status van de toepassing weergeven
Nu de toepassing is geïmplementeerd, gaan we de details van de app in Power Shell bekijken.

1. Een query uitvoeren op alle toepassingen in het cluster:
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    Aannemende dat u alleen de app WordCount hebt geïmplementeerd, ziet u iets dat lijkt op het volgende:
   
    ![Een query uitvoeren op alle geïmplementeerde toepassingen in PowerShell;][ps-getsfapp]
2. Ga naar het volgende niveau door de set van services op te vragen die zijn opgenomen in de toepassing WordCount.
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Lijst met services voor de toepassing in PowerShell][ps-getsfsvc]
   
    Deze toepassing bestaat uit twee services: het webfront-end en de stateful service waarmee de woorden worden beheerd.
3. Bekijk ten slotte de lijst met partities voor WordCountService:
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![De servicepartities in PowerShell weergeven][ps-getsfpartitions]
   
    De reeks opdrachten die u hebt gebruikt, zoals alle Service Fabric PowerShell-opdrachten, zijn beschikbaar voor elk cluster waarmee u verbinding zou kunnen maken, lokaal of op afstand.
   
    U kunt de interactie met het cluster visueel weergeven met het Service Fabric Explorer-webhulpprogramma door te navigeren naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer) in de browser.
   
    ![Toepassingdetails weergeven in Service Fabric Explorer][sfx-service-overview]
   
   > [!NOTE]
   > Zie [Uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) voor meer informatie over Service Fabric Explorer.
   > 
   > 

## <a name="upgrade-an-application"></a>Een upgrade van een app uitvoeren
Service Fabric bevat upgrades zonder downtime door de status van de toepassing te bewaken tijdens de implementatie in het cluster. Voer een eenvoudige upgrade uit van de WordCount-toepassing.

De nieuwe versie van de toepassing telt nu alleen woorden die met een klinker beginnen. Tijdens het uitvoeren van de upgrade zijn er twee zichtbare wijzigingen in de werking van de toepassing. Als eerste moet de snelheid waarmee het aantal groeit vertragen, omdat er minder woorden worden geteld. Ten tweede, aangezien de eerste partitie twee klinkers (A en E) bevat en alle andere partities slechts één bevatten, wordt het aantal uiteindelijk hoger.

1. [Download het WordCount versie 2-pakket](http://aka.ms/servicefabric-wordcountappv2) naar dezelfde locatie waar u het versie 1-pakket hebt gedownload.
2. Ga terug naar het PowerShell-venster en gebruikt de opdracht voor het upgraden van de SDK’s om de nieuwe versie te registreren bij het cluster. U kunt vervolgens de fabric:/WordCount-toepassing upgraden.
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    Wanneer de upgrade van start gaat, ziet u de volgende uitvoer in PowerShell.
   
    ![Voortgang van de upgrade in PowerShell][ps-appupgradeprogress]
3. Tijdens de upgrade kunt u de status controleren vanuit Service Fabric Explorer. Open een browservenster en ga naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Vouw **Toepassingen** in de structuur aan de linkerkant uit en kies **WordCount** en **fabric:/WordCount**. Op het tabblad met essentiële gegevens wordt de status van de upgrade weergegeven terwijl deze de upgradedomeinen van het cluster verwerkt.
   
    ![Voortgang van de upgrade in Service Fabric Explorer][sfx-upgradeprogress]
   
    Terwijl de upgrade wordt uitgevoerd in elk domein, wordt er statuscontroles uitgevoerd om ervoor te zorgen dat de toepassing correct werkt.
4. Als u de eerdere query voor de set met services in de toepassing fabric:/WordCount opnieuw uitvoert, ziet u dat de versie van WordCountService is gewijzigd, maar de versie van WordCountWebService niet:
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Query’s uitvoeren op toepassingsservices na de upgrade][ps-getsfsvc-postupgrade]
   
    In dit voorbeeld wordt uitgelegd hoe toepassingsupgrades worden beheerd met Service Fabric. Alleen de set met services (of code/configuratiepakketten in die services) die zijn gewijzigd wordt gewijzigd, waardoor het upgradeproces sneller en betrouwbaarder wordt.
5. Ga ten slotte terug naar de browser om het gedrag van de nieuwe toepassingsversie te controleren. Zoals verwacht neemt het aantal langzamer toe en wordt er iets meer volume toegewezen aan de eerste partitie.
   
    ![De nieuwe versie van de toepassing bekijken in de browser][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Opschonen
Voordat u afsluit, is het belangrijk om te onthouden dat het lokale cluster echt is. Toepassingen blijven op de achtergrond uitgevoerd worden tot u deze verwijdert.  Afhankelijk van de aard van uw apps, kan een app die wordt uitgevoerd een aanzienlijke hoeveelheid resources op uw computer in beslag nemen. U hebt verschillende mogelijkheden om toepassingen en het cluster te beheren:

1. Voer de volgende opdracht uit als u een afzonderlijke toepassing en alle bijbehorende gegevens wilt verwijderen:
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    Of verwijder de toepassing uit het menu **ACTIES** van Service Fabric Explorer of het contextmenu in de lijst met toepassingen in het linkerdeelvenster.
   
    ![Een toepassing verwijderen in Service Fabric Explorer][sfe-delete-application]
2. Nadat de toepassing uit het cluster is verwijderd, kunt u de registratie van versies 1.0.0 en 2.0.0 van het toepassingstype WordCount ongedaan maken. Verwijderen houdt in dat de toepassingspakketten, met inbegrip van de code en configuratie, worden verwijderd uit het archief van de installatiekopie van het cluster.
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    Of kies in Service Fabric Explorer de optie **Unprovision Type** (Type inrichting verwijderen) voor de toepassing.
3. Als u het cluster wilt verwijderen, maar de toepassingsgegevens en -traceringen wilt behouden, klikt u op **Lokaal cluster stoppen** in het systeemvak.
4. Als u het cluster volledig wilt verwijderen, klikt u op **Lokaal cluster verwijderen** in het systeemvak van de app. Als u dit doet, verloopt de implementatie mogelijk langzamer wanneer u in Visual Studio op F5 drukt. Verwijder het lokale cluster alleen als u van plan bent het lokale cluster een tijd niet te gebruiken of als u resources moet vrijmaken.

## <a name="one-node-and-five-node-cluster-mode"></a>Clustermodus voor één en voor vijf knooppunten
Wanneer u toepassingen ontwikkelt, moet u vaak snel dezelfde bewerkingen uitvoeren, zoals code schrijven, fouten opsporen, code wijzigen en fouten opsporen. Het lokale cluster kan in twee modi worden uitgevoerd om dit proces te optimaliseren: met één knooppunt of met vijf knooppunten. Beide clustermodi hebben hun eigen voordelen. In de clustermodus met vijf knooppunten kunt u werken met een echt cluster. U kunt failover-scenario's testen en werken met meerdere exemplaren en replica's van uw services. De clustermodus met één knooppunt is geoptimaliseerd voor snelle implementaties en om snel services te registreren. Zo kunt u direct code valideren met de runtime Service Fabric.

Een clustermodus met één knooppunt en een clustermodus met vijf knooppunten kunnen beide niet worden gebruikt als een emulator of simulator. Het lokale ontwikkelcluster wordt uitgevoerd op dezelfde platformcode als de code op clusters voor meerdere machines.

> [!WARNING]
> Wanneer u de clustermodus wijzigt, wordt het huidige cluster verwijderd van uw systeem en wordt er een nieuw cluster gemaakt. De gegevens die in het cluster zijn opgeslagen, worden verwijderd wanneer u de clustermodus wijzigt.
> 
> 

Als u de modus wijzigt naar een cluster met één knooppunt, selecteert u **Switch Cluster Mode** in de Local Cluster Manager van Service Fabric.

![De clustermodus wijzigen][switch-cluster-mode]

Of wijzig de clustermodus met behulp van PowerShell:

1. Start een nieuw PowerShell-venster als beheerder.
2. Voer het installatiescript van het  cluster uit wat in de SDK-map staat:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Het installeren van een cluster duurt even. Nadat Setup is voltooid, ziet u soortgelijke uitvoer als deze:
   
    ![Cluster-installatieuitvoer][cluster-setup-success-1-node]

## <a name="next-steps"></a>Volgende stappen
* Nu u een aantal vooraf gemaakt toepassen hebt geïmplementeerd een upgrade hebt uitgevoerd, kunt u [proberen zelf toepassingen te ontwikkelen in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
* Alle acties die zijn uitgevoerd op het lokale cluster in dit artikel kunnen ook worden uitgevoerd op een [Azure-cluster](service-fabric-cluster-creation-via-portal.md).
* De upgrade die wij in dit artikel hebben uitgevoerd was zeer eenvoudig. Zie de [upgradedocumentatie](service-fabric-application-upgrade.md) voor meer informatie over de kracht en flexibiliteit van Service Fabric-upgrades.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
