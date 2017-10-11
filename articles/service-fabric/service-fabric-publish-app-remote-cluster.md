---
title: Een app publiceren naar een externe cluster met Visual Studio | Microsoft Docs
description: Informatie over het publiceren van een toepassing op een externe service fabric-cluster met behulp van Visual Studio.
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: c440c520d84fc503ff9e705555449e92555d4721
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Implementeren en verwijderen van toepassingen met Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

De Azure Service Fabric-extensie voor Visual Studio biedt een eenvoudige, herhaalbare en scriptbare manier voor het publiceren van een toepassing naar een Service Fabric-cluster.

## <a name="the-artifacts-required-for-publishing"></a>De artefacten die vereist zijn voor publicatie
### <a name="deploy-fabricapplicationps1"></a>Implementeer FabricApplication.ps1
Dit is een PowerShell-script die gebruikmaakt van een profielpad publiceren als een parameter voor publishing Service Fabric-toepassingen. Omdat dit script deel van uw toepassing uitmaakt, bent u Welkom bij de Wijzig deze zo nodig zijn voor uw toepassing.

### <a name="publish-profiles"></a>Profielen publiceren
Een map in het project Service Fabric-toepassing met de naam **PublishProfiles** bevat XML-bestanden die voor het opslaan van essentiële informatie voor het publiceren van een toepassing, zoals:

* Verbindingsparameters service Fabric-cluster
* Pad naar een bestand van de parameter toepassing
* Upgrade-instellingen

Standaard uw toepassing bevat drie profielen publiceren: Local.1Node.xml Local.5Node.xml en Cloud.xml. U kunt meer profielen toevoegen door te kopiëren en plakken van een van de standaard-bestanden.

### <a name="application-parameter-files"></a>Parameter voor toepassingsbestanden
Een map in het project Service Fabric-toepassing met de naam **ApplicationParameters** XML-bestanden voor de gebruiker opgegeven application manifest parameterwaarden bevat. De parameters van Application manifest-bestanden kunnen worden gebruikt zodat u andere waarden voor de implementatie-instellingen gebruiken kunt. Zie voor meer informatie over parameters voorzien van uw toepassing, [omgevingen met meerdere in Service Fabric beheren](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Voor actorservices, moet u het project eerst voordat u het bestand in een teksteditor of via het dialoogvenster publiceren bewerken bouwen. Dit is omdat het deel van de manifest-bestanden wordt gegenereerd tijdens het opbouwen.

## <a name="to-publish-an-application-using-the-publish-service-fabric-application-dialog-box"></a>Voor een toepassing publiceren via het dialoogvenster Service Fabric-toepassing publiceren
De volgende stappen laten zien hoe u voor het publiceren van een toepassing met behulp van de **Service Fabric-toepassing publiceren** geleverd door de Visual Studio Tools van het Fabric-Service in het dialoogvenster.

1. Kies in het snelmenu van het project Service Fabric-toepassing **publiceren...** om weer te geven de **Service Fabric-toepassing publiceren** in het dialoogvenster.
   
    ![De ** publiceren Service Fabric toepassing ** in het dialoogvenster][0]
   
    Het geselecteerde bestand in de **profiel als doel** vervolgkeuzelijst is waar u alle instellingen, met uitzondering van **Manifest versies**, worden opgeslagen. U kunt een bestaand profiel gebruiken of een nieuwe maken door te kiezen **< profielen beheren... >** in de **profiel als doel** vervolgkeuzelijst. Wanneer u een publicatieprofiel kiest, wordt de inhoud ervan weergegeven in de overeenkomende velden in het dialoogvenster. Sla uw wijzigingen op elk gewenst moment, kies de **profiel opslaan** koppeling.    
2. In de **verbindingseindpunt** sectie, geeft u een lokale of externe Service Fabric-cluster van publishing eindpunt. Als u wilt toevoegen of wijzigen van het verbindingseindpunt, klikt u op de **verbindingseindpunt** vervolgkeuzelijst. De lijst bevat de beschikbare Service Fabric-cluster eindpunten van de verbinding die u kunt publiceren op basis van uw Azure-abonnementen. Houd er rekening mee dat als u bent niet al aangemeld bij Visual Studio, wordt u gevraagd om dit te doen.
   
    Gebruik het dialoogvenster voor selectie van cluster om te kiezen uit de set beschikbare abonnementen en -clusters.
   
    ![De ** Selecteer Service Fabric Cluster ** in het dialoogvenster][1]
   
   > [!NOTE]
   > Als u wilt publiceren naar een willekeurig eindpunt (zoals een cluster partij), raadpleegt u de **publiceren naar een willekeurig clustereindpunt** hieronder.
   > 
   > 
   
    Wanneer u een eindpunt kiest, wordt de verbinding met het geselecteerde Service Fabric-cluster in Visual Studio gevalideerd. Als het cluster niet is beveiligd, kunt Visual Studio verbinden met deze onmiddellijk. Echter, als het cluster beveiligd is, u moet een certificaat installeren op uw lokale computer voordat u doorgaat. Zie [het configureren van beveiligde verbindingen](service-fabric-visualstudio-configure-secure-connections.md) voor meer informatie. Als u bent klaar, kiest u de **OK** knop. Het geselecteerde cluster wordt weergegeven in de **Service Fabric-toepassing publiceren** in het dialoogvenster.
3. In de **toepassing parameterbestand** vervolgkeuzelijst vak, gaat u naar een parameterbestand. Een parameterbestand toepassing bevat de gebruiker opgegeven waarden voor parameters in het manifestbestand van de toepassing. Als u wilt toevoegen of wijzigen van een parameter, kies de **bewerken** knop. Typ of wijzig de waarde van de parameter in de **Parameters** raster. Als u bent klaar, kiest u de **opslaan** knop.
   
    ![De ** bewerken Parameters ** in het dialoogvenster][2]
4. Gebruik de **Upgrade van de toepassing** selectievakje in om op te geven of deze actie publiceren is een upgrade. Upgrade publiceert acties die afwijken van de normale publiceert acties. Zie [Service Fabric-toepassing upgraden](service-fabric-application-upgrade.md) voor een lijst van verschillen. Upgrade om instellingen te configureren, kiest u de **instellingen configureren voor Upgrade** koppeling. De editor upgradeparameter weergegeven. Zie [configureren van de upgrade van een Service Fabric-toepassing](service-fabric-visualstudio-configure-upgrade.md) voor meer informatie over parameters voor het bijwerken.
5. Kies de **Manifest versies...** knop om weer te geven de **versies bewerken** in het dialoogvenster. U moet bijwerken van de toepassing en Serviceversies voor een upgrade te kunnen uitvoeren. Zie [upgrade zelfstudie over Service Fabric](service-fabric-application-upgrade-tutorial.md) voor meer informatie over welke toepassing en service manifest versies gevolgen hebben voor een upgradeproces.
   
    ![De ** bewerken versies ** in het dialoogvenster][3]
   
    Als de toepassing en Serviceversies semantische versioning zoals 1.0.0 of numerieke waarden in de indeling van 1.0.0.0, selecteer de **automatisch bijwerken van toepassing en Serviceversies** optie. Wanneer u deze optie, de service en toepassing versienummers worden automatisch bijgewerkt wanneer een code, de configuratie of de gegevens van het pakket versie is bijgewerkt. Als u liever handmatig bewerken van de versies, schakelt u het selectievakje in als deze functie wilt uitschakelen.
   
   > [!NOTE]
   > Voor alle pakket-vermeldingen weergegeven voor een actor-project, moet u eerst het project voor het genereren van de vermeldingen in de Service Manifest-bestanden maken.
   > 
   > 
6. Wanneer u klaar bent geven alle benodigde instellingen, kies de **publiceren** knop voor het publiceren van uw toepassing met het geselecteerde Service Fabric-cluster. De instellingen die u hebt opgegeven, worden toegepast op het publicatieproces.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publiceren naar een willekeurig clustereindpunt (met inbegrip van derden clusters)
De Visual Studio ervaring publiceren is geoptimaliseerd voor publicatie naar externe clusters die zijn gekoppeld aan een van uw Azure-abonnementen. Het is echter mogelijk om te publiceren op een willekeurige eindpunten (zoals Service Fabric-clusters voor derden) door het publicatieprofiel XML rechtstreeks te bewerken. Zoals hierboven wordt beschreven, drie profielen publiceren standaard--**Local.1Node.xml**, **Local.5Node.xml**, en **Cloud.xml**--, maar u Welkom bij het maken van aanvullende profielen voor verschillende omgevingen. Bijvoorbeeld, kunt u een profiel maken voor publicatie naar derden clusters, mogelijk met de naam **Party.xml**.

Als u verbinding maakt met een niet-beveiligde cluster, alle die vereist is van het verbindingseindpunt cluster zoals `partycluster1.eastus.cloudapp.azure.com:19000`. In dat geval het verbindingseindpunt in het publicatieprofiel zou als volgt uitzien:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Als u verbinding met een beveiligde cluster maakt, moet u ook de gegevens van het certificaat uit het lokale archief moet worden gebruikt voor verificatie. Zie voor meer informatie [configureren van beveiligde verbindingen naar een Service Fabric-cluster](service-fabric-visualstudio-configure-secure-connections.md).

  Zodra uw publicatieprofiel is ingesteld, kunt u het raadplegen in het dialoogvenster publish zoals hieronder wordt weergegeven.

  ![Nieuwe publicatieprofiel in het dialoogvenster publish][4]

  Houd er rekening mee dat in dit geval wordt het nieuwe publicatieprofiel naar een van de standaardbestanden voor de parameter van toepassing verwijst. Dit is geschikt als u wilt publiceren dezelfde configuratie van de toepassing naar een aantal omgevingen. Daarentegen in gevallen waar u verschillende configuraties voor elke omgeving die u publiceren wilt naar zou het zinvol voor het maken van een bijbehorende parameterbestand van toepassing.

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over het automatiseren van het publicatieproces in een omgeving met continue integratie, [Stel doorlopende integratie van Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
