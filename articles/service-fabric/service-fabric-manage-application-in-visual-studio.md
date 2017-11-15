---
title: Beheren van uw toepassingen in Visual Studio | Microsoft Docs
description: Visual Studio gebruiken om te maken, ontwikkelen, verpakken, implementeren en foutopsporing van uw Service Fabric-toepassingen en services.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: 81814ef07cfab06855b3bfbf0eb6ef51b8dfce4f
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio gebruiken om te schrijven en beheren van uw Service Fabric-toepassingen vereenvoudigen
U kunt uw Azure Service Fabric-toepassingen en services met Visual Studio kunt beheren. Zodra u hebt [uw ontwikkelomgeving instellen](service-fabric-get-started.md), kunt u Visual Studio Service Fabric-toepassingen maken, het toevoegen van de services of pakket, registreren en het implementeren van toepassingen in uw lokaal ontwikkelcluster.

## <a name="deploy-your-service-fabric-application"></a>Service Fabric-toepassing implementeren
Standaard is de volgende stappen uit in één bewerking combineert een toepassing implementeren:

1. Het toepassingspakket maken
2. Het toepassingspakket uploaden naar de image store
3. Het toepassingstype registreren
4. Als u een actieve toepassingsexemplaren
5. Maken van een toepassingsexemplaar

In Visual Studio, drukt u op **F5** implementeert van uw toepassing en het foutopsporingsprogramma koppelen aan alle exemplaren van een toepassing. U kunt **Ctrl + F5** voor het implementeren van een toepassing zonder foutopsporing, of u kunnen publiceren naar een lokale of externe cluster met behulp van het publicatieprofiel. 

### <a name="application-debug-mode"></a>De foutopsporingsmodus toepassing
Visual Studio bieden een eigenschap genaamd **toepassing foutopsporingsmodus**, die bepaalt hoe u wilt dat Visual Studios voor het afhandelen van de implementatie van toepassing als onderdeel van het opsporen van fouten.

#### <a name="to-set-the-application-debug-mode-property"></a>De eigenschap foutopsporingsmodus toepassing instellen
1. Op het Service Fabric-toepassingsproject (*.sfproj) snelmenu kiezen **eigenschappen** (of drukt u op de **F4** sleutel).
2. In de **eigenschappen** Stel venster de **toepassing foutopsporingsmodus** eigenschap.

![Toepassing foutopsporing modus eigenschap instellen][debugmodeproperty]

#### <a name="application-debug-modes"></a>Toepassing Foutopsporingsmodi

1. **Vernieuwen van de toepassing** deze modus kunt u snel wijzigen en fouten opsporen in uw code en ondersteunt het bewerken van statische webbestanden tijdens het opsporen van fouten. Deze modus werkt alleen als uw lokaal ontwikkelcluster in [modus 1-Node](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).
2. **Toepassing verwijderen** zorgt ervoor dat de toepassing moet worden verwijderd wanneer de debug-sessie wordt beëindigd.
3. **Automatische Upgrade** de toepassing nog wordt uitgevoerd wanneer de debug-sessie wordt beëindigd. De volgende foutopsporingssessie zal de implementatie worden beschouwd als een upgrade. Het upgradeproces behoudt alle gegevens die u hebt ingevoerd in een vorige foutopsporingssessie.
4. **Toepassing houden** de toepassing blijft in het cluster uitgevoerd wanneer de debug-sessie wordt beëindigd. Aan het begin van de volgende foutopsporingssessie, wordt de toepassing verwijderd.

Voor **automatische Upgrade** gegevens blijven behouden door het toepassen van de toepassing upgrade mogelijkheden van Service Fabric. Zie voor meer informatie over het upgraden van toepassingen en hoe u een upgrade kunt uitvoeren in een omgeving met echte [upgrade van de Service Fabric-toepassing](service-fabric-application-upgrade.md).

## <a name="add-a-service-to-your-service-fabric-application"></a>Een service aan uw Service Fabric-toepassing toevoegen
U kunt nieuwe services toevoegen aan uw toepassing uit te breiden de functionaliteit hiervan.  Om ervoor te zorgen dat de service is opgenomen in het toepassingspakket, de service toevoegen via de **nieuwe Fabric-Service...**  menu-item.

![Een nieuwe Service Fabric-service toevoegen][newservice]

Selecteer een type Service Fabric-project toevoegen aan uw toepassing en geef een naam voor de service.  Zie [kiezen een framework voor uw service](service-fabric-choose-framework.md) om te bepalen welk servicetype te gebruiken.

![Selecteer een type Service Fabric-service-project toevoegen aan uw toepassing][addserviceproject]

De nieuwe service die wordt toegevoegd aan de oplossing en de bestaande toepassingspakket. De Serviceverwijzingen en een standaard service-exemplaar wordt toegevoegd aan het toepassingsmanifest, waardoor de service niet kan worden gemaakt en de volgende keer dat u de toepassing implementeren is gestart.

![De nieuwe service die wordt toegevoegd aan uw toepassingsmanifest][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Uw toepassing Service Fabric-pakket
Voor het implementeren van de toepassing en de bijbehorende services naar een cluster, moet u een toepassingspakket maken.  Het pakket ordent het toepassingsmanifest, service manifesten en andere benodigde bestanden in een specifieke indeling.  Visual Studio instellen en beheren van het pakket in het toepassingsproject-map in de map 'pkg'.  Te klikken op **pakket** van de **toepassing** contextmenu maken of bijwerken van het toepassingspakket.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Verwijderen van toepassingen en toepassingstypen met Cloud Explorer
U kunt basiscluster beheerbewerkingen uitvoeren vanuit Visual Studio gebruikt Cloud Explorer, die u vanuit starten kunt de **weergave** menu. U kunt bijvoorbeeld verwijderen van toepassingen en inrichting van toepassingstypen op lokale of externe clusters.

![Een toepassing verwijderen][removeapplication]

> [!TIP]
> Zie voor uitgebreidere cluster beheerfunctionaliteit, [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Implementatie van service Fabric-toepassing](service-fabric-deploy-remove-applications.md)
* [Parameters voor de toepassing voor omgevingen met meerdere beheren](service-fabric-manage-multiple-environment-app-configuration.md)
* [Foutopsporing van uw Service Fabric-toepassing](service-fabric-debugging-your-application.md)
* [Uw cluster visualiseren met behulp van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png