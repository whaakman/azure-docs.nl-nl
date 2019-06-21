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
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176248"
---
## <a name="deleting-personal-information"></a>Verwijderen van persoonlijke gegevens

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

Persoonlijke informatie is relevant zijn voor de import/export-service (via de portal en API) tijdens het importeren en exporteren van bewerkingen. Gegevens die worden gebruikt tijdens deze processen zijn onder andere:

- Naam van contactpersoon
- Telefoonnummer
- Email
- Adres
- Plaats
- Postcode
- Status
- Land/Provincie/Regio
- Stations-ID
- Accountnummer van transporteur
- Volgnummer van verzending

Wanneer een import-/ exporttaak is gemaakt, bieden gebruikers de contactgegevens en een verzendadres. Persoonlijke gegevens worden opgeslagen in twee verschillende locaties: in de taak en (optioneel) in de portalinstellingen. Persoonlijke gegevens worden alleen opgeslagen in de portalinstellingen als u het selectievakje met het label, **vervoerder en keer terug adres opslaan als standaard** tijdens de *verzendinfo retourneren* sectie van het exportproces begeleidt.

Persoonlijke gegevens kan worden verwijderd in de volgende manieren:

- Gegevens die worden opgeslagen met de taak is in combinatie met de taak verwijderd. Gebruikers kunnen taken handmatig verwijderen en voltooide taken worden na 90 dagen automatisch verwijderd. De taken via de REST-API of de Azure-portal kunt u handmatig verwijderen. Als u wilt verwijderen van de taak in Azure portal, gaat u naar uw import-/ exporttaak en klikt u op *verwijderen* vanuit de opdrachtbalk. Raadpleeg voor meer informatie over het verwijderen van een taak voor importeren/exporteren via de REST API [een import-/ exporttaak verwijderen](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- Neem contact op met gegevens die zijn opgeslagen in de portal-instellingen kan worden verwijderd door het verwijderen van de portalinstellingen. Portal-instellingen kunt u verwijderen door de volgende stappen:
  - Meld u aan bij [Azure Portal](https://portal.azure.com).
  - Klik op de *instellingen* pictogram ![pictogram voor Azure-instellingen](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Klik op *alle instellingen exporteren* (om op te slaan, uw huidige instellingen een `.json` bestand).
  - Klik op *verwijdert u alle instellingen en privédashboards* om alle instellingen met inbegrip van opgeslagen gegevens te verwijderen.

Voor meer informatie raadpleegt u het beleid van Microsoft Privacy [Vertrouwenscentrum](https://www.microsoft.com/trustcenter)