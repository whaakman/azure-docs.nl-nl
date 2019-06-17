---
title: Realtime gegevensvisualisatie van sensorgegevens uit Azure IoT hub in een web-app | Microsoft Docs
description: Een web-App gebruiken om temperatuur en vochtigheid gegevens die worden verzameld van een sensor en verzonden naar uw Iot-hub te visualiseren.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/31/2019
ms.author: robinsh
ms.openlocfilehash: 22b15a95e529d4f09560e9b7e59d9f78f70651bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475995"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-in-a-web-application"></a>Visualiseren van realtime-sensorgegevens uit Azure IoT hub in een webtoepassing

![Diagram voor end-to-end](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

In deze zelfstudie leert u hoe u voor het visualiseren van realtime-sensorgegevens die uw IoT-hub ontvangt met een node.js-web-app op uw lokale computer. Na de web-app lokaal uitvoert, kunt u eventueel stappen voor het hosten van de web-app in Azure App Service. Als u wilt proberen om te visualiseren van de gegevens in uw IoT-hub met behulp van Power BI, Zie [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Wat u allemaal doen

* Een consumentengroep toevoegen aan uw IoT-hub die de web-App gebruiken voor het lezen van sensorgegevens
* De code van de web-app downloaden vanuit GitHub
* Bekijk de code van de web-app
* Omgevingsvariabelen voor het opslaan van de IoT-Hub artefacten die nodig zijn voor uw web-app configureren
* De web-app op uw ontwikkelcomputer uitvoeren
* Open een webpagina om te zien van realtime temperatuur en vochtigheid gegevens van uw IoT-hub
* (Optioneel) Azure CLI gebruiken voor het hosten van uw web-app in Azure App Service

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) zelfstudie of een van de apparaat-zelfstudies, bijvoorbeeld [Raspberry Pi met node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Deze betrekking hebben op de volgende vereisten:

  * Een actief Azure-abonnement
  * Een Iot-hub in uw abonnement
  * Een clienttoepassing waarmee berichten worden verzonden naar uw Iot hub

* [Git downloaden](https://www.git-scm.com/downloads)

* De stappen in dit artikel wordt ervan uitgegaan dat een Windows-ontwikkelcomputer; u kunt echter eenvoudig deze stappen uitvoeren op een Linux-systeem in uw favoriete shell.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Voer de volgende opdracht om toe te voegen van de Microsoft Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie worden IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS) specifieke opdrachten toegevoegd aan Azure CLI.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

## <a name="add-a-consumer-group-to-your-iot-hub"></a>Een consumentengroep toevoegen aan uw IoT-hub

[Consumentengroepen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) bieden onafhankelijke weergaven in de gebeurtenisstroom waarmee apps en gegevens uit hetzelfde Event Hub-eindpunt onafhankelijk van elkaar te gebruiken Azure-services. In deze sectie maakt toevoegen u een consumergroep aan uw IoT-hub ingebouwd eindpunt dat door de web-app wordt gebruikt om gegevens uit te lezen.

Voer de volgende opdracht om een consumentengroep toevoegen aan het ingebouwde eindpunt van uw IoT-hub:

```azurecli-interactive
az iot hub consumer-group create --hub-name YourIoTHubName --name YourConsumerGroupName
```

Houd er rekening mee in de naam die u kiest, moet u deze later in deze zelfstudie.

## <a name="get-a-service-connection-string-for-your-iot-hub"></a>Een service-verbindingsreeks ophalen voor uw IoT-hub

IoT-hubs worden gemaakt met verschillende beleidsregels van de standaard toegang. Een dergelijk beleid is de **service** beleid waarmee u voldoende machtigingen voor een service te lezen en schrijven van de IoT hub-eindpunten. Voer de volgende opdracht om een verbindingsreeks voor uw IoT-hub die aan de service-beleid voldoet:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIotHub --policy-name service
```

De verbindingsreeks moet er ongeveer als volgt uitzien:

```javascript
"HostName={YourIotHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"
```

Noteer de serviceverbindingsreeks, u hebt deze verderop in deze zelfstudie nodig.

## <a name="download-the-web-app-from-github"></a>De web-app downloaden vanuit GitHub

Open een opdrachtvenster en voer de volgende opdrachten om te downloaden van het voorbeeld uit GitHub en Ga naar de map voorbeeld:

```cmd
git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
cd web-apps-node-iot-hub-data-visualization
```

## <a name="examine-the-web-app-code"></a>Bekijk de code van de web-app

Open in de web-apps-node-iot-hub-data-visualization map en de web-app in uw favoriete editor. Hieronder ziet u de structuur van het bestand weergegeven in de VS Code:

![Bestandsstructuur voor web-app](./media/iot-hub-live-data-visualization-in-web-apps/web-app-files.png)

Neem even de tijd om te controleren van de volgende bestanden:

* **Server.js** is een service-side-script dat de socket web en de Event Hub-wrapper-klasse initialiseert. Het biedt een callback naar de Event Hub-wrapperklasse waarmee de klasse wordt gebruikt voor het uitzenden van binnenkomende berichten naar de web-socket.

* **Gebeurtenis-hub-reader.js** is een service-script dat is verbonden met de IoT-hub ingebouwd eindpunt met behulp van de groep van de opgegeven verbinding tekenreeks en consumenten. Het haalt de apparaat-id en EnqueuedTimeUtc uit de metagegevens op binnenkomende berichten en stuurt vervolgens het bericht met de callback-methode door server.js geregistreerd.

* **Grafiek-apparaat-data.js** is een client-side-script dat luistert op het web socket, houdt van elk apparaat-id en de laatste 50 punten van binnenkomende gegevens voor elk apparaat worden opgeslagen. Vervolgens wordt het geselecteerde apparaatgegevens naar het grafiekobject.

* **Index.HTML** werkt met de indeling van de gebruikersinterface voor de webpagina wordt weergegeven, en verwijst naar de vereiste scripts voor client-side-logica.

## <a name="configure-environment-variables-for-the-web-app"></a>Omgevingsvariabelen voor de web-app configureren

Als u wilt lezen van gegevens van uw IoT-hub, moet de web-app van uw IoT hub-verbindingsreeks en de naam van de consumentengroep dat deze via moet lezen. Deze tekenreeksen krijgt van de omgeving in de volgende regels in server.js:

```javascript
const iotHubConnectionString = process.env.IotHubConnectionString;
const eventHubConsumerGroup = process.env.EventHubConsumerGroup;
```

Stel de omgevingsvariabelen in het opdrachtvenster met de volgende opdrachten. Vervang de tijdelijke aanduiding door de serviceverbindingsreeks voor uw IoT-hub en de naam van de consumergroep die u eerder hebt gemaakt. Geen offerte de tekenreeksen.

```cmd
set IotHubConnectionString=YourIoTHubConnectionString
set EventHubConsumerGroup=YourConsumerGroupName
```

## <a name="run-the-web-app"></a>De web-app uitvoeren

1. Zorg ervoor dat uw apparaat wordt uitgevoerd en die gegevens verzenden.

2. Voer de volgende regels om te downloaden en installeren van pakketten waarnaar wordt verwezen, en start u de website in het opdrachtvenster:

   ```cmd
   npm install
   npm start
   ```

3. U ziet uitvoer in de console die aangeeft dat de web-app is met uw IoT-hub verbonden en op poort 3000 luistert:

   ![Web-app gestart op console](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-start.png)

## <a name="open-a-web-page-to-see-data-from-your-iot-hub"></a>Open een webpagina om gegevens uit uw IoT-hub te bekijken

Open een browser naar `http://localhost:3000`.

In de **selecteert u een apparaat** , selecteert u uw apparaat om te zien van een actieve diagram van het laatste 50 temperatuur en vochtigheid gegevenspunten die door het apparaat verzonden naar uw IoT-hub.

![Web-app-pagina met realtime temperatuur en vochtigheid](./media/iot-hub-live-data-visualization-in-web-apps/web-page-output.png)

Ook ziet u uitvoer in de console die toont de berichten die uw web-app wordt uitgezonden naar de browserclient:  

![Web-app broadcast uitvoer op console](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-broadcast.png)

## <a name="host-the-web-app-in-app-service"></a>Host voor de web-app in App Service

De [functie van Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) biedt een platform as a service (PAAS) voor het hosten van webtoepassingen. Webtoepassingen die worden gehost in Azure App Service kunnen profiteren van krachtige functies van Azure, zoals extra beveiliging, taakverdeling en schaalbaarheid als goed als Azure en partner DevOps-oplossingen, zoals continue implementatie, Pakketbeheer, enzovoort. Azure App Service biedt ondersteuning voor webtoepassingen in vele populaire talen ontwikkeld en geïmplementeerd op Windows of Linux-infrastructuur.

In deze sectie maakt u een web-app in App Service inrichten en uw code implementeren met behulp van Azure CLI-opdrachten. U vindt meer informatie van de opdrachten de [az webapp](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest) documentatie. Zorg ervoor dat u de stappen hebt voltooid voordat u begint, [een resourcegroep toevoegen aan uw IoT-hub](#add-a-consumer-group-to-your-iot-hub), [een service-verbindingsreeks ophalen voor uw IoT-hub](#get-a-service-connection-string-for-your-iot-hub), en [downloaden van de web-app vanuit GitHub](#download-the-web-app-from-github).

1. Een [App Service-plan](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) definieert een reeks rekenresources voor een app die wordt gehost in App Service om uit te voeren. In deze zelfstudie gebruiken we de ontwikkelaar/gratis-laag voor het hosten van de web-app. Met de gratis laag, wordt uw web-app uitgevoerd op gedeelde bronnen voor Windows met andere App Service-apps, met inbegrip van apps van andere klanten. Azure ook biedt die App Service-plannen om te implementeren web App on Linux compute-resources. U kunt deze stap overslaan als u al een App Service-plan dat u wilt gebruiken.

   Voer de volgende opdracht voor het maken van een App Service-plan met behulp van de gratis laag van Windows. Uw IoT-hub zich in dezelfde resourcegroep gebruiken. De naam van uw service-plan mag hoofdletters en kleine letters, cijfers en afbreekstreepjes bevatten.

   ```azurecli-interactive
   az appservice plan create --name <app service plan name> --resource-group <your resource group name> --sku FREE
   ```

2. Nu een web-app inrichten in uw App Service-plan. De `--deployment-local-git` parameter kunt u de code van de web-app worden geüpload en geïmplementeerd vanuit een Git-opslagplaats op uw lokale computer. De naam van uw web-app moet globaal uniek zijn en mag hoofdletters en kleine letters, cijfers en afbreekstreepjes bevatten.

   ```azurecli-interactive
   az webapp create -n <your web app name> -g <your resource group name> -p <your app service plan name> --deployment-local-git
   ```

3. Voeg nu toepassingsinstellingen toe voor de omgevingsvariabelen die de IoT hub-verbindingsreeks en de Event hub-consumentengroep opgeven. Afzonderlijke instellingen worden door spaties gescheiden de `-settings` parameter. Gebruik de serviceverbindingsreeks voor uw IoT-hub en de consumergroep die u eerder hebt gemaakt in deze zelfstudie. De waarden niet worden citeren.

   ```azurecli-interactive
   az webapp config appsettings set -n <your web app name> -g <your resource group name> --settings EventHubConsumerGroup=<your consumer group> IotHubConnectionString=<your IoT hub connection string>
   ```

4. Schakel de Websockets-protocol voor de web-app en stel de web-app voor het ontvangen HTTPS-aanvragen alleen (HTTP-aanvragen worden omgeleid naar HTTPS).

   ```azurecli-interactive
   az webapp config set -n <your web app name> -g <your resource group name> --web-sockets-enabled true
   az webapp update -n <your web app name> -g <your resource group name> --https-only true
   ```

5. Als u wilt de code implementeren in App Service, kunt u uw [op gebruikersniveau implementatiereferenties](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials). De referenties voor implementatie op gebruikersniveau verschillen van uw Azure-referenties en worden gebruikt voor lokale Git en FTP-implementaties voor een web-app. Nadat deze is ingesteld, zijn ze geldig in al uw App Service-apps in alle abonnementen in uw Azure-account. Als u referenties voor implementatie op gebruikersniveau eerder hebt ingesteld, kunt u ze kunt gebruiken.

   Als u nog niet eerder implementatiereferenties op gebruikersniveau ingesteld of u uw wachtwoord kan niet meer weet, voert u de volgende opdracht uit. De gebruikersnaam voor uw implementatie moet uniek zijn binnen Azure en mag niet de ' @' symbool voor lokale Git-pushes. Wanneer u wordt gevraagd, typ en Bevestig uw nieuwe wachtwoord. Het wachtwoord moet ten minste acht tekens lang zijn, met twee van de volgende drie elementen: letters, cijfers en symbolen.

   ```azurecli-interactive
   az webapp deployment user set --user-name <your deployment user name>
   ```

6. De Git-URL gebruiken om uw code tot App Service ophalen.

   ```azurecli-interactive
   az webapp deployment source config-local-git -n <your web app name> -g <your resource group name>
   ```

7. Een externe toevoegen aan uw kloon die verwijst naar de Git-opslagplaats voor de web-app in App Service. Voor \<Git-kloon-URL\>, gebruikt u de URL die is geretourneerd in de vorige stap. Voer de volgende opdracht in uw opdrachtvenster.

   ```cmd
   git remote add webapp <Git clone URL>
   ```

8. Als u wilt de code implementeren in App Service, voer de volgende opdracht in uw opdrachtvenster. Als u wordt gevraagd om referenties, voert u de beveiliging op gebruikersniveau implementatiereferenties die u hebt gemaakt in stap 5. Zorg ervoor dat u naar de hoofdvertakking van de externe App Service-instantie pushen.

    ```cmd
    git push webapp master:master
    ```

9. De voortgang van de implementatie wordt bijgewerkt in uw opdrachtvenster. Een geslaagde implementatie eindigt met regels die vergelijkbaar is met de volgende uitvoer:

    ```cmd
    remote:
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://contoso-web-app-3.scm.azurewebsites.net/contoso-web-app-3.git
    6b132dd..7cbc994  master -> master
    ```

10. Voer de volgende opdracht om de status van uw web-app en zorg ervoor dat deze wordt uitgevoerd:

    ```azurecli-interactive
    az webapp show -n <your web app name> -g <your resource group name> --query state
    ```

11. Ga naar `https://<your web app name>.azurewebsites.net` in een browser. Een webpagina die lijkt op de website die u hebt gezien tijdens het uitvoeren van de web-app lokaal weergegeven. Ervan uitgaande dat het apparaat wordt uitgevoerd en die gegevens verzenden, ziet u een actieve diagram van de 50 meest recente temperatuur en vochtigheid metingen verzonden door het apparaat.

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen ondervindt met dit voorbeeld tegenkomt, probeert u de stappen in de volgende secties. Als u nog steeds problemen ondervindt, stuur ons feedback onder aan dit onderwerp.

### <a name="client-issues"></a>Problemen met clients

* Als een apparaat niet in de lijst weergegeven wordt, of geen grafiek wordt getekend, zorg er dan voor dat de apparaatcode wordt uitgevoerd op uw apparaat.

* Open de ontwikkelhulpprogramma's in de browser (in veel browsers het F12 sleutel wordt deze geopend), en zoek de-console. Zoek naar eventuele waarschuwingen of fouten er afgedrukt.

* U kunt fouten opsporen in /js/chat-device-data.js client-side-script.

### <a name="local-website-issues"></a>Problemen met lokale website

* Bekijk de uitvoer in het venster waaruit u knooppunt voor console-uitvoer hebt gestart.

* Fouten opsporen in de code, speciaal server.js en /scripts/event-hub-reader.js.

### <a name="azure-app-service-issues"></a>Problemen met Azure App Service

* Ga naar uw web-app in Azure-portal. Onder **bewaking** selecteren in het linkerdeelvenster **App Service-logboeken**. Schakel **toepassingslogboeken (bestandssysteem)** om in te stellen, **niveau** fout, en selecteer vervolgens **opslaan**. Open vervolgens **logboekstream** (onder **bewaking**).

* Van uw web-app in Azure-portal onder **ontwikkeltools** Selecteer **Console** en valideren van knooppunt- en npm-versies met `node -v` en `npm -v`.

* Als u een fout over het vinden van een pakket niet ziet, is het mogelijk dat u de niet-geordende stappen hebt uitgevoerd. Wanneer de site is geïmplementeerd (met `git push`) de service wordt uitgevoerd van de app `npm install`, waaronder wordt uitgevoerd op basis van de huidige versie van het knooppunt is geconfigureerd. Als die in configuratie later wordt gewijzigd, moet u een betekenisloze wijziging aanbrengt in de code en push-opnieuw.

## <a name="next-steps"></a>Volgende stappen

U hebt uw web-app is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw IoT-hub.

Zie voor een andere manier om gegevens van Azure IoT Hub te visualiseren, [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit IoT hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
