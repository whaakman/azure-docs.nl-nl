---
title: Afhandeling van tijdelijke connectiviteitsfouten voor Azure Database for PostgreSQL
description: Leer hoe u voor het afhandelen van tijdelijke connectiviteitsfouten voor Azure Database voor PostgreSQL.
keywords: postgresql-verbinding, verbindingsreeks, problemen met de netwerkverbinding, tijdelijke fout, -verbindingsfout
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 264656da38608026e3f9e866e2184ff55ba102d8
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536216"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-postgresql"></a>Afhandeling van tijdelijke connectiviteitsfouten voor Azure Database for PostgreSQL

Dit artikel wordt beschreven hoe u voor het afhandelen van tijdelijke fouten verbinden met Azure Database voor PostgreSQL.

## <a name="transient-errors"></a>Tijdelijke fouten

Een tijdelijke fout, ook wel bekend als een tijdelijke fouten, is een fout die wordt automatisch opgelost. Deze fouten manifest doorgaans als een verbinding met de database-server wordt verwijderd. Ook kunnen niet nieuwe verbindingen met een server worden geopend. Tijdelijke fouten kunnen bijvoorbeeld optreden wanneer de hardware-of netwerk gebeurt. Een andere reden kan zijn van een nieuwe versie van een PaaS-service die is geïmplementeerd. De meeste van deze gebeurtenissen worden automatisch door het systeem verholpen in minder dan 60 seconden. Er is een best practice voor het ontwerpen en ontwikkelen van toepassingen in de cloud kunt verwachten van tijdelijke fouten. Wordt ervan uitgegaan dat ze kunnen gebeuren in een onderdeel op elk gewenst moment en om de juiste logica voor het verwerken van deze situaties.

## <a name="handling-transient-errors"></a>Afhandeling van tijdelijke fouten

Tijdelijke fouten moeten worden verwerkt met behulp van de logica voor opnieuw proberen. Situaties die moeten worden beschouwd:

* Er treedt een fout op wanneer u probeert om een verbinding te openen
* Een niet-actieve verbindingen is op de server verwijderd. Wanneer u een opdracht kan niet worden uitgevoerd
* Een actieve verbinding die momenteel is een opdracht is uitgevoerd, is verwijderd.

Het eerste en tweede geval zijn redelijk eenvoudig om af te handelen. Probeer de verbinding opnieuw te openen. Als u erin slaagt, is de tijdelijke fout is verholpen door het systeem. U kunt uw Azure Database for PostgreSQL opnieuw gebruiken. We raden u aan wacht voordat opnieuw wordt geprobeerd de verbinding. Back-ups uitgeschakeld als de eerste nieuwe pogingen mislukken. Op deze manier het systeem kunt alle resources die beschikbaar zijn om te strijden tegen de foutsituatie gebruiken. Er is een goede patroon te volgen:

* Wacht 5 seconden voordat de eerste poging.
* Voor elke volgende nieuwe poging, worden de toename van het wachten tot exponentieel toeneemt, 60 seconden.
* Stel een maximum aantal nieuwe pogingen op het moment waarop uw toepassing rekening gehouden met de bewerking is mislukt.

Wanneer een verbinding met een actieve transactie mislukt, is het moeilijker voor het afhandelen van het herstel correct. Er zijn twee mogelijke situaties: Als de transactie alleen-lezen in aard is, is het veilig om de verbinding opnieuw te openen en voer de transactie opnieuw uit. Als echter als de transactie is ook schrijven naar de database, moet u bepalen als de transactie is teruggedraaid of als deze is geslaagd voordat wordt de tijdelijke fout is opgetreden. In dat geval wordt u mogelijk niet hebt ontvangen de bevestiging van het doorvoeren van de database-server.

Er is één manier om dit te doen, het genereren van een unieke ID op de client die wordt gebruikt voor alle nieuwe pogingen. U doorgeven deze unieke ID als onderdeel van de transactie op de server en bewaar deze in een kolom met een unique-beperking. Op deze manier die u veilig opnieuw de transactie proberen kunt. Deze zal slagen als de vorige transactie is teruggedraaid en de unieke ID van de client gegenereerd nog niet in het systeem bestaat. Deze mislukken, die wijzen op een schending van de dubbele sleutel als de unieke ID eerder opgeslagen is omdat de vorige transactie is voltooid.

Wanneer uw programma met Azure Database for PostgreSQL via externe middleware communiceert, vraagt u de leverancier of de middleware Pogingslogica voor tijdelijke fouten bevat.

Zorg ervoor dat voor het testen van u logica voor opnieuw proberen. Bijvoorbeeld, probeert uit te voeren van uw code tijdens schaal omhoog of omlaag de compute-resources van de Azure Database for PostgreSQL-server. Uw toepassing moet de korte uitvaltijd die wordt aangetroffen tijdens deze bewerking zonder problemen verwerken.

## <a name="next-steps"></a>Volgende stappen

* [Verbindingsproblemen met Azure Databases for PostgreSQL oplossen](howto-troubleshoot-common-connection-issues.md)
