---
title: Service Fabric-app upgrade-zelfstudie | Microsoft Docs
description: Dit artikel helpt bij de ervaring van een Service Fabric-toepassing implementeren, de code te wijzigen en implementeren van een upgrade met behulp van Visual Studio.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 940440688ec770a4aeb932b574bd6be173f494d4
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Service Fabric-toepassing upgraden zelfstudie met Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric vereenvoudigt het proces van updaten van cloud-toepassingen door ervoor te zorgen dat alleen gewijzigde services worden bijgewerkt en dat de toepassingsstatus wordt bewaakt gedurende het upgradeproces. Deze ook automatisch teruggedraaid in de toepassing naar de vorige versie wordt uitgevoerd voor problemen. Service Fabric-toepassingsupgrades worden *Zero Downtime*, omdat de toepassing kan worden bijgewerkt zonder uitvaltijd. Deze zelfstudie bevat informatie over het uitvoeren van een uitrollende upgrade vanuit Visual Studio.

## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>Stap 1: Maken en publiceren van de steekproef visuele objecten
Download eerst het [visuele objecten](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) toepassing vanuit GitHub. Vervolgens maken en publiceren van de toepassing door met de rechtermuisknop op het toepassingsproject **VisualObjects**, en het selecteren van de **publiceren** opdracht in het menu-item van het Service Fabric.

![Contextmenu voor een Service Fabric-toepassing][image1]

Selecteren **publiceren** opstart, wordt een pop-up en u kunt de **profiel als doel** naar **PublishProfiles\Local.xml**. Het venster moet eruitzien als in het volgende voordat u op **publiceren**.

![Een Service Fabric-toepassing publiceren][image2]

Nu kunt u **publiceren** in het dialoogvenster. U kunt [Service Fabric Explorer om het cluster en de toepassing weer te geven](service-fabric-visualizing-your-cluster.md). De visuele objecten-toepassing heeft een webservice die u naar door te typen gaat [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) in de adresbalk van uw browser.  Hier ziet u 10 zwevende visual objecten verplaatsen op het scherm.

**Opmerking:** als implementeert voor `Cloud.xml` profiel (Azure Service Fabric) moet beschikbaar zijn op de toepassing vervolgens **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Zorg ervoor dat u hebt `8081/TCP` geconfigureerd in de Load Balancer (zoeken de Load Balancer in dezelfde resourcegroep bevinden als de Service Fabric-instantie).

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: Het voorbeeld visuele objecten bijwerken
U wellicht opgevallen dat door de versie die is geïmplementeerd in stap 1, visuele objecten niet draaien. Deze toepassing op een waar de visuele objecten ook draaien gaat upgraden.

Selecteer het project VisualObjects.ActorService binnen de oplossing VisualObjects en open de **VisualObjectActor.cs** bestand. In dat bestand, gaat u naar de methode `MoveObject`, uitcommentariëren `visualObject.Move(false)`, en verwijder het commentaarteken `visualObject.Move(true)`. Deze codewijziging draait de objecten na de upgrade van de service.  **Nu kunt u (niet opnieuw opbouwen) de oplossing bouwen**, die de gewijzigde projecten bouwt. Als u selecteert *alles opnieuw opbouwen*, u moet de Updateversies voor alle projecten.

Er moet ook versie onze toepassing. Om de versiewijzigingen nadat u met de rechtermuisknop op de **VisualObjects** project, kunt u de Visual Studio **Manifest versies bewerken** optie. Deze optie wordt het dialoogvenster voor versie als volgt:

![Dialoogvenster versiebeheer][image3]

Bijwerken van de versies voor de gewijzigde projecten en hun code-pakketten, samen met de toepassing naar versie 2.0.0. Nadat de wijzigingen zijn aangebracht, het manifest moet eruitzien als in de volgende (vet gedeelten weergeven van de wijzigingen):

![Versies bijwerken][image4]

De Visual Studio-hulpprogramma's kunnen doen automatische samentellingen van versies bij het selecteren van **automatisch bijwerken van toepassing en Serviceversies**. Als u [SemVer](http://www.semver.org), moet u de code bijwerken en/of configuratie pakket versie alleen als deze optie is geselecteerd.

De wijzigingen opslaan en controleren nu de **Upgrade van de toepassing** vak.

## <a name="step-3--upgrade-your-application"></a>Stap 3: Uw toepassing bijwerken
Vertrouwd raken met de [parameters voor het bijwerken van toepassing](service-fabric-application-upgrade-parameters.md) en de [upgradeproces](service-fabric-application-upgrade.md) ophalen van een goed begrip van de verschillende parameters voor het bijwerken, time-outs en health criterium die kan worden toegepast. Voor dit scenario moet is het criterium service health evaluatie ingesteld op de standaardwaarde (niet-bewaakte modus). U kunt deze instellingen configureren door te selecteren **instellingen configureren voor Upgrade** en vervolgens de parameters naar wens te bewerken.

Nu we alle is ingesteld op het begin van de upgrade van de toepassing met het selecteren zijn **publiceren**. Deze optie voert een upgrade van uw toepassing naar versie 2.0.0, waarin de objecten draaien. Service Fabric upgrades één updatedomein tegelijk (sommige objecten zijn bijgewerkt eerst, gevolgd door anderen) en de service blijft toegankelijk tijdens de upgrade. Toegang tot de service kan worden gecontroleerd via de client (browser).  

Nu als de upgrade opbrengst toepassing u kunt deze bewaken met Service Fabric Explorer met de **Upgrades uitgevoerd** tabblad onder de toepassingen.

Alle domeinen van de update moeten worden bijgewerkt (voltooid) in een paar minuten en het Visual Studio-uitvoervenster moet ook zijn dat de upgrade is voltooid. En u ziet dat *alle* nu roteren van de visual objecten in uw browservenster!

U kunt proberen de versies, wijzigen en verplaatsen van versie 2.0.0 naar versie 3.0.0 wijze van oefening maakt, of zelfs van versie 2.0.0 terug naar versie 1.0.0. Afspelen met time-outs en statusbeleid voor uzelf bekend mee te maken. Wanneer naar een Azure-cluster in plaats van een lokaal cluster implementeert, wordt de gebruikte parameters wellicht afwijken. Het is raadzaam dat u de time-outs conservatieve ingesteld.

## <a name="next-steps"></a>Volgende stappen
[Bijwerken van uw toepassing met behulp van PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [parameters upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [probleemoplossing toepassingsupgrades](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
