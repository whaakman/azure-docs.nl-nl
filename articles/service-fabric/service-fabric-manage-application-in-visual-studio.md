---
title: Beheren van uw Azure Servic Fabric-toepassingen in Visual Studio | Microsoft Docs
description: Visual Studio gebruiken om te maken, ontwikkelen, verpakken, implementeren en fouten opsporen in uw Azure Service Fabric-toepassingen en services.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 03/26/2018
ms.author: mikhegn
ms.openlocfilehash: 01a9852157cbaefd966be974a3175c25e7ff69f8
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001699"
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio gebruiken om te schrijven en beheren van uw Service Fabric-toepassingen vereenvoudigen
U kunt uw Azure Service Fabric-toepassingen en services via Visual Studio kunt beheren. Nadat u hebt [uw ontwikkelomgeving instellen](service-fabric-get-started.md), kunt u Visual Studio te maken van Service Fabric-toepassingen, services, of het pakket, register toevoegen en implementeren van toepassingen in uw lokale ontwikkelcluster.

## <a name="deploy-your-service-fabric-application"></a>Uw Service Fabric-toepassing implementeren
Standaard is de volgende stappen uit in één bewerking combineert implementeren van een toepassing:

1. Het maken van het toepassingspakket
2. Het toepassingspakket uploaden naar de installatiekopieopslag
3. Het toepassingstype te registreren
4. Verwijderen van alle exemplaren van de toepassing uitgevoerd
5. Het exemplaar van een toepassing maken

In Visual Studio drukken **F5** implementeert uw toepassing en het foutopsporingsprogramma koppelen aan alle exemplaren van een toepassing. U kunt **Ctrl + F5** om te een toepassing implementeren zonder foutopsporing, of u kunnen publiceren naar een lokaal of extern cluster met behulp van het publicatieprofiel.

### <a name="application-debug-mode"></a>Foutopsporingsmodus van toepassing
Visual Studio bevatten een eigenschap genaamd **toepassing foutopsporingsmodus**, die bepaalt hoe wilt u Visual Studio's om af te handelen toepassingsimplementatie als onderdeel van het opsporen van fouten.

#### <a name="to-set-the-application-debug-mode-property"></a>De eigenschap foutopsporingsmodus toepassing instellen
1. Op de Service Fabric-toepassing van het project (*.sfproj) snelmenu kiezen **eigenschappen** (of drukt u op de **F4** sleutel).
2. In de **eigenschappen** venster de **toepassing foutopsporingsmodus** eigenschap.

![Instellen van toepassingseigenschap foutopsporing modus][debugmodeproperty]

#### <a name="application-debug-modes"></a>Toepassing Foutopsporingsmodi

1. **Aktualizovat Aplikaci** in deze modus kunt u snel wijzigen en fouten opsporen in uw code en ondersteunt het bewerken van statische webbestanden tijdens het opsporen van fouten. In deze modus werkt alleen als uw lokale ontwikkelcluster in [modus 1-Node](./service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode). Dit is de standaardinstelling toepassing foutopsporingsmodus.
2. **Toepassing verwijderen** zorgt ervoor dat de toepassing moet worden verwijderd wanneer de debug-sessie wordt beëindigd.
3. **Automatische Upgrade** blijft de toepassing wordt uitgevoerd wanneer de debug-sessie wordt beëindigd. De volgende foutopsporingssessie zal de implementatie worden beschouwd als een upgrade. Het upgradeproces bewaart alle gegevens die u hebt ingevoerd in een vorige foutopsporingssessie.
4. **Zachovat Aplikaci** de toepassing in het cluster blijft actief wanneer de debug-sessie wordt beëindigd. Aan het begin van de volgende foutopsporingssessie, wordt de toepassing verwijderd.

Voor **automatische Upgrade** gegevens blijven behouden door toe te passen van de toepassing upgrade mogelijkheden van Service Fabric. Zie voor meer informatie over het upgraden van toepassingen en hoe u een upgrade kunt uitvoeren in een omgeving met echte [Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md).

## <a name="add-a-service-to-your-service-fabric-application"></a>Een service toevoegen aan uw Service Fabric-toepassing
U kunt nieuwe services toevoegen aan uw toepassing uit te breiden de functionaliteit ervan. Om ervoor te zorgen dat de service wordt opgenomen in uw toepassingspakket, voeg de service via de **nieuwe Fabric-Service...**  menu-item.

![Een nieuwe Service Fabric-service toevoegen][newservice]

Selecteer Service Fabric-project dat u wilt toevoegen aan uw toepassing en geef een naam voor de service.  Zie [kiezen van een framework voor uw service](service-fabric-choose-framework.md) om te bepalen welk servicetype te gebruiken.

![Selecteer een Service Fabric service project dat u wilt toevoegen aan uw toepassing][addserviceproject]

De nieuwe service is toegevoegd aan de oplossing en de bestaande toepassingspakket. De Serviceverwijzingen en een standaard service-exemplaar wordt toegevoegd aan het toepassingsmanifest, waardoor de service worden gemaakt en gestart van de volgende keer dat u de toepassing implementeren.

![De nieuwe service wordt toegevoegd aan uw toepassingsmanifest][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Uw Service Fabric-toepassing
Als u wilt de toepassing en services implementeren in een cluster, moet u een toepassingspakket maken.  Het pakket worden het toepassingsmanifest, servicemanifesten en andere benodigde bestanden in een specifieke indeling.  Visual Studio instellen en beheren van het pakket in het toepassingsproject map, in de map 'pkg'.  Te klikken op **pakket** uit de **toepassing** contextmenu Hiermee maken of bijwerken van het toepassingspakket.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Verwijderen van toepassingen en de soorten toepassingen met behulp van Cloud Explorer
U kunt basiscluster beheerbewerkingen uitvoeren vanuit Visual Studio Cloud Explorer gebruikt, die u vanuit starten kunt de **weergave** menu. U kunt bijvoorbeeld verwijderen van toepassingen en inrichting van de toepassingstypen op lokale of externe-clusters.

![Een toepassing verwijderen][removeapplication]

> [!TIP]
> Zie voor een uitgebreidere beheerfunctionaliteit van de cluster [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Implementatie van service Fabric-toepassing](service-fabric-deploy-remove-applications.md)
* [Parameters voor de toepassing voor meerdere omgevingen beheren](service-fabric-manage-multiple-environment-app-configuration.md)
* [Foutopsporing van uw Service Fabric-toepassing](service-fabric-debugging-your-application.md)
* [Uw cluster visualiseren met behulp van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png
