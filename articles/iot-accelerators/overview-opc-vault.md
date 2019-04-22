---
title: Wat is er OPC-kluis - Azure | Microsoft Docs
description: Overzicht van de OPC-kluis
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 99dfcaeb1ef5b52e6827f1b3ac65d6201557a8fb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783514"
---
# <a name="what-is-opc-vault"></a>Wat is de OPC-kluis?

OPC-kluis is een microservice die u kunt configureren, te registreren en beheren van de levenscyclus van het certificaat voor OPC UA-server en clienttoepassingen in de cloud. Dit artikel beschrijft de eenvoudige gebruiksvoorbeelden van de OPC-kluis.

## <a name="certificate-management"></a>Certificaatbeheer

Bijvoorbeeld, moet een productiebedrijf verbinding maken met de OPC UA-server-machine naar de zojuist gemaakte clienttoepassing. Wanneer de fabrikant van de eerste toegang tot de server-machine maakt, wordt onmiddellijk een foutbericht weergegeven op de OPC UA-server-toepassing om aan te geven dat de clienttoepassing niet beveiligd is. Dit mechanisme is ingebouwd in de OPC UA-server-machine om te voorkomen dat een niet-geautoriseerde toegang tot toepassingen, waarmee wordt voorkomen dat gemene hacken op de werkvloer.

## <a name="application-security-management"></a>Beveiliging van Toepassingsbeheer
Een professionele beveiliging maakt gebruik van OPC kluis microservice OPC UA-server om te communiceren met elke clienttoepassing eenvoudig inschakelen omdat OPC kluis alle functies voor het register van de certificaat-, opslag- en levenscyclusbeheer heeft. OPC UA-server is nu veilig verbonden, kan het communiceren met de zojuist gemaakte clienttoepassing

## <a name="the-complete-opc-vault-architecture"></a>De volledige OPC-kluis-architectuur
Het volgende diagram illustreert de volledige OPC-kluis-architectuur.

![Architectuur van de OPC-kluis](media/overview-opc-vault-architecture/opc-vault.png)