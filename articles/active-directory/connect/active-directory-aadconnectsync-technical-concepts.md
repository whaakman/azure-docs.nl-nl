---
title: 'Azure AD Connect-synchronisatie: technische concepten | Microsoft Docs'
description: Legt uit de technische concepten van Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi;andkjell
ms.openlocfilehash: 2cb5143729fa92d12b21701e62416bced31c1fc7
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect-synchronisatie: technische concepten
In dit artikel is een overzicht van het onderwerp [Understanding architectuur](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect-synchronisatie is gebaseerd op een effen metadirectory synchronisatie-platform.
De volgende secties vindt de concepten voor metadirectory synchronisatie.
Bouwt voort op MIIS, ILM en FIM, biedt Azure Active Directory Sync Services de volgende platform voor verbinding maken met gegevensbronnen, synchroniseren van gegevens tussen gegevensbronnen, evenals de inrichting en het opheffen van inrichting van identiteiten.

![Technische concepten](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

De volgende secties bevatten meer informatie over de volgende aspecten van de FIM-synchronisatieservice:

* Connector
* Kenmerkstroom
* Connectorgebied
* Metaverse
* Inrichten

## <a name="connector"></a>Connector
De codemodules die worden gebruikt om te communiceren met een gekoppelde adreslijst worden connectors (voorheen bekend als beheeragents (MAs)) genoemd.

Deze wordt geïnstalleerd op de computer met Azure AD Connect-synchronisatie. De connectors bieden zonder agent kunnen manier converseren door extern systeem te gebruiken in plaats van te vertrouwen op de implementatie van gespecialiseerde agents. Dit betekent dat minder risico en implementatietijden, vooral wanneer omgaan met bedrijfskritieke toepassingen en systemen.

In de bovenstaande afbeelding is de connector is gelijk aan het connectorgebied overgebracht, maar omvat alle communicatie met het externe systeem.

De connector is verantwoordelijk voor alle importeren en exporteren functionaliteit aan het systeem en ontwikkelaars van hoeft te begrijpen hoe u verbinding maken met elk systeem systeemeigen gegevenstransformaties aanpassen met declaratieve inrichting maakt.

Invoer en uitvoer alleen worden uitgevoerd als gepland, waardoor verdere isolatie van veranderingen in het systeem omdat wijzigingen niet automatisch naar de gekoppelde gegevensbron doorgegeven kunnen. Ontwikkelaars kunnen bovendien ook hun eigen connectors voor het verbinden met vrijwel elke gegevensbron maken.

## <a name="attribute-flow"></a>Kenmerkstroom
De metaverse is de geconsolideerde weergave van alle gekoppelde identiteiten van de aangrenzende connectorspaces. Kenmerkstroom wordt in de figuur hierboven is beschreven door regels met pijlpunten voor stroom inkomende en uitgaande. Kenmerkstroom is het proces van het kopiëren of omzetten van gegevens van het ene systeem naar een andere en alle kenmerk stromen (inkomend of uitgaand).

Kenmerkstroom vindt plaats tussen het connectorgebied overgebracht en de metaverse twee richtingen als synchronisatiebewerkingen (volledige of delta) zijn gepland.

Kenmerkstroom treedt alleen op wanneer deze synchronisaties worden uitgevoerd. Kenmerkstromen zijn in synchronisatieregels gedefinieerd. Deze kunnen zich inkomende ISR (in de bovenstaande afbeelding) of uitgaande (OSR in de bovenstaande afbeelding).

## <a name="connected-system"></a>Verbonden systeem
Verbonden systeem (aka verbonden directory) verwijst naar het externe systeem Azure AD Connect sync is verbonden met en leest en schrijft van identiteitsgegevens naar en van.

## <a name="connector-space"></a>Connectorgebied
Elke verbonden gegevensbron wordt weergegeven als een gefilterde subset van de objecten en kenmerken in het connectorgebied overgebracht.
Hiermee kan de synchronisatieservice lokaal werken zonder de noodzaak contact opnemen met het externe systeem bij het synchroniseren van de objecten en beperkt-interactie voor invoer en uitvoer alleen.

Wanneer de gegevensbron en de connector hebt de mogelijkheid om te voorzien in een lijst met wijzigingen (een delta-import), klikt u vervolgens stijgt de operationele efficiëntie aanzienlijk als alleen wijzigingen sinds de laatste polling-cyclus worden uitgewisseld. Het connectorgebied overgebracht schermt de gekoppelde gegevensbron uit wijzigingen doorgeven automatisch door te vereisen dat de planning van de connector importeert en exporteert. Deze extra verzekering verleent u gemoedsrust tijdens het testen, bekijken of bevestiging van de volgende update.

## <a name="metaverse"></a>Metaverse
De metaverse is de geconsolideerde weergave van alle gekoppelde identiteiten van de aangrenzende connectorspaces.

Als identiteiten aan elkaar zijn gekoppeld en instantie is toegewezen voor verschillende kenmerken via stroomtoewijzingen importeren, wordt de status van de centrale metaverse-object begint met het verzamelde gegevens uit meerdere systemen. Toewijzingen uitvoeren van deze overdracht van het kenmerk object informatie naar uitgaande systemen.

Objecten worden gemaakt wanneer een bindende systeem ze naar de metaverse-projecten. Zodra alle verbindingen zijn verwijderd, wordt het metaverse-object verwijderd.

Objecten in de metaverse kunnen niet rechtstreeks worden bewerkt. Alle gegevens in het object moet worden bijgedragen via kenmerkstroom. De metaverse onderhoudt permanente connectors met elke connectorgebied overgebracht. Deze connectors hoeven niet opnieuw evalueren voor elke synchronisatie uitvoeren. Dit betekent dat dat Azure AD Connect-synchronisatie geen telkens wanneer voor de overeenkomende externe object vinden. Dit voorkomt dat de behoefte aan dure agents om te voorkomen dat wijzigingen in kenmerken die normaal gesproken verantwoordelijk is voor het correleren van de objecten.

Bij het detecteren van nieuwe gegevensbronnen die wellicht al bestaande objecten die moeten worden beheerd, Azure AD Connect-synchronisatie maakt gebruik van een proces genaamd een join-regel om te evalueren mogelijke kandidaten waarmee een koppeling tot stand brengen.
Wanneer de koppeling is gemaakt, wordt deze evaluatie niet meer optreedt en normale kenmerkstroom kan optreden tussen de verbonden externe gegevensbron en de metaverse.

## <a name="provisioning"></a>Inrichten
Wanneer een gezaghebbende bronprojecten kan een nieuw object naar de metaverse een nieuw object van de connector-ruimte in een andere Connector voor een downstream verbonden gegevensbron worden gemaakt.

Dit inherent legt een koppeling vast en kenmerkstroom twee richtingen kunt doorgaan.

Wanneer een regel bepaalt dat er een nieuw object van de connector-ruimte moet worden gemaakt, wordt het genoemd inrichten. Echter, omdat deze bewerking alleen uitgevoerd binnen het connectorgebied overgebracht wordt, deze komt niet meegenomen in de gekoppelde gegevensbron totdat het exporteren van een wordt uitgevoerd.

## <a name="additional-resources"></a>Aanvullende resources
* [Azure AD Connect-synchronisatie: Synchronisatie-opties voor aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
