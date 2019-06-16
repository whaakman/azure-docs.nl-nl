---
title: Meer gegevens, items of records met paginering - Azure Logic Apps ophalen
description: Paginering instellen die groter zijn dan de standaardlimiet voor het formaat van pagina voor connectoracties in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 2d1bcf2cf83fab106f79120c3caacc424f839836
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64476539"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Meer gegevens, items of records ophalen met behulp van paginering in Azure Logic Apps

Wanneer u gegevens, items of records ophalen met behulp van een actie van de connector in [Azure Logic Apps](../logic-apps/logic-apps-overview.md), krijgt u mogelijk resultatensets zo groot is dat alle resultaten van de actie niet worden geretourneerd op hetzelfde moment. Met sommige acties, het aantal resultaten van de connector standaardpaginaformaat groter zijn dan. In dit geval wordt retourneert de actie alleen de eerste pagina van de resultaten. Bijvoorbeeld, de standaardpagina van de SQL Server-connector **rijen ophalen** actie is 2048, maar kan variëren op basis van andere instellingen.

Sommige acties kunnen u inschakelen op een *paginering* instellen zodat uw logische app meer resultaten tot de paginering is bereikt ophalen, maar deze resultaten geretourneerd als een enkel bericht wanneer de actie is voltooid. Wanneer u paginering gebruikt, moet u een *drempelwaarde* waarde, het doelaantal resultaten die u wilt dat de actie om terug te keren. De actie haalt u resultaten tot het bereiken van de opgegeven drempelwaarde. Wanneer het totale aantal items lager dan de opgegeven drempelwaarde is, worden alle resultaten opgehaald door de actie.

Inschakelen van de paginering instelling haalt pagina's van resultaten op basis van het formaat van pagina van de connector. Dit gedrag betekent dat soms u meer resultaten dan de opgegeven drempelwaarde krijgt mogelijk. Bijvoorbeeld, als u de SQL-Server **rijen ophalen** actie, die ondersteuning biedt voor paginering instelling:

* Standaardpaginaformaat dat van de actie is 2048 records per pagina.
* Stel dat u hebt 10.000 records en 5000 records opgeven als het minimum.
* Paginering pagina's van records, om ten minste het opgegeven minimum ontvangt, de actie 6144 records (3 pagina's x 2048 records), niet-5000 records geretourneerd.

Hier volgt een lijst met slechts enkele van de connectors waar u groter zijn dan de standaardgrootte van de pagina voor specifieke acties:

* [Azure Blob Storage](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Oracle Database](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De logische app en de actie waar u Paginering inschakelen. Als u een logische app niet hebt, raadpleegt u [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Paginering inschakelen

Om te bepalen of een actie biedt ondersteuning voor paginering in Logic App Designer, Controleer de instellingen van de actie voor de **paginering** instelling. In dit voorbeeld laat zien hoe paginering in de SQL-Server inschakelen **rijen ophalen** actie.

1. In de rechterbovenhoek van de actie, kiest u de weglatingstekens ( **...** ) en selecteer **instellingen**.

   ![Open de instellingen van de actie](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Als de actie paginering ondersteunt, de actie wordt de **paginering** instelling.

1. Wijzig de **paginering** instellen op basis van **uit** naar **op**. In de **drempelwaarde** eigenschap, Geef een geheel getal voor het doelgetal van resultaten die u wilt dat de actie om terug te keren.

   ![Minimum aantal resultaten om terug te keren opgeven](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Wanneer u klaar bent, kiest u **gedaan**.

## <a name="workflow-definition---pagination"></a>Definitie van de werkstroom - paginering

Wanneer u paginering voor een actie die ondersteuning biedt voor deze functie inschakelt, definitie van de werkstroom van uw logische app bevat de `"paginationPolicy"` eigenschap samen met de `"minimumItemCount"` eigenschap in van de actie `"runtimeConfiguration"` eigenschap, bijvoorbeeld:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Ondersteuning krijgen

Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
