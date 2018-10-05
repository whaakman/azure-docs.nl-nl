---
title: IoT Edge Module Marketplace Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over IoT Edge Module Marketplace.
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
ms.openlocfilehash: 2c2e7729eb70a5dd37dc4df10605eec9429e1043
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809768"
---
# <a name="iot-edge-module-marketplace-frequently-asked-questions"></a>Veelgestelde vragen over IoT Edge Module Marketplace


## <a name="what-is-the-edge-module-marketplace"></a>Wat is de Edge-Module-Marketplace?


De Marketplace met IoT Edge-Module is een lijst met *gecertificeerde* vooraf gemaakte Edge-modules die zijn *detecteerbaar* via Azure Marketplace.

![IoT Edge-modules](./media/cloud-partner-portal-iot-edge-module-faq/iot-edge-modules.png)

## <a name="where-will-edge-modules-be-visible"></a>Waar wordt Edge-modules worden weergegeven? 


In de [Azure portal marketplace](https://ms.portal.azure.com/) (geverifieerd ervaring), onder de categorie Internet of Things gemarkeerd als "IoT Edge-Module".

En in de [Azure marketplace-website](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1) (anonieme experience) onder de categorie Internet of Things gemarkeerd als "IoT Edge-Module".

## <a name="is-it-open-to-partners"></a>Deze toegankelijk is voor partners?


Nog niet. Op dit moment worden alleen Microsoft ontworpen modules gepubliceerd in de IoT Edge-sectie van de marketplace. 

## <a name="why-should-partners-publish-their-iot-edge-modules"></a>Waarom moeten partners hun IoT Edge-modules publiceren?


-   De productzichtbaarheid verbeteren door dit kanaal toe te voegen op de markt brengen en hun oplossingen die laat zien.

-   Meer leads ophalen. Als onderdeel van de Azure Marketplace, kunnen krijgen meer informatie over wie er geïnteresseerd in de oplossing.

-   Om te worden van de eersten die gebruikmaken van meer verdiensten mogelijkheden.

## <a name="what-is-the-onboarding-process"></a>Wat is het onboarding-proces?


Terwijl niet nog is geopend, wordt het onboarding-proces wordt uitgevoerd via Azure Marketplace. Gedetailleerde richtlijnen zijn de [Azure Marketplace en AppSource publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). Raadpleeg de transact type aanbieding voor Containers. 

Partners moet eerst een uitgever voor de Azure Marketplace zijn geworden. En ze willen vrijgeven zijn hun Edge-modules via de [Cloud Partner-Portal](./cloud-partner-portal-getting-started-with-the-cloud-partner-portal.md).

## <a name="are-there-any-monetization-capabilities"></a>Zijn er mogelijkheden verdiensten?


Niet buiten het vak voor nu. Ons eerste doel is het openen van een marketplace met *gratis* of *hun eigen licentie mee* Edge-modules. Zullen hierna we meer verdiensten mogelijkheden, zoals een factureringsmodel verbruik toevoegen.

## <a name="what-is-bring-your-own-license-byol"></a>Wat is er bring-your-own-license (BYOL)?


De officiële definitie van de [Deelnamebeleid voor Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) is:

- *Klanten verkrijgen buiten de Azure Marketplace het recht voor toegang tot of gebruik van de aanbieding en betalen geen Azure Marketplace-kosten voor het gebruik van de aanbieding op Azure Marketplace.*

Het is aan partners licentie hun software en omzet voor het verzamelen.

## <a name="can-partners-publish-a-freemium-module"></a>Kunnen partners een module 'freemium' publiceren?


Ja freemium-modules, die gratis modules die beschikbaar zijn, maar met enkele beperkingen worden beschouwd als een andere publicatie.

## <a name="is-intellectual-property-protected"></a>Is het intellectuele eigendom beveiligd?


Een Edge-module is een compatibele Docker-container. Het is aan partners hun intellectuele eigendom (IP) zich in de gedistribueerde containers worden beschermd.

## <a name="where-will-the-modules-be-hosted"></a>Waar wordt de modules worden gehost?


IoT Edge-modules worden gehost in een containerregister eigendom van Microsoft die anoniem query-kan, zoals Docker Hub.

## <a name="what-are-the-integration-plans-between-the-azure-marketplace-and-the-azure-iot-tools"></a>Wat zijn de plannen van de integratie tussen de Azure Marketplace en de hulpprogramma's voor Azure IoT?

Bouwen we betere integratie tussen de Azure Marketplace en de hulpprogramma's voor Azure IoT. Voorbeelden van wat we in gedachten hebt zijn om in te schakelen sommige zoeken in de IoT Edge module marketplace rechtstreeks vanuit de portal voor IoT-Hub of rechtstreeks vanuit Visual Studio Code.

## <a name="which-operating-system-or-architecture-should-my-container-support"></a>Welke besturingssystemen of architectuur moet mijn container dan ondersteunen?

IoT Edge-modules moeten ondersteuning bieden voor het besturingssysteem dezelfde / architectuur matrix als IoT Edge in de productieomgeving. Deze lijst wordt bijgehouden in [ondersteuning voor Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support). Een module moet bijvoorbeeld momenteel ondersteuning voor Linux x64- en Linux ARM32.

## <a name="are-there-any-other-certification-constraints-to-be-aware-of"></a>Zijn er andere beperkingen certificering rekening mee moet houden?

We werken nog steeds aan het exacte aantal certificering beperkingen. Onze principes zijn:

-   Bevordering van de kwaliteit inbegrepen hoeveelheid.

-   IoT Edge-specifieke containers (geen willekeurige waarden).

-   Productie-kwaliteit.

-   1-klikken-implementatie (ten minste met een reeks standaard configuratie waarden opgegeven).

## <a name="any-other-questions"></a>Andere vragen?


Neem contact op met [Azure IoT Edge-bepalen](mailto:azureiotedgeonboarding@service.microsoft.com).
