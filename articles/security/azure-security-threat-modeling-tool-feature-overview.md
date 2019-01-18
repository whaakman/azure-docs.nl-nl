---
title: Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Meer informatie over alle functies die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 601f3bf05388406c8f96a7351f7fb3aa4de2650a
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389329"
---
# <a name="threat-modeling-tool-feature-overview"></a>Functieoverzicht van Threat Modeling Tool

De Threat Modeling Tool kan u helpen bij uw threat modeling behoeften. Zie voor een eenvoudige Inleiding tot het hulpprogramma, [aan de slag met de Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md).

> [!NOTE]
>De Threat Modeling Tool regelmatig wordt bijgewerkt, Raadpleeg deze handleiding vaak als u wilt zien van onze nieuwste functies en verbeteringen.

Als u wilt openen op een lege pagina, selecteer **maakt een Model**.

![Lege pagina](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

Als de functies die momenteel beschikbaar in het hulpprogramma weergeven, gebruikt u de risicomodel die zijn gemaakt door ons team in de [aan de slag](./azure-security-threat-modeling-tool-getting-started.md) voorbeeld.

![Basic risicomodel](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

## <a name="navigation"></a>Navigatie

Voordat we de ingebouwde functies bespreken, laten we bekijken de belangrijkste onderdelen die zijn gevonden in het hulpprogramma.

### <a name="menu-items"></a>Menu-items

De ervaring is vergelijkbaar met andere Microsoft-producten. Laten we bekijken de op het hoogste niveau menu-items.

![Menu-items](./media/azure-security-threat-modeling-tool-feature-overview/menuitems.png)

| Label                               | Details      |
| --------------------------------------- | ------------ |
| **File** | <ul><li>Bestanden openen, opslaan en sluiten</li><li>Aanmelden en afmelden bij OneDrive-accounts.</li><li>Deel koppelingen (weergeven en bewerken).</li><li>Informatie weergeven.</li><li>Een nieuwe sjabloon toepassen op bestaande modellen.</li></ul> |
| **Edit** | Ongedaan maken en opnieuw uitvoeren van acties zoals kopiëren, plakken en verwijderen. |
| **Weergave** | <ul><li>Schakelen tussen **Analysis** en **ontwerp** weergaven.</li><li>Open gesloten windows (zo stencils, eigenschappen van het element en berichten).</li><li>Indeling opnieuw instellen op de standaardinstellingen.</li></ul> |
| **Diagram** | Toevoegen en verwijderen van diagrammen en schakelen tussen tabbladen diagrammen. |
| **Rapporten** | HTML-rapporten maken om met anderen delen. |
| **Help** | Gidsen voor het gebruik van het hulpprogramma vinden. |

De symbolen zijn snelkoppelingen voor de menu's in op het hoogste niveau:

| Symbool                               | Details      |
| --------------------------------------- | ------------ |
| **openen** | Een nieuw bestand wordt geopend. |
| **Opslaan** | Hiermee slaat u het huidige bestand. |
| **Ontwerp** | Hiermee opent u de **ontwerp** weergeven, waarin u modellen kunt maken. |
| **Analyseren** | Toont gegenereerd bedreigingen en de bijbehorende eigenschappen. |
| **Diagram toevoegen** | Voegt een nieuw diagram (vergelijkbaar met de nieuwe tabbladen in Excel). |
| **Diagram verwijderen** | Hiermee verwijdert u het huidige diagram. |
| **Knippen/kopiëren/plakken** | Kopieert, delen en elementen worden geplakt. |
| **Ongedaan maken/opnieuw uitvoeren** | Hiermee wordt ongedaan gemaakt en opnieuw uitvoeren van acties. |
| **Zoom in / uitzoomen** | Zoomt in en uit het diagram voor een betere weergave. |
| **Feedback** | Hiermee opent u de MSDN-Forum. |

### <a name="canvas"></a>Canvas

Het canvas is de ruimte waar u slepen en neerzetten elementen. Slepen en neerzetten is de snelste en meest efficiënte manier om modellen te bouwen. U kunt ook met de rechtermuisknop op en selecteert u items in het menu om toe te voegen algemene versies van elementen, zoals wordt weergegeven:

#### <a name="drop-the-stencil-on-the-canvas"></a>Het stencil neerzetten op het canvas

![Canvas neerzetten](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Selecteer het stencil

![Eigenschappen van het element](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop2.png)

### <a name="stencils"></a>Stencils

Op basis van de sjabloon die u selecteert, kunt u de beschikbaar voor gebruik stencils vinden. Als u niet de juiste elementen kunt vinden, gebruikt u een andere sjabloon. Of u kunt een sjabloon aanpassen aan uw behoeften aanpassen. U vindt in het algemeen een combinatie van categorieën zoals deze:

| Naam van stencil                               | Details      |
| --------------------------------------- | ------------ |
| **Proces** | Toepassingen, browser-invoegtoepassingen, threads, virtuele machines |
| **Externe interactie** | Verificatieproviders, browsers, gebruikers, web-apps |
| **Gegevensarchief** | Cache, opslag, configuratiebestanden, databases, register |
| **Gegevensstroom** | Binaire ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, met de naam van de pipe, RPC/DCOM, SMB, UDP |
| **Lijnrand/grens vertrouwen** | Bedrijfsnetwerken, internet, machine, sandbox-, gebruiker/kernel-modus |

### <a name="notesmessages"></a>Opmerkingen/berichten

| Onderdeel                               | Details      |
| --------------------------------------- | ------------ |
| **Messages** | Interne hulpprogramma logica waarmee gebruikers een waarschuwing wanneer er een fout is opgetreden, zoals geen gegevensstromen tussen de elementen. |
| **Opmerkingen** | Opmerkingen bij de handmatige worden toegevoegd aan het bestand door technische teams overal in de ontwerp- en evaluatieprocedure. |

### <a name="element-properties"></a>Eigenschappen van het element

Eigenschappen van het element zijn afhankelijk van de elementen die u selecteert. Naast de grenzen van vertrouwensrelaties bevatten alle andere elementen drie algemene instellingen:

| De eigenschap element                               | Details      |
| --------------------------------------- | ------------ |
| **Naam** | Dit is handig voor het benoemen van uw processen, winkels, interacties en stromen, zodat ze eenvoudig worden herkend. |
| **Buiten het bereik** | Indien geselecteerd, wordt het element uit (niet aanbevolen) in de matrix threat generatie gehaald. |
| **Reden voor het buiten het bereik** | Reden is om gebruikers te informeren waarom buiten het bereik ingeschakeld. |

Eigenschappen worden gewijzigd in elke categorie element. Selecteer voor elk element in om te controleren van de beschikbare opties. Of u kunt de sjabloon voor meer informatie. Laten we de functies bekijken.

## <a name="welcome-screen"></a>Welkomstscherm wordt weergegeven

Wanneer u de app opent, ziet u de **Welkom** scherm.

### <a name="open-a-model"></a>Een model openen

Beweeg de muisaanwijzer over **Open een Model** om twee opties weer te geven: **Open vanaf deze Computer** en **openen vanuit OneDrive**. De eerste optie, verschijnt de **bestand openen** scherm. De tweede optie doorloopt u de aanmeldprocedure voor OneDrive. Na een geslaagde authenticatie, kunt u mappen en bestanden.

![Model openen](./media/azure-security-threat-modeling-tool-feature-overview/openmodel.png)

![Openen vanaf de computer of OneDrive](./media/azure-security-threat-modeling-tool-feature-overview/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Feedback, suggesties en problemen

Wanneer u selecteert **Feedback, suggesties en problemen**, u gaat u naar de MSDN-Forum voor SDL-hulpprogramma's. U kunt lezen wat anderen zeggen over het hulpprogramma, met inbegrip van oplossingen en nieuwe ideeën.

![Feedback](./media/azure-security-threat-modeling-tool-feature-overview/feedback.png)

## <a name="design-view"></a>Weergave voor het ontwerpen

Wanneer u opent of een nieuw model maken de **ontwerp** weer wordt geopend.

### <a name="add-elements"></a>Elementen toevoegen

U kunt elementen in het raster op twee manieren toevoegen:

- **Slepen en neerzetten**: Sleep het gewenste element aan het raster. De eigenschappen van het element vervolgens gebruiken om aanvullende informatie te geven.
- **Met de rechtermuisknop op**: Klik met de rechtermuisknop op het raster en selecteert u items in de vervolgkeuzelijst. Een algemene weergave van het element dat u selecteert, wordt weergegeven op het scherm.

### <a name="connect-elements"></a>Verbinding maken met elementen

U kunt verbinding maken met elementen op twee manieren:

- **Slepen en neerzetten**: Sleep de gewenste gegevensstroom naar het raster en beide kanten verbinden met de juiste elementen.
- **Klik op + Shift**: Klik op het eerste element (verzenden van gegevens), houd de Shift-toets ingedrukt en selecteer vervolgens het tweede element (ontvangen van gegevens). Met de rechtermuisknop op en selecteer **Connect**. Als u een bidirectionele-gegevensstroom, is de volgorde niet zo belangrijk.

### <a name="properties"></a>Properties

 Als u wilt zien van de eigenschappen die kunnen worden gewijzigd op de stencils, selecteer het stencil en de informatie wordt dienovereenkomstig wordt uitgevoerd. Het volgende voorbeeld ziet u voor en na een **Database** stencil naar het diagram is gesleept:

#### <a name="before"></a>Voor

![Voor](./media/azure-security-threat-modeling-tool-feature-overview/properties1.png)

#### <a name="after"></a>Na

![Na](./media/azure-security-threat-modeling-tool-feature-overview/properties2.png)

### <a name="messages"></a>Berichten

Als u een risicomodel maakt en verbinding maken met gegevensstromen elementen vergeet, ontvangt u een melding. U kunt dit bericht negeren of u kunt de instructies om het probleem te verhelpen. 

![Berichten](./media/azure-security-threat-modeling-tool-feature-overview/messages.png)

### <a name="notes"></a>Opmerkingen

Notities toevoegen aan uw diagram, overschakelen van de **berichten** tab naar het **opmerkingen bij de** tabblad.

## <a name="analysis-view"></a>Analyse weergeven

Nadat u uw diagram bouwt, selecteert u de **Analysis** symbool (Vergrootglas) op de werkbalk snelkoppelingen overschakelen naar de **Analysis** weergeven.

![Analyse weergeven](./media/azure-security-threat-modeling-tool-feature-overview/analysisview.png)

### <a name="generated-threat-selection"></a>Gegenereerde threat selectie

Wanneer u een bedreiging selecteert, kunt u drie afzonderlijke functies:

| Functie                               | Informatie      |
| --------------------------------------- | ------------ |
| **Lezen-indicator** | <p>De bedreiging is gemarkeerd als gelezen, waarmee u de items die u bekeken.</p><p>![Lezen/ongelezen indicator](./media/azure-security-threat-modeling-tool-feature-overview/readmode.png)</p> |
| **Interactie focus** | <p>Interactie in het diagram die deel uitmaakt van een bedreiging wordt gemarkeerd.</p><p>![Interactie focus](./media/azure-security-threat-modeling-tool-feature-overview/interactionfocus.png)</p> |
| **Eigenschappen van de bedreiging** | <p>Als u meer informatie over de bedreiging wordt weergegeven in de **Threat eigenschappen** venster.</p><p>![Eigenschappen van de bedreiging](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)</p> |

### <a name="priority-change"></a>Prioriteit wijzigen

U kunt de prioriteit van elke gegenereerde bedreiging wijzigen. Verschillende kleuren maken het gemakkelijk te herkennen, hoog, gemiddeld en bedreigingen met lage prioriteit.

![Prioriteit wijzigen](./media/azure-security-threat-modeling-tool-feature-overview/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Threat eigenschappen bewerkbare velden

Zoals in de voorgaande afbeelding wordt weergegeven, kunt u de informatie die wordt gegenereerd door het hulpprogramma kunt wijzigen. U kunt ook informatie toevoegen aan bepaalde velden, zoals reden. Deze velden worden gegenereerd door de sjabloon. Als u meer informatie voor elke bedreiging nodig hebt, kunt u wijzigingen aanbrengen.

![Eigenschappen van de bedreiging](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)

## <a name="reports"></a>Rapporten

Nadat u klaar bent voor veranderende prioriteiten in uw en het bijwerken van de status van elke gegenereerde bedreiging, u kunt het bestand opslaan en/of afdrukken. Ga naar **rapport** > **volledige rapport maken**. Het rapport een naam en u moet er ongeveer uitzien als in de volgende afbeelding:

![Rapport](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

## <a name="next-steps"></a>Volgende stappen

- Stuur uw vragen, opmerkingen en problemen van tmtextsupport@microsoft.com. **[Download](https://aka.ms/threatmodelingtool)**  Threat Modeling Tool aan de slag.
- Als u wilt een sjabloon voor de community bijdragen, gaat u naar onze [GitHub](https://github.com/Microsoft/threat-modeling-templates) pagina.