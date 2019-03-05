---
title: Gegevens van de klant aanvragen functies in Azure IoT Central | Microsoft Docs
description: Gegevens van de klant functies in Azure IoT Central aanvragen
author: dominicbetts
ms.author: dobett
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 2952008ca788a620f2b558d5997aeebd59196b7a
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314565"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag

Azure IoT Central is een volledig beheerde oplossing uit Internet of Things (IoT)-software-as-a-service waarmee u eenvoudig verbinding maken, bewaken, en uw IoT-apparaten op schaal beheren, diepe inzichten uit uw IoT-gegevens maken en onderneem actie.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

Azure Active Directory Object-id's worden gebruikt voor het identificeren van gebruikers en rollen toewijzen. Azure IoT Central portal geeft gebruiker e-mailadressen van roltoewijzingen, maar alleen de Azure Active Directory-Object-ID is opgeslagen, het e-mailadres is dynamisch opgevraagd uit Azure Active Directory. Azure IoT Central-beheerders kunnen weergeven, exporteren en verwijderen van gebruikers van de toepassing in de sectie voor het beheer van gebruiker van een Azure IoT Central-toepassing.

In de toepassing, kunnen de e-mailadressen worden geconfigureerd om waarschuwingen te ontvangen. In dit geval e-mailadressen worden opgeslagen in IoT Central en moeten worden beheerd vanaf de pagina account in het app-beheer.

Met betrekking tot apparaten, Microsoft geen gegevens worden bijgehouden en heeft geen toegang tot gegevens die kan apparaat gebruiker correlatie. Veel van de apparaten die worden beheerd in Azure IoT Central zijn niet persoonlijke apparaten, bijvoorbeeld een Verkoopautomaat of koffie maker. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en bij naar eigen goeddunken hun eigen asset of inventaris bijhouden van systemen die verbinden van apparaten naar personen kunnen onderhouden. Azure IoT Central beheert en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Wanneer u Microsoft enterprise-services gebruikt, genereert Microsoft bepaalde informatie bekend als het systeem gegenereerde logboeken. Deze logboeken vormen feitelijke acties die zijn uitgevoerd binnen de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten en niet zijn gerelateerd aan gebruikersactiviteit. Azure IoT Central het systeem gegenereerde logboeken zijn niet toegankelijk of door toepassingsbeheerders van de kan worden geëxporteerd.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

De mogelijkheid om gebruikersgegevens te verwijderen is alleen beschikbaar via de pagina met IoT Central administration. Beheerders voor de gebruiker om te worden verwijderd en selecteer kunnen selecteren **verwijderen** in de rechterbovenhoek van de toepassing om de record te verwijderen. Toepassingsbeheerders kunnen ook afzonderlijke accounts die niet langer gekoppeld aan de betreffende toepassing verwijderen.

Nadat een gebruiker is verwijderd, worden er geen verdere waarschuwingen verzonden naar deze. Het e-mailadres moet echter afzonderlijk verwijderd uit elke geconfigureerde waarschuwing.

Zie voor meer informatie, [regels en acties voor uw apparaat configureren](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

De mogelijkheid om gegevens te exporteren, is alleen beschikbaar via de pagina met IoT Central administration. Gegevens van de klant, inclusief toegewezen rollen, kan worden geselecteerd, gekopieerd en geplakt door een beheerder van de toepassing.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Zie voor meer informatie over het accountbeheer van, met inbegrip van roldefinities, [beheren van uw toepassing](howto-administer.md).
