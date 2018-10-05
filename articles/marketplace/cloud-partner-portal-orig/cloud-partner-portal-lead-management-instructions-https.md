---
title: HTTPS-eindpunt | Microsoft Docs
description: Beheer van leads voor Https configureren.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: fd13a7281c7e8702fd199364261ebcd458db0555
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809440"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Beheer van leads met behulp van een HTTPS-eindpunt configureren

U kunt een HTTPS-eindpunt gebruiken voor het afhandelen van Azure Marketplace en AppSource leads die kunnen worden geschreven naar een CRM-systeem. In dit artikel wordt beschreven hoe u lead configureren met behulp van de automation-service van Microsoft Flow.


## <a name="create-a-flow-using-microsoft-flow"></a>Een stroom maken met Microsoft Flow

1.  Open de [Flow](https://flow.microsoft.com/) webpagina. Selecteer **aanmelden** of selecteer **gratis aanmelden** een gratis Flow-account maken.

2.  Meld u aan en selecteer **mijn stromen** op de menubalk.

    ![Mijn stromen](./media/cloud-partner-portal-lead-management-instructions-https/image001.png)

3.  Selecteer **leeg item maken**.

    ![Leeg item maken](./media/cloud-partner-portal-lead-management-instructions-https/image003.png)


4.  Selecteer de **aanvraag/antwoord** connector en zoek vervolgens naar de aanvraagtrigger. 

    ![Leeg item maken](./media/cloud-partner-portal-lead-management-instructions-https/image005.png)

5. Selecteer de **aanvragen** trigger.
    ![Trigger voor de aanvraag](./media/cloud-partner-portal-lead-management-instructions-https/image007.png)


6.  Kopieer de **voorbeeld van JSON** aan het einde van dit artikel in de **hoofdtekst van de JSON-Schema aanvragen**.

7.  Een nieuwe stap toevoegen en kies de CRM-systeem van uw keuze met de actie voor het maken van een nieuwe record. De volgende schermopname ziet **Dynamics 365 - Maak een nieuwe record** als voorbeeld.

    ![Een nieuwe record maken](./media/cloud-partner-portal-lead-management-instructions-https/image009.png)

8.  Geef op de invoer voor de verbinding voor uw connector en selecteer de **leidt** entiteit.

    ![Selecteer leads](./media/cloud-partner-portal-lead-management-instructions-https/image011.png)

9.  Stromen een formulier voor het leveren van gegevens over leads wordt weergegeven. U kunt items uit de ingevoerde aanvraag toewijzen door te kiezen dynamische inhoud toe te voegen.

    ![Dynamische inhoud toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/image013.png)

10.  De velden die u wilt en selecteer vervolgens toewijzen **opslaan** om op te slaan van de stroom.

11. Een HTTP POST-URL wordt gemaakt in de aanvraag. Deze URL te kopiëren en deze gebruiken als het HTTPS-eindpunt.

    ![URL van de HTTP Post](./media/cloud-partner-portal-lead-management-instructions-https/image015.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Uw aanbieding leads verzenden naar het HTTPS-eindpunt configureren

Wanneer u de gegevens over leads management voor uw aanbieding configureert, selecteert u **HTTPS-eindpunt** voor het doel leiden en plakken in de HTTP POST-URL die u in de vorige stap hebt gekopieerd.  

![Dynamische inhoud toevoegen](./media/cloud-partner-portal-lead-management-instructions-https/image017.png)

Wanneer leads worden gegenereerd, verzendt Microsoft leads naar de stroom, en u doorgestuurd naar de CRM-systeem die u hebt geconfigureerd.


## <a name="json-example"></a>Voorbeeld van JSON

``` json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
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
