---
title: Connectoren voor Azure Logic Apps | Microsoft Docs
description: Kies uit alle beschikbare door Microsoft beheerde connectors voor het maken van logische apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: c14ac7592efabfec8668d7437463e2d8771ee072
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connectors-list"></a>Lijst van connectors
> [!TIP]
> De [volledige alfabetische lijst](#az) in dit onderwerp bevat alle beschikbare connectors die u in uw Logic Apps kunt gebruiken. [Connectorgegevens](/connectors/) geeft alle triggers en acties weer die in de swagger zijn gedefinieerd, plus eventuele beperkingen voor elke connector.

Connectors vormen een integraal onderdeel van het maken van logische apps. Met behulp van deze connectors kunt u uw on-premises en cloudtoepassingen echt uitbreiden om verschillende acties uit te voeren voor gegevens die u maakt en gegevens die u al hebt. De connectors zijn beschikbaar in de volgende categorieën: 

* **Standaardconnectors**: automatisch beschikbaar en inbegrepen wanneer u logische apps gebruikt. Een aantal voorbeelden zijn Service Bus, Power BI, Oracle Database en OneDrive.

* **Integratieaccountconnectors**: beschikbaar wanneer u een integratieaccount aanschaft. Met behulp van deze connectors kunt u XML transformeren en valideren, business-to-business-berichten verwerken met AS2/X12/EDIFACT en platte bestanden coderen en decoderen. Als u met BizTalk Server werkt, zijn deze connectors zeer geschikt om uw BizTalk-werkstromen naar Azure uit te breiden.  

    BizTalk Server heeft ook een [Logic Apps-adapter](https://msdn.microsoft.com/library/mt787163.aspx) die ontvangt van en verzendt naar een logische app.

* **Bedrijfsconnectors**: bevat MQ en SAP. Beschikbaar tegen een meerprijs. 

In [Logic Apps-prijzen](https://azure.microsoft.com/pricing/details/logic-apps/) en [Prijsmodel](../logic-apps/logic-apps-pricing.md) vindt u meer informatie over de kosten. 

## <a name="popular-connectors"></a>Populaire connectors
Er zijn duizenden toepassingen en miljoenen uitvoeringen die succesvol data en informatie verwerken met behulp van deze connectors. De volgende tabel bevat de meest populaire connectors en een aantal favorieten van onze gebruikers:

| |  |  |  |
| --- | --- | --- | --- |
| [![API-pictogram][AzureBlobStorageicon]<br/>**Azure Blob<br/>Opslag**][AzureBlobStoragedoc] | U kunt deze connector gebruiken als u taken wilt automatiseren met uw opslagaccount. Ondersteunt CRUD-bewerkingen (maken, lezen, bijwerken en verwijderen). | [![API-pictogram][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Hiermee kunt u functies maken die aangepaste fragmenten van C# of node.js uitvoeren en deze functies vervolgens gebruiken in uw logische apps.  |
| [![API-pictogram][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | Een van de populairste connectors. Deze bevat triggers en acties voor het automatiseren van werkstromen met leads en meer. | [![API-pictogram][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Hiermee kunt u gebeurtenissen gebruiken en publiceren op een Event Hub. U kunt bijvoorbeeld uitvoer van uw logische app ophalen met Event Hubs en de uitvoer vervolgens verzenden naar een realtime analytics-provider. |
| [![API-pictogram][FTPicon]<br/>**FTP**][FTPdoc] | Als uw FTP-server via internet toegankelijk is, kunt u werkstromen automatiseren om met bestanden en mappen te werken. <br/><br/>SFTP is ook beschikbaar met de SFTP-connector. | [![API-pictogram][HTTPicon]<br/>**HTTP**][httpdoc] | Hiermee kunt u logische apps gebruiken om via HTTP te communiceren met een willekeurig eindpunt. |
| [![API-pictogram][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Hiermee krijgt u veel triggers en nog veel meer acties om Office 365-e-mail en -gebeurtenissen binnen uw werkstromen te gebruiken. <br/><br/>Deze connector bevat een actie voor een *goedkeurings-e-mails* om vakantieaanvragen, onkostennota's enzovoort goed te keuren. <br/><br/>Office 365-gebruikers zijn ook beschikbaar met de connector Office 365-gebruikers.| [![API-pictogram][HTTP-Requesticon]<br/>**Aanvraag/antwoord**][HTTP-Requestdoc] | Deze connector biedt een HTTPS-URL. Als de logische app een aanvraag voor deze URL ontvangt, wordt de logische app gestart. |
| [![API-pictogram][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Hiermee kunt u zich eenvoudig aanmelden met uw Salesforce-account om toegang te krijgen tot objecten (zoals leads) en meer. |  [![API-pictogram][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | De meest populaire connector binnen logische apps. Deze bevat triggers en acties voor asynchrone berichten en publiceren/abonneren met wachtrijen, abonnementen en onderwerpen. |
|  [![API-pictogram][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | We raden deze connector aan als u met SharePoint werkt en automatisering handig voor u zou zijn. Kan worden gebruikt met een on-premises SharePoint en SharePoint Online. | [![API-pictogram][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | Een van de meest gebruikte connectors. Deze kan verbinding maken met een on-premises SQL Server en Azure SQL Database. | 
| [![API-pictogram][Twittericon]<br/>**Twitter**][Twitterdoc] | Hiermee kunt u zich eenvoudig aanmelden met een Twitter-account en vervolgens een werkstroom starten wanneer er een nieuwe tweet wordt geplaatst. U kunt deze tweets vervolgens opslaan in een SQL Database of SharePoint-lijst. | | | 

## <a name="integration-account-connectors"></a>Integratieaccountconnectoren 

De Enterprise Integration Pack (EIP) bevat connectors die erg bekend zijn in de BizTalk Server-community. Wanneer u een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) aanschaft, krijgt u de volgende connectors: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API-pictogram][as2icon]<br/>**AS2</br>-decodering**][as2decode] | [![API-pictogram][as2icon]<br/>**AS2</br>-codering**][as2encode] | [![API-pictogram][x12icon]<br/>**EDIFACT</br>-decodering**][EDIFACTdecode] | [![API-pictogram][x12icon]<br/>**EDIFACT</br>-codering**][EDIFACTencode] |
[![API-pictogram][flatfileicon]<br/>**Plat bestand</br>, decodering**][flatfiledoc] | [![API-pictogram][flatfiledecodeicon]<br/>**Plat bestand</br>, codering**][flatfiledecodedoc] | [![API-pictogram][integrationaccounticon]<br/>**Integratie<br/>account**][integrationaccountdoc] | [![API-pictogram][xmltransformicon]<br/>**Transformeren<br/>XML**][xmltransformdoc] |
| [![API-pictogram][x12icon]<br/>**X12</br>-decodering**][x12decode] | [![API-pictogram][x12icon]<br/>**X12</br>-codering**][x12encode] | [![API-pictogram][xmlvalidateicon]<br/>**XML <br/>validatie**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Bedrijfsconnectoren

Hiermee maakt u verbinding met uw zakelijke toepassingen binnen uw logische apps.

|  |  |
| --- | --- |
|[![API-pictogram][MQicon]<br/>**MQ**][mqdoc]|[![API-pictogram][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>Volledige alfabetische lijst

[Connectorgegevens](/connectors/) geeft alle triggers en acties weer die in de swagger zijn gedefinieerd, plus eventuele beperkingen voor elke connector.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>10to8 Appointment Scheduling<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Azure API Management<br/>Azure App Services<br/>Azure Application<br/>Azure Automation<br/>[Azure Blob Storage][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure Functions][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Azure-wachtrijen<br/>Azure Resource Manager<br/>[Azure SQL Database][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Benchmark Email<br/>Bing Zoeken<br/>Bitbucket<br/>Bitly<br/>BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito Forms<br/>Cognitive Services - Computer Vision-API<br/>Cognitive Services - Face-API<br/>Cognitive Services - LUIS<br/>Cognitive Services - Tekstanalyse<br/>Common Data Service<br/>Content Conversion<br/>Control-Terminate<br/>[Aangepaste API's/web-apps][api/web-appdoc]<br/><br/><a name="d"></a>Gegevensbewerkingen<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[Bestandssysteem][filesystemdoc]<br/>[Plat bestand][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Google Agenda<br/>Google Contactpersonen<br/>Google Drive<br/>Google Sheets<br/>Google Tasks<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP-webhook][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Integratieaccount<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Middelgroot<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN weer<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Office 365-gebruikers<br/>Office 365 Video<br/>OneDrive<br/>OneDrive voor Bedrijven<br/>OneNote (Bedrijven)<br/>[Oracle Database][oracle-db-doc]<br/>Outlook Customer Manager<br/>Outlook-taken<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[Aanvraag/antwoord][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[SAP Application Server][sapconnector]<br/>[SAP Message Server][sapconnector]<br/>[Planning][recurrencedoc]<br/>Bereik<br/>SendGrid<br/>Berichten verzenden naar batch<br/>[Service Bus][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[XML transformeren][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Variabelen<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[XML-validatie][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> Als u wilt beginnen met Azure Logic Apps en u zich nog niet hebt aangemeld voor een Azure-account, gaat u naar [Logic Apps uitproberen](https://tryappservice.azure.com/?appservice=logic). U kunt onmiddellijk een tijdelijke logische app maken. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.

## <a name="connectors-as-triggers-and-actions"></a>Connectors als triggers en acties

Een **trigger** start een exemplaar van uw logische app of voert deze uit. Sommige connectors bieden triggers die aan uw app doorgeven dat er specifieke gebeurtenissen plaatsvinden. De FTP-connector heeft bijvoorbeeld de trigger `OnUpdatedFile`, die uw logische app start wanneer een bestand is bijgewerkt. 

Logische apps bevatten de volgende typen triggers:  

* *Poll-triggers*: met deze triggers wordt uw service met een opgegeven frequentie gepeild om te controleren of er nieuwe gegevens zijn. 

    Als er nieuwe gegevens beschikbaar zijn, wordt een nieuw exemplaar van uw logische app uitgevoerd waarbij de gegevens als invoer worden gebruikt. 

* *Push-triggers*: met deze triggers wordt geluisterd naar gegevens op een eindpunt of wordt er geluisterd of er een gebeurtenis plaatsvindt. Vervolgens wordt er een nieuwe instantie van uw logische app geactiveerd.

* *Trigger met terugkeerpatroon*: deze trigger start een exemplaar van uw logische app op basis van een voorgeschreven planning.

Connectors bieden ook de **acties** die u in uw werkstroom kunt gebruiken. Uw logische app kan bijvoorbeeld gegevens zoeken en deze gegevens later in uw logische app gebruiken. Om specifiek te zijn: u kunt klantgegevens opzoeken van een SQL Database en deze klantgegevens vervolgens gebruiken om uw werkstroom te maken. 

> [!TIP]
> In [Overzicht van connectors](connectors-overview.md) vindt u gedetailleerdere informatie over triggers en acties. 


## <a name="message-manipulation-actions"></a>Acties voor berichtbewerking

Logische apps bevatten ingebouwde acties waarmee u nettoladinggegevens kunt wijzigen of bewerken. De ingebouwde connector **Gegevensbewerkingen** bevat de volgende acties: 

| | |
|---|---|
| **Opstellen** | Maak of genereer waarden of objecten om later of tijdens het maken van uw werkstroom te gebruiken. U kunt bijvoorbeeld een JSON-object maken met waarden uit meerdere stappen of een constante berekenen waarnaar u later kunt verwijzen bij het uitvoeren van een logische app. |
| **CSV-tabel maken**<br/>**HTML-tabel maken** | Zet een matrixresultatenset om in een CSV- of HTML-tabel. Voeg bijvoorbeeld de CRM-actie Records vermelden toe en voeg een filter toe voor records die vandaag zijn toegevoegd. Verzend de resultaten vervolgens als een HTML-tabel in een e-mailbericht. |
| **Matrix filteren** (query) | Filter een resultatenset op de vermeldingen waarin u geïnteresseerd bent. Zoek bijvoorbeeld naar alle tweets met `#Azure` en 'filter' de geretourneerde tweets vervolgens om alleen resultaten te retourneren die `Tweeted_by_followers > 50` zijn. |
| **Koppelen** | Koppel een matrix met een delimeter. De bewerking Sleuteltermen detecteren retourneert bijvoorbeeld een matrix met sleuteltermen. U kunt ze 'koppelen' met een `,` of iets vergelijkbaars. In plaats van `["Some", "Phrase"]` hebt u dan `"Some, Phrase"`. |
| **JSON parseren** | Parseer en krijg toegang tot waarden van een JSON-object in de ontwerper. Als uw Azure-functie bijvoorbeeld een JSON-nettolading retourneert, kunt u deze parseren om later toegang te krijgen tot de JSON-eigenschappen in een andere stap. De actie valideert ook dat de JSON overeenkomt met het opgegeven schema tijdens runtime. | 
| **Selecteren** | Selecteer bepaalde eigenschappen van een matrix voor verdere verwerking. Als u records vermeldt vanuit SQL en er 15 kolommen worden geretourneerd, selecteert u slechts enkele van die kolommen voor verdere verwerking. De uitvoer is een matrix die alleen de door u geselecteerde eigenschappen bevat. |

## <a name="custom-connectors-and-azure-certification"></a>Aangepaste connectors en Azure-certificering 

Als u API's wilt aanroepen waarmee aangepaste code wordt uitgevoerd of die niet beschikbaar zijn als connectors, kunt u het Logic Apps-platform uitbreiden door [op REST gebaseerde API Apps te maken als aangepaste connectors](../logic-apps/logic-apps-create-api-app.md). 

Als u uw aangepaste API Apps openbaar wilt maken en beschikbaar wilt maken voor gebruik in Azure, verzendt u uw nominaties naar het [Microsoft Azure Certified-programma](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Help opvragen

Ga naar het [forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](http://aka.ms/logicapps-wish).

Ontbreekt er een onderwerp over connectors of bepaalde belangrijke details? Zo ja, help ons dan door onze bestaande onderwerpen uit te breiden of uw eigen te schrijven. Onze documentatie is open source en wordt gehost op GitHub. Ga aan de slag in onze [GitHub-opslagplaats](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Volgende stappen
* [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md)
* [Aangepaste API's maken voor logische apps](../logic-apps/logic-apps-create-api-app.md)
* [Uw logische apps bewaken](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Logische apps integreren met App Service API Apps."
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Bestanden in uw blobcontainer beheren met Azure Blob Storage-container."
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Logische apps integreren met Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Verbinding maken met IBM DB2 in de cloud of on-premises. Een rij bijwerken, een tabel ophalen, en meer"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Verbinding maken met Dynamics CRM Online zodat u kunt werken met CRM Online-gegevens."
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Verbinding maken met Azure Event Hubs. Gebeurtenissen tussen logische apps en Event Hubs verzenden en ontvangen"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Verbinding maken met een on-premises bestandssysteem"
[ftpdoc]: ./connectors-create-api-ftp.md "Verbinding maken met een FTP-/FTPS-server voor FTP-taken, zoals het uploaden, ophalen en verwijderen van bestanden, en meer."
[httpdoc]: ./connectors-native-http.md "HTTP-connector gebruiken voor HTTP-aanroepen"
[http-requestdoc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "HTTP + Swagger-connector gebruiken voor HTTP-aanroepen"
[informixdoc]: ./connectors-create-api-informix.md "Verbinding maken met Informix in de cloud of on-premises. Een rij lezen, de tabellen vermelden, en meer"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Logische apps integreren met geneste werkstromen"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Uw Office 365-account koppelen. E-mailberichten verzenden en ontvangen, uw agenda en contactpersonen beheren, en meer"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Verbinding maken met een Oracle-database om rijen toe te voegen, in te voegen, te verwijderen en meer"
[mqdoc]: ./connectors-create-api-mq.md "Verbinding maken met MQ on-premises of Azure en berichten verzenden en ontvangen"
[recurrencedoc]:  ./connectors-native-recurrence.md "Terugkerende acties voor logische apps activeren"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Verbinding maken met uw Salesforce-account. Accounts, potentiële klanten en verkoopkansen beheren, en meer"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Verbinding maken met een on-premises SAP-systeem"
[service-busdoc]: ./connectors-create-api-servicebus.md "Berichten vanuit de Service Bus-wachtrijen en -onderwerpen verzenden en berichten vanuit de Service Bus-wachtrijen en abonnementen ontvangen"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Verbinding maken met SharePoint Online. Documenten beheren, items vermelden, en meer"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Verbinding maken met de on-premises SharePoint-server. Documenten beheren, items vermelden, en meer"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Verbinding maken met Azure SQL Database of SQL server. Vermeldingen in een SQL-databasetabel maken, bijwerken, ophalen en verwijderen."
[twitterdoc]: ./connectors-create-api-twitter.md "Verbinding maken met Twitter. Tijdlijnen ophalen, tweets posten, en meer"
[webhookdoc]: ./connectors-native-webhook.md "Webhookacties en triggers toevoegen aan uw logische apps"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Meer informatie over Enterprise Integration AS2."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Meer informatie over Enterprise Integration X12"
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Meer informatie over Enterprise Integration met XML-validatie."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Meer informatie over Enterprise Integration-transformaties."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Meer informatie over AS2-decodering in Enterprise Integration"
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Meer informatie over AS2-codering in Enterprise Integration"
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Meer informatie over X12-decodering in Enterprise Integration"
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Meer informatie over X12-codering in Enterprise Integration"
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Meer informatie over EDIFACT-decodering in Enterprise Integration"
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Meer informatie over EDIFACT-codering in Enterprise Integration"
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Schema's, kaarten, partners en meer opzoeken in uw integratieaccount"


[boxDoc]: ./connectors-create-api-box.md "Verbinding maken met Box. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[delaydoc]: ./connectors-native-delay.md "Vertraagde acties uitvoeren"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Verbinding maken met Dropbox. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[facebookdoc]: ./connectors-create-api-facebook.md "Verbinding maken met Facebook. Iets op een tijdlijn posten, een paginafeed ophalen, en meer"
[githubdoc]: ./connectors-create-api-github.md "Verbinding maken met GitHub en problemen bijhouden"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Verbinding maken met GoogleDrive, zodat u met uw gegevens kunt werken"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Verbinding maken met Google Spreadsheets, zodat u uw werkbladen kunt wijzigen"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Maakt verbinding met Google Taken, zodat u uw taken kunt beheren"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Maakt verbinding met Google Calendar en kan een agenda beheren."
[http-responsedoc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[instagramdoc]: ./connectors-create-api-instagram.md "Verbinding maken met Instagram. Gebeurtenissen activeren of erop reageren"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Verbinding maken met uw MailChimp-account. E-mails beheren en automatiseren"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Verbinding maken met Mandrill voor communicatie"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Verbinding maken met Microsoft Translator. Tekst vertalen, talen detecteren, en meer " 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Informatie over video's, videolijsten, videokanalen en URL's voor het afspelen van Office 365-video's ophalen"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Verbinding maken met uw persoonlijke Microsoft OneDrive. Bestanden uploaden, verwijderen, vermelden, en meer"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Verbinding maken met uw zakelijke Microsoft OneDrive. Uw bestanden uploaden, verwijderen, vermelden, en meer"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Verbinding maken met uw Outlook-postvak. Uw e-mail, agenda's en contactpersonen beheren, en meer"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Verbinding maken met Microsoft Project Online. Uw projecten, taken, resources beheren, en meer"
[querydoc]: ./connectors-native-query.md "Matrices selecteren en filteren met behulp van de queryactie."
[rssdoc]: ./connectors-create-api-rss.md "Feeditems publiceren en ophalen, bewerkingen activeren wanneer een nieuw item wordt gepubliceerd in een RSS-feed."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Verbinding maken met SendGrid. E-mail verzenden en lijsten met geadresseerden beheren"
[sftpdoc]: ./connectors-create-api-sftp.md "Verbinding maken met uw SFTP-account. Bestanden uploaden, ophalen, verwijderen, en meer"
[slackdoc]: ./connectors-create-api-slack.md "Verbinding maken met Slack en berichten posten op Slack-kanalen"
[smtpdoc]: ./connectors-create-api-smtp.md "Verbinding maken met een SMTP-server en e-mail met bijlagen verzenden"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Maakt verbinding met SparkPost voor communicatie"
[trellodoc]: ./connectors-create-api-trello.md "Verbinding maken met Trello. Uw projecten beheren en dingen organiseren met anderen"
[twiliodoc]: ./connectors-create-api-twilio.md "Verbinding maken met Twilio. Berichten verzenden en ophalen, beschikbare nummers ophalen, binnenkomende telefoonnummers beheren, en meer."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Verbinding maken met Wunderlist. Uw taken en takenlijsten beheren, alles synchroniseren, en meer"
[yammerdoc]: ./connectors-create-api-yammer.md "Verbinding maken met Yammer. Berichten posten, nieuwe berichten ophalen, en meer"
[youtubedoc]: ./connectors-create-api-youtube.md "Verbinding maken met YouTube. Uw video's en kanalen beheren"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
