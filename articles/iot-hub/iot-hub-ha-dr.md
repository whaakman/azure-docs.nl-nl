---
title: Azure IoT Hub hoge beschikbaarheid en herstel na noodgevallen | Microsoft Docs
description: Beschrijft de functies voor Azure en IoT-Hub die u helpen bij het ontwikkelen van maximaal beschikbare Azure-IoT-oplossingen met disaster recovery-mogelijkheden.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: elioda
ms.openlocfilehash: 992c42511f7bc9e9af71ff552ee91bc2472ebcf8
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185700"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hoge beschikbaarheid en herstel na noodgevallen
Als een Azure-service biedt IoT Hub hoge beschikbaarheid (HA) met behulp van redundantie op het niveau van de Azure-regio, zonder extra werk vereist voor de oplossing. De Microsoft Azure-platform bevat ook functies voor informatie over het bouwen van oplossingen met mogelijkheden voor disaster recovery (DR) of regio-overschrijdende beschikbaarheid. Als u wilt voor globale, interregionale hoge beschikbaarheid voor apparaten of gebruikers profiteren van deze functies Azure herstel na Noodgevallen. Het artikel [technische richtlijnen voor Azure Business Continuity](../resiliency/resiliency-technical-guidance.md) beschrijving van de ingebouwde functies in Azure voor bedrijfscontinuïteit en herstel na Noodgevallen. De [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen] [ Disaster recovery and high availability for Azure applications] document bevat architectuurrichtlijnen voor strategieën voor Azure-toepassingen om HA en DR te realiseren.

## <a name="azure-iot-hub-dr"></a>Herstel na Noodgevallen voor Azure IoT Hub
IoT Hub implementeert naast intra-regio HA, failover-mechanismen voor herstel na noodgevallen waarvoor geen tussenkomst van de gebruiker. Herstel na Noodgevallen van IoT Hub wordt automatisch gestart en heeft een beoogde hersteltijd (RTO) van 2-26 uur en de volgende herstelpuntdoelen (RPO's):

| Functionaliteit | RPO |
| --- | --- |
| Beschikbaarheid van de service voor register- en communicatiebewerkingen |CName gegevensverlies |
| Identiteitsgegevens in register-id 's |0-5 minuten gegevensverlies |
| Apparaat-naar-cloud-berichten |Alle ongelezen berichten gaan verloren |
| Bewerkingen controleren van berichten |Alle ongelezen berichten gaan verloren |
| Cloud-naar-apparaat-berichten |0-5 minuten gegevensverlies |
| Cloud-naar-apparaat feedbackwachtrij |Alle ongelezen berichten gaan verloren |
| Dubbele apparaatgegevens |0-5 minuten gegevensverlies |
| Bovenliggende en apparaat-taken |0-5 minuten gegevensverlies |

## <a name="regional-failover-with-iot-hub"></a>Regionale failover met IoT Hub
Een volledige verwerking van de implementatietopologieën in IoT-oplossingen is buiten het bereik van dit artikel. Het artikel behandelt de *regionale failover* implementatiemodel ten behoeve van hoge beschikbaarheid en herstel na noodgevallen.

In een regionale failover-model, de oplossing back-end wordt voornamelijk in één datacenterlocatie. Een secundaire IoT-hub en de back-end zijn geïmplementeerd in een ander datacenterlocatie. Als de IoT hub in het primaire datacenter een storing voordoet of de netwerkverbinding van het apparaat naar het primaire datacenter wordt onderbroken, apparaten een secundaire service-eindpunt gebruiken. U kunt de oplossing beschikbaarheid verbeteren door het implementeren van een model interregionale failover in plaats van binnen één regio blijft. 

Op een hoog niveau voor het implementeren van een model regionale failover met IoT Hub, moet u het volgende:

* **Een secundaire IoT-hub en apparaat routering logische**: als de service in de primaire regio wordt onderbroken, apparaten verbinding maken met uw secundaire regio moeten beginnen. Gezien de statusbewust aard van de meeste services die betrokken zijn, is het gebruikelijk voor beheerders van de oplossing voor het activeren van het failoverproces tussen regio's. De beste manier om te communiceren van het nieuwe eindpunt naar apparaten, terwijl het beheer van het proces is dat ze regelmatig controleren op een *concierge* service voor het huidige actieve eindpunt. De conciërge-service kan een webtoepassing die wordt gerepliceerd en opgeslagen bereikbaar zijn met behulp van DNS-omleiding technieken (bijvoorbeeld met behulp van [Azure Traffic Manager][Azure Traffic Manager]).
* **Replicatie voor Identity-registry**: om te worden gebruikt, de secundaire IoT-hub alle apparaat-id's die u verbinding met de oplossing maken kunnen moet bevatten. De oplossing moet bewaren van back-ups via geo-replicatie van apparaat-id's en upload deze naar de secundaire IoT-hub voordat u overschakelt van de actieve eindpunt voor de apparaten. De exportfunctie van apparaat-id van IoT-Hub is handig in deze context. Zie voor meer informatie, [het Ontwikkelaarshandleiding voor IoT Hub - identiteitsregister][IoT Hub developer guide - identity registry].
* **Samenvoegen van logische**: wanneer de primaire regio weer beschikbaar is, alle de status en gegevens die zijn gemaakt op de secundaire site moeten worden gemigreerd terug naar de primaire regio. Deze status en gegevens, voornamelijk betrekking hebben op apparaat-id's en metagegevens van de toepassing, die moet worden samengevoegd met de primaire IoT-hub en andere toepassingsspecifieke winkels in de primaire regio. Ter vereenvoudiging van deze stap, moet u idempotente bewerkingen. Idempotente bewerkingen Minimaliseer de neveneffecten van de uiteindelijke consistente distributie van gebeurtenissen en van duplicaten of out volgorde bezorging van gebeurtenissen. Bovendien moet de toepassingslogica worden ontworpen te tolereren mogelijke inconsistenties of 'enigszins' verouderde status. Deze situatie kan optreden vanwege de extra tijd die nodig is voor het systeem "herstel knooppuntservice" op basis van de doelstellingen voor herstelpunten (RPO).

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure IoT Hub:

* [Aan de slag met IoT-Hubs (zelfstudie)][lnk-get-started]
* [Wat is Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[What is Azure IoT Hub?]: about-iot-hub.md
