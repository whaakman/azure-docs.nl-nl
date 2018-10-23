---
title: Overzicht van Azure Digital Twins | Microsoft Docs
description: Kom meer te weten over Azure Digital Twins, een Azure-IoT-oplossing voor ruimtelijke intelligentie.
author: julieseto
ms.author: jseto
ms.date: 10/10/2018
ms.topic: overview
ms.service: digital-twins
services: digital-twins
manager: bertvanhoof
ms.custom: mvc
ms.openlocfilehash: 85b67683730c8352051b4d6b48f813576b11615b
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322757"
---
# <a name="overview-of-azure-digital-twins"></a>Overzicht van Azure Digital Twins

Azure Digital Twins is een Azure IoT-service waarmee uitgebreide modellen van de fysieke omgeving worden gemaakt. Een van de vele functies is de mogelijkheid om grafieken voor ruimtelijke intelligentie te maken voor het modelleren van de relaties en interacties tussen mensen, ruimten en apparaten.

Met Azure Digital Twins kunnen gegevens worden opgevraagd van een fysieke ruimte in plaats van uit veel verschillende sensoren. Deze service helpt u om herbruikbare, zeer schaalbare, ruimtebewuste ervaringen te bouwen die streaminggegevens uit de digitale en fysieke wereld koppelen. Of u nu de onderhoudsbehoeften van een fabriek voorspelt, realtime energievereisten voor een elektriciteitsnet analyseert of het gebruik van de beschikbare ruimte voor een kantoor optimaliseert, uw apps worden verbeterd met deze unieke relevante contextuele functies.

Azure Digital Twins is van toepassing op alle typen omgevingen, bijvoorbeeld warenhuizen, kantoren, scholen, ziekenhuizen, banken, stadions, fabrieken, parkeerplaatsen, parken, slimme netwerken en steden. Denk bijvoorbeeld aan scenario's zoals het bijhouden van de dagelijkse temperatuur in verschillende statussen, het bewaken van drukke droneroutes, de identificatie van autonome voertuigen, het analyseren van bezettingsniveaus van een gebouw of het bepalen welke kassa in de winkel het meest wordt gebruikt. Wat uw bedrijfsscenario ook is, er kan een overeenkomstig digitaal exemplaar worden ingericht via Azure Digital Twins.

In de volgende video wordt Digital Twins gedetailleerder besproken:

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

De belangrijkste mogelijkheden van Azure Digital Twins zijn onder andere:

### <a name="spatial-intelligence-graph"></a>Grafiek voor ruimtelijke intelligentie

De [*grafiek voor ruimtelijke intelligentie* ](./concepts-objectmodel-spatialgraph.md) of *ruimtelijke grafiek* is een virtuele weergave van de fysieke omgeving waarmee u de relaties tussen personen, locaties en apparaten kunt modelleren.

Overweeg een slimme app voor nutsvoorzienngen die kan samenwerken met verschillende verbonden elektriciteitverbruiksmeters in een omgeving. Voor het nauwkeurig bewaken en voorspellen van elektriciteitsverbruik en -facturering, moet het bedrijf met de slimme elektriciteitsmeter alle apparaten en sensoren modelleren met context over de locatie en de klant die wordt gefactureerd. Dankzij de grafiek voor ruimtelijke intelligentie kunt u dit soort complexe relaties modelleren.

### <a name="digital-twin-object-models"></a>Digital Twin-objectmodellen

[Digital Twin-objectmodellen](./concepts-objectmodel-spatialgraph.md) zijn vooraf gedefinieerde apparaatprotocollen en -gegevensschema's die de domeinspecifieke behoeften van uw oplossing afstemmen om de ontwikkeling te versnellen en vereenvoudigen.

Een toepassing voor de bezetting van een ruimte kan bijvoorbeeld vooraf gedefinieerde ruimtetypen gebruiken, zoals een campus, gebouw, verdieping, ruimte enzovoort.

### <a name="multiple-and-nested-tenants"></a>Meerdere en geneste tenants

U kunt oplossingen bouwen die veilig schalen en die kunnen worden hergebruikt voor meerdere tenants. U kunt ook meerdere subtenants maken die op een geïsoleerde en veilige manier kunnen worden geopend en gebruikt.

Een app voor ruimtegebruik kan bijvoorbeeld worden geconfigureerd om de gegevens van een tenant te isoleren van gegevens van andere tenants binnen een gebouw of om gegevens voor een enkele tenant te combineren voor meerdere gebouwen.

### <a name="advanced-compute-capabilities"></a>Geavanceerde rekenmogelijkheden

Met geavanceerde rekenmogelijkheden die [door de gebruiker gedefinieerde functies](./concepts-user-defined-functions.md) worden genoemd, kunt u aangepaste functies definiëren en uitvoeren op basis van binnenkomende [apparaatgegevens](./concepts-device-ingress.md) om signalen te verzenden naar vooraf gedefinieerde eindpunten. Deze functie verbetert de aanpassing en automatisering van apparaattaken.

Een slimme landbouwtoepassing kan bijvoorbeeld een door de gebruiker gedefinieerde functie gebruiken om door sensoren gemeten grondvochtwaarden en weersvoorspellingen te beoordelen en vervolgens signalen over de behoefte aan irrigatie te verzenden.

### <a name="built-in-access-control"></a>Ingebouwd toegangsbeheer

Met toegangs- en identiteitsbeheerfuncties zoals [op rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md) en [Azure Active Directory](./security-authenticating-apis.md) kunt u de toegang van personen en apparaten veilig beheren.

Een app voor faciliteitbeheer kan bijvoorbeeld zo worden geconfigureerd dat gebruikers van een ruimte de temperatuur binnen een opgegeven bereik kunnen instellen, en dat faciliteitenmanagers in alle ruimten elke gewenste temperatuur kunnen instellen.

### <a name="ecosystem"></a>Ecosysteem

U kunt een exemplaar van Azure Digital Twins verbinden met veel krachtige Azure-services, waaronder: Azure Stream Analytics, AI- en Storage-services, evenals Azure Maps, Microsoft Mixed Reality, Dynamics 365 of Office 365.

Een slimme toepassing voor kantoorgebouwen kan bijvoorbeeld Azure Digital Twins gebruiken om teams en apparaten op een groot aantal verdiepingen te vertegenwoordigen. Als apparaten live-gegevens naar het ingerichte Digital Twin-exemplaar streamen, kan Azure Stream Analytics die gegevens verwerken om tot belangrijke conclusies te komen waarop actie kan worden ondernomen. Vervolgens kunnen de gegevens worden opgeslagen in Azure Storage en worden geconverteerd naar een bestandsindeling die via Office 365 kan worden gedeeld en kan worden verspreid over de hele organisatie.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Oplossingen die profiteren van Azure Digital Twins

Azure Digital Twins is handig voor de representatie van de fysieke wereld en de vele bijbehorende relaties, aangezien de oplossing zorgt voor een vereenvoudiging van IoT-modellen, gegevensverwerking, gebeurtenisafhandeling en apparaattracking. Hieronder volgen nog enkele mogelijke scenario's in diverse branches, waarin de oplossing van pas komt:

* Weergave van de bezettingsniveaus van een ruimte gedurende een langere periode voor een vastgoedbeheerbedrijf, om inzichten te verzamelen over de beste manieren om het kantoorgebouw te configureren.
* Het activeren van werkordertickets voor een mobiele app voor het inzetten van beveiligingsmedewerkers, de planning van schoonmaakdiensten en andere services in een detailhandelruimte of sportlocatie.
* Realtime weergave van welke ruimten in een gebouw bezet zijn. Vervolgens kunnen de gebruikers de gewenste ruimten reserveren.
* Bijhouden waar bedrijfsmiddelen zich in een ruimte bevinden.
* Het opladen van elektrische voertuigen optimaliseren door gebruikersvoorkeuren en beperkingen van het energienetwerk te modelleren.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Azure Digital Twins in de context van andere IoT-services

Azure Digital Twins gebruikt Azure IoT Hub om een verbinding tot stand te brengen tussen de IoT-apparaten en sensoren die alles up-to-date houden met de fysieke wereld. In het volgende diagram ziet u hoe Azure Digital Twins zich verhoudt tot andere Azure IoT-services:

![Azure Digital Twins is een service die is gebouwd op Azure IoT Hub](./media/overview/azure-digital-twins-in-iot-ecosystem.png)

Zie voor een gedetailleerde beschrijving van de rest van het IoT-ecosysteem [Azure IoT-technologieën en -oplossingen](https://docs.microsoft.com/azure/iot-fundamentals/iot-services-and-technologies).

## <a name="next-steps"></a>Volgende stappen

Bekijk een korte demo van de mogelijkheden van Azure Digital Twins:

> [!div class="nextstepaction"]
> [Snelstart: Beschikbare ruimten zoeken met behulp van Azure Digital Twins](./quickstart-view-occupancy-dotnet.md)

Dieper ingaan op een toepassing voor faciliteitbeheer met behulp van Azure Digital Twins:

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Digital Twins implementeren en een ruimtelijke grafiek configureren](./tutorial-facilities-setup.md)

Meer informatie over de basisconcepten van Azure Digital Twins:

> [!div class="nextstepaction"]
> [Informatie over het objectmodel en de grafiek voor ruimtelijke intelligentie van Digital Twins](./concepts-objectmodel-spatialgraph.md)
