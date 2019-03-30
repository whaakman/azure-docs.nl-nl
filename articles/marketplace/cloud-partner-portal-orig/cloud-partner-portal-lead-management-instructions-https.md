---
title: HTTPS-eindpunt | Microsoft Docs
description: Beheer van de potentiële klant voor een HTTPS-eindpunt configureren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: pbutlerm
ms.openlocfilehash: cfcd154b2f44c9e8acf12a9666abc9ce95fb3c26
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648347"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Beheer van leads met behulp van een HTTPS-eindpunt configureren

U kunt een HTTPS-eindpunt gebruiken voor het afhandelen van Azure Marketplace en AppSource leidt. Deze leads kunnen worden geschreven naar die kunnen worden geschreven naar een systeem Customer Relationship Management (CRM) of als een e-mailmelding verzonden. In dit artikel wordt beschreven hoe u lead beheer met de [Microsoft Flow](https://powerapps.microsoft.com/automate-processes/) automation-service.

## <a name="create-a-flow-using-microsoft-flow"></a>Een stroom maken met Microsoft Flow

1. Open de [Flow](https://flow.microsoft.com/) webpagina. Selecteer **aanmelden** of selecteer **gratis aanmelden** een gratis Flow-account maken.

2. Meld u aan en selecteer **mijn stromen** op de menubalk.

    ![Mijn stromen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

3. Selecteer **+ maken met een lege App**.

    ![Leeg item maken](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

4. Selecteer **leeg item maken**.

    ![Leeg item maken](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank2.png)

5. In de **connectors en triggers doorzoeken** veld, typt u 'aanvragen' te vinden van de aanvraag-connector.
6. Onder **Triggers**, selecteer **wanneer een HTTP-aanvraag wordt ontvangen**. 

    ![Selecteer de trigger HTTP-aanvraag ontvangen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

7. Gebruik een van de volgende stappen uit om te configureren de **hoofdtekst van de JSON-Schema aanvragen**:

   - Kopieer de [JSON-schema](#json-schema) aan het einde van dit artikel in de **hoofdtekst van de JSON-Schema aanvragen** in het tekstvak.
   - Selecteer **Voorbeeldnettolading om een schema te genereren**. In de **typt of plakt u een voorbeeld-JSON-nettolading** tekstvak plakken in de [voorbeeld van JSON](#json-example). Selecteer **gedaan** om het schema te maken.

   >[!Note]
   >Op dit moment in de stroom kunt u verbinding maken met een CRM-systeem of een e-mailmeldingen configureren.

### <a name="to-connect-to-a-crm-system"></a>Verbinding maken met een CRM-systeem

1. Selecteer **+ nieuwe stap**.
2. Kies de CRM-systeem van uw keuze met de actie voor het maken van een nieuwe record. De volgende schermopname ziet **Dynamics 365 - Maak een nieuwe record** als voorbeeld.

    ![Een nieuwe record maken](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Geef de **organisatienaam** dat de invoer voor de verbinding voor de connector is. Selecteer **leidt** uit de **entiteitnaam** vervolgkeuzelijst.

    ![Selecteer leads](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Stroom ziet u een formulier voor het leveren van gegevens over leads. U kunt items uit de ingevoerde aanvraag toewijzen door te kiezen dynamische inhoud toe te voegen. De volgende schermopname ziet **OfferTitle** als voorbeeld.

    ![Dynamische inhoud toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. De velden die u wilt en selecteer vervolgens toewijzen **opslaan** om op te slaan van de stroom.

6. Een HTTP POST-URL wordt gemaakt in de aanvraag. Deze URL te kopiëren en deze gebruiken als het HTTPS-eindpunt.

    ![URL van de HTTP Post](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>Voor het instellen van e-mailmelding

1. Selecteer **+ nieuwe stap**.
2. Onder **een actie kiezen**, selecteer **acties**.
3. Selecteer **Een e-mail verzenden** onder **Acties**.

    ![Een e-mailactie toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. In **een e-mailbericht verzenden**, configureert u de volgende vereiste velden:

   - **Naar** -ten minste één geldig e-mailadres invoeren.
   - **Onderwerp** -Aanmeldingsstroom biedt u de mogelijkheid om dynamische inhoud toevoegen, zoals **LeadSource** in de volgende schermopname.

     ![Een e-mailactie met behulp van dynamische inhoud toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Hoofdtekst** : in de lijst met dynamische inhoud, voegt u de informatie die u wilt dat in de hoofdtekst van het e-mailbericht. Bijvoorbeeld, LastName, FirstName, e-mailbericht, en de bedrijfsportal.

   Wanneer u klaar bent met instellen van het e-mailbericht, ziet het als in het voorbeeld in de volgende schermopname.

   ![Een e-mailactie toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Selecteer **opslaan** aan uw stroom voltooien.
6. Een HTTP POST-URL wordt gemaakt in de aanvraag. Deze URL te kopiëren en deze gebruiken als het HTTPS-eindpunt.

    ![URL van de HTTP Post](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Uw aanbieding leads verzenden naar het HTTPS-eindpunt configureren

Wanneer u de gegevens over leads management voor uw aanbieding configureert, selecteert u **HTTPS-eindpunt** voor de **leiden bestemming** en plak deze in de HTTP POST-URL die u in de vorige stap hebt gekopieerd.  

![Dynamische inhoud toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Wanneer leads worden gegenereerd, wordt in Microsoft leads verzendt naar de stroom, en u doorgestuurd naar de CRM-systeem of e-mailadres die u hebt geconfigureerd.

## <a name="json-schema-and-example"></a>Voorbeeld van de en JSON-schema

Het voorbeeld van een WebTest JSON maakt gebruik van het volgende schema:

### <a name="json-schema"></a>JSON-schema

``` json
{
  "$schema": "https://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```

U kunt kopiëren en bewerken van de volgende JSON-voorbeeld te gebruiken als een test in uw Microsoft-Flow.

### <a name="json-example"></a>Voorbeeld van JSON

```json
{
"OfferTitle": "Test Microsoft",
"LeadSource": "Test run through MS Flow",
"UserDetails": {
"Company": "Contoso",
"Country": "USA",
"Email": "someone@contoso.com",
"FirstName": "Some",
"LastName": "One",
"Phone": "16175555555",
"Title": "Esquire"
}
}
```

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, configureert u klant [leidt](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) in de Cloud Partner-Portal.
