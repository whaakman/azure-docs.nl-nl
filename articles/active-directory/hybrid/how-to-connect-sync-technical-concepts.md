---
title: 'Azure AD Connect-synchronisatie: Technische concepten | Microsoft Docs'
description: Worden de technische concepten van Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: b5af004cda60111259e95e833d6661e1d9d67ff1
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487248"
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect-synchronisatie: Technische concepten
Dit artikel is een overzicht van het onderwerp [Understanding architectuur](how-to-connect-sync-technical-concepts.md).

Azure AD Connect-synchronisatie is gebaseerd op een solide metadirectory synchronisatie-platform.
De volgende secties vindt de concepten voor metadirectory synchronisatie.
Bouwen voort op MIIS ILM en FIM, biedt de Azure Active Directory Sync Services de volgende platform voor verbinding maken met gegevensbronnen, synchroniseren van gegevens tussen gegevensbronnen, evenals de inrichting en ongedaan maken van inrichting van identiteiten.

![Technische concepten](./media/how-to-connect-sync-technical-concepts/scenario.png)

De volgende secties vindt u meer informatie over de volgende aspecten van de FIM-synchronisatieservice:

* Connector
* Kenmerkstroom
* Connectorgebied
* Metaverse
* Inrichten

## <a name="connector"></a>Connector
De codemodules die worden gebruikt om te communiceren met een verbonden adreslijst worden connectors (voorheen bekend als beheeragents (MAs)) genoemd.

Deze zijn geïnstalleerd op de computer met Azure AD Connect-synchronisatie. De connectors bieden de zonder agent kunnen manier converseren met behulp van protocollen voor extern systeem in plaats van de implementatie van gespecialiseerde agents. Dit betekent dat minder risico en de implementatietijden, met name wanneer er sprake is van kritieke toepassingen en systemen.

In de bovenstaande afbeelding is de connector is gelijk aan het connectorgebied, maar omvat alle communicatie met het externe systeem.

De connector is verantwoordelijk voor alle importeren en exporteren functionaliteit aan het systeem en kunnen ontwikkelaars te weten hoe u verbinding maken met elk systeem systeemeigen bij het gebruik van declaratieve inrichting om aan te passen gegevenstransformaties hoeft.

Invoer en uitvoer alleen optreden bij het plannen, waardoor verdere isolatie van wijzigingen moet worden uitgevoerd binnen het systeem, omdat wijzigingen niet automatisch met de verbonden gegevensbron doorgeven doen. Ontwikkelaars kunnen bovendien ook hun eigen connectors voor het verbinden met vrijwel elke gegevensbron maken.

## <a name="attribute-flow"></a>Kenmerkstroom
De metaverse is de geconsolideerde weergave van alle gekoppelde id's van de aangrenzende connectorspaces. In de bovenstaande afbeelding kenmerkstroom weergegeven met regels met pijlpunten voor zowel binnenkomende als uitgaande stroom. Kenmerkstroom is het proces van het kopiëren of transformeren van gegevens uit het ene systeem naar een andere en alle stromen (inkomend of uitgaand) het kenmerk.

Kenmerkstroom vindt plaats tussen het connectorgebied overgebracht en de metaverse richtingen wanneer synchronisatiebewerkingen (volledige of delta) zijn gepland om uit te voeren.

Kenmerkstroom is alleen sprake wanneer deze synchronisaties worden uitgevoerd. Kenmerkstromen zijn gedefinieerd in synchronisatieregels. Dit kunnen binnenkomende (ISR in de vorige afbeelding) of uitgaande (OSR in de vorige afbeelding) zijn.

## <a name="connected-system"></a>Verbonden systeem
Verbonden systeem (ook wel verbonden directory) is die verwijzen naar het externe systeem Azure AD Connect sync is verbonden met en lezen en schrijven van identiteitsgegevens naar en van.

## <a name="connector-space"></a>Connectorgebied
Elke verbonden gegevensbron wordt weergegeven als een gefilterde subset van de objecten en kenmerken in het connectorgebied.
Dit kan de synchronisatieservice lokaal werken zonder de noodzaak contact opnemen met het externe systeem bij het synchroniseren van de objecten en beperkt-interactie voor invoer en uitvoer alleen.

Wanneer de gegevensbron en de connector hebt de mogelijkheid te bieden een overzicht van de wijzigingen (een delta-import), klikt u vervolgens verhoogd de operationele efficiëntie aanzienlijk als alleen de wijzigingen sinds de laatste polling-cyclus worden uitgewisseld. Het connectorgebied schermt de verbonden gegevensbron tegen wijzigingen doorgeven automatisch door te vereisen dat het schema van de connector importeert en exporteert. Deze extra verzekering verleent u gemoedsrust tijdens het testen, is beschikbaar als Preview of bevestigen van de volgende update.

## <a name="metaverse"></a>Metaverse
De metaverse is de geconsolideerde weergave van alle gekoppelde id's van de aangrenzende connectorspaces.

Als id's aan elkaar zijn gekoppeld en instantie is toegewezen voor verschillende kenmerken via stroomtoewijzingen importeren, begint de centrale metaverse-object met het verzamelde gegevens uit meerdere systemen. Toewijzingen uitvoeren van deze stroom van het kenmerk object informatie aan uitgaande-systemen.

Objecten worden gemaakt wanneer een gezaghebbende systeem ze naar de metaverse-projecten. Zodra alle verbindingen zijn verwijderd, wordt het metaverse-object verwijderd.

Objecten in de metaverse kunnen niet rechtstreeks worden bewerkt. Alle gegevens in het object moet worden bijgedragen door middel van de kenmerkstroom. De metaverse onderhoudt permanente connectors met elke-connectorgebied. Deze connectors hoeven niet opnieuw evalueren van implementaties voor elke synchronisatie uitgevoerd. Dit betekent dat Azure AD Connect-synchronisatie geen het overeenkomende externe object telkens vinden. Hiermee voorkomt de noodzaak van kostbare agents om te voorkomen dat wijzigingen in kenmerken die doorgaans verantwoordelijk is voor het correleren van de objecten.

Bij het detecteren van nieuwe gegevensbronnen die wellicht al bestaande objecten die moeten worden beheerd, Azure AD Connect sync maakt gebruik van een proces genaamd een join-regel om te evalueren mogelijke kandidaten waarmee u een koppeling tot stand brengen.
Nadat de koppeling tot stand is gebracht, deze evaluatie is opgelost en normale kenmerkstroom kan plaatsvinden tussen de externe verbonden gegevensbron en de metaverse.

## <a name="provisioning"></a>Inrichten
Als een gezaghebbende source-projecten kan een nieuw object in de metaverse een nieuwe connector space-object worden gemaakt in een andere Connector vertegenwoordigt een downstream verbonden gegevensbron.

Op deze manier inherent een koppeling maakt en kenmerkstroom richtingen kunt doorgaan.

Wanneer een regel bepaalt dat een nieuwe connector space-object moet worden gemaakt, wordt het inrichten van genoemd. Echter omdat deze bewerking alleen plaats in het connectorgebied overgebracht vindt, wordt deze niet meegenomen in de gekoppelde gegevensbron totdat een export wordt uitgevoerd.

## <a name="additional-resources"></a>Aanvullende resources
* [Azure AD Connect Sync: Synchronisatieopties aanpassen](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
