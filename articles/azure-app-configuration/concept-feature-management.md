---
title: Beheer van de functie Azure-App-configuratie | Microsoft Docs
description: Een overzicht van hoe Azure App-configuratie kan worden gebruikt om in te schakelen in- en uitschakelen toepassingsfuncties op aanvraag.
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
ms.openlocfilehash: 46f39e87e4e4cf115cbc1fceeabf0dab38fade28
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393361"
---
# <a name="feature-management-overview"></a>Overzicht van management-functies

Traditioneel moet verzending van een nieuwe functie van de toepassing een volledige opnieuw distribueren van de toepassing zelf. Als u wilt een functie testen, moet u waarschijnlijk uw toepassing vaak naar een besturingselement wanneer de functie zichtbaar wordt of om het te zien wie implementeren.

Beheer van de functie is een moderne softwareontwikkeling-procedure die wordt losgekoppeld functieversie van implementatie van code en kan snel wijzigingen in de beschikbaarheid van functies op aanvraag. Hierbij de techniek *functie vlaggen* (ook wel bekend als *Schakelknoppen functie*, *functieschakelopties zijn*, enzovoort) voor het dynamisch beheer van levenscyclus van een functie.

Functie management helpt ontwikkelaars omgaan met de volgende problemen:

* **Code vertakkingsbeheer**: Functie vlaggen worden vaak gebruikt het inpakken van nieuwe functionaliteit van de toepassing die is in ontwikkeling. Deze functionaliteit is "" standaard verborgen. U kunt de functie veilig verzenden zelfs als er onvoltooide, en blijft aanwezig in de productieomgeving. Met behulp van deze benadering, met de naam *donker implementatie*, u kunt alle uw code vrijgeven aan het einde van elke ontwikkelingscyclus. U moet niet meer onderhouden van een vertakking van de code in meerdere cycli vanwege een functie waarbij meer dan één cyclus en worden voltooid.
* **Testen tijdens productie**: Vlaggen voor functie kunt u vroege toegang verlenen tot nieuwe functionaliteit in de productieomgeving. Bijvoorbeeld, kunt u deze toegang beperken naar alleen de leden van uw team of interne bètatesters. Deze gebruikers krijgen de hoogwaardige productie-ervaring, in plaats van een gesimuleerde of gedeeltelijke ervaring in een testomgeving.
* **Flighting**: U kunt ook de functie vlaggen gebruiken incrementeel implementeren nieuwe functionaliteit voor eindgebruikers. U kunt richten op een klein percentage van de gebruikerspopulatie eerst en vergroten dat percentage geleidelijk gedurende een periode, nadat u hebben opgedaan met meer vertrouwen in de implementatie.
* **Directe kill switch**: Functie vlaggen bevatten een inherente veiligheidsnet voor het vrijgeven van nieuwe functionaliteit. U kunt gemakkelijk toepassingsfuncties inschakelen met hen, in- en uitschakelen. Indien nodig, kunt u snel een functie uitschakelen zonder opnieuw te bouwen en uw toepassing opnieuw te implementeren.
* **Selectief activering**: Terwijl de meeste functie vlaggen bestaat alleen totdat hun bijbehorende functies met succes zijn vrijgegeven, kunnen sommige laatste gedurende een lange periode. Vlaggen voor functie kunt u hiermee uw gebruikers en levert u een specifieke set functies aan elke groep. Bijvoorbeeld, wellicht u een functie waarmee u alleen op een bepaalde webbrowser werkt. U kunt een functievlag definiëren zodat alleen gebruikers van de browser kunnen zien en gebruiken van de functie. Met deze methode kunt u eenvoudig uitbreiden de lijst met ondersteunde browser later zonder te hoeven aanbrengen van codewijzigingen.

## <a name="basic-concepts"></a>Basisbegrippen

Hier volgen enkele nieuwe voorwaarden met betrekking tot beheer van de functie:

* **Vlag functie**: Een functievlag is een variabele met een binaire status van *op* of *uit*. De functievlag is ook een bijbehorende codeblok. De status van de markering van de functie wordt geactiveerd of het codeblok wordt uitgevoerd of niet.
* **Functie manager**: De beheerder van een functie is een toepassingspakket die verantwoordelijk is voor de levenscyclus van de functie-vlaggen in een toepassing. De functie manager biedt doorgaans aanvullende functionaliteit, zoals caching functie vlaggen en hun status bij te werken.
* **Filter**: Een filter is een regel voor het evalueren van de status van een functievlag. Een gebruikersgroep, een apparaat of de browser type een geografische locatie en een bepaalde periode zijn alle voorbeelden van wat een filter kan vertegenwoordigen.

Een succesvolle implementatie van het beheer van de functie bestaat uit ten minste twee onderdelen werken samen:

* Een toepassing die u maakt gebruik van de functie vlaggen.
* Een aparte opslagplaats waarin de functie-vlaggen en hun huidige status.

Interactie tussen deze onderdelen wordt weergegeven in de volgende voorbeelden.

## <a name="feature-flag-usage-in-code"></a>Gebruik van functies vlag in code

Het algemene patroon voor het implementeren van functie-vlaggen in een toepassing is eenvoudig. U kunt een functievlag beschouwen als een variabele met Booleaanse status gebruikt in combinatie met een `if` voorwaardelijke statement in uw code:

```csharp
if (featureFlag) {
    // Run the following code
}
```

In dit geval als `featureFlag` is ingesteld op `True`, het blok ingesloten code is uitgevoerd, anders wordt deze overgeslagen. U kunt de waarde van `featureFlag` statisch, zoals in het volgende codevoorbeeld:

```csharp
bool featureFlag = true;
```

U kunt ook evalueren van de markering-status op basis van bepaalde regels:

```csharp
bool featureFlag = isBetaUser();
```

Een iets gecompliceerdere functie vlag patroon bevat een `else` instructie ook:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

U kunt dit gedrag in het algemene patroon, maar herschrijven. [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md) ziet u de voordelen van een patroon eenvoudig code standaardiseren. Bijvoorbeeld:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Functie vlag opslagplaats

Functie vlaggen als effectief wilt gebruiken, moet u extern maken van alle functie-markeringen die worden gebruikt in een toepassing. Deze aanpak kunt u vlag Functiestatus wijzigen zonder te wijzigen en opnieuw implementeren van de toepassing zelf.

Azure App-configuratie is ontworpen om te worden van een centrale opslagplaats voor de functie vlaggen. U kunt deze gebruiken voor het definiëren van verschillende soorten functie vlaggen en manipuleren van hun status snel en met vertrouwen. Vervolgens kunt u in de App-configuratie-bibliotheken voor verschillende programming taalframeworks eenvoudig toegang krijgen tot deze functie vlaggen van uw toepassing.

[Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md) laat zien hoe de configuratieprovider voor .NET Core-App en de functie Management-bibliotheken samen gebruikt worden voor het implementeren van de functie vlaggen voor uw ASP.NET-webtoepassing.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vlaggen voor functie toevoegen aan een ASP.NET Core web-app](./quickstart-feature-flag-aspnet-core.md)  
