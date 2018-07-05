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
ms.openlocfilehash: 25cb3ba53c663a642f0871becbfbcab39d521c67
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437712"
---
# <a name="door-monitor"></a>De deur van Monitor          

De MXChip IoT DevKit bevat een ingebouwde magnetische sensor. In dit project detecteren u de aanwezigheid of afwezigheid van een sterke magnetische veld in de buurt--in dit geval die afkomstig zijn van een klein. Permanente magneet.

## <a name="what-you-learn"></a>Wat u leert

In dit project leert u het volgende:
- Het gebruik van de MXChip IoT DevKit magnetische sensor voor het detecteren van het verplaatsen van een in de buurt Magneet.
- Het gebruik van de SendGrid-service een melding verzenden naar uw e-mailadres.

> [!NOTE]
> Voor een praktische gebruik van dit project:
> - Koppel een Magneet aan de rand van een klep.
> - Koppel de DevKit op het identificatienummer dicht bij de magneet. Openen of sluiten van de deur wordt de sensor, wat resulteert in uw ontvangt een e-mailmelding van de gebeurtenis geactiveerd.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](iot-hub-arduino-iot-devkit-az3166-get-started.md) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u een hebt, kunt u registreren via een van deze methoden:

* Activeer een [gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/).
* Claim uw [Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u een MSDN of Visual Studio-abonnee bent.

## <a name="deploy-sendgrid-service-in-azure"></a>SendGrid-service in Azure implementeren

[SendGrid](https://sendgrid.com/) is een cloud-gebaseerd e delivery-platform. Deze service wordt gebruikt voor het verzenden van e-mailmeldingen.

> [!NOTE]
> Als u al een SendGrid-service hebt geïmplementeerd, kunt u doorgaan rechtstreeks naar [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid-implementatie

Voor het inrichten van Azure-services, gebruikt u de **implementeren in Azure** knop. Deze knop kunt snel en eenvoudig implementeren in uw open-source-projecten met Microsoft Azure.

Klik op de **implementeren in Azure** knop hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Vervolgens ziet u de volgende pagina.

> [!NOTE]
> Als u de volgende pagina niet ziet, moet u mogelijk eerst aanmelden bij uw Azure-account.

Voer het aanmeldingsformulier hebt ingevuld:

  * **Resourcegroep**: Maak een resourcegroep voor het hosten van de SendGrid-service of gebruik een bestaande resourcegroep. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **Naam**: de naam voor uw SendGrid-service. Kies een unieke naam, die afwijkt van andere services die u hebt.

  * **Wachtwoord**: een wachtwoord, dat zich niet voor alles wat in dit project is vereist voor de service.

  * **E-mailbericht**: de SendGrid-service kan de verificatie wordt verzonden naar dit e-mailadres.

  > [!NOTE]
  > Controleer de **vastmaken aan dashboard** optie om deze toepassing makkelijker te vinden in de toekomst.
 
![SendGrid-implementatie](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

### <a name="sendgrid-api-key-creation"></a>SendGrid-API-sleutel maken

Nadat de implementatie is voltooid, klikt u op deze en klik vervolgens op de **beheren** knop. U gaat naar de pagina met SendGrid en nodig hebt om te controleren of uw e-mailadres.

![SendGrid beheren](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

Klik op de pagina SendGrid **instellingen** > **API-sleutels** > **API-sleutel maken**. Invoer van de **de naam van de API-sleutel** en klikt u op **maken & weergave**.

![SendGrid-API eerst maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

![SendGrid-API ten tweede maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

Uw API-sleutel wordt slechts één keer weergegeven. Moet u kopiëren en sla deze op veilig, omdat deze wordt gebruikt in de volgende stap.

## <a name="deploy-iot-hub-in-azure"></a>IoT-Hub in Azure implementeren

De volgende stappen uit andere Azure-IoT wordt ingericht gerelateerde services en Azure Functions implementeren voor dit project.

Klik op de **implementeren in Azure** knop hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Vervolgens ziet u de volgende pagina.

> [!NOTE]
> Als u de volgende pagina niet ziet, moet u mogelijk eerst aanmelden bij uw Azure-account.

Voer het aanmeldingsformulier hebt ingevuld:

  * **Resourcegroep**: Maak een resourcegroep voor het hosten van de SendGrid-service of gebruik een bestaande resourcegroep. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **De naam van de IOT-Hub**: de naam van uw IoT-hub. Kies een unieke naam, die afwijkt van andere services die u hebt.

  * **IOT Hub Sku**: F1 (beperkte één per abonnement) is gratis. Ziet u meer informatie over de prijzen bij [prijs- en schaalniveau](https://azure.microsoft.com/pricing/details/iot-hub/).

  * **Van e-mailbericht**: dit moet hetzelfde e-mailadres dat u hebt gebruikt bij het instellen van de SendGrid-service.

  > [!NOTE]
  > Controleer de **vastmaken aan dashboard** optie om deze toepassing makkelijker te vinden in de toekomst.
 
![IoTHub-implementatie](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

## <a name="build-and-upload-the-code"></a>Bouwen en de code uploaden

### <a name="start-vs-code"></a>Start VS Code

- Zorg ervoor dat uw DevKit is **niet** op uw computer is aangesloten.
- Start VS Code.
- De DevKit verbinden met uw computer.

> [!NOTE]
> Als u VS Code starten, krijgt u mogelijk een foutbericht dat aangeeft dat deze het Arduino IDE of gerelateerde Bord pakket kan niet vinden. Als u dit foutbericht ontvangt, sluiten VS Code, starten, opnieuw de Arduino IDE en VS Code moeten Ga naar het pad met Arduino IDE correct.

### <a name="open-arduino-examples-folder"></a>Map openen Arduino-voorbeelden

Vouw aan de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **DoorMonitor**. Deze actie wordt een nieuwe VS Code-venster geopend met een projectmap daarin.

![Mini-solution-voorbeelden](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

> [!NOTE]
> U kunt voorbeeld ook openen vanuit het opdrachtenpalet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

### <a name="provision-azure-services"></a>Azure-services inrichten

Voer in het venster van de oplossing, de cloudinrichting-taak:
- Type `Ctrl+P` (Mac OS: `Cmd+P`).
- Voer `task cloud-provision` in het tekstvak opgegeven.

In de terminal van VS Code en helpt een interactieve vanaf de opdrachtregel u de vereiste Azure-services voor het inrichten. Selecteer alle dezelfde items in de na vragen aan gebruiker lijst die u eerder hebt ingericht in [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

![Cloud inrichten](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Als de pagina reageert niet meer in de status van de geladen bij het aanmelden bij Azure, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) om dit probleem te verhelpen. 

### <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

#### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.
2. De terminal vraagt u om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven de DevKit id-nummer en het woord *configuratie*.

Deze procedure stelt u de verbindingsreeks die is opgehaald uit de [inrichten Azure services](#provision-azure-services) stap.

VS Code vervolgens begint te controleren en uploaden van de Arduino schetsen naar de DevKit:

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Van tijd tot tijd, ontvangt u mogelijk een ' fout: AZ3166: onbekend pakket "foutbericht. Deze fout treedt op wanneer de bord package index niet correct wordt vernieuwd. U kunt deze fout oplossen, verwijzen naar dit [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

#### <a name="macos"></a>macOS

1. De DevKit in configuratiemodus geplaatst: houd knop A, en vervolgens push- en release de knop opnieuw instellen. Het scherm wordt weergegeven dat 'Configuration'.
2. Gebruik `Cmd+P` om uit te voeren `task device-upload`.

Deze procedure stelt u de verbindingsreeks die is opgehaald uit de [inrichten Azure services](#provision-azure-services) stap.

VS Code vervolgens begint te controleren en uploaden van de Arduino schetsen naar de DevKit:

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> Van tijd tot tijd, ontvangt u mogelijk een ' fout: AZ3166: onbekend pakket "foutbericht. Deze fout treedt op wanneer de bord package index niet correct wordt vernieuwd. U kunt deze fout oplossen, verwijzen naar dit [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Het programma wordt eerst geïnitialiseerd wanneer de DevKit met een stabiele magnetische-veld.

Na de initialisatie van, `Door closed` op het scherm wordt weergegeven. Als er een wijziging in het veld magnetische is, de status gewijzigd in `Door opened`. Telkens wanneer de status van de deur verandert, ontvangt u een e-mailmelding. (Deze e-mailberichten kunnen tot vijf minuten om te worden ontvangen duren).

![Magnetische dicht bij de sensor: vergrendeld](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "magnetische dicht bij de sensor: vergrendeld")

![Magneet verplaatst van de sensor: deur geopend](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Magneet verplaatst van de sensor: deur geopend")

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of verbinding maken met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een apparaat DevKit verbinden met uw externe controle van Azure IoT-oplossingsversnellers en de SendGrid-service gebruiken om een e-mail te verzenden. Hier volgen de voorgestelde volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
