---
title: Stel doorlopende integratie voor Azure microservices | Microsoft Docs
description: Biedt een overzicht van continue integratie en implementatie voor een Service Fabric-toepassing instellen met behulp van Visual Studio Team Services (VSTS).
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: ryanwi
ms.openlocfilehash: d73120018fa02a64c2a895a93b43661fccd4e5aa
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Service Fabric continue integratie en implementatie met Visual Studio Team Services instellen
In dit artikel beschrijft de stappen voor het instellen van continue integratie en implementatie voor een Azure Service Fabric-toepassing met behulp van Visual Studio Team Services (VSTS).

Dit document weerspiegelt de huidige procedure en naar verwachting gewijzigd gedurende een bepaalde periode.

## <a name="prerequisites"></a>Vereisten
Voer de volgende stappen uit om te beginnen:

1. Zorg ervoor dat u toegang tot een Team Services-account hebben of [maken van een](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) zelf.
2. Zorg ervoor dat u toegang tot een teamproject Team Services hebben of [maken van een](https://www.visualstudio.com/docs/setup-admin/create-team-project) zelf.
3. Zorg ervoor dat er een Service Fabric-cluster waarnaar u kunt uw toepassing implementeren of er een maken met de [Azure-portal](service-fabric-cluster-creation-via-portal.md), een [Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md), of [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).
4. Zorg ervoor dat u al een Service Fabric-toepassing (.sfproj)-project hebt gemaakt. U moet een project dat is gemaakt of bijgewerkt met Service Fabric SDK 2.1 of hoger (het bestand .sfproj bevatten een waarde van de eigenschap ProjectVersion van 1.1 of hoger) hebben.

> [!NOTE]
> Aangepaste build agents zijn niet langer vereist. Teamservices gehost agents nu afkomstig zijn vooraf worden geïnstalleerd met de software voor beheer van Service Fabric-cluster, zodat voor de implementatie van uw toepassingen rechtstreeks vanuit de agents.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Configureren en delen van de bronbestanden
Het eerste wat dat u wilt doen, is een publicatieprofiel voorbereiden voor gebruik door het implementatieproces die wordt uitgevoerd binnen Team Services.  Het publicatieprofiel dat moet worden geconfigureerd voor het doel van het cluster dat u eerder hebt voorbereid:

1. Kies een publicatieprofiel binnen uw toepassing-project dat u wilt gebruiken voor uw werkstroom continue integratie. Ga als volgt de [publiceren instructies](service-fabric-publish-app-remote-cluster.md) voor het uitgeven van een toepassing in een externe-cluster. U hoeft niet daadwerkelijk al uw toepassing te publiceren. U kunt klikken op de **opslaan** hyperlink in het dialoogvenster publish nadat u hebt dingen op de juiste wijze geconfigureerd.
2. Als u wilt dat uw toepassing moet worden bijgewerkt voor elke implementatie die in een Team Services, die u wilt het publicatieprofiel zodat upgrade configureren. Zorg ervoor dat in het hetzelfde publiceren dialoogvenster gebruikt in stap 1 de **Upgrade van de toepassing** selectievakje is ingeschakeld.  Meer informatie over [extra upgrade-instellingen configureren](service-fabric-visualstudio-configure-upgrade.md). Klik op de **opslaan** hyperlink naar de instellingen in het publicatieprofiel opslaan.
3. Zorg ervoor dat u uw wijzigingen hebt opgeslagen in het publicatieprofiel en het dialoogvenster publish annuleren.
4. Nu is het tijd om [delen van de bronbestanden van uw toepassing project](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) met Team Services. Wanneer de bronbestanden toegankelijk in Team Services zijn, kunt u nu op verplaatsen naar de volgende stap voor het genereren van builds. 

## <a name="create-a-build-definition"></a>Een build-definitie maken
De definitie van een Team Services build beschrijft een werkstroom die bestaat uit een reeks build stappen die sequentieel worden uitgevoerd. Het doel van de build-definitie die u maakt is voor het produceren van een Service Fabric-toepassingspakket en andere onderdelen, die kunnen worden gebruikt om de toepassing te implementeren. Meer informatie over Services Team [bouwen definities](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-the-build-template"></a>Een definitie van de build-sjabloon maken
1. Open uw teamproject in Visual Studio Team Services.
2. Selecteer de **bouwen** tabblad.
3. Selecteer de groene  **+**  Meld u aan een nieuwe build-definitie maken.
4. Selecteer in het dialoogvenster dat wordt geopend, **Azure Service Fabric-toepassing** binnen de **bouwen** sjablooncategorie.
5. Selecteer **volgende**.
6. Selecteer de opslagplaats en de vertakking die zijn gekoppeld aan uw Service Fabric-toepassing.
7. Selecteer de agent-wachtrij die u wilt gebruiken. Gehoste agents worden ondersteund.
8. Selecteer **Maken**.
9. Sla de definitie van de build en geef een naam op.
10. Het volgende lid is een beschrijving van de build-stappen die worden gegenereerd door de sjabloon:

| Stap bouwen | Beschrijving |
| --- | --- |
| NuGet terugzetten |Hiermee herstelt u de NuGet-pakketten voor de oplossing. |
| Oplossing bouwen \*.sln |De volledige oplossing is gebaseerd. |
| Oplossing bouwen \*.sfproj |Genereert het Service Fabric-toepassingspakket dat wordt gebruikt om de toepassing te implementeren. De locatie van het pakket toepassing is binnen de build artefact directory opgegeven. |
| Bijwerken van Service Fabric-App-versies |De Versiewaarden die zijn opgenomen in het toepassingspakket manifestbestanden om toe te staan voor de ondersteuning van upgrades bijgewerkt. Zie de [taak documentatiepagina](https://go.microsoft.com/fwlink/?LinkId=820529) voor meer informatie. |
| Bestanden kopiëren |Publiceren profiel en toepassing parameters bestanden gekopieerd naar de build artefacten te worden gebruikt voor implementatie. |
| Publiceren van artefacten |De build artefacten publiceert. Kan de definitie van een release te gebruiken van de build-artefacten. |

### <a name="verify-the-default-set-of-tasks"></a>Controleer of de standaardset van taken
1. Controleer of de **oplossing** invoerveld voor de **NuGet terugzetten** en **oplossing bouwen** stappen bouwen.  Standaard deze build-stappen uitvoeren op alle oplossingsbestanden die zijn opgenomen in de opslagplaats gekoppeld.  Als u wilt dat alleen de build-definitie bewerkingen uitvoeren op een van deze oplossingsbestanden, moet u expliciet het pad naar dat bestand bijwerken.
2. Controleer of de **oplossing** invoerveld voor de **pakkettoepassing** stap bouwen.  Standaard deze build stap wordt ervan uitgegaan dat slechts één Service Fabric-toepassing-project (.sfproj) bestaat in de opslagplaats.  Als u meerdere dergelijke bestanden in uw opslagplaats hebt en zich wilt richten slechts één van deze voor deze definitie build, moet u expliciet het pad naar dat bestand bijwerken.  Als u meerdere toepassing projecten in uw opslagplaats van het pakket wilt, moet u om aanvullende **Visual Studio bouwen** stappen in de definitie van de build elk gericht op een project voor een toepassing.  U hebt ook zou nodig om bij te werken de **MSBuild-argumenten** veld voor elk van deze stappen bouwen, zodat de pakketlocatie is uniek voor elk van deze servers.
3. Controleer of het gedrag versiebeheer is gedefinieerd in de **updateversies Service Fabric-App** stap bouwen.  Standaard wordt deze stap build het build-nummer toegevoegd aan alle Versiewaarden in de manifest-bestanden van het toepassingspakket. Zie de [taak documentatiepagina](https://go.microsoft.com/fwlink/?LinkId=820529) voor meer informatie. Dit is handig voor het ondersteunen van upgrade van uw toepassing omdat elke upgrade-implementatie vereist een andere Versiewaarden uit de vorige implementatie. Als u geen upgrade van de toepassing gebruiken in de werkstroom wilde bent, kunt u overwegen deze build stap uitschakelen. Worden moet uitgeschakeld als het uw bedoeling is voor het produceren van een build waarvan kan worden gebruikt voor het overschrijven van een bestaande Service Fabric-toepassing. De implementatie mislukt als de versie van de toepassing die wordt geproduceerd door de build komt niet overeen met de versie van de toepassing in het cluster.
4. Als uw oplossing een .NET Core-project bevat, moet u ervoor zorgen dat de definitie van de build een build stap de afhankelijkheden herstelt bevat:
   1. Selecteer **build-stap toevoegen...** .
   2. Zoek de **opdrachtregelprogramma** taak binnen het hulpprogramma tabblad en klik op de knop toevoegen.
   3. Gebruik voor de taak invoervelden, de volgende waarden:
   4. Hulpprogramma: dotnet
   5. Argumenten: herstellen
   6. Sleep de taak, zodat deze onmiddellijk na de **NuGet terugzetten** stap.
5. Sla alle wijzigingen aan de build-definitie.

### <a name="try-it"></a>Probeer het nu
Selecteer **wachtrij bouwen** een build handmatig te starten. Voortbouwt ook triggers op push of inchecken. Nadat u hebt geverifieerd dat de build met succes wordt uitgevoerd, kunt u nu verder met het definiëren van de definitie van een versie die uw toepassing naar een cluster wordt geïmplementeerd.

## <a name="create-a-release-definition"></a>Een release-definitie maken
De definitie van een Team Services release beschrijft een werkstroom die bestaat uit een aantal taken die sequentieel worden uitgevoerd. Het doel van de release-definitie die u maakt is moet worden overgenomen van een toepassingspakket en deze implementeren naar een cluster. Wanneer samen worden gebruikt, kunnen de definitie van de build en release definitie de volledige werkstroom uitvoeren vanuit beginnen met bronbestanden om te eindigen op een actieve toepassing in uw cluster. Meer informatie over Services Team [definities release](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-the-release-template"></a>Een definitie van de releasesjabloon maken
1. Open het project in Visual Studio Team Services.
2. Selecteer de **Release** tabblad.
3. Selecteer de groene  **+**  Meld u aan een nieuwe release-definitie maken en selecteer **maken release definitie** in het menu.
4. Selecteer in het dialoogvenster dat wordt geopend, **Azure Service Fabric-implementatie** binnen de **implementatie** sjablooncategorie.
5. Selecteer **volgende**.
6. Selecteer de build definitie die u wilt gebruiken als de bron van deze release-definitie.  De definitie van de release verwijst naar de artefacten die zijn gemaakt door de geselecteerde build-definitie.
7. Controleer de **continue implementatie** selectievakje in als u laten Team Services automatisch een nieuwe release gemaakt en de Service Fabric-toepassing implementeren wilt wanneer een build is voltooid.
8. Selecteer de agent-wachtrij die u wilt gebruiken. Gehoste agents worden ondersteund.
9. Selecteer **Maken**.
10. De Definitienaam bewerken door te klikken op het potloodpictogram boven aan de pagina.
11. Selecteer het cluster waarop de toepassing moet worden geïmplementeerd vanaf de **Cluster verbinding** invoerveld van de taak. De verbinding met cluster biedt de benodigde informatie waarmee u de implementatie-taak verbinding maken met het cluster. Als u nog geen cluster-verbinding voor uw cluster, selecteert u de **beheren** hyperlink naast het veld een toe te voegen. Op de pagina die wordt geopend, moet u de volgende stappen uitvoeren:
    1. Selecteer **nieuwe Service-eindpunt** en selecteer vervolgens **Azure Service Fabric** in het menu.
    2. Selecteer het type verificatie wordt gebruikt door het cluster dat door dit eindpunt is gericht.
    3. Definieer een naam voor de verbinding in de **verbindingsnaam** veld.  Normaal gesproken gebruikt u de naam van het cluster.
    4. Definieer de client verbinding eindpunt-URL in de **Clustereindpunt** veld.  Voorbeeld: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Voor Azure Active Directory-referenties, definieert u de referenties die u gebruiken wilt voor verbinding met het cluster in de **gebruikersnaam** en **wachtwoord** velden.
    6. Voor verificatie op basis van certificaten, definiëren de Base64-codering van het certificaatbestand in de **clientcertificaat** veld.  Zie de pop-help op het veld voor informatie over het verkrijgen van die waarde.  Als uw certificaat beveiligd met een wachtwoord is, definieert u het wachtwoord in de **wachtwoord** veld.
    7. Controleer uw wijzigingen door te klikken op **OK**. Na het navigeren naar de definitie van de release, klikt u op het pictogram Vernieuwen in de **Cluster verbinding** veld voor een overzicht van het eindpunt dat u zojuist hebt toegevoegd.
12. Sla de release-definitie.

> [!NOTE]
> Microsoft-Accounts (bijvoorbeeld @hotmail.com of @outlook.com) worden niet ondersteund met Azure Active Directory-verificatie.
> 
> 

De definitie die is gemaakt, bestaat uit één taak van het type **Service Fabric-toepassingsimplementatie**. Zie de [taak documentatiepagina](https://go.microsoft.com/fwlink/?LinkId=820528) voor meer informatie over deze taak.

### <a name="verify-the-template-defaults"></a>Controleer of de standaardinstellingen van de sjabloon
1. Controleer of de **publiceren profiel** invoerveld voor de **Service Fabric-toepassing implementeren** taak. Standaard is dit veld verwijst naar een publicatieprofiel met de naam Cloud.xml opgenomen in de build-artefacten. Als u wilt verwijzen naar een andere publicatieprofiel of als de build meerdere toepassingspakketten in de artefacten bevat, moet u het pad op de juiste wijze bijwerken.
2. Controleer of de **toepassingspakket** invoerveld voor de **Service Fabric-toepassing implementeren** taak. Standaard is dit veld verwijst naar de toepassing pakket standaardpad gebruikt in de sjabloon build-definitie.  Als u het standaardpad voor het pakket van toepassing in de definitie van de build hebt gewijzigd, moet u het pad op de juiste wijze hier ook bijwerken.

### <a name="try-it"></a>Probeer het nu
Selecteer **maken Release** van de **Release** menu van de knop handmatig maken van een release. Selecteer in het dialoogvenster dat wordt geopend, de build die u wilt de release baseren op en klik vervolgens op **maken**. Als u continue implementatie hebt ingeschakeld, wordt releases automatisch gemaakt wanneer de bijbehorende build-definitie een build is voltooid.

## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen voor meer informatie over continue integratie met Service Fabric-toepassingen:

* [Thuis Team Services-documentatie](https://www.visualstudio.com/docs/overview)
* [Beheer in Team Services bouwen](https://www.visualstudio.com/docs/build/overview)
* [Releasebeheer in Team Services](https://www.visualstudio.com/docs/release/overview)

