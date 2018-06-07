---
title: Klantgegevens aanvragen functies in Azure IoT centrale | Microsoft Docs
description: Klantgegevens functies in Azure IoT centrale aanvragen
author: dominicbetts
ms.author: dobett
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: bb5e263b0332f957b4e7f4a928ccd53f639bcd9c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629402"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant aanvraag gegevensfuncties

Azure IoT centraal is een volledig beheerde software as a service oplossing voor Internet der dingen (IoT) waarmee u gemakkelijk verbinding maken, bewaken, en beheren van uw IoT-assets op grote schaal veel inzicht in uw IoT-gegevens maken en maatregelen nemen op de hoogte.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificeren van klantgegevens

Azure Active Directory Object-id's worden gebruikt voor het identificeren van gebruikers en rollen toewijst. De Azure IoT centrale portal geeft gebruikers e-mailadressen voor roltoewijzingen, maar alleen de Azure Active Directory-Object-ID zijn opgeslagen, het e-mailadres wordt dynamisch opgevraagd van Azure Active Directory. Beheerders van Azure IoT centraal kunnen weergeven, exporteren en verwijderen van gebruikers van de toepassing in het gedeelte voor het beheer van gebruiker van een Azure IoT centrale toepassing.

In de toepassing kunnen de e-mailadressen worden geconfigureerd om waarschuwingen te ontvangen. In dit geval e-mailadressen binnen IoT centraal worden opgeslagen en moeten worden beheerd vanaf de pagina van de account in-app-beheer.

Met betrekking tot de apparaten, Microsoft houdt geen informatie en heeft geen toegang tot gegevens waarmee de gebruiker correlatie van het apparaat. Veel van de apparaten die worden beheerd in Azure IoT centrale zijn niet persoonlijke apparaten, bijvoorbeeld een snoep-machine of de maker koffie. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en goeddunken van hun eigen asset of Voorraadbeheer systemen die apparaten aan personen koppelen kunnen handhaven. Azure IoT centraal beheert en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Wanneer u gebruikmaakt van Microsoft enterprise-services, genereert Microsoft bepaalde informatie, bekend als het systeem gegenereerde logboeken. Deze logboeken vormen feitelijke acties die deel uitmaken van de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten en niet zijn gerelateerd aan gebruikersactiviteit. Azure IoT centraal systeem gegenereerde logboeken zijn niet toegankelijk of exporteerbaar door toepassingsbeheerders.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

De mogelijkheid om gebruikersgegevens te verwijderen is alleen beschikbaar via de pagina IoT Centraal beheer. Toepassingsbeheerders kunnen de gebruiker worden verwijderd en klik op selecteren **verwijderen** in de rechterbovenhoek van de toepassing de record verwijderen. Toepassingsbeheerders kunnen ook afzonderlijke accounts die niet meer gekoppeld aan de desbetreffende toepassing verwijderen.

Nadat een gebruiker wordt verwijderd, worden er geen verdere waarschuwingen verzonden toe. Het e-mailadres moet echter afzonderlijk verwijderd van elke geconfigureerde waarschuwing.

Zie voor meer informatie [regels en acties voor uw apparaat configureren](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Exporteren van klantgegevens

De mogelijkheid om gegevens te exporteren is alleen beschikbaar via de pagina IoT Centraal beheer. Klantgegevens, inclusief toegewezen rollen kan worden geselecteerd, gekopieerd en geplakt door de toepassingsbeheerder.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Zie voor meer informatie over het beheer van de account, inclusief roldefinities, [het beheren van uw toepassing](howto-administer.md).
