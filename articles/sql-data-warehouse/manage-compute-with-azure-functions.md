---
title: 'Zelfstudie: Beheren van Computing met Azure Functions in Azure SQL Data Warehouse | Microsoft Docs'
description: Azure Functions gebruiken om de rekenniveaus van uw datawarehouse te beheren.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 04/27/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 0832fbcacd8b58ffaf36ce2e55e3add151a881db
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470192"
---
# <a name="use-azure-functions-to-manage-compute-resources-in-azure-sql-data-warehouse"></a>Gebruik Azure Functions voor het beheren van rekenresources in Azure SQL Data Warehouse

In deze zelfstudie maakt gebruik van Azure Functions voor het beheren van rekenresources voor een datawarehouse in Azure SQL Data Warehouse.

Als u de Azure-functie-app met SQL Data Warehouse wilt gebruiken, dient u voor hetzelfde abonnement als uw datawarehouse-exemplaar een [service-principalaccount](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) te maken met inzenderstoegang. 

## <a name="deploy-timer-based-scaling-with-an-azure-resource-manager-template"></a>Een timer gebaseerde schalen met een Azure Resource Manager-sjabloon implementeren

Als u wilt implementeren de sjabloon, moet u de volgende informatie:

- Naam van de resourcegroep waarin zich het SQL DW-exemplaar bevindt
- Naam van de logische server waarin zich het SQL DW-exemplaar bevindt
- Naam van het SQL DW-exemplaar
- Tenant-id (directory-id) van de Azure Active Directory
- Abonnements-id 
- Toepassings-id van de service-principal
- Geheime sleutel van de service-principal

Zodra u de voorgaande informatie hebt, kunt u deze sjabloon implementeren:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Nadat u de sjabloon hebt geïmplementeerd, vindt u drie nieuwe resources: een gratis Azure App Service-Plan, een functie-App op basis van gebruik plan en een opslagaccount dat wordt gebruikt voor het registreren en de wachtrij voor bewerkingen. Lees ook de overige secties om te zien hoe u de geïmplementeerde functies aan uw behoeften kunt aanpassen.

## <a name="change-the-compute-level"></a>Het rekenniveau wijzigen

1. Ga naar de functie-app-service. Als u de sjabloon met de standaardwaarden hebt geïmplementeerd, wordt *DWOperations* de naam van deze service. Als de functie-app is geopend, ziet u dat er vijf functies voor de functie-app-service zijn geïmplementeerd. 

   ![Functies die met sjabloon zijn geïmplementeerd](media/manage-compute-with-azure-functions/five-functions.png)

2. Selecteer *DWScaleDownTrigger* of *DWScaleUpTrigger*, afhankelijk van of u de tijd voor omhoog of omlaag schalen wilt wijzigen. Selecteer integreren in de vervolgkeuzelijst.

   ![Integreren als functie selecteren](media/manage-compute-with-azure-functions/select-integrate.png)

3. De waarde die momenteel moeten worden weergegeven is *%ScaleDownTime%* of *%ScaleUpTime%*. Deze waarden geven aan dat de planning is gebaseerd op waarden die zijn gedefinieerd in de [Toepassingsinstellingen][Application Settings]. Voor nu kunt u deze waarde genegeerd en de planning aanpassen aan uw tijd van voorkeur op basis van de volgende stappen.

4. Voeg in het gedeelte met de planning de tijd toe die door de CRON-expressie moet worden uitgedrukt. Deze tijd geeft aan hoe vaak u SQL Data Warehouse omhoog wilt schalen. 

  ![Functieplanning wijzigen](media/manage-compute-with-azure-functions/change-schedule.png)

  De waarde voor `schedule` is een [CRON-expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) die de volgende zes velden omvat: 
  ```json
  {second} {minute} {hour} {day} {month} {day-of-week}
  ```

  Bijvoorbeeld, *"0 30 9 ** 1-5"* geven een trigger elke weekdag om 9:30 uur. Ga naar Azure Functions [Voorbeelden van de planning][schedule examples] voor meer informatie.


## <a name="change-the-time-of-the-scale-operation"></a>Wijzigen van de tijd van de schaalbewerking

1. Ga naar de functie-app-service. Als u de sjabloon met de standaardwaarden hebt geïmplementeerd, wordt *DWOperations* de naam van deze service. Als de functie-app is geopend, ziet u dat er vijf functies voor de functie-app-service zijn geïmplementeerd. 

2. Selecteer *DWScaleDownTrigger* of *DWScaleUpTrigger*, afhankelijk van of u de rekenwaarde voor omhoog of omlaag schalen wilt wijzigen. Na het selecteren van de functies, moet in het deelvenster het bestand *index.js* te zien zijn.

   ![Rekenniveau van functietrigger wijzigen](media/manage-compute-with-azure-functions/index-js.png)

3. Wijzig de waarde van *ServiceLevelObjective* tot het gewenste niveau en druk op Opslaan. Deze waarde is het rekenniveau waarnaar uw datawarehouse-exemplaar wordt geschaald op basis van de planning in de sectie integreren is gedefinieerd.

## <a name="use-pause-or-resume-instead-of-scale"></a>Onderbreken of Hervatten gebruiken in plaats van Schalen 

De functies die momenteel standaard zijn ingeschakeld zijn *DWScaleDownTrigger* en *DWScaleUpTrigger*. Als u in plaats daarvan de functies voor onderbreken en hervatten wilt gebruiken, kunt u *DWPauseTrigger* of *DWResumeTrigger* inschakelen.

1. Ga naar het deelvenster Functies.

   ![Deelvenster Functies](media/manage-compute-with-azure-functions/functions-pane.png)



2. Klik op de schuifschakelaar van de trigger(s) die u wilt inschakelen.

3. Ga naar de tabbladen *Integreren* van de bijbehorende triggers om de planning te wijzigen.

   > [!NOTE]
   > Het functionele verschil tussen de triggers vergroten/verkleinen en de triggers onderbreken/hervatten is het bericht dat wordt verzonden naar de wachtrij. Zie voor meer informatie, [een nieuwe triggerfunctie toevoegen][Add a new trigger function].


## <a name="add-a-new-trigger-function"></a>Een nieuwe triggerfunctie toevoegen

Er zijn momenteel slechts twee schaalfuncties in de sjabloon opgenomen. Met deze functies kunt in de loop van een dag, u alleen schalen eenmaal omlaag en eenmaal. Voor gedetailleerdere controle, zoals meerdere keren per dag omlaag schalen of andere schaalgedrag hebben in het weekend, moet u een andere trigger toevoegen.

1. Maak een nieuwe, lege functie. Selecteer de *+* knop naast functies om het deelvenster van de sjabloon functie weer te geven.

   ![Nieuwe functie maken](media/manage-compute-with-azure-functions/create-new-function.png)

2. Selecteer in Taal de optie *JavaScript* en vervolgens *TimerTrigger*.

   ![Nieuwe functie maken](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. Geef de functie een naam en stel de planning in. In de afbeelding ziet u hoe u elke zaterdag om middernacht (van vrijdag op zaterdag) een functie kunt laten activeren.

   ![Zaterdag omlaag schalen](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Kopieer de inhoud van *index.js* vanuit een van de andere triggerfuncties.

   ![index.js kopiëren](media/manage-compute-with-azure-functions/index-js.png)

5. De variabele van de bewerking op het gewenste gedrag als volgt instellen:

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


## <a name="complex-scheduling"></a>Complex plannen

In deze sectie kort zien wat nodig is om op te halen meer complexe planning onderbreken, hervatten en mogelijkheden voor schalen.

### <a name="example-1"></a>Voorbeeld 1:

Elke dag om 8:00 uur omhoog schalen naar DW600 en om 20:00 uur omlaag schalen naar DW200.

| Function  | Planning     | Bewerking                                |
| :-------- | :----------- | :--------------------------------------- |
| Functie1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| Functie2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-2"></a>Voorbeeld 2: 

Dag omhoog schalen om 8 uur naar DW1000, omlaag schalen naar DW600: 00 uur en omlaag schalen naar DW200 exemplaar om 22 uur.

| Function  | Planning     | Bewerking                                |
| :-------- | :----------- | :--------------------------------------- |
| Functie1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| Functie2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Functie3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-3"></a>Voorbeeld 3: 

Op weekdagen om 8:00 uur omhoog schalen naar DW1000 en om 16:00 uur omlaag schalen naar DW600. Onderbreken op vrijdag om 23:00 uur, hervatten op maandag om 7:00 uur.

| Function  | Planning       | Bewerking                                |
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
