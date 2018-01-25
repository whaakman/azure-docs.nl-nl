---
title: Azure-Service Fabric-invoegtoepassing voor Eclipse | Microsoft Docs
description: Aan de slag met de Service Fabric- invoegtoepassing voor Eclipse.
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: rapatchi
ms.openlocfilehash: 291bbd35d6e3c89eb9568130ad144831452142ad
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Service Fabric-invoegtoepassing voor de ontwikkeling van Eclipse Java-toepassingen
Eclipse is een van de meest gebruikte Integrated Development Environments (IDE's) voor Java-ontwikkelaars. In dit artikel wordt beschreven hoe u een Eclipse-ontwikkelomgeving instelt voor gebruik met Azure Service Fabric. Ontdek hoe u de Service Fabric-invoegtoepassing installeert en een Service Fabric-toepassing implementeert in een lokaal of extern Service Fabric-cluster in Eclipse Neon. 

> [!NOTE]
> De Eclipse-invoegtoepassing wordt momenteel niet ondersteund in Windows. 

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse-neon"></a>De Service Fabric-invoegtoepassing installeren of bijwerken in Eclipse Neon
U kunt een Service Fabric-invoegtoepassing in Eclipse installeren. De invoegtoepassing vereenvoudigt het proces voor het maken en implementeren van Java-services.

1.  Zorg ervoor dat u de nieuwste versie van Eclipse Neon en de nieuwste versie van Buildship (1.0.17 of hoger) hebt geïnstalleerd:
    -   U kunt de versies van geïnstalleerde onderdelen controleren door in Eclipse Neon **Help** > **Installation Details** te kiezen.
    -   Zie [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Eclipse-invoegtoepassingen voor Gradle) als u Buildship wilt bijwerken.
    -   Als u updates voor Eclipse Neon wilt zoeken en installeren, gaat u naar **Help** > **Check for Updates**.

2.  Als u de Service Fabric-invoegtoepassing wilt installeren, gaat u in Eclipse Neon naar **Help** > **Install New Software**.
  1.    Geef in het vak **Work with** **http://dl.microsoft.com/eclipse** op.
  2.    Klik op **Add**.

         ![De Service Fabric-invoegtoepassing voor Eclipse Neon][sf-eclipse-plugin-install]
  3.    Selecteer de Fabric Service-invoegtoepassing en klik op **Next**.
  4.    Voer de installatiestappen uit en accepteer de licentievoorwaarden voor Microsoft-software.

Als u de Service Fabric-invoegtoepassing al hebt geïnstalleerd, controleert u of u de meest recente versie gebruikt. Ga naar **Help** > **Installation Details** om te controleren of er updates beschikbaar zijn. Selecteer Service Fabric in de lijst met geïnstalleerde invoegtoepassingen en klik op **Update**. Beschikbare updates worden geïnstalleerd.

> [!NOTE]
> Als de installatie of update van de Service Fabric-invoegtoepassing traag verloopt, kan dit het gevolg zijn van een instelling in Eclipse. Eclipse verzamelt metagegevens over alle wijzigingen in updatesites die zijn geregistreerd bij uw exemplaar van Eclipse. Als u het proces voor het controleren op en installeren van updates van Service Fabric-invoegtoepassingen wilt versnellen, gaat u naar **Available Software Sites**. Schakel de selectievakjes uit voor alle sites, behalve voor de site die naar de locatie van de Fabric Service-invoegtoepassing wijst (http://dl.microsoft.com/eclipse/azure/servicefabric).

> [!NOTE]
>Als Eclipse op uw Mac niet werkt zoals u verwacht of als u het moet uitvoeren als supergebruiker), gaat u naar de map **ECLIPSE_INSTALLATION_PATH** en vervolgens naar de submap **Eclipse.app/Contents/MacOS**. Start Eclipse door `./eclipse` uit te voeren.


## <a name="create-a-service-fabric-application-in-eclipse"></a>Een Service Fabric-toepassing maken in Eclipse

1.  Ga in Eclipse Neon naar **File** > **New** > **Other**. Selecteer **Fabric Service Project** en klik op **Next**.

    ![Nieuw Service Fabric-project pagina 1][create-application/p1]

2.  Voer een naam in voor het project en klik op **Next**.

    ![Nieuw Service Fabric-project pagina 2][create-application/p2]

3.  Selecteer **Service Template** in de lijst met sjablonen. Selecteer het type servicesjabloon (Actor, Stateless, Container of Guest Binary) en klik op **Next**.

    ![Nieuw Service Fabric-project pagina 3][create-application/p3]

4.  Voer de servicenaam en servicegegevens in en klik op **Finish**.

    ![Nieuw Service Fabric-project pagina 4][create-application/p4]

5. Wanneer u uw eerste Service Fabric-project maakt, klikt u in het dialoogvenster **Open Associated Perspective** op **Yes**.

    ![Nieuw Service Fabric-project pagina 5][create-application/p5]

6.  Het nieuwe project ziet er als volgt uit:

    ![Nieuw Service Fabric-project pagina 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Een Service Fabric-toepassing maken en implementeren in Eclipse

1.  Klik met de rechtermuisknop op de nieuwe Service Fabric-toepassing en selecteer **Service Fabric**.

    ![Snelmenu van Service Fabric][publish/RightClick]

2. Selecteer de gewenste optie in het submenu:
    -   Klik op **Build Application** als u de toepassing wilt maken zonder op te schonen.
    -   Klik op **Rebuild Application** als u een schone build van de toepassing wilt maken.
    -   Klik op **Clean Application** als u de gebouwde artefacts uit de toepassing wilt verwijderen.

3.  Vanuit dit menu kunt u de toepassing ook implementeren, de implementatie ervan verwijderen en de toepassing publiceren:
    -   Klik op **Deploy Application** als u wilt implementeren naar het lokale cluster.
    -   Selecteer in het dialoogvenster **Publish Application** een publicatieprofiel:
        -  **Local.json**
        -  **Cloud.json**

     Deze JSON-bestanden (JavaScript Object Notation) bevatten informatie (zoals verbindingseindpunten en beveiligingsgegevens) die nodig zijn om verbinding te maken met het lokale cluster of het cloudcluster (Azure).

  ![Service Fabric-menu Publish][publish/Publish]

U kunt de Service Fabric-toepassing ook implementeren met behulp van Eclipse-uitvoerconfiguraties.

  1.    Ga naar **Run** > **Run Configurations**.
  2.    Selecteer onder **Gradle Project** de uitvoerconfiguratie **ServiceFabricDeployer**.
  3.    Klik in het rechterdeelvenster op het tabblad **Arguments** en selecteer bij **publishProfile** de optie **local** of **cloud**.  De standaardinstelling is **local**. Selecteer **cloud** voor een implementatie in een extern of cloudcluster.
  4.    Bewerk **Local.json** of **Cloud.json** als dat nodig is om ervoor te zorgen dat de juiste informatie in de publicatieprofielen wordt ingevuld. U kunt eindpuntdetails en beveiligingsreferenties toevoegen of bijwerken.
  5.    Zorg ervoor dat **Working Directory** wijst naar de toepassing die u wilt implementeren. Als u de toepassing wilt wijzigen, klikt u op de knop **Workspace** en selecteert u de gewenste toepassing.
  6.    Klik op **Apply** en vervolgens op **Run**.

De toepassing is binnen enkele ogenblikken gemaakt en geïmplementeerd. U kunt de implementatiestatus controleren in Service Fabric Explorer.  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a>Een Service Fabric-service toevoegen aan uw Service Fabric-toepassing

Voer de volgende stappen uit als u een Service Fabric-service aan een bestaande Service Fabric-toepassing wilt toevoegen:

1.  Klik met de rechtermuisknop op het project waaraan u een service wilt toevoegen en klik vervolgens op **Service Fabric**.

    ![Service aan Service Fabric toevoegen pagina 1][add-service/p1]

2.  Klik op **Add Service Fabric Service** en voer de stappen uit om een service aan het project toe te voegen.
3.  Selecteer de servicesjabloon die u aan het project wilt toevoegen en klik op **Next**.

    ![Service aan Service Fabric toevoegen pagina 2][add-service/p2]

4.  Voer de servicenaam (en andere gevraagde informatie) in en klik op de knop **Add Service**.  

    ![Service aan Service Fabric toevoegen pagina 3][add-service/p3]

5.  Nadat de service is toegevoegd, ziet de algehele projectstructuur er ongeveer als volgt uit:

    ![Service aan Service Fabric toevoegen pagina 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Manifestversies van uw Service Fabric Java-toepassing bewerken

Als u manifestversies wilt bewerken, klikt u met de rechtermuisknop op het project, gaat u naar **Service Fabric** en selecteert u **Edit Manifest Versions...** in de vervolgkeuzelijst. In de wizard kunt u de manifestversies bijwerken voor het toepassingsmanifest, servicemanifest en de versies voor de pakketten **Code**, **Config** en **Data**.

Als u de optie **Automatically update application and service versions** inschakelt en vervolgens een versie bijwerkt, worden de manifestversies automatisch bijgewerkt. Als u bijvoorbeeld eerst het selectievakje inschakelt, vervolgens de versie van **Code** bijwerkt van 0.0.0 naar 0.0.1 en op **Finish** klikt, worden de versies van het servicemanifest en toepassingsmanifest automatisch bijgewerkt naar 0.0.1.

## <a name="upgrade-your-service-fabric-java-application"></a>Uw Service Fabric Java-toepassing upgraden

Stel dat u voor een upgradescenario het project **App1** hebt gemaakt met behulp van de Service Fabric-invoegtoepassing in Eclipse. U hebt dit vervolgens met behulp van de invoegtoepassing geïmplementeerd om een toepassing met de naam **fabric:/App1Application** te maken. Het toepassingstype is **App1AppicationType** en de toepassingsversie is 1.0. Nu wilt u een toepassingsupgrade uitvoeren zonder de beschikbaarheid van de toepassing te onderbreken.

Breng eerst eventuele wijzigingen aan in de toepassing en bouw vervolgens de gewijzigde service opnieuw op. Werk het manifestbestand van de gewijzigde service (ServiceManifest.xml) bij met de bijgewerkte versies voor de service (en code, configuratie of gegevens, indien van toepassing). Wijzig ook het toepassingsmanifest (ApplicationManifest.xml) met het bijgewerkte versienummer voor de toepassing en de gewijzigde service.  

Als u de toepassingsupgrade wilt uitvoeren met behulp van Eclipse Neon, kunt u een dubbel uitvoerconfiguratieprofiel maken. Vervolgens gebruikt u dit om de toepassingsupgrade uit te voeren.

1.  Ga naar **Run** > **Run Configurations**. Klik in het linkerdeelvenster op de kleine pijl, links van **Gradle Project**.
2.  Klik met de rechtermuisknop op **ServiceFabricDeployer** en selecteer **Duplicate**. Voer een nieuwe naam in voor deze configuratie, bijvoorbeeld **ServiceFabricUpgrader**.
3.  Ga in het rechterdeelvenster naar het tabblad **Arguments** en wijzig **-Pconfig='deploy'** in **-Pconfig=upgrade**. Klik vervolgens op **Apply**.

Met dit proces wordt een uitvoerconfiguratieprofiel gemaakt en opgeslagen dat u op elk gewenst moment kunt gebruiken om een upgrade van uw toepassing uit te voeren. Hiermee wordt ook de laatst bijgewerkte versie van het toepassingstype opgehaald uit het manifest-bestand van de toepassing.

De toepassingsupgrade duurt enkele minuten. U kunt de upgrade van de toepassing bewaken in Service Fabric Explorer.

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Oude Service Fabric Java-toepassingen migreren die moeten worden gebruikt met Maven
We hebben onlangs de bibliotheken van Java Service Fabric verplaatst van de Service Fabric Java SDK naar de Maven-opslagplaats. De nieuwe toepassingen die u genereert met behulp van Eclipse produceren projecten die u zonder problemen kunt gebruiken met Maven. Uw bestaande stateless Service Fabric- of Java-actortoepassingen zult u echter moeten bijwerken om ze te laten werken met de Service Fabric Java-afhankelijkheden van Maven. Deze oudere toepassingen maken namelijk nog gebruik van de Service Fabric Java SDK. Volg [deze stappen](service-fabric-migrate-old-javaapp-to-use-maven.md) om te controleren of een oudere toepassing werkt met Maven.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

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
