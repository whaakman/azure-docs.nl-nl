---
title: Azure SQL Data Warehouse - Azure Functions gebruiken om SQL Data Warehouse-rekenniveaus te automatiseren | Microsoft Docs
description: Azure Functions gebruiken om de rekenniveaus van uw datawarehouse te beheren.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 11/06/2017
ms.author: elbutter
ms.openlocfilehash: 8947da9d34261be46ad9aea961b6020141484172
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-azure-functions-to-automate-sql-dw-compute-levels"></a>Azure Functions gebruiken om SQL DW-rekenniveaus te automatiseren

In deze zelfstudie wordt gedemonstreerd hoe u Azure Functions kunt gebruiken om de rekenniveaus van Azure SQL Data Warehouse te beheren. Deze architecturen zijn aanbevolen voor gebruik met SQL Data Warehouse [Geoptimaliseerd voor Elasticity][Performance Tiers].

Als u de Azure-functie-app met SQL Data Warehouse wilt gebruiken, dient u voor hetzelfde abonnement als uw datawarehouse-exemplaar een [service-principalaccount](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) te maken met inzenderstoegang. 

## <a name="deploy-timer-based-scaler-with-an-azure-resource-manager-template"></a>Tijdgebaseerde schaler implementeren met een Azure Resource Manager-sjabloon

Als u de sjabloon wilt implementeren, hebt u de volgende informatie nodig:

- Naam van de resourcegroep waarin zich het SQL DW-exemplaar bevindt
- Naam van de logische server waarin zich het SQL DW-exemplaar bevindt
- Naam van het SQL DW-exemplaar
- Tenant-id (directory-id) van de Azure Active Directory
- Abonnements-id 
- Toepassings-id van de service-principal
- Geheime sleutel van de service-principal

Met de bovenstaande informatie kunt u de volgende sjabloon implementeren:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Als u de sjabloon hebt geïmplementeerd, vindt u drie nieuwe resources: een gratis Azure App Service-abonnement, een op verbruik gebaseerd functie-app-abonnement en een opslagaccount waarmee het aanmelden en de wachtrij voor bewerkingen wordt afgehandeld. Lees ook de overige secties om te zien hoe u de geïmplementeerde functies aan uw behoeften kunt aanpassen.

### <a name="change-the-scale-up-or-scale-down-compute-level"></a>Het rekenniveau voor omhoog of omlaag schalen wijzigen

1. Ga naar de functie-app-service. Als u de sjabloon met de standaardwaarden hebt geïmplementeerd, wordt *DWOperations* de naam van deze service. Als de functie-app is geopend, ziet u dat er vijf functies voor de functie-app-service zijn geïmplementeerd. 

   ![Functies die met sjabloon zijn geïmplementeerd](media/manage-compute-with-azure-functions/five-functions.png)

2. Selecteer *DWScaleDownTrigger* of *DWScaleUpTrigger*, afhankelijk van of u de tijd voor omhoog of omlaag schalen wilt wijzigen. Selecteer Integreren in de vervolgkeuzelijst.

   ![Integreren als functie selecteren](media/manage-compute-with-azure-functions/select-integrate.png)

3. De waarde die momenteel moeten worden weergegeven is *%ScaleDownTime%* of *%ScaleUpTime%*. Deze waarden geven aan dat de planning is gebaseerd op waarden die zijn gedefinieerd in de [Toepassingsinstellingen][Application Settings]. Voorlopig kunt u dit negeren en de planning aanpassen aan uw tijd van voorkeur op basis van de volgende stappen.

4. Voeg in het gedeelte met de planning de tijd toe die door de CRON-expressie moet worden uitgedrukt. Deze tijd geeft aan hoe vaak u SQL Data Warehouse omhoog wilt schalen. 

  ![Functieplanning wijzigen](media/manage-compute-with-azure-functions/change-schedule.png)

  De waarde voor `schedule` is een [CRON-expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) die de volgende zes velden omvat: 
  ```json
  {second} {minute} {hour} {day} {month} {day-of-week}
  ```

  Zo geeft *"0 30 9 * * 1-5"* een trigger aan voor elke weekdag om 9:30 uur. Ga naar Azure Functions [Voorbeelden van de planning][schedule examples] voor meer informatie.


### <a name="change-the-scale-up-or-scale-down-time"></a>De tijd voor omhoog of omlaag schalen wijzigen

1. Ga naar de functie-app-service. Als u de sjabloon met de standaardwaarden hebt geïmplementeerd, wordt *DWOperations* de naam van deze service. Als de functie-app is geopend, ziet u dat er vijf functies voor de functie-app-service zijn geïmplementeerd. 

2. Selecteer *DWScaleDownTrigger* of *DWScaleUpTrigger*, afhankelijk van of u de rekenwaarde voor omhoog of omlaag schalen wilt wijzigen. Na het selecteren van de functies, moet in het deelvenster het bestand *index.js* te zien zijn.

   ![Rekenniveau van functietrigger wijzigen](media/manage-compute-with-azure-functions/index-js.png)

3. Wijzig de waarde van *ServiceLevelObjective* tot het gewenste niveau en druk op Opslaan. Dit is het rekenniveau waarnaar het exemplaar van uw datawarehouse wordt geschaald op basis van de planning die in de sectie Integreren is gedefinieerd.

### <a name="use-pause-or-resume-instead-of-scale"></a>Onderbreken of Hervatten gebruiken in plaats van Schalen 

De functies die momenteel standaard zijn ingeschakeld zijn *DWScaleDownTrigger* en *DWScaleUpTrigger*. Als u in plaats daarvan de functies voor onderbreken en hervatten wilt gebruiken, kunt u *DWPauseTrigger* of *DWResumeTrigger* inschakelen.

1. Ga naar het deelvenster Functies.

   ![Deelvenster Functies](media/manage-compute-with-azure-functions/functions-pane.png)



2. Klik op de schuifschakelaar van de trigger(s) die u wilt inschakelen.

3. Ga naar de tabbladen *Integreren* van de bijbehorende triggers om de planning te wijzigen.

   [!NOTE]: The functional difference between the scaling triggers and the pause/resume triggers is the message that is sent to the queue. See [Add a new trigger function][Add a new trigger function] for more information.



### <a name="add-a-new-trigger-function"></a>Een nieuwe triggerfunctie toevoegen

Er zijn momenteel slechts twee schaalfuncties in de sjabloon opgenomen. Dat betekent dat u per dag slechts eenmaal omhoog of omlaag kunt schalen. Als u een meer verfijnde regulering wilt, bijvoorbeeld om vaker per dag te kunnen schalen of als tijdens het weekend een andere schaling wilt, dient u een andere trigger toe te voegen.

1. Maak een nieuwe, lege functie. Selecteer de *+*-knop naast Functies om het deelvenster voor functiesjablonen weer te geven.

   ![Nieuwe functie maken](media/manage-compute-with-azure-functions/create-new-function.png)

2. Selecteer in Taal de optie *JavaScript* en vervolgens *TimerTrigger*.

   ![Nieuwe functie maken](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. Geef de functie een naam en stel de planning in. In de afbeelding ziet u hoe u elke zaterdag om middernacht (van vrijdag op zaterdag) een functie kunt laten activeren.

   ![Zaterdag omlaag schalen](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Kopieer de inhoud van *index.js* vanuit een van de andere triggerfuncties.

   ![index.js kopiëren](media/manage-compute-with-azure-functions/index-js.png)

5. Stel de bewerkingsvariabele als volgt in op het gewenste gedrag:

   ```javascript
   // Resume the data warehouse instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the data warehouse instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the data warehouse instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```


### <a name="complex-scheduling"></a>Complex plannen

In deze sectie wordt een korte demonstratie gegeven van wat er bij een meer complexe planning komt kijken, bijvoorbeeld onderbreken, hervatten en mogelijkheden voor schalen.

#### <a name="example-1"></a>Voorbeeld 1:

Elke dag om 8:00 uur omhoog schalen naar DW600 en om 20:00 uur omlaag schalen naar DW200.

| Functie  | Planning     | Bewerking                                |
| :-------- | :----------- | :--------------------------------------- |
| Functie1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| Functie2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-2"></a>Voorbeeld 2: 

Elke dag om 8:00 uur omhoog schalen naar DW1000, om 16:00 uur eenmaal omlaag schalen naar DW600 en om 22:00 uur omlaag schalen naar DW200.

| Functie  | Planning     | Bewerking                                |
| :-------- | :----------- | :--------------------------------------- |
| Functie1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| Functie2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Functie3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-3"></a>Voorbeeld 3: 

Op weekdagen om 8:00 uur omhoog schalen naar DW1000 en om 16:00 uur omlaag schalen naar DW600. Onderbreken op vrijdag om 23:00 uur, hervatten op maandag om 7:00 uur.

| Functie  | Planning       | Bewerking                                |
| :-------- | :------------- | :--------------------------------------- |
| Functie1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| Functie2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Functie3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| Functie4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |



## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure-functies die door een [timertrigger](../azure-functions/functions-create-scheduled-function.md) worden geactiveerd.

Bekijk de [opslagplaats met voorbeelden](https://github.com/Microsoft/sql-data-warehouse-samples) van SQL Data Warehouse (Engelstalig).



[schedule examples]: ../azure-functions/functions-bindings-timer.md#example

[Application Settings]: ../azure-functions/functions-how-to-use-azure-function-app-settings.md
[Add a new trigger function]: manage-compute-with-azure-functions.md#add-a-new-trigger-function

[Performance Tiers]: performance-tiers.md
