---
title: Azure IoT Hub hoge beschikbaarheid en herstel na noodgevallen | Microsoft Docs
description: Beschrijft de functies voor Azure en IoT-Hub die u helpen bij het bouwen van maximaal beschikbare Azure IoT-oplossingen met na noodgevallen herstelfuncties.
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.openlocfilehash: 3ea10ee8652dc2a03791feb66041431e7b3c6ae1
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hoge beschikbaarheid en herstel na noodgevallen
Een Azure-service biedt IoT Hub redundantie op het niveau van de Azure-regio, zonder extra werk vereist voor de oplossing met hoge beschikbaarheid (HA). Het Microsoft Azure-platform omvat ook functies waarmee u oplossingen bouwen met herstelfuncties van noodherstel (DR) of de regio-overschrijdende beschikbaarheid. Als u wilt bieden globale, hoge beschikbaarheid van de regio-overschrijdende voor apparaten of gebruikers profiteren van deze Azure DR-functies. Het artikel [Azure zakelijke continuïteit technische richtlijnen](../resiliency/resiliency-technical-guidance.md) beschrijving van de ingebouwde functies in Azure voor bedrijfscontinuïteit en Noodherstel. De [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen] [ Disaster recovery and high availability for Azure applications] papier biedt architectuurrichtlijnen op strategieën voor het Azure-toepassingen om HA en Noodherstel te bereiken.

## <a name="azure-iot-hub-dr"></a>Azure IoT-Hub DR
IoT Hub implementeert naast intra-regio HA, failover-mechanismen voor herstel na noodgevallen waarvoor geen tussenkomst van de gebruiker. IoT Hub DR automatisch wordt gestart en heeft een beoogde hersteltijd (RTO) van 2-26 uur en de volgende herstelpuntdoelen (RPO's):

| Functionaliteit | RPO |
| --- | --- |
| Beschikbaarheid van de service voor register- en bewerkingen |Mogelijk CName verlies |
| Identiteitsgegevens in register-id 's |0-5 minuten gegevensverlies |
| Apparaat-naar-cloud-berichten |Alle ongelezen berichten gaan verloren |
| Bewerkingen berichten controleren |Alle ongelezen berichten gaan verloren |
| Cloud-naar-apparaat-berichten |0-5 minuten gegevensverlies |
| Cloud-naar-apparaat feedbackwachtrij |Alle ongelezen berichten gaan verloren |

## <a name="regional-failover-with-iot-hub"></a>Regionale failover met IoT Hub
Een volledige behandeling van implementatietopologieën in IoT-oplossingen is buiten het bereik van dit artikel. Het artikel behandelt de *regionale failover* implementatiemodel omwille van de hoge beschikbaarheid en herstel na noodgevallen.

De oplossing in een model regionale failover back-end uitgevoerd voornamelijk in een datacenter-locatie. Een secundaire IoT-hub en de back-end zijn geïmplementeerd in een andere locatie van het datacenter. Als de IoT-hub in het primaire datacenter te lijden heeft onder een storing of de netwerkverbinding van het apparaat met het primaire datacenter wordt onderbroken, gebruikt u apparaten een secundaire service-eindpunt. U kunt de beschikbaarheid van de oplossing verbeteren door het implementeren van een model regio-overschrijdende failover in plaats van binnen één regio blijft. 

Op een hoog niveau voor het implementeren van een model regionale failover met IoT Hub, moet u het volgende:

* **Een secundaire iothub en routering logica apparaat**: als de service in uw primaire regio wordt onderbroken, apparaten verbinding maken met uw secundaire regio moeten beginnen. Gezien de statusbewust aard van de meeste services die zijn betrokken, is het gebruikelijk dat beheerders van de oplossing voor het activeren van het failoverproces tussen regio. De beste manier om het nieuwe eindpunt naar apparaten, terwijl controle van het proces wordt behouden communiceren is dat ze regelmatig controleren op een *concierge* service voor het huidige actieve eindpunt. De service concierge kan een webtoepassing die is gerepliceerd en bereikbaar bewaard zijn met behulp van DNS-omleiding technieken (bijvoorbeeld [Azure Traffic Manager][Azure Traffic Manager]).
* **Identiteit register repliceren**: om te worden gebruikt, de secundaire IoT-hub alle apparaat-id's die verbinding met de oplossing maken kunnen moet bevatten. De oplossing moet back-ups geogerepliceerde van apparaat-id's houden en te uploaden naar de secundaire IoT-hub voordat u overschakelt van de actieve eindpunt voor de apparaten. De apparaat-id exportfunctie van IoT Hub is handig in deze context. Zie voor meer informatie [IoT Hub developer guide - id-register][IoT Hub developer guide - identity registry].
* **Samenvoegen van logica**: wanneer de primaire regio weer beschikbaar is, alle de status en gegevens die zijn gemaakt in de secundaire site moeten worden gemigreerd terug naar de primaire regio. Deze status en gegevens, vooral betrekking hebben op apparaat-id's en metagegevens van toepassing moet worden samengevoegd met de primaire IoT-hub en andere toepassingsspecifieke winkels in de primaire regio. Om te vereenvoudigen in deze stap, moet u de idempotent-bewerkingen. De Idempotent operations minimaliseert de neveneffecten van de uiteindelijke consistente distributie van gebeurtenissen en van duplicaten of out-van-levering van gebeurtenissen. Bovendien moet de toepassingslogica worden ontworpen voor mogelijke inconsistenties of 'enigszins' verouderde status tolereren. Deze situatie kan optreden vanwege de extra tijd die nodig is voor het systeem 'retoucheren' op basis van herstelpuntdoelen (RPO).

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure IoT Hub:

* [Aan de slag met IoT Hubs (zelfstudie)][lnk-get-started]
* [Wat is Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
