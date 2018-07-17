---
title: Inleiding tot Azure IoT Hub | Microsoft Docs
description: Lees hier alles over Azure IoT Hub. Deze IoT-service is gebouwd voor schaalbare gegevensopname, apparaatbeheer en beveiliging.
author: nberdy
ms.author: nberdy
ms.date: 07/04/2018
ms.topic: overview
ms.custom: mvc
ms.service: iot-hub
services: iot-hub
manager: briz
ms.openlocfilehash: 0e54c48eb63c5b70626e72f83de6d3676710d4c4
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860153"
---
# <a name="what-is-azure-iot-hub"></a>Wat is Azure IoT Hub?

IoT Hub is een beheerde service die wordt gehost in de cloud en die fungeert als een centrale berichtenhub voor bidirectionele communicatie tussen uw IoT-toepassing en de apparaten die hiermee worden beheerd. Met Azure IoT Hub kunt u IoT-oplossingen bouwen met betrouwbare en veilige communicatie tussen miljoenen IoT-apparaten en een back-end die in de cloud wordt gehost. U kunt vrijwel elk apparaat verbinden met IoT Hub.

IoT Hub biedt ondersteuning voor communicatie van het apparaat naar de cloud en omgekeerd. IoT Hub ondersteunt meerdere berichtenpatronen, zoals telemetrie voor apparaat-naar-cloud, bestandsupload vanaf apparaten en request-reply-methoden voor het beheren van uw apparaten vanuit de cloud. Controle met behulp van IoT Hub helpt u bij het onderhouden van uw oplossing door het traceren van gebeurtenissen, zoals het maken van apparaten, apparaatstoringen en apparaatverbindingen.

De mogelijkheden van IoT Hub helpen u bij het bouwen van schaalbare, complete IoT-oplossingen, bijvoorbeeld voor het beheren van industriële apparatuur die worden gebruikt in productieomgevingen, het volgen van waardevolle items in de gezondheidszorg en het monitoren van het gebruik van kantoorgebouwen.

## <a name="scale-your-solution"></a>Uw oplossing schalen

IoT Hub kan worden opgeschaald naar miljoenen gelijktijdig verbonden apparaten en miljoenen gebeurtenissen per seconde om zo uw IoT-workloads te ondersteunen. IoT Hub biedt verschillende servicelagen om optimaal tegemoet te kunnen komen aan de schaalbaarheid die u nodig hebt. [Meer informatie](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="secure-your-communications"></a>Uw communicatie beveiligen

Met IoT Hub beschikt u over een beveiligd communicatiekanaal dat uw apparaten kunnen gebruiken om gegevens te verzenden.

* Verificatie per apparaat zorgt ervoor dat elk apparaat een veilige verbinding kan opzetten met IoT Hub en dat elk apparaat veilig kan worden beheerd.
* U hebt volledige controle over de toegang van apparaten en u kunt de verbindingen beheren op apparaatniveau.
* De [IoT Hub Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/) zorgt ervoor dat apparaten automatisch worden ingericht met de juiste IoT-hub wanneer het apparaat voor het eerst wordt opgestart.
* Verschillende verificatietypen ondersteunen allerlei mogelijkheden voor apparaten:
  * Verificatie aan de hand van een SAS-token betekent dat u snel aan de slag kunt met uw IoT-oplossing.
  * Verificatie via afzonderlijke X.509-certificaten zorgt voor een veilige, op standaarden gebaseerde verificatie.
  * X.509-CA-verificatie is beschikbaar voor eenvoudige, op standaarden gebaseerde inschrijving.

## <a name="route-device-data"></a>Apparaatgegevens routeren

De ingebouwde functionaliteit voor het routeren van berichten biedt u de flexibiliteit voor het instellen van automatische berichtendistributie op basis van regels:

* Gebruik berichtroutering om te bepalen naar welke locatie de hub telemetriegegevens van apparaten verstuurt.
* Er zijn geen extra kosten verbonden aan het routeren van berichten naar meerdere eindpunten.
* Routeringsregels zonder code hebben de plaats ingenomen van aangepaste code voor het verzenden van berichten.

## <a name="integrate-with-other-services"></a>Integreren met andere services

U kunt IoT Hub integreren met andere Azure-services om zo complete, end-to-end-oplossingen te bouwen. Enkele voorbeelden:

* Gebruik [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) om op een betrouwbare, veilige en schaalbare manier snel te reageren op kritieke gebeurtenissen.

* Gebruik [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) voor het automatiseren van bedrijfsprocessen.

* Gebruik [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/) om machine learning en AI-modellen toe te voegen aan uw oplossing.

* Gebruik [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) voor het in realtime uitvoeren van analytische berekeningen op de gegevens die door uw apparaten worden gestreamd.

## <a name="configure-and-control-your-devices"></a>Uw apparaten configureren en beheren

U hebt de beschikking over allerlei ingebouwde functies om de apparaten te beheren die zijn verbonden met IoT Hub.

* Metagegevens en statusinformatie van al uw apparaten opslaan, synchroniseren en opvragen.
* Apparaatstatus instellen per apparaat of op basis van algemene eigenschappen van apparaten.
* Automatisch reageren op een door het apparaat gerapporteerde statuswijziging met geïntegreerde berichtroutering.

## <a name="make-your-solution-highly-available"></a>Uw oplossing maximaal beschikbaar maken

Er geldt een [SLA (Service Level Agreement) voor IoT-Hub](https://azure.microsoft.com/support/legal/sla/iot-hub/) van 99,9%. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

## <a name="connect-your-devices"></a>Uw apparaten verbinden

Gebruik de bibliotheken uit de [apparaat-SDK van Azure IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) om toepassingen te bouwen die worden uitgevoerd op uw apparaten en die communiceren met IoT Hub. Voorbeelden van ondersteunde platforms zijn diverse Linux-distributies, Windows en realtime-besturingssystemen. Enkele ondersteunde talen:

* C
* C#
* Java
* Python
* Node.js.

IoT Hub en de apparaat-SDK's ondersteunen de volgende protocollen voor het verbinden van apparaten:

* HTTPS
* AMQP
* AMQP via WebSockets
* MQTT
* MQTT via WebSockets

Als uw oplossing de apparaatbibliotheken niet kan gebruiken, kunnen apparaten het protocol MQTT v3.1.1, HTTPS 1.1 of AMQP 1.0 gebruiken om op systeemeigen wijze verbinding te maken met uw hub.

Als uw oplossing geen van de ondersteunde protocollen kan gebruiken, kunt u IoT Hub uitbreiden voor ondersteuning van aangepaste protocollen:

* Gebruik [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) om een veldgateway te maken waarmee u aan de rand protocolomzetting kunt uitvoeren.
* Pas de [protocolgateway van Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) aan om protocolomzetting uit te voeren in de cloud.

## <a name="quotas-and-limits"></a>Quota en limieten

Voor elk Azure-abonnement gelden standaardquotalimieten ter voorkoming van misbruik van de service. Deze limieten kunnen invloed hebben op het bereik van uw IoT-oplossing. De huidige limiet voor een abonnementsvariant is 10 IoT-hubs per abonnement. U kunt een verzoek voor een groter quota indienen door contact op te nemen met de ondersteuning. Meer informatie over quotalimieten vindt u hier:

* [Limieten, quota en beperkingen van Azure-abonnementen en -services](../azure-subscription-service-limits.md)
* [IoT Hub throttling and you](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/)

## <a name="next-steps"></a>Volgende stappen

Bekijk de snelstartgidsen voor IoT Hub als u een end-to-end IoT-oplossing wilt uitproberen:

* [Snelstartgids: Telemetrie verzenden van een apparaat naar een IoT-hub](quickstart-send-telemetry-node.md)
