---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 3d85208096b1300904e1cc1f59c45dd4dd5713a3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34684418"
---
## <a name="deleting-personal-information"></a>Verwijderen van persoonlijke informatie

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

Persoonlijke gegevens relevant is voor het importeren/exporteren (via de portal en API)-service tijdens het importeren en exporteren van bewerkingen. Gegevens die worden gebruikt tijdens deze processen zijn onder andere:

- Naam van contactpersoon
- Telefoonnummer
- Email
- Adres
- Plaats
- Postcode
- Status
- Provincie-land/regio
- Stations-ID
- Het nummer van Carrier
- Back-upfunctie volgnummer

Wanneer een taak van import/export is gemaakt, bieden gebruikers de contactgegevens en een verzendadres. Persoonlijke informatie wordt opgeslagen in twee verschillende locaties: de taak en optioneel in de portalinstellingen. Persoonlijke gegevens alleen wordt opgeslagen in de portal-instellingen als u het selectievakje met het label, **carrier en keer terug adres opslaan als standaard** tijdens de *back-ups van gegevens retourneren* sectie van het exportproces.

Persoonlijke gegevens kan worden verwijderd op de volgende manieren:

- Gegevens die zijn opgeslagen met de taak is verwijderd met de taak. Gebruikers kunnen taken handmatig verwijderen en voltooide taken worden na 90 dagen automatisch verwijderd. De taken via de REST-API of de Azure-portal kunt u handmatig verwijderen. Verwijder de taak in de Azure portal, gaat u naar uw taak van import/export, te klikken en *verwijderen* uit de opdrachtbalk. Raadpleeg voor meer informatie over het verwijderen van een taak voor importeren/exporteren via de REST-API [verwijderen van een taak van import/export](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- Neem contact op met gegevens die zijn opgeslagen in de portal-instellingen kunnen worden verwijderd door het verwijderen van de portalinstellingen. U kunt de portal-instellingen verwijderen met de volgende stappen:
  - Meld u aan bij [Azure Portal](https://portal.azure.com).
  - Klik op de *instellingen* pictogram ![Instellingenpictogram Azure](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Klik op *alle instellingen exporteren* (opslaan van uw huidige instellingen een `.json` bestand).
  - Klik op *verwijdert u alle instellingen en persoonlijke dashboards* om alle instellingen die met opgeslagen contactgegevens te verwijderen.

Raadpleeg voor meer informatie, het beleid voor Microsoft Privacy [Vertrouwenscentrum](https://www.microsoft.com/trustcenter)