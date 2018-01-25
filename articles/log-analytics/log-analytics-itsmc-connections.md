---
title: Verbindingen met de IT-Service Management-Connector ondersteund in Azure Log Analytics | Microsoft Docs
description: In dit artikel bevat informatie over het aansluiten van uw ITSM producten/services met de IT Service Management Connector (ITSMC) in de OMS-logboekanalyse centraal kunt bewaken en beheren van de werkitems ITSM.
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: a51ba4b45b7f6c72037d5c562a4ccd59e601cee4
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Verbinding maken met ITSM producten/services met IT Service Management-Connector
In dit artikel bevat informatie over het configureren van de verbinding tussen uw ITSM producten, services en de IT-Service Management-Connector (ITSMC) in Log Analytics om uw werkitems centraal te beheren. Zie voor meer informatie over ITSMC [overzicht](log-analytics-itsmc-overview.md).

De volgende ITSM producten/services worden ondersteund. Selecteer het product om gedetailleerde informatie over het verbinding maken met het product ITSMC weer te geven.

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]

> ITSM Connector kan alleen verbinding maken met cloud-gebaseerde ServiceNow-exemplaren. Lokale ServiceNow-exemplaren worden momenteel niet ondersteund.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>Verbinding maken met System Center Service Manager IT Service Management-Connector in Azure

De volgende secties bevatten meer informatie over het verbinding maken met uw product System Center Service Manager ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- De Service Manager-webtoepassing (Web-app) is geïmplementeerd en geconfigureerd. Informatie over Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
- Hybride verbinding gemaakt en geconfigureerd. Meer informatie: [configureren van de hybride verbinding](#configure-the-hybrid-connection).
- Ondersteunde versies van Service Manager: 2012 R2 of 2016.
- Gebruikersrol: [geavanceerde operator](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure verbinding maken met uw exemplaar van System Center Service Manager ITSMC:

1. Ga in de Azure-portal naar **alle Resources** en zoekt u **ServiceDesk(YourWorkspaceName)**

2.  Onder **WERKRUIMTE GEGEVENSBRONNEN** klikt u op **ITSM verbindingen**.

    ![Nieuwe verbinding](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Klik boven in het rechterdeelvenster op **toevoegen**.

4. Geef de informatie, zoals beschreven in de volgende tabel en klik op **OK** om de verbinding te maken.

> [!NOTE]

> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor de System Center Service Manager-instantie die u wilt verbinden met ITSMC.  U gebruikt deze naam later wanneer u werkitems in dit exemplaar te configureren / gedetailleerde logboekanalyse bekijken. |
| **Type van de accountpartner**   | Selecteer **System Center Service Manager**. |
| **Server-URL**   | Typ de URL van de Service Manager-Web-app. Meer informatie over Service Manager-Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Typ de client-ID die u hebt gegenereerd (met behulp van het script voor automatische) voor het verifiëren van de Web-app. Meer informatie over de geautomatiseerde script [hier.](log-analytics-itsmc-service-manager-script.md)|
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Gegevens synchroniseren bereik**   | Selecteer de Service Manager-werkitems die u wilt synchroniseren via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk. **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt OMS betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

![Service manager-verbinding](./media/log-analytics-itsmc/service-manager-connection.png)

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde werkitems van Service Manager worden geïmporteerd in Azure **logboekanalyse.** U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.

- U kunt incidenten maken van logboekanalyse waarschuwingen of logboekrecords of waarschuwingen van Azure in dit exemplaar van de Service Manager.


Meer informatie: [werkitems ITSM maken voor logboekanalyse waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [ITSM maken werkitems van logboekanalyse logboeken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Maken en implementeren van Service Manager web-app service

Microsoft heeft een Service Manager-Web-app voor de lokale Service-beheer verbinding met ITSMC in Azure, op de GitHub gemaakt.

U stelt de ITSM Web-app voor uw Service Manager door het volgende doen:

- **De Web-app implementeren** : de Web-app implementeren, stelt u de eigenschappen en verifiëren met Azure AD. U kunt de web-app implementeren met behulp van de [script geautomatiseerde](log-analytics-itsmc-service-manager-script.md) dat Microsoft u heeft gekregen.
- **Configureer de hybride verbinding** - [configureren van deze verbinding](#configure-the-hybrid-connection)handmatig.

#### <a name="deploy-the-web-app"></a>De web-app implementeren
Gebruik de geautomatiseerde [script](log-analytics-itsmc-service-manager-script.md) stelt u de eigenschappen voor het implementeren van de Web-app en zich verifiëren met Azure AD.

Voer het script door de volgende vereiste gegevens:

- Details van de Azure-abonnement
- Resourcegroepnaam
- Locatie
- Details van Service Manager-server (server name, domein, gebruikersnaam en wachtwoord)
- Site-voorvoegsel voor uw Web-app
- Service Bus-Namespace.

Het script maakt de Web-app met de naam die u hebt opgegeven (samen met enkele extra tekenreeksen uniek te maken). Genereert het **Web-app-URL**, **client-ID** en **clientgeheim**.

Sla de waarden, u deze gebruiken wanneer u een verbinding met ITSMC maakt.

**Controleer de installatie van de Web-app**

1. Ga naar **Azure-portal** > **Resources**.
2. Selecteer de Web-app, klik op **instellingen** > **toepassingsinstellingen**.
3. Controleer de informatie over de Service Manager-instantie die u hebt opgegeven op het moment van de implementatie van de app via het script.

### <a name="configure-the-hybrid-connection"></a>De hybride verbinding configureren

Gebruik de volgende procedure voor het configureren van de hybride verbinding de Service Manager-instantie met ITSMC in Azure tussen.

1. De Service Manager-Web-app onder vinden **Azure-Resources**.
2. Klik op **instellingen** > **Networking**.
3. Onder **hybride verbindingen**, klikt u op **uw hybride-verbindingseindpunten configureren**.

    ![Netwerken voor hybride verbinding](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. In de **hybride verbindingen** blade, klikt u op **hybride verbinding toevoegen**.

    ![Hybride verbinding toevoegen](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. In de **hybride verbindingen toevoegen** blade, klikt u op **maken nieuwe hybride verbinding**.

    ![Nieuwe hybride verbinding](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Typ de volgende waarden:

    - **Eindpuntnaam**: Geef een naam voor de nieuwe hybride verbinding.
    -  **Eindpunt Host**: FQDN-naam van de Service Manager-beheerserver.
    - **Eindpuntpoort**: Typ 5724
    - **Service Bus-naamruimte**: gebruik een bestaande service bus-naamruimte of een nieuwe maken.
    - **Locatie**: Selecteer de locatie.
    -  **Naam**: Geef een naam voor de service bus als u deze maakt.

    ![Waarden voor hybride verbinding](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Klik op **OK** sluiten de **hybride verbinding maken** blade en de hybride verbinding hebt gemaakt.

    Zodra de hybride verbinding is gemaakt, wordt deze weergegeven in de blade.

7. Nadat de hybride verbinding is gemaakt, selecteert u de verbinding en klikt u op **toevoegen geselecteerd hybride verbinding**.

    ![Nieuwe hybride verbinding](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>De listener-instellingen configureren

Gebruik de volgende procedure voor het configureren van de installatie van de listener voor de hybride verbinding.

1. In de **hybride verbindingen** blade, klikt u op **downloaden van het Verbindingsbeheer** en installeer deze op de computer waarop een exemplaar van System Center Service Manager wordt uitgevoerd.

    Zodra de installatie voltooid is, **hybride Verbindingsbeheer UI** optie is beschikbaar onder **Start** menu.

2. Klik op **hybride Verbindingsbeheer UI** , wordt u gevraagd om uw Azure-referenties.

3. Meld u aan met uw Azure-referenties en selecteer uw abonnement waarin de hybride verbinding is gemaakt.

4. Klik op **Opslaan**.

Uw hybride verbinding is verbonden.

![geslaagde hybride verbinding](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Nadat de hybride verbinding is gemaakt, controleren en de verbinding testen door naar de geïmplementeerde Service Manager-Web-app te gaan. Zorg ervoor dat de verbinding is geslaagd voordat u probeert verbinding maken met ITSMC in Azure.

De installatiekopie van het volgende voorbeeld ziet u de details van de verbinding is geslaagd:

![Hybride verbinding testen](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>Verbinding maken tussen ServiceNow met IT Service Management-Connector in Azure

De volgende secties bevatten meer informatie over het verbinding maken met uw ServiceNow-product ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende vereisten wordt voldaan:
- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- ServiceNow ondersteunde versies: ondersteuning, Istanboel, Helsinki, Geneva

**De volgende items in hun ServiceNow-exemplaar moet doen door ServiceNow Admins**:
- Client-ID en clientgeheim voor het product ServiceNow genereren. Zie de volgende informatie zoals vereist voor informatie over het maken van de client-ID en geheim:

    - [Instellen van OAuth voor ondersteuning](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth voor Istanboel instellen](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth voor Helsinki instellen](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth voor Geneva instellen](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)


- Installeer de App gebruiker voor Microsoft OMS-integratie (ServiceNow-app). [Meer informatie](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
- Maak gebruikersrol voor integratie voor de gebruiker app is geïnstalleerd. Informatie over het maken van de gebruikersrol integratie [hier](#create-integration-user-role-in-servicenow-app).

### <a name="connection-procedure"></a>**Verbindingsprocedure**
Gebruik de volgende procedure om een ServiceNow-verbinding te maken:


1. Ga in de Azure-portal naar **alle Resources** en zoekt u **ServiceDesk(YourWorkspaceName)**

2.  Onder **WERKRUIMTE GEGEVENSBRONNEN** klikt u op **ITSM verbindingen**.
    ![Nieuwe verbinding](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Klik boven in het rechterdeelvenster op **toevoegen**.

4. Geef de informatie, zoals beschreven in de volgende tabel en klik op **OK** om de verbinding te maken.


> [!NOTE]
> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het ServiceNow-exemplaar dat u wilt verbinden met ITSMC.  U gebruikt deze naam verderop in OMS wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Type van de accountpartner**   | Selecteer **ServiceNow**. |
| **Gebruikersnaam**   | Typ de gebruikersnaam van de integratie die u in de app ServiceNow ter ondersteuning van de verbinding met ITSMC gemaakt. Meer informatie: [gebruikersrol maken tussen ServiceNow-app](#create-integration-user-role-in-servicenow-app).|
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking**: gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en niet zijn opgeslagen, overal in de service ITSMC.  |
| **Server-URL**   | Typ de URL van het ServiceNow-exemplaar dat u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID die u wilt gebruiken voor OAuth2-authenticatie, wat u eerder hebt gegenereerd.  Meer informatie over het genereren van client-ID en geheim: [OAuth-installatie](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Gegevens synchroniseren bereik**   | Selecteer de ServiceNow-werkitems die u wilt synchroniseren met Azure Log Analytics, via de ITSMC.  De geselecteerde waarden worden geïmporteerd in logboekanalyse.   **Opties:** incidenten en wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt ITSMC betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

![ServiceNow-verbinding](./media/log-analytics-itsmc/itsm-connection-servicenow-connection-latest.png)

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde items uit ServiceNow-exemplaar worden geïmporteerd in Azure **logboekanalyse.** U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.

- U kunt incidenten maken van logboekanalyse waarschuwingen of logboekrecords of waarschuwingen in deze ServiceNow-exemplaar van Azure.

Meer informatie: [werkitems ITSM maken voor logboekanalyse waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [ITSM maken werkitems van logboekanalyse logboeken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>Integratie-gebruikersrol in ServiceNow-app maken

Gebruiker met de volgende procedure:

1.  Ga naar de [ServiceNow store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) en installeer de **gebruiker-App voor ServiceNow en Microsoft OMS-integratie** naar uw ServiceNow-exemplaar.
2.  Ga naar de linkernavigatiebalk ServiceNow-exemplaar, zoeken en selecteer Microsoft OMS integrator na de installatie.  
3.  Klik op **controlelijst voor de installatie**.

    De status wordt weergegeven als **niet voltooien** als de gebruikersrol nog is moet worden gemaakt.

4.  In de tekstvakken naast **integratie van de gebruiker**, geef de gebruikersnaam op voor de gebruiker die verbinding met ITSMC in Azure maken kan.
5.  Voer het wachtwoord voor deze gebruiker en klikt u op **OK**.  

>[!NOTE]

> U gebruikt deze referenties om de verbinding tussen ServiceNow in Azure.

De nieuwe gebruiker wordt weergegeven met de standaardrollen toegewezen.

**Standaard rollen**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   ITIL
-   template_editor
-   view_changer

Als de gebruiker is gemaakt, de status van **controlelijst voor de installatie controleren** wordt verplaatst naar voltooid, de details van de gebruikersrol van de aanbieding gemaakt voor de app.

> [!NOTE]

> Toestaan dat een gebruiker maken **waarschuwingen** en **gebeurtenissen** in ServiceNow van Azure:

> - Zorg ervoor dat u de module Beheer van gebeurtenissen geïnstalleerd hebben op uw ServiceNow-exemplaar.

> - De volgende rollen toevoegen aan de integratiegebruiker:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Verbinding maken met Provance IT Service Management-Connector in Azure

De volgende secties bevatten meer informatie over het verbinding maken met uw product Provance ITSMC in Azure.


### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:


- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Provance App moet worden geregistreerd met Azure AD - en client-ID beschikbaar wordt gesteld. Zie voor gedetailleerde informatie [het configureren van active directory-verificatie](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure om een Provance verbinding te maken:

1. Ga in de Azure-portal naar **alle Resources** en zoekt u **ServiceDesk(YourWorkspaceName)**

2.  Onder **WERKRUIMTE GEGEVENSBRONNEN** klikt u op **ITSM verbindingen**.
    ![Nieuwe verbinding](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Klik boven in het rechterdeelvenster op **toevoegen**.

4. Geef de informatie, zoals beschreven in de volgende tabel en klik op **OK** om de verbinding te maken.

> [!NOTE]

> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het Provance-exemplaar dat u wilt verbinden met ITSMC.  U gebruikt deze naam later wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Type van de accountpartner**   | Selecteer **Provance**. |
| **Gebruikersnaam**   | Typ de naam van de gebruiker verbinding met ITSMC maken kan.    |
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en niet zijn opgeslagen, overal in de service ITSMC. _|
| **Server-URL**   | Typ de URL van uw exemplaar van Provance die u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID voor het verifiëren van deze verbinding die u in uw exemplaar Provance gegenereerd.  Zie voor meer informatie over client-ID [het configureren van active directory-verificatie](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Gegevens synchroniseren bereik**   | Selecteer de Provance werkitems die u wilt synchroniseren met Azure Log Analytics via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk.   **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt ITSMC betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld.|

![Provance verbinding](./media/log-analytics-itsmc/itsm-connections-provance-latest.png)

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde werkitems van dit exemplaar Provance worden geïmporteerd in Azure **logboekanalyse.** U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.

- U kunt incidenten maken van logboekanalyse waarschuwingen of logboekrecords of waarschuwingen van Azure in dit exemplaar Provance.

Meer informatie: [werkitems ITSM maken voor logboekanalyse waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [ITSM maken werkitems van logboekanalyse logboeken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Verbinding maken met Cherwell IT Service Management-Connector in Azure

De volgende secties bevatten meer informatie over het verbinding maken met uw product Cherwell ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Client-ID gegenereerd. Meer informatie: [client-ID genereren voor Cherwell](#generate-client-id-for-cherwell).
- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure om een Provance verbinding te maken:

1. Ga in de Azure-portal naar **alle Resources** en zoekt u **ServiceDesk(YourWorkspaceName)**

2.  Onder **WERKRUIMTE GEGEVENSBRONNEN** klikt u op **ITSM verbindingen**.
    ![Nieuwe verbinding](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Klik boven in het rechterdeelvenster op **toevoegen**.

4. Geef de informatie, zoals beschreven in de volgende tabel en klik op **OK** om de verbinding te maken.

> [!NOTE]

> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het Cherwell-exemplaar dat u wilt verbinding maken met ITSMC.  U gebruikt deze naam later wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Type van de accountpartner**   | Selecteer **Cherwell.** |
| **Gebruikersnaam**   | Typ de gebruikersnaam van Cherwell die verbinding met ITSMC maken kan. |
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en niet zijn opgeslagen, overal in de service ITSMC.|
| **Server-URL**   | Typ de URL van uw exemplaar van Cherwell die u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID voor het verifiëren van deze verbinding die u in uw exemplaar Cherwell gegenereerd.   |
| **Gegevens synchroniseren bereik**   | Selecteer de Cherwell werkitems die u wilt synchroniseren via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk.   **Opties:** incidenten, wijzigingsaanvragen. |
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt ITSMC betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |


![Provance verbinding](./media/log-analytics-itsmc/itsm-connections-cherwell-latest.png)

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde werkitems van dit exemplaar Cherwell worden geïmporteerd in Azure **logboekanalyse.** U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.

- U kunt incidenten maken van logboekanalyse waarschuwingen of logboekrecords of waarschuwingen van Azure in dit exemplaar Cherwell.

Meer informatie: [werkitems ITSM maken voor logboekanalyse waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [ITSM maken werkitems van logboekanalyse logboeken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Client-ID voor Cherwell genereren

Als de client-ID/sleutel gegenereerd voor Cherwell, gebruikt u de volgende procedure:

1. Meld u aan bij uw Cherwell-exemplaar als beheerder.
2. Klik op **beveiliging** > **bewerken REST-API-clientinstellingen**.
3. Selecteer **maken nieuwe client** > **clientgeheim**.

    ![Cherwell gebruikers-id](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Volgende stappen
 - [Werkitems ITSM voor logboekanalyse waarschuwingen maken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts)
 - [ITSM werkitems uit logboekanalyse logboek records logboekbestanden maken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records)
 - [Werkitems ITSM van waarschuwingen van Azure maken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
