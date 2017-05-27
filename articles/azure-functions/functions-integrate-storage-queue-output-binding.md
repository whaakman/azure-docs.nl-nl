---
title: Een functie in Azure maken die wordt geactiveerd door wachtrijberichten | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door berichten die zijn verzonden naar een Azure Storage-wachtrij.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0e2501b0eb218d3c8a62dd4959b08ff85ec565eb
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Berichten aan een Azure Storage-wachtrij toevoegen met behulp van Functions

In Azure Functions bieden invoer- en uitvoerbindingen een verklarende manier om verbinding te maken met externe servicegegevens vanuit uw functie. In dit onderwerp vindt u informatie over hoe u een bestaande functie kunt bijwerken door een uitvoerbinding toe te voegen waarmee u berichten naar Azure Queue Storage verzendt.  

![Bekijk het bericht in de logboeken.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

Het doorlopen van de stappen in dit onderwerp kost u minder dan vijf minuten.

## <a name="prerequisites"></a>Vereisten 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

U moet ook de [Microsoft Azure Storage Explorer](http://storageexplorer.com/) downloaden en installeren. 

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="add-binding"></a>Een uitvoerbinding toevoegen
 
1. Vouw de functie-app en de functie uit.

2. Klik op **Integreren** en **+ Nieuwe uitvoer**. Klik vervolgens op **Azure Queue Storage** en klik op **Selecteren**.
    
    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Gebruik de instellingen zoals die in de tabel zijn opgegeven en klik vervolgens op **Opslaan**: 

    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Wachtrijnaam**   | myqueue-items    | De naam van de wachtrij waarmee u verbinding moet maken in uw opslagaccount. |
    | **Opslagaccountverbinding** | AzureWebJobStorage | U kunt de opslagaccountverbinding gebruiken die al door de functie-app wordt gebruikt of u kunt een nieuwe maken.  |
    | **Naam van de berichtparameter** | outQueueItem | De naam van de uitvoerbindingparameter. | 

Nu u een uitvoerbinding hebt gedefinieerd, moet u de code bijwerken, zodat u de binding kunt gebruiken om berichten aan een wachtrij toe te voegen.  

## <a name="update-the-function-code"></a>De functiecode bijwerken

1. Klik op de functie om de functiecode in de editor weer te geven. 

2. Werk de functiedefinitie voor een C#-functie als volgt bij om de **outQueueItem**-opslagbindingsparameter toe te voegen. Sla deze stap over voor een JavaScript-functie.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Voeg de volgende code aan de functie toe net voordat de methode retourneert. Gebruik het juiste fragment voor de taal van uw functie.

    ```javascript
    context.bindings.outQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Klik op **Opslaan** om de wijzigingen op te slaan.

De waarde die aan de HTTP-trigger wordt doorgegeven, is opgenomen in een bericht dat aan de wachtrij wordt toegevoegd.
 
## <a name="test-the-function"></a>De functie testen 

1. Nadat de codewijzigingen zijn opgeslagen, klikt u op **Uitvoeren**. 

    ![Voeg een Queue Storage-uitvoerbinding toe aan een functie in Azure Portal.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Raadpleeg de logboeken om er zeker van te zijn dat de functie is voltooid. Wanneer de uitvoerbinding voor het eerst wordt gebruikt, wordt er door de runtime van Functions een nieuwe wachtrij met de naam **outqueue** gemaakt in uw opslagaccount.

Vervolgens kunt u verbinding maken met uw opslagaccount om de nieuwe wachtrij en het bericht dat u hieraan hebt toegevoegd te controleren. 

## <a name="connect-to-the-queue"></a>Verbinding met de wachtrij maken

Sla de eerste drie stappen over als u Storage Explorer al hebt geïnstalleerd en met uw opslagaccount hebt verbonden.    

1. Klik in de functie op **Integreren** en op de nieuwe **Azure Queue Storage**-uitvoerbinding en vouw vervolgens **Documentatie** uit. Kopieer de **Accountnaam** en de **Accountsleutel**. Met deze referenties kunt u verbinding maken met het opslagaccount.
 
    ![Haal de verbindingsreferenties voor het opslagaccount op.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Voer het hulpprogramma [Microsoft Azure Storage Explorer](http://storageexplorer.com/) uit, klik op het verbindingspictogram aan de linkerkant, kies **Een opslagaccountnaam en -sleutel gebruiken** en klik op **Volgende**.

    ![Voer het hulpprogramma Storage Account Explorer uit.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Voer de **Accountnaam** en de **Accountsleutel** van stap 1 in. Klik op **Volgende** en vervolgens op **Verbinding maken**. 
  
    ![Voer de opslagreferenties in en maak verbinding.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Vouw het gekoppelde opslagaccount uit. Klik met de rechtermuisknop op **Wachtrijen** en controleer of er een wachtrij met de naam **myqueue-items** bestaat. Er moet ook al een bericht in de wachtrij worden weergegeven.  
 
    ![Maak een opslagwachtrij.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een uitvoerbinding aan een bestaande functie toegevoegd. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Opslagwachtrijbindingen van Azure Functions) voor meer informatie over de binding met Queue Storage. 




