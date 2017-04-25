---
title: Een Azure Service Fabric-cluster instellen | Microsoft Docs
description: Quick Start - Maak een Service Fabric-cluster voor Windows of Linux in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 813217567c7e1fa5fc9ac11492933ad0c34553d1
ms.lasthandoff: 04/18/2017


---

# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Uw eerste Service Fabric-cluster maken in Azure
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Op basis van deze Quick Start kunt u een cluster met vijf knooppunten maken voor Windows of Linux. Dit kunt u binnen slechts enkele minuten doen in [Azure Portal](http://portal.azure.com).  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u bij Azure Portal aan via [http://portal.azure.com](http://portal.azure.com).

## <a name="create-the-cluster"></a>Het cluster maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Berekenen** op de blade **Nieuw** en selecteer vervolgens **Service Fabric-cluster** op de blade **Berekenen**.
3. Vul het Service Fabric-formulier **Basisinformatie** in. Bij **Besturingssysteem** selecteert u de versie van Windows of Linux waarop u de clusterknooppunten wilt uitvoeren. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Selecteer een **resourcegroep** of maak een nieuwe. Een resourcegroep is een logische container waarin Azure-resources worden gemaakt en waarin ze collectief worden beheerd. Na het voltooien klikt u op **OK**.

    ![Cluster-installatieuitvoer][cluster-setup-basics]

4. Vul het formulier **Clusterconfiguratie** in.  Bij **Aantal knooppunten** voert u 1 in en bij [Duurzaamheidslaag](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) vult u Brons in.

5. Selecteer **Configureer elk knooppunttype** en vul het formulier **Configuratie van knooppunttypen** in. Op basis van knooppunttypen worden de VM-grootte, het aantal virtuele machines, aangepaste eindpunten en andere instellingen voor de virtuele machines van elk type gedefinieerd. Elk gedefinieerd knooppunttype wordt ingesteld als een afzonderlijke VM-schaalset die wordt gebruikt voor het als set implementeren en beheren van virtuele machines. Elk knooppunttype kan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en verschillende capaciteitsstatistieken hebben.  Het eerste (primaire) knooppunttype is waar Service Fabric-systeemservices worden gehost. Dit type moet vijf of meer virtuele machines bevatten.

    Bij elke implementatie voor productie is [capaciteitsplanning](service-fabric-cluster-capacity.md) van groot belang.  Voor deze Quick Start voert u echter geen toepassingen uit, dus selecteert u de VM-grootte *DS1_v2 Standard*.  Selecteer Zilver als [betrouwbaarheidslaag](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) en selecteer een schaalset van 5 voor de virtuele machine.  

    Met aangepaste eindpunten opent u poorten in Azure Load Balancer, zodat u verbinding kunt maken met toepassingen die op het cluster worden uitgevoerd.  Voer '80, 8172' in om de poorten 80 en 8172 te openen.

    Schakel het selectievakje bij **Geavanceerde instellingen configureren** in. Deze instellingen hebben betrekking op het aanpassen van TCP/HTTP-beheereindpunten, poortbereiken voor toepassingen, [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) en [capaciteitseigenschappen](service-fabric-cluster-resource-manager-metrics.md).    

    Selecteer **OK**.

6. In het formulier **Clusterconfiguratie** stelt u **Diagnostische gegevens** in op **Aan**.  Voor deze Quick Start hoeft u geen eigenschappen op te geven voor de [infrastructuurinstelling](service-fabric-cluster-fabric-settings.md).  In **Infrastructuurversie** selecteert u de modus voor **automatisch** upgraden, zodat Microsoft de versie van de infrastructuurcode van het cluster automatisch bijwerkt.  Stel de modus in op **Handmatig** als u een [ondersteunde versie](service-fabric-cluster-upgrade.md) wilt kiezen om naar te upgraden. 

    ![Configuratie van knooppunttypen][node-type-config]

    Selecteer **OK**.

7. Vul het formulier **Beveiliging** in.  Voor deze Quick Start selecteert u **Onbeveiligd**.  Het wordt echter aanbevolen om een beveiligd cluster te maken voor productieworkloads, omdat iedereen anoniem verbinding kan maken met een onbeveiligd cluster en beheerbewerkingen kan uitvoeren.  

    Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie [Service Fabric-clusterbeveiligingsscenario's](service-fabric-cluster-security.md) voor meer informatie over hoe certificaten worden gebruikt in Service Fabric.  Als u gebruikersverificatie wilt inschakelen met Azure Active Directory of als u certificaten wilt instellen voor toepassingsbeveiliging, [maakt u een cluster op basis van een Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md).

    Selecteer **OK**.

8. Bekijk de samenvatting.  Als u een Resource Manager-sjabloon wilt downloaden dat is gebaseerd op de instellingen die u hebt ingevoerd, selecteert u **Sjabloon en parameters downloaden**.  Selecteer **Maken** om het cluster te maken.

    U kunt de voortgang van het maken in de meldingen bekijken. (Klik op het belpictogram bij de statusbalk rechtsboven in uw scherm.) Als u tijdens het maken van het cluster op **Vastmaken aan Startboard** hebt geklikt, ziet u dat **Service Fabric-cluster implementeren** is vastgemaakt aan het **Startboard**.

## <a name="view-cluster-status"></a>De clusterstatus bekijken
Wanneer uw cluster is gemaakt, kunt u het bekijken via de blade **Overzicht** in de portal. U kunt de gegevens van uw cluster nu bekijken op het dashboard, waaronder het openbare eindpunt van het cluster en een koppeling naar Service Fabric Explorer.

![De clusterstatus][cluster-status]

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a>Het cluster visualiseren met Service Fabric Explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een goed hulpmiddel om een cluster te visualiseren en toepassingen te beheren.  Service Fabric Explorer is een service die in het cluster wordt uitgevoerd.  U kunt de service openen via een webbrowser. Klik daarvoor op de **Service Fabric Explorer**-koppeling van de clusterpagina **Overzicht** in de portal.  U kunt ook het adres rechtstreeks in de browser invoeren: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)

Het clusterdashboard bevat een overzicht van het cluster, inclusief een overzicht van de toepassings- en knooppuntstatus. In de knooppuntweergave ziet u de fysieke indeling van het cluster. Voor elk knooppunt kunt u controleren voor welke toepassingen er op het knooppunt code is geïmplementeerd.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="connect-to-the-cluster-using-powershell"></a>Verbinding maken met het cluster met behulp van PowerShell
Controleer met PowerShell of het cluster actief is.  De Service Fabric PowerShell-module wordt met de [Service Fabric-SDK](service-fabric-get-started.md) geïnstalleerd.  De cmdlet [Connect-ServiceFabricCluster](/powershell/module/ServiceFabric/Connect-ServiceFabricCluster) brengt een verbinding met het cluster tot stand.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Zie [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) (Verbinding maken met een beveiligd cluster) voor meer voorbeelden van clusterverbindingen. Nadat u verbinding hebt gemaakt met het cluster, gebruikt u de cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode) om een lijst weer te geven van de knooppunten in het cluster, evenals statusinformatie voor elk knooppunt. **HealthState** moet *OK* zijn voor elk knooppunt.

```powershell
PS C:\> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  ----------- ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
```

## <a name="remove-the-cluster"></a>Het cluster verwijderen
Een Service Fabric-cluster bevat de clusterresource zelf én andere Azure-resources. Als u een Service Fabric-cluster volledig wilt verwijderen, moet u dus ook alle resources waar het cluster uit bestaat, verwijderen. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen. Zie [Een cluster verwijderen](service-fabric-cluster-delete.md) voor andere manieren om een cluster of enkele (maar niet alle) resources in een resourcegroep te verwijderen

Een resourcegroep verwijderen in Azure Portal:
1. Ga naar het Service Fabric-cluster dat u wilt verwijderen.
2. Klik op de naam van de **resourcegroep** op de pagina met clusterbenodigdheden.
3. Klik op de pagina met **resourcegroepbenodigdheden** op **Verwijderen**. Volg de instructies op die pagina om het verwijderen van de resourcegroep te voltooien.
    ![De resourcegroep verwijderen][cluster-delete]

## <a name="next-steps"></a>Volgende stappen
Nu u een zelfstandig ontwikkelingscluster hebt ingesteld, kunt u het volgende proberen:
* [Een beveiligd cluster maken in de portal](service-fabric-cluster-creation-via-portal.md)
* [Een cluster maken op basis van een sjabloon](service-fabric-cluster-creation-via-arm.md) 
* [Apps implementeren met behulp van PowerShell](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
