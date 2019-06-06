---
title: Aanvraag verwerken en e-mailmeldingen in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Meer informatie over het proces voor aanvragen voor een pakket toegang en het e-mailmeldingen worden verzonden in beheer van de rechten van een Azure Active Directory (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: aede5e315141251026867f7028ebf989d44da4d5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473053"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Aanvraag verwerken en e-mailmeldingen in beheer van de rechten van een Azure AD (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Wanneer een gebruiker een aanvraag aan een pakket toegang verzendt, wordt een proces gestart om te leveren die aanvraag. Beheer van de rechten van een Azure AD verzendt ook e-mailmeldingen naar goedkeurders en aanvragers wanneer belangrijke gebeurtenissen tijdens het proces optreden.

In dit artikel beschrijft het proces voor aanvragen en de e-mailmeldingen die worden verzonden.

## <a name="request-process"></a>Proces voor aanvragen

Een gebruiker die toegang tot een pakket toegang nodig heeft, kunt een aanvraag voor toegang indienen. Afhankelijk van de configuratie van het beleid, kan de aanvraag een goedkeuring vereisen. Wanneer een aanvraag is goedgekeurd, wordt een proces begint met de gebruikerstoegang toewijzen aan elke resource in het pakket toegang. Het volgende diagram toont een overzicht van het proces en de verschillende statussen.

![Procesdiagram van goedkeuring](./media/entitlement-management-process/request-process.png)

| Status | Description |
| --- | --- |
| Verzonden | Gebruiker indient een aanvraag. |
| Wachten op goedkeuring | Als het beleid voor een pakket toegang moet worden goedgekeurd, een aanvraag wordt verplaatst naar in afwachting van goedkeuring. |
| Vervallen | Als u geen fiatteurs een aanvraag binnen de time-out van de aanvraag voor goedkeuring goedkeurt, wordt de aanvraag is verlopen. Probeer het opnieuw heeft de gebruiker de aanvraag opnieuw indienen. |
| Geweigerd | Goedkeurder een aanvraag wordt geweigerd. |
| Goedgekeurd | Goedkeurder goedkeurt een aanvraag. |
| Leveren | Gebruiker heeft **niet** is toegang tot alle resources in het pakket met toegang tot toegewezen. Als dit een externe gebruiker, is nog niet de gebruiker toegang krijgen tot de resource-map en de prompt machtigingen geaccepteerd. |
| Afgeleverd | Gebruiker is toegang toegewezen aan alle resources in het pakket toegang. |
| Uitgebreide toegang | Als de extensies zijn toegestaan in het beleid, wordt door de gebruiker de toewijzing uitgebreid. |
| Toegang is verlopen | Van de gebruiker toegang tot het pakket toegang is verlopen. Om toegang te krijgen, heeft de gebruiker een aanvraag indienen. |

## <a name="email-notifications"></a>E-mailmeldingen

Als u een goedkeurder, kunt u e-mailmeldingen worden verzonden wanneer u wilt goedkeuren van een aanvraag voor toegang tot en wanneer een aanvraag voor toegang tot is voltooid. Als u een aanvrager bent, kunt u e-mailmeldingen die wijzen op de status van uw aanvraag worden verzonden. Het volgende diagram laat zien wanneer deze e-mailmeldingen worden verzonden.

![E-proces voor het beheren van rechten](./media/entitlement-management-process/email-notifications.png)

De volgende tabel vindt u meer informatie over elk van deze e-mailmeldingen.

| # | E-mailonderwerp | Wanneer verzonden | Verzonden naar |
| --- | --- | --- | --- |
| 1 | Actie vereist: Toegang tot aanvraag beoordelen van *[aanvrager]* naar *[package toegang]* door *[datum]* | Wanneer een aanvrager verzendt een verzoek voor een pakket toegang | Alle goedkeurders |
| 2 | Actie vereist: Toegang tot aanvraag beoordelen van *[aanvrager]* naar *[package toegang]* door *[datum]* | Time-out voor de X dagen voor de goedkeuring van de aanvraag | Alle goedkeurders |
| 3 | Statusmelding: *[aanvrager]* van toegangsaanvraag voor *[package toegang]* is verlopen | Wanneer de goedkeurders niet goedkeuren of weigeren van een aanvraag voor toegang tot binnen de duur van de aanvraag | Aanvrager |
| 4 | Statusmelding: *[aanvrager]* toegangsaanvraag voor *[package toegang]* is voltooid | Wanneer de eerste fiatteur goedkeurt of weigert een aanvraag voor toegang tot | Alle goedkeurders |
| 5 | U kunt geen toegang tot *[package toegang]* | Wanneer een aanvrager toegang tot het pakket toegang is geweigerd | Aanvrager |
| 6 | U hebt nu toegang tot *[package toegang]*  | Wanneer een aanvrager toegang tot elke resource in het pakket toegang is verleend | Aanvrager |
| 7 | Uw toegang tot *[package toegang]* verloopt over X dag(en) | X dagen voor de toegang van de aanvrager aan het pakket toegang is verlopen | Aanvrager |
| 8 | Uw toegang tot *[package toegang]* is verlopen | Wanneer de toegang van de aanvrager aan een pakket toegang is verlopen | Aanvrager |

### <a name="access-request-emails"></a>Toegang tot aanvraag e-mailberichten

Wanneer een aanvrager een verzoek om een access-pakket dat is geconfigureerd verzendt voor goedkeuring vereisen voor toegang tot, ontvangen alle goedkeurders die zijn geconfigureerd in het beleid voor een e-mailbericht met details van de aanvraag. Details van de aanvrager de naam, organisatie, toegang tot de begin- en datum indien opgegeven, zakelijke reden, wanneer de aanvraag is ingediend en verloopt de aanvraag. Het e-mailbericht bevat een koppeling waar goedkeurders kunnen goedkeuren of weigeren van de aanvraag. Hier volgt een voorbeeld e-mailmelding die aan een goedkeurder wordt verzonden wanneer een aanvrager een aanvraag voor toegang verstuurt.

![Een e-mail met toegang controleren](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>E-mailberichten goedgekeurd of geweigerd

Aanvragers worden op de hoogte gesteld als hun toegang tot de aanvraag is goedgekeurd en beschikbaar voor toegang, of wanneer de toegangsaanvraag voor is geweigerd. Wanneer een goedkeurder een aanvraag voor toegang tot verzonden door een aanvrager ontvangt, kunnen ze goedkeuren of afwijzen van de aanvraag. De fiatteur moet een zakelijke rechtvaardiging voor hun besluit toevoegen.

Wanneer een aanvraag voor toegang tot is goedgekeurd, begint management waar u recht op het proces van de aanvrager toegang tot elk van de resources in het pakket toegang verlenen. Nadat de aanvrager heeft toegang tot elke resource in het pakket toegang is verleend, wordt een e-mailmelding verzonden naar de aanvrager dat de access-aanvraag is goedgekeurd en of ze nu toegang tot het pakket toegang hebben. Hier volgt een voorbeeld e-mailbericht wordt verzonden naar een aanvrager wanneer ze toegang tot een package toegang worden verleend.

Wanneer een aanvraag voor toegang is geweigerd, wordt een e-mailmelding verzonden naar de aanvrager. Hier volgt een voorbeeld e-mailbericht wordt verzonden naar een aanvrager als hun toegang tot de aanvraag is geweigerd.

### <a name="expired-access-request-emails"></a>Toegang tot aanvraag e-mailberichten verlopen

Aanvragers worden gewaarschuwd wanneer de toegangsaanvraag is verlopen. Wanneer een aanvrager een aanvraag voor toegang tot verstuurt, heeft de aanvraag de aanvraagduur van een daarna verloopt. Als er geen goedkeurders een beslissing goedkeuren/weigeren verzenden, blijft de aanvraag moet worden bewaard in een status in afwachting van goedkeuring. Wanneer de aanvraag de vervaltijd van de geconfigureerde bereikt, kan de aanvraag is verlopen, en niet meer worden goedgekeurd of geweigerd door de goedkeurders. In dit geval wordt de aanvraag wordt verwerkt in een verlopen staat. Een verlopen aanvraag kan niet meer worden goedgekeurd of geweigerd. Een e-mailmelding wordt verzonden naar de aanvrager dat de aanvraag voor toegang tot is verlopen, en die ze nodig hebben om de aanvraag opnieuw indienen. Hier volgt een voorbeeld e-mailbericht wordt verzonden naar een aanvrager wanneer de toegangsaanvraag is verlopen.

![Toegang tot aanvraag e-mail over verlopen](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>Volgende stappen

- [Aanvraag voor toegang tot een package toegang](entitlement-management-request-access.md)
- [Goedkeuren of weigeren van toegangsaanvragen](entitlement-management-request-approve.md)
