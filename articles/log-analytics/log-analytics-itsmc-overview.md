---
title: IT Service Management-Connector in Azure Log Analytics | Microsoft Docs
description: In dit artikel biedt een overzicht van IT Service Management Connector (ITSMC) en informatie over het gebruik van deze oplossing centraal kunt bewaken en beheren van de ITSM werkitems in OMS Log Analytics en eventuele problemen snel worden opgelost.
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
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 6a08f042aad8ad00d712420d8f4d3b17305188e1
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centraal beheren van werkitems ITSM met IT Service Management-Connector (Preview)

![Symbool van IT-Service Management-Connector](./media/log-analytics-itsmc/itsmc-symbol.png)

De IT Service Management-Connector (ITSMC) biedt een bidirectionele integratie tussen een ondersteund IT Service Management (ITSM) productservice en Log Analytics.  U kunt via deze verbinding, incidenten, waarschuwingen of gebeurtenissen maken in ITSM product op basis van waarschuwingen, logboekrecords of waarschuwingen van Azure Log Analytics'. De connector importeert ook gegevens, zoals incidenten en wijzigingsaanvragen van ITSM product in de OMS-logboekanalyse.

Met ITSMC, kunt u het volgende doen:

  - Operationele waarschuwingen worden geïntegreerd met uw procedures incidentbeheer in het hulpprogramma ITSM van uw keuze.
    - Maak in ITSM werkitems (zoals waarschuwing, gebeurtenis, incident) van OMS-waarschuwingen en via logboek zoeken.
    - Werkitems die op basis van uw Azure Activity Log waarschuwingen via de actie ITSM in Actiegroepen maken.

  - Combineer de bewaking, logboekbestanden en service management-gegevens in uw organisatie gebruikt.
    - Incident correleren en wijzig gegevens van aanvragen van uw ITSM tooling met relevante logboekgegevens in de werkruimte voor logboekanalyse.   
    - Op het hoogste niveau dashboards voor een overzicht weergeven van incidenten, wijzigingsaanvragen en betrokken systemen.
    - Log Analytics-query's voor het verkrijgen van inzicht in Service Management-gegevens schrijven.

## <a name="adding-the-it-service-management-connector-solution"></a>Toevoegen van IT Service Management-Connector oplossing

De oplossing IT Service Management-Connector toevoegen aan uw werkruimte voor logboekanalyse, met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).

Dit is de tegel ITSMC zoals u in de galerie met oplossingen ziet:

![Connector-tegel](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Na een geslaagde toevoeging, ziet u de IT-Service Management-Connector onder **OMS** > **instellingen** > **verbonden bronnen.**

![ITSMC verbonden](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Standaard vernieuwt ITSMC van de verbinding gegevens eenmaal in elke 24 uur. Uw verbinding om gegevens te vernieuwen direct voor alle bewerkingen of de sjabloon-updates die u aanbrengt, klikt u op de knop 'Vernieuwen' weergegeven naast de verbinding.

 ![ITSMC vernieuwen](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-itsmc-connection-with-your-itsm-productsservices"></a>De ITSMC-verbinding configureren met uw ITSM producten/services

ITSMC ondersteunt verbindingen met **System Center Service Manager**, **ServiceNow**, **Provance**, en **Cherwell**.

Gebruik de volgende procedures als die van toepassing zijn voor u:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de connector hebt geconfigureerd, wordt gestart voor het verzamelen van de gegevens van de verbonden ITSM producten/services. Afhankelijk van het aantal incidenten en wijzigingsaanvragen in de ITSM product/service, moet de initiële synchronisatie worden uitgevoerd in enkele minuten.

> [!NOTE]
> - Gegevens die zijn geïmporteerd uit het product ITSM door ITSMC oplossing worden weergegeven in logboekanalyse als logboekrecords van het Type **ServiceDesk_CL**.
> - Logboekrecord bevat een veld met de naam **ServiceDeskWorkItemType_s**, dit verzoek incident of wijzigingsaanvraag, de twee soorten gegevens die zijn geïmporteerd uit het product ITSM is.

## <a name="data-synced-from-itsm-product"></a>Gegevens die zijn gesynchroniseerd vanaf de product ITSM
Incidenten en wijzigingsaanvragen worden gesynchroniseerd vanuit uw product ITSM met uw werkruimte voor logboekanalyse.
Hieronder ziet u voorbeelden van gegevens die worden verzameld door ITSMC:

> [!NOTE]

> Afhankelijk van het werkitemtype geïmporteerd in Log Analytics **ServiceDesk_CL** bevat de volgende velden:

**Werkitem:** **incidenten**  
ServiceDeskWorkItemType_s = "Incident"

**Velden**

- ServiceDeskConnectionName
- Helpdesk-service-ID
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

ServiceDeskWorkItemType_s = "ChangeRequest"

**Velden**
- ServiceDeskConnectionName
- Helpdesk-service-ID
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

| OMS-veld | ITSM veld |
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

| OMS-veld | ITSM veld |
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

**Voorbeeld logboekanalyse scherm voor ITSM gegevens:**

![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="itsmc-integration-with-other-oms-solutions"></a>ITSMC integratie met andere OMS-oplossingen

ITSM connector biedt momenteel ondersteuning voor integratie met de oplossing Serviceoverzicht.

Serviceoverzicht automatisch detecteert de toepassingsonderdelen van op Windows- en Linux-systemen en de communicatie tussen services wordt toegewezen. Hiermee kunt u uw servers bekijken als u denkt dat ze – als onderling verbonden systemen die essentiële services leveren. Serviceoverzicht ziet u de verbindingen tussen servers, processen, en poorten voor elke architectuur TCP-verbinding waarvoor geen configuratie nodig andere dan de installatie van een agent.

Meer informatie: [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md).

Als u ook de oplossing Serviceoverzicht gebruikt, kunt u de service helpdesk items gemaakt in de ITSM oplossingen zoals weergegeven in het volgende voorbeeld weergeven:

![ServiceMap-integratie](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Werkitems ITSM voor OMS waarschuwingen maken

Met ITSMC oplossing, kunt u waarschuwingen voor het activeren van het maken van werkitems in uw verbonden ITSM hulpprogramma OMS configureren. Gebruik de volgende procedure:

1. Van **logboek zoeken** venster voor het uitvoeren van een zoekquery logboek om gegevens weer te geven. Queryresultaten zijn de bron voor werkitems.
2. In **logboek zoeken**, klikt u op **waarschuwing** openen de **waarschuwingsregel toevoegen** pagina.

    ![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Op de **waarschuwingsregel toevoegen** venster, geef de vereiste gegevens voor **naam**, **ernst**, **zoekquery**, en **waarschuwing criteria** (Tijdsmeting venster meting).
4. Selecteer **Ja** voor **ITSM acties**.
5. Selecteer uw ITSM verbinding van de **verbinding selecteren** lijst.
6. Geef de details op zoals vereist.
7. Als u een afzonderlijke werkitem van elke logboekvermelding van deze waarschuwing, selecteert de **maken van afzonderlijke werkitems van elke logboekvermelding** selectievakje.

    of

    Laat dit selectievakje niet geselecteerd voor het maken van slechts één werkitem voor een willekeurig aantal vermeldingen in het logboek in deze waarschuwing.

7. Klik op **Opslaan**.

De OMS-waarschuwing die u hebt gemaakt, kan worden weergegeven onder **instellingen**>**waarschuwingen**. Werkitems van de bijbehorende ITSM verbinding worden gemaakt wanneer de waarschuwing opgegeven voorwaarde wordt voldaan.

## <a name="create-itsm-work-items-from-oms-logs"></a>Maken van werkitems ITSM van OMS-Logboeken

U kunt ook werkitems maken in de verbonden ITSM bronnen rechtstreeks vanuit een logboekrecord. Gebruik de volgende procedure:

1. Van **logboek zoeken**, de vereiste gegevens zoeken, selecteert u de details en klik op **maken werkitem**.

    De **ITSM werkitem maken** venster wordt weergegeven:

    ![Log Analytics scherm](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

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

## <a name="create-itsm-work-items-from-azure-alerts"></a>Werkitems ITSM van waarschuwingen van Azure maken

ITSMC is geïntegreerd met groepen in te grijpen.

[Actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) bieden een manier computertoepassingen van acties voor uw Azure-waarschuwingen activeren. Met de actie ITSM in Actiegroepen, kunt u werkitems in uw ITSM product met een bestaande verbinding met ITSM connector oplossing.

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

Bij het maken/bewerken van een Azure waarschuwingsregel, moet u een actiegroep, waarvoor een actie ITSM gebruiken. Als de waarschuwing wordt gegenereerd, wordt in het hulpprogramma ITSM werkitem gemaakt.

>[!NOTE]

> Op dit moment alleen activiteit logboek waarschuwingen de actie ITSM ondersteunen, ondersteunen andere Azure-waarschuwingen dit niet.


## <a name="troubleshoot-itsm-connections-in-oms"></a>ITSM verbindingen in OMS oplossen
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
