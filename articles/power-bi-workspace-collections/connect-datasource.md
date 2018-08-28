---
title: Verbinding maken met een gegevensbron in Power BI Workspace Collections | Microsoft Docs
description: Leer hoe u verbinding maken met een gegevensbron in Power BI Workspace Collections.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: c626601d9eae7732779020b153c624f80605b56a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051872"
---
# <a name="connect-to-a-data-source"></a>Verbinding maken met een gegevensbron

Met **Power BI Workspace Collections**, kunt u rapporten insluiten in uw eigen app. Wanneer u een Power BI-rapport in uw app insluiten, het rapport maakt verbinding met de onderliggende gegevens door **importeren** een kopie van de gegevens of door **rechtstreeks verbinding maken** met de gegevensbron via **DirectQuery** .

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Dit zijn de verschillen tussen **Importeren** en **DirectQuery**.

| Importeren | DirectQuery |
| --- | --- |
| Tabellen, kolommen, *en gegevens* worden geïmporteerd of gekopieerd naar de gegevensset van het rapport. Als u wilt zien wijzigingen zijn doorgevoerd in de onderliggende gegevens, moet u vernieuwen of een volledige, actuele gegevensset opnieuw importeren. |Alleen *tabellen en kolommen* worden geïmporteerd of gekopieerd naar de gegevensset van het rapport. U bekijken altijd de meest recente gegevens. |

Met Power BI Workspace COllections, u kunt gebruikmaken van DirectQuery met cloud-gegevensbron, maar niet on-premises gegevensbronnen op dit moment.

> [!NOTE]
> De On-Premises gegevensgateway wordt niet ondersteund met Power BI Workspace Collections op dit moment. Dit betekent dat u niet DirectQuery gebruiken met on-premises gegevensbronnen.

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen

**DirectQuery**
* Azure SQL-database
* Azure SQL Data Warehouse

**Importeren**

U kunt importeren met alle van de beschikbare gegevensbronnen in Power BI Desktop. U wordt **niet** kunnen gegevens in Power BI Workspace Collections vernieuwen. U hebt voor het uploaden van wijzigingen naar uw PBIX-bestand naar Power BI Workspace Collections. Dit komt door geen beschikbare gateway. 

## <a name="benefits-of-using-directquery"></a>Voordelen van het gebruik van DirectQuery

Er zijn twee belangrijke voordelen bij het gebruik van **DirectQuery**:

* **DirectQuery** kunt u visualisaties maken voor grote gegevenssets, waar dit anders onwerkbaar om naar de eerste import zijn zou alle gegevens.
* Bij onderliggende gegevenswijzigingen kan vereisen een vernieuwing van gegevens en voor sommige rapporten kan de noodzaak tot het weergeven van actuele gegevens grote gegevensoverdrachten om de gegevens opnieuw importeren onwerkbaar om kunt vereist. Daarentegen **DirectQuery** rapporten altijd actuele gegevens gebruikt.

## <a name="limitations-of-directquery"></a>Beperkingen van DirectQuery

Er zijn enkele beperkingen voor het gebruik van **DirectQuery**:

* Alle tabellen moeten afkomstig zijn uit één database.
* Als de query te complex is, wordt er een fout optreedt. Als u wilt de fout te verhelpen moet u de query herstructureren, dus is het minder complex. Als de query complex zijn moet, moet u de gegevens in plaats van importeren **DirectQuery**.
* Filteren van relatie is beperkt tot één richting, in plaats van beide richtingen.
* U kunt het gegevenstype van een kolom niet wijzigen.
* Standaard beperkingen voor DAX-expressies die zijn toegestaan in metingen. Zie [DirectQuery en metingen](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery en metingen
Beperkingen worden opgelegd om ervoor te zorgen voor query's verzonden naar de onderliggende gegevensbron hebben tot aanvaardbare prestaties, voor metingen. Bij het gebruik van **Power BI Desktop**, geavanceerde gebruikers kunnen ervoor kiezen deze beperking omzeilen door te kiezen **bestand > Opties en instellingen > opties**. In de **opties** dialoogvenster kiezen **DirectQuery**, en selecteer de optie **onbeperkte metingen toestaan in de modus DirectQuery**. Wanneer deze optie is geselecteerd, kan elke DAX-expressie die geldig is voor een meting kan worden gebruikt. Gebruikers moeten zich op de hoogte; echter van dat bepaalde expressies die goed presteren wanneer de gegevens worden geïmporteerd tot trage leiden kunnen query's voor de back-end in de gegevensbron **DirectQuery** modus. 

## <a name="see-also"></a>Zie ook

* [Aan de slag met Microsoft Power BI-Werkruimteverzamelingen](get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)

