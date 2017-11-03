---
title: Configureren van beveiligde verbindingen voor Azure Service Fabric-cluster | Microsoft Docs
description: Informatie over het gebruik van Visual Studio voor het configureren van beveiligde verbindingen die worden ondersteund door de Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: dc07b2f38d6fd2de941ebbe99303f6e63cbf122d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Configureren van beveiligde verbindingen naar een Service Fabric-cluster vanuit Visual Studio
Informatie over het gebruik van Visual Studio veilig toegang krijgen tot een Azure Service Fabric-cluster met beleid voor toegangsbeheer geconfigureerd.

## <a name="cluster-connection-types"></a>Cluster-verbindingstypen
Twee soorten verbindingen worden ondersteund door de Azure Service Fabric-cluster: **niet-beveiligde** verbindingen en **x509 op basis van certificaten** beveiligde verbindingen. (Voor Service Fabric-clusters on-premises gehoste **Windows** en **dSTS** verificaties worden ook ondersteund.) U moet het verbindingstype van het cluster configureren wanneer het cluster wordt gemaakt. Zodra deze gemaakt, kan het verbindingstype kan niet worden gewijzigd.

De Visual Studio Service Fabric-hulpprogramma's ondersteunen alle authenticatietypen voor het verbinden met een cluster voor publicatie. Zie [Service Fabric-cluster instellen vanuit de Azure-portal](service-fabric-cluster-creation-via-portal.md) voor instructies over het instellen van een beveiligde Service Fabric-cluster.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Clusterverbindingen configureren in profielen publiceren
Als u een Service Fabric-project vanuit Visual Studio publiceert, gebruikt u de **Service Fabric-toepassing publiceren** in het dialoogvenster om te kiezen van een Azure Service Fabric-cluster. Onder **verbindingseindpunt**, selecteer een bestaand cluster in uw abonnement.

![De ** publiceren Service Fabric toepassing ** in het dialoogvenster wordt gebruikt voor het configureren van een Service Fabric-verbinding.][publishdialog]

De **Service Fabric-toepassing publiceren** in het dialoogvenster automatisch valideert de cluster-verbinding. Als u wordt gevraagd, moet u zich aanmelden bij uw Azure-account. Als de validatie is geslaagd, betekent dit dat uw systeem de juiste certificaten zijn geïnstalleerd om veilig verbinding kunnen maken met het cluster en of het cluster niet-beveiligde is. Mislukte gegevensvalidatie kunnen worden veroorzaakt door netwerkproblemen of omdat u niet hoeft uw systeem correct is geconfigureerd voor verbinding met een beveiligde cluster.

![De ** publiceren Service Fabric toepassing ** in het dialoogvenster valideert een bestaande correct geconfigureerd verbinding voor Service Fabric-cluster.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligde cluster
1. Zorg ervoor dat u toegang hebt tot een van de clientcertificaten die het doelcluster vertrouwt. Het certificaat wordt gewoonlijk gedeeld als Personal Information Exchange (.pfx)-bestand. Zie [Service Fabric-cluster instellen vanuit de Azure-portal](service-fabric-cluster-creation-via-portal.md) voor het configureren van de server om toegang te verlenen aan een client.
2. Het vertrouwde certificaat installeren. Om dit te doen, dubbelklik op het .pfx-bestand of de PowerShell-script importeren PfxCertificate gebruiken om de certificaten te importeren. Installeer het certificaat naar **Cert: \LocalMachine\My**. Wilt u alle standaardinstellingen accepteren bij het importeren van het certificaat is.
3. Kies de **publiceren...**  opdracht in het snelmenu van het project openen de **Publish Azure Application** in het dialoogvenster en selecteer vervolgens het doelcluster. Het hulpprogramma wordt automatisch de verbinding wordt omgezet en slaat de parameters van de beveiligde verbinding in het publicatieprofiel.
4. Optioneel: U kunt bewerken het publicatieprofiel om verbinding met een beveiligde cluster opgeven.
   
   Omdat u het publiceren profiel XML-bestand voor het opgeven van gegevens van het certificaat en noteer de naam van het certificaat handmatig bewerkt, locatie en de vingerafdruk van certificaat opslaan. U moet deze waarden voor het certificaatarchief van de naam en locatie is opgeslagen. Zie [hoe: ophalen van de vingerafdruk van een certificaat](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) voor meer informatie.
   
   U kunt de *ClusterConnectionParameters* parameters om op te geven van de PowerShell-parameters moet worden gebruikt wanneer verbinding met het Service Fabric-cluster. Geldige parameters zijn die worden geaccepteerd door de cmdlet Connect-ServiceFabricCluster. Zie [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) voor een lijst met beschikbare parameters.
   
   Als u op een externe cluster publiceren wilt, moet u de juiste parameters voor die specifieke cluster opgeven. Hier volgt een voorbeeld van verbinding maken met een niet-beveiligde cluster:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Hier volgt een voorbeeld voor het verbinden met een x509 beveiligde cluster op basis van certificaat:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Eventuele andere vereiste instellingen, zoals parameters voor het bijwerken en toepassing Parameter bestandslocatie, bewerken en vervolgens publiceren van uw toepassing uit de **Service Fabric-toepassing publiceren** dialoogvenster in Visual Studio.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het openen van Service Fabric-clusters [uw cluster visualiseren met behulp van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
