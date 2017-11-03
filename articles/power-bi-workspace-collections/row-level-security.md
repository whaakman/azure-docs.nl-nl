---
title: Beveiliging op rijniveau met Power BI werkruimte verzamelingen
description: Meer informatie over beveiliging met Power BI werkruimte verzamelingen
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 8c3ce8bc69a098d3133f27a2604f9d564693ea54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="row-level-security-with-power-bi-workspace-collections"></a>Beveiliging op rijniveau met Power BI werkruimte verzamelingen

Beveiliging op rijniveau (RLS) kan gebruikerstoegang te beperken tot bepaalde gegevens binnen een rapport of de gegevensset, zodat meerdere verschillende gebruikers gebruiken hetzelfde rapport tijdens het weergeven van alle andere gegevens worden gebruikt. Power BI werkruimte verzamelingen bieden ondersteuning voor gegevenssets die zijn geconfigureerd voor beveiliging op Rijniveau.

![Stroom van beveiliging op rijniveau in Power BI werkruimte verzamelingen](media/row-level-security/flow-1.png)

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Om te profiteren van RLS, is het belangrijk dat u begrijpt dat drie belangrijkste concepten; Gebruikers, rollen en -regels. Laten we dichter bij elke:

**Gebruikers** : deze worden de werkelijke eindgebruikers weergeven van rapporten. In Power BI werkruimte verzamelingen worden gebruikers geïdentificeerd door de eigenschap username in een App-Token.

**Rollen** – gebruikers tot functies behoren. Een rol is een container voor regels en kan de naam zoals 'Sales Manager' of 'Vertegenwoordiger'. In Power BI werkruimte verzamelingen worden gebruikers geïdentificeerd door de eigenschap rollen in een App-Token.

**Regels** : rollen regels hebben en deze regels zijn de werkelijke filters die u op de gegevens worden toegepast. Dit wordt mogelijk net zo eenvoudig als ' land Verenigde Staten = ' of iets meer dynamische.

### <a name="example"></a>Voorbeeld

Voor de rest van dit artikel bieden we een voorbeeld van RLS ontwerpen en verbruikt die in een ingesloten toepassing. Dit voorbeeld wordt de [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-bestand.

![Voorbeeld verkooprapport](media/row-level-security/scenario-2.png)

Onze Retail Analysis sample verkoopcijfers voor alle winkels in een bepaalde winkelketen. Ze zien dezelfde gegevens zonder RLS, ongeacht welke regionale manager zich aanmeldt en bekijkt het rapport. Senior management heeft elke regionale manager ziet alleen de verkoop voor de winkels die ze beheren en om dit te doen, gebruiken we RLS vastgesteld.

RLS is geschreven in Power BI Desktop. Wanneer de gegevensset en het rapport wordt geopend, kunnen we overschakelen naar diagram weer te geven van het schema:

![Modeldiagram van het in Power BI Desktop](media/row-level-security/diagram-view-3.png)

Hier volgen een aantal dingen opvalt met dit schema:

* Alle maateenheden zoals **totale verkoop**, worden opgeslagen in de **verkoop** feitentabel.
* Er zijn vier extra gerelateerde dimensietabellen: **Item**, **tijd**, **Store**, en **regionale**.
* De pijlen op de relatie regels aangeven welke filters van één tabel naar een andere stromen kunnen manier. Bijvoorbeeld, als een filter is geplaatst **tijd [datum]**, in het huidige schema deze alleen filtert u de waarden in de **verkoop** tabel. Er zijn geen andere tabellen wordt beïnvloed door dit filter omdat alle van de pijlen op de relatie regels naar de tabel sales en niet verwijderd verwijzen.
* De **regionale** tabel geeft aan wie de manager is voor elk:
  
  ![Regionale tabelrijen](media/row-level-security/district-table-4.png)

Op basis van dit schema als we een filter toepassen op de **regionale Manager** kolom in de tabel regionale en als dat filter overeenkomt met de gebruiker het rapport weer te geven, die filteren ook filters omlaag de **Store** en  **Verkoop** tabellen alleen gegevens weergeven die voor die specifieke regionale manager.

Hier ziet u hoe:

1. Klik op het tabblad modellering **rollen beheren**.  
   ![Knop in het lint modelleren rollen beheren](media/row-level-security/modeling-tab-5.png)
2. Maak een nieuwe rol aangeroepen **Manager**.  
   ![Het maken van rollen in Power BI Desktop](media/row-level-security/manager-role-6.png)
3. In de **regionale** tabel voert de volgende DAX-expressie: **[regionale Manager] USERNAME() =**  
   ![DAX-filterexpressie voor de tabel in de rol](media/row-level-security/manager-role-7.png)
4. Om ervoor te zorgen dat de regels werkt, op de **modelleren** tabblad **weergave als rollen**, en voer de volgende:  
   ![Als functies weergeven](media/row-level-security/view-as-roles-8.png)

   Gegevens worden nu door de rapporten worden weergegeven alsof u bent aangemeld als **Andrew Ma**.

Filter toe te passen, de manier waarop werkwijze, filtert u alle records in de **regionale**, **Store**, en **verkoop** tabellen. Echter, vanwege de Filterrichting van de relaties tussen **verkoop** en **tijd**, **verkoop** en **Item**, en **Item** en **tijd** tabellen worden niet gefilterd omlaag.

![Diagramweergave relaties die zijn gemarkeerd](media/row-level-security/diagram-view-9.png)

Die mogelijk voor deze vereiste ok, echter als we managers items waarvoor ze niet beschikken over een verkopen zien, niet wilt dat we kan bidirectionele cross-filteren voor de relatie inschakelen en het beveiligingsfilter in beide richtingen stromen. Dit kan worden gedaan door het bewerken van de relatie tussen **verkoop** en **Item**, zoals deze:

![Cross-Filterrichting voor de relatie](media/row-level-security/edit-relationship-10.png)

Filters kunnen nu ook stromen van de verkoop-tabel om de **Item** tabel:

![Pictogram van relatie in de diagramweergave richting filteren](media/row-level-security/diagram-view-11.png)

> [!NOTE]
> Als u de modus DirectQuery voor uw gegevens, moet u inschakelen bidirectionele cross filteren op deze twee opties selecteren:

1. **Bestand** -> **opties en instellingen** -> **Voorbeeldfuncties** -> **inschakelen kruislings filteren in beide richtingen voor DirectQuery**.
2. **Bestand** -> **opties en instellingen** -> **DirectQuery** -> **onbeperkte meting in DirectQuery-modus toestaan**.

Voor meer informatie over bidirectionele cross-filtering, downloaden de [bidirectionele cross-filteren in SQL Server Analysis Services 2016 en Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) technisch document.

Dit wordt afgerond al het werk dat moet worden gedaan in Power BI Desktop, maar er is een meer stuk werk dat worden gedaan moet om de RLS regels dat hebben we werken in Power BI Embedded gedefinieerd. Gebruikers worden geverifieerd en geautoriseerd door uw toepassing en App-tokens worden gebruikt voor die gebruikerstoegang verlenen tot een specifiek Power BI Embedded rapport. Power BI Embedded geen specifieke gegevens die op die de gebruiker is. U moet enkele aanvullende context doorgeven als onderdeel van uw app-token voor RLS werken:

* **gebruikersnaam** (optioneel) – gebruikt voor beveiliging op Rijniveau dit is een tekenreeks die kan worden gebruikt om vast te stellen van de gebruiker bij het toepassen van RLS regels. Zie rij beveiliging op rijniveau met Power BI Embedded
* **rollen** : een tekenreeks met de rollen selecteren bij het toepassen van regels voor beveiliging op rijniveau. Als meer dan één rol wordt doorgegeven, moeten worden doorgegeven als een tekenreeksmatrix.

Maken van het token met behulp van de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) methode. Als de eigenschap username aanwezig is, moet u ook ten minste één waarde doorgeven in rollen.

U kan bijvoorbeeld de EmbedSample wijzigen. DashboardController regel 55 kan worden bijgewerkt vanuit

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

tot

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

Het volledige app-token ziet er ongeveer als volgt uit:

![JSON web token voorbeeld](media/row-level-security/app-token-string-12.png)

Nu met alle benodigde onderdelen samen zien wanneer iemand zich aan onze toepassing aanmeldt voor dit rapport bekijken ze de gegevens die ze zien, kunnen zoals gedefinieerd door onze beveiliging.

![Rapport wordt weergegeven in de toepassing](media/row-level-security/dashboard-13.png)

## <a name="see-also"></a>Zie ook

[Beveiliging (RLS) met Power](https://powerbi.microsoft.com/documentation/powerbi-admin-rls/)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)