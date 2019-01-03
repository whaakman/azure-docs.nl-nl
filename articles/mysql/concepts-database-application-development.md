---
title: Overzicht van database-toepassing voor ontwikkelaars voor Azure Database for MySQL
description: Introduceert ontwerpoverwegingen die een ontwikkelaar volgen moet bij het schrijven van code van de toepassing verbinding maakt met Azure Database voor MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 946f7011c51b7c6844e023d03e01e4c2043d2578
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53544460"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Overzicht van toepassing voor ontwikkelaars voor Azure Database for MySQL 
Dit artikel worden besproken ontwerpoverwegingen die een ontwikkelaar volgen moet bij het schrijven van code van de toepassing verbinding maakt met Azure Database voor MySQL. 

> [!TIP]
> Zie voor een zelfstudie waarin u over het maken van een server, een server gebaseerde firewall maken, weergeven van servereigenschappen, -database maken en verbinding maken en query's uitvoeren met behulp van workbench en mysql.exe [uw eerste Azure Database voor MySQL-database ontwerpen](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Taal en platform
Er zijn codevoorbeelden beschikbaar voor verschillende programmeertalen en platforms. Hier vindt u koppelingen naar de codevoorbeelden: [Connectiviteitsbibliotheken gebruikt voor het verbinding maken met Azure Database for MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Hulpprogramma's
Azure Database voor MySQL maakt gebruik van de MySQL community-versie, compatibel zijn met MySQL algemene beheerprogramma's zoals Workbench of MySQL-hulpprogramma's zoals mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), enzovoort. U kunt ook de Azure portal, Azure CLI en REST-API's gebruiken om te communiceren met de database-service.

## <a name="resource-limitations"></a>Resourcebeperkingen
Azure Database voor MySQL beheert de resources die beschikbaar zijn voor een server met behulp van twee verschillende mechanismen: 
- Resources Governance.
- Afdwinging van limieten.

## <a name="security"></a>Beveiliging
Azure Database voor MySQL biedt resources voor beperkende toegang, beveiligt gegevens, configureren gebruikers en rollen en bewakingsactiviteiten in een MySQL-database.

## <a name="authentication"></a>Verificatie
Azure Database voor MySQL biedt ondersteuning voor server-verificatie van gebruikers en aanmeldgegevens.

## <a name="resiliency"></a>Flexibiliteit
Wanneer er een tijdelijke fout optreedt bij het verbinden met een MySQL-database, moet uw code de aanroep opnieuw. Wordt aanbevolen dat de logica voor opnieuw proberen uitstel logische zodat deze niet overbelast raakt de SQL-database met meerdere clients tegelijk opnieuw proberen.

- Codevoorbeelden: Voor codevoorbeelden met daarin logica voor opnieuw proberen, ziet u voorbeelden van de taal van uw keuze: [Connectiviteitsbibliotheken gebruikt voor het verbinding maken met Azure Database for MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Verbindingen beheren
Databaseverbindingen zijn hier een beperkte resource, raden wij aan zijn verbindingen worden gebruikt bij het openen van uw MySQL-database om betere prestaties te behalen.
- Toegang tot de database met behulp van permanente verbindingen of Groepsgewijze verbinding.
- Toegang tot de database met behulp van de levensduur van een korte verbinding. 
- Pogingslogica voor gebruik in uw toepassing op het moment van de verbindingspoging om af te vangen van fouten die voortvloeien uit gelijktijdige verbindingen hebt bereikt van het maximaal toegestane aantal. In de logica voor opnieuw proberen, een korte vertraging instellen en vervolgens wachten op een willekeurige periode voordat de aanvullende verbindingspogingen.