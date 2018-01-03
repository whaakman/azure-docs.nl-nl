---
title: Microsoft Threat Modeling hulpprogramma - Azure | Microsoft Docs
description: Meer informatie over alle functies die beschikbaar zijn in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 76e49fc0e680acbc2b7b7c62b69fbf72d6690acf
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Overzicht van Threat Modeling hulpprogramma-functies

Het hulpprogramma Threat Modeling kan u helpen bij uw threat modeling behoeften. Zie voor een algemene inleiding tot voor het hulpprogramma [aan de slag met het hulpprogramma Threat Modeling](./azure-security-threat-modeling-tool-getting-started.md).

> [!NOTE]
>Het hulpprogramma Threat Modeling vaak wordt bijgewerkt, dus deze handleiding vaak als u wilt zien van onze nieuwste functies en verbeteringen.

Als u een lege pagina, schakelt **een Model maken**.

![Lege pagina](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

Als de functies die momenteel beschikbaar in het hulpprogramma wilt weergeven, gebruikt u de risicomodel gemaakt door ons team in het [aan de slag](./azure-security-threat-modeling-tool-getting-started.md) voorbeeld.

![Basic risicomodel](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

## <a name="navigation"></a>Navigatie

Voordat we de ingebouwde functies bespreken, gaan we de belangrijkste onderdelen gevonden in het hulpprogramma te bekijken.

### <a name="menu-items"></a>Menu-items

De ervaring is vergelijkbaar met andere Microsoft-producten. We bekijken op het hoogste niveau menu-items.

![Menu-items](./media/azure-security-threat-modeling-tool-feature-overview/menuitems.png)

| Label                               | Details      |
| --------------------------------------- | ------------ |
| **File** | <ul><li>Bestanden openen, opslaan en sluiten</li><li>Aanmelden en afmelden bij OneDrive-accounts.</li><li>Koppelingen (weergeven en bewerken) delen.</li><li>Informatie weergeven.</li><li>Een nieuwe sjabloon toepassen op bestaande modellen.</li></ul> |
| **Bewerken** | Ongedaan maken en opnieuw uitvoeren van acties zoals kopiëren, plakken, en verwijder. |
| **Weergave** | <ul><li>Schakelen tussen **Analysis** en **ontwerp** weergaven.</li><li>Open gesloten windows (bijvoorbeeld stencils, eigenschappen van het element en berichten).</li><li>Lay-out standaardinstellingen herstellen.</li></ul> |
| **Diagram** | Schakelen tussen tabbladen diagrammen toevoegen en verwijderen diagrammen. |
| **Rapporten** | HTML-rapporten maken om te delen met anderen. |
| **Help** | Handleidingen voor het gebruik van het hulpprogramma vinden. |

De symbolen zijn sneltoetsen voor de site op het hoogste menu's:

| Symbool                               | Details      |
| --------------------------------------- | ------------ |
| **Open** | Hiermee opent u een nieuw bestand. |
| **Opslaan** | Hiermee slaat u het huidige bestand. |
| **Ontwerp** | Hiermee opent u de **ontwerp** weergeven, waarin u modellen kunt maken. |
| **Analyseren** | Toont gegenereerd bedreigingen en hun eigenschappen. |
| **Diagram toevoegen** | Voegt een nieuw diagram (vergelijkbaar met de nieuwe tabbladen in Excel). |
| **Diagram verwijderen** | Hiermee verwijdert u het huidige diagram. |
| **Knippen/kopiëren/plakken** | Kopieert en plakt elementen knippen. |
| **Ongedaan maken/opnieuw uitvoeren** | Hiermee wordt ongedaan gemaakt en opnieuw uitvoeren van acties. |
| **Inzoomen- / uitzoomen** | Zoomt in en uit het diagram voor een beter beeld. |
| **Feedback** | Hiermee opent u de MSDN-Forum. |

### <a name="canvas"></a>Canvas

Het canvas is de ruimte waar u slepen en neerzetten van elementen. Slepen en neerzetten is de snelste en meest efficiënte manier om modellen te maken. U kunt ook met de rechtermuisknop op en selecteer de items in het menu toevoegen algemene versies van elementen, zoals wordt weergegeven:

#### <a name="drop-the-stencil-on-the-canvas"></a>Het stencil op het canvas te verwijderen

![Canvas neerzetten](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Selecteer het stencil

![Eigenschappen van het element](./media/azure-security-threat-modeling-tool-feature-overview/canvasdrop2.png)

### <a name="stencils"></a>Stencils

Op basis van de sjabloon die u selecteert, kunt u de beschikbaar voor gebruik stencils vinden. Als u de juiste elementen niet vinden, gebruikt u een andere sjabloon. Of u een sjabloon aanpassen aan uw behoeften kunt wijzigen. In het algemeen kunt u een combinatie van categorieën dergelijke vinden:

| Stencilnaam                               | Details      |
| --------------------------------------- | ------------ |
| **Proces** | Toepassingen, browserinvoegtoepassingen, threads, virtuele machines |
| **Externe interactie** | Verificatieproviders, browsers, gebruikers, webtoepassingen |
| **Gegevensopslag** | Cache, opslag, configuratiebestanden, databases, register |
| **Gegevensstroom** | Binair ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, benoemde pijp, RPC/DCOM, SMB, UDP |
| **Lijn grens vertrouwen** | Bedrijfsnetwerken, internet, machine, sandbox, gebruiker/kernelmodus |

### <a name="notesmessages"></a>Opmerkingen bij de/messages

| Onderdeel                               | Details      |
| --------------------------------------- | ------------ |
| **Berichten** | Interne hulpprogramma logica die gebruikers waarschuwt wanneer er een fout optreedt, zoals geen gegevensoverdrachten tussen elementen. |
| **Opmerkingen bij de** | Handmatige opmerkingen worden toegevoegd aan het bestand door engineering-teams overal in het ontwerp en evaluatieproces. |

### <a name="element-properties"></a>Eigenschappen van het element

Eigenschappen van het element is afhankelijk van de elementen die u selecteert. Alle andere elementen bevatten naast de grenzen van vertrouwensrelaties drie algemene selecties:

| Eigenschap element                               | Details      |
| --------------------------------------- | ------------ |
| **Naam** | Dit is handig voor de naamgeving van uw processen, archieven, interacties en stromen, zodat ze gemakkelijk wordt herkend. |
| **Buiten het bereik** | Indien geselecteerd, wordt het element genomen buiten de threat generatie-matrix (niet aanbevolen). |
| **Reden voor buiten het bereik** | Reden is gebruikers laten weten waarom buiten het bereik ingeschakeld. |

Eigenschappen worden onder elke categorie element gewijzigd. Selecteer voor elk element in de beschikbare opties controleren. Of u kunt de sjabloon voor meer informatie. Laten we de functies bekijken.

## <a name="welcome-screen"></a>Welkomstscherm

Wanneer u de app opent, ziet u de **Welkom** scherm.

### <a name="open-a-model"></a>Een model openen

Beweeg de muisaanwijzer over **Open een Model** blijkt dat er twee opties: **openen vanaf deze Computer** en **Open van OneDrive**. De eerste optie, verschijnt de **bestand openen** scherm. De tweede optie gaat u door het proces aanmelden voor OneDrive. Wanneer u bent geverifieerd, kunt u mappen en bestanden.

![Model openen](./media/azure-security-threat-modeling-tool-feature-overview/openmodel.png)

![Openen van de computer of OneDrive](./media/azure-security-threat-modeling-tool-feature-overview/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Feedback, suggesties en problemen

Wanneer u selecteert **Feedback, suggesties en problemen**, u gaat u naar de MSDN-Forum voor SDL-hulpprogramma's. U kunt lezen wat anderen hierover het hulpprogramma, inclusief tijdelijke oplossingen en nieuwe ideeën.

![Feedback](./media/azure-security-threat-modeling-tool-feature-overview/feedback.png)

## <a name="design-view"></a>Ontwerpweergave

Bij het openen of maken van een nieuw model, de **ontwerp** weergeven wordt geopend.

### <a name="add-elements"></a>Elementen toevoegen

U kunt de elementen in het raster op twee manieren toevoegen:

- **Slepen en neerzetten**: Sleep het gewenste element aan het raster. De eigenschappen van het element vervolgens gebruiken om aanvullende informatie te geven.
- **Met de rechtermuisknop op**: klik met de rechtermuisknop op het raster en selecteren van items uit de vervolgkeuzelijst. Een algemene representatie van het element dat u selecteert weergegeven op het scherm.

### <a name="connect-elements"></a>Elementen verbinden

U kunt de verbinding van elementen op twee manieren maken:

- **Slepen en neerzetten**: Sleep de gewenste gegevensstroom naar het raster en beide uiteinden verbinding met de juiste elementen.
- **Klik op + Shift**: klik op het eerste element (het versturen van gegevens), houd de Shift-toets, en selecteer vervolgens het tweede element (ontvangen van gegevens). Met de rechtermuisknop en selecteer **Connect**. Als u een bidirectionele gegevensstroom gebruikt, is de volgorde niet als belangrijk.

### <a name="properties"></a>Eigenschappen

 Vult dienovereenkomstig om de eigenschappen die kunnen worden gewijzigd op de stencils, selecteer het stencil en de informatie te bekijken. Het volgende voorbeeld ziet voor en na een **Database** stencil wordt gesleept naar het diagram:

#### <a name="before"></a>Voor

![Voor](./media/azure-security-threat-modeling-tool-feature-overview/properties1.png)

#### <a name="after"></a>Na

![Na](./media/azure-security-threat-modeling-tool-feature-overview/properties2.png)

### <a name="messages"></a>Berichten

Als u een risicomodel maakt en vergeet gegevensoverdrachten verbinding maken met de elementen, krijgt u een melding. U kunt dit bericht negeren of u kunt de instructies om het probleem te verhelpen. 

![Berichten](./media/azure-security-threat-modeling-tool-feature-overview/messages.png)

### <a name="notes"></a>Opmerkingen

Als u opmerkingen toevoegen aan het diagram, wilt overschakelen van de **berichten** tab naar de **notities** tabblad.

## <a name="analysis-view"></a>Analyse weergeven

Nadat u het diagram bouwen, selecteert u de **Analysis** symbool (Vergrootglas) op de werkbalk snelkoppelingen overschakelen naar de **Analysis** weergeven.

![Analyse weergeven](./media/azure-security-threat-modeling-tool-feature-overview/analysisview.png)

### <a name="generated-threat-selection"></a>Gegenereerde threat selectie

Wanneer u een bedreiging selecteert, kunt u drie afzonderlijke functies:

| Functie                               | Informatie      |
| --------------------------------------- | ------------ |
| **Lees indicator** | <p>De bedreiging is gemarkeerd als gelezen, waarmee u de items die u gecontroleerd.</p><p>![Lezen/ongelezen indicator](./media/azure-security-threat-modeling-tool-feature-overview/readmode.png)</p> |
| **Interactie focus** | <p>Interactie in het diagram die deel uitmaakt van een bedreiging wordt gemarkeerd.</p><p>![Interactie focus](./media/azure-security-threat-modeling-tool-feature-overview/interactionfocus.png)</p> |
| **Eigenschappen van de Threat** | <p>Meer informatie over de bedreiging wordt weergegeven in de **Threat eigenschappen** venster.</p><p>![Eigenschappen van de Threat](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)</p> |

### <a name="priority-change"></a>Prioriteit wijzigen

U kunt het prioriteitsniveau van elke gegenereerde bedreiging wijzigen. Verschillende kleuren maakt het eenvoudig is te hoog, gemiddeld en prioriteit Laag bedreigingen identificeren.

![Prioriteit wijzigen](./media/azure-security-threat-modeling-tool-feature-overview/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Threat eigenschappen bewerkbare velden

Zoals u in de voorgaande afbeelding, kunt u de gegevens die worden gegenereerd door het hulpprogramma. U kunt ook informatie toevoegen aan bepaalde velden, zoals reden. Deze velden zijn gegenereerd door de sjabloon. Als u meer informatie nodig voor elke bedreiging, kunt u wijzigingen kunt aanbrengen.

![Eigenschappen van de Threat](./media/azure-security-threat-modeling-tool-feature-overview/threatproperties.png)

## <a name="reports"></a>Rapporten

Nadat u prioriteiten wijzigen en bijwerken van de status van elke gegenereerde bedreiging, u kunt het bestand opslaan en/of afdrukken. Ga naar **rapport** > **volledige rapport maken**. Het rapport een naam en ziet u iets soortgelijks als in de volgende afbeelding:

![Rapport](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

## <a name="next-steps"></a>Volgende stappen

* Als u wilt een sjabloon voor de community bijdragen, gaat u naar onze [GitHub](https://github.com/Microsoft/threat-modeling-templates) pagina. 
* Als u wilt beginnen met het hulpprogramma, gaat u naar de [downloaden](https://aka.ms/tmtpreview) pagina.
