---
title: IT Service Management-Connector in Azure Log Analytics | Microsoft Docs
description: In dit artikel biedt een overzicht van IT Service Management Connector (ITSMC) en informatie over het gebruik van deze oplossing centraal bewaken en beheren van de ITSM-werkitems in Azure Log Analytics en los eventuele problemen snel.
services: log-analytics
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.component: ''
ms.openlocfilehash: 5f5b03090f374f936e03d487596ca0462feea348
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042425"
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Azure verbinden met ITSM-hulpprogramma's met behulp van IT Service Management-Connector

![Symbool van IT Service Management-Connector](./media/log-analytics-itsmc/itsmc-symbol.png)

De IT Service Management Connector (ITSMC) kunt u verbinding maken met Azure en een ondersteunde IT Service Management (ITSM) product/service.

Azure-services zoals Log Analytics en Azure Monitor over hulpprogramma's om te detecteren, analyseren en oplossen van problemen met uw Azure- en niet-Azure-resources. Echter, de werkitems die betrekking hebben op een probleem meestal zich bevinden in een ITSM-product/service. De ITSM-connector biedt een bidirectionele verbinding tussen Azure en ITSM-hulpprogramma's kunt u problemen sneller oplossen.

ITSMC ondersteunt verbindingen met de volgende ITSM-hulpprogramma's:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

Met ITSMC, kunt u het volgende doen:

-  Werkitems in ITSM-hulpprogramma, op basis van uw Azure-waarschuwingen (metrische waarschuwingen, activiteitenlogboek waarschuwingen en Log Analytics-waarschuwingen) maken.
-  U kunt eventueel synchroniseren van uw incident en gegevens van aanvragen van uw ITSM-hulpprogramma naar een Azure Log Analytics-werkruimte wijzigen.


U kunt starten met behulp van de ITSM-Connector met behulp van de volgende stappen uit:

1.  [De ITSM-Connector-oplossing toevoegen](#adding-the-it-service-management-connector-solution)
2.  [Maak een ITSM-verbinding](#creating-an-itsm-connection)
3.  [De verbinding gebruiken](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Toevoegen van IT Service Management Connector-oplossing

Voordat u een verbinding maken kunt, moet u de oplossing voor ITSM-Connector toevoegen.

1.  Klik in Azure portal op **+ nieuw** pictogram.

    ![Nieuwe Azure-resource](./media/log-analytics-itsmc/azure-add-new-resource.png)

2.  Zoeken naar **IT Service Management Connector** in de Marketplace en klik op **maken**.

    ![ITSMC oplossing toevoegen](./media/log-analytics-itsmc/add-itsmc-solution.png)

3.  In de **OMS-werkruimte** sectie, selecteer de Azure Log Analytics-werkruimte waar u wilt installeren van de oplossing.
4.  In de **instellingen van de OMS-werkruimte** sectie, selecteer de resourcegroep waar u om de bron van de oplossing te maken.

    ![ITSMC werkruimte](./media/log-analytics-itsmc/itsmc-solution-workspace.png)

5.  Klik op **Create**.

Wanneer de resource van de oplossing wordt geïmplementeerd, wordt een melding weergegeven aan de bovenkant van het venster.


## <a name="creating-an-itsm--connection"></a>Het maken van een ITSM-verbinding

Nadat u de oplossing hebt geïnstalleerd, kunt u een verbinding kunt maken.

Voor het maken van een verbinding, moet u uw ITSM-voorbereidingshulpprogramma zodat de verbinding van de ITSM-Connector-oplossing.  

Afhankelijk van de ITSM-product dat u verbinding maakt, gebruikt u de volgende stappen uit:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

Nadat u uw ITSM-hulpprogramma's hebt voorbereid, de volgende stappen om een verbinding te maken:

1.  Ga naar **alle Resources**, zoek naar **ServiceDesk(YourWorkspaceName)**.
2.  Onder **GEGEVENSBRONNEN voor WERKRUIMTE** in het linkerdeelvenster klikt u op **ITSM-verbindingen**.
    ![ITSM-verbindingen](./media/log-analytics-itsmc/itsm-connections.png)

    Deze pagina bevat de lijst met verbindingen.
3.  Klik op **verbinding toevoegen**.

    ![ITSM-verbinding toevoegen](./media/log-analytics-itsmc/add-new-itsm-connection.png)

4.  Instellingen voor de verbinding opgeven, zoals beschreven in [de ITSMC-verbinding configureren met uw ITSM-producten en services-artikel](log-analytics-itsmc-connections.md).

    > [!NOTE]

    > Standaard vernieuwt ITSMC configuratiegegevens van de verbinding eenmaal in elke 24 uur. Als u wilt vernieuwen van uw verbinding gegevens direct voor alle bewerkingen of de sjabloon-updates die u aanbrengt, klikt u op de **synchronisatie** knop op de blade van uw verbinding.

    ![Verbinding vernieuwen](./media/log-analytics-itsmc/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>De oplossing gebruiken
   U kunt met behulp van de ITSM-Connector-oplossing, werkitems maken van Azure-waarschuwingen, Log Analytics-waarschuwingen en records in logboek registreren Log Analytics.

## <a name="create-itsm-work-items-from-azure-alerts"></a>ITSM-werkitems maken vanuit Azure-waarschuwingen

Als u de ITSM-verbinding gemaakt hebt, kunt u work item (s) maken in uw ITSM-hulpprogramma op basis van waarschuwingen van Azure, met behulp van de **ITSM-actie** in **actiegroepen**.

Actiegroepen bieden een manier modulaire en herbruikbare van acties voor uw Azure-waarschuwingen activeren. U kunt actiegroepen gebruiken met metrische waarschuwingen, activiteitenlogboek waarschuwingen en Azure Log Analytics-waarschuwingen in Azure portal.

Gebruik de volgende procedure:

1. Klik in Azure portal op **Monitor**.
2. Klik in het linkerdeelvenster op **actiegroepen**. De **actiegroep toevoegen** venster wordt weergegeven.

    ![Actiegroepen](media/log-analytics-itsmc/action-groups.png)

3. Geef **naam** en **ShortName** voor de actiegroep. Selecteer de **resourcegroep** en **abonnement** waar u uw actiegroep maken.

    ![Details van de actie-groepen](media/log-analytics-itsmc/action-groups-details.png)

4. Selecteer in de lijst met acties **ITSM** uit de vervolgkeuzelijst voor **actietype**. Geef een **naam** voor de actie en klikt u op **details bewerken**.
5. Selecteer de **abonnement** waar uw Log Analytics-werkruimte zich bevindt. Selecteer de **verbinding** naam (de naam van de ITSM-Connector), gevolgd door de naam van uw werkruimte. Bijvoorbeeld, "MyITSMMConnector(MyWorkspace)."

    ![Details van de ITSM-actie](./media/log-analytics-itsmc/itsm-action-details.png)

6. Selecteer **werkitem** type uit de vervolgkeuzelijst.
   Kies voor het gebruik van een bestaande sjabloon of vul de velden die vereist zijn door uw ITSM-product.
7. Klik op **OK**.

Wanneer een Azure waarschuwingsregel maken of bewerken, gebruikt u een actiegroep, waarvoor een ITSM-actie. Wanneer de waarschuwing wordt geactiveerd, is het werkitem gemaakt/bijgewerkt in de ITSM-hulpprogramma.

>[!NOTE]

> Zie voor meer informatie over prijzen van de ITSM-actie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/) voor Actiegroepen.


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualiseren en analyseren van het incident en het wijzigen van gegevens van aanvragen

Op basis van uw configuratie bij het instellen van een verbinding, kan ITSM-connector maximaal 120 dagen van de gegevens van aanvragen van Incident en wijzigingsaanvraag synchroniseren. Het schema van de record logboek voor deze gegevens vindt u in de [volgende sectie](#additional-information).

De gegevens van het incident en wijzigingsaanvraag aanvragen kunnen worden gevisualiseerd met behulp van de ITSM-Connector-dashboard in de oplossing.

![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

Het dashboard bevat ook informatie over de connectorstatus van de die kan worden gebruikt als een beginpunt voor het analyseren van problemen met de verbindingen.

U kunt ook de incidenten die zijn gesynchroniseerd op basis van de betrokken computers, binnen de oplossing Serviceoverzicht visualiseren.

Serviceoverzicht wordt automatisch de toepassingsonderdelen op Windows en Linux-systemen detecteert en de communicatie tussen services toewijst. Hiermee kunt u weergeven van uw servers beschouwen zoals u ze: als onderling verbonden systemen die kritieke services verlenen. Servicetoewijzing toont verbindingen tussen servers, processen en poorten in alle via TCP verbonden architectuur zonder configuratie dan het installeren van een agent vereist. [Meer informatie](../operations-management-suite/operations-management-suite-service-map.md).

Als u van de oplossing Serviceoverzicht gebruikmaakt, kunt u de servicedeskitems gemaakt in de ITSM-oplossingen zoals wordt weergegeven in het volgende voorbeeld bekijken:

![Log Analytics scherm](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)

Meer informatie: [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md)


## <a name="additional-information"></a>Aanvullende informatie

### <a name="data-synced-from-itsm-product"></a>Gegevens die zijn gesynchroniseerd vanuit ITSM-product
Incidenten en wijzigingsaanvragen worden gesynchroniseerd vanuit uw ITSM-product met uw Log Analytics-werkruimte op basis van de configuratie van de verbinding.

De volgende informatie ziet u voorbeelden van gegevens die worden verzameld door ITSMC:

> [!NOTE]

> Afhankelijk van het werkitemtype geïmporteerd in Log Analytics, **ServiceDesk_CL** bevat de volgende velden:

**Werkitem:** **incidenten**  
ServiceDeskWorkItemType_s="Incident"

**Velden**

- ServiceDeskConnectionName
- Servicedesk-id
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
- Categorie
- Titel
- Beschrijving
- Gemaakt op
- Datum gesloten
- Datum opgelost
- Datum van laatste wijziging
- Computer


**Werkitem:** **wijzigingsaanvragen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Velden**
- ServiceDeskConnectionName
- Servicedesk-id
- Gemaakt door
- Gesloten door
- Bron
- Toegewezen aan
- Titel
- Type
- Categorie
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
- Aanvraagdatum
- Geplande begindatum
- Geplande einddatum
- Werk de begindatum
- Werk einddatum
- Beschrijving
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Uitvoergegevens voor een ServiceNow-incident

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
| Category_s | Categorie |
| Title_s|  Korte beschrijving |
| Description_s|  Opmerkingen |
| CreatedDate_t|  Geopend |
| ClosedDate_t| Gesloten|
| ResolvedDate_t|Opgelost|
| Computer  | Configuratie-item |

## <a name="output-data-for-a-servicenow-change-request"></a>Uitvoergegevens voor een ServiceNow-wijzigingsaanvraag

| Log Analytics | ServieNow veld |
|:--- |:--- |
| ServiceDeskId_s| Aantal |
| CreatedBy_s | Aangevraagd door |
| ClosedBy_s | Gesloten door |
| AssignedTo_s | Toegewezen aan  |
| Title_s|  Korte beschrijving |
| Type_s|  Type |
| Category_s|  Categorie |
| CRState_s|  Status|
| Urgency_s|  Urgentie |
| Priority_s| Prioriteit|
| Risk_s| Risico|
| Impact_s| Impact|
| RequestedDate_t  | Aangevraagd op datum |
| ClosedDate_t | Datum gesloten |
| PlannedStartDate_t  |     Geplande begindatum |
| PlannedEndDate_t  |   Geplande einddatum |
| WorkStartDate_t  | Werkelijke begindatum |
| WorkEndDate_t | Werkelijke einddatum|
| Description_s | Beschrijving |
| Computer  | Configuratie-Item |


## <a name="troubleshoot-itsm-connections"></a>ITSM-verbindingen oplossen
1.  Als er geen verbinding kan vanuit de gebruikersinterface van de verbonden bron met een **fout bij het opslaan van de verbinding** ontvangt, voert de volgende stappen uit:
 - Voor ServiceNow, Cherwell en Provance-verbindingen  
    - Zorg ervoor dat u de gebruikersnaam, wachtwoord, client-ID en clientgeheim correct voor elk van de verbindingen ingevoerd.  
    - Controleer of u voldoende bevoegdheden hebt in het bijbehorende ITSM-product om de verbinding te maken.  
 - Voor Service Manager-verbindingen  
    - Zorg ervoor dat de Web-app is geïmplementeerd en hybride verbinding wordt gemaakt. Als u wilt controleren of de verbinding is tot stand gebracht met de Service Manager on-premises machine, gaat u naar de URL van de Web-app zoals beschreven in de documentatie voor het maken van de [hybride verbinding](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).  

2.  Als u niet ophalen van gegevens van ServiceNow is gesynchroniseerd met Log Analytics, zorg ervoor dat het ServiceNow exemplaar niet in de slaapstand staat. Exemplaren van ServiceNow Dev gaat soms u naar de slaapstand als niet-actieve gedurende een lange periode. Anders, meld u het probleem.
3.  Als de OMS-waarschuwingen worden geactiveerd, maar werkt items worden niet gemaakt in de ITSM-product of configuratie-items zijn niet gemaakt/gekoppeld aan werkitems of voor andere algemene informatie, zoekt u in de volgende locaties:
 -  ITSMC: De oplossing bevat een overzicht van verbindingen/zakelijke items/computers enzovoort. Klik op de tegel weergegeven **Connector-Status**, die gaat u naar **zoeken in logboeken** met de relevante query. Bekijk de records in logboek registreren met LogType_S als fout voor meer informatie.
 - **Zoeken in logboeken** pagina: Bekijk de fouten/gerelateerde informatie rechtstreeks met behulp van de query `*`ServiceDeskLog_CL`*`.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Service Manager-Web-App-implementatie oplossen
1.  In het geval van problemen met web app-implementatie, zorg ervoor dat u voldoende rechten hebt in het abonnement vermeld maken/om resources te implementeren.
2.  Als er een **"Objectverwijzing niet ingesteld op exemplaar van een object"** fout tijdens het uitvoeren van de [script](log-analytics-itsmc-service-manager-script.md), zorg ervoor dat u hebt ingevoerd geldige waarden onder **Gebruikersconfiguratie** sectie .
3.  Als u een failback uitvoeren naar service bus relay-naamruimte maken, zorgt u ervoor dat de vereiste resourceprovider is geregistreerd in het abonnement. Als niet is geregistreerd, moet u handmatig service bus relay-naamruimte maken vanuit Azure portal. U kunt ook maken terwijl [het maken van de hybride verbinding](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) vanuit Azure portal.


## <a name="contact-us"></a>Contact opnemen

Contact met ons op voor alle query's of feedback over de IT Service Management-Connector, [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Volgende stappen
[ITSM-producten en services toevoegen aan IT Service Management Connector](log-analytics-itsmc-connections.md).
