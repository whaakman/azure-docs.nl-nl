---
title: Beveiligingsbewerkingen integreren met Microsoft Graph-Security - Azure Logic Apps
description: Van uw app threat mogelijkheden voor beveiliging, detectie en reactie verbeteren door het beheer van beveiligingsbewerkingen met Microsoft Graph beveiligings- en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: preetikr
ms.author: preetikr
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/30/19
tags: connectors
ms.openlocfilehash: 647df9e73804c8f261b58d5ede7c4b030d448fed
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513517"
---
# <a name="improve-threat-protection-by-integrating-security-operations-with-microsoft-graph-security--azure-logic-apps"></a>Beveiliging tegen bedreigingen verbeteren door de beveiligingsbewerkingen integratie met Microsoft Graph beveiligings- en Azure Logic Apps

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en de [Microsoft Graph Security](https://docs.microsoft.com/graph/security-concept-overview) -connector, u kunt verbeteren hoe uw app wordt gedetecteerd, worden beveiligd en reageert op bedreigingen met het maken van geautomatiseerde werkstromen voor het integreren van Microsoft Security-producten, services en partners. Bijvoorbeeld, kunt u [Azure Security Center-playbooks](../security-center/security-center-playbooks.md) die bewaken en beheren van Microsoft Graph-Security-entiteiten, zoals waarschuwingen. Hier volgen enkele scenario's ondersteund door de Microsoft Graph-Security-connector:

* Ontvang waarschuwingen op basis van query's of van waarschuwings-ID. Bijvoorbeeld, krijgt u een lijst met waarschuwingen met hoge urgentie.
* Waarschuwingen bijwerken. U kunt bijvoorbeeld waarschuwing toewijzingen, opmerkingen toevoegen aan waarschuwingen of waarschuwingen label bijwerken.
* Controleren wanneer waarschuwingen worden gemaakt of gewijzigd door het maken van [waarschuwen abonnementen (webhooks)](https://docs.microsoft.com/graph/api/resources/webhooks).
* Uw waarschuwing abonnementen beheren. U kunt bijvoorbeeld actieve abonnementen ophalen, de verlooptijd voor een abonnement verlengen of abonnementen verwijderen.

Werkstroom van uw logische app kunt acties te antwoorden krijgen van de Microsoft Graph-Security-connector en die uitvoer beschikbaar maken voor andere acties in uw werkstroom. U kunt ook andere acties in uw werkstroom gebruiken de uitvoer van de Microsoft Graph-Security-connector-acties hebben. Bijvoorbeeld, als u waarschuwingen met hoge urgentie via de Microsoft Graph-Security-connector, u kunt deze waarschuwingen verzenden in een e-mailbericht met behulp van de Outlook-connector. 

Zie voor meer informatie over de beveiliging van Microsoft Graph, de [Security-API van Microsoft Graph-overzicht](https://aka.ms/graphsecuritydocs). Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md). Als u Microsoft Flow of PowerApps zoekt, raadpleegt u [wat stroom is?](https://flow.microsoft.com/) of [wat is PowerApps?](https://powerapps.microsoft.com/)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Voor het gebruik van de Microsoft Graph-Security-connector, moet u hebben *expliciet opgegeven* Azure Active Directory (AD) tenant administrator toestemming, die deel uitmaakt van de [verificatievereisten voor Microsoft Graph-beveiliging ](https://aka.ms/graphsecurityauth). Deze toestemming is vereist voor de toepassings-ID van de Microsoft Graph-Security-connector en de naam, die u kunt ook vinden in de [Azure-portal](https://portal.azure.com):

   | Eigenschap | Value |
   |----------|-------|
   | **De naam van toepassing** | `MicrosoftGraphSecurityConnector` |
   | **Toepassings-ID** | `c4829704-0edc-4c3d-a347-7c4a67586f3c` |
   |||

   Als u wilt verlenen toestemming voor de connector, kan de beheerder van de Azure AD-tenant beide deze stappen volgen:

   * [Tenant-beheerder toestemming voor Azure AD-toepassingen](../active-directory/develop/v2-permissions-and-consent.md).

   * Tijdens de eerste uitvoering van uw logische app, kan uw app toestemming aanvragen van uw Azure AD-tenant-beheerder via de [toestemming toepassingservaring](../active-directory/develop/application-consent-experience.md).
   
* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot de beveiliging van Microsoft Graph-entiteiten, zoals waarschuwingen. Deze connector zijn momenteel geen triggers. Dus als u wilt een actie van de Microsoft Graph-beveiliging gebruikt, start uw logische app met een trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-microsoft-graph-security"></a>Verbinding maken met Microsoft Graph-beveiliging 

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Voeg voor lege, logische apps, de trigger en eventuele andere vereiste acties die u wilt voordat u een Microsoft Graph-Security-actie toevoegen.

   -of-

   Voor bestaande logische apps, onder de laatste stap waaraan u wilt toevoegen van een actie Microsoft Graph Security Kies **nieuwe stap**.

   -of-

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (+) die wordt weergegeven, en selecteer **een actie toevoegen**.

1. Typ 'microsoft graph security' als filter in het zoekvak. Selecteer de actie die u wilt in de lijst met acties.

1. Meld u aan met uw Microsoft Graph-Security-referenties.

1. Geef de benodigde informatie voor de geselecteerde actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="add-actions"></a>Acties toevoegen

Hier vindt u meer gedetailleerde informatie over het gebruik van de verschillende acties die beschikbaar zijn met de Microsoft Graph-Security-connector.

### <a name="manage-alerts"></a>Waarschuwingen beheren

Als u wilt filteren, sorteren, of de meest recente resultaten oplevert, bieden *alleen* de [ODATA-queryparameters ondersteund door Microsoft Graph](https://docs.microsoft.com/graph/query-parameters). *Geef geen* de volledige basis-URL of de HTTP-actie, bijvoorbeeld `https://graph.microsoft.com/v1.0/security/alerts`, of de `GET` of `PATCH` bewerking. Hier volgt een voorbeeld waarin de parameters voor een **waarschuwingen ontvangen** actie wanneer u een lijst met waarschuwingen met hoge urgentie:

`Filter alerts value as Severity eq 'high'`

Zie voor meer informatie over de query's met deze connector kunt u de [beveiligingswaarschuwingen van Microsoft Graph-referentiedocumentatie](https://docs.microsoft.com/graph/api/alert-list). Meer informatie voor het bouwen van betere ervaringen met deze connector, over de [schema eigenschappen waarschuwingen](https://docs.microsoft.com/graph/api/resources/alert) die ondersteuning biedt voor de connector.

| Bewerking | Description |
|--------|-------------|
| **Waarschuwingen ontvangen** | Ontvang waarschuwingen gefilterd op basis van een of meer [waarschuwen eigenschappen](https://docs.microsoft.com/graph/api/resources/alert), bijvoorbeeld: <p>`Provider eq 'Azure Security Center' or 'Palo Alto Networks'` | 
| **Waarschuwing met ID ophalen** | Ophalen van een specifieke waarschuwing op basis van de waarschuwing-ID. | 
| **Waarschuwing bijwerken** | Bijwerken van een specifieke waarschuwing op basis van de waarschuwing-ID. <p>Als u wilt controleren of u de eigenschappen vereist en bewerkbare doorgeven in uw aanvraag, Zie de [bewerkbare eigenschappen van waarschuwingen](https://docs.microsoft.com/graph/api/alert-update). Als u wilt een waarschuwing toewijst aan een beveiligingsanalist, zodat ze kunnen onderzoeken, kunt u bijvoorbeeld van de waarschuwing bijwerken **toegewezen aan** eigenschap. |
|||

### <a name="manage-alert-subscriptions"></a>Waarschuwing-abonnementen beheren

Biedt ondersteuning voor Microsoft Graph [ *abonnementen*](https://docs.microsoft.com/graph/api/resources/subscription), of [ *webhooks*](https://docs.microsoft.com/graph/api/resources/webhooks). Als u wilt ophalen, bijwerken, of abonnementen verwijderen, bieden de [ODATA-queryparameters ondersteund door Microsoft Graph](https://docs.microsoft.com/graph/query-parameters) naar de Microsoft Graph-entiteit maken en opnemen `security/alerts` gevolgd door de ODATA-query. 
*Neem geen* de basis-URL, bijvoorbeeld `https://graph.microsoft.com/v1.0`. Gebruik in plaats daarvan de indeling in het volgende voorbeeld:

`security/alerts?$filter=status eq 'New'`

| Bewerking | Description |
|--------|-------------|
| **Maken van abonnementen** | [Maken van een abonnement](https://docs.microsoft.com/graph/api/subscription-post-subscriptions) die ontvangt u een melding over wijzigingen. U kunt dit abonnement voor de specifieke Waarschuwingstypen die u wilt filteren. Bijvoorbeeld, kunt u een abonnement dat u ontvangt een melding over waarschuwingen met hoge urgentie. |
| **Actieve abonnementen ophalen** | [Niet-verlopen abonnementen ophalen](https://docs.microsoft.com/graph/api/subscription-list). | 
| **Abonnement bijwerken** | [Bijwerken van een abonnement](https://docs.microsoft.com/graph/api/subscription-update) door te geven van de abonnements-ID. Om uit te breiden van uw abonnement, kunt u bijvoorbeeld van het abonnement bijwerken `expirationDateTime` eigenschap. | 
| **Abonnement verwijderen** | [Een abonnement verwijdert](https://docs.microsoft.com/graph/api/subscription-delete) door te geven van de abonnements-ID. | 
||| 

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](https://aka.ms/graphsecurityconnectorreference).

## <a name="get-support"></a>Ondersteuning krijgen

Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
