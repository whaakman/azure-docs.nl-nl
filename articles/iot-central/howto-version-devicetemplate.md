---
title: Informatie over apparaat sjabloon versiebeheer voor apps in uw Azure IoT Central | Microsoft Docs
description: Uw apparaatsjablonen herhalen met het maken van nieuwe versies en zonder enige impact op uw live verbonden apparaten
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/19/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 3b9e6a59b44db9295d86e3bc8a8dda9ec9761f38
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51009205"
---
# <a name="create-a-new-device-template-version"></a>Maak een nieuwe versie van de apparaat-sjabloon

Azure IoT Central kunnen snelle ontwikkeling van IoT-toepassingen. U kunt uw apparaat sjabloonontwerpen snel herhalen door toe te voegen, te bewerken of verwijderen van metingen, instellingen of eigenschappen. Het is mogelijk dat sommige van deze wijzigingen firewallverbinding voor de momenteel verbonden apparaten. Azure IoT Central identificeert deze belangrijke wijzigingen en biedt een manier om het veilig implementeren van deze updates op de apparaten.

Een apparaat-sjabloon is een uniek versienummer op wanneer u deze maakt. Standaard is het versienummer 1.0.0. Als u een apparaat-sjabloon bewerken, en als deze wijziging kan gevolgen hebben voor live verbonden apparaten, Azure IoT Central vraagt u een nieuwe versie van de apparaat-sjabloon wilt maken.

> [!NOTE]
> Voor meer informatie over het maken van een apparaat-sjabloon raadpleegt [een apparaat-sjabloon instellen](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Wijzigingen die een versie vragen wijzigen

Wijzigingen in eigenschappen van de sjabloon voor het apparaat of instellingen gevraagd in het algemeen een wijziging versie.

> [!NOTE]
> Wijzigingen in de sjabloon van het apparaat niet vragen voor het maken van een nieuwe versie wanneer er geen apparaat of een apparaat op de meeste is verbonden.

De volgende lijst beschrijft de acties van de gebruiker waarvoor een nieuwe versie:

* Eigenschappen (vereist)
    * Toevoegen of verwijderen van een vereiste eigenschap
    * Wijzigen van de naam van het veld van een eigenschap, de naam van het veld dat wordt gebruikt door uw apparaten om berichten te verzenden.
*  Eigenschappen (optioneel)
    * Een optionele eigenschap verwijderen
    * Wijzigen van de naam van het veld van een eigenschap, de naam van het veld dat wordt gebruikt door uw apparaten om berichten te verzenden.
    * Een optionele eigenschap wijzigen in een vereiste eigenschap
*  Instellingen
    * Toevoegen of verwijderen van een instelling
    * Wijzigen van de naam van het veld van een instelling voor de naam van het veld dat wordt gebruikt door uw apparaten te verzenden en ontvangen van berichten.

## <a name="what-happens-on-version-change"></a>Wat gebeurt er op versie wijzigen?

Wat gebeurt er met de regels en dashboards apparaat wanneer er een wijziging versie?

**Regels** voorwaarden die afhankelijk van eigenschappen zijn kan bevatten. Als u een of meer van deze eigenschappen hebt verwijderd, kunnen deze regels worden onderverdeeld in de nieuwe versie van de apparaat-sjabloon. U kunt gaat u naar deze specifieke regels en bijwerken van de voorwaarden om op te lossen van de regels. Regels voor de vorige versie, kunnen met geen invloed.

**Apparaat dashboards** kunnen verschillende soorten tegels bevatten. Sommige van de tegels kunnen instellingen en eigenschappen bevatten. Wanneer een eigenschap of de instelling die wordt gebruikt in een tegel wordt verwijderd, wordt de tegel volledig of gedeeltelijk verbroken. U kunt naar de tegel en los het probleem door de tegel verwijderen of bijwerken van de inhoud van de tegel.

## <a name="migrate-a-device-across-device-template-versions"></a>Migreren van een apparaat tussen versies van apparaat

U kunt meerdere versies van de apparaat-sjabloon maken. Na verloop van tijd hebt u meerdere verbonden apparaten met behulp van deze apparaatsjablonen. U kunt apparaten uit één versie van de sjabloon voor het apparaat naar de andere migreren. De volgende stappen wordt beschreven hoe u migreert van een apparaat:

1. Ga naar de **Explorer** pagina.
1. Selecteer het apparaat dat u wilt migreren naar een andere versie.
1. Kies **migreren apparaat**.
1. Selecteer het versienummer dat u wilt migreren van het apparaat en kies **migreren**.

![Het migreren van een apparaat](media\howto-version-devicetemplate\pick-version.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe apparaat sjabloonversies in uw Azure IoT Central-toepassing gebruiken, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Over het maken van regels voor telemetrie](howto-create-telemetry-rules.md)