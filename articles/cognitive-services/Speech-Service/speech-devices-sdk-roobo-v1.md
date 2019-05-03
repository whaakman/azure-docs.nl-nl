---
title: Spraak apparaten SDK Roobo Smart Audio Dev Kit v1 - spraakservices
titleSuffix: Azure Cognitive Services
description: Vereisten en instructies voor het aan de slag met de SDK van de apparaten spraak, Roobo slimme Audio Dev Kit v1.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 0e5dc73c1f24ccbc2032cecbb857587eb20c6806
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026206"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Apparaat: Roobo Smart Audio Dev Kit

Dit artikel bevat specifieke informatie voor de Roobo slimme Audio Dev Kit apparaat.

## <a name="set-up-the-development-kit"></a>Instellen van de development kit

1. De development kit heeft twee micro USB-connectors. De linker-connector is om de development kit en is gemarkeerd als Power in de onderstaande afbeelding. Het recht een is om dit te beheren, en is gemarkeerd als fouten opsporen in de afbeelding.

    ![de dev kit verbinding te maken](media/speech-devices-sdk/qsg-1.png)

1. De development kit voor energiebeheer met behulp van een micro USB-kabel op de power-poort verbinding te maken met een PC of het samenstellen van de adapter. Een indicator groen power licht op onder de bovenste kaart.

1. Voor het beheren van de development kit, verbinding maken met de poort voor foutopsporing op een computer met behulp van een tweede micro USB-kabel. Het is essentieel voor een hoge kwaliteit-kabel gebruiken om ervoor te zorgen betrouwbare communicatie.

1. Uw development kit voor de configuratie van cirkelvormige of lineaire plaatsen.

    |Development kit configuratie|Afdrukstand|
    |-----------------------------|------------|
    |Kringverwijzing|Rechtop, met microfoons geconfronteerd met het maximum|
    |Lineair|Aan de zijde met microfoons gericht u (weergegeven in de volgende afbeelding)|

    ![lineaire dev kit afdrukstand](media/speech-devices-sdk/qsg-2.png)

1. Installeren van de certificaten en stel de machtigingen van het apparaat. Typ de volgende opdrachten in een opdrachtpromptvenster:

   ```powershell
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Deze opdrachten gebruiken de brug Android foutopsporing `adb.exe`, deze maakt deel uit van de Android Studio-installatie. Dit hulpprogramma bevindt zich in C:\Users\[gebruikersnaam] \AppData\Local\Android\Sdk\platform-hulpprogramma's. U kunt deze map toevoegen aan het pad naar het eenvoudig om aan te roepen kunnen `adb`. Anders moet u het volledige pad naar uw installatie van adb.exe in elke opdracht die wordt aangeroepen `adb`.
    >
    > Als er een fout `no devices/emulators found` controleert u de USB-kabel is aangesloten en een hoge kwaliteit-kabel. U kunt `adb devices` om te controleren dat de computer communiceren met de development kit, kan zoals een lijst met apparaten wordt geretourneerd.
    >
    > [!TIP]
    > De microfoon van uw PC en de spreker om ervoor te zorgen dat u werkt met de development kit microfoons dempen. Op deze manier wordt niet u per ongeluk het apparaat met audio van de PC activeren.

1. Als u wilt een spreker koppelen aan de dev kit, kunt u het verbinding maken met de lijn-audio uit. U moet een goede kwaliteit spreker met een analoge plug 3,5 mm.

    ![Vysor audio](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Informatie over ontwikkelingen

Zie voor meer informatie over ontwikkelingen, de [Roobo development guide](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio"></a>Audio

Roobo voorziet in een hulpprogramma waarmee alle audio Flash-geheugen wordt vastgelegd. Het kan helpen bij het oplossen van problemen met audio. Een versie van het hulpprogramma is opgegeven voor de configuratie van elke development kit. Op de [Roobo site](http://ddk.roobo.com/), selecteer uw apparaat en selecteer vervolgens de **Roobo extra** koppelen aan de onderkant van de pagina.

## <a name="next-steps"></a>Volgende stappen

* [De Android-voorbeeld-app uitvoeren](speech-devices-sdk-android-quickstart.md)
