---
title: Het bijwerken van een bestaande Azure-blauwdruktoewijzing
description: Meer informatie over het mechanisme voor het bijwerken van een bestaande toewijzing in blauwdrukken voor Azure.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: ecac0fb21a6691874d5e8db49eadd7114d41845f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956197"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Het bijwerken van een bestaande blauwdruktoewijzing van

Wanneer een blauwdruk wordt toegewezen, kan de toewijzing kan worden bijgewerkt. Er zijn diverse redenen voor het bijwerken van een bestaande toewijzing, met inbegrip van:

- Toevoegen of verwijderen [resource vergrendelen](../concepts/resource-locking.md)
- Wijzig de waarde van [dynamische parameters](../concepts/parameters.md#dynamic-parameters)
- De toewijzing van een upgrade uitvoert naar een nieuwere **gepubliceerd** versie van de blauwdruk

## <a name="updating-assignments"></a>Toewijzingen bijwerken

1. Start de blauwdrukken voor Azure-service in Azure portal door te klikken op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Op de **beleid** pagina, klikt u op **blauwdrukken**.

1. Selecteer **toegewezen blauwdrukken** op de pagina aan de linkerkant.

1. De blauwdruktoewijzing te klikken in de lijst met blauwdrukken, en klik vervolgens op de **bijwerken van de toewijzing** of met de rechtermuisknop op de blauwdruktoewijzing en selecteer **bijwerken van de toewijzing**.

   ![Bijwerken van de toewijzing](../media/update-existing-assignments/update-assignment.png)

1. De **blauwdruk toewijzen** pagina vooraf gevuld met alle waarden uit de oorspronkelijke toewijzing wordt geladen. Kunt u de **blauwdruk definitieversies**, wordt de **vergrendeling toewijzing** staat, en een van de dynamische parameters die aanwezig zijn op de blauwdrukdefinitie van de. Klik op **toewijzen** wanneer alle wijzigingen hebt aangebracht.

1. Op de pagina met details van bijgewerkte toewijzing, moet u de nieuwe status zien. In dit voorbeeld hebben we toegevoegd **vergrendelen** aan de toewijzing.

   ![Bijgewerkte toewijzing - vergrendeld](../media/update-existing-assignments/updated-assignment.png)

1. Bekijk details over andere **toewijzingsbewerkingen** met behulp van de vervolgkeuzelijst. De tabel van **beheerde Resources** updates voor geselecteerde toewijzingsbewerking.

   ![Toewijzingsbewerkingen](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Regels voor het bijwerken van toewijzingen

De implementatie van de bijgewerkte toewijzingen volgt een aantal belangrijke regels. Deze regels bepalen wat er gebeurt met een bestaande resource, afhankelijk van de aangevraagde wijziging en het type artefact resource wordt geïmplementeerd of bijgewerkt.

- Roltoewijzingen
  - Als de functie of de rol toegewezen gebruiker (gebruiker, groep of app) wordt gewijzigd, wordt een nieuwe roltoewijzing wordt gemaakt. De roltoewijzing van de eerder geïmplementeerde blijft aanwezig.
- Beleidstoewijzingen
  - Als de parameters van de beleidstoewijzing zijn gewijzigd, wordt de bestaande toewijzing wordt bijgewerkt.
  - Als de definitie van de beleidstoewijzing zijn gewijzigd, wordt een nieuwe beleidstoewijzing gemaakt. De eerder geïmplementeerde beleidstoewijzing is blijven.
  - Als het beleid toewijzen-artefact wordt verwijderd uit de blauwdruk, wordt de eerder geïmplementeerde beleidstoewijzing links op locatie.
- Azure Resource Manager-sjablonen
  - De sjabloon wordt verwerkt via Resource Manager als een **plaatsen**. Als u elk resourcetype verwerkt dit anders, Raadpleeg de documentatie voor elke resource opgenomen om de impact van deze actie als uitgevoerd door de blauwdrukken te bepalen.

## <a name="possible-errors-on-updating-assignments"></a>Mogelijke fouten bij het bijwerken van toewijzingen

Tijdens het bijwerken van toewijzingen, is het mogelijk wijzigingen aanbrengen die afbreken wanneer uitgevoerd. Een voorbeeld hiervan is de locatie van een resourcegroep te wijzigen nadat deze al is geïmplementeerd. Eventuele wijzigingen die worden ondersteund door [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) kan worden gemaakt, maar een wijzigen dat zou leiden tot een fout via Azure Resource Manager wordt ook leiden tot het mislukken van de toewijzing.

Er is geen limiet voor het aantal keren dat een toewijzing kunnen worden bijgewerkt. Als er een fout optreedt, vanwege een ongeldige parameter, een bestaand object of een wijziging is niet toegestaan door Azure Resource Manager, de fout te bepalen en dus moet u een andere update in de toewijzing.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk levenscyclus](../concepts/lifecycle.md)
- Meer informatie over het gebruik van [statische en dynamische parameters](../concepts/parameters.md)
- Meer informatie over het aanpassen van de [volgorde blauwdruk](../concepts/sequencing-order.md)
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](../concepts/resource-locking.md)
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)
