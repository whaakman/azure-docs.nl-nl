---
title: Verbinding maken met een gegevensbron in Power BI werkruimte verzamelingen | Microsoft Docs
description: Informatie over het verbinding maken met een gegevensbron in Power BI werkruimte verzamelingen.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 24600c4343e3bfebe14f25020c5a7ba02d15af64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-a-data-source"></a>Verbinding maken met een gegevensbron

Met **Power BI werkruimte verzamelingen**, kunt u rapporten insluiten in uw eigen app. Wanneer u een Power BI-rapport in uw app insluiten, het rapport maakt verbinding met de onderliggende gegevens door **importeren** een kopie van de gegevens of door **rechtstreeks verbinding maken** naar de gegevensbron via **DirectQuery**.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Dit zijn de verschillen tussen **Importeren** en **DirectQuery**.

| Importeren | DirectQuery |
| --- | --- |
| Tabellen, kolommen *en gegevens* zijn geïmporteerd of gekopieerd naar het rapport gegevensset. Om wijzigingen te bekijken die zijn opgetreden in de onderliggende gegevens, moet u vernieuwen of een volledige, actuele gegevensset opnieuw importeren. |Alleen *tabellen en kolommen* zijn geïmporteerd of gekopieerd naar het rapport gegevensset. U weergeven altijd de meest recente gegevens. |

Met Power BI werkruimte verzamelingen u DirectQuery kunt gebruiken met gegevensbronnen van de cloud, maar niet on-premises gegevensbronnen op dit moment.

> [!NOTE]
> De gegevensgateway On-Premises wordt niet ondersteund met Power BI werkruimte verzamelingen op dit moment. Dit betekent dat u DirectQuery niet gebruiken met on-premises gegevensbronnen.

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen

**DirectQuery**
* Azure SQL-database
* Azure SQL Data Warehouse

**Importeren**

U kunt importeren met alle van de beschikbare gegevensbronnen in Power BI Desktop. U gaat **niet** kunnen vernieuwen van gegevens in Power BI werkruimte verzamelingen. U hebt voor het uploaden van wijzigingen naar uw PBIX-bestand naar Power BI werkruimte verzamelingen. Dit komt door geen beschikbare gateway. 

## <a name="benefits-of-using-directquery"></a>Voordelen van het gebruik van DirectQuery

Er zijn twee belangrijke voordelen bij gebruik van **DirectQuery**:

* **DirectQuery** kunt u bouwen visualisaties alle gegevens via grote gegevenssets, waar deze anders unfeasible eerste importeren zijn zou.
* Voor sommige rapporten de noodzaak om weer te geven gegevens van de huidige overdrachten van grote hoeveelheden gegevens, waardoor gegevens opnieuw te importeren unfeasible kunt vereisen en onderliggende gegevenswijzigingen vereist een vernieuwing van gegevens. Daarentegen **DirectQuery** rapporten altijd actuele gegevens gebruiken.

## <a name="limitations-of-directquery"></a>Beperkingen van DirectQuery

Er zijn enkele beperkingen voor het gebruik van **DirectQuery**:

* Alle tabellen moeten afkomstig zijn van een individuele database.
* Als de query te complex is is, treedt een fout op. Als u wilt de fout te verhelpen moet u de query opsplitsen zodat deze minder complex is. Als de query complex zijn moet, moet u de gegevens in plaats van importeren **DirectQuery**.
* Relatie voor het filteren is beperkt tot één richting, in plaats van beide richtingen.
* U kunt het gegevenstype van een kolom niet wijzigen.
* Standaard worden de beperkingen op DAX-expressies toegestaan in metingen geplaatst. Zie [DirectQuery en metingen](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery en metingen
Query's verzonden naar de onderliggende gegevensbron hebben aanvaardbare prestaties, zodat worden beperkingen opgelegd voor metingen. Wanneer u **Power BI Desktop**, geavanceerde gebruikers kunnen kiezen deze beperking omzeilen door te kiezen **bestand > Opties en instellingen > opties**. In de **opties** dialoogvenster kiezen **DirectQuery**, en selecteer de optie **onbeperkte metingen in DirectQuery-modus toestaan**. Wanneer deze optie is geselecteerd, kan een DAX-expressie die is geldig voor een meting kan worden gebruikt. Gebruikers moeten zich op de hoogte; echter dat sommige expressies die ook uitvoeren wanneer de gegevens zijn geïmporteerd leiden traag tot kunnen vragen naar de back-end bron wanneer in **DirectQuery** modus. 

## <a name="see-also"></a>Zie ook

* [Aan de slag met Microsoft Power BI werkruimte verzamelingen](get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)

