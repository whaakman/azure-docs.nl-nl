---
title: Instellen van omgeving - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Uw omgeving voorbereiden voor ontwikkeling en implementatie van de modules voor machine learning aan de rand.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f8652dab5db8bbd59982cb1dda727698868df23c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057741"
---
# <a name="tutorial-set-up-an-environment-for-machine-learning-on-iot-edge"></a>Zelfstudie: Instellen van een omgeving voor machine learning op IoT Edge

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

Dit artikel uit de end-to-end Azure Machine Learning op IoT Edge-zelfstudie helpt u uw omgeving voorbereiden voor de ontwikkeling en implementatie. Eerst instellen op een ontwikkelcomputer met alle hulpmiddelen die u nodig hebt. Vervolgens maakt u de benodigde cloudresources in Azure.

## <a name="set-up-a-development-machine"></a>Een ontwikkelcomputer instellen

Deze stap wordt doorgaans uitgevoerd door een cloud-ontwikkelaar. Sommige van de software is mogelijk ook nuttig zijn voor een gegevenswetenschapper.

In de loop van dit artikel uitvoeren we verschillende developer-taken, waaronder coderen, compileren, configureren en implementeren van IoT Edge-modules en IoT-apparaten. Voor het gebruiksgemak gebruikt, hebben we een PowerShell-script dat wordt gemaakt van een virtuele machine van Azure met veel van de vereisten die al geconfigureerd gemaakt. De virtuele machine die we maken moet overweg kan met [geneste virtualisatie](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization), dit is de reden waarom we hebben gekozen voor een machinegrootte DS8V3.

De ontwikkeling VM zal worden ingesteld:

* Windows 10
* [Chocolatey](https://chocolatey.org/)
* [Docker-bureaublad voor Windows](https://www.docker.com/products/docker-desktop)
* [Git voor Windows](https://gitforwindows.org/)
* [GIT Credential Manager voor Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)
* [.NET Core-SDK](https://dotnet.microsoft.com/)
* [Python 3](https://www.python.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0)
* [VS Code-extensies](https://marketplace.visualstudio.com/search?target=VSCode)
  * [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
  * [PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

De ontwikkelaar van de virtuele machine is niet strikt noodzakelijk – de ontwikkelhulpprogramma's kunnen worden uitgevoerd op een lokale computer. Echter, wordt aangeraden met behulp van de virtuele machine om te controleren of een veld niveau afspelen.

Het duurt ongeveer 30 minuten maken en configureren van de virtuele machine.

### <a name="get-the-script"></a>Haal het script

Klonen of downloaden van de PowerShell-script uit de [Machine Learning en IoT Edge](https://github.com/Azure-Samples/IoTEdgeAndMlSample) voorbeeldopslagplaats.

### <a name="create-an-azure-virtual-machine"></a>Een virtuele Azure-machine maken

De map DevVM bevat de bestanden die nodig zijn voor een Azure-machine geschikt is voor het voltooien van deze zelfstudie maakt.

1. Open Powershell als beheerder en navigeer naar de map waar u de code hebt gedownload. Verwijzen we naar de hoofdmap voor de bron als `<srcdir>`.

    ```powershell
    cd \<srcdir\>\IoTEdgeAndMlSample\DevVM
    ```

2. Voer de volgende opdracht om te leiden tot uitvoering van scripts. Kies **Ja op Alles** wanneer hierom wordt gevraagd.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

3. Maken-AzureDevVM.ps1 uitvoeren vanuit deze map.

    ```powershell
    .\Create-AzureDevVm.ps1
    ```

    * Wanneer u hierom wordt gevraagd, moet u de volgende informatie leveren:
      * **Azure-abonnement-ID**: Uw abonnements-ID, die kan worden gevonden in de Azure-portal
      * **Naam resourcegroep**: De naam van een nieuwe of bestaande resourcegroep in Azure
      * **Locatie**: Kies een Azure-locatie waar de virtuele machine wordt gemaakt. Bijvoorbeeld, westus2 of northeurope. Zie voor meer informatie, [Azure-locaties](https://azure.microsoft.com/global-infrastructure/locations/).
      * **AdminUsername**: Geef een gemakkelijk te onthouden naam voor het beheerdersaccount dat u wilt maken en gebruiken op de virtuele machine.
      * **AdminPassword**: Stel een wachtwoord voor de beheerdersaccount op de virtuele machine.

    * Als u geen Azure PowerShell is geïnstalleerd, installeert het script [Az van Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0)

    * U wordt gevraagd zich aan te melden bij Azure.

    * Het script wordt bevestigd dat de gegevens voor het maken van uw virtuele machine. Druk op `y` of `Enter` om door te gaan.

Het script wordt uitgevoerd voor enkele minuten als deze wordt uitgevoerd de volgende stappen uit:

* De resourcegroep te maken als deze niet bestaat
* De virtuele machine implementeren
* Hyper-V op de virtuele machine inschakelen
* Installeren van software nodig voor de ontwikkeling en kloon de voorbeeldopslagplaats
* De virtuele machine opnieuw starten
* Een RDP-bestand op uw bureaublad om verbinding te maken met de virtuele machine maken

### <a name="set-auto-shutdown-schedule"></a>Set-schema voor automatisch afsluiten

Als u uw kosten verlagen, is de virtuele machine gemaakt met een schema voor automatisch afsluiten die is ingesteld op 1900 PST. Mogelijk moet u deze timing afhankelijk van uw locatie en het schema bijwerken. Het afsluiten schema bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Navigeer naar uw virtuele machine in de resourcegroep die u hebt opgegeven in de vorige sectie.

3. Selecteer **Auto-shutdown** in de navigator aan clientzijde.

4. Voer een nieuwe tijd afsluiten in **gepland afsluiten** of wijzig de **tijdzone** klikt u vervolgens op **opslaan**.

### <a name="connect-and-configure-development-machine"></a>Verbinding maken en configureren van de ontwikkelcomputer

Nu dat we een virtuele machine hebt gemaakt, moeten we klaar bent met het installeren van de software die nodig zijn om de zelfstudie te voltooien.

#### <a name="start-a-remote-desktop-session"></a>Start een extern bureaublad-sessiehost

1. Het script voor het maken van virtuele machine gemaakt een RDP-bestand op uw bureaublad.

2. Dubbelklik op het bestand met de naam van de  **\<Azure VM-naam\>RDP**.

3. U krijgt een dialoogvenster waarin wordt gemeld dat de uitgever van de externe verbinding is onbekend. Klik op de **niet opnieuw vragen voor verbindingen op deze computer** selectievakje selecteert **Connect**.

4. Geef desgevraagd de AdminPassword die u hebt gebruikt bij het script is uitgevoerd voor het instellen van de virtuele machine en klik op **OK**.

5. U wordt gevraagd om te accepteren van het certificaat voor de virtuele machine. Selecteer **niet opnieuw vragen voor verbindingen op deze computer** en kies **Ja**.

#### <a name="install-visual-studio-code-extensions"></a>Visual Studio Code-extensies installeren

Nu dat u hebt aangesloten op de ontwikkelcomputer, moet u enkele nuttige extensies toevoegen aan Visual Studio Code om de ontwikkeling eenvoudiger ervaring.

1. Navigeer in een PowerShell-venster naar **C:\\bron\\IoTEdgeAndMlSample\\DevVM**.

2. Toestaan dat scripts worden uitgevoerd op de virtuele machine door te typen.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope CurrentUser -Force
    ```

3. Voer het script uit.

    ```powershell
    .\\Enable-CodeExtensions.ps1
    ```

4. Het script wordt uitgevoerd voor een paar minuten VS code-extensies installeren:

    * Azure IoT Tools
    * Python
    * C#
    * Docker
    * PowerShell

## <a name="set-up-iot-hub-and-storage"></a>Stel IoT-Hub en opslag

Deze stappen worden doorgaans uitgevoerd door een ontwikkelaar van de cloud.

Azure IoT Hub is de kern van een IoT-toepassing. Veilige communicatie tussen IoT-apparaten en de cloud worden verwerkt. Dit is het belangrijkste coördinatie punt voor de werking van de IoT Edge-machine learning-oplossing.

* IoT Hub maakt gebruik van routes om te leiden van binnenkomende gegevens van IoT-apparaten naar andere downstream-services. We zullen profiteren van IoT Hub de routes naar het apparaatgegevens verzenden naar Azure Storage waar deze kan worden gebruikt door Azure Machine Learning te trainen onze resterende nuttige levensduur (RUL)-classificatie.

* Later in de zelfstudie gebruiken we IoT Hub configureren en beheren van onze Azure IoT Edge-apparaat.

In deze sectie maakt u een script gebruiken om een Azure-IoT-hub en een Azure Storage-account te maken. Vervolgens configureert u een route die stuurt gegevens die zijn ontvangen door de hub aan een Azure Storage-Blob-container met behulp van de Azure portal. Deze stappen duren ongeveer tien minuten om uit te voeren.

### <a name="create-cloud-resources"></a>Cloudresources maken

1. Open een PowerShell-venster op uw ontwikkelcomputer.

1. Ga naar de IoTHub-map.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\IoTHub
    ```

1. Voer het script maken. Net als bij het maken van de virtuele ontwikkelmachine, moet u de dezelfde waarden gebruiken voor abonnement-ID, de locatie en resource-groep.

    ```powershell
    .\New-HubAndStorage.ps1 -SubscriptionId <subscription id> -Location
    <location> -ResourceGroupName <resource group>
    ```

    * U wordt gevraagd zich aan te melden bij Azure.
    * Het script wordt bevestigd dat de gegevens voor het maken van uw Hub- en Storage-account. Druk op `y` of `Enter` om door te gaan.

Het script neemt ongeveer twee minuten om uit te voeren. Als u klaar bent, voert het script de naam van de hub en de storage-account.

### <a name="review-route-to-storage-in-iot-hub"></a>Route naar de opslag van IoT-Hub bekijken

Als onderdeel van het maken van de IoT-hub gemaakt het script dat er in de vorige sectie is ook een aangepast eindpunt en een route. IoT Hub-routes bestaan uit een query-expressie en een eindpunt. Als een bericht overeenkomt met de expressie, wordt de gegevens op de route verzonden naar het bijbehorende eindpunt. Eindpunten kunnen worden Event Hubs, Service Bus-wachtrijen en onderwerpen. Het eindpunt is in dit geval een blobcontainer in een storage-account. We gebruiken de Azure-portal om te controleren van de route die zijn gemaakt door het script.

1. Open de [Azure Portal](http://portal.azure.com).

1. Kies alle services uit de linker navigator, IoT typt in het zoekvak en selecteer **IoT-Hub**.

1. Kies dat de IoT-Hub in de vorige stap hebt gemaakt.

1. Kies in de navigator IoT-Hub aan clientzijde **berichtroutering**.

1. Het bericht routering pagina bevat twee tabbladen **Routes** en **aangepaste eindpunten**. Selecteer de **aangepaste eindpunten** tabblad.

1. Onder **Blob storage**, selecteer **turbofanDeviceStorage**.

1. Houd er rekening mee dat dit eindpunt naar een blobcontainer verwijst met de naam **devicedata** in het opslagaccount in de laatste stap hebt gemaakt, die met de naam **iotedgeandml\<uniek achtervoegsel\>** .

1. Let ook op de **blobbestandsnaam** uit de standaardindeling te plaatsen in plaats daarvan de partitie als het laatste element in de naam is gewijzigd. We kunnen deze indeling is voor de bestandsbewerkingen die wij met Azure-laptops later in de zelfstudie doen vinden.

1. Sluit de blade eindpunt om terug te keren naar de **berichtroutering** pagina.

1. Selecteer de **Routes** tabblad.

1. Selecteer de route met de naam **turbofanDeviceDataToStorage**.

1. Merk op dat eindpunt van de route is de **turbofanDeviceStorage** aangepast eindpunt.

1. Bekijk de routering query, die is ingesteld op **waar**. Dit betekent dat alle telemetrieberichten van apparaten, komt overeen met deze route en daarom alle berichten worden verzonden naar de **turbofanDeviceStorage** eindpunt.

1. Sluit de Routedetails van de.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een IoT-Hub gemaakt en een route naar een Azure Storage-account geconfigureerd. In het volgende artikel sturen we gegevens uit een set gesimuleerde apparaten via de IoT Hub in de storage-account. Later in de zelfstudie nadat we hebben onze IoT Edge-apparaat en de modules, geconfigureerd we terug naar routes en iets meer op de routering query.

Zie voor meer informatie over de stappen beschreven in dit gedeelte van de Machine Learning op IoT Edge-zelfstudie:

* [Basisprincipes van Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/)
* [Routering van berichten met IoT Hub configureren](../iot-hub/tutorial-routing.md)
* [Een IoT-hub met behulp van de Azure portal maken](../iot-hub/iot-hub-create-through-portal.md)

Doorgaan met het volgende artikel voor het maken van een gesimuleerd apparaat om te controleren.

> [!div class="nextstepaction"]
> [Genereren van apparaatgegevens](tutorial-machine-learning-edge-03-generate-data.md)
