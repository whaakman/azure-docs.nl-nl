---
title: Azure Table | Microsoft Docs
description: Leadbeheer configureren voor Azure Table.
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
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 60e3e3d81b07bf7ae681b5cef2d6d9681877a35f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810050"
---
<a name="lead-management-instructions-for-azure-table"></a>Potentiële klanten Management instructies voor het Azure-tabel
============================================

In dit artikel wordt beschreven hoe u Azure Table configureren voor het opslaan van de potentiële verkopen. Azure-tabel kunt u opslaan en aanpassen van klantgegevens.

## <a name="to-configure-azure-table"></a>Het configureren van Azure-tabel

1.  Als u een Azure-account niet hebt, kunt u [maken van een gratis proefaccount](https://azure.microsoft.com/pricing/free-trial/).

2.  Nadat u uw Azure-account actief is, aanmelden bij de [Azure-portal](https://portal.azure.com).
3.  Maak een opslagaccount in de Azure-portal. De volgende schermopname ziet u hoe u een opslagaccount maken. Zie voor meer informatie over prijzen voor storage [prijzen voor storage](https://azure.microsoft.com/pricing/details/storage/).

    ![Stappen voor het maken van een Azure storage-account](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Kopieer de verbindingsreeks van de storage-account voor de sleutel en plak deze in de **Storage Account Connection String** veld in de Cloud Partner-Portal. Een voorbeeld van een connection-string is `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Azure-opslagsleutel](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

U kunt [Azure storage explorer](http://azurestorageexplorer.codeplex.com/) of een ander hulpprogramma om de gegevens in uw storage-tabel te bekijken. U kunt ook de gegevens in Azure Table exporteren.
de gegevens.

## <a name="optional-to-use-azure-functions-with-an-azure-table"></a>**(Optioneel)**  Azure Functions gebruikt met een Azure-tabel

Als u wilt aanpassen hoe ontvangt u leads, gebruik [Azure Functions](https://azure.microsoft.com/services/functions/) met een Azure-tabel. De service Azure Functions kunt u het proces voor het genereren van leads te automatiseren.

De volgende stappen laten zien hoe een Azure-functie die gebruikmaakt van een timer maken. Om de vijf minuten de functie zoekt in de Azure-tabel voor nieuwe records en gebruikt vervolgens de SendGrid-service voor het verzenden van een e-mailmelding.


1.  [Maak](https://portal.azure.com/#create/SendGrid.SendGrid) een gratis SendGrid-account in uw Azure-abonnement.

    ![SendGrid maken](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)

2.  Maak een SendGrid-API-sleutel 
    - Selecteer **beheren** naar SendGrid UI
    - Selecteer **instellingen**, **API-sleutels**, en vervolgens maakt u een sleutel met e-Mail verzenden -\> volledige toegang
    - De API-sleutel opslaan


    ![SendGrid-API-sleutel](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)


3.  [Maak](https://portal.azure.com/#create/Microsoft.FunctionApp) een Azure-functie-app met de optie voor het abonnement voor webhosting met de naam 'Verbruik plannen'.

    ![Azure-functie-App maken](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)


4.  Maak een nieuwe functiedefinitie.

    ![De definitie van de Azure-functie maken](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)
 

5.  Als u de functie voor het verzenden van een update voor een bepaald tijdstip, selecteer de **TimerTrigger-CSharp** als de optie starter.

     ![Azure functie tijd trigger-optie](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)


6.  Vervang de code 'Opstellen' door het volgende codevoorbeeld. Bewerk de e-mailadressen met adressen die u wilt gebruiken voor de afzender en de ontvanger.

        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;
        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }
        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";
            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();
            // Check how many rows were added
            int rowsCount = rowsList.Count;
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");
                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };
                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);
                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }

    ![Azure functie-codefragment](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)


7.  Selecteer **integreren** en **invoer** voor het definiëren van de Azure Table-verbinding.

    ![Azure-functie integreren](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)


8.  Voer de naam van de tabel en het definiëren van de verbindingsreeks door te selecteren **nieuwe**.


    ![Verbinding met het Azure functie tabel](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9.  Nu de uitvoer als SendGrid definiëren en laat de standaardwaarden staan.

    ![SendGrid-uitvoer](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)

    ![Standaardinstellingen voor SendGrid-uitvoer](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. SendGrid-API-sleutel toevoegen aan de functie App-instellingen met de naam 'SendGridApiKey' en de waarde uit de API-sleutels in SendGrid UI

    ![Beheren van SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![SendGrid sleutel beheren](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

Wanneer u klaar bent met het configureren van de functie, wordt de code in de sectie integreren moet eruitzien als in het volgende voorbeeld.

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }

11. De laatste stap is om te navigeren naar de gebruikersinterface ontwikkelen van de functie en selecteer vervolgens **uitvoeren** de timer gestart. Nu u een melding ontvangt wanneer een nieuwe lead is beschikbaar in.
