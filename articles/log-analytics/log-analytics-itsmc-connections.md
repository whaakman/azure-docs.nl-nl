---
title: Verbindingen met de IT Service Management-Connector wordt ondersteund in Azure Log Analytics | Microsoft Docs
description: In dit artikel bevat informatie over het verbinden van uw ITSM-producten en services met de IT Service Management Connector (ITSMC) in Azure Monitor centraal bewaken en beheren van de ITSM-werkitems.
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.component: ''
ms.openlocfilehash: e504f8c4fb34084d613e7ab1e506187a856defef
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961837"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Verbinding maken met ITSM-producten en services met IT Service Management-Connector
In dit artikel bevat informatie over het configureren van de verbinding tussen uw ITSM-product/service en de IT Service Management-Connector (ITSMC) in Log Analytics om uw werkitems centraal te beheren. Zie voor meer informatie over ITSMC [overzicht](log-analytics-itsmc-overview.md).

De volgende ITSM-producten en services worden ondersteund. Selecteer het product om gedetailleerde informatie over hoe u verbinding maken met het product ITSMC weer te geven.

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]

> ITSM-Connector kan alleen verbinding maken met de cloud-gebaseerde ServiceNow-exemplaren. On-premises ServiceNow-instanties zijn momenteel niet ondersteund.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>Verbinding maken met System Center Service Manager IT Service Management-Connector in Azure

De volgende secties bevatten informatie over hoe u verbinding maken met uw System Center Service Manager-product ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de oplossing IT Service Management Connector](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- De Service Manager-webtoepassing (Web-app) is geïmplementeerd en geconfigureerd. Informatie over de Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
- Hybride verbinding hebt gemaakt en geconfigureerd. Meer informatie: [configureren van de hybride verbinding](#configure-the-hybrid-connection).
- Ondersteunde versies van Service Manager: 2012 R2 of 2016.
- Gebruikersrol: [geavanceerde operator](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>-Verbindingsprocedure

Gebruik de volgende procedure verbinding maken met uw exemplaar van System Center Service Manager ITSMC:

1. In Azure portal, gaat u naar **alle Resources** en zoek naar **ServiceDesk(YourWorkspaceName)**

2.  Onder **GEGEVENSBRONNEN voor WERKRUIMTE** klikt u op **ITSM-verbindingen**.

    ![Nieuwe verbinding](media/log-analytics-itsmc-connections/add-new-itsm-connection.png)

3. Aan de bovenkant van het rechter deelvenster, klikt u op **toevoegen**.

4. Geef de informatie zoals beschreven in de volgende tabel, en klikt u op **OK** om de verbinding te maken.

> [!NOTE]

> Alle deze parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor de System Center Service Manager-instantie die u wilt verbinden met ITSMC.  U gebruikt deze naam later wanneer u werkitems in dit geval configureren / gedetailleerd logboek analytics weergeven. |
| **Partnertype**   | Selecteer **System Center Service Manager**. |
| **Server-URL**   | Typ de URL van de Service Manager-Web-app. Meer informatie over Service Manager-Web-app is [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Typ de client-ID die u hebt gegenereerd (met behulp van het script voor automatische) voor het verifiëren van de Web-app. Meer informatie over de geautomatiseerd script [hier.](log-analytics-itsmc-service-manager-script.md)|
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Bereik voor gegevenssynchronisatie**   | Selecteer de Service Manager-werkitems die u wilt synchroniseren via ITSMC.  Deze items worden geïmporteerd in Log Analytics werk. **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal afgelopen dagen die u wilt dat de gegevens uit. **Maximumlimiet**: 120 dagen. |
| **Nieuwe configuratie-item maken in de ITSM-oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in de ITSM-product. Als u selecteert, maakt Log Analytics de betrokken configuratie-items als configuratie-items (in het geval van niet-bestaande configuratie-items) in de ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

![Service manager-verbinding](media/log-analytics-itsmc-connections/service-manager-connection.png)

**Wanneer er verbinding gemaakt met en gesynchroniseerd**:

- Geselecteerde werkitems van Service Manager zijn geïmporteerd naar Azure **Log Analytics.** U kunt een overzicht bekijken van deze taken op de tegel IT Service Management-Connector.

- U kunt incidenten maken van Log Analytics-waarschuwingen of records in logboek registreren of Azure-waarschuwingen in dit exemplaar van Service Manager.


Meer informatie: [maken ITSM-werkitems van Azure-waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Maken en implementeren van Service Manager-service voor web-app

Als u wilt verbinding maken met de on-premises Service Manager met ITSMC in Azure, heeft Microsoft een Service Manager-Web-app gemaakt op de GitHub.

Om in te stellen de ITSM-Web-app voor uw Service Manager, het volgende doen:

- **De Web-app implementeren** : de Web-app implementeren, stelt u de eigenschappen en verifiëren met Azure AD. U kunt de web-app implementeren met behulp van de [geautomatiseerd script](log-analytics-itsmc-service-manager-script.md) dat Microsoft u heeft gekregen.
- **Configureren van de hybride verbinding** - [configureren van deze verbinding](#configure-the-hybrid-connection)handmatig.

#### <a name="deploy-the-web-app"></a>De web-app implementeren
Gebruik de geautomatiseerde [script](log-analytics-itsmc-service-manager-script.md) voor het implementeren van de Web-app, stel de eigenschappen en verifiëren met Azure AD.

Voer het script door te geven van de volgende vereiste gegevens:

- Details van de Azure-abonnement
- Resourcegroepnaam
- Locatie
- Service Manager-server-gegevens (naam, domein, gebruikersnaam en wachtwoord)
- Voorvoegsel van de site de naam voor uw Web-app
- Service Bus-Namespace.

Het script maakt de Web-app met behulp van de naam die u hebt opgegeven (samen met enkele extra tekenreeksen uniek te maken). Het genereert de **Web-app-URL**, **client-ID** en **clientgeheim**.

Sla de waarden, u deze gebruiken wanneer u een verbinding met ITSMC maakt.

**Controleer de installatie van de Web-app**

1. Ga naar **Azure-portal** > **Resources**.
2. Selecteer de Web-app, klikt u op **instellingen** > **toepassingsinstellingen**.
3. Controleer of de informatie over de Service Manager-instantie die u hebt opgegeven op het moment van de implementatie van de app via het script.

### <a name="configure-the-hybrid-connection"></a>Configureren van de hybride verbinding

Gebruik de volgende procedure om de hybride verbinding die verbinding maakt van de Service Manager-instantie met ITSMC in Azure te configureren.

1. De Service Manager-Web-app onder vinden **Azure-Resources**.
2. Klik op **instellingen** > **netwerken**.
3. Onder **hybride verbindingen**, klikt u op **uw hybride-verbindingseindpunten configureren**.

    ![Netwerken voor hybride verbinding](media/log-analytics-itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. In de **hybride verbindingen** blade, klikt u op **hybride verbinding toevoegen**.

    ![Hybride verbinding toevoegen](media/log-analytics-itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. In de **hybride verbindingen toevoegen** blade, klikt u op **maken nieuwe hybride verbinding**.

    ![Nieuwe hybride verbinding](media/log-analytics-itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Typ de volgende waarden:

    - **Naam van het eindpunt**: Geef een naam voor de nieuwe hybride verbinding.
    -  **Host van het eindpunt**: FQDN-naam van de Service Manager-beheerserver.
    - **Poort van het eindpunt**: Typ 5724
    - **Service Bus-naamruimte**: gebruik een bestaande service bus-naamruimte of een nieuwe maken.
    - **Locatie**: Selecteer de locatie.
    -  **Naam**: Geef een naam op voor de service bus als u deze maakt.

    ![Hybride verbindingswaarden](media/log-analytics-itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Klik op **OK** sluiten de **hybride verbinding maken** blade en beginnen met het maken van de hybride verbinding.

    Zodra de hybride verbinding is gemaakt, wordt deze weergegeven op de blade.

7. Nadat de hybride verbinding is gemaakt, selecteert u de verbinding en klikt u op **toevoegen geselecteerde hybride verbinding**.

    ![Nieuwe hybride verbinding](media/log-analytics-itsmc-connections/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>De listener-instellingen configureren

Gebruik de volgende procedure voor het configureren van de setup van listener voor de hybride verbinding.

1. In de **hybride verbindingen** blade, klikt u op **downloaden van het Verbindingsbeheer** en installeer deze op de computer waarop de System Center Service Manager-exemplaar wordt uitgevoerd.

    Zodra de installatie voltooid is, **Hybrid Connection Manager UI** optie is beschikbaar onder **Start** menu.

2. Klik op **Hybrid Connection Manager UI** , wordt u gevraagd voor uw Azure-referenties.

3. Meld u aan met uw Azure-referenties en selecteer uw abonnement waar de hybride verbinding is gemaakt.

4. Klik op **Opslaan**.

De hybride verbinding is verbonden.

![geslaagde hybride verbinding](media/log-analytics-itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Nadat de hybride verbinding is gemaakt, controleren en de verbinding testen door naar de geïmplementeerde Service Manager-Web-app te gaan. Zorg ervoor dat de verbinding is geslaagd voordat u probeert verbinding maken met ITSMC in Azure.

Het volgende voorbeeldafbeelding ziet u de details van de verbinding is geslaagd:

![Hybride verbinding is getest](media/log-analytics-itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>Verbinding maken met ServiceNow met IT Service Management-Connector in Azure

De volgende secties bevatten informatie over hoe u verbinding maken met uw ServiceNow-product ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende vereisten wordt voldaan:
- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de oplossing IT Service Management Connector](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- ServiceNow ondersteunde versies: Kingston, Jakarta, Istanboel, Helsinki, Genève.

**ServiceNow Admins moet het volgende in de ServiceNow-exemplaar doen**:
- Client-ID en clientgeheim voor de ServiceNow-product te genereren. Zie de volgende informatie zoals vereist voor meer informatie over het maken van de client-ID en -geheim:

    - [Instellen van OAuth voor Kingston](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Instellen van OAuth voor Jakarta](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Instellen van OAuth voor Istanboel](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Instellen van OAuth voor Helsinki](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Instellen van OAuth voor Geneva](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)


- Installeer de gebruikers-App voor Microsoft Log Analytics-integratie (ServiceNow-app). [Meer informatie](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
- Maak integratie van de rol van de gebruiker naar de gebruiker geïnstalleerd. Informatie over het maken van de gebruikersrol integratie [hier](#create-integration-user-role-in-servicenow-app).

### <a name="connection-procedure"></a>**-Verbindingsprocedure**
Gebruik de volgende procedure om een ServiceNow-verbinding te maken:


1. In Azure portal, gaat u naar **alle Resources** en zoek naar **ServiceDesk(YourWorkspaceName)**

2.  Onder **GEGEVENSBRONNEN voor WERKRUIMTE** klikt u op **ITSM-verbindingen**.
    ![Nieuwe verbinding](media/log-analytics-itsmc-connections/add-new-itsm-connection.png)

3. Aan de bovenkant van het rechter deelvenster, klikt u op **toevoegen**.

4. Geef de informatie zoals beschreven in de volgende tabel, en klikt u op **OK** om de verbinding te maken.


> [!NOTE]
> Alle deze parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het ServiceNow-exemplaar dat u wilt verbinden met ITSMC.  U kunt deze naam later in Log Analytics gebruiken wanneer u configuratie-items in deze ITSM configureren / gedetailleerd logboek analytics weergeven. |
| **Partnertype**   | Selecteer **ServiceNow**. |
| **Gebruikersnaam**   | Typ de naam van de integratie-gebruiker die u hebt gemaakt in de ServiceNow-app voor de ondersteuning van de verbinding met ITSMC. Meer informatie: [gebruikersrol maken tussen ServiceNow-app](#create-integration-user-role-in-servicenow-app).|
| **Wachtwoord**   | Typ het wachtwoord dat is gekoppeld aan deze gebruikersnaam. **Houd er rekening mee**: gebruikersnaam en wachtwoord worden gebruikt voor het genereren van verificatietokens alleen en worden niet overal opgeslagen in de ITSMC-service.  |
| **Server-URL**   | Typ de URL van het ServiceNow-exemplaar dat u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ de client-ID die u wilt gebruiken voor OAuth2-verificatie, die u eerder hebt gegenereerd.  Meer informatie over het genereren van de client-ID en -geheim: [OAuth-installatie](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Clientgeheim**   | Typ het clientgeheim gegenereerd voor deze ID.   |
| **Bereik voor gegevenssynchronisatie**   | Selecteer de ServiceNow-werkitems die u wilt synchroniseren met Azure Log Analytics, via de ITSMC.  De geselecteerde waarden worden geïmporteerd in log analytics.   **Opties:** incidenten en wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal afgelopen dagen die u wilt dat de gegevens uit. **Maximumlimiet**: 120 dagen. |
| **Nieuwe configuratie-item maken in de ITSM-oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in de ITSM-product. Als u selecteert, maakt ITSMC de betrokken configuratie-items als configuratie-items (in het geval van niet-bestaande configuratie-items) in de ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |

![ServiceNow-verbinding](media/log-analytics-itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Wanneer er verbinding gemaakt met en gesynchroniseerd**:

- Geselecteerde werkitems van ServiceNow-exemplaar worden geïmporteerd in Azure **Log Analytics.** U kunt een overzicht bekijken van deze taken op de tegel IT Service Management-Connector.

- U kunt incidenten maken van Log Analytics-waarschuwingen of records in logboek registreren of Azure-waarschuwingen in deze ServiceNow-exemplaar.

Meer informatie: [maken ITSM-werkitems van Azure-waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>Integratie van een gebruikersrol in ServiceNow-app maken

Gebruiker met de volgende procedure:

1.  Ga naar de [ServiceNow store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) en installeer de **gebruiker-App voor ServiceNow en Microsoft OMS Integration** in uw ServiceNow-exemplaar.
   
   >[!NOTE]
   >Als onderdeel van de lopende overgang van Microsoft Operations Management Suite (OMS) naar Azure Monitor, wordt OMS nu aangeduid als Log Analytics.     
2.  Na de installatie, gaat u naar de linker navigatiebalk van het ServiceNow-exemplaar, zoeken en selecteer Microsoft OMS integrator.  
3.  Klik op **controlelijst voor de installatie**.

    De status wordt weergegeven als **niet worden voltooid** als de gebruikersrol nog is moet worden gemaakt.

4.  In de tekstvakken naast **maken de integratiegebruiker**, voer de gebruikersnaam voor de gebruiker die verbinding met ITSMC in Azure maken kan.
5.  Voer het wachtwoord voor deze gebruiker en klikt u op **OK**.  

>[!NOTE]

> U gebruikt deze referenties voor het maken van het ServiceNow-verbinding in Azure.

De nieuwe gebruiker wordt weergegeven met de standaardrollen is toegewezen.

**Standaard rollen**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   ITIL
-   template_editor
-   view_changer

Als de gebruiker is gemaakt, de status van **controlelijst voor de installatie controleren** wordt verplaatst naar voltooid, de details van de gebruikersrol van de aanbieding hebt gemaakt voor de app.

> [!NOTE]

> ITSM-Connector kunt incidenten verzenden naar ServiceNow zonder alle modules die op uw ServiceNow-exemplaar is geïnstalleerd. Als u van EventManagement module in uw ServiceNow-exemplaar gebruikmaakt en gebeurtenissen of waarschuwingen in ServiceNow met behulp van de connector maken, moet u de volgende rollen toevoegen aan de van integratiegebruiker:

>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Verbinding maken met Provance IT Service Management-Connector in Azure

De volgende secties bevatten informatie over hoe u verbinding maken met uw product Provance ITSMC in Azure.


### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:


- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de oplossing IT Service Management Connector](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Provance App moet worden geregistreerd bij Azure AD - en client-ID beschikbaar wordt gesteld. Zie voor gedetailleerde informatie [active directory-verificatie configureren](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md).

- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>-Verbindingsprocedure

Gebruik de volgende procedure om een Provance verbinding te maken:

1. In Azure portal, gaat u naar **alle Resources** en zoek naar **ServiceDesk(YourWorkspaceName)**

2.  Onder **GEGEVENSBRONNEN voor WERKRUIMTE** klikt u op **ITSM-verbindingen**.
    ![Nieuwe verbinding](media/log-analytics-itsmc-connections/add-new-itsm-connection.png)

3. Aan de bovenkant van het rechter deelvenster, klikt u op **toevoegen**.

4. Geef de informatie zoals beschreven in de volgende tabel, en klikt u op **OK** om de verbinding te maken.

> [!NOTE]

> Alle deze parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het Provance-exemplaar dat u wilt verbinden met ITSMC.  U gebruikt deze naam later wanneer u configuratie-items in deze ITSM configureren / gedetailleerd logboek analytics weergeven. |
| **Partnertype**   | Selecteer **Provance**. |
| **Gebruikersnaam**   | Typ de naam van de gebruiker die verbinding met ITSMC maken kan.    |
| **Wachtwoord**   | Typ het wachtwoord dat is gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord worden gebruikt voor het genereren van verificatietokens alleen en worden niet overal opgeslagen in de service ITSMC. _|
| **Server-URL**   | Typ de URL van uw Provance-exemplaar dat u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ de client-ID voor het verifiëren van deze verbinding, die u in uw exemplaar Provance gegenereerd.  Zie voor meer informatie over client-ID, [active directory-verificatie configureren](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Bereik voor gegevenssynchronisatie**   | Selecteer de Provance werkitems die u wilt synchroniseren met Azure Log Analytics, via ITSMC.  Deze werk-items worden geïmporteerd in log analytics.   **Opties:** incidenten, wijzigingsaanvragen.|
| **Gegevens synchroniseren** | Typ het aantal afgelopen dagen die u wilt dat de gegevens uit. **Maximumlimiet**: 120 dagen. |
| **Nieuwe configuratie-item maken in de ITSM-oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in de ITSM-product. Als u selecteert, maakt ITSMC de betrokken configuratie-items als configuratie-items (in het geval van niet-bestaande configuratie-items) in de ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld.|

![Provance verbinding](media/log-analytics-itsmc-connections/itsm-connections-provance-latest.png)

**Wanneer er verbinding gemaakt met en gesynchroniseerd**:

- Geselecteerde werkitems van dit exemplaar Provance worden geïmporteerd in Azure **Log Analytics.** U kunt een overzicht bekijken van deze taken op de tegel IT Service Management-Connector.

- U kunt incidenten maken van Log Analytics-waarschuwingen of records in logboek registreren of Azure-waarschuwingen in dit geval Provance.

Meer informatie: [maken ITSM-werkitems van Azure-waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Verbinding maken met Cherwell IT Service Management-Connector in Azure

De volgende secties bevatten informatie over hoe u verbinding maken met uw product Cherwell ITSMC in Azure.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan:

- ITSMC geïnstalleerd. Meer informatie: [toevoegen van de oplossing IT Service Management Connector](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Client-ID die is gegenereerd. Meer informatie: [client-ID genereren voor Cherwell](#generate-client-id-for-cherwell).
- Gebruikersrol: beheerder.

### <a name="connection-procedure"></a>-Verbindingsprocedure

Gebruik de volgende procedure om een Provance verbinding te maken:

1. In Azure portal, gaat u naar **alle Resources** en zoek naar **ServiceDesk(YourWorkspaceName)**

2.  Onder **GEGEVENSBRONNEN voor WERKRUIMTE** klikt u op **ITSM-verbindingen**.
    ![Nieuwe verbinding](media/log-analytics-itsmc-connections/add-new-itsm-connection.png)

3. Aan de bovenkant van het rechter deelvenster, klikt u op **toevoegen**.

4. Geef de informatie zoals beschreven in de volgende tabel, en klikt u op **OK** om de verbinding te maken.

> [!NOTE]

> Alle deze parameters zijn verplicht.

| **Veld** | **Beschrijving** |
| --- | --- |
| **Verbindingsnaam**   | Typ een naam voor het Cherwell-exemplaar dat u wilt verbinding maken met ITSMC.  U gebruikt deze naam later wanneer u configuratie-items in deze ITSM configureren / gedetailleerd logboek analytics weergeven. |
| **Partnertype**   | Selecteer **Cherwell.** |
| **Gebruikersnaam**   | Typ de naam van de Cherwell-gebruiker die verbinding met ITSMC maken kan. |
| **Wachtwoord**   | Typ het wachtwoord dat is gekoppeld aan deze gebruikersnaam. **Opmerking:** gebruikersnaam en wachtwoord worden gebruikt voor het genereren van verificatietokens alleen en worden niet overal opgeslagen in de ITSMC-service.|
| **Server-URL**   | Typ de URL van uw Cherwell-exemplaar dat u wilt verbinding maken met ITSMC. |
| **Client-ID**   | Typ de client-ID voor het verifiëren van deze verbinding, die u hebt gegenereerd in uw exemplaar Cherwell.   |
| **Bereik voor gegevenssynchronisatie**   | Selecteer de Cherwell werkitems die u wilt synchroniseren via ITSMC.  Deze werk-items worden geïmporteerd in log analytics.   **Opties:** incidenten, wijzigingsaanvragen. |
| **Gegevens synchroniseren** | Typ het aantal afgelopen dagen die u wilt dat de gegevens uit. **Maximumlimiet**: 120 dagen. |
| **Nieuwe configuratie-item maken in de ITSM-oplossing** | Selecteer deze optie als u wilt maken van de configuratie-items in de ITSM-product. Als u selecteert, maakt ITSMC de betrokken configuratie-items als configuratie-items (in het geval van niet-bestaande configuratie-items) in de ondersteunde ITSM-systeem. **Standaard**: uitgeschakeld. |


![Provance verbinding](media/log-analytics-itsmc-connections/itsm-connections-cherwell-latest.png)

**Wanneer er verbinding gemaakt met en gesynchroniseerd**:

- Geselecteerde werkitems van dit exemplaar Cherwell worden geïmporteerd in Azure **Log Analytics.** U kunt een overzicht bekijken van deze taken op de tegel IT Service Management-Connector.

- U kunt incidenten maken van Log Analytics-waarschuwingen of records in logboek registreren of Azure-waarschuwingen in dit geval Cherwell.

Meer informatie: [maken ITSM-werkitems van Azure-waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Client-ID voor Cherwell genereren

Voor het genereren van de client-ID-sleutel voor Cherwell, gebruikt u de volgende procedure:

1. Meld u aan bij uw exemplaar Cherwell als beheerder.
2. Klik op **Security** > **bewerken REST-API-clientinstellingen**.
3. Selecteer **maken nieuwe client** > **clientgeheim**.

    ![Cherwell gebruikers-id](media/log-analytics-itsmc-connections/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Volgende stappen
 - [ITSM-werkitems maken vanuit Azure-waarschuwingen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
