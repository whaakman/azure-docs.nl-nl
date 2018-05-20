---
title: 'Zelfstudie: Azure Functions integreren met Azure SignalR-service | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure Functions gebruikt met de Azure SignalR-service
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/24/2018
ms.author: wesmc
ms.openlocfilehash: b1bb6b3fe545d5a42fa985ab85bd7a914128e56b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-integrate-azure-functions-with-azure-signalr-service"></a>Zelfstudie: Azure Functions integreren met Azure SignalR-service

Deze zelfstudie is gebaseerd op de toepassing voor een chatruimte die in eerdere zelfstudies is gemaakt. Als u de zelfstudies [Create a chat room with SignalR Service (Een chatruimte maken met SignalR)](signalr-quickstart-dotnet-core.md) en [Azure SignalR Service authentication (Verificatie van Azure SignalR)](./signalr-authenticate-oauth.md) nog niet hebt voltooid, moet u dat eerst doen. 

Een veelvoorkomend scenario met realtime-toepassingen is dat updates van inhoud op een server worden gepubliceerd naar webclients. [Azure Functions](../azure-functions/functions-overview.md) is bij uitstek geschikt voor het genereren van deze updates van inhoud. Een belangrijk voordeel van het gebruik van Azure Functions is dat u uw code op aanvraag kunt uitvoeren, zonder dat u zich zorgen hoeft te maken over de architectuur van een complete toepassing, of de infrastructuur om deze uit te voeren. Bovendien betaalt u alleen voor de tijd dat de code daadwerkelijk wordt uitgevoerd.  

Normaal gesproken zou dit scenario een probleem opleveren bij gebruik van SignalR omdat SignalR probeert een verbinding tussen de client en server te onderhouden voor het pushen van updates van inhoud. Aangezien de code alleen op aanvraag wordt uitgevoerd, kan er geen verbinding worden onderhouden. De Azure SignalR-service kan dit scenario echter ondersteunen omdat de service de verbindingen voor u beheert tijdens runtime.

In deze zelfstudie gebruikt u Azure Functions om aan het begin van elke minuut berichten te genereren met behulp van een timerfunctie. De functie publiceert de berichten naar alle clients van de chatruimte die in eerdere zelfstudies is gemaakt. Zie [Timer Function](../azure-functions/functions-create-scheduled-function.md) (Timer, functie) voor meer informatie over timerfuncties.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

De code voor deze zelfstudie is beschikbaar als download in de [GitHub-opslagplaats met AzureSignalR-voorbeelden](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer).

![Chat-app met serverberichten](./media/signalr-integrate-functions/signalr-functions-complete.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe Timer-functie maken met Azure Functions met behulp van de Azure CLI.
> * De Timer-functie configureren voor implementatie vanuit de lokale Git-opslagplaats.
> * De timer verbinden met de SignalR-service om elke minuut updates te pushen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* [Git](https://git-scm.com/)
* [.NET Core-SDK](https://www.microsoft.com/net/download/windows) 
* [Azure Cloud Shell geconfigureerd](https://docs.microsoft.com/azure/cloud-shell/quickstart)
* Download of kloon de [AzureSignalR-voorbeelden](https://github.com/aspnet/AzureSignalR-samples)uit de GitHub-opslagplaats.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app maken om de uitvoeringsomgeving voor uw functies te definiëren. Met de functie-app kunt u ook functies groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Zie [Uw eerste functie maken met Azure CLI](../azure-functions/functions-create-first-azure-function-azure-cli.md) voor meer informatie.

In dit gedeelte gebruikt u Azure Cloud Shell voor het maken van een nieuwe Azure Functions-app die is geconfigureerd voor implementatie vanuit een lokale Git-opslagplaats. 

Wanneer u de resources van de functie-app maakt, moet u deze opslaan in de resourcegroep die u in de vorige zelfstudies hebt gebruikt. Door deze aanpak is het makkelijker om alle resources van de zelfstudies te beheren.

Kopieer het onderstaande script in de teksteditor en vervang de waarden van de variabele parameters met de waarden voor uw resources. Kopieer en plak het bijgewerkte script in Azure Cloud Shell en druk op **Enter** om een opslagaccount en functie-app te maken.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your values from previous    === 
#=== tutorials.                                                   ===
#====================================================================
ResourceGroupName=SignalRTestResources
location=eastus

#====================================================================
#=== Update these variables for the new function app and storage  ===
#=== account.                                                     ===
#====================================================================
functionappName=mysignalfunctionapp
storageAccountName=mystorageaccount

# Create a storage account to hold function app code and settings
az storage account create --resource-group $ResourceGroupName \
--name $storageAccountName \
--location $location --sku Standard_LRS

# Create the function app
az functionapp create --resource-group $ResourceGroupName \
--name $functionappName \
--consumption-plan-location $location \
--storage-account $storageAccountName

```

| Parameter | Beschrijving |
| -------------------- | --------------- |
| ResourceGroupName | De naam van deze resourcegroep is voorgesteld in eerdere zelfstudies. Het is een goed idee om alle resources van de zelfstudies bij elkaar te zetten. Gebruik de resourcegroep die u in de eerdere zelfstudies hebt gebruikt. | 
| location | Gebruik voor deze variabele de locatie die u ook hebt gebruikt bij het maken van de resourcegroep in de eerdere zelfstudies. | 
| functionappName | Werk deze variabele bij met een unieke naam voor uw nieuwe functie-app, zoals signalrfunctionapp22665120. | 
| storageAccountName | Voer een naam voor het nieuwe opslagaccount waarin de code en instellingen van de functie-app moeten worden opgeslagen. | 



## <a name="configure-the-function-app"></a>De functie-app configureren

In dit gedeelte configureert u de functie-app met een app-instelling met daarin de verbindingsreeks voor de resource voor uw Azure SignalR-service. Uw functiecode gebruikt deze instelling om verbinding te maken met de chatruimte en hiernaar berichten te publiceren. Daarnaast gaat u de functie-app configureren voor implementatie vanuit een lokale Git-opslagplaats.

Kopieer het onderstaande script en vervang de waarden van de parameters. Plak het bijgewerkte script in Azure Cloud Shell en druk op **Enter**.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your resources.              === 
#====================================================================
ResourceGroupName=SignalRTestResources
functionappName=mysignalfunctionapp

#========================================================================
#=== Replace this value with the connection string for your           ===
#=== SignalR Service resource.                                        ===
#========================================================================
connstring="Endpoint=<service_endpoint>;AccessKey=<access_key>;"

# Add the SignalR Service connection string app setting
az functionapp config appsettings set --resource-group $ResourceGroupName \
    --name $functionappName \
    --setting "Azure:SignalR:ConnectionString=$connstring"

# configure for deployment from a local Git repository
az functionapp deployment source config-local-git --name $functionappName \
    --resource-group $ResourceGroupName

```

| Parameter | Beschrijving |
| -------------------- | --------------- |
| ResourceGroupName | De naam van deze resourcegroep is voorgesteld in eerdere zelfstudies. Het is een goed idee om alle resources van de zelfstudies bij elkaar te zetten. Gebruik de resourcegroep die u in de eerdere zelfstudies hebt gebruikt. | 
| functionappName | Werk deze variabele bij met een unieke naam voor uw nieuwe functie-app, zoals signalrfunctionapp22665120. | 
| connstring | Voer de verbindingsreeks voor de resource van de SignalR-service in. U kunt deze verbindingsreeks ophalen op de pagina van de resource van de SignalR-service in Azure Portal door onder **INSTELLINGEN** te klikken op **Sleutels**. | 



Noteer de URL van de Git-implementatie die wordt geretourneerd door de laatste opdracht. U hebt deze URL later nodig voor het implementeren van de functiecode.


## <a name="the-timer-function"></a>De Timer-functie

Het voorbeeld van de Timer-functie bevindt zich in de map */samples/Timer* van de download, of kloon van de GitHub-opslagplaats met [AzureSignalR-voorbeelden](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer). De code van de Timer-functie bevindt zich in *TimerFunction.cs*. Deze code maakt gebruik van de verbindingsreeks die is opgeslagen met de standaardconfiguratiesleutel (*Azure:SignalR:ConnectionString*) om een proxy naar de hub te maken. Aangezien de functiecode op de server wordt uitgevoerd, hoeft de code niet te worden geverifieerd als een gewone client. De code wordt vertrouwd voor het gebruik van de verbindingsreeks. Met behulp van deze hub-proxy kan de functiecode elke methode aanroepen die u op de hub hebt gedefinieerd. De code roept de methode `BroadcastMessage` aan voor het publiceren van een bericht met de huidige tijd wanneer de trigger wordt geactiveerd.

De trigger voor de functiecode is een *timerTrigger*, die is gedefinieerd in de bindingen in *TimerFunction/function.json*. De functie bevat een [CRON expressie](https://wikipedia.org/wiki/Cron#CRON_expression) waarmee wordt ingesteld dat de timertrigger aan het begin van elke minuut moet worden geactiveerd.

```json
{
  "bindings": [
    {
      "type": "timerTrigger",
      "schedule": "0 * * * * *",
      "useMonitor": true,
      "runOnStartup": false,
      "name": "myTimer"
    }
  ],
  "disabled": false,
  "scriptFile": "../Timer.dll",
  "entryPoint": "Timer.TimerFunction.Run"
}
```


## <a name="building-the-timer-function"></a>De Timer-functie compileren

Gebruik in de volgende stappen de [opdrachtregelinterface (CLI) van .NET Core](https://docs.microsoft.com/dotnet/core/tools/) voor het bouwen van de functie en om de implementatie voor te bereiden:

1. Ga naar de submap */samples/Timer* van de download, of kloon van de GitHub-opslagplaats met [AzureSignalR-voorbeelden](https://github.com/aspnet/AzureSignalR-samples).

2. Herstel de NuGet-pakketten met de volgende opdracht:

        dotnet restore

3. Compileer de *Timer*-functie-app met de volgende opdracht:

        msbuild /p:Configuration=Release


## <a name="create-and-deploy-the-local-git-repo"></a>De lokale Git-opslagplaats maken en implementeren

1. Ga in een Git-shell naar de submap van de build, */samples/Timer/bin/Release/net461*.

        cd ./AzureSignalR-samples/samples/Timer/bin/Release/net461

2. Initialiseer de map als een nieuwe Git-opslagplaats met de volgende opdracht:

        git init

3. Voeg een nieuwe commit toe voor alle bestanden in de build-map.

        git add -A
        git commit -v -a -m "Initial Timer function commit"        

4. Voeg een extern eindpunt toe voor de URL van de Git-implementatie, die u hebt genoteerd tijdens de configuratie van de functie-app:

        git remote add Azure <enter your Git deployment URL>

5. De functie-app implementeren

        git push Azure master

   Zodra de code is geïmplementeerd, wordt de timer onmiddellijk aan het begin van minuut geactiveerd om de code uit te voeren.

## <a name="test-the-chat-app"></a>De chat-app testen

Ga naar de chattoepassing. De Timer-functie die u net hebt gemaakt, zal nu aan het begin van elke minuut de tijd laten zien.

![Chat-app met serverberichten](./media/signalr-integrate-functions/signalr-functions-complete.png)



## <a name="clean-up-resources"></a>Resources opschonen

Als u door wilt gaan met het testen van de toepassing, moet u de gemaakte resources behouden.

Als u niet door wilt gaan, kunt u de Azure-resources verwijderen om kosten te voorkomen. 

> [!IMPORTANT]
> Houd er rekening mee dat het verwijderen van een resourcegroep niet ongedaan kan worden gemaakt, en dat de resourcegroep en alle bijbehorende resources permanent worden verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de resources voor het hosten van dit voorbeeld in een bestaande resourcegroep hebt gemaakt en deze groep ook resources bevat die u wilt behouden, kunt u elke resource afzonderlijk verwijderen via hun respectievelijke blades.
> 
> 

Meld u aan bij de [Azure-portal](https://portal.azure.com) en klik op **Resourcegroepen**.

Typ de naam van de resourcegroep in het tekstvak **Filteren op naam...**. In de instructies voor dit artikel is een resourcegroep met de naam *SignalRTestResources* gebruikt. Klik in de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.

   
![Verwijderen](./media/signalr-integrate-functions/signalr-delete-resource-group.png)


U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep en klik op **Verwijderen**.
   
Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u Azure Functions kunt integreren om updates te pushen naar clients op basis van triggers van Azure Functions. Voor meer informatie over het gebruik van Azure SignalR Server gaat u verder met de voorbeelden van Azure CLI voor de SignalR-service.

> [!div class="nextstepaction"]
> [Azure CLI-voorbeelden voor SignalR](./signalr-cli-samples.md)



