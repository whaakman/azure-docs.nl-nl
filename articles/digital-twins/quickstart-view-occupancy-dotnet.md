---
title: Beschikbare ruimten zoeken met Azure Digital Twins (C#) | Microsoft Docs
description: In deze snelstart voert u twee .NET Core-voorbeeldtoepassingen uit om telemetriegegevens over gesimuleerde beweging en koolstofdioxide te verzenden naar een ruimte in Azure Digital Twins. Het doel is om beschikbare ruimten met frisse lucht van beheer-API's te vinden na berekende verwerking in de cloud.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/7/2018
ms.author: alinast
ms.openlocfilehash: 6e83ca543937948ad8028969cceca0f8787972c9
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281215"
---
# <a name="quickstart-find-available-rooms-using-azure-digital-twins"></a>Snelstart: Beschikbare ruimten zoeken met behulp van Azure Digital Twins

Met de Azure Digital Twins-service kunt u een digitale kopie van uw fysieke omgeving opnieuw maken. U kunt vervolgens een melding ontvangen van gebeurtenissen in uw omgeving en uw reacties hierop aanpassen. 

Deze snelstart maakt gebruik van [een combinatie van .NET-voorbeelden](https://github.com/Azure-Samples/digital-twins-samples-csharp) voor het digitaliseren van een denkbeeldige kantoorgebouw, en laat u zien hoe u beschikbare ruimten kunt vinden in het gebouw. Met Digital Twins kunt u meerdere sensoren koppelen aan uw omgeving. Samen met de beschikbaarheid van ruimten kunt u hier met behulp van een gesimuleerde sensor voor koolstofdioxide ook vaststellen of de luchtkwaliteit van de beschikbare ruimte optimaal is. Door een van de voorbeeldtoepassingen worden willekeurige sensorgegevens gegenereerd, zodat u dit scenario beter kunt visualiseren.

De volgende video geeft een overzicht van de installatie van de snelstart:

> [!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Vereisten

1. Als u geen Azure-account hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

1. De twee consoletoepassingen die u uitvoert in deze snelstart zijn geschreven in C#. U moet [.NET Core SDK-versie 2.1.403 of hoger](https://www.microsoft.com/net/download) op de ontwikkelcomputer installeren. Als u .NET Core-SDK hebt geïnstalleerd, kunt u de huidige versie van C# op uw ontwikkelmachine controleren door de `dotnet --version` in een opdrachtprompt uit te voeren.

1. Download het [C#-voorbeeldproject](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) en pak het archief digital-twins-samples-csharp-master.zip uit. 


## <a name="create-a-digital-twins-instance"></a>Een Digital Twins-exemplaar maken

Maak een nieuw exemplaar van Digital Twins in de [portal](https://portal.azure.com) met behulp van de stappen in deze sectie.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Machtigingen instellen voor uw app

In deze sectie registreert u uw voorbeeldtoepassing in Azure Active Directory (AAD), zodat deze toegang heeft tot uw Digital Twins-exemplaar. Als u al een AAD-app-registratie hebt, kunt u deze mogelijk opnieuw gebruiken voor uw voorbeeld. Zorg er wel voor dat deze is geconfigureerd zoals vermeld in deze sectie. 

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="build-application"></a>Toepassing bouwen

Gebruik de volgende opdracht om de bezettingstoepassing te maken:

1. Open een opdrachtprompt en navigeer naar de map waarin uw digital-twins-samples-csharp-master.zip-bestanden zijn uitgepakt.
1. Voer `cd occupancy-quickstart/src` uit.
1. Voer `dotnet restore` uit.
1. Bewerk *appSettings.json* om de volgende variabelen bij te werken:
    - *ClientId*: voer de *Toepassings-ID* van uw AAD-app-registratie in, die u in de voorgaande sectie hebt genoteerd.
    - *Tenant*: voer de *Map-Id* van uw AAD-tenant in, die u ook in de vorige sectie hebt genoteerd.
    - *BaseUrl*: de URL van de *Beheer-API* van uw Digital Twins-exemplaar, die wordt weergegeven in de volgende indeling `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Vervang de tijdelijke aanduidingen in deze URL door de waarden voor uw exemplaar uit de vorige sectie.

## <a name="provision-graph"></a>Grafiek inrichten

In deze stap wordt uw ruimtelijke Digital Twins-grafiek ingericht met verschillende ruimten, één apparaat, twee sensoren, een aangepaste functie en één roltoewijzing. De ruimtelijke grafiek wordt ingericht met behulp van het bestand [*provisionSample.yaml*](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

1. Voer `dotnet run ProvisionSample` uit.
    >[!NOTE]
    >We gebruiken het programma voor apparaataanmelding van de Azure CLI om de gebruiker bij Azure Active Directory te verifiëren. De gebruiker moet een bepaalde code invoeren om zich te verifiëren met behulp van [de Microsoft-aanmeldingspagina](https://microsoft.com/devicelogin). Nadat de code is ingevoerd, volgt u de stappen voor verificatie. De gebruiker wordt telkens wanneer het hulpprogramma wordt uitgevoerd verzocht om zich te verifiëren.
    
    >[!TIP]
    > Als u de volgende fout ontvangt bij het uitvoeren van deze stap, controleert u of uw variabelen juist zijn gekopieerd. 
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`


1. De inrichtingsstap kan enkele minuten duren. Er wordt ook een IoT Hub in uw Digital Twins-exemplaar ingericht en deze wordt uitgevoerd totdat IoTHub Status=`Running` heeft.

    ![Voorbeeld van inrichting][4]

1. Aan het einde van de uitvoering, kopieert u de `ConnectionString` van het apparaat voor gebruik in de voorbeeldapparaatsimulator. Kopieer alleen de tekenreeks die wordt beschreven op de onderstaande afbeelding:

    ![Voorbeeld van inrichting][1]

## <a name="send-sensor-data"></a>Sensorgegevens verzenden

U kunt de sensorsimulatortoepassing bouwen en uitvoeren met behulp van de onderstaande stappen:

1. Open een nieuwe opdrachtprompt en navigeer naar het project dat u hebt gedownload, in de map digital-twins-samples-csharp-master.
1. Voer `cd device-connectivity` uit.
1. Voer `dotnet restore` uit.
1. Bewerk *appsettings.json* om *DeviceConnectionString* bij te werken met de `ConnectionString` hierboven.
1. Voer `dotnet run` uit om te beginnen met het verzenden van sensorgegevens. U ziet dat deze naar de Digital Twins-service worden verzonden zoals op de onderstaande afbeelding wordt weergegeven:

     ![Connectiviteit van apparaten][2]

1. Voer deze simulator uit, zodat u de resultaten naast de actie van de volgende stap kunt bekijken. In dit venster worden de gesimuleerde sensorgegevens weergegeven die zijn verzonden naar Digital Twins. In de volgende stap wordt in realtime een query uitgevoerd om te zoeken naar beschikbaar ruimten met frisse lucht.

    >[!TIP]
    > Als u de volgende fout ontvangt bij het uitvoeren van deze stap, controleert u of uw `DeviceConnectionString` op de juiste manier is gekopieerd.  
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Beschikbare ruimten met frisse lucht zoeken

In het sensorvoorbeeld worden willekeurige gegevenswaarden voor twee sensoren, beweging en koolstofdioxide, gesimuleerd. In ons voorbeeld worden beschikbare ruimten met frisse lucht gedefinieerd door geen aanwezigheid in de ruimte en een koolstofdioxidegehalte dat onder de 1000 ppm ligt. Als niet aan de voorwaarde wordt voldaan, is de ruimte niet beschikbaar of is de luchtkwaliteit slecht.

1. Open de opdrachtprompt die u hebt gebruikt voor het uitvoeren van de bovenstaande inrichtingsstap.
1. Voer `dotnet run GetAvailableAndFreshSpaces` uit.
1. Bekijk deze opdrachtprompt en de opdrachtprompt voor sensorgegevens naast elkaar zoals hieronder wordt beschreven. 
1. De ene opdrachtprompt stuurt gesimuleerde gegevens over beweging en koolstofdioxide om de 5 seconden naar Digital Twins. De andere opdrachtprompt leest in realtime de grafiek om beschikbare ruimten met frisse lucht te vinden op basis van willekeurige gesimuleerde gegevens. Een van deze voorwaarden wordt in bijna realtime weergegeven op basis van welke sensorgegevens de laatste keer zijn verzonden:
    - Beschikbare ruimten met frisse lucht.
    - Ruimte bezet of slechte luchtkwaliteit in de ruimte.

     ![Beschikbare ruimten met frisse lucht ophalen][3]

Om te begrijpen wat is er gebeurd in deze snelstart en welke API's zijn aangeroepen, opent u [Visual Studio Code](https://code.visualstudio.com/Download) met het codewerkruimteproject dat zich in digital-twins-samples-csharp (zie de onderstaande opdracht) bevindt. In de zelfstudies wordt uitgebreid ingegaan op de code en u leert er hoe u configuratiegegevens kunt wijzigen en welke API's worden aangeroepen. Ga voor meer inzicht in de beheer-API's naar uw Digital Twins Swagger-pagina `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net//management/swagger` of blader voor uw gemak naar [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger). 

```
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

## <a name="clean-up-resources"></a>Resources opschonen

In de zelfstudies wordt gedetailleerd ingegaan op hoe u een toepassing bouwt voor faciliteitsbeheerders waarmee zij de bezettingsproductiviteit kunnen verhogen en het gebouw efficiënter kunnen exploiteren.

Als u wilt doorgaan met de zelfstudies, verwijder dan niet de resources die u in deze snelstart hebt gemaakt. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze snelstart zijn gemaakt, te verwijderen:

1. Verwijder de map die is gemaakt bij het downloaden van de opslagplaats met voorbeelden.
1. Klik in het linkermenu van de [Azure-portal](http://portal.azure.com) op **Alle resources** en selecteer uw Digital Twins-resource. Klik boven aan het deelvenster **Alle resources** op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een overzicht gekregen van een eenvoudig scenario voor het vinden van ruimten met goede werkomstandigheden. Voor een meer diepgaande analyse van dit scenario gaat u verder met deze zelfstudie:

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Digital Twins implementeren en een ruimtelijke grafiek configureren](tutorial-facilities-setup.md)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
