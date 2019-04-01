---
title: Aangepaste waarschuwingen maken voor Azure Security Center voor IoT-Preview | Microsoft Docs
description: Maken en toewijzen van aangepaste waarschuwingen voor Azure Security Center voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 40bec01b402d7a597a480a738840cba8f388d88b
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755145"
---
# <a name="quickstart-create-custom-alerts"></a>Quickstart: Aangepaste waarschuwingen maken

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Met behulp van aangepaste beveiligingsgroepen en waarschuwingen, kunnen profiteren van de gegevens van de end-to-end beveiliging en de categorische apparaat kennis om te zorgen voor betere beveiliging voor uw IoT-oplossing. 

## <a name="why-use-custom-alerts"></a>Aangepaste waarschuwingen waarom gebruiken? 

Weet u het beste IoT-apparaten.

Voor klanten die hun gedrag van het verwachte apparaat beter te begrijpen, Azure Security Center (ASC) voor IoT, kunt u voor de omzetting van deze overeenkomst in een beleid voor het gedrag van apparaat en een waarschuwing voor een afwijking van verwacht, normale gedrag.

## <a name="security-groups"></a>Beveiligingsgroepen

-Beveiligingsgroepen kunnen u logische groepen van apparaten definiëren en beheren van de beveiligingsstatus in een gecentraliseerde manier.

Deze groepen kunnen apparaten met specifieke hardware, apparaten die zijn geïmplementeerd in een bepaalde locatie of een andere groep die geschikt is voor uw specifieke behoeften vertegenwoordigen.

Beveiligingsgroepen worden gedefinieerd door een security module dubbele eigenschap tag met de naam **Toewijzingsmodule**. Wijzig de waarde van deze eigenschap te wijzigen van de beveiligingsgroep van een apparaat.  

Elke IoT-oplossing op IoT-Hub heeft standaard één beveiligingsgroep met de naam **standaard**.

Beveiligingsgroepen gebruikt om uw apparaten te groeperen in logische categorieën. Na het maken van de groepen, moet u ze aan de aangepaste waarschuwingen van uw keuze, voor de meest efficiënte end-to-end oplossing toewijzen. 

## <a name="customize-an-alert"></a>Een waarschuwing aanpassen

1. Open uw IoT-Hub. 
2. Selecteer **Security**en selecteer vervolgens **aangepaste waarschuwingen**. 
3. Kies de beveiligingsgroepen die u wilt de aanpassing van toepassing. 
4. Klik op **een aangepaste waarschuwing toevoegen**
5. Voer een naam van waarschuwing (Houd er rekening mee dat de namen van de waarschuwing kunnen niet worden gewijzigd na het maken). 
6. Selecteer een gedrag voor aangepaste waarschuwingen in de vervolgkeuzelijst. 
7. De vereiste eigenschappen bewerken, klikt u op **OK**.
8. Zorg ervoor dat u **opslaan**. Zonder op te slaan in de nieuwe waarschuwing, wordt de waarschuwing de volgende keer dat u IoT-Hub sluiten verwijderd.

 
## <a name="alerts-available-for-customization"></a>Waarschuwingen die beschikbaar zijn voor aanpassing

De volgende tabel bevat een samenvatting van waarschuwingen die beschikbaar zijn voor aanpassing.

| Severity | Name                                                                                                    | Gegevensbron | Beschrijving                                                                                                                                     |
|----------|---------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Laag      | Aangepaste waarschuwing - nummer van cloud naar apparaat-berichten in AMQP-protocol is niet in het toegestane bereik          | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (AMQP-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - aantal geweigerde cloud naar apparaat-berichten in AMQP-protocol is niet in het toegestane bereik | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (AMQP-protocol) die zijn geweigerd door het apparaat in een bepaalde periode is niet in de geconfigureerde het toegestane bereik |
| Laag      | Aangepaste waarschuwing - nummer van apparaat naar cloud-berichten in AMQP-protocol is niet in het toegestane bereik          | IoT Hub     | De hoeveelheid van het apparaat naar cloud-berichten (AMQP-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - nummer van de directe methode aanroept valt niet binnen het toegestane bereik                              | IoT Hub     | Het bedrag van de directe methode aanroept in een venster bevindt zich niet in de geconfigureerde het toegestane bereik tijd                                                     |
| Laag      | Aangepaste waarschuwing - nummer van het uploaden van bestanden is niet in het toegestane bereik                                       | IoT Hub     | Het bedrag van het uploaden van bestanden in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                                              |
| Laag      | Aangepaste waarschuwing - nummer van cloud naar apparaat-berichten in de HTTP-protocol is niet in het toegestane bereik          | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (HTTP-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - aantal geweigerde cloud naar apparaat-berichten in de HTTP-protocol is niet in het toegestane bereik | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (HTTP-protocol) die zijn geweigerd door het apparaat in een bepaalde periode is niet in de geconfigureerde het toegestane bereik |
| Laag      | Aangepaste waarschuwing - nummer van apparaat naar cloud-berichten in de HTTP-protocol is niet in het toegestane bereik          | IoT Hub     | De hoeveelheid van het apparaat naar cloud-berichten (HTTP-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - nummer van cloud naar apparaat-berichten in MQTT-protocol is niet in het toegestane bereik          | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (MQTT-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - aantal geweigerde cloud naar apparaat-berichten in MQTT-protocol is niet in het toegestane bereik | IoT Hub     | Het bedrag van cloud naar apparaat-berichten (MQTT-protocol) die zijn geweigerd door het apparaat in een bepaalde periode is niet in de geconfigureerde het toegestane bereik |
| Laag      | Aangepaste waarschuwing - nummer van apparaat naar cloud-berichten in MQTT-protocol is niet in het toegestane bereik          | IoT Hub     | De hoeveelheid van het apparaat naar cloud-berichten (MQTT-protocol) in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                  |
| Laag      | Aangepaste waarschuwing - nummer van de opdracht wachtrij worden zich niet in het toegestane bereik                               | IoT Hub     | Hiermee verwijdert u het bedrag van de wachtrij van de opdracht in een venster bevindt zich niet in de geconfigureerde het toegestane bereik tijd                                                      |
| Laag      | Aangepaste waarschuwing - nummer van de apparaatdubbel werkt bij bevindt zich niet in het toegestane bereik                                       | IoT Hub     | De hoeveelheid apparaatdubbel werkt in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                                              |
| Laag      | Aangepaste waarschuwing - aantal niet-geautoriseerde bewerkingen bevindt zich niet in het toegestane bereik                            | IoT Hub     | De hoeveelheid niet-geautoriseerde bewerkingen in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                                   |
| Laag      | Aangepaste waarschuwing - het aantal actieve verbindingen niet binnen het toegestane bereik                                        | Agent       | Het bedrag van actieve verbindingen in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                                        |
| Laag      | Aangepaste waarschuwing - de uitgaande verbinding met een IP-adres dat is niet toegestaan is gemaakt                              | Agent       | Een uitgaande verbinding met een IP-adres dat niet is toegestaan is gemaakt                                                                                  |
| Laag      | Aangepaste waarschuwing: het aantal mislukte aanmeldingen van de lokale zich niet in het toegestane bereik                                | Agent       | De hoeveelheid mislukte lokale aanmeldingen in een bepaalde periode is niet in de geconfigureerde het toegestane bereik                                                       |
| Laag      | Aangepaste waarschuwing - aanmelding van een gebruiker die is niet toegestaan                                                      | Agent       | Een lokale gebruiker die niet is toegestaan dat is aangemeld bij het apparaat                                                                                        |
| Laag      | Aangepaste waarschuwing - uitvoering van een proces dat niet is toegestaan                                               | Agent       | Een proces dat niet is toegestaan Er is op het apparaat uitgevoerd |          |

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het implementeren van een beveiligingsagent...

> [!div class="nextstepaction"]
> [Een beveiligingsagent implementeren](how-to-deploy-agent.md)
