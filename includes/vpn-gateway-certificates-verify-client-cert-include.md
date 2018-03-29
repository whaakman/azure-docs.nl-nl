---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9fa18b14b82376a25bb434acd770d340b1ef9262
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
Als u problemen hebt met de verbinding, controleer dan het volgende:

- Als u een clientcertificaat hebt geëxporteerd, moet u ervoor zorgen dat u dit als een PFX-bestand exporteert met behulp van deze standaardwaarde: Indien mogelijk alle certificaten in het certificeringspad opnemen. Als u het certificaat met behulp van deze waarde exporteert, worden de gegevens van het basiscertificaat ook geëxporteerd. Wanneer het certificaat is geïnstalleerd op de clientcomputer, wordt het basiscertificaat dat is opgenomen in het PFX-bestand, vervolgens ook geïnstalleerd op de clientcomputer. De gegevens van het basiscertificaat moeten zijn geïnstalleerd op de clientcomputer. Ga naar **Gebruikerscertificaten beheren** en navigeer naar **Vertrouwde basiscertificeringsinstanties\Certificaten** om dit te controleren. Controleer of het basiscertificaat wordt weergegeven. Verificatie werkt alleen als het basiscertificaat aanwezig is.

- Als u een certificaat gebruikt dat is verleend met behulp van een oplossing voor een CA voor ondernemingen en problemen ondervindt bij de verificatie, controleert u de verificatievolgorde op het clientcertificaat. U kunt de volgorde van de verificatielijst controleren door op het clientcertificaat te dubbelklikken en naar **Details > Uitgebreid sleutelgebruik** te gaan. Controleer of Clientverificatie als het eerste item in de lijst wordt weergegeven. Als dat niet het geval is, moet u een clientcertificaat op basis van de gebruikerssjabloon verlenen waarin clientverificatie als het eerste item in de lijst wordt weergegeven.

- Zie [Troubleshoot P2S connections](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) (Problemen met P2S-verbindingen oplossen) voor extra informatie over het oplossen van problemen met P2S,.