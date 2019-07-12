---
title: Wat is er OPC-kluis - Azure | Microsoft Docs
description: Overzicht van de OPC-kluis
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 40a9016ac7a10175b51f0fb6f072dd089bde3a51
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606281"
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