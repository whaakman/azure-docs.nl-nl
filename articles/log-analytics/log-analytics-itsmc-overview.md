---
title: IT Service Management-Connector in Azure Log Analytics | Microsoft Docs
description: In dit artikel biedt een overzicht van IT Service Management Connector (ITSMC) en informatie over het gebruik van deze oplossing centraal kunt bewaken en beheren van de ITSM werkitems in Azure Log Analytics en eventuele problemen snel worden opgelost.
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: d586ee1b96b34d6ca83e1ffd76aee38e79bdd727
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Verbinding maken met Azure ITSM hulpprogramma IT Service Management-Connector

![Symbool van IT-Service Management-Connector](./media/log-analytics-itsmc/itsmc-symbol.png)

De IT Service Management-Connector (ITSMC) kunt u verbinding maken met Azure en een ondersteunde IT Service Management (ITSM)-producten en diensten.

Azure-services zoals Log Analytics en Azure Monitor bieden hulpprogramma's om te detecteren, analyseren en oplossen van problemen met uw Azure en niet-Azure-resources. De werkitems die betrekking hebben op een probleem doorgaans zich echter in een ITSM producten/services bevinden. De connector ITSM biedt een bidirectionele verbinding tussen Azure en ITSM hulpmiddelen voor het oplossen van problemen sneller.

ITSMC ondersteunt verbindingen met de volgende ITSM-hulpprogramma's:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

Met ITSMC kunt u

-  Werkitems in ITSM hulpprogramma, op basis van uw Azure waarschuwingen (metrische waarschuwingen, activiteitenlogboek waarschuwingen en meldingen Log Analytics) maken.
-  U kunt eventueel synchroniseren van uw incident en aanvraaggegevens wijzigen vanuit uw ITSM hulpprogramma naar een Azure-logboekanalyse-werkruimte.


U kunt starten met de ITSM-Connector met de volgende stappen:

1.  [De Connector ITSM oplossing toevoegen](#adding-the-it-service-management-connector-solution)
2.  [Maak een verbinding ITSM](#creating-an-itsm-connection)
3.  [De verbinding gebruiken](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Toevoegen van IT Service Management-Connector oplossing

Voordat u een verbinding maken kunt, moet u de Connector ITSM oplossing toevoegen.

1.  Klik in de Azure-portal op **+ nieuw** pictogram.

    ![Nieuwe Azure-resource](./media/log-analytics-itsmc/azure-add-new-resource.png)

2.  Zoeken naar **IT Service Management-Connector** in de Marketplace en klik op **maken**.

    ![ITSMC oplossing toevoegen](./media/log-analytics-itsmc/add-itsmc-solution.png)

3.  In de **OMS-werkruimte** sectie, selecteert u de Azure Log Analytics-werkruimte waarop u wilt installeren van de oplossing.
4.  In de **OMS-werkruimte-instellingen** sectie, selecteert u de ResourceGroup waar u wilt de bron van de oplossing maakt.

    ![ITSMC werkruimte](./media/log-analytics-itsmc/itsmc-solution-workspace.png)

5.  Klik op **Create**.

Wanneer de bron van de oplossing is geïmplementeerd, wordt een melding weergegeven aan de bovenkant van het venster Direct.


## <a name="creating-an-itsm--connection"></a>Maken van een verbinding ITSM

Als u de oplossing hebt geïnstalleerd, kunt u een verbinding kunt maken.

Voor het maken van een verbinding, moet u het hulpprogramma ITSM zodat de verbinding van de oplossing ITSM Connector voorbereiden.  

Afhankelijk van het product ITSM gebruikt dat u verbinding maakt, gebruik de volgende stappen:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

Zodra u hebt uw extra ITSM domainprep, de volgende stappen om een verbinding te maken:

1.  Ga naar **alle Resources**, zoekt u naar **ServiceDesk(YourWorkspaceName)**.
2.  Onder **WERKRUIMTE GEGEVENSBRONNEN** in het linkerdeelvenster klikt u op **ITSM verbindingen**.
    ![ITSM-verbindingen](./media/log-analytics-itsmc/itsm-connections.png)

    Deze pagina bevat de lijst met verbindingen.
3.  Klik op **verbinding toevoegen**.

    ![ITSM verbinding toevoegen](./media/log-analytics-itsmc/add-new-itsm-connection.png)

4.  Geef de verbindingsinstellingen zoals beschreven in [configureren van de ITSMC verbinding met uw ITSM producten/services artikel](log-analytics-itsmc-connections.md).

    > [!NOTE]

    > Standaard wordt de configuratiegegevens van de verbinding in ITSMC vernieuwd eenmaal in elke 24 uur. Uw verbinding om gegevens te vernieuwen direct voor alle bewerkingen of de sjabloon-updates die u aanbrengt, klikt u op de knop 'Vernieuwen' weergegeven naast de verbinding.

    ![Verbinding vernieuwen](./media/log-analytics-itsmc/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>De oplossing gebruiken
   U kunt met behulp van de oplossing ITSM Connector werkitems maken van waarschuwingen van Azure, logboekanalyse waarschuwingen en Log Analytics logboekrecords.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Werkitems ITSM van waarschuwingen van Azure maken

Zodra u uw ITSM verbinding die is gemaakt hebt, kunt u work items maken in uw ITSM-hulpprogramma op basis van waarschuwingen van Azure, met behulp van de **ITSM actie** in **actiegroepen**.

Actiegroepen bieden een manier computertoepassingen van acties voor uw Azure-waarschuwingen activeren. U kunt de actiegroepen met metrische waarschuwingen, activiteitenlogboek van waarschuwingen en Azure-logboekanalyse waarschuwingen in Azure portal gebruiken.

Gebruik de volgende procedure:

1. Klik in de Azure-portal op **Monitor**.
2. Klik in het linkerdeelvenster op **actiegroepen**. De **actie-groep toevoegen** venster wordt weergegeven.

    ![Actiegroepen](media/log-analytics-itsmc/action-groups.png)

3. Geef **naam** en **korte naam** voor uw groep in te grijpen. Selecteer de **resourcegroep** en **abonnement** waar u wilt maken van uw groep in te grijpen.

    ![Detail van actie-groepen](media/log-analytics-itsmc/action-groups-details.png)

4. Selecteer in de lijst met acties **ITSM** uit de vervolgkeuzelijst voor **actietype**. Geef een **naam** voor de actie en klik op **details bewerken**.
5. Selecteer de **abonnement** waar uw werkruimte voor logboekanalyse zich bevindt. Selecteer de **verbinding** naam (uw ITSM Connector-naam), gevolgd door de naam van uw werkruimte. Bijvoorbeeld 'MyITSMMConnector(MyWorkspace)'.

    ![Details van ITSM actie](./media/log-analytics-itsmc/itsm-action-details.png)

6. Selecteer **werkitem** type uit de vervolgkeuzelijst.
   Kies voor het gebruik van een bestaande sjabloon of vul de velden die vereist zijn voor het product ITSM.
7. Klik op **OK**.

Bij het maken/bewerken van een Azure waarschuwingsregel, moet u een actiegroep, waarvoor een actie ITSM gebruiken. Wanneer de waarschuwing wordt geactiveerd, is het werkitem gemaakt/bijgewerkt in het hulpprogramma ITSM.

>[!NOTE]

> Zie voor meer informatie over prijzen van ITSM actie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/) voor groepen in te grijpen.


## <a name="create-itsm-work-items-from-log-analytics-alerts"></a>Werkitems ITSM van logboekanalyse waarschuwingen maken

U kunt regels voor waarschuwingen configureren in Azure Log Analytics-portal voor het maken van werkitems in ITSM hulpmiddel met de volgende procedure.

1. Van **logboek zoeken** venster voor het uitvoeren van een zoekquery logboek om gegevens weer te geven. Queryresultaten zijn de bron voor werkitems.
2. In **logboek zoeken**, klikt u op **waarschuwing** openen de **waarschuwingsregel toevoegen** pagina.

    ![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-work-items-for-azure-alerts.png)

3. Op de **waarschuwingsregel toevoegen** venster, geef de vereiste gegevens voor **naam**, **ernst**, **zoekquery**, en **waarschuwing criteria** (Tijdsmeting venster meting).
4. Selecteer **Ja** voor **ITSM acties**.
5. Selecteer uw ITSM verbinding van de **verbinding selecteren** lijst.
6. Geef de details op zoals vereist.
7. Als u een afzonderlijke werkitem van elke logboekvermelding van deze waarschuwing, selecteert de **maken van afzonderlijke werkitems van elke logboekvermelding** selectievakje.

    of

    Laat dit selectievakje niet geselecteerd voor het maken van slechts één werkitem voor een willekeurig aantal vermeldingen in het logboek in deze waarschuwing.

7. Klik op **Opslaan**.

U kunt de Log Analytics-waarschuwing die u hebt gemaakt onder weergeven **instellingen > waarschuwingen**. Werkitems van de bijbehorende ITSM verbinding worden gemaakt wanneer de waarschuwing opgegeven voorwaarde wordt voldaan.


## <a name="create-itsm-work-items-from-log-analytics-log-records"></a>ITSM-werkitems maken uit logboekrecords Log Analytics

U kunt ook werkitems maken in de verbonden ITSM bronnen rechtstreeks vanuit een logboekrecord. Dit kan worden gebruikt om te controleren of de verbinding correct werkt.


1. Van **logboek zoeken**, de vereiste gegevens zoeken, selecteert u de details en klik op **maken werkitem**.

    De **ITSM werkitem maken** venster wordt weergegeven:

    ![Log Analytics scherm](media/log-analytics-itsmc/itsmc-work-items-from-azure-logs.png)

2.   Voeg de volgende details:

  - **Titel werkitem**: titel voor het werkitem.
  - **Beschrijving van werkitem**: beschrijving voor het nieuwe werkitem.
  - **Van invloed op een Computer**: naam van de computer waar deze logboekgegevens is gevonden.
  - **Selecteer verbinding**: ITSM verbinding waarin u wilt maken van dit werkitem.
  - **Werkitem**: Type werkitem.

3. Met een bestaande werkitemsjabloon voor een incident, klikt u op **Ja** onder **item werken genereren op basis van sjabloon** optie en klik vervolgens op **maken**.

    Of

    Klik op **Nee** als u wilt dat uw aangepaste waarden opgeven.

4. Geef de juiste waarden in de **Type Contact**, **Impact**, **urgentie**, **categorie**, en **subcategorie** tekstvakken en klik vervolgens op **maken**.


##<a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualiseren en analyseren van het incident en wijzigen van gegevens van aanvragen

Op basis van uw configuratie wanneer u een verbinding instelt, kan ITSM connector maximaal 120 dagen van gegevens van aanvragen voor Incident- en synchroniseren. Het schema van de logboek-record voor deze gegevens vindt u in de [volgende sectie](#additional-information).

Aanvraaggegevens incident en de wijziging kan worden weergegeven in de oplossing met behulp van het dashboard ITSM Connector.

![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

Het dashboard bevat ook informatie over de status van de connector die kan worden gebruikt als een beginpunt voor het analyseren van problemen met de verbindingen.

U kunt ook de incidenten die zijn gesynchroniseerd op basis van de betrokken computers, binnen de oplossing Serviceoverzicht visualiseren.

Serviceoverzicht automatisch detecteert de toepassingsonderdelen van op Windows- en Linux-systemen en de communicatie tussen services wordt toegewezen. Hiermee kunt u uw servers bekijken als u denkt dat ze – als onderling verbonden systemen die essentiële services leveren. Serviceoverzicht ziet u de verbindingen tussen servers, processen, en poorten voor elke architectuur TCP-verbinding waarvoor geen configuratie nodig andere dan de installatie van een agent. [Meer informatie](../operations-management-suite/operations-management-suite-service-map.md).

Als u de oplossing Serviceoverzicht gebruikt, kunt u de service helpdesk items gemaakt in de ITSM oplossingen zoals weergegeven in het volgende voorbeeld weergeven:

![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)

Meer informatie: [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md)


## <a name="additional-information"></a>Aanvullende informatie

### <a name="data-synced-from-itsm-product"></a>Gegevens die zijn gesynchroniseerd vanaf de product ITSM
Incidenten en wijzigingsaanvragen worden gesynchroniseerd vanuit uw product ITSM met uw werkruimte voor logboekanalyse op basis van de configuratie van de verbinding.

Hieronder ziet u voorbeelden van gegevens die worden verzameld door ITSMC:

> [!NOTE]

> Afhankelijk van het werkitemtype geïmporteerd in Log Analytics **ServiceDesk_CL** bevat de volgende velden:

**Werkitem:** **incidenten**  
ServiceDeskWorkItemType_s="Incident"

**Fields**

- ServiceDeskConnectionName
- Service Desk ID
- Status
- Urgentie
- Impact
- Prioriteit
- Escalatie
- Gemaakt door
- Opgelost door
- Gesloten door
- Bron
- Toegewezen aan
- Category
- Titel
- Beschrijving
- Gemaakt op
- Datum gesloten
- Datum opgelost
- Datum van laatste wijziging
- Computer


**Werkitem:** **wijzigingsaanvragen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Fields**
- ServiceDeskConnectionName
- Service Desk ID
- Gemaakt door
- Gesloten door
- Bron
- Toegewezen aan
- Titel
- Type
- Category
- Status
- Escalatie
- Status conflict
- Urgentie
- Prioriteit
- Risico
- Impact
- Toegewezen aan
- Gemaakt op
- Datum gesloten
- Datum van laatste wijziging
- Gevraagde datum
- Geplande begindatum
- Geplande einddatum
- Werk de begindatum
- Werk einddatum
- Beschrijving
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Uitvoergegevens voor een incident ServiceNow

| Log Analytics-veld | ServiceNow-veld |
|:--- |:--- |
| ServiceDeskId_s| Aantal |
| IncidentState_s | Status |
| Urgency_s |Urgentie |
| Impact_s |Impact|
| Priority_s | Prioriteit |
| CreatedBy_s | Geopend door |
| ResolvedBy_s | Opgelost door|
| ClosedBy_s  | Gesloten door |
| Source_s| Neem contact op met het type |
| AssignedTo_s | Toegewezen aan  |
| Category_s | Category |
| Title_s|  Korte beschrijving |
| Description_s|  Opmerkingen |
| CreatedDate_t|  Geopend |
| ClosedDate_t| gesloten|
| ResolvedDate_t|Opgelost|
| Computer  | configuratie-item |

## <a name="output-data-for-a-servicenow-change-request"></a>Wijzigingsaanvraag uitvoergegevens voor een ServiceNow

| Log Analytics | ServieNow veld |
|:--- |:--- |
| ServiceDeskId_s| Aantal |
| CreatedBy_s | Aangevraagd door |
| ClosedBy_s | Gesloten door |
| AssignedTo_s | Toegewezen aan  |
| Title_s|  Korte beschrijving |
| Type_s|  Type |
| Category_s|  Category |
| CRState_s|  Status|
| Urgency_s|  Urgentie |
| Priority_s| Prioriteit|
| Risk_s| Risico|
| Impact_s| Impact|
| RequestedDate_t  | Aangevraagd door datum |
| ClosedDate_t | Datum gesloten |
| PlannedStartDate_t  |     Geplande begindatum |
| PlannedEndDate_t  |   Geplande einddatum |
| WorkStartDate_t  | Werkelijke begindatum |
| WorkEndDate_t | Werkelijke einddatum|
| Description_s | Beschrijving |
| Computer  | Configuratie-Item |


## <a name="troubleshoot-itsm-connections"></a>ITSM verbindingen oplossen
1.  Als de verbinding is mislukt door de gebruikersinterface van de verbonden bron met een **fout bij het opslaan van de verbinding** bericht wordt weergegeven, voert de volgende stappen uit:
- Voor ServiceNow, Cherwell en Provance-verbindingen  
       -Controleer of u de gebruikersnaam, wachtwoord, client-ID en clientgeheim correct voor elk van de verbindingen opgegeven.  
       -Controleer of u voldoende rechten hebt in het bijbehorende ITSM product de verbinding te maken.  
- Voor Service Manager-verbindingen  
       -Zorg ervoor dat de Web-app is geïmplementeerd en hybride verbinding is gemaakt. Om te controleren of de verbinding is tot stand gebracht met de Service Manager on-premises machine, gaat u naar de Web-app-URL zoals beschreven in de documentatie voor de [hybride verbinding](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).  

2.  Als u gegevens van ServiceNow is niet ophalen die zijn gesynchroniseerd met Log Analytics, zorg ervoor dat het ServiceNow exemplaar zich niet in de slaapstand. ServiceNow Dev exemplaren gaat soms slaapstand bij inactiviteit gedurende een lange periode. Anders Meld het probleem.
3.  Als OMS waarschuwingen geactiveerd, maar werkt items ITSM product niet worden gemaakt of configuratie-items zijn niet gemaakt/gekoppeld aan werkitems of andere algemene informatie, vindt u in de volgende locaties:
 -  ITSMC: De oplossing bevat een overzicht van verbindingen werk items computers enzovoort. Klik op de tegel waarin **Connector-Status**, wat u duurt **logboek zoeken** met de relevante query. Bekijk de records in het logboek met LogType_S als fout voor meer informatie.
 - **Meld u zoekopdracht** pagina: Bekijk de fouten/gerelateerde informatie direct met de query *Type = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Problemen met de Web-App Service Manager-implementatie oplossen
1.  In geval van problemen met web-app-implementatie, zorg ervoor dat u gemachtigd bent in het abonnement vermeld resources maken/implementeren.
2.  Als u krijgt een **'-objectverwijzing niet ingesteld op exemplaar van een object'** fout tijdens het uitvoeren van de [script](log-analytics-itsmc-service-manager-script.md), zorg ervoor dat u geldige waarden onder ingevoerd **Gebruikersconfiguratie** sectie .
3.  Als u niet om ervoor te zorgen dat de provider van de vereiste bronnen is geregistreerd in het abonnement te maken van service bus relay-naamruimte. Als het niet is geregistreerd, moet u handmatig service bus relay-naamruimte maken vanuit de Azure-portal. U kunt ook maken terwijl [maken van de hybride verbinding](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) vanuit de Azure-portal.


## <a name="contact-us"></a>Contact opnemen

Voor query's of feedback over de IT-Service Management-Connector, contact met ons op [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Volgende stappen
[ITSM producten/services toegevoegd aan de IT-Service Management-Connector](log-analytics-itsmc-connections.md).
