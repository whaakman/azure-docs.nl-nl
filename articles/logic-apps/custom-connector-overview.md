---
title: Overzicht van aangepaste connectors - Azure Logic Apps | Microsoft Docs
description: Overzicht over het maken van aangepaste connectors voor ondersteuning en integratiescenario's uit te vouwen
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 0515b21bc7c7cc737e14fda016606bbea1aab476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="custom-connectors-overview"></a>Overzicht van aangepaste connectors

Alle zonder code te schrijven, kunt u werkstromen of apps met [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), [Microsoft Flow](https://flow.microsoft.com), of [Microsoft PowerApps](https://powerapps.microsoft.com). Om te integreren van uw gegevens en bedrijfsprocessen, bieden deze services [100 + connectors](../connectors/apis-list.md), niet alleen voor Microsoft-services en producten, zoals Azure en SQL Server, maar andere services, GitHub, Salesforce en Twitter . 

U wilt echter soms aanroepen van API's, services en systemen die niet beschikbaar als vooraf gedefinieerde connectors. Ter ondersteuning van meer op maat gemaakte scenario's voor bedrijven en productiviteit behoeften van uw gebruikers, kunt u *aangepaste connectors* met hun eigen triggers en acties.
Aangepaste connectors Vouw de integraties, reach, detectie en gebruik voor uw service of product die helpen kan verhogen en de acceptatie van de klant versnellen.

Dit diagram ziet u de interactie tussen Web-API, een aangepaste Logic Apps-connector maken voor die API en een logische app die geschikt is voor die API met behulp van de aangepaste connector:

![Conceptueel overzicht voor een Web-API, aangepaste connector en logische app](./media/custom-connector-overview/custom-connector-conceptual.png)

Dit overzicht bevat een overzicht van de algemene taken op hoog niveau voor het maken, beveiligen, registreren, en met behulp van plus eventueel delen of gecertificeerd door uw connectors:

![Aangepaste connector stappen ontwerpen](./media/custom-connector-overview/authoring-steps.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt maken van de connector van begin tot eind, moet u deze items:

* Een Azure-abonnement. Als u geen een abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). Anders zich aanmelden voor een [abonnement met betalen naar gebruik](https://azure.microsoft.com/pricing/purchase-options/).

* Een RESTful-API met geverifieerde toegang van bepaalde typen. Zie voor meer informatie [aangepaste connectors maken op basis van Web-API's](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Zie voor de patronen die u voor de triggers en acties van de connector gebruiken kunt, [maken van aangepaste API's die u vanuit logic app werkstromen aanroepen kunt](../logic-apps/logic-apps-create-api-app.md).

* Een item hier:

  * Een [OpenAPI 2.0-bestand](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md), voorheen bekend als Swagger
  * Een URL naar de definitie van een OpenAPI
  * Een [Postman verzameling](../logic-apps/custom-connector-api-postman-collection.md) voor uw API 

  Als u een van deze items niet hebt, bieden we hulp voor u.

* Optioneel: Een afbeelding gebruiken als een pictogram voor uw aangepaste connector

## <a name="1-build-your-restful-api"></a>1. Uw RESTful-API maken

Technisch gezien maakt een connector is een wrapper rond een REST-API die gebaseerd op een OpenAPI (voorheen Swagger) specificatie en kunt u de onderliggende service Neem contact op met Logic Apps, stroom of PowerApps. Dus eerst moet u een volledig werkend API voordat u uw aangepaste connector maakt. 

U kunt elke taal en het platform voor uw API gebruiken. Voor Microsoft-technologieën raden we aan een van deze platforms:

* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/)
* [Azure API-Apps](https://azure.microsoft.com/services/app-service/api/)

Deze zelfstudie ziet u bijvoorbeeld [het bouwen van een aangepaste connector van een Web-API](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Zie voor de patronen die u voor de triggers en acties van de connector gebruiken kunt, [maken van aangepaste API's die u vanuit logic app werkstromen aanroepen kunt](../logic-apps/logic-apps-create-api-app.md).

## <a name="2-secure-your-api"></a>2. Uw API beveiligen

U kunt deze normen verificatie voor uw API's en -connectors gebruiken:

   * [OAuth 2.0](https://oauth.net/2/), waaronder [Azure Active Directory](https://azure.microsoft.com/develop/identity/) of bepaalde services, zoals Dropbox, GitHub en SalesForce
   * Algemene OAuth 2.0
   * [Basisverificatie](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [API-sleutel](https://swagger.io/docs/specification/authentication/api-keys/)

U kunt verificatie van de Azure Active Directory (Azure AD) voor uw API in de Azure-portal instellen zodat u niet hoeft te implementeren, verificatie via code. Of u kunt vereisen en afdwingen van verificatie via de API-code. 

Ga als volgt de juiste zelfstudies voor meer informatie:

* [Logic Apps: Beveiligen van uw aangepaste connector](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Stroom: Beveiligen van uw aangepaste connector](https://ms.flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/)
* [PowerApps: Beveiligen van uw aangepaste connector](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/)

## <a name="3-describe-your-api"></a>3. Uw API wordt beschreven 

Ervan uitgaande dat uw API een soort geverifieerde toegang is, moet u uw API-interface en bewerkingen beschrijven zodat Logic Apps, stroom of PowerApps met uw API communiceren kunnen.
Gebruik een van deze standaarddefinities branche:

* Een [OpenAPI 2.0-bestand](https://swagger.io/) kunt u starten door bouwen met een bestaand OpenAPI-bestand.

  Als u geen ervaring met OpenAPI, gaat u naar [aan de slag met Swagger](http://swagger.io/getting-started/) op de site swagger.io.
  Zie voor een voorbeeldbestand OpenAPI, de [Text Analytics API-documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/export?DocumentFormat=Swagger&ApiName=Azure). 

* Een verzameling Postman, die automatisch voor u een OpenAPI-bestand genereert. Wanneer u een bestand OpenAPI nog geen en niet een wilt maken, kunt u een aangepaste connector nog steeds gemakkelijk maken met behulp van een verzameling Postman.

  Als u geen ervaring met Postman, [installeren de app Postman](https://www.getpostman.com/apps) van hun site. Zie voor meer informatie [beschrijven aangepaste connectors met Postman](../logic-apps/custom-connector-api-postman-collection.md).

> [!IMPORTANT]
> De bestandsgrootte moet minder dan 1 MB.

Achter de schermen Logic Apps, stroom en PowerApps uiteindelijk OpenAPI gebruiken, een verzameling Postman parseert en zet de verzameling een definitiebestand OpenAPI. Hoewel OpenAPI 2.0 en Postman verzamelingen verschillende indelingen gebruiken, zijn beide taal networkdirect, machineleesbare documenten die bewerkingen en parameters van uw API wordt beschreven. U kunt deze documenten genereren uit verschillende hulpprogramma's, op basis van de taal en platform gebruikt uw API. Wanneer u de connector registreert, kunt u een bestand OpenAPI maken.

Bijvoorbeeld, kunt u een OpenAPI-bestand of een verzameling Postman van een REST-API-eindpunt, met inbegrip van:

* Openbaar beschikbare connectors, bijvoorbeeld [Spotify](https://developer.spotify.com/), [Slack](https://api.slack.com/), [Rackspace](http://docs.rackspace.com/), enzovoort

* Een API die u hebt gemaakt en geïmplementeerd naar een cloud-hostingprovider, zoals Azure, Heroku en Google Cloud

* Een aangepaste regel-of-business-API die is geïmplementeerd in uw netwerk, maar alleen als die API wordt weergegeven op het openbare internet

## <a name="4-register-your-connector"></a>4. Registreren van de connector

Het registratieproces kunt Logic Apps, stroom, of PowerApps inzicht in uw API-kenmerken, inclusief de beschrijving, de vereiste verificatie en de bewerkingen, inclusief de parameters en uitvoer voor elke bewerking. Wanneer u het registratieproces start, kunt u een bestand OpenAPI of een verzameling Postman, die automatisch wordt ingevuld in de van metagegevensvelden in de registratiewizard opgeven. Op elk gewenst moment kunt u deze velden waarden bewerken.

![Uw API wordt beschreven](./media/custom-connector-overview/choose-api-definition-file.png)

Voor het registreren van de connector, de juiste zelfstudie te volgen:

* [Logic Apps: Registreer uw connector](../logic-apps/logic-apps-custom-connector-register.md)
* [Stroom: Registreer uw connector](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registreer uw connector](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)

## <a name="5-use-your-connector-in-a-logic-app-flow-or-app"></a>5. Gebruik de connector in een logische app, de stroom of de app 

Op dezelfde manier dat u Microsoft beheerde connectors kunt u de connector. Bijvoorbeeld in een werkstroom logic app toevoegen, uw aangepaste connector zodat u kunt verbinding met uw API maken en bewerkingen die de API biedt op dezelfde manier als u bewerkingen voor Microsoft beheerde connectors aanroepen aanroepen.

## <a name="6-share-your-connector"></a>6. Delen van de connector 

U kunt uw connector delen met gebruikers in uw organisatie op dezelfde manier als u resources in Logic Apps, stroom of PowerApps te delen. Hoewel delen optioneel is, moet u wellicht scenario's waar u uw connectors delen met andere gebruikers.

Geregistreerd maar niet-gecertificeerde aangepaste connectors werken zoals Microsoft beheerde connectors, maar zichtbaar zijn en beschikbare *alleen* voor de auteur en gebruikers die werken met de dezelfde Azure Active Directory-tenant en de Azure-abonnement van de connector voor logische apps in de regio waar de apps die zijn geïmplementeerd. De volgende stap wordt beschreven hoe u kunt delen de connector met externe gebruikers buiten deze grenzen, bijvoorbeeld met alle Logic Apps, stroom en PowerApps gebruikers.

> [!IMPORTANT]
> Als u een connector deelt, gaan anderen mogelijk afhankelijk zijn van die connector. 
> ***De connector verwijdert, worden alle verbindingen met die connector.***

* [Logic Apps: Uw connector delen](../logic-apps/logic-apps-custom-connector-register.md)
* [Stroom: Uw connector delen](https://ms.flow.microsoft.com/documentation/register-custom-api/#share-your-custom-connector)
* [PowerApps: Uw connector delen](https://powerapps.microsoft.com/tutorials/register-custom-api/#share-your-custom-connector)

## <a name="7-certify-your-connector"></a>7. De connector certificeren

De connector delen met alle gebruikers in Logic Apps, stroom en PowerApps, indienen desgewenst uw connector voor Microsoft-certificeringsinstantie. Dit proces controleert uw connector, wordt gecontroleerd op naleving van technische en inhoud en functionaliteit voor Logic Apps, stroom en PowerApps valideert voordat Microsoft uw connector kunt publiceren. Meer informatie over [het indienen van de connector voor Microsoft-certificeringsinstantie](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Ondersteuning krijgen

* Voor ondersteuning bij het voorbereiden en ontwikkeling, e- [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft actief bewaakt dit account voor ontwikkelaars vragen en problemen en doorgestuurd naar het juiste team. 

* Zie voor veelgestelde vragen de [aangepaste connector Veelgestelde vragen](../logic-apps/custom-connector-faq.md)

## <a name="next-steps"></a>Volgende stappen

* [Een aangepaste connector van een Web-API maken](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Verificatie voor aangepaste connectors instellen](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Aangepaste connectors met Postman verzamelingen beschrijven](../logic-apps/custom-connector-api-postman-collection.md)
* [Aangepaste connectors voor Microsoft-certificeringsinstantie indienen](../logic-apps/custom-connector-submit-certification.md)
