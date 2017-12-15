---
title: Berichten aan een Azure Storage-wachtrij toevoegen met behulp van Functions | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door berichten die zijn verzonden naar een Azure Storage-wachtrij.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 822879861ee8189cdd413f0061f26fb91819d88d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Berichten aan een Azure Storage-wachtrij toevoegen met behulp van Functions

In Azure Functions bieden invoer- en uitvoerbindingen een verklarende manier om verbinding te maken met externe servicegegevens vanuit uw functie. In dit onderwerp vindt u informatie over hoe u een bestaande functie kunt bijwerken door een uitvoerbinding toe te voegen waarmee u berichten naar Azure Queue Storage verzendt.  

![Bekijk het bericht in de logboeken.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Vereisten 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* [Microsoft Azure Storage Explorer](http://storageexplorer.com/) installeren.

## <a name="add-binding"></a>Een uitvoerbinding toevoegen
 
1. Vouw de functie-app en de functie uit.

2. Selecteer **Integreren** en **+ Nieuwe uitvoer**. Kies vervolgens **Azure Queue Storage** en kies **Selecteren**.
    
    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Gebruik de instellingen zoals opgegeven in de tabel: 

    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Wachtrijnaam**   | myqueue-items    | De naam van de wachtrij waarmee u verbinding moet maken in uw opslagaccount. |
    | **Opslagaccountverbinding** | AzureWebJobStorage | U kunt de opslagaccountverbinding gebruiken die al door de functie-app wordt gebruikt of u kunt een nieuwe maken.  |
    | **Naam van de berichtparameter** | outputQueueItem | De naam van de uitvoerbindingparameter. | 

4. Klik op **Opslaan** om de binding toe te voegen.
 
Nu u een uitvoerbinding hebt gedefinieerd, moet u de code bijwerken, zodat u de binding kunt gebruiken om berichten aan een wachtrij toe te voegen.  

## <a name="update-the-function-code"></a>De functiecode bijwerken

1. Selecteer de functie om de functiecode in de editor weer te geven. 

2. Werk de functiedefinitie voor een C#-functie als volgt bij om de **outputQueueItem**-opslagbindingparameter toe te voegen. Sla deze stap over voor een JavaScript-functie.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Voeg de volgende code aan de functie toe net voordat de methode retourneert. Gebruik het juiste fragment voor de taal van uw functie.

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Selecteer **Opslaan** om de wijzigingen op te slaan.

De waarde die aan de HTTP-trigger wordt doorgegeven, is opgenomen in een bericht dat aan de wachtrij wordt toegevoegd.
 
## <a name="test-the-function"></a>De functie testen 

1. Nadat de codewijzigingen zijn opgeslagen, selecteert u **Uitvoeren**. 

    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Raadpleeg de logboeken om er zeker van te zijn dat de functie is voltooid. Wanneer de uitvoerbinding voor het eerst wordt gebruikt, wordt er door de runtime van Functions een nieuwe wachtrij met de naam **outqueue** gemaakt in uw opslagaccount.

Vervolgens kunt u verbinding maken met uw opslagaccount om de nieuwe wachtrij en het bericht dat u hieraan hebt toegevoegd te controleren. 

## <a name="connect-to-the-queue"></a>Verbinding met de wachtrij maken

Sla de eerste drie stappen over als u Storage Explorer al hebt geïnstalleerd en met uw opslagaccount hebt verbonden.    

1. Kies in de functie de optie **Integreren** en nieuwe **Azure Queue Storage**-uitvoerbinding, en vouw vervolgens **Documentatie** uit. Kopieer de **Accountnaam** en de **Accountsleutel**. Met deze referenties kunt u verbinding maken met het opslagaccount.
 
    ![Haal de verbindingsreferenties voor het opslagaccount op.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Voer het hulpprogramma [Microsoft Azure Storage Explorer](http://storageexplorer.com/) uit, selecteer het verbindingspictogram aan de linkerkant, kies **Een opslagaccountnaam en -sleutel gebruiken** en selecteer **Volgende**.

    ![Voer het hulpprogramma Storage Account Explorer uit.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Plak de **accountnaam** en **accountsleutel** uit stap 1 in de bijbehorende velden, en selecteer vervolgens **Volgende** en **Verbinden**. 
  
    ![Plak de opslagreferenties en maak verbinding.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Vouw het gekoppelde opslagaccount uit. Vouw **Wachtrijen** uit en controleer of er een wachtrij met de naam **myqueue-items** bestaat. Er moet ook al een bericht in de wachtrij worden weergegeven.  
 
    ![Maak een opslagwachtrij.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een uitvoerbinding aan een bestaande functie toegevoegd. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Opslagwachtrijbindingen van Azure Functions) voor meer informatie over de binding met Queue Storage. 



