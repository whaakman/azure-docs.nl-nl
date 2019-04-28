---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 0068bd151c3d7d243b05c326ec73a201f4131296
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679948"
---
Als u een P2S-verbinding wilt maken vanaf een andere clientcomputer dan de computer die u gebruikt om de clientcertificaten te genereren, moet u een clientcertificaat installeren. Wanneer u een clientcertificaat installeert, hebt u het wachtwoord nodig dat is gemaakt tijdens het exporteren van het clientcertificaat.

1. Zoek het *PFX*-bestand en kopieer het naar de clientcomputer. Dubbelklik op de clientcomputer op het *PFX*-bestand om het te installeren. Laat de waarde voor **Archieflocatie** staan op **Huidige gebruiker** en klik op **Volgende**.
2. Laat de pagina **Te exporteren bestand** ongewijzigd. Klik op **volgende**.
3. Op de **beveiliging met persoonlijke sleutel** pagina, het wachtwoord voor het certificaat of Controleer of de beveiligingsprincipal juist is en klik vervolgens op **volgende**.
4. Laat op de pagina **Certificaatarchief** de standaardlocatie staan en klik op **Volgende**.
5. Klik op **Voltooien**. Klik in het venster **Beveiligingswaarschuwing** voor de certificaatinstallatie op **Ja**. U kunt zonder problemen op Ja klikken omdat u zelf het certificaat hebt gegenereerd. Het certificaat wordt nu geïmporteerd.