---
title: Office 365-oplossing in de Operations Management Suite (OMS) | Microsoft Docs
description: Dit artikel bevat informatie over configuratie en gebruik van de Office 365-oplossing in OMS.  Het bevat een gedetailleerde beschrijving van de Office 365-records gemaakt in logboekanalyse.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 711071eaff7ab5e5199793663aa3cbb36a1e8d8a
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Office 365-oplossing in de Operations Management Suite (OMS)

![Office 365-logo](media/oms-solution-office-365/icon.png)

De Office 365-oplossing voor Operations Management Suite (OMS) kunt u uw Office 365-omgeving in logboekanalyse bewaken.  

- Controleren van gebruikersactiviteiten op uw Office 365-accounts te analyseren gebruikspatronen en trends voor gebruikersgedrag. U kunt bijvoorbeeld specifieke gebruiksscenario's, zoals bestanden die worden gedeeld buiten uw organisatie of de meest populaire SharePoint-sites ophalen.
- Activiteiten bijhouden van wijzigingen in de configuratie of hoge bevoegdheid bewerkingen beheerder bewaken.
- Detecteren en onderzoeken van ongewenste gebruikersgedrag die behoeften van uw organisatie kan worden aangepast.
- Controle en naleving demonstreren. U kunt bijvoorbeeld toegang bestandsbewerkingen op vertrouwelijke bestanden, wat u bij de controle en naleving waarborgen helpen kunnen bewaken.
- Uitvoeren van operationele problemen oplossen met behulp van OMS zoeken boven op Office 365-activiteitsgegevens van uw organisatie.

## <a name="prerequisites"></a>Vereisten
Het volgende is vereist voordat u deze oplossing wordt geïnstalleerd en geconfigureerd.

- Organisatie Office 365-abonnement.
- Referenties voor een gebruikersaccount dat een globale beheerder.
- Audit om gegevens te ontvangen, moet u [controle configureren](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) in uw Office 365-abonnement.  Houd er rekening mee dat [Postvakcontrole](https://technet.microsoft.com/library/dn879651.aspx) wordt afzonderlijk geconfigureerd.  U kunt nog steeds installeren van de oplossing en andere gegevens verzamelen als controle is niet geconfigureerd.
 


## <a name="management-packs"></a>Management packs
Deze oplossing wordt niet geïnstalleerd voor alle management packs in de verbonden beheergroepen.
  

## <a name="configuration"></a>Configuratie
Eenmaal u [de Office 365-oplossing aan uw abonnement toevoegt](../log-analytics/log-analytics-add-solutions.md), u verbinding maakt met uw Office 365-abonnement hebt.

1. De oplossing voor beheer van waarschuwingen toevoegen aan uw OMS-werkruimte met behulp van de procedure beschreven in [oplossingen toevoegen](../log-analytics/log-analytics-add-solutions.md).
2. Ga naar **instellingen** in de OMS-portal.
3. Onder **verbonden bronnen**, selecteer **Office 365**.
4. Klik op **verbinding maken met Office 365**.<br>![Processtappen verbinden Office 365](media/oms-solution-office-365/configure.png)
5. Meld u aan bij Office 365 met een account dat een globale beheerder voor uw abonnement. 
6. Het abonnement wordt vermeld met de werklasten die de oplossing zal worden gecontroleerd.<br>![Processtappen verbinden Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Gegevensverzameling
### <a name="supported-agents"></a>Ondersteunde agents
De Office 365-oplossing niet ophalen van gegevens uit een van de [OMS agents](../log-analytics/log-analytics-data-sources.md).  Het ophalen van gegevens rechtstreeks vanuit Office 365.

### <a name="collection-frequency"></a>Verzamelingsfrequentie
Office 365 verzendt een [webhook melding](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) met gedetailleerde gegevens met logboekanalyse telkens wanneer een record wordt gemaakt.

## <a name="using-the-solution"></a>De oplossing gebruiken
Wanneer u de Office 365-oplossing aan de OMS-werkruimte toevoegt, de **Office 365** tegel wordt toegevoegd aan uw dashboard OMS. Deze tegel toont het aantal computers in uw omgeving, evenals een grafische voorstelling hiervan, en de bijbehorende updatenaleving.<br><br>
![Tegel samenvatting van Office 365](media/oms-solution-office-365/tile.png)  

Klik op de **Office 365** tegel openen de **Office 365** dashboard.

![Office 365-Dashboard](media/oms-solution-office-365/dashboard.png)  

Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat de top tien waarschuwingen op het aantal die overeenkomt met de criteria die kolom voor het opgegeven bereik en tijdsbereik. U kunt een zoekopdracht logboek waarmee de volledige lijst door te klikken op Zie alle aan de onderkant van de kolom of door te klikken op de kolomkop uitvoeren.

| Kolom | Beschrijving |
|:--|:--|
| Bewerkingen | Bevat informatie over de actieve gebruikers van uw abonnementen op alle gecontroleerde Office 365. Ook is mogelijk om te zien van het aantal activiteiten die ontstaan gedurende een bepaalde periode.
| Exchange | U kunt de uitsplitsing van Exchange Server-activiteiten zoals de machtiging voor toevoegen of Set-postvak. |
| SharePoint | Toont de belangrijkste activiteiten dat gebruikers op de SharePoint-documenten uitvoeren. Wanneer u inzoomen op deze tegel de zoekpagina geeft de details van deze activiteiten, zoals het doeldocument en de locatie van deze activiteit. Bijvoorbeeld: voor een gebeurtenis bestand geopend, kunt u zich kunt zien van het document dat wordt geopend, de bijbehorende accountnaam en het IP-adres. |
| Azure Active Directory | Bovenste gebruikersactiviteiten, zoals gebruiker-wachtwoord opnieuw instellen en aanmeldingspogingen bevat. Wanneer u inzoomen, zich u kunt u de details van deze activiteiten zoals de resultaatstatus. Dit is vooral handig als u wilt bewaken van verdachte activiteiten op uw Azure Active Directory. |




## <a name="log-analytics-records"></a>Log Analytics-records

Alle records in de werkruimte voor logboekanalyse is gemaakt door de Office 365-oplossing een **Type** van **OfficeActivity**.  De **OfficeWorkload** eigenschap bepaalt welke Office 365-service de record naar - Exchange, AzureActiveDirectory, SharePoint of OneDrive verwijst.  De **RecordType** eigenschap geeft u het type bewerking.  De eigenschappen voor elk bewerkingstype varieert en worden weergegeven in de onderstaande tabellen.

### <a name="common-properties"></a>Algemene eigenschappen
De volgende eigenschappen gelden voor alle Office 365-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type | *OfficeActivity* |
| client-IP | Het IP-adres van het apparaat dat werd gebruikt toen de activiteit is geregistreerd. Het IP-adres wordt weergegeven in de notatie voor een IPv4- of IPv6-adres. |
| OfficeWorkload | Office 365-service die de record verwijst.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Bewerking | De naam van de activiteit van de gebruiker of beheerder.  |
| OrganizationId | De GUID voor Office 365-tenant van uw organisatie. Deze waarde is altijd hetzelfde zijn voor uw organisatie, ongeacht de Office 365-service waarin dit zich voordoet. |
| recordType | Type van een bewerking uitgevoerd. |
| ResultStatus | Hiermee wordt aangegeven of de actie (opgegeven in de eigenschap Operation) gelukt is. Mogelijke waarden zijn geslaagd, PartiallySucceded of mislukt. Voor Exchange-admin-activiteit is de waarde True of False. |
| Gebruikers-id | De UPN (User Principal Name) van de gebruiker die de actie dat heeft geresulteerd in de record aan te melden; is uitgevoerd bijvoorbeeld: my_name@my_domain_name. Houd er rekening mee dat records voor de activiteit uitgevoerd door systeemaccounts (zoals SHAREPOINT\system of NTAUTHORITY\SYSTEM) ook opgenomen worden. | 
| UserKey | Een alternatieve ID voor de gebruiker in de eigenschap UserId geïdentificeerd.  Deze eigenschap wordt bijvoorbeeld gevuld met de passport unieke ID (PUID) voor gebeurtenissen die worden uitgevoerd door gebruikers in SharePoint, OneDrive voor bedrijven en Exchange. Deze eigenschap kan ook Geef dezelfde waarde als de eigenschap UserID voor gebeurtenissen in andere services en de gebeurtenissen die worden uitgevoerd door systeemaccounts|
| UserType | Het type van de gebruiker die de bewerking uitgevoerd.<br><br>Beheerder<br>Toepassing<br>DcAdmin<br>Reguliere<br>Gereserveerd<br>ServicePrincipal<br>Systeem |


### <a name="azure-active-directory-base"></a>Azure Active Directory-basis
De volgende eigenschappen gelden voor alle Azure Active Directory-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Het type van Azure AD-gebeurtenis. |
| extendedProperties | De uitgebreide eigenschappen van de Azure AD-gebeurtenis. |


### <a name="azure-active-directory-account-logon"></a>Aanmelding bij Azure Active Directory-Account
Deze records worden gemaakt wanneer u een Active Directory-gebruiker zich probeert aan te melden.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectoryAccountLogon |
| Toepassing | De toepassing die de gebeurtenis account aanmelden, zoals Office 15. |
| Client | Meer informatie over de client apparaat, een besturingssysteem van het apparaat en browser van een apparaat dat is gebruikt voor de van de account aanmelding-gebeurtenis. |
| loginStatus | Deze eigenschap is rechtstreeks uit OrgIdLogon.LoginStatus. De toewijzing van verschillende interessante aanmeldingsfouten kan worden uitgevoerd door waarschuwingen algoritmen. |
| UserDomain | De Tenant identiteitsgegevens (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Deze records worden gemaakt wanneer een wijziging of toevoegingen worden aangebracht in Azure Active Directory-objecten.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AADTarget | De gebruiker die de actie (aangeduid met de eigenschap Operation) is uitgevoerd op. |
| Actor | De gebruiker of service-principal die de actie heeft uitgevoerd. |
| ActorContextId | De GUID van de organisatie die de actor behoort. |
| ActorIpAddress | De actor IP-adres in IPV4 of IPV6-adres-indeling. |
| InterSystemsId | De GUID die de acties voor onderdelen in de Office 365-service controleren. |
| IntraSystemId |   De GUID die wordt gegenereerd door Azure Active Directory voor het bijhouden van de actie. |
| SupportTicketId | De klant ondersteunt ticket-ID voor de actie in situaties 'act-op-andere gebruikers-of'. |
| TargetContextId | De GUID van de organisatie die de betreffende gebruiker behoort. |


### <a name="data-center-security"></a>Data Center-beveiliging
Deze records worden gemaakt van controlegegevens Center gegevensbeveiliging.  

| Eigenschap | Beschrijving |
|:--- |:--- |
| EffectiveOrganization | De naam van de tenant die de uitbreiding van bevoegdheden/cmdlet is gericht op. |
| ElevationApprovedTime | Het tijdstempel voor wanneer de uitbreiding is goedgekeurd. |
| ElevationApprover | De naam van een Microsoft-manager. |
| ElevationDuration | De duur waarvoor de uitbreiding van bevoegdheden actief was. |
| ElevationRequestId |  Een unieke id voor de uitbreiding van bevoegdheden-aanvraag. |
| ElevationRole | De rol van de uitbreiding is aangevraagd voor. |
| ElevationTime | De begintijd van de uitbreiding van bevoegdheden. |
| Start_Time | De begintijd van de uitvoering van cmdlets. |


### <a name="exchange-admin"></a>Exchange-beheerder
Deze records worden gemaakt wanneer er wijzigingen zijn aangebracht in de configuratie van Exchange.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeAdmin |
| ExternalAccess |  Hiermee geeft u op of de cmdlet is uitgevoerd door een gebruiker in uw organisatie, door medewerkers van Microsoft-datacenter of in een datacenter-serviceaccount of door een gedelegeerde beheerder. De waarde False geeft aan dat de cmdlet is uitgevoerd door iemand die in uw organisatie. De waarde True geeft aan dat de cmdlet is uitgevoerd door datacenter personeel, een datacenter-serviceaccount of een gedelegeerde beheerder. |
| ModifiedObjectResolvedName |  Dit is de beschrijvende naam van het object dat door de cmdlet is gewijzigd. Dit is alleen geregistreerd als de cmdlet het object wijzigt. |
| Organisatienaam | De naam van de tenant. |
| OriginatingServer | De naam van de server van waaruit u de cmdlet is uitgevoerd. |
| Parameters | De naam en waarde voor alle parameters die zijn gebruikt met de cmdlet die wordt geïdentificeerd in de Operations-eigenschap. |


### <a name="exchange-mailbox"></a>Exchange-postvak
Deze records worden gemaakt wanneer wijzigingen of toevoegingen worden aangebracht in de Exchange-postvakken.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| ClientInfoString | Informatie over de e-mailclient die is gebruikt om de bewerking, zoals een browserversie, de Outlook-versie en de informatie van mobiele apparaten. |
| Client_IPAddress | Het IP-adres van het apparaat dat is gebruikt bij de bewerking werd geregistreerd. Het IP-adres wordt weergegeven in de notatie voor een IPv4- of IPv6-adres. |
| ClientMachineName | De naam van de machine die als host fungeert voor de Outlook-client. |
| ClientProcessName | De e-mailclient die is gebruikt voor toegang tot het postvak. |
| ClientVersion | De versie van de e-mailclient. |
| InternalLogonType | Gereserveerd voor intern gebruik. |
| Logon_Type | Geeft het type van de gebruiker die toegang tot het postvak en de bewerking die is vastgelegd uitgevoerd. |
| LogonUserDisplayName |    De beschrijvende naam van de gebruiker die de bewerking uitgevoerd. |
| LogonUserSid | De SID van de gebruiker die de bewerking uitgevoerd. |
| MailboxGuid | De Exchange-GUID van het postvak dat is geopend. |
| MailboxOwnerMasterAccountSid | Postvak eigenaarsaccount hoofdaccount SID. |
| MailboxOwnerSid | De SID van de eigenaar van het postvak. |
| MailboxOwnerUPN | Het e-mailadres van degene die eigenaar is van het postvak dat is geopend. |


### <a name="exchange-mailbox-audit"></a>Exchange-postvak Audit
Deze records worden gemaakt wanneer u een vermelding postvak wordt gemaakt.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| Item | Hiermee geeft u het item waarop de bewerking is uitgevoerd | 
| SendAsUserMailboxGuid | De Exchange-GUID van het postvak dat is geopend voor het verzenden van e-mailadres als. |
| SendAsUserSmtp | SMTP-adres van de gebruiker die wordt geïmiteerd. |
| SendonBehalfOfUserMailboxGuid | De Exchange-GUID van het postvak dat is geopend voor het verzenden van e-mail namens. |
| SendOnBehalfOfUserSmtp | SMTP-adres van de gebruiker namens wie het e-mailbericht wordt verzonden. |


### <a name="exchange-mailbox-audit-group"></a>Exchange-postvak Audit-groep
Deze records worden gemaakt wanneer wijzigingen of toevoegingen worden aangebracht in de Exchange-groepen.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Informatie over elk item in de groep. |
| CrossMailboxOperations | Hiermee wordt aangegeven als de bewerking meer dan één postvak betrokken. |
| DestMailboxId | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u het doelpostvak GUID. |
| DestMailboxOwnerMasterAccountSid | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de SID voor het hoofdaccount SID van de eigenaar van de doel-postvak. |
| DestMailboxOwnerSid | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de SID van het doelpostvak. |
| DestMailboxOwnerUPN | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de UPN van de eigenaar van het doelpostvak. |
| DestFolder | De doelmap voor bewerkingen, zoals verplaatsen. |
| Map | De map waarin een groep items zich bevindt. |
| Mappen |     Informatie over de bronmappen die zijn betrokken bij een bewerking. Als bijvoorbeeld mappen zijn geselecteerd en vervolgens verwijderd. |


### <a name="sharepoint-base"></a>SharePoint-basis
Deze eigenschappen gelden voor alle SharePoint-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Geeft aan dat een gebeurtenis is opgetreden in SharePoint. Mogelijke waarden zijn SharePoint of ObjectModel. |
| itemType | Het type object dat is geopend of gewijzigd. Zie de tabel ItemType voor meer informatie over de verschillende typen objecten. |
| MachineDomainInfo | Informatie over apparaat synchronisatiebewerkingen. Deze informatie wordt gerapporteerd als het aanwezig is in de aanvraag. |
| MachineId |   Informatie over apparaat synchronisatiebewerkingen. Deze informatie wordt gerapporteerd als het aanwezig is in de aanvraag. |
| Site_ | De GUID van de site waar het bestand of map gebruikt door de gebruiker zich bevindt. |
| Source_Name | De entiteit die de controlebewerking geactiveerd. Mogelijke waarden zijn SharePoint of ObjectModel. |
| UserAgent | Informatie over de client of de browser van de gebruiker. Deze informatie wordt verstrekt door de client of de browser. |


### <a name="sharepoint-schema"></a>SharePoint-Schema
Deze records worden gemaakt wanneer configuratiewijzigingen worden aangebracht in SharePoint.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Optionele tekenreeks voor aangepaste gebeurtenissen. |
| Event_Data |  De nettolading voor aangepaste gebeurtenissen is optioneel. |
| ModifiedProperties | De eigenschap is opgenomen voor admin-gebeurtenissen, zoals een gebruiker toe te voegen als lid van een site of een groep siteverzamelingen-beheerder. De eigenschap bevat de naam van de eigenschap die is aangepast (bijvoorbeeld de groep sitebeheerder), de nieuwe waarde van de aangepaste eigenschap (die de gebruiker die als een beheerder van de site is toegevoegd) en de vorige waarde van het gewijzigde object. |


### <a name="sharepoint-file-operations"></a>SharePoint-bestandsbewerkingen
Deze records worden gemaakt in reactie op bestandsbewerkingen in SharePoint.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | De bestandsextensie van een bestand dat wordt verplaatst of gekopieerd. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| DestinationFileName | De naam van het bestand wordt gekopieerd of verplaatst. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| DestinationRelativeUrl | De URL van de doelmap waar een bestand wordt gekopieerd of verplaatst. De combinatie van de waarden voor parameters SiteURL, DestinationRelativeURL en DestinationFileName is hetzelfde als de waarde voor de object-id-eigenschap, die de volledige padnaam voor het bestand dat is gekopieerd. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| SharingType | Het type van de machtigingen die zijn toegewezen aan de gebruiker die de resource is gedeeld met delen. Deze gebruiker wordt geïdentificeerd door de parameter UserSharedWith. |
| Site_Url | De URL van de site waar het bestand of map gebruikt door de gebruiker zich bevindt. |
| SourceFileExtension | De bestandsextensie van het bestand dat door de gebruiker is geopend. Deze eigenschap is leeg als het object dat is geopend, een map is. |
| Bronbestandsnaam |  De naam van het bestand of map gebruikt door de gebruiker. |
| SourceRelativeUrl | De URL van de map waarin het bestand gebruikt door de gebruiker. De combinatie van de waarden voor de parameters SiteURL SourceRelativeURL en bronbestandsnaam is hetzelfde als de waarde voor de object-id-eigenschap, die de volledige padnaam voor het bestand gebruikt door de gebruiker. |
| UserSharedWith |  De gebruiker die met een bron is gedeeld. |




## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken
De volgende tabel biedt voorbeeldzoekopdrachten in logboeken voor updaterecords die worden verzameld door deze oplossing.

| Query’s uitvoeren | Beschrijving |
| --- | --- |
|Telling van alle bewerkingen voor uw Office 365-abonnement |Type = OfficeActivity &#124; meting count() per bewerking |
|Informatie over het gebruik van SharePoint-sites|Type = OfficeActivity OfficeWorkload = sharepoint &#124; meten count() als tellen per SiteUrl &#124; Aantal asc sorteren|
|Toegang bestandsbewerkingen door gebruikerstype|Type = OfficeActivity OfficeWorkload = sharepoint bewerking = FileAccessed &#124; meting count() door UserType|
|Zoeken met specifieke trefwoorden|Type = OfficeActivity OfficeWorkload = azureactivedirectory 'MyTest'|
|Externe acties van de monitor op Exchange|Type = OfficeActivity OfficeWorkload = exchange ExternalAccess = true|



## <a name="troubleshooting"></a>Problemen oplossen

Als uw Office 365-oplossing is geen gegevens worden verzameld zoals verwacht, controleert de status ervan in de OMS-portal op **instellingen** -> **verbonden bronnen** -> **Office 365**. De volgende tabel beschrijft elke status.

| Status | Beschrijving |
|:--|:--|
| Actief | Het Office 365-abonnement actief is en de werkbelasting is verbonden met de OMS-werkruimte. |
| In behandeling | Het Office 365-abonnement actief is, maar de werkbelasting is nog niet verbonden met de OMS-werkruimte is. De eerste keer dat u verbinding maken met de Office 365-abonnement zijn alle werkbelastingen op deze status totdat ze zijn verbonden. Wacht 24 uur voor alle werkbelastingen overschakelen naar de actieve. |
| Inactieve | Het Office 365-abonnement is in een inactieve status heeft. Controleer op de pagina van uw Office 365-beheerder voor meer informatie. Nadat u uw Office 365-abonnement hebt geactiveerd, wordt het ontkoppelen van OMS-werkruimte en koppel deze opnieuw uit om te beginnen met het ontvangen van gegevens. |



## <a name="next-steps"></a>Volgende stappen
* Gebruik Logboekzoekopdrachten in [Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Maak uw eigen dashboards](../log-analytics/log-analytics-dashboards.md) om uw favoriete Office 365 zoekquery's weer te geven.
* [Waarschuwingen maken](../log-analytics/log-analytics-alerts.md) proactief informeren van belangrijke Office 365-activiteiten.  
