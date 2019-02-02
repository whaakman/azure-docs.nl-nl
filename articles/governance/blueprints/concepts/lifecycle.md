---
title: Informatie over de levenscyclus van een blauwdruk
description: Meer informatie over de levenscyclus van een blauwdruk doorloopt en details over elke fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/01/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 2dbf63e745af102de05ec6dc3e3bcb3e98cd3a32
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563708"
---
# <a name="understand-the-lifecycle-of-an-azure-blueprint"></a>Informatie over de levenscyclus van een Azure-blauwdruk

Net als veel resources in Azure heeft een blauwdruk in Azure blauwdrukken een typische en natuurlijke levenscyclus. Ze zijn gemaakt, geïmplementeerd en ten slotte verwijderd wanneer het niet meer nodig of nuttig.
Blauwdrukken ondersteunt standaard levenscyclusbewerkingen. Het bouwt vervolgens bij om extra niveaus van de status van die ondersteuning bieden voor algemene continue integratie en continue implementatie pijplijnen voor organisaties die hun infrastructuur als Code – een belangrijk element bij DevOps beheren.

Om volledig te begrijpen een blauwdruk en fasen, aan bod de levensduur van een standaard:

> [!div class="checklist"]
> - Het maken en bewerken van een blauwdruk
> - De blauwdruk publiceren
> - Het maken en bewerken van een nieuwe versie van de blauwdruk
> - Een nieuwe versie van de blauwdruk publiceren
> - Verwijderen van een specifieke versie van de blauwdruk
> - De blauwdruk verwijderen

## <a name="creating-and-editing-a-blueprint"></a>Het maken en bewerken van een blauwdruk

Wanneer het maken van een blauwdruk toevoegen artefacten, opslaan in een beheergroep of het abonnement en een unieke naam en een unieke versie opgegeven. De blauwdruk is nu in een **Draft** modus en nog kan niet worden toegewezen. Terwijl de **Draft** modus, die u kunt doorgaan moet worden bijgewerkt en gewijzigd.

Een blauwdruk in nooit gepubliceerd **Draft** modus wordt een ander pictogram weergegeven op de **Blauwdrukdefinities** pagina dan de wijzigingsaanvragen die zijn **gepubliceerd**. De **meest recente versie** wordt ook weergegeven als **Draft** voor deze blauwdrukken nooit gepubliceerd.

Maken en bewerken van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#create-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Een blauwdruk publiceren

Zodra alle geplande wijzigingen zijn aangebracht aan een blauwdruk in **Draft** modus, kan het zijn **gepubliceerd** en beschikbaar gesteld voor toewijzing. De **gepubliceerd** versie van de blauwdruk kan niet worden gewijzigd.
Eenmaal **gepubliceerd**, de blauwdruk wordt weergegeven met een ander pictogram dan **Draft** blauwdrukken en wordt de opgegeven versie in de **meest recente versie** kolom.

Publiceren van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#publish-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Het maken en bewerken van een nieuwe versie van de blauwdruk

Een **gepubliceerd** versie van een blauwdruk kan niet worden gewijzigd. Een nieuwe versie van de blauwdruk kan echter worden toegevoegd aan de bestaande blauwdruk en naar wens aanpassen. Breng wijzigingen aan een bestaande blauwdruk door deze te bewerken. Wanneer de nieuwe wijzigingen worden opgeslagen, heeft de blauwdruk nu **niet-gepubliceerde wijzigingen**. Deze wijzigingen zijn een nieuwe **Draft** versie van de blauwdruk.

Een blauwdruk met bewerken de [Azure-portal](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Een nieuwe versie van de blauwdruk publiceren

Elke bewerkte versie van een blauwdruk moet **gepubliceerd** voordat deze kan worden toegewezen. Wanneer **niet-gepubliceerde wijzigingen** zijn aangebracht aan een blauwdruk, maar niet **gepubliceerd**, wordt de **blauwdruk publiceren** knop is beschikbaar op de pagina van de blauwdruk bewerken. Als de knop niet zichtbaar is, de blauwdruk is al **gepubliceerd** en heeft geen **niet-gepubliceerde wijzigingen**.

> [!NOTE]
> Een enkele blauwdruk kunt hebben meerdere **gepubliceerd** versies die elk afzonderlijk kunnen worden toegewezen aan abonnementen.

Voor het publiceren van een blauwdruk met **niet-gepubliceerde wijzigingen**, gebruikt u dezelfde stappen voor het publiceren van een nieuwe blauwdruk.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Verwijderen van een specifieke versie van de blauwdruk

Elke versie van een blauwdruk is een unieke object en kan afzonderlijk worden **gepubliceerd**. Elke versie van een blauwdruk kan daarom ook worden verwijderd. Als u een versie van een blauwdruk verwijdert geen dit van invloed is op andere versies van deze blauwdruk.

> [!NOTE]
> Het is niet mogelijk om te verwijderen van een blauwdruk die actieve toewijzingen heeft. Verwijder eerst de toewijzingen en verwijder vervolgens de versie die u wilt verwijderen.

1. Klik op **Alle services** en selecteer **Beleid** in het linkerdeelvenster. Klik op de pagina **Beleid** op **Blueprints**.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant en gebruik het filter te vinden van de blauwdruk die u wilt verwijderen van een versie van de opties. Klik op de pagina bewerken te openen.

1. Klik op de **gepubliceerde versies** tabblad en zoek de versie die u wilt verwijderen.

1. Met de rechtermuisknop op de versie die u wilt verwijderen en selecteer **deze versie verwijderen**.

## <a name="deleting-the-blueprint"></a>De blauwdruk verwijderen

De blauwdruk core kan ook worden verwijderd. Het verwijderen van de blauwdruk core verwijdert ook de blauwdruk versies van deze blauwdruk, met inbegrip van beide **Draft** en **gepubliceerd** blauwdrukken. Als met het verwijderen van een versie van een blauwdruk verwijdert de blauwdruk core niet de bestaande toewijzing van een van de blauwdruk-versies.

> [!NOTE]
> Het is niet mogelijk om te verwijderen van een blauwdruk die actieve toewijzingen heeft. Verwijder eerst de toewijzingen en verwijder vervolgens de versie die u wilt verwijderen.

Verwijderen van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#delete-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Toewijzingen

Is er verschillende punten tijdens de levenscyclus van die een blauwdruk kan worden toegewezen aan een abonnement. Wanneer de modus van een versie van de blauwdruk is **gepubliceerd**, en vervolgens die versie kan worden toegewezen aan een abonnement. Deze levenscyclus beheren kunt versies van een blauwdruk worden gebruikt en actief toegewezen terwijl een nieuwere versie is ontwikkeld.

Als de versies van blauwdrukken zijn toegewezen, is het belangrijk om te weten waar ze zijn toegewezen en met welke parameters hebben zij is toegewezen met. De parameters kunnen statisch of dynamisch zijn. Zie voor meer informatie, [statische en dynamische parameters](parameters.md).

### <a name="updating-assignments"></a>Toewijzingen bijwerken

Wanneer een blauwdruk wordt toegewezen, kan de toewijzing kan worden bijgewerkt. Er zijn diverse redenen voor het bijwerken van een bestaande toewijzing, met inbegrip van:

- Toevoegen of verwijderen [resource vergrendelen](resource-locking.md)
- Wijzig de waarde van [dynamische parameters](parameters.md#dynamic-parameters)
- De toewijzing van een upgrade uitvoert naar een nieuwere **gepubliceerd** versie van de blauwdruk

Voor meer informatie Zie [bijwerken, bestaande toewijzingen](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Volgende stappen

- Informatie over hoe u [statische en dynamische parameters](parameters.md) gebruikt
- Meer informatie over hoe u de [blauwdrukvolgorde](sequencing-order.md) aanpast
- Ontdek hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](resource-locking.md)
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md)
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)