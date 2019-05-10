---
title: Beheer van de functie Azure-App-configuratie | Microsoft Docs
description: Een overzicht van hoe Azure App-configuratie kan worden gebruikt om in te schakelen in- en uitschakelen toepassing onderdelen op verzoek
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 304ca08275ccf4bf32d38e3fc89dd5cf07460fa4
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413770"
---
# <a name="feature-management"></a>Functiebeheer

Traditioneel verzend een nieuwe functie van de toepassing moet een volledige opnieuw distribueren van de toepassing zelf. Als u wilt testen van een functie, hebt u waarschijnlijk het implementeren van uw toepassing vaak naar een besturingselement wanneer deze zichtbaar wordt of wie er om het te bekijken. Functie management is een procedure voor het ontwikkelen van moderne software die worden losgekoppeld functieversie van implementatie van code en kan snel wijzigingen in de beschikbaarheid van functies op aanvraag. Hierbij de techniek *functie vlag* (ook wel bekend als *functie in-/ uitschakelen*, *functieswitch*, enzovoort) voor het beheer van levenscyclus van de functie dynamisch. Het helpt ontwikkelaars omgaan met de volgende problemen:

* **Code vertakkingsbeheer**: Functie vlaggen worden vaak gebruikt het inpakken van nieuwe functionaliteit van de toepassing die is in ontwikkeling. Deze functionaliteit is "" standaard verborgen. U kunt veilig af te leveren, hoewel onvoltooide en blijft aanwezig in de productieomgeving. Met behulp van deze benadering met de naam *donker implementatie*, u kunt alle uw code vrijgeven aan het einde van elke ontwikkelingscyclus. U moet niet meer onderhouden van een vertakking van de code in meerdere cycli vanwege een functie waarbij meer dan één punt te worden voltooid.
* **Testen tijdens productie**: Vlaggen voor functie kunt u vroege toegang verlenen tot nieuwe functionaliteit in de productieomgeving. Bijvoorbeeld, kunt u deze toegang tot alleen uw teamleden of interne bètatesters beperken. Deze gebruikers krijgen de hoogwaardige productie-ervaring, in plaats van een gesimuleerde of gedeeltelijke ervaring in een testomgeving.
* **Flighting**: U kunt ook de functie vlaggen gebruiken in nieuwe functionaliteit voor eindgebruikers incrementeel. U kunt richten op een klein percentage van de gebruikerspopulatie eerst en vergroten dat percentage geleidelijk gedurende een periode, nadat u hebben opgedaan met meer vertrouwen in de implementatie.
* **Directe kill switch**: Functie vlaggen bevatten een inherente veiligheidsnet voor het vrijgeven van nieuwe functionaliteit. Met deze, kunt u niet alleen inschakelen maar ook afgesloten toepassingsfuncties gemakkelijk. U krijgt de mogelijkheid snel uitschakelen van een functie, indien nodig, zonder de noodzaak om te bouwen en implementeren van uw toepassing.
* **Selectief activering**: Terwijl de meeste functie vlaggen bestaat alleen totdat hun bijbehorende functies met succes zijn vrijgegeven, kunnen sommige laatste gedurende een lange periode. U kunt deze gebruiken als een manier om uw gebruikers in segmenten en levert u een specifieke set functies aan elke groep. Bijvoorbeeld, wellicht u een functie die geschikt is voor een bepaalde webbrowser. U kunt een functievlag definiëren zodanig dat alleen gebruikers van de browser kunnen zien en gebruiken van de functie. Het voordeel van deze benadering is dat u kunt eenvoudig Vouw de lijst met ondersteunde browser later zonder te hoeven maken van code wijzigen.

## <a name="basic-concepts"></a>Basisbegrippen

Hier volgen enkele nieuwe voorwaarden met betrekking tot beheer van de functie.

* **Vlag functie**: Een functievlag is een variabele met een binaire status van *op* of *uit* en heeft een bijbehorende codeblok. De status wordt bepaald of het codeblok wordt uitgevoerd of niet.
* **Functie manager**: De beheerder van een functie is een toepassingspakket die verantwoordelijk is voor de levenscyclus van alle functie-vlaggen in een toepassing. Het biedt doorgaans aanvullende functionaliteit, zoals in cache plaatsen van vlaggen en bijwerken van hun status.
* **Filter**: Een filter is een regel voor het evalueren van de status van een functievlag. Gebruikersgroep, apparaat of de browser type, geografische locatie, tijdvenster zijn alle voorbeelden van wat een filter kan vertegenwoordigen.

Een succesvolle implementatie van het beheer van de functie bestaat uit ten minste twee onderdelen werken samen: een toepassing die gebruik van de functie vlaggen en een aparte-opslagplaats die maakt functie vlaggen en hun huidige status worden opgeslagen. De interacties worden hieronder weergegeven.

## <a name="feature-flag-usage-in-code"></a>Gebruik van functies vlag in code

Het algemene patroon voor het implementeren van functie-vlaggen in een toepassing is eenvoudig. U kunt deze visualiseren als een variabele met Booleaanse status gebruikt in combinatie met een `if` voorwaardelijke statement in uw code.

```csharp
if (featureFlag) {
    // Run the following code
}
```

In dit geval als `featureFlag` is ingesteld op `True`, het ingesloten codeblok wordt uitgevoerd; anders wordt deze overgeslagen. De waarde van `featureFlag` statisch kan worden ingesteld:

```csharp
bool featureFlag = true;
```

Deze kan ook worden geëvalueerd op basis van bepaalde regels:

```csharp
bool featureFlag = isBetaUser();
```

Een iets gecompliceerdere functie vlag patroon bevat een `else` instructie ook.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Dit kan worden herschreven in het algemene patroon, maar. [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md) ziet u het voordeel van een patroon eenvoudig code standaardiseren.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Functie vlag opslagplaats

Voor het gebruik ervan effectief, moet u extern maken van alle functie-markeringen gebruikt in een toepassing. Dat kunt u de vlag Functiestatus wijzigen zonder te wijzigen en opnieuw implementeren van de toepassing zelf. Azure App-configuratie is ontworpen om te worden van een centrale opslagplaats voor de functie vlaggen. U kunt gebruiken om verschillende soorten functie vlaggen definiëren en manipuleren van hun status snel en met vertrouwen. U kunt deze vlaggen functie vervolgens eenvoudig openen vanuit uw toepassing met behulp van de App-configuratie-bibliotheken voor verschillende programming taalframeworks. [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md) laat zien hoe de configuratieprovider voor .NET Core-App en de functie Management-bibliotheken worden gebruikt als een volledige oplossing voor het implementeren van de functie vlaggen voor uw ASP.NET-webtoepassing.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vlaggen voor functie toevoegen aan een ASP.NET Core web-app](./quickstart-feature-flag-aspnet-core.md)  
