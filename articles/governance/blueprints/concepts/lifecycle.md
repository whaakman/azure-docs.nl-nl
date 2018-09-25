---
title: Informatie over de levenscyclus van een Azure-blauwdruk
description: Meer informatie over de levenscyclus die een blauwdruk doorloopt en details over elke fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e0790168a8b9590aaa440a04cd99f26c2ece2818
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991536"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Informatie over de levenscyclus van een Azure-blauwdruk

Net als veel resources in Azure heeft een blauwdruk in Azure blauwdrukken een typische en natuurlijke levenscyclus. Ze zijn gemaakt, geïmplementeerd en ten slotte verwijderd wanneer het niet meer nodig of nuttig.
Blauwdrukken ondersteunt traditionele CRUD (maken/lezen/bijwerken/verwijderen) levenscyclus van bewerkingen, maar ook uitbreiden om extra niveaus van de status van die ondersteuning bieden voor algemene continue integratie / continue implementatie (CI/CD) voor gebruik door de pijplijnen. Infrastructure as Code (IaC) beheren van hun infrastructuur via code – een belangrijk element bij DevOps genoemd.

Om volledig te begrijpen een blauwdruk en fasen, aan bod standaard levenscyclus:

> [!div class="checklist"]
> - Het maken en bewerken van een blauwdruk
> - De blauwdruk publiceren
> - Het maken en bewerken van een nieuwe versie van de blauwdruk
> - Een nieuwe versie van de blauwdruk publiceren
> - Verwijderen van een specifieke versie van de blauwdruk
> - De blauwdruk verwijderen

## <a name="creating-and-editing-a-blueprint"></a>Het maken en bewerken van een blauwdruk

Bij het maken van een blauwdruk, artefacten, opslaan in een beheergroep en een unieke naam en een unieke versie opgegeven toevoegen. Op dit moment de blauwdruk is in een **Draft** modus en nog kan niet worden toegewezen. Echter, terwijl de **Draft** modus die u kunt doorgaan moet worden bijgewerkt en gewijzigd.

Een blauwdruk in **Draft** modus die nooit is gepubliceerd een ander pictogram wordt weergegeven op de **Blauwdrukdefinities** pagina dan die zijn **gepubliceerd**. De **meest recente versie** wordt ook weergegeven als **Draft** voor deze blauwdrukken nooit gepubliceerd.

Maken en bewerken van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#create-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Een blauwdruk publiceren

Zodra alle gewenste wijzigingen zijn aangebracht aan een blauwdruk in **Draft** modus, kan het zijn **gepubliceerd** en beschikbaar gesteld voor toewijzing. De **gepubliceerd** versie van de blauwdruk kan niet worden gewijzigd.
Eenmaal **gepubliceerd**, de blauwdruk wordt weergegeven met een ander pictogram dan **Draft** blauwdrukken en wordt de opgegeven versie in de **meest recente versie** kolom.

Publiceren van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#publish-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Het maken en bewerken van een nieuwe versie van de blauwdruk

Hoewel een **gepubliceerd** versie van een blauwdruk kan niet worden gewijzigd, een nieuwe versie van de blauwdruk kan worden toegevoegd aan de bestaande blauwdruk en naar wens aanpassen. Dit wordt gedaan door het aanbrengen van wijzigingen in een bestaande blauwdruk. Als de blauwdruk al is **gepubliceerd**, wanneer deze wijzigingen worden opgeslagen, ze worden weergegeven als **niet-gepubliceerde wijzigingen** in de lijst met blauwdrukdefinities. Opslaan van de wijzigingen slaat een **Draft** versie van de blauwdruk.

Een blauwdruk met bewerken de [Azure-portal](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Een nieuwe versie van de blauwdruk publiceren

Net als de eerste versie van een blauwdruk is **gepubliceerd** toe te wijzen, moet elke latere versie van die dezelfde blauwdruk zijn **gepubliceerd** voordat deze kan worden toegewezen. Wanneer **niet-gepubliceerde wijzigingen** zijn aangebracht aan een blauwdruk, maar deze nog niet hebt nog **gepubliceerd**, wordt de **blauwdruk publiceren** knop is beschikbaar op de pagina van de blauwdruk bewerken. Als de knop niet zichtbaar is, de blauwdruk is al **gepubliceerd**, maar heeft geen **niet-gepubliceerde wijzigingen**.

> [!NOTE]
> Een enkele blauwdruk kunt hebben meerdere **gepubliceerd** versies die elk afzonderlijk kunnen worden toegewezen aan abonnementen.

De stappen voor het publiceren van een blauwdruk met **niet-gepubliceerde wijzigingen** als een nieuwe versie van een bestaande blauwdruk zijn hetzelfde als de stappen voor het publiceren van een nieuwe blauwdruk.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Verwijderen van een specifieke versie van de blauwdruk

Elke versie van een blauwdruk is een unieke object en kan afzonderlijk worden **gepubliceerd**. Dit betekent ook dat elke versie van een blauwdruk kan worden verwijderd. Als u een versie van een blauwdruk verwijdert geen dit van invloed is op andere versies van deze blauwdruk.

> [!NOTE]
> Het is niet mogelijk om te verwijderen van een blauwdruk die actieve toewijzingen heeft. Verwijder eerst de toewijzingen en verwijder vervolgens de versie die u wilt verwijderen.

1. Start de blauwdrukken voor Azure-service in Azure portal door te klikken op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Op de **beleid** pagina, klikt u op **blauwdrukken**.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant en gebruik het filter te vinden van de blauwdruk die u wilt verwijderen van een versie van de opties. Klik op de pagina bewerken te openen.

1. Klik op de **gepubliceerde versies** tabblad en zoek de versie die u wilt verwijderen.

1. Met de rechtermuisknop op de versie die u wilt verwijderen en selecteer **deze versie verwijderen**.

## <a name="deleting-the-blueprint"></a>De blauwdruk verwijderen

De blauwdruk core kan ook worden verwijderd. De blauwdruk core ook verwijdert, worden alle versies van de blauwdruk van deze blauwdruk ongeacht **Draft** of **gepubliceerd** status. Als met het verwijderen van een versie van een blauwdruk verwijdert de blauwdruk core niet de bestaande toewijzing van een van de blauwdruk-versies.

> [!NOTE]
> Het is niet mogelijk om te verwijderen van een blauwdruk die actieve toewijzingen heeft. Verwijder eerst de toewijzingen en verwijder vervolgens de versie die u wilt verwijderen.

Verwijderen van een blauwdruk met de [Azure-portal](../create-blueprint-portal.md#delete-a-blueprint) of [REST-API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Toewijzingen

Er zijn enkele punten tijdens de levenscyclus die een blauwdruk kan worden toegewezen aan een abonnement.
Telkens wanneer de modus van een versie van de blauwdruk is **gepubliceerd**, en vervolgens die versie kan worden toegewezen aan een abonnement. Zelfs als er een **Draft** versie van de blauwdruk, als er een of meer blauwdruk versies in een **gepubliceerd** modus en elk van deze **gepubliceerd** versies is beschikbaar om toe te wijzen. Hierdoor kunnen versies van een blauwdruk worden gebruikt en actief toegewezen terwijl een nieuwere versie is ontwikkeld.

Als de versies van blauwdrukken zijn toegewezen, is het belangrijk om te weten waar ze zijn toegewezen en met welke parameters hebben zij is toegewezen met. De parameters kunnen statisch of dynamisch zijn. Zie voor meer informatie, [statische en dynamische parameters](parameters.md).

### <a name="updating-assignments"></a>Toewijzingen bijwerken

Wanneer een blauwdruk wordt toegewezen, kan de toewijzing kan worden bijgewerkt. Er zijn diverse redenen voor het bijwerken van een bestaande toewijzing, met inbegrip van:

- Toevoegen of verwijderen [resource vergrendelen](resource-locking.md)
- Wijzig de waarde van [dynamische parameters](parameters.md#dynamic-parameters)
- De toewijzing van een upgrade uitvoert naar een nieuwere **gepubliceerd** versie van de blauwdruk

Voor meer informatie Zie [bijwerken, bestaande toewijzingen](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [statische en dynamische parameters](parameters.md)
- Meer informatie over het aanpassen van de [volgorde blauwdruk](sequencing-order.md)
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](resource-locking.md)
- Meer informatie over het [bestaande toewijzingen bijwerken](../how-to/update-existing-assignments.md)
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)