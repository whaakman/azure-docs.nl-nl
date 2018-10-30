---
title: Connectoren voor Azure Logic Apps | Microsoft Docs
description: Automatiseer werkstromen met connectors voor Azure Logic Apps, met inbegrip van ingebouwde, beheerde on-premises integratie-account en enterprise-connectors
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 3230692208d27ebac073e257266c885b5e03095c
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233630"
---
# <a name="connectors-for-azure-logic-apps"></a>Connectors voor Azure Logic Apps

Connectors spelen een integraal onderdeel is wanneer u geautomatiseerde werkstromen met Azure Logic Apps maken. Met behulp van connectors in uw logische apps, vouw de mogelijkheden voor uw on-premises en cloud-apps uitvoeren met de gegevens die u maakt en al hebt. 

Logic Apps biedt terwijl [~ meer dan 200 connectoren](https://docs.microsoft.com/connectors), populaire en veelgebruikte connectors die met succes worden gebruikt door duizenden apps en miljoenen uitvoeringen voor het verwerken van gegevens en informatie door dit artikel wordt beschreven. Connectors zijn beschikbaar als dient te worden of beheerde connectors. 

> [!NOTE]
> Voor een volledige lijst van connectors en van elke connector referentie-informatie, zoals acties, geen triggers en -limieten, vindt u de volledige lijst onder de [overzicht van Connectors](https://docs.microsoft.com/connectors).

* [**Dient te worden**](#built-ins): deze ingebouwde acties en triggers kunt u bij het maken van logische apps die worden uitgevoerd op aangepaste schema's, communiceren met andere eindpunten ontvangen en het reageren op aanvragen en het aanroepen van Azure functions, Azure API Apps (Web-Apps), uw eigen API's worden beheerd en die zijn gepubliceerd met Azure API Management en geneste logische apps die aanvragen kunnen ontvangen. U kunt ook ingebouwde acties waarmee u ordenen en beheren van uw logische app-werkstroom en ook werken met gegevens.

* **Beheerde connectoren**: deze connectors bieden triggers en acties voor toegang tot andere services en systemen. Sommige connectors moeten eerst de verbindingen die worden beheerd door Azure Logic Apps te maken. Beheerde connectors zijn ingedeeld in deze groepen:

  |   |   |
  |---|---|
  | [**Beheerde API-connectors**](#managed-api-connectors) | Het maken van logische apps die gebruikmaken van services zoals Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online, en nog veel meer. | 
  | [**On-premises connectors**](#on-premises-connectors) | Na het installeren en instellen van de [on-premises gegevensgateway][gateway-doc], deze connectors helpt om uw logische apps toegang tot on-premises systemen, zoals SQL Server, SharePoint Server, Oracle DB, bestandsshares en anderen. | 
  | [**Integratieaccountconnectors**](#integration-account-connectors) | Beschikbaar wanneer u maken en voor een integratie-account, deze transformatie connectors betaalt en XML-validatie, coderen en decoderen van platte bestanden en verwerken van business-to-business (B2B) berichten met AS2, EDIFACT en X12 protocollen. | 
  | [**Enterprise-connectors**](#enterprise-connectors) | Bieden toegang tot bedrijfssystemen zoals SAP en IBM MQ tegen extra kosten. |
  ||| 

  Bijvoorbeeld, als u van Microsoft BizTalk Server gebruikmaakt, uw logische apps kunnen verbinding maken met en communiceren met uw BizTalk-Server met behulp van de [BizTalk Server-connector](#on-premises-connectors). 
  U kunt vervolgens uitbreiden of BizTalk-achtige bewerkingen uitvoeren in uw logische apps met behulp van de [integratieaccountconnectors](#integration-account-connectors). 

> [!NOTE] 
> Voor een volledige lijst van connectors en van elke connector referentie-informatie, zoals acties en geen triggers, die door een beschrijving van de Swagger zijn gedefinieerd, plus eventuele beperkingen, vindt u de volledige lijst onder de [overzicht van Connectors](/connectors/). Zie voor informatie over de prijzen [Logic Apps prijsinformatie](https://azure.microsoft.com/pricing/details/logic-apps/) en de [Logic Apps prijsmodel](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Ingebouwde apps

Logic Apps biedt ingebouwde triggers en acties, zodat u werkstromen op basis van een planning maken voor help uw logische apps communiceren met andere apps en services, beheer de werkstroom door middel van uw logische apps, en te beheren of te bewerken van gegevens. 

|   |   |   |   | 
|---|---|---|---| 
| [![API-pictogram][schedule-icon]<br/>**plannen**][recurrence-doc] | -Uw logische app uitvoeren op een opgegeven schema, variërend van eenvoudige tot complexe herhalingen met de **terugkeerpatroon** trigger. <p>-Uw logische app onderbreken gedurende een opgegeven periode met de **vertraging** actie. <p>-Uw logische app onderbreken tot de opgegeven datum en tijd met de **vertragen tot** actie. | [![API-pictogram][http-icon]<br/>**HTTP**][http-doc] | Communiceren met een willekeurig eindpunt via HTTP met triggers en acties voor HTTP, HTTP + Swagger, en HTTP-Webhook. | 
| [![API-pictogram][http-request-icon]<br/>**aanvragen**][http-request-doc] | -Controleer uw logische app aanroepbare van andere apps of services, trigger van Event Grid resource gebeurtenissen of trigger op reacties op Azure Security Center-waarschuwingen met de **aanvragen** trigger. <p>-Antwoorden verzenden op een app of service met de **antwoord** actie. | [![API-pictogram][batch-icon]<br/>**Batch**][batch-doc] | -Verwerking van berichten in batches met de **berichten batchgewijs** trigger. <p>-Aanroep van logische apps die bestaande triggers met batch de **verzenden van berichten naar batches** actie. | 
| [![API-pictogram][azure-functions-icon]<br/>**Azure Functions**][azure-functions-doc] | Azure functions met aangepaste codefragmenten (C# of Node.js) van uw logische apps aanroepen. | [![API-pictogram][azure-api-management-icon]</br>**Azure API Management**][azure-api-management-doc] | Aanroepen van triggers en acties die zijn gedefinieerd door uw eigen API's die u beheren en publiceren met Azure API Management. | 
| [![API-pictogram][azure-app-services-icon]<br/>**Azure App Services**][azure-app-services-doc] | Azure API Apps of Web-Apps, die worden gehost op Azure App Service aanroepen. De triggers en acties die zijn gedefinieerd door deze apps worden weergegeven, zoals alle andere eersteklas triggers en acties wanneer Swagger opgenomen is. | [![API-pictogram][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | Andere logische apps die met een aanvraagtrigger beginnen aanroepen. | 
||||| 

### <a name="control-workflow"></a>Controle-werkstroom

Hier vindt u ingebouwde acties voor het structureren en beheren van de acties in uw logic app-werkstroom:

|   |   |   |   | 
|---|---|---|---| 
| [![Ingebouwde pictogram][condition-icon]<br/>**voorwaarde**][condition-doc] | Beoordeel een voorwaarde en voer verschillende acties op basis van of u de voorwaarde is true of false. | [![Ingebouwde pictogram][for-each-icon]</br>**voor elk**][for-each-doc] | De dezelfde acties uitvoeren op elk item in een matrix. | 
| [![Ingebouwde pictogram][scope-icon]<br/>**bereik**][scope-doc] | Acties in groep *scopes*, die hun eigen status ophalen nadat de acties in het bereik wordt uitgevoerd. | [![Ingebouwde pictogram][switch-icon]</br>**Switch**][switch-doc] | Acties in groep *gevallen*, unieke waarden, met uitzondering van de standaard-aanvraag worden toegewezen. Voer alleen die gevallen waarvan de toegewezen waarde komt overeen met het resultaat van een expressie, een object of een token. Als er geen overeenkomsten bestaat, de standaard-aanvraag worden uitgevoerd. | 
| [![Ingebouwde pictogram][terminate-icon]<br/>**beëindigen**][terminate-doc] | Stoppen van een werkstroom voor logische Apps actief wordt uitgevoerd. | [![Ingebouwde pictogram][until-icon]<br/>**totdat**][until-doc] | Acties herhalen totdat de opgegeven voorwaarde waar is of sommige status is gewijzigd. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Beheren of bewerken van gegevens

Hier vindt u ingebouwde acties voor het werken met gegevens uitvoer en hun indelingen:  

|   |   | 
|---|---| 
| ![Ingebouwde pictogram][data-operations-icon]<br/>**Gegevensbewerkingen** | Bewerkingen uitvoeren met gegevens: <p>- **Opstellen**: een enkele van de uitvoer van meerdere invoergegevens met verschillende typen maken. <br>- **CSV-tabel maken**: een tabel met door komma's gescheiden waarden (CSV) van een matrix met JSON-objecten maken. <br>- **HTML-tabel maken**: een HTML-tabel maken van een matrix met JSON-objecten. <br>- **Matrix filteren**: maken van een matrix van items in een andere matrix die voldoen aan uw criteria. <br>- **Deelnemen aan**: maken van een tekenreeks van alle items in een matrix en scheid de items met het opgegeven scheidingsteken. <br>- **JSON parseren**: gebruiksvriendelijke-tokens aanmaken van de eigenschappen en hun waarden in de JSON-inhoud zodat u deze eigenschappen in uw werkstroom gebruiken kunt. <br>- **Selecteer**: maken van een matrix met JSON-objecten door items of waarden in een andere matrix te transformeren en het toewijzen van deze items naar de opgegeven eigenschappen. | 
| ![Ingebouwde pictogram][date-time-icon]<br/>**Datum en tijd** | Met tijdstempels bewerkingen wilt uitvoeren: <p>- **Optellen bij tijd**: het opgegeven aantal eenheden toevoegen aan een tijdstempel. <br>- **Tijdzone converteren**: een tijdstempel van de brontijdzone converteren naar de doeltijdzone. <br>- **Huidige tijd**: de huidige tijdstempel als tekenreeks geretourneerd. <br>- **Toekomstige tijd ophalen**: retourneert de huidige tijdstempel plus de opgegeven periode-eenheden. <br>- **In het verleden tijd ophalen**: retourneert de huidige tijdstempel minus de opgegeven periode-eenheden. <br>- **Aftrekken van tijd**: moet worden afgetrokken van een aantal eenheden van een tijdstempel. |
| [![Ingebouwde pictogram][variables-icon]<br/>**variabelen**][variables-doc] | Bewerkingen uitvoeren met variabelen: <p>- **Toevoegen aan een matrixvariabele**: plaats een waarde als het laatste item in een matrix die zijn opgeslagen door een variabele. <br>- **Toevoegen aan een tekenreeksvariabele**: plaats een waarde als het laatste teken in een tekenreeks door een variabele opgeslagen. <br>- **Variabele verlagen**: verkleinen van een variabele met een constante waarde. <br>- **Variabele verhogen**: een variabele verhoogd met een constante waarde. <br>- **Variabele initialiseren**: Maak een variabele en declareert de gegevens van het type en de initiële waarde. <br>- **Variabele instellen**: een andere waarde toewijzen aan een bestaande variabele. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Beheerde API-connectors

Hier volgen de meer populaire connectors voor het automatiseren van taken, processen en werkstromen met deze services of systemen:

|   |   |   |   | 
|---|---|---|---| 
| [![API-pictogram][azure-service-bus-icon]<br/>**Azure Service Bus**][azure-service-bus-doc] | Beheren van asynchrone berichten, sessies en onderwerpabonnementen met de meest gebruikte connector in Logic Apps. | [![API-pictogram][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Verbinding maken met uw SQL Server on-premises of een Azure SQL Database in de cloud zodat u kunt records beheren, opgeslagen procedures uitvoeren of uitvoeren van query's. | 
| [![API-pictogram][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Verbinding maken met uw Office 365-e-mailaccount, zodat u kunt maken en beheren van e-mailberichten, taken, agenda-gebeurtenissen en vergaderingen, contactpersonen, aanvragen en meer. | [![API-pictogram][azure-blob-storage-icon]<br/>**Azure Blob<br/>opslag**][azure-blob-storage-doc] | Verbinding maken met uw storage-account zodat u kunt maken en beheren van blobinhoud. | 
| [![API-pictogram][sftp-icon]<br/>**SFTP**][sftp-doc] | Verbinding maken met SFTP-servers die u openen vanaf het internet, kunt zodat u met uw bestanden en mappen werken kunt. | [![API-pictogram][sharepoint-online-icon]<br/>**SharePoint<br/>Online**][sharepoint-online-doc] | Verbinding maken met SharePoint Online, zodat u bestanden, bijlagen en mappen kunt beheren. | 
| [![API-pictogram][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Verbinding maken met uw Dynamics 365-account zodat u kunt maken en beheren van records, artikelen en meer. | [![API-pictogram][ftp-icon]<br/>**FTP**][ftp-doc] | Verbinding maken met de FTP-servers die u openen vanaf het internet, kunt zodat u met uw bestanden en mappen werken kunt. | 
| [![API-pictogram][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Verbinding maken met uw Salesforce-account zodat u kunt maken en beheren van items zoals records, taken en -objecten. | [![API-pictogram][twitter-icon]<br/>**Twitter**][twitter-doc] | Verbinding maken met uw Twitter-account, zodat u tweets, hobbyisten en uw tijdlijn kunt beheren. Uw tweets opslaan in SQL-, Excel of SharePoint. | 
| [![API-pictogram][azure-event-hubs-icon]<br/>**Azure Event Hubs**][azure-event-hubs-doc] | Gebruiken en publiceren van gebeurtenissen tot en met een Event Hub. Bijvoorbeeld uitvoer van uw logische app met Event Hubs ophalen en vervolgens verzenden die worden uitgevoerd naar een realtime analytics-provider. | [![API-pictogram][azure-event-grid-icon]<br/>**Azure Event**</br>**raster**][azure-event-grid-doc] | Controleren op gebeurtenissen die zijn gepubliceerd door een Gebeurtenisraster, bijvoorbeeld wanneer het Azure-resources of resources van derden worden gewijzigd. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>On-premises connectors 

Hier volgen enkele veelgebruikte connectors die toegang tot gegevens en resources in on-premises systemen bieden. Voordat u een verbinding met een on-premises systeem maken kunt, moet u eerst [downloaden, installeren en instellen van een on-premises gegevensgateway][gateway-doc]. Deze gateway biedt een beveiligd communicatiekanaal zonder het instellen van de netwerkinfrastructuur die nodig zijn. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![API-pictogram][biztalk-server-icon]<br/>**BizTalk**</br> **Server** | [![API-pictogram][file-system-icon]<br/>**bestand</br> System**][file-system-doc] | [![API-pictogram][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![API-pictogram][ibm-informix-icon]<br/>**IBM** </br> **Informix**][ibm-informix-doc] | ![API-pictogram][mysql-icon]<br/>**MySQL** | 
| [![API-pictogram][oracle-db-icon]<br/>**Oracle DB**][oracle-db-doc] | ![API-pictogram][postgre-sql-icon]<br/>**PostgreSQL** | [![API-pictogram][sharepoint-server-icon]<br/>**SharePoint</br> Server**][sharepoint-server-doc] | [![API-pictogram][sql-server-icon]<br/>**SQL</br> Server**][sql-server-doc] | ![API-pictogram][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Integratieaccountconnectoren 

Hier vindt u connectors voor het bouwen van oplossingen met uw logische apps business-to-business (B2B) wanneer u maakt en u betaalt voor een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), deze is beschikbaar via de Enterprise Integration Pack (EIP) in Azure. Met dit account, kunt u maken en opslaan van B2B-artefacten, zoals trading partners, overeenkomsten, kaarten, schema's, certificaten, enzovoort. Voor het gebruik van deze artefacten, koppelt u uw logische apps met uw integratie-account. Als u momenteel gebruikmaakt van BizTalk Server, deze connectors lijkt het alsof bekend al.

|   |   |   |   | 
|---|---|---|---| 
| [![API-pictogram][as2-icon]<br/>**AS2</br> decodering**][as2-decode-doc] | [![API-pictogram][as2-icon]<br/>**AS2</br> codering**][as2-encode-doc] | [![API-pictogram][edifact-icon]<br/>**EDIFACT</br> decodering**][edifact-decode-doc] | [![API-pictogram][edifact-icon]<br/>**EDIFACT</br> codering**][edifact-encode-doc] | 
| [![API-pictogram][flat-file-decode-icon]<br/>**plat bestand</br> decodering**][flat-file-decode-doc] | [![API-pictogram][flat-file-encode-icon]<br/>**plat bestand</br> codering**][flat-file-encode-doc] | [![API-pictogram][integration-account-icon]<br/>**integratie<br/>account**][integration-account-doc] | [![API-pictogram][liquid-icon]<br/>**Liquid**</br>**transformeert**][json-liquid-transform-doc] | 
| [![API-pictogram][x12-icon]<br/>**X12</br> decodering**][x12-decode-doc] | [![API-pictogram][x12-icon]<br/>**X12</br> codering**][x12-encode-doc] | [![API-pictogram][xml-transform-icon]<br/>**XML**</br>**transformeert**][xml-transform-doc] | [![API-pictogram][xml-validate-icon]<br/>**XML <br/>validatie**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Bedrijfsconnectoren

Uw logische apps hebben toegang tot bedrijfssystemen, zoals SAP en IBM MQ:

|   |   | 
|---|---| 
| [![API-pictogram][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![API-pictogram][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

## <a name="more-about-triggers-and-actions"></a>Meer informatie over triggers en acties

Sommige connectors bieden *triggers* die uw logische app waarschuwen wanneer er specifieke gebeurtenissen plaatsvinden. Dus wanneer deze gebeurtenissen optreden, de trigger wordt gemaakt en een exemplaar van uw logische app wordt uitgevoerd. De FTP-connector biedt bijvoorbeeld een 'wanneer een bestand wordt toegevoegd of gewijzigd' trigger die uw logische app wordt gestart wanneer een bestand wordt bijgewerkt. 

Logic Apps biedt deze soorten triggers:  

* *Poll-triggers*: deze triggers wordt uw service met een opgegeven frequentie en controles voor nieuwe gegevens gepeild. 

  Wanneer er nieuwe gegevens beschikbaar is, wordt een nieuw exemplaar van uw logische app wordt gemaakt en wordt uitgevoerd met de gegevens die wordt doorgegeven als invoer. 

* *Push-triggers*: deze triggers wordt geluisterd voor nieuwe gegevens op een eindpunt of een gebeurtenis plaatsvindt, die wordt gemaakt en het nieuwe exemplaar van uw logische app wordt uitgevoerd.

* *Trigger met terugkeerpatroon*: deze trigger wordt gemaakt en een exemplaar van uw logische app op basis van een opgegeven planning wordt uitgevoerd.

Connectors bieden ook *acties* die taken uitvoeren in uw logic app-werkstroom. Uw logische app kan bijvoorbeeld gegevens lezen en deze gegevens gebruiken in latere stappen van uw logische app. Uw logische app ook meer specifiek, te zoeken naar klantgegevens van een SQL-database en verwerken van deze gegevens later in uw logic app-werkstroom. 

Zie voor meer informatie over triggers en acties, de [overzicht van Connectors](connectors-overview.md). 

## <a name="custom-apis-and-connectors"></a>Aangepaste API's en connectors 

Voor het aanroepen van API's waarmee aangepaste code wordt uitgevoerd of die niet beschikbaar zijn als connectors, kunt u de Logic Apps-platform door uitbreiden [het maken van aangepaste API Apps](../logic-apps/logic-apps-create-api-app.md). U kunt ook [maken van aangepaste connectors](../logic-apps/custom-connector-overview.md) voor *eventuele* REST of SOAP-API's die deze API's beschikbaar zijn voor een logische app in uw Azure-abonnement maken.
Als u aangepaste API Apps of connectors openbare voor iedereen die het gebruik in Azure, kunt u [connectors indienen voor certificering door Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Als u wilt indienen of hierop stemmen op ideeën voor Azure Logic Apps en connectors, gaat u naar de [site voor gebruikersfeedback van Logic Apps](https://aka.ms/logicapps-wish).

* Zijn de ontbrekende artikelen of de details die u denkt dat documenten zijn belangrijk? Zo ja, kunt u helpen door toe te voegen aan de bestaande artikelen of door uw eigen te schrijven. De documentatie is open-source en gehost op GitHub. Aan de slag van de Azure-documentatie [GitHub-opslagplaats](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Volgende stappen

* Zoek de [volledige lijst van connectors](https://docs.microsoft.com/connectors)
* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Aangepaste connectors voor logische apps maken](https://docs.microsoft.com/connectors/custom-connectors/)
* [Aangepaste API's maken voor logische apps](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Verbinding maken met on-premises gegevensbronnen vanuit logische apps met on-premises gegevensgateway"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Logische apps integreren met Azure Functions"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Azure API Management service-exemplaar voor het beheren en publiceren van uw API's maken"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Logische apps integreren met App Service API Apps."
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Berichten vanuit de Service Bus-wachtrijen en -onderwerpen verzenden en berichten vanuit de Service Bus-wachtrijen en abonnementen ontvangen"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Verwerken van berichten in groepen, of als batches"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Beoordeel een voorwaarde en voer verschillende acties op basis van of u de voorwaarde is true of false"
[delay-doc]: ./connectors-native-delay.md "Vertraagde acties uitvoeren"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "De dezelfde acties uitvoeren op elk item in een matrix"
[http-doc]: ./connectors-native-http.md "HTTP-connector gebruiken voor HTTP-aanroepen"
[http-request-doc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[http-response-doc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[http-swagger-doc]: ./connectors-native-http-swagger.md "HTTP + Swagger-connector gebruiken voor HTTP-aanroepen"
[http-webook-doc]: ./connectors-native-webhook.md "HTTP-webhookacties en triggers toevoegen aan uw logische apps"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Logische apps integreren met geneste werkstromen"
[query-doc]: ./connectors-native-query.md "Matrices selecteren en filteren met behulp van de queryactie."
[recurrence-doc]:  ./connectors-native-recurrence.md "Terugkerende acties voor logische apps activeren"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Indelen van acties in groepen die hun eigen status ophalen nadat de acties in de groep die wordt uitgevoerd" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organiseer acties in gevallen waarin de unieke waarden zijn toegewezen. Alleen het geval is uitgevoerd waarvan de waarde komt overeen met het resultaat van een expressie, een object of een token. Als er geen overeenkomsten bestaat, de standaard-aanvraag uitvoeren"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Stoppen of annuleren van een actief actieve werkstroom voor uw logische app"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Acties herhalen totdat de opgegeven voorwaarde waar is of sommige status is gewijzigd"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Met variabelen, zoals het initialiseren, set, verhogen, verlagen, bewerkingen wilt uitvoeren en toevoegen aan de tekenreeks of matrix van variabele"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Bestanden in uw blobcontainer beheren met Azure Blob Storage-container."
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md " Controleren van gebeurtenissen die zijn gepubliceerd door een Gebeurtenisraster, bijvoorbeeld wanneer het Azure-resources of externe resources wijzigen"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Verbinding maken met Azure Event Hubs. Gebeurtenissen tussen logische apps en Event Hubs verzenden en ontvangen"
[box-doc]: ./connectors-create-api-box.md "Verbinding maken met Box. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Verbinding maken met Dropbox. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Verbinding maken met Dynamics CRM Online zodat u kunt werken met CRM Online-gegevens."
[facebook-doc]: ./connectors-create-api-facebook.md "Verbinding maken met Facebook. Iets op een tijdlijn posten, een paginafeed ophalen, en meer"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Verbinding maken met een on-premises bestandssysteem"
[ftp-doc]: ./connectors-create-api-ftp.md "Verbinding maken met een FTP-/FTPS-server voor FTP-taken, zoals het uploaden, ophalen en verwijderen van bestanden, en meer."
[github-doc]: ./connectors-create-api-github.md "Verbinding maken met GitHub en problemen bijhouden"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Maakt verbinding met Google Calendar en kan een agenda beheren."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Verbinding maken met GoogleDrive, zodat u met uw gegevens kunt werken"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Maak verbinding met Google spreadsheets zodat u uw werkbladen kunt wijzigen"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Maakt verbinding met Google Taken, zodat u uw taken kunt beheren"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Verbinding maken met IBM DB2 in de cloud of on-premises. Een rij bijwerken, een tabel ophalen, en meer"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Verbinding maken met Informix in de cloud of on-premises. Een rij lezen, de tabellen vermelden, en meer"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Verbinding maken met IBM MQ on-premises of in Azure te verzenden en ontvangen van berichten"
[instagram-doc]: ./connectors-create-api-instagram.md "Verbinding maken met Instagram. Gebeurtenissen activeren of erop reageren"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Verbinding maken met uw MailChimp-account. E-mails beheren en automatiseren"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Verbinding maken met Mandrill voor communicatie"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Verbinding maken met Microsoft Translator. Tekst vertalen, talen detecteren, en meer " 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Uw Office 365-account koppelen. E-mailberichten verzenden en ontvangen, uw agenda en contactpersonen beheren, en meer"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Informatie over video's, videolijsten, videokanalen en URL's voor het afspelen van Office 365-video's ophalen"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Verbinding maken met uw persoonlijke Microsoft OneDrive. Bestanden uploaden, verwijderen, vermelden, en meer"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Verbinding maken met uw zakelijke Microsoft OneDrive. Uw bestanden uploaden, verwijderen, vermelden, en meer"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Verbinding maken met een Oracle-database om rijen toe te voegen, in te voegen, te verwijderen en meer"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Verbinding maken met uw Outlook-postvak. Uw e-mail, agenda's en contactpersonen beheren, en meer"
[project-online-doc]: ./connectors-create-api-projectonline.md "Verbinding maken met Microsoft Project Online. Uw projecten, taken, resources beheren, en meer"
[rss-doc]: ./connectors-create-api-rss.md "Feeditems publiceren en ophalen, bewerkingen activeren wanneer een nieuw item wordt gepubliceerd in een RSS-feed."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Verbinding maken met uw Salesforce-account. Accounts, leads en verkoopkansen beheren, en meer"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Verbinding maken met een on-premises SAP-systeem"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Verbinding maken met SendGrid. E-mail verzenden en ontvangerslijsten beheren"
[sftp-doc]: ./connectors-create-api-sftp.md "Verbinding maken met uw SFTP-account. Bestanden uploaden, ophalen, verwijderen, en meer"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Verbinding maken met on-premises SharePoint-server. Documenten beheren, items vermelden, en meer"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Verbinding maken met SharePoint Online. Documenten beheren, items vermelden, en meer"
[slack-doc]: ./connectors-create-api-slack.md "Verbinding maken met Slack en berichten posten op Slack-kanalen"
[smtp-doc]: ./connectors-create-api-smtp.md "Verbinding maken met een SMTP-server en e-mailbericht met bijlagen verzenden"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Maakt verbinding met SparkPost voor communicatie"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Verbinding maken met Azure SQL Database of SQL Server. Vermeldingen in een SQL-databasetabel maken, bijwerken, ophalen en verwijderen."
[trello-doc]: ./connectors-create-api-trello.md "Verbinding maken met Trello. Uw projecten beheren en dingen organiseren met anderen"
[twilio-doc]: ./connectors-create-api-twilio.md "Verbinding maken met Twilio. Berichten verzenden en ophalen, beschikbare nummers ophalen, binnenkomende telefoonnummers beheren, en meer."
[twitter-doc]: ./connectors-create-api-twitter.md "Verbinding maken met Twitter. Tijdlijnen ophalen, tweets posten, en meer"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Verbinding maken met Wunderlist. Uw taken en takenlijsten beheren, alles synchroniseren, en meer"
[yammer-doc]: ./connectors-create-api-yammer.md "Verbinding maken met Yammer. Berichten posten, nieuwe berichten ophalen, en meer"
[youtube-doc]: ./connectors-create-api-youtube.md "Verbinding maken met YouTube. Uw video's en kanalen beheren"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Meer informatie over Enterprise Integration AS2."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Meer informatie over AS2-decodering in Enterprise Integration"
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Meer informatie over AS2-codering in Enterprise Integration"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Meer informatie over EDIFACT-decodering in Enterprise Integration"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Meer informatie over EDIFACT-codering in Enterprise Integration"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Schema's, kaarten, partners en meer opzoeken in uw integratieaccount"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Meer informatie over JSON-transformaties met Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Meer informatie over Enterprise Integration X12"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Meer informatie over X12-decodering in Enterprise Integration"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Meer informatie over X12-codering in Enterprise Integration"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Meer informatie over Enterprise Integration-transformaties."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Meer informatie over Enterprise Integration met XML-validatie."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png