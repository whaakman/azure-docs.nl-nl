---
title: Het bijwerken van een bestaande Azure-blauwdruktoewijzing
description: Meer informatie over het mechanisme voor het bijwerken van een bestaande toewijzing in blauwdrukken voor Azure.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 2c9f660e54da50e32ce1d0dc43b0efeacd643c57
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093782"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Het bijwerken van een bestaande blauwdruktoewijzing van

Wanneer een blauwdruk wordt toegewezen, kan de toewijzing kan worden bijgewerkt. Er zijn diverse redenen voor het bijwerken van een bestaande toewijzing, met inbegrip van:

- Toevoegen of verwijderen [resource vergrendelen](../concepts/resource-locking.md)
- Wijzig de waarde van [dynamische parameters](../concepts/parameters.md#dynamic-parameters)
- De toewijzing van een upgrade uitvoert naar een nieuwere **gepubliceerd** versie van de blauwdruk

## <a name="updating-assignments"></a>Toewijzingen bijwerken

1. Klik op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Klik op de pagina **Beleid** op **Blueprints**.

1. Selecteer **Toegewezen blauwdrukken** op de pagina aan de linkerkant.

1. In de lijst met blauwdrukken, de blauwdruktoewijzing te klikken. Klik vervolgens op de **bijwerken van de toewijzing** of met de rechtermuisknop op de blauwdruktoewijzing en selecteer **bijwerken van de toewijzing**.

   ![Bijwerken van de toewijzing](../media/update-existing-assignments/update-assignment.png)

1. De **blauwdruk toewijzen** pagina vooraf gevuld met alle waarden uit de oorspronkelijke toewijzing wordt geladen. Kunt u de **blauwdruk definitieversies**, wordt de **vergrendeling toewijzing** staat, en een van de dynamische parameters die aanwezig zijn op de blauwdrukdefinitie van de. Klik op **toewijzen** wanneer alle wijzigingen hebt aangebracht.

1. Op de pagina met details van bijgewerkte toewijzing, moet u de nieuwe status zien. In dit voorbeeld hebben we toegevoegd **vergrendelen** aan de toewijzing.

   ![Bijgewerkte toewijzing - vergrendeld](../media/update-existing-assignments/updated-assignment.png)

1. Bekijk details over andere **toewijzingsbewerkingen** met behulp van de vervolgkeuzelijst. De tabel van **beheerde Resources** updates voor geselecteerde toewijzingsbewerking.

   ![Toewijzingsbewerkingen](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Regels voor het bijwerken van toewijzingen

De implementatie van de bijgewerkte toewijzingen volgt een aantal belangrijke regels. Deze regels bepalen wat er gebeurt met de reeds geïmplementeerde resources. De aangevraagde wijziging en het type artefact-resource wordt geïmplementeerd of bijgewerkt, bepalen welke acties worden uitgevoerd.

- Roltoewijzingen
  - Als de functie of de rol toegewezen gebruiker (gebruiker, groep of app) wordt gewijzigd, wordt een nieuwe roltoewijzing wordt gemaakt. Roltoewijzingen eerder geïmplementeerd blijven.
- Beleidstoewijzingen
  - Als de parameters van de beleidstoewijzing zijn gewijzigd, wordt de bestaande toewijzing wordt bijgewerkt.
  - Als de definitie van de beleidstoewijzing is gewijzigd, wordt een nieuwe beleidstoewijzing gemaakt. Beleidstoewijzingen eerder geïmplementeerd blijven.
  - Als het beleid toewijzen-artefact wordt verwijderd uit de blauwdruk, geïmplementeerd beleid toewijzingen blijven.
- Azure Resource Manager-sjablonen
  - De sjabloon wordt verwerkt via Resource Manager als een **plaatsen**. Als u deze actie worden elk resourcetype anders verwerkt, Raadpleeg de documentatie voor elke resource opgenomen om de impact van deze actie als uitgevoerd door de blauwdrukken te bepalen.

## <a name="possible-errors-on-updating-assignments"></a>Mogelijke fouten bij het bijwerken van toewijzingen

Tijdens het bijwerken van toewijzingen, is het mogelijk te wijzigen die bij uitvoering worden afgebroken. Een voorbeeld is de locatie van een resourcegroep te wijzigen nadat deze al is geïmplementeerd. Eventuele wijzigingen die worden ondersteund door [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) kan worden gemaakt, maar een wijzigen dat zou leiden tot een fout via Azure Resource Manager wordt ook leiden tot het mislukken van de toewijzing.

Er is geen limiet voor het aantal keren dat een toewijzing kunnen worden bijgewerkt. Als er een fout optreedt, bepalen van de fout en moet u een andere update in de toewijzing.  Voorbeeldscenario-fout:

- Een ongeldige parameter
- Een bestaand object
- Een wijziging die niet wordt ondersteund door Azure Resource Manager

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](../concepts/lifecycle.md)
- Informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast
- Ontdek hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../concepts/resource-locking.md)
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)
