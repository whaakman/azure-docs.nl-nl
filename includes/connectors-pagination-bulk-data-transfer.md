---
title: bestand opnemen
description: bestand opnemen
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/09/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 524bc1d3a19ad8ecfc8d0d210e735d6a9ae0058b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678211"
---
### <a name="set-up-pagination"></a>Paginering instellen

Voor sommige verbindingslijnen en hun bewerkingen waarmee meerdere items worden opgehaald, kunnen u uw resultaten van de connector pagina standaardgrootte overschrijden. In dit geval wordt retourneert de actie alleen de eerste pagina van de resultaten. Bijvoorbeeld, de pagina standaardgrootte voor de **SQL Server - Get-rijen** actie is 2048, maar mogelijk verschillen op basis van andere instellingen. Om ervoor te zorgen dat u alle records, schakelt u de **paginering** instellen voor deze actie. Deze instelling heeft uw logische app vraagt u de connector voor de resterende records, maar alle resultaten worden geretourneerd als een enkel bericht wanneer de actie is voltooid. 

Hier volgen enkele connectors waar u paginering voor specifieke acties kunt inschakelen: 

* <a href="https://docs.microsoft.com/connectors/db2/" target="_blank">DB2</a>
* <a href="https://docs.microsoft.com/connectors/dynamicscrmonline/" target="_blank">Dynamics 365 CRM Online</a>
* <a href="https://docs.microsoft.com/connectors/excel/" target="_blank">Excel</a>
* <a href="https://docs.microsoft.com/connectors/oracle/" target="_blank">Oracle Database</a>
* <a href="https://docs.microsoft.com/connectors/sharepointonline/" target="_blank">SharePoint</a>
* <a href="https://docs.microsoft.com/connectors/sql/" target="_blank">SQL Server</a> 

Hier volgt een voorbeeld van de **rijen ophalen** actie:

1. Als u wilt zoeken of de actie paginering ondersteunt, opent u de actie **instellingen**. 

   ![Open op de actie 'Instellingen'](./media/connectors-pagination-bulk-data-transfer/sql-action-settings.png)

2. Als de actie biedt ondersteuning voor paginering, wijzigt u de **paginering** instellen uit **uit** naar **op**. Om ervoor te zorgen dat de actie die een minimale set resultaten retourneert, Geef een waarde op voor **limiet**.

   ![Opgeven of de actie een minimum aantal resultaten retourneren](./media/connectors-pagination-bulk-data-transfer/sql-action-settings-pagination.png)

3. Als u klaar bent, kiest u **gedaan**.