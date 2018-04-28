---
title: Een e-mailbericht ontvangen wanneer de deur wordt geopend met de SendGrid-service en Azure Functions | Microsoft Docs
description: De magnetische sensor om te detecteren wanneer een deur wordt geopend en Azure Functions gebruiken voor het verzenden van een e-mailmelding bewaken.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: ed0718f2d9d5093442001005e43551e7e9749eef
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="door-monitor"></a>Klep van Monitor          

De MXChip IoT DevKit bevat een ingebouwde magnetische sensor. In dit project detecteren u de aanwezigheid of afwezigheid van een sterke magnetische veld in de buurt--in dit geval afkomstig is van een klein. Permanente magneet.

## <a name="what-you-learn"></a>Wat u leert

In dit project leert u de:
- Het gebruik van de MXChip IoT-DevKit magnetische sensor voor het detecteren van de verplaatsing van een in de buurt Magneet.
- Klik hier voor meer informatie over het gebruik van de SendGrid-service een melding te verzenden naar uw e-mailadres.

> [!NOTE]
> Voor een praktische gebruik van dit project:
> - Koppel een magneet naar de rand van een deur.
> - Koppel de DevKit op het identificatienummer dicht bij de magneet. Openen of sluiten van de deur wordt de sensor, wat resulteert in uw ontvangt een e-mailbericht van de gebeurtenis geactiveerd.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}) naar:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u geen abonnement hebt, kunt u via een van de volgende manieren registreren:

* Activeren van een [gratis 30-daagse evaluatieversie Microsoft Azure-account](https://azure.microsoft.com/free/).
* Claim uw [Azure-tegoeden](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u een MSDN- of Visual Studio-abonnee bent.

## <a name="deploy-sendgrid-service-in-azure"></a>SendGrid-service in Azure implementeren

[SendGrid](https://sendgrid.com/) is een platform voor cloud-gebaseerd e-levering. Deze service wordt gebruikt voor het verzenden van e-mailmeldingen.

> [!NOTE]
> Als u een SendGrid-service al hebben geïmplementeerd, u rechtstreeks naar kan doorgaan [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>SendGrid-implementatie

Voor het inrichten van Azure-services gebruiken de **implementeren in Azure** knop. Deze knop kunt snel en eenvoudig implementatie van de open source-projecten naar Microsoft Azure.

Klik op de **implementeren in Azure** knop hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Vervolgens ziet u de volgende pagina.

> [!NOTE]
> Als u de volgende pagina niet ziet, moet u wellicht eerst aanmelden bij uw Azure-account.

Vul het aanmeldingsformulier:

  * **Resourcegroep**: maken van een resourcegroep voor het hosten van de SendGrid-service, of gebruik een bestaande. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **Naam**: de naam voor uw SendGrid-service. Kies een unieke naam, verschilt van andere services die u hebt.

  * **Wachtwoord**: de service vereist een wachtwoord, niet voor alles in dit project worden.

  * **E-mailadres**: de SendGrid-service kan de verificatie wordt verzonden naar dit e-mailadres.

  > [!NOTE]
  > Controleer de **vastmaken aan dashboard** optie om deze toepassing gemakkelijker te vinden in de toekomst.
 
![SendGrid-implementatie](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

### <a name="sendgrid-api-key-creation"></a>SendGrid-API-sleutel is gemaakt

Nadat de implementatie is geslaagd, klikt u op het en klik vervolgens op de **beheren** knop. U gaat naar uw SendGrid-pagina en moet controleren of uw e-mailadres.

![SendGrid beheren](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

Klik op de pagina SendGrid **instellingen** > **API-sleutels** > **API-sleutel maken**. Invoer de **API sleutelnaam** en klik op **maken & weergave**.

![SendGrid eerst API maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

![SendGrid tweede API maken](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

Uw API-sleutel wordt slechts één keer weergegeven. Zorg ervoor dat kopiëren en bewaar deze veilig, omdat deze wordt gebruikt in de volgende stap.

## <a name="deploy-iot-hub-in-azure"></a>IoT-Hub in Azure implementeren

De volgende stappen uit andere Azure-IoT inricht gerelateerde services en implementeren van Azure Functions voor dit project.

Klik op de **implementeren in Azure** knop hieronder. 

[![Implementeren in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Vervolgens ziet u de volgende pagina.

> [!NOTE]
> Als u de volgende pagina niet ziet, moet u wellicht eerst aanmelden bij uw Azure-account.

Vul het aanmeldingsformulier:

  * **Resourcegroep**: maken van een resourcegroep voor het hosten van de SendGrid-service, of gebruik een bestaande. Zie [resourcegroepen gebruiken voor het beheren van uw Azure-resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

  * **De naam van de IOT-Hub**: de naam voor uw IoT-hub. Kies een unieke naam, verschilt van andere services die u hebt.

  * **IOT Hub Sku**: F1 (beperkte één per abonnement) is gratis. U kunt meer informatie over de prijzen op zien [prijzen en schaalniveau](https://azure.microsoft.com/pricing/details/iot-hub/).

  * **Via e-mailbericht**: dit moet hetzelfde e-mailadres dat u hebt gebruikt bij het instellen van de SendGrid-service.

  > [!NOTE]
  > Controleer de **vastmaken aan dashboard** optie om deze toepassing gemakkelijker te vinden in de toekomst.
 
![IoTHub-implementatie](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

## <a name="build-and-upload-the-code"></a>Bouwen en de code uploaden

### <a name="start-vs-code"></a>VS Code starten

- Zorg ervoor dat uw DevKit **niet** aangesloten op uw computer.
- Start VS-Code.
- De DevKit aansluiten op uw computer.

> [!NOTE]
> Wanneer u Code tegenover start, kunt u mogelijk een foutbericht dat aangeeft dat geen met de Arduino IDE of het gerelateerde mededelingenbord pakket gevonden. Als u dit foutbericht ontvangt, moeten sluiten tegenover Code, de Arduino IDE opnieuw starten en VS-Code Zoek het pad Arduino IDE correct.

### <a name="open-arduino-examples-folder"></a>Voorbeelden van Arduino-map openen

Vouw de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **DoorMonitor**. Deze actie wordt een nieuw venster van de Code van de VS met een projectmap erin.

![Mini solution-voorbeelden](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

> [!NOTE]
> U kunt bijvoorbeeld ook openen vanuit opdracht palet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

### <a name="provision-azure-services"></a>Azure-services inrichten

Voer in het oplossingsvenster de inrichting van de taak cloud:
- Type `Ctrl+P` (Mac OS: `Cmd+P`).
- Voer `task cloud-provision` in het tekstvak opgegeven.

In de terminal VS-Code in doorloopt een interactieve opdrachtregel u de vereiste Azure-services inrichten. Selecteer alle dezelfde items in de vraag lijst die u eerder hebt ingericht in [IoT-Hub implementeren in Azure](#deploy-iot-hub-in-azure).

![Cloud inrichten](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Als de pagina reageert niet meer in de status geladen wanneer u probeert aan te melden bij Azure, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) om dit probleem te verhelpen. 

### <a name="build-and-upload-the-device-code"></a>Maken en uploaden van de apparaatcode

#### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.
2. De terminal vraagt u om de configuratie activeren. Om dit te doen, houdt u A, en vervolgens push en release van de knop herstellen. Het scherm wordt weergegeven het identificatienummer DevKit en het woord *configuratie*.

Deze procedure stelt u de verbindingsreeks die is opgehaald uit de [inrichten Azure-services](#provision-azure-services) stap.

VS-Code is gestart, controleren en de Arduino uploaden werk op de DevKit:

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> In sommige gevallen krijgt u een ' fout: AZ3166: onbekende pakket ' foutbericht. Deze fout treedt op wanneer de mededelingenbord package index niet correct worden vernieuwd. U lost deze fout, neem deze [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

#### <a name="macos"></a>macOS

1. De DevKit in configuratiemodus geplaatst: houd knop A, en vervolgens push en release van de knop herstellen. Het scherm weergegeven 'Configuration'.
2. Gebruik `Cmd+P` om uit te voeren `task device-upload`.

Deze procedure stelt u de verbindingsreeks die is opgehaald uit de [inrichten Azure-services](#provision-azure-services) stap.

VS-Code is gestart, controleren en de Arduino uploaden werk op de DevKit:

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE]
> In sommige gevallen krijgt u een ' fout: AZ3166: onbekende pakket ' foutbericht. Deze fout treedt op wanneer de mededelingenbord package index niet correct worden vernieuwd. U lost deze fout, neem deze [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Het programma eerst initialiseert wanneer de DevKit in aanwezigheid van een stabiele magnetische-veld.

Na de initialisatie van, `Door closed` op het scherm wordt weergegeven. Wanneer er een wijziging in het veld magnetische is, wordt de status verandert in `Door opened`. Wanneer de status van de deur verandert, ontvangt u een e-mailbericht. (Deze e-mailberichten kunnen maximaal vijf minuten om te worden ontvangen duren).

![Magnetische dicht bij de sensor: deur gesloten](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "magnetische dicht bij de sensor: deur is gesloten")

![Magneet verwijderd van de sensor: deur geopend](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Magneet verwijderd van de sensor: deur geopend")

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of verbinding maken met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een DevKit apparaat aansluit op uw Azure IoT Suite en de SendGrid-service gebruiken om een e-mail te verzenden. Hier volgen de mogelijke volgende stappen:

* [Overzicht van Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Sluit een apparaat MXChip IoT DevKit aan uw toepassing Microsoft IoT centrale](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
