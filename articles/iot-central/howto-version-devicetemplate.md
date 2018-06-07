---
title: Informatie over apparaat sjabloon versiebeheer voor uw Azure IoT centrale apps | Microsoft Docs
description: Uw apparaat sjablonen doorlopen door te maken van nieuwe versies en zonder enige impact op uw live verbonden apparaten
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/19/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: b125d822596675b138560c14c76f9a3120ce3424
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628824"
---
# <a name="create-a-new-device-template-version"></a>Maak een nieuwe versie van de apparaat-sjabloon

Microsoft Azure IoT Central kunt snel ontwikkelen van IoT-toepassingen. U kunt snel sequentieel via uw apparaat sjabloonontwerpen toe te voegen, bewerken of verwijderen van metingen, instellingen of eigenschappen. Sommige van deze wijzigingen mogelijk Tussenkomende voor de momenteel verbonden apparaten. Azure IoT centraal identificeert deze grote wijzigingen en biedt een manier voor het implementeren van deze updates veilig naar de apparaten.

Een sjabloon van het apparaat heeft een versienummer als u dit hebt gemaakt. Standaard is het versienummer 1.0.0. Als u een apparaat-sjabloon bewerken, en als deze wijziging kan invloed hebben op actieve verbonden apparaten, Azure IoT centrale vraagt u een nieuwe versie van de apparaat-sjabloon maken.

> [!NOTE]
> Voor meer informatie Zie informatie over het maken van een sjabloon apparaat [een sjabloon van het apparaat instellen](howto-set-up-template.md)

## <a name="changes-that-prompt-a-version-change"></a>Wijzigingen die een versie vragen wijzigen

Wijzigingen in de eigenschappen van de sjabloon voor het apparaat of instellingen gevraagd in het algemeen een wijziging versie.

> [!NOTE]
> Wijzigingen in de sjabloon van het apparaat niet vragen voor het maken van een nieuwe versie wanneer er geen apparaat of een apparaat op de meeste is verbonden.

De volgende lijst beschrijft de acties van de gebruiker waarvoor een nieuwe versie:

* Eigenschappen (vereist)
    * Toevoegen of verwijderen van een vereiste eigenschap
    * Wijzigen van de naam van een eigenschap, veldnaam die door uw apparaten wordt gebruikt om berichten te verzenden.
*  Eigenschappen (optioneel)
    * Een optionele eigenschap verwijderen
    * Wijzigen van de naam van een eigenschap, veldnaam die door uw apparaten wordt gebruikt om berichten te verzenden.
    * Een optionele eigenschap wijzigen in een vereiste eigenschap
*  Instellingen
    * Toevoegen of verwijderen van een instelling
    * Wijzigen van de naam van een instelling voor de naam van het veld dat wordt gebruikt door uw apparaten verzenden en ontvangen van berichten.

## <a name="what-happens-on-version-change"></a>Wat gebeurt er op de versie wijzigen?

Wat gebeurt er met de regels en -dashboards voor apparaat wanneer er een wijziging versie?

**Regels** voorwaarden die afhankelijk van eigenschappen zijn kan bevatten. Als u een of meer van deze eigenschappen hebt verwijderd, kunnen deze regels worden onderverdeeld in de nieuwe versie van de apparaat-sjabloon. U kunt gaat u naar deze specifieke regels en bijwerken van de voorwaarden om op te lossen de regels. Regels voor de vorige versie moeten werken zonder impact.

**Apparaat dashboards** kunnen verschillende typen tegels bevatten. Sommige van de tegels mogelijk instellingen en eigenschappen bevatten. Wanneer een eigenschap of de instelling die wordt gebruikt in een tegel wordt verwijderd, is de tegel volledig of gedeeltelijk verbroken. U kunt gaat u naar de tegel en los het probleem door de tegel verwijderen of bijwerken van de inhoud van de tegel.

## <a name="migrate-a-device-across-device-template-versions"></a>Een apparaat via apparaat sjabloonversies migreren

U kunt meerdere versies van de sjabloon van het apparaat kunt maken. Na verloop van tijd hebt u meerdere verbonden apparaten met behulp van deze sjablonen van het apparaat. U kunt apparaten van één versie van de sjabloon voor het apparaat naar de andere migreren. De volgende stappen beschrijven het migreren van een apparaat:

1. Ga naar de **Explorer** pagina.
1. Selecteer het apparaat dat u wilt migreren naar een andere versie.
1. Kies **apparaat migreren**.
1. Selecteer het versienummer dat u wilt migreren van het apparaat en kies **migreren**.

![Het migreren van een apparaat](media\howto-version-devicetemplate\pick-version.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe sjabloonversies apparaat gebruiken in uw Azure IoT centrale toepassing, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Telemetrie-regels maken](howto-create-telemetry-rules.md)