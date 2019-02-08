---
title: Beveiliging op rijniveau met Power BI-werkruimteverzamelingen
description: Meer informatie over de beveiliging op rijniveau met Power BI-Werkruimteverzamelingen
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: f8a5c12bb57a8f59960320c6227174b240bcbc3d
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892232"
---
# <a name="row-level-security-with-power-bi-workspace-collections"></a>Beveiliging op rijniveau met Power BI-werkruimteverzamelingen

Beveiliging op rijniveau (RLS) kan worden gebruikt om toegang van gebruikers beperken tot bepaalde gegevens in een rapport of gegevensset, zodat voor verschillende gebruikers hetzelfde rapport terwijl er verschillende gegevens. Power BI Workspace Collections ondersteuning voor gegevenssets die zijn geconfigureerd met beveiliging op Rijniveau.

![Stroom van beveiliging op rijniveau in Power BI-Werkruimteverzamelingen](media/row-level-security/flow-1.png)

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Om te profiteren van RLS, is het belangrijk dat u drie hoofdbegrippen; Gebruikers, rollen en regels. We nemen een items eens nader bekijken:

**Gebruikers** : dit zijn de werkelijke eindgebruikers weergeven van rapporten. In Power BI Workspace Collections, worden gebruikers geïdentificeerd door de eigenschap username in een App-Token.

**Rollen** : gebruikers behoren tot rollen. Een rol is een container voor regels en kan namen hebben, zoals 'Sales Manager' of 'Vertegenwoordiger'. In Power BI Workspace Collections, worden gebruikers geïdentificeerd door de eigenschap van de rollen in een App-Token.

**Regels** : rollen bevatten regels, en deze regels zijn de werkelijke filters die worden toegepast op de gegevens. Dit kan worden net zo eenvoudig als "land = Nederland" of ook veel ingewikkelder.

### <a name="example"></a>Voorbeeld

Voor de rest van dit artikel bieden we een voorbeeld van het ontwerpen van beveiliging op Rijniveau en gebruik binnen een ingesloten toepassing. Dit voorbeeld gebruiken de [Retail Analysis Sample](https://go.microsoft.com/fwlink/?LinkID=780547) PBIX-bestand.

![Voorbeeld-verkooprapport](media/row-level-security/scenario-2.png)

Onze Retailanalyse als voorbeeld worden de verkoopcijfers voor alle winkels van een bepaalde winkelketen. Ze zien dezelfde gegevens zonder RLS zou elke regiomanager die zich aanmeldt en het rapport weergeeft. Senior management heeft besloten dat elke regiomanager ziet alleen de verkoopcijfers voor de winkels die ze beheren en om dit te doen, kunnen we RLS gebruiken.

RLS is geschreven in Power BI Desktop. Wanneer de gegevensset en het rapport wordt geopend, kunnen we overschakelen naar de diagramweergave van het schema:

![Modeldiagram van het in Power BI Desktop](media/row-level-security/diagram-view-3.png)

Hier volgen enkele dingen die opvallen in dit schema:

* Alle metingen, zoals **totale verkoop**, worden opgeslagen in de **verkoop** feitentabel.
* Er zijn vier extra gerelateerde dimensietabellen: **Item**, **tijd**, **Store**, en **District**.
* De pijlen op de relatielijnen aangeven welke filters van de ene tabel naar een andere stromen kunnen manier. Bijvoorbeeld, als een filter wordt belast **tijd [datum]**, in het huidige schema deze alleen filtert u de waarden in de **verkoop** tabel. Er zijn geen andere tabellen worden niet beïnvloed door dit filter omdat alle pijlen op de relatielijnen naar de tabel verkoop en niet ervandaan verwijzen.
* De **District** tabel wordt aangegeven wie de manager voor elk district is:
  
  ![Tabelrijen district](media/row-level-security/district-table-4.png)

Op basis van dit schema als we een filter toepassen op de **District Manager** kolom in de tabel District en als dat filter overeenkomt met de gebruiker die het rapport weergeeft, die filteren ook filters de **Store** en  **Verkoop** tabellen alleen gegevens weergeven voor die bepaalde district manager.

Dit doet u als volgt:

1. Klik op het tabblad modellering **rollen beheren**.  
   ![Beheren van rollen knop in het lint model maken](media/row-level-security/modeling-tab-5.png)
2. Maak een nieuwe rol met de naam **Manager**.  
   ![Het maken van rollen in Power BI Desktop](media/row-level-security/manager-role-6.png)
3. In de **District** tabel voert de volgende DAX-expressie: **[regiomanager] = USERNAME()**  
   ![DAX-filterexpressie voor de tabel in de rol](media/row-level-security/manager-role-7.png)
4. Om ervoor te zorgen de regels correct functioneren, op de **modelleren** tabblad **als rollen weergeven**, en voer het volgende:  
   ![Als rollen weergeven](media/row-level-security/view-as-roles-8.png)

   De rapporten worden de gegevens weergegeven alsof u bent aangemeld als **Andrew Ma**.

Filter toe te passen, de manier waarop we hier hebben gedaan, filtert u alle records in de **District**, **Store**, en **verkoop** tabellen. Echter, vanwege de Filterrichting van de relaties tussen **verkoop** en **tijd**, **verkoop** en **Item**, en **Item** en **tijd** tabellen wordt niet gefilterd.

![Diagramweergave met relaties die zijn gemarkeerd](media/row-level-security/diagram-view-9.png)

Die ok voor deze vereiste kan worden, als we niet wilt dat managers als items waarvoor ze geen omzet wilt bekijken, we kan in twee richtingen kruislings filteren voor de relatie inschakelen en het beveiligingsfilter in beide richtingen stroom. Dit kan worden gedaan door het bewerken van de relatie tussen **verkoop** en **Item**, zoals deze:

![Richting kruislings filteren voor relatie](media/row-level-security/edit-relationship-10.png)

Filters kunnen nu ook stromen in de tabel Sales naar de **Item** tabel:

![Pictogram van relatie in de diagramweergave richting filteren](media/row-level-security/diagram-view-11.png)

> [!NOTE]
> Als u de modus DirectQuery om uw gegevens gebruikt, moet u om in te schakelen in twee richtingen kruislings filteren door deze twee opties te selecteren:

1. **Bestand** -> **opties en instellingen** -> **Preview-functies** -> **inschakelen kruislings filteren in beide richtingen voor DirectQuery** .
2. **Bestand** -> **opties en instellingen** -> **DirectQuery** -> **onbeperkte meting toestaan in de modus DirectQuery**.

Voor meer informatie over het in twee richtingen kruislings filteren, downloaden de [in twee richtingen kruislings filteren in SQL Server Analysis Services 2016 en Power BI Desktop](https://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional%20cross-filtering%20in%20Analysis%20Services%202016%20and%20Power%20BI.docx) technisch document.

Dit al het werk dat moet worden aangebracht in Power BI Desktop hebt afgerond, maar er is een meer stuk werk dat moet worden gedaan om te maken van de beveiliging op Rijniveau regels dat hebben we gedefinieerd werk in Power BI Embedded. Gebruikers worden geverifieerd en geautoriseerd door de toepassing en App-tokens worden gebruikt voor die gebruikerstoegang verlenen tot een specifiek Power BI Embedded-rapport. Power BI Embedded beschikt niet over een specifieke informatie op die de gebruiker is. Voor een juiste werking van RLS moet u enkele aanvullende context doorgeven als onderdeel van uw app-token:

* **gebruikersnaam** (optioneel): beveiliging op rijniveau gebruikt dit is een tekenreeks die kan worden gebruikt voor het identificeren van de gebruiker bij het toepassen van RLS-regels. Zie de rij op rijniveau met Power BI Embedded
* **rollen** : een tekenreeks met de rollen selecteren bij het toepassen van beveiliging op rijniveau regels. Als meer dan één rol wordt doorgegeven, moeten ze worden doorgegeven als een tekenreeksmatrix.

Maken van het token met behulp van de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) methode. Als de eigenschap username aanwezig is, moet u ook ten minste één waarde doorgeven in rollen.

U kunt bijvoorbeeld de EmbedSample wijzigen. DashboardController regel 55 kan worden gewijzigd van

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

tot

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

Het volledige app-token ziet er ongeveer als volgt uit:

![Voorbeeld van JSON web token](media/row-level-security/app-token-string-12.png)

Nu met alle benodigde onderdelen aanwezig zien wanneer iemand zich aanmeldt bij de toepassing om dit rapport weer te geven ze de gegevens die zij mag zien, zoals gedefinieerd in de beveiliging op rijniveau.

![Rapport wordt weergegeven in de toepassing](media/row-level-security/dashboard-13.png)

## <a name="see-also"></a>Zie ook

[Beveiliging van beveiliging op rijniveau (RLS) met de kracht van](https://powerbi.microsoft.com/documentation/powerbi-admin-rls/)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
