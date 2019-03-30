---
title: Zelfstudie voor service Fabric-app bijwerken | Microsoft Docs
description: Dit artikel helpt bij de ervaring van de implementatie van een Service Fabric-toepassing, de code te wijzigen en een upgrade uitrollen met behulp van Visual Studio.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 8fe0bf9c8827b7248195f89377176fd834845e32
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663669"
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Service Fabric application upgrade zelfstudie met Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric vereenvoudigt het proces van het upgraden van cloudtoepassingen door ervoor te zorgen dat alleen gewijzigde services worden bijgewerkt en dat de status van de toepassing wordt bewaakt tijdens het upgradeproces. Deze ook automatisch teruggedraaid in de toepassing naar de vorige versie wordt uitgevoerd voor problemen. Upgrades van de service Fabric-toepassingen zijn *nul Downtime*, omdat de toepassing kan worden bijgewerkt zonder uitvaltijd. Deze zelfstudie wordt uitgelegd hoe u een rolling upgrade vanuit Visual Studio.

## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>Stap 1: Bouwt en publiceert u het voorbeeld Visual objecten
Download eerst de [Visual objecten](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) toepassing vanuit GitHub. Klik, bouwt en publiceert u de toepassing met de rechtermuisknop op het toepassingsproject **VisualObjects**, en selecteer de **publiceren** opdracht in het menu-item van het Service Fabric.

![Contextmenu voor een Service Fabric-toepassing][image1]

Selecteren **publiceren** wordt een pop-upvenster en u kunt de **doelprofiel** naar **PublishProfiles\Local.xml**. Het venster moet eruitzien als het volgende voordat u klikt op **publiceren**.

![Een Service Fabric-toepassing publiceren][image2]

Nu kunt u klikken op **publiceren** in het dialoogvenster. U kunt [Service Fabric Explorer om het cluster en de toepassing weer te geven](service-fabric-visualizing-your-cluster.md). De toepassing Visual objecten heeft een webservice die u kunt naar door te typen [ http://localhost:8081/visualobjects/ ](http://localhost:8081/visualobjects/) in de adresbalk van uw browser.  Hier ziet u 10 zwevende visual objecten verplaatsen op het scherm.

**OPMERKING:** Als naar implementeert `Cloud.xml` profiel (Azure Service Fabric), moet beschikbaar zijn op de toepassing vervolgens **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Zorg ervoor dat u hebt `8081/TCP` geconfigureerd in de Load Balancer (zoeken naar de Load Balancer in dezelfde resourcegroep bevinden als de Service Fabric-instantie).

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: Het voorbeeld Visual objecten bijwerken
U zult merken dat met de versie die is geïmplementeerd in stap 1, de visual objecten niet draaien. Laten we deze toepassing op een waar de visual objecten ook draaien upgraden.

Selecteer het project VisualObjects.ActorService binnen de oplossing VisualObjects en open de **VisualObjectActor.cs** bestand. In dat bestand, gaat u naar de methode `MoveObject`, commentaar `visualObject.Move(false)`, en verwijder het commentaarteken `visualObject.Move(true)`. Deze gewijzigde code worden de objecten na de upgrade van de service.  **Nu u de oplossing kunt bouwen voor (niet opnieuw opbouwen)**, die de gewijzigde projecten bouwt. Als u selecteert *alles opnieuw opbouwen*, hebt u de versies voor alle projecten bijwerken.

Er moet ook versie onze toepassing. De versiewijzigingen aanbrengen nadat u met de rechtermuisknop op de **VisualObjects** project, kunt u de Visual Studio **Edit Manifest Versions** optie. Deze optie selecteert, wordt het dialoogvenster voor edition-versies als volgt:

![In het dialoogvenster voor versiebeheer][image3]

Bijwerken van de versies voor de gewijzigde projecten en hun codepakketten, samen met de toepassing bij naar versie 2.0.0. Nadat de wijzigingen zijn aangebracht, het manifest moet eruitzien als de volgende (vet gedeelten weergeven van de wijzigingen):

![Versie bijwerken][image4]

De hulpprogramma's voor Visual Studio kunnen doen automatische rollups van versies bij het selecteren van **toepassing en Serviceversies automatisch bijwerken**. Als u [SemVer](http://www.semver.org), moet u de code bijwerken en/of configuratie pakket versie alleen als deze optie is geselecteerd.

Sla de wijzigingen op en nu controleren de **Upgrade van de toepassing** vak.

## <a name="step-3--upgrade-your-application"></a>Stap 3:  Uw toepassing upgraden
Maak uzelf vertrouwd met de [parameters toepassingsupgrade](service-fabric-application-upgrade-parameters.md) en de [upgradeproces](service-fabric-application-upgrade.md) om op te halen van een goed begrip van de verschillende parameters voor het bijwerken, time-outs en de gezondheid van criteria die kan worden toegepast. Voor dit scenario wordt het criterium voor evaluatie van health service ingesteld op de standaardwaarde (niet-bewaakte modus). U kunt deze instellingen configureren door het selecteren van **Upgrade-instellingen configureren** en vervolgens de parameters naar wens wijzigen.

Nu we zijn klaar om te beginnen de upgrade van de toepassing selecteren **publiceren**. Deze optie voert u een upgrade van uw toepassing bij naar versie 2.0.0, waarin de objecten draaien. Service Fabric upgrades één updatedomein tegelijk (sommige objecten zijn bijgewerkt eerst, gevolgd door anderen) en de service blijft toegankelijk tijdens de upgrade. Toegang tot de service kan worden gecontroleerd via de client (browser).  

Nu als de upgrade opbrengst van toepassing, kunt u het bewaken met Service Fabric Explorer, met behulp van de **Upgrades in uitvoering** tabblad onder de toepassingen.

Alle update-domeinen moeten worden bijgewerkt (voltooid) in een paar minuten en de Visual Studio-uitvoervenster moet ook zijn dat de upgrade is voltooid. En u moet zien dat *alle* de visual objecten in het browservenster worden nu draaien!

U kunt de versies, wijzig en verplaatsen van versie 2.0.0 naar versie 3.0.0 bij wijze van oefening of zelfs van versie 2.0.0 terug naar versie 1.0.0. Speel met time-outs en statusbeleid om uzelf bekend met hen. Bij het implementeren van een Azure-cluster in plaats van een lokaal cluster, moeten de parameters die worden gebruikt, mogelijk verschillen. U wordt aangeraden de time-outs voorzichtig te stellen.

## <a name="next-steps"></a>Volgende stappen
[Bijwerken van uw toepassing met behulp van PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing is bijgewerkt met behulp van [Upgradeparameters](service-fabric-application-upgrade-parameters.md).

Upgrades van uw toepassingen compatibel maken door te leren hoe u [gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [problemen met toepassingsupgrades oplossen](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
