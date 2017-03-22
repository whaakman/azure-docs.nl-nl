---
title: Aan de slag met de Eclipse-invoegtoepassing voor Azure Service Fabric | Microsoft Docs
description: Aan de slag met de Eclipse-invoegtoepassing voor Azure Service Fabric.
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 891df38aa685cac7bbfecb71b15c88d557ac493b
ms.lasthandoff: 03/11/2017


---

# <a name="getting-started-with-eclipse-plugin-for-service-fabric-java-application-development"></a>Aan de slag met de Eclipse-invoegtoepassing voor de ontwikkeling van Java-toepassingen met Service Fabric
Eclipse is een van de meest gebruikte IDE's voor Java-ontwikkelaars. In dit artikel wordt besproken hoe u uw Eclipse-ontwikkelomgeving kunt instellen voor gebruik met Service Fabric. Dit artikel helpt u om de invoegtoepassing te installeren, Service Fabric-toepassingen te maken en uw Service Fabric-toepassing naar een lokaal of extern Service Fabric-cluster te implementeren.

## <a name="install-or-update-service-fabric-plugin-on-eclipse-neon"></a>De Service Fabric-invoegtoepassing installeren of bijwerken in Eclipse Neon
Service Fabric biedt een invoegtoepassing voor de **Eclipse IDE voor Java-ontwikkelaars** die het proces van het maken en implementeren van Java-services kan vereenvoudigen.

1. Zorg ervoor dat u de meest recente Eclipse **Neon** en meest recente Buildship-versie (1.0.17 of hoger) hebt geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door ``Help => Installation Details`` te kiezen. U kunt Buildship bijwerken met behulp van [deze instructies][buildship-update]. Als u wilt controleren of u de meest recente versie van Eclipse Neon gebruikt of als u deze wilt bijwerken, gaat u naar ``Help => Check for Updates``.

2. Als u de Service Fabric-invoegtoepassing wilt installeren, kiest u ``Help => Install New Software...``.
  1. Voer in het tekstvak 'Werken met' ``http://dl.windowsazure.com/eclipse/servicefabric`` in.
  2. Klik op Add.

  ![Eclipse Neon-invoegtoepassing voor Service Fabric][sf-eclipse-plugin-install]

  3. Kies de Service Fabric-invoegtoepassing en klik op Next.
  4. Doorloop de installatie en accepteer de gebruiksrechtovereenkomst.

Als u de Service Fabric Eclipse-invoegtoepassing al hebt geïnstalleerd, zorgt u ervoor dat u de meest recente versie gebruikt. U kunt controleren of deze kan worden bijgewerkt door het volgende uit te voeren: ``Help => Installation Details``. Zoek naar Service Fabric in de lijst met geïnstalleerde invoegtoepassingen en klik op Bijwerken. Als er een update beschikbaar is, wordt deze opgehaald en geïnstalleerd.

> [!NOTE]
> Wanneer het installeren of bijwerken van de Service Fabric Eclipse-invoegtoepassing op uw Eclipse veel tijd in beslag neemt, komt dit omdat Eclipse elke keer metagegevens probeert op te halen van alle nieuwe wijzigingen die zijn toegepast op alle bijgewerkte sites die met uw Eclipse-exemplaar zijn geregistreerd. U kunt het volgende trucje gebruiken om de benodigde tijd te verminderen. Ga naar `Available Software Sites` en schakel alle selectievakjes uit behalve het selectievakje dat naar de locatie van de Service Fabric-invoegtoepassing `http://dl.windowsazure.com/eclipse/servicefabric` verwijst.
>

## <a name="create-service-fabric-application-using-eclipse"></a>Een Service Fabric-toepassing maken met behulp van Eclipse

1. Ga naar ``File => New => Other``. Selecteer ``Service Fabric Project``. Klik op ``Next``.

    ![Nieuw Service Fabric-project pagina 1][create-application/p1]

2. Geef uw project een naam. Klik op ``Next``.

    ![Nieuw Service Fabric-project pagina 2][create-application/p2]

3. Selecteer Servicesjabloon in de set beschikbare sjablonen (Actor, Staatloos, Container of Door gast uitvoerbaar). Klik op ``Next``.

    ![Nieuw Service Fabric-project pagina 3][create-application/p3]

4. Voer op deze pagina de naam van de service en/of de relevante servicedetails in en klik op ``Finish``.

    ![Nieuw Service Fabric-project pagina 4][create-application/p4]

5. Wanneer u uw eerste Service Fabric-project maakt, wordt u gevraagd of het Service Fabric-perspectief moet worden ingesteld. Selecteer ``yes`` om door te gaan.

    ![Nieuw Service Fabric-project pagina 5][create-application/p5]

6. Nadat dit is gemaakt, ziet het project er als volgt uit:

    ![Nieuw Service Fabric-project pagina 6][create-application/p6]

## <a name="build-and-deploy-the-service-fabric-application-using-eclipse"></a>De Service Fabric-toepassing bouwen en implementeren met behulp van Eclipse

* Klik met de rechtermuisknop op de Service Fabric-toepassing die u hierboven hebt gemaakt. Selecteer de optie ``Service Fabric`` in het contextmenu. Er wordt een submenu met meerdere opties weergegeven, dat er als volgt uitziet:

    ![Snelmenu van Service Fabric][publish/RightClick]

  Als u op de opties klikt voor bouwen, herbouwen en opschonen, worden de gewenste acties uitgevoerd.
  - Met ``Build Application`` wordt de toepassing gebouwd zonder op te schonen
  - Met ``Rebuild Application`` wordt de toepassing opgeschoond en gebouwd
  - Met ``Clean Application`` wordt de toepassing opgeschoond van de gebouwde artefacten


* U kunt in dit menu ook kiezen om uw toepassing te implementeren, te publiceren en de implementatie ongedaan te maken.
  - Met ``Deploy Application`` wordt de toepassing naar uw lokale cluster geïmplementeerd
  - Met ``Publish Application...`` wordt u gevraagd voor welk publicatieprofiel u wilt selecteren tussen ``Local.json`` en ``Cloud.json``. Deze JSON-bestanden worden gebruikt om informatie (zoals verbindingseindpunten en beveiligingsgegevens) in op te slaan die nodig is om verbinding te maken met het lokale of cloudcluster (Azure).

  ![Snelmenu van Service Fabric][publish/Publish]

* Er is nog een manier waarop u uw Service Fabric-toepassing kunt implementeren met behulp van Eclipse-uitvoerconfiguraties.

  1. Kies ``Run => Run Configurations``. Selecteer de uitvoerconfiguratie ``ServiceFabricDeployer`` onder ``Grade Project``.
  2. Geef onder het tabblad ``Arguments`` in het rechterdeelvenster **lokaal** of **cloud** op als het ``publishProfile``. De standaardinstelling is **lokaal**. Selecteer **cloud** voor een implementatie op een extern/cloudcluster.
  3. Zorg ervoor dat de juiste informatie in de publicatieprofielen is ingevuld door de `Local.json` of `Cloud.json` te bewerken met eventuele eindpuntgegevens en beveiligingsreferenties.
  4. Zorg ervoor dat ``Working Directory`` in het rechterdeelvenster onder ``Grade Project`` verwijst naar de toepassing die u wilt implementeren. Als dit niet het geval is, klikt u op de knop ``Workspace...`` en selecteert u de gewenste toepassing.
  5. Klik op **Toepassen** en **Uitvoeren**.

De toepassing is binnen enkele ogenblikken gemaakt en geïmplementeerd. U kunt de status bewaken vanuit Service Fabric Explorer.  

## <a name="add-new-service-fabric-service-to-your-service-fabric-application"></a>Nieuwe Service Fabric-service toevoegen aan uw Service Fabric-toepassing

U kunt een nieuwe Service Fabric-service aan een bestaande Service Fabric-toepassing toevoegen door de volgende stappen uit te voeren:

1. Klik met de rechtermuisknop op het project waaraan u een service wilt toevoegen. Open het contextmenu en selecteer de optie Service Fabric. Er wordt een submenu met meerdere opties weergegeven.

    ![Service aan Service Fabric toevoegen pagina 1][add-service/p1]

2. Selecteer de optie `Add ServiceFabric Service`. Deze leidt u door de volgende reeks stappen om een service aan het project toe te voegen.
3. Selecteer de servicesjabloon die u aan het project wilt toevoegen en klik op Volgende.

    ![Service aan Service Fabric toevoegen pagina 2][add-service/p2]

4. Voer de servicenaam (en eventuele andere benodigde informatie) in en klik op de onderstaande knop Service toevoegen.  

    ![Service aan Service Fabric toevoegen pagina 3][add-service/p3]

5. Nadat de service is toegevoegd, ziet de volledige projectstructuur er ongeveer uit zoals hieronder wordt weergegeven:

    ![Service aan Service Fabric toevoegen pagina 4][add-service/p4]

## <a name="upgrade-your-service-fabric-java-application"></a>Uw Service Fabric Java-toepassing upgraden

We gaan ervan uit dat u het ``App1``-project hebt gemaakt met behulp van de Service Fabric Eclipse-invoegtoepassing en hebt geïmplementeerd met behulp van de invoegtoepassing voor het maken van een toepassing met de naam ``fabric:/App1Application`` van toepassingstype ``App1AppicationType`` en toepassingsversie 1.0. U wilt nu uw toepassing upgraden zonder deze af te breken.

Breng de wijziging aan in uw toepassing en bouw de gewijzigde service opnieuw op.  Werk het gewijzigde servicemanifest-bestand (``ServiceManifest.xml``) bij met de bijgewerkte versies voor de service (en code, configuratie of gegevens, indien van toepassing). Wijzig ook het toepassingsmanifest (``ApplicationManifest.xml``) met het bijgewerkte versienummer voor de toepassing en de gewijzigde service.  

Als u uw toepassing met Eclipse wilt bijwerken, kunt u een dubbele uitvoerconfiguratie maken en deze gebruiken om uw toepassing te upgraden wanneer dit nodig is. Dit doet u door de volgende stappen uit te voeren:
1. Kies ``Run => Run Configurations``. Klik op de kleine pijl aan de linkerkant van ``Grade Project`` in het linkerdeelvenster.
2. Klik met de rechtermuisknop op ``ServiceFabricDeployer`` en selecteer ``Duplicate``. Geef deze configuratie een nieuwe naam, bijvoorbeeld ``ServiceFabricUpgrader``.
3. In het rechterdeelvenster onder het tabblad ``Arguments`` wijzigt u ``-Pconfig='deploy'`` in ``-Pconfig=upgrade`` en klikt u op ``Apply``.
4. U hebt nu een uitvoerconfiguratie gemaakt en opgeslagen waarmee u uw toepassing kunt upgraden. Deze kunt u ``Run`` wanneer u wilt. Dit zorgt voor het ophalen van de meest recente bijgewerkte versie van het toepassingstype van het manifest-bestand van de toepassing.

U kunt nu de upgrade van de toepassing bewaken met behulp van Service Fabric Explorer. De upgrade van de toepassing is binnen enkele minuten voltooid.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

