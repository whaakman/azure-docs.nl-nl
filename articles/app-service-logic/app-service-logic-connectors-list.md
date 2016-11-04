---
title: Lijst met beschikbare connectors en API-apps | Microsoft Docs
description: Meer informatie over connectors en API-apps in Azure App Service
services: logic-apps
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: cgronlun

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: mandia

---
# Lijst met connectors en API-apps voor gebruik in Logic Apps
> [!NOTE]
> Deze versie van het artikel is van toepassing Logic Apps-previewschemaversie 2014-12-01. Zie [New Connectors List](../connectors/apis-list.md) (Lijst met nieuwe connectors) voor de General Availability (GA)-versie van Logic Apps.
> 
> 

Lees meer over de beschikbare connectors en API Apps die door Microsoft zijn gemaakt en die u kunt gebruiken in Logic Apps.

Zie [Prijzen van Azure App Service](https://azure.microsoft.com/pricing/details/app-service/) voor informatie over prijzen en een overzicht van wat is opgenomen in elke servicecategorie.

> [!NOTE]
> Als u aan de slag wilt met Logic Apps en u zich nog niet hebt aangemeld voor een Azure-account, gaat u naar [Logic Apps uitproberen](https://tryappservice.azure.com/?appservice=logic). U kunt onmiddellijk een tijdelijke logische app in App Service maken. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## Basisconnectors
In de volgende tabel ziet u de beschikbare connectors en API-apps die zijn gemaakt door Microsoft en die beschikbaar zijn als basisconnectors.

| Naam | Beschrijving |
| --- | --- |
| [Bing Vertalen](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) |Gebruik Bing om tekst in een andere taal te vertalen. |
| [HTTP](app-service-logic-connector-http.md) |De HTTP-listener opent een eindpunt dat als een HTTP-server fungeert en luistert naar inkomende HTTP- of HTTPS-aanvragen. Voor de HTTP-actie is geen API-app vereist en de actie wordt op systeemeigen wijze ondersteund in Logic Apps. |
| [Slack](app-service-logic-connector-slack.md) |U kunt verbinding maken met Slack en berichten plaatsen in ongebruikte kanalen. |

## Enterprise Integration-connectors
In de volgende tabel ziet u de beschikbare connectors en API-apps die zijn gemaakt door Microsoft en die beschikbaar zijn als Enterprise Integration-connectors.

| Naam | Beschrijving |
| --- | --- |
| [BizTalk Rules](app-service-logic-use-biztalk-rules.md) |Gebruik BizTalk Rules om de bedrijfslogica binnen een organisatie te definiëren en te beheren. Hiermee kunt u het bedrijfsbeleid bijwerken zonder dat u hiervoor de gekoppelde toepassingen opnieuw hoeft te compileren of te implementeren. |
| [BizTalk XPath Extractor](app-service-logic-xpath-extract.md) |Hiermee kunt u de gegevens uit XML-inhoud opzoeken en ophalen op basis van het gewenste XPath. |
| [DB2-connector](app-service-logic-connector-db2.md) |Met deze connector kunt u verbinding maken met een IBM DB2-database, on-premises en via een virtuele Azure-machine waarop een Windows-besturingssysteem wordt uitgevoerd. Hiermee kunnen Web-API- en OData-API-bewerkingen worden toegewezen aan Informix Structured Query Language-opdrachten. <br/><br/>Er zijn geen triggers. Acties omvatten tabel selecteren, invoegen, bijwerken, verwijderen en aangepaste instructies.<br/><br/>Met deze connector kan Microsoft Client voor DRDA ook verbinding maken met een Informix-server via een TCP/IP-netwerk. |
| [File](app-service-logic-connector-file.md) |Met deze connector kunt u verbinding maken met het on-premises bestandssysteem of netwerk en verschillende bestandstaken uitvoeren, waaronder het uploaden, verwijderen en weergeven van bestanden. |
| [Informix](app-service-logic-connector-informix.md) |Hiermee kunt verbinding maken met een IBM Informix-database, on-premises en via een virtuele Azure-machine waarop een Windows-besturingssysteem wordt uitgevoerd. Hiermee kunnen Web-API- en OData-API-bewerkingen worden toegewezen aan Informix Structured Query Language-opdrachten.<br/><br/>Er zijn geen triggers. Acties omvatten tabel selecteren, invoegen, bijwerken, verwijderen en aangepaste instructies.<br/><br/>Bij on-premises gebruik kunt u VPN of Azure ExpressRoute gebruiken. Met deze connector kan Microsoft Client voor DRDA ook verbinding maken met een Informix-server via een TCP/IP-netwerk. |
| [Microsoft SQL Server](app-service-logic-connector-sql.md) |Hiermee kunt u verbinding maken met on-premises SQL Server of Azure SQL Database. U kunt items in een SQL-databasetabel maken, bijwerken, ophalen en verwijderen. |
| MQ |Hiermee kunt u verbinding maken met IBM WebSphere MQ Server versie 8, on-premises en via een virtuele Azure-machine waarop een Windows-besturingssysteem wordt uitgevoerd. Bij on-premises gebruik kunt u VPN of Azure ExpressRoute gebruiken. De connector bevat ook Microsoft Client voor MQ.<br/><br/>Er zijn geen triggers. Er zijn geen acties.<br/><br/>**Opmerking** Kan momenteel niet worden gebruikt met Logic Apps. |

## Connectors als triggers
Verschillende connectors bieden triggers voor Logic Apps. Er zijn twee typen triggers:

1. Poll-triggers: Met deze triggers wordt uw service met een opgegeven frequentie gepeild om te controleren of er nieuwe gegevens zijn. Als er nieuwe gegevens beschikbaar zijn, wordt een nieuw exemplaar van uw logische app uitgevoerd waarbij de gegevens als invoer worden gebruikt. Om te voorkomen dat dezelfde gegevens meerdere keren wordt gebruikt, kunnen met de trigger gegevens worden opgeschoond die zijn gelezen en aan de logische app zijn doorgegeven. Voorbeelden van dergelijke connectors zijn File, SQL en Azure Storage.
2. Push-triggers: Met deze triggers wordt geluisterd naar gegevens op een eindpunt of wordt geluisterd of een gebeurtenis plaatsvindt. Vervolgens wordt een nieuw exemplaar van een logische app getriggerd. Voorbeelden van dergelijke connectors zijn HTTP-listener en Twitter.

## Connectors als acties
Connectors kunnen ook worden gebruikt als acties in uw logische app. Acties zijn handig voor het opzoeken van gegevens in de logische app, die vervolgens in de uitvoering kunnen worden gebruikt. U kunt tijdens het afhandelen van een order bijvoorbeeld gegevens in een SQL-database opzoeken voor meer informatie over een klant. Of misschien wilt u gegevens op een doel schrijven, bijwerken of verwijderen. U kunt dit doen met de acties die via de connectors beschikbaar zijn. Acties worden toegewezen aan bewerkingen in API-apps (zoals gedefinieerd door de Swagger-metagegevens).

## Uw eigen connectors en API-apps maken
[Naslaginformatie over connectors en API-apps](http://aka.ms/appservicesconnectorreference)  
[API-app-triggers voor Azure App Service](../app-service-api/app-service-api-dotnet-triggers.md)  
[Naslaginformatie over Logic Apps](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## Meer informatie over connectors en API-apps
[Wat zijn connectors en BizTalk API-apps?](app-service-logic-what-are-biztalk-api-apps.md)  
[Hybride verbindingsbeheer gebruiken in Azure App Service](app-service-logic-hybrid-connection-manager.md)  
[Ingebouwde API-apps en connectors beheren en bewaken](app-service-logic-monitor-your-connectors.md)

<!--HONumber=Oct16_HO3-->


