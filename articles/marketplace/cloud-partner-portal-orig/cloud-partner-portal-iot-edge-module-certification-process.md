---
title: IoT Edge Module certificering | Microsoft Docs
description: Een IoT Edge-Module voor de Marketplace certificeren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c8056bd4912605a4cd3ee333b1be87d4f3a6d5ba
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198045"
---
# <a name="the-iot-edge-module-certification-process"></a>Het certificeringsproces van IoT Edge-Module

In dit artikel beschrijft de stappen en de vereisten voor het certificeren van een IoT Edge-Module voor het publiceren op Azure Marketplace. 

>[!Note]
>Dit is een tijdelijke document. De IoT Edge Module Marketplace parallel wordt opgebouwd en in dit artikel wordt vervangen door de openbare documentatie.

## <a name="understanding-an-iot-edge-module"></a>Inzicht krijgen in een IoT Edge-module

Module-terminologie:

-   Een **installatiekopie module** is een pakket met de software die een module definieert.

-   Een **module-exemplaar** is de specifieke rekeneenheid waarop u de installatiekopie van de module wordt uitgevoerd op een IoT Edge-apparaat. De module-exemplaar wordt gestart door de IoT Edge-runtime.

Modules kunnen ook de module IoT SDK, die gebruikmaakt van de volgende terminologie:

-   Een **module identiteit** is een stukje informatie (met inbegrip van beveiligingsreferenties) die zijn opgeslagen in IoT-Hub die is gekoppeld aan elk exemplaar van de module.

-   Een **moduledubbel** is een JSON-document dat is opgeslagen in IoT Hub, met informatie over de status voor een module-exemplaar, met inbegrip van metagegevens, configuraties en voorwaarden.

-   **SDK's** worden gebruikt voor het ontwikkelen van aangepaste modules in meerdere talen, zoals: C\#, C, Python, Java en Node.JS.

## <a name="the-onboarding-process-for-an-iot-edge-module"></a>Het onboarding-proces voor een IoT Edge-module

De volgende schermafbeelding ziet u het proces voor een IoT Edge-module worden openbare in de Azure Marketplace.

![Certificeringsproces](./media/cloud-partner-portal-iot-edge-module-certification-process/onboarding-process.png)

### <a name="onboarding-step-details"></a>Details van de onboarding-stappen

-   **Stap 1** -Partners hun aanbiedingen verzenden met het aanbiedingtype **IoT Edge-Module** in het hulpprogramma voor Cloud Partner-Portal door het team van Azure Marketplace. U moet een officiële MS-partner voor toegang tot het hulpprogramma Cloud Partner-Portal. Zie voor meer informatie de [van publisher guide](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   **Stap 2** - antivirusprogramma's in de Marketplace automatisch wordt uitgevoerd en anti-malware is gecontroleerd. De IoT-team kan de opname van aangepaste geautomatiseerde tests tijdens deze fase uitgevoerd.

-   **Stap 3** -de module openbaar is. Wordt deze weergegeven in de marketplace en de container kunt anoniem worden opgehaald uit een URL. Bijvoorbeeld, *marketplace.azurecr.io/module-identifier*.

## <a name="iot-edge-module-certification-criteria"></a>Criteria voor certificering van IoT Edge-module

Hieronder vindt u de belangrijkste vereisten waaraan moeten worden voldaan voor een IoT Edge-module worden gecertificeerd en gepubliceerd in de Azure Marketplace.

>[!Note]
>Deze vereisten kunnen worden gewijzigd. U ontvangt tijdig en kunnen uw containers werken, zodat ze kunnen voldoen aan de bijgewerkte vereisten.

### <a name="technical-requirements"></a>Technische vereisten

**Een IoT Edge-module worden**

-   Alleen compatibel is met docker-containers worden momenteel ondersteund als IoT Edge-modules. De module moet worden uitgevoerd op [Moby](https://mobyproject.org/). 

    >[!Note]
    >Docker-containers werkt waarschijnlijk met Moby omdat Moby de onderliggende open-source-project voor Docker is.

-   De partner moet de volgende standaardinstellingen opgeven: 

    -   Een standaard **tag** (welke mag niet leeg zijn.

    -   Een standaard **createOptions** (welke kan niet leeg zijn.)

    -   Als de SDK voor IoT-Module, een standaard **dubbele** (welke kan niet leeg zijn.)

    -   Als de SDK voor IoT-Module, een standaard **routes** (welke kan niet leeg zijn.)

**Platformondersteuning**

-   Alleen de platforms die zijn **algemeen beschikbaar** in IoT Edge-laag 1 (zoals vastgelegd in [ondersteuning voor Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)) moet worden ondersteund. Bijvoorbeeld, betekent dit momenteel ondersteunen het volgende besturingssysteem en de architectuur combinaties:

    -   Ubuntu-Server 18.04 voor x64

    -   Ubuntu Server 16.04 voor x64

    -   Raspbian-stretch voor arm32 (armhf)

**Apparaat afmetingen**

-   Elk apparaat met de equivalente dimensies (CPU/RAM-geheugen/opslag/GPU/en enz.) van een Raspberry Pi of hoger kan een IoT Edge-apparaat zijn. Als een module alleen in specifieke dimensiebeperkingen werkt, moeten deze beperkingen worden opgegeven in de Modulebeschrijving van de.

**Standaardinstellingen**

-   Een module moet beginnen met de standaardparameters voor elk ondersteund platform (besturingssysteem + architectuur).

-   Configuratie-instellingen moet duidelijk gedocumenteerd (tags, omgevingsvariabelen, dubbele, routes.) Als onderdeel van de aanbieding, kunnen partners een beschrijving voor de Module die ondersteuning biedt voor eenvoudige HTML-opmaak of verwijst naar een webpagina van de externe definiëren.

**Versiebeheer**

-   Klanten moeten kunnen kiezen of ze willen automatisch bijwerken van een module die afkomstig zijn van de marketplace of als ze willen een exacte versie gebruiken ze hebt getest. Marketplace-modules moeten volgen hetzelfde patroon als het IoT Edge-runtime versiebeheer. Bijvoorbeeld:

    -   Rolling labels, zoals "1.0", kunnen worden bijgewerkt.

    -   Secundaire versie van tags, zoals '1.3.24' kunnen niet worden bijgewerkt.

**Telemetry**

-   Modules met behulp van de IoT-SDK-Module moeten de id van het unieke module toegewezen door de marketplace voor telemetrie-toepassing instellen. Hierdoor kunnen de Azure Marketplace om te identificeren van het aantal uitgevoerde module-exemplaren die worden uitgevoerd. Deze unieke id is de naam van het toegewezen door de marketplace van opname. De methoden van de volgende SDK's gebruiken om in te stellen van deze id:
    - [C\#](https://hub.docker.com/_/mysql/)
    - [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

-   Minder nauwkeurig inzichten zijn voor Modules die niet van de IoT-SDK-Module gebruikmaken is beschikbaar via de Cloud Partner-Portal. Bijvoorbeeld, het aantal downloads.

**Beveiliging**

-   Containers moeten de minste bevoegdheden toegang tot de host mogelijk hebben. "Beschermde" containers moeten worden zelden.

-   Partners hun module moeten blijven beveiligen als onderdeel van de ondersteuning die ze bieden.

**Updates**

-   Partners moeten hun module up-to-date en functionele houden. Deze krijgt een bericht van tevoren als er belangrijke wijzigingen voor IoT Edge-Runtime gepland zijn.

**Module IoT-SDK**

-   Met inbegrip van de IoT-SDK-Module is niet een vereiste voor certificering.
    Met inbegrip van de IoT-SDK-Module kan evenwel een betere gebruikerservaring. Als u bijvoorbeeld ondersteuning voor routering, verzenden van berichten naar de Cloud, enzovoort. De IoT-SDK-Module is vereist om telemetrie te verkrijgen voor het aantal module-exemplaren die worden uitgevoerd.

**Subjectief vereiste**

-   Moet voldoen aan ten minste één praktijk IoT Edge-use-case. Bijvoorbeeld, mag niet een WordPress-container worden uitgevoerd, tenzij een klant wil deze gebruikt in IoT Edge.

**Juridische vereisten (van AMP beleid)**

-   De module moet voldoen aan de Microsoft Azure Marketplace-overeenkomsten en het beleid. Zie voor meer informatie de gekoppelde overeenkomst voor uitgevers en [deelname beleid](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

-   Naast de Azure Marketplace-overeenkomsten en het beleid, kunnen er aanvullende juridische vereisten specifiek is voor een IoT Edge-Module-aanbiedingtype. Dit is op dit moment wordt gecontroleerd.

-   De module moet een *gebruiksvoorwaarden* en een *privacybeleid*.

-   De module moet ook een derde partij kennisgeving als de module gebruikmaakt van derden.

**De vereisten van het ondersteuningsteam (van AMP beleid)**

-   Partners zijn verantwoordelijk voor de ondersteuning van hun IoT Edge-Modules. Ze zorgt ervoor dat de ondersteuningsopties die zijn opgegeven in de beschrijving van de IoT Edge-Module beschikbaar blijven en dat ten minste één ondersteuningsoptie altijd beschikbaar is. (Zie sectie 4 van de overeenkomst voor uitgevers van AMP voor meer informatie.)

**Categorisatie**

-   Alle IoT Edge-modules worden weergegeven onder de categorie **Internet van dingen \>IoT Edge-module**. Het is aan de partner om te kiezen van de beste subcategorie voor hun product.
