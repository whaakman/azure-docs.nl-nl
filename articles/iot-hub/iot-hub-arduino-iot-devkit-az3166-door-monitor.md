---
title: Een e-mailbericht ontvangen wanneer de deur wordt geopend met behulp van SendGrid-service en Azure Functions | Microsoft Docs
description: De magnetische sensor om te detecteren wanneer een deur wordt geopend en Azure Functions gebruiken voor het verzenden van een e-mailmelding bewaken.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: d5c92e31850cdac5e05021fd231259b0d3b064d1
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822801"
---
# <a name="door-monitor"></a>De deur van Monitor          

De MXChip IoT DevKit bevat een ingebouwde magnetische sensor. In dit project detecteren u de aanwezigheid of afwezigheid van een sterke magnetische veld in de buurt--in dit geval die afkomstig zijn van een kleine, permanente Magneet.

## <a name="what-you-learn"></a>Wat u leert

In dit project leert u het volgende:
- Het gebruik van de MXChip IoT DevKit magnetische sensor voor het detecteren van het verplaatsen van een in de buurt Magneet.
- Het gebruik van de SendGrid-service een melding verzenden naar uw e-mailadres.

> [!NOTE]
> Voor een praktische gebruik van dit project, moet u de volgende taken uitvoeren:
> - Koppel een Magneet aan de rand van een klep.
> - Koppel de DevKit op het identificatienummer dicht bij de magneet. Openen of sluiten van de deur wordt de sensor, wat resulteert in uw ontvangt een e-mailmelding van de gebeurtenis geactiveerd.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](iot-hub-arduino-iot-devkit-az3166-get-started.md) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u een hebt, kunt u registreren via een van deze methoden:

* Activeer een [gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/).
* Claim uw [Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u een MSDN of Visual Studio-abonnee bent.

## <a name="deploy-the-sendgrid-service-in-azure"></a>De SendGrid-service in Azure implementeren

[SendGrid](https://sendgrid.com/) is een cloud-gebaseerd e delivery-platform. Deze service wordt gebruikt voor het verzenden van e-mailmeldingen.

> [!NOTE]
> Als u al een SendGrid-service hebt geïmplementeerd, kunt u doorgaan rechtstreeks naar [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid-implementatie

Voor het inrichten van Azure-services, gebruikt u de **implementeren in Azure** knop. Deze knop kunt snel en eenvoudig implementeren in uw open-source-projecten met Microsoft Azure.

Klik op de **implementeren in Azure** hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Als u bent niet al aangemeld bij uw Azure-account, nu aanmelden. 

U ziet nu het SendGrid aanmeldingsformulier hebt ingevuld.

![SendGrid-implementatie](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

Voer het aanmeldingsformulier hebt ingevuld:

   * **Resourcegroep**: Maak een resourcegroep voor het hosten van de SendGrid-service of gebruik een bestaande resourcegroep. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](../azure-resource-manager/manage-resource-groups-portal.md).

   * **Naam**: De naam voor uw SendGrid-service. Kies een unieke naam, die afwijkt van andere services die u hebt.

   * **Wachtwoord**: De service is een wachtwoord, die niet worden gebruikt voor alles wat in dit project.

   * **e-mailbericht**: De SendGrid-service wordt verificatie naar dit e-mailadres verzonden.

Controleer de **vastmaken aan dashboard** om deze toepassing makkelijker te vinden in de toekomst optie en klik vervolgens op **aankoop** om in te dienen het aanmeldingsformulier.
 
### <a name="sendgrid-api-key-creation"></a>SendGrid-API-sleutel maken

Nadat de implementatie is voltooid, klikt u op deze en klik vervolgens op de **beheren** knop. Uw SendGrid-account-pagina wordt weergegeven, waar u nodig hebt om te controleren of uw e-mailadres.

![SendGrid beheren](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

Klik op de pagina SendGrid **instellingen** > **API-sleutels** > **API-sleutel maken**.

![SendGrid-API eerst maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

Op de **API-sleutel maken** pagina, voer de **de naam van de API-sleutel** en klikt u op **maken & weergave**.

![SendGrid-API ten tweede maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

Uw API-sleutel wordt slechts één keer weergegeven. Moet u kopiëren en sla deze op veilig, omdat deze wordt gebruikt in de volgende stap.

## <a name="deploy-iot-hub-in-azure"></a>IoT-Hub in Azure implementeren

De volgende stappen uit andere Azure-IoT wordt ingericht gerelateerde services en Azure Functions implementeren voor dit project.

Klik op de **implementeren in Azure** hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Het aanmeldingsformulier hebt ingevuld, wordt weergegeven.

![IoTHub Deployment](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

Vul de velden in het aanmeldingsformulier hebt ingevuld.

   * **Resourcegroep**: Maak een resourcegroep voor het hosten van de SendGrid-service of gebruik een bestaande resourcegroep. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](../azure-resource-manager/manage-resource-groups-portal.md).

   * **De naam van IOT Hub**: De naam voor uw IoT-hub. Kies een unieke naam, die afwijkt van andere services die u hebt.

   * **IOT Hub-Sku**: F1 (beperkt tot één per abonnement) is gratis. U kunt meer informatie over de prijzen zien op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/iot-hub/).

   * **Van e-mailbericht**: Dit veld moet hetzelfde e-mailadres dat u hebt gebruikt bij het instellen van de SendGrid-service.

Controleer de **vastmaken aan dashboard** om deze toepassing makkelijker te vinden in de toekomst optie en klik vervolgens op **aankoop** wanneer u bent klaar om door te gaan met de volgende stap.
 
## <a name="build-and-upload-the-code"></a>Bouwen en de code uploaden

Vervolgens laadt de voorbeeldcode in VS Code en de benodigde Azure-services inrichten.

### <a name="start-vs-code"></a>Start VS Code

- Zorg ervoor dat uw DevKit is **niet** op uw computer is aangesloten.
- Start VS Code.
- De DevKit verbinden met uw computer.

> [!NOTE]
> Als u VS Code starten, krijgt u mogelijk een foutbericht dat aangeeft dat deze het Arduino IDE of gerelateerde Bord pakket kan niet vinden. Als u dit foutbericht ontvangt, sluiten VS Code, starten, opnieuw de Arduino IDE en VS Code moeten Ga naar het pad met Arduino IDE correct.

### <a name="open-arduino-examples-folder"></a>Map openen Arduino-voorbeelden

Vouw aan de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **DoorMonitor**. Deze actie wordt een nieuwe VS Code-venster geopend met een projectmap daarin.

![Mini-solution-voorbeelden](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

U kunt ook de voorbeeld-app openen vanuit het opdrachtenpalet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: Voorbeelden**.

### <a name="provision-azure-services"></a>Azure-services inrichten

Voer in het venster van de oplossing, de cloudinrichting-taak:
- Type `Ctrl+P` (Mac OS: `Cmd+P`).
- Voer `task cloud-provision` in het tekstvak opgegeven.

In de terminal van VS Code en helpt een interactieve vanaf de opdrachtregel u de vereiste Azure-services voor het inrichten. Selecteer alle dezelfde items in de na vragen aan gebruiker lijst die u eerder hebt ingericht in [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

![Cloud inrichten](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Als de pagina reageert niet meer in de status van de geladen bij het aanmelden bij Azure, raadpleegt u de ["page wijzigingen wanneer u zich aanmeldt" sectie van de IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) om dit probleem te verhelpen. 

### <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

Upload vervolgens de code voor het apparaat.

#### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.

2. De terminal vraagt u om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven de DevKit id-nummer en het woord *configuratie*.

#### <a name="macos"></a>macOS

1. De DevKit in configuratiemodus geplaatst: Houd ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven dat 'Configuration'.

2. Klik op `Cmd+P` om uit te voeren `task device-upload`.

#### <a name="verify-upload-and-run-the-sample-app"></a>Verifiëren en uitvoeren van de voorbeeld-app uploaden

De verbindingsreeks die is opgehaald uit de [inrichten Azure services](#provision-azure-services) stap nu is ingesteld. 

VS Code vervolgens begint te controleren en uploaden van de Arduino schetsen naar de DevKit.

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Van tijd tot tijd, ontvangt u mogelijk een ' fout: AZ3166: Onbekend pakket"foutbericht. Deze fout treedt op wanneer de bord package index niet correct wordt vernieuwd. U kunt deze fout oplossen, raadpleegt u de [ontwikkeling-sectie van de IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Het programma wordt eerst geïnitialiseerd wanneer de DevKit met een stabiele magnetische-veld.

Na de initialisatie van, `Door closed` op het scherm wordt weergegeven. Als er een wijziging in het veld magnetische is, de status gewijzigd in `Door opened`. Telkens wanneer de status van de deur verandert, ontvangt u een e-mailmelding. (Deze e-mailberichten kunnen tot vijf minuten om te worden ontvangen duren).

![Magnetische dicht bij de sensor: Klep gesloten](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "magnetische dicht bij de sensor: De deur is gesloten")

![Magneet weg van de sensor is verplaatst: Klep geopend](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Magneet weg van de sensor verplaatst: De deur van geopend")

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of verbinding maken met behulp van de volgende kanalen:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een apparaat DevKit verbinden met uw externe controle van Azure IoT-oplossingsversnellers en de SendGrid-service gebruikt voor het verzenden van een e-mailbericht. Hier volgen de voorgestelde volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
