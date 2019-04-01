---
title: Wat is Azure IoT OPC UA-Certificaatbeheer | Microsoft Docs
description: Overzicht van de OPC-kluis
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ba3313d927ec4317d6c051f6058d75a415b92288
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759264"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-ua-certificate-management"></a>Wat is Azure IoT Open Platform-communicatie (OPC) UA Certificaatbeheer?

Azure IoT OPC UA Certificaatbeheer, ook weten als OPC-kluis is een microservice die configureren kunt, registreren, en de levenscyclus van het certificaat voor de OPC UA-server en client-toepassingen in de cloud beheren. Dit artikel beschrijft de eenvoudige gebruiksvoorbeelden van de OPC-kluis.

## <a name="certificate-management"></a>Certificaatbeheer

Bijvoorbeeld, moet een productiebedrijf verbinding maken met de OPC UA-server-machine naar de zojuist gemaakte clienttoepassing. Wanneer de fabrikant van de eerste toegang tot de server-machine maakt, wordt onmiddellijk een foutbericht weergegeven op de OPC UA-server-toepassing om aan te geven dat de clienttoepassing niet beveiligd is. Dit mechanisme is ingebouwd in de OPC UA-server-machine om te voorkomen dat een niet-geautoriseerde toegang tot toepassingen, waarmee wordt voorkomen dat gemene hacken op de werkvloer.

## <a name="application-security-management"></a>Beveiliging van Toepassingsbeheer
Een professionele beveiliging maakt gebruik van OPC kluis microservice OPC UA-server om te communiceren met elke clienttoepassing eenvoudig inschakelen omdat OPC kluis alle functies voor het register van de certificaat-, opslag- en levenscyclusbeheer heeft. OPC UA-server is nu veilig verbonden, kan het communiceren met de zojuist gemaakte clienttoepassing

## <a name="the-complete-opc-vault-architecture"></a>De volledige OPC-kluis-architectuur
Het volgende diagram illustreert de volledige OPC-kluis-architectuur.

![Architectuur van de OPC-kluis](media/overview-opc-vault-architecture/opc-vault.png)