---
title: Wat is er dubbele OPC - Azure | Microsoft Docs
description: Overzicht van de OPC-Twin
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 9daf1a7e58af23cb78705691217bf9709359c4d5
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496808"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-device-management"></a>Wat is Azure IoT Open Platform-communicatie (OPC)-Apparaatbeheer?

OPC-Twin bestaat uit microservices die Azure IoT Edge en IoT-Hub gebruiken om de cloud en het netwerk factory. OPC-Twin voorziet in detectie, registratie en beheer op afstand van industriële apparaten via REST API's. Een OPC Unified-architectuur (OPC UA) SDK, taalneutraal is programmeren en kan worden opgenomen in een serverloze werkstroom vereist OPC dubbele niet. Dit artikel beschrijft de verschillende OPC-dubbele van use cases.

## <a name="discovery-and-control"></a>Detectie en beheer
U kunt OPC dubbele gebruiken voor eenvoudige voor detectie en registratie.

### <a name="simple-discovery-and-registration"></a>Eenvoudige detectie en registratie
OPC-Twin kunt factory operators om te scannen op het netwerk factory zodat OPC UA-servers kunnen worden gedetecteerd en geregistreerd. Als alternatief kunnen factory operators OPC UA-apparaten met behulp van een bekende detectie-URL ook handmatig registreren. Bijvoorbeeld, voor verbinding met alle apparaten van de OPC UA nadat de IoT Edge-gateway met een dubbele OPC-module is geïnstalleerd op de werkvloer, kan de operator factory op afstand activeren een scan van het netwerk en visueel Zie alle OPC UA-servers. 

### <a name="simple-control"></a>Eenvoudig beheer
OPC-Twin kunt factory operators om te reageren op gebeurtenissen en hun factory floor machines vanuit de cloud automatisch of handmatig op elk gewenst moment te configureren. OPC-Twin biedt REST-API's voor het aanroepen van services op de OPC UA-server, bladert u ook de adresruimte te lezen/schrijven variabelen en methoden uitvoeren. Een cv gebruikt bijvoorbeeld de temperatuur KPI voor het beheren van de productielijn. De temperatuursensor publiceert de wijziging in de gegevens met behulp van OPC Publisher. De operator factory ontvangt de waarschuwing dat de temperatuur van de drempelwaarde is bereikt. De productielijn koelt omlaag automatisch via OPC dubbel. De operator factory is een melding van de coolbar omlaag.

## <a name="authentication"></a>Authentication
U kunt OPC-Twin gebruiken voor eenvoudige voor verificatie en voor een eenvoudige ontwikkelaarservaring.

### <a name="simple-authentication"></a>Eenvoudige authenticatie 
OPC-Twin maakt gebruik van Azure Active Directory (AAD) gebaseerde verificatie- en controleniveaus end tot einde. OPC-Twin kan bijvoorbeeld de toepassing worden gebouwd op de OPC-Twin om te bepalen wat een operator op een virtuele machine is uitgevoerd. Het is aan de machine via OPC UA-controle. Het is aan de cloud via het opslaan van een controlelogboek onveranderbare client en de AAD-verificatie op de REST-API.

### <a name="simple-developer-experience"></a>Eenvoudige ontwikkelervaring 
OPC-Twin kan worden gebruikt met toepassingen die zijn geschreven in elke programmeertaal via REST API's. Als ontwikkelaars een OPC UA-client in een oplossing integreren, is kennis van de OPC UA-SDK niet nodig. OPC-Twin kunt naadloos integreren in stateless, serverloze architecturen. Bijvoorbeeld, een volledige stack webontwikkelaar bent die een toepassing ontwikkelt voor een waarschuwing en gebeurtenis-dashboard de logica om te reageren op gebeurtenissen in JavaScript- of TypeScript met OPC-Twin zonder de kennis van C, kunt schrijven C#, of de volledige implementatie van de OPC UA-stack. 

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over OPC dubbel en zijn gebruikt, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Wat is OPC Vault?](overview-opc-twin-architecture.md)