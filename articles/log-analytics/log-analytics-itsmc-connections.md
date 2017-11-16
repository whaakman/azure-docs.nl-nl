---
title: Verbindingen met de IT-Service Management-Connector ondersteund in Azure Log Analytics | Microsoft Docs
description: Verbinding maken met uw ITSM producten/services met IT Service Management-Connector in Azure-logboekanalyse centraal kunt bewaken en beheren van de werkitems ITSM.
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
ms.date: 05/29/2017
ms.author: v-jysur
ms.openlocfilehash: e801d484ffb40a0d4aed517a741c45dc76b62b37
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Verbinding maken met ITSM producten/services met IT Service Management-Connector (Preview)
In dit artikel bevat informatie over verbinding maken met uw ITSM product of dienst aan IT Service Management Connector (ITSMC) in OMS en centraal beheren van uw werkitems. Zie voor meer informatie over ITSMC [ITSMC overzicht](log-analytics-itsmc-overview.md).

De volgende producten/services worden ondersteund:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-oms"></a>Verbinding maken met System Center Service Manager IT Service Management-Connector in OMS

De volgende secties bevatten meer informatie over het verbinding maken met uw product System Center Service Manager ITSMC in OMS.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- De Service Manager-webtoepassing (Web-app) is geïmplementeerd en geconfigureerd. Informatie over Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
- Hybride verbinding gemaakt en geconfigureerd. Meer informatie: [configureren van de hybride verbinding](#configure-the-hybrid-connection).
- Ondersteunde versies van Service Manager: 2012 R2 of 2016.
- Gebruikersrol: [geavanceerde operator](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure verbinding maken met uw exemplaar van System Center Service Manager ITSMC:

1. Ga naar **OMS** >**instellingen** > **verbonden gegevensbronnen**.
2. Selecteer **ITSM Connector** klikt u op **nieuwe verbinding toevoegen**.

    ![Servicemanager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Geef de informatie, zoals beschreven in de volgende tabel en klik op **opslaan** om de verbinding te maken:

> [!NOTE]
> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Naam**   | Typ een naam voor de System Center Service Manager-instantie die u wilt verbinden met ITSMC.  U gebruikt deze naam later wanneer u werkitems in dit exemplaar te configureren / gedetailleerde logboekanalyse bekijken. |
| **Selecteer het type verbinding**   | Selecteer **System Center Service Manager**. |
| **Server-URL**   | Typ de URL van de Service Manager-Web-app. Meer informatie over Service Manager-Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Typ de client-ID die u hebt gegenereerd (met behulp van het script voor automatische) voor het verifiëren van de Web-app. Meer informatie over de geautomatiseerde script [hier.](log-analytics-itsmc-service-manager-script.md)|
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Gegevens synchroniseren bereik**   | Selecteer de Service Manager-werkitems die u wilt synchroniseren via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk. **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt OMS betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde werkitems van Service Manager worden geïmporteerd in OMS **logboekanalyse.** U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.

- U kunt van OMS, incidenten van OMS-waarschuwingen of logboek zoeken of waarschuwingen van Azure maken in dit exemplaar van de Service Manager.

Meer informatie: [werkitems ITSM maken voor OMS waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts), [ITSM maken werkitems uit logboeken van OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Maken en implementeren van Service Manager web-app service

Microsoft heeft een Service Manager-Web-app voor de lokale Service-beheer verbinding met ITSMC op OMS, op de GitHub gemaakt.

U stelt de ITSM Web-app voor uw Service Manager door het volgende doen:

- **De Web-app implementeren** : de Web-app implementeren, stelt u de eigenschappen en verifiëren met Azure AD. U kunt de web-app implementeren met behulp van de [script geautomatiseerde](log-analytics-itsmc-service-manager-script.md) dat Microsoft u heeft gekregen.
- **Configureer de hybride verbinding** - [configureren van deze verbinding](#configure-the-hybrid-connection)handmatig.

#### <a name="deploy-the-web-app"></a>De web-app implementeren
Gebruik de geautomatiseerde [script](log-analytics-itsmc-service-manager-script.md) stelt u de eigenschappen voor het implementeren van de Web-app en zich verifiëren met Azure AD.

Voer het script door de volgende vereiste gegevens:

- Details van de Azure-abonnement
- De naam van resourcegroep
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

Gebruik de volgende procedure voor het configureren van de hybride verbinding de Service Manager-instantie met ITSMC in OMS tussen.

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

> Nadat de hybride verbinding is gemaakt, controleren en de verbinding testen door naar de geïmplementeerde Service Manager-Web-app te gaan. Zorg ervoor dat de verbinding is geslaagd voordat u probeert verbinding maken met ITSMC in OMS.

De volgende afbeelding ziet u de details van de verbinding is geslaagd:

![Hybride verbinding testen](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-oms"></a>Verbinding maken tussen ServiceNow met IT Service Management-Connector in OMS

De volgende secties bevatten meer informatie over het verbinding maken met uw ServiceNow-product ITSMC in OMS.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- ServiceNow versies – Fuji, Geneva, Helsinki ondersteund.

ServiceNow Admins moet in hun ServiceNow-exemplaar het volgende doen:
- Client-ID en clientgeheim voor het product ServiceNow genereren. Zie voor meer informatie over het maken van de client-ID en geheim [OAuth-installatie](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Installeer de App gebruiker voor Microsoft OMS-integratie (ServiceNow-app). [Meer informatie](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Maak gebruikersrol voor integratie voor de gebruiker app is geïnstalleerd. Informatie over het maken van de gebruikersrol integratie [hier](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Verbindingsprocedure**
Gebruik de volgende procedure om een ServiceNow-verbinding te maken:

1. Ga naar **OMS** > **instellingen** > **verbonden gegevensbronnen**.
2. Selecteer **ITSM Connector** klikt u op **nieuwe verbinding toevoegen**.

    ![ServiceNow-verbinding](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Geef de informatie, zoals beschreven in de volgende tabel en klik op **opslaan** om de verbinding te maken:

> [!NOTE]
> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Naam**   | Typ een naam voor het ServiceNow-exemplaar dat u wilt verbinden met ITSMC.  U gebruikt deze naam verderop in OMS wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Selecteer het type verbinding**   | Selecteer **ServiceNow**. |
| **Gebruikersnaam**   | Typ de gebruikersnaam van de integratie die u in de app ServiceNow ter ondersteuning van de verbinding met ITSMC gemaakt. Meer informatie: [gebruikersrol maken tussen ServiceNow-app](#create-integration-user-role-in-servicenow-app).|
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking**: gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en worden niet overal opgeslagen in de OMS-service.  |
| **Server-URL**   | Typ de URL van het ServiceNow-exemplaar dat u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID die u wilt gebruiken voor OAuth2-authenticatie, wat u eerder hebt gegenereerd.  Meer informatie over het genereren van client-ID en geheim: [OAuth-installatie](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Gegevens synchroniseren bereik**   | Selecteer de ServiceNow-werkitems die u wilt synchroniseren met OMS via de ITSMC.  De geselecteerde waarden worden geïmporteerd in logboekanalyse.   **Opties:** incidenten en wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt OMS betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |


**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Werk items uit ServiceNow-verbinding worden geïmporteerd in de OMS-logboekanalyse geselecteerd.  U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.
- U kunt de incidenten, waarschuwingen en gebeurtenissen maken vanaf OMS waarschuwingen logboek zoeken of waarschuwingen in deze ServiceNow-exemplaar van Azure.  


Meer informatie: [werkitems ITSM maken voor OMS waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts), [ITSM maken werkitems uit logboeken van OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>Integratie-gebruikersrol in ServiceNow-app maken

Gebruiker met de volgende procedure:

1.  Ga naar de [ServiceNow store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) en installeer de **gebruiker-App voor ServiceNow en Microsoft OMS-integratie** naar uw ServiceNow-exemplaar.
2.  Ga naar de linkernavigatiebalk ServiceNow-exemplaar, zoeken en selecteer Microsoft OMS integrator na de installatie.  
3.  Klik op **controlelijst voor de installatie**.

    De status wordt weergegeven als **niet voltooien** als de gebruikersrol nog is moet worden gemaakt.

4.  In de tekstvakken naast **integratie van de gebruiker**, geef de gebruikersnaam op voor de gebruiker die verbinding met ITSMC in OMS maken kan.
5.  Voer het wachtwoord voor deze gebruiker en klikt u op **OK**.  

>[!NOTE]

> U gebruikt deze referenties om de verbinding tussen ServiceNow in OMS.

De nieuwe gebruiker wordt weergegeven met de standaardrollen toegewezen.

**Standaard rollen**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   ITIL
-   template_editor
-   view_changer

Als de gebruiker is gemaakt, de status van **controlelijst voor de installatie controleren** wordt verplaatst naar voltooid, de details van de gebruikersrol van de aanbieding gemaakt voor de app.

> [!NOTE]

> Toestaan dat een gebruiker maken **waarschuwingen** en **gebeurtenissen** in ServiceNow van OMS:

> - Zorg ervoor dat u de module Beheer van gebeurtenissen geïnstalleerd hebben op uw ServiceNow-exemplaar.

> - De volgende rollen toevoegen aan de integratiegebruiker:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-oms"></a>Verbinding maken met Provance IT Service Management-Connector in OMS

De volgende secties bevatten meer informatie over het verbinding maken met uw product Provance ITSMC in OMS.


### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:


- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Provance App moet worden geregistreerd met Azure AD - en client-ID beschikbaar wordt gesteld. Zie voor gedetailleerde informatie [het configureren van active directory-verificatie](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure om een Provance verbinding te maken:

1. Ga naar **OMS** > **instellingen** > **verbonden gegevensbronnen**.
2. Selecteer **ITSM Connector** klikt u op **nieuwe verbinding toevoegen**.  

    ![Provance verbinding](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Geef de informatie, zoals beschreven in de volgende tabel en klik op **opslaan** om de verbinding te maken.

> [!NOTE]
> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Naam**   | Typ een naam voor het Provance-exemplaar dat u wilt verbinden met ITSMC.  U gebruikt deze naam verderop in OMS wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Selecteer het type verbinding**   | Selecteer **Provance**. |
| **Gebruikersnaam**   | Typ de naam van de gebruiker verbinding met ITSMC maken kan.    |
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en worden niet overal opgeslagen in de OMS-service. _|
| **Server-URL**   | Typ de URL van uw exemplaar van Provance die u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID voor het verifiëren van deze verbinding die u in uw exemplaar Provance gegenereerd.  Zie voor meer informatie over client-ID [het configureren van active directory-verificatie](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Gegevens synchroniseren bereik**   | Selecteer de Provance werkitems die u wilt synchroniseren met OMS via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk.   **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt OMS betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld.|

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Geselecteerde werkitems van Provance verbinding worden geïmporteerd in OMS **logboekanalyse.**  U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.
- U kunt vanaf OMS waarschuwingen logboek zoeken of waarschuwingen van Azure in dit exemplaar Provance incidenten en gebeurtenissen maken.

Meer informatie: [werkitems ITSM maken voor OMS waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts), [ITSM maken werkitems uit logboeken van OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-oms"></a>Verbinding maken met Cherwell IT Service Management-Connector in OMS

De volgende secties bevatten meer informatie over het verbinding maken met uw product Cherwell ITSMC in OMS.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de IT-Service Management-Connector oplossing](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Client-ID gegenereerd. Meer informatie: [client-ID genereren voor Cherwell](#generate-client-id-for-cherwell).
- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>Verbindingsprocedure

Gebruik de volgende procedure om een Cherwell verbinding te maken:

1. Ga naar **OMS** >  **instellingen** > **verbonden gegevensbronnen**.
2. Selecteer **ITSM Connector** klikt u op **nieuwe verbinding toevoegen**.  

    ![Cherwell gebruikers-id](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Geef de informatie, zoals beschreven in de volgende tabel en klik op **opslaan** om de verbinding te maken.

> [!NOTE]
> Alle volgende parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Naam**   | Typ een naam voor het Cherwell-exemplaar dat u wilt verbinding maken met ITSMC.  U gebruikt deze naam verderop in OMS wanneer u werkitems in deze ITSM configureren / gedetailleerde logboekanalyse bekijken. |
| **Selecteer het type verbinding**   | Selecteer **Cherwell.** |
| **Gebruikersnaam**   | Typ de gebruikersnaam van Cherwell die verbinding met ITSMC maken kan. |
| **Wachtwoord**   | Typ het wachtwoord die zijn gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord voor het genereren van verificatietokens alleen worden gebruikt, en worden niet overal opgeslagen in de OMS-service.|
| **Server-URL**   | Typ de URL van uw exemplaar van Cherwell die u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ op de client-ID voor het verifiëren van deze verbinding die u in uw exemplaar Cherwell gegenereerd.   |
| **Gegevens synchroniseren bereik**   | Selecteer de Cherwell werkitems die u wilt synchroniseren via ITSMC.  Deze items worden geïmporteerd in logboekanalyse werk.   **Opties:** incidenten, wijzigingsaanvragen. |
| **Gegevens synchroniseren** | Typ het aantal voorbije dagen die u wilt dat de gegevens uit. **Maximum aantal**: 120 dagen. |
| **Nieuw configuratie-item maken in ITSM oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in het product ITSM. Wanneer u selecteert, maakt OMS betrokken configuratie-items als configuratie-items (in geval van een niet-bestaande configuratie-items) in het ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

**Wanneer verbinding gemaakt en gesynchroniseerd**:

- Werk items uit deze verbinding Cherwell worden geïmporteerd in de OMS-logboekanalyse geselecteerd. U kunt de samenvatting van deze weergeven werkitems op de tegel IT Service Management-Connector.
- U kunt vanaf OMS waarschuwingen logboek zoeken of waarschuwingen van Azure in dit exemplaar Cherwell incidenten en gebeurtenissen maken.

Meer informatie: [werkitems ITSM maken voor OMS waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts), [ITSM maken werkitems uit logboeken van OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs) en [werkitems ITSM maken van waarschuwingen van Azure](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Client-ID voor Cherwell genereren

Als de client-ID/sleutel gegenereerd voor Cherwell, gebruikt u de volgende procedure:

1. Meld u aan bij uw Cherwell-exemplaar als beheerder.
2. Klik op **beveiliging** > **bewerken REST-API-clientinstellingen**.
3. Selecteer **maken nieuwe client** > **clientgeheim**.

    ![Cherwell gebruikers-id](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Volgende stappen
 - [Werkitems ITSM voor OMS waarschuwingen maken](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)
 - [Maken van werkitems ITSM van OMS-Logboeken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)
 - [Werkitems ITSM van waarschuwingen van Azure maken](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
 - [Logboekanalyse weergave voor de verbinding](log-analytics-itsmc-overview.md#using-the-solution)
