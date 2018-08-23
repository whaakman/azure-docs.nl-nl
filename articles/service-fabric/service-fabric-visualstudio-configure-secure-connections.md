---
title: Beveiligde verbindingen van Azure Service Fabric-cluster configureren | Microsoft Docs
description: Leer hoe u Visual Studio gebruiken voor beveiligde verbindingen configureren die worden ondersteund door de Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: 8d76a2144234591792359ed8dd4a0779e6a2fc5c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42058059"
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Beveiligde verbindingen configureren met een Service Fabric-cluster vanuit Visual Studio
Informatie over het gebruik van Visual Studio veilig toegang krijgen tot een Azure Service Fabric-cluster met beleid voor toegangsbeheer geconfigureerd.

## <a name="cluster-connection-types"></a>Cluster-verbindingstypen
Twee typen verbindingen worden ondersteund door de Azure Service Fabric-cluster: **niet-beveiligde** verbindingen en **x509 op basis van certificaten** beveiligde verbindingen. (Voor Service Fabric-clusters die worden gehost on-premises **Windows** en **dSTS** verificaties worden ook ondersteund.) U moet het verbindingstype van het cluster configureren wanneer het cluster wordt gemaakt. Zodra deze gemaakt, kan het verbindingstype kan niet worden gewijzigd.

De Visual Studio Service Fabric-hulpprogramma's bieden ondersteuning voor alle verificatietypen voor het verbinden met een cluster voor publicatie. Zie [instellen van een Service Fabric-cluster in Azure portal](service-fabric-cluster-creation-via-portal.md) voor instructies over het instellen van een beveiligd Service Fabric-cluster.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Clusterverbindingen configureren in profielen publiceren
Als u een Service Fabric-project in Visual Studio publiceert, gebruikt u de **Service Fabric-toepassing publiceren** in het dialoogvenster voor het kiezen van een Azure Service Fabric-cluster. Onder **verbindingseindpunt**, selecteert u een bestaand cluster in uw abonnement.

![De ** publiceert Service Fabric Application ** in het dialoogvenster wordt gebruikt om een Service Fabric-verbinding te configureren.][publishdialog]

De **Service Fabric-toepassing publiceren** in het dialoogvenster automatisch valideert de clusterverbinding. Als u hierom wordt gevraagd, moet u zich aanmelden bij uw Azure-account. Als de validatie is geslaagd, betekent dit dat het systeem de juiste certificaten zijn geïnstalleerd heeft om veilig verbinding maken met het cluster of het cluster niet-beveiligde is. Validatiefouten kunnen worden veroorzaakt door netwerkproblemen of omdat u niet hoeft uw systeem correct zijn geconfigureerd voor het verbinding maken met een beveiligd cluster.

![De ** publiceert Service Fabric Application ** in het dialoogvenster valideert een bestaande correct geconfigureerd verbinding van Service Fabric-cluster.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster
1. Zorg ervoor dat u hebt toegang tot een van de clientcertificaten die het doelcluster vertrouwt. Het certificaat wordt gewoonlijk gedeeld als een Personal Information Exchange (PFX)-bestand. Zie [instellen van een Service Fabric-cluster in Azure portal](service-fabric-cluster-creation-via-portal.md) voor het configureren van de server om toegang te verlenen aan een client.
2. Installeer het vertrouwde certificaat. Dubbelklik op het pfx-bestand om dit te doen, of de Import-PfxCertificate-PowerShell-script gebruiken om te importeren van de certificaten. Het certificaat te installeren **Cert: \LocalMachine\My**. Wilt u alle standaardinstellingen accepteren tijdens het importeren van het certificaat is.
3. Kies de **publiceren...**  opdracht in het snelmenu van het project te openen de **Publish Azure Application** in het dialoogvenster en selecteer vervolgens het doelcluster. Het hulpprogramma automatisch de verbinding wordt omgezet en worden de parameters van de beveiligde verbinding opgeslagen in het publicatieprofiel.
4. Optioneel: U kunt bewerken het publicatieprofiel om op te geven van de verbinding van een beveiligd cluster.
   
   Nadat u het publiceren profiel XML-bestand voor het opgeven van informatie over het certificaat en noteer de naam van het certificaatarchief handmatig bewerkt, locatie en de vingerafdruk van certificaat opslaan. U moet voor deze waarden voor het certificaatarchief van de naam en locatie opslaan. Zie [hoe: de vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) voor meer informatie.
   
   U kunt de *ClusterConnectionParameters* parameters om op te geven van de PowerShell-parameters gebruiken bij het verbinden met de Service Fabric-cluster. Geldige parameters zijn die worden geaccepteerd door de cmdlet Connect-ServiceFabricCluster. Zie [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster) voor een lijst met beschikbare parameters.
   
   Als u in een extern cluster publiceren wilt, moet u de juiste parameters voor dat specifieke cluster opgeven. Hier volgt een voorbeeld van verbinding maken met een niet-beveiligde cluster:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Hier volgt een voorbeeld voor het verbinden met een x509 beveiligd cluster op basis van certificaat:
   
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
5. Eventuele andere benodigde instellingen, zoals parameters voor het bijwerken en toepassing Parameter bestandslocatie, bewerken en vervolgens Publiceer de toepassing uit de **Service Fabric-toepassing publiceren** in het dialoogvenster in Visual Studio.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het openen van Service Fabric-clusters [uw cluster visualiseren met behulp van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
