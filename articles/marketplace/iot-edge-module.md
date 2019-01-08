---
title: Azure IoT Edge-Modules
description: De IoT Edge-Module bieden in de Azure Marketplace voor uitgevers van app- en service.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, IoT Edge module offer
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/22/2018
ms.author: qianw211
ms.openlocfilehash: f13d3f780e24d71babb1bda48a9b85d1208e3c4c
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074594"
---
# <a name="iot-edge-modules"></a>IoT Edge-modules

De [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) platform wordt ondersteund door Azure-Cloud.  Dit platform kan gebruikers om cloudworkloads uit te voeren op IoT-apparaten rechtstreeks te implementeren.  Een IoT Edge-module kunt offline-workloads uitvoeren en analyseren van gegevens lokaal. Dit aanbiedingtype helpt bij het opslaan van bandbreedte, lokale en gevoelige gegevens beveiligen, en biedt een lage latentie reactietijd.  U hebt nu de opties om te profiteren van deze vooraf gemaakte werkbelastingen. Tot nu toe zijn slechts een handvol eigen oplossingen van Microsoft beschikbaar.  Moest u de tijd en resources in uw eigen aangepaste IoT-oplossingen bouwen investeren.

Door de introductie van de [IoT Edge-modules in de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1), we hebben nu één bestemming uitgevers beschikbaar stellen en hun oplossingen verkopen aan de IoT-doelgroep. IoT-ontwikkelaars kunnen uiteindelijk vinden en kopen mogelijkheden om hun oplossing-ontwikkeling te versnellen.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Belangrijkste voordelen van het IoT Edge-modules in Azure Marketplace:

| **Voor uitgevers**    | **Voor klanten (IoT-ontwikkelaars)**  |
| :------------------- | :-------------------|
| Bereik miljoenen ontwikkelaars die willen bouwen en implementeren van IoT Edge-oplossingen.  | Een IoT Edge-oplossing met het vertrouwen van het gebruik van beveiligde en geteste onderdelen opstellen. |
| Eenmaal publiceren en uitvoeren op alle IoT Edge-hardware die ondersteuning biedt voor containers. | Minder tijd op de markt door te zoeken en implementeren van de 1e en 3e partij IoT Edge-modules voor specifieke behoeften. |
| Genereer inkomsten met flexibele factureringsopties <ul> <li> Gratis en Bring Your Own License (BYOL). </li> </ul> | Aankopen met uw eigen keuze aan facturering. <ul> <li> Gratis en Bring Your Own License (BYOL). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>Wat is een IoT Edge-module?

Azure IoT Edge kunt u implementeren en beheren van zakelijke logica aan de rand in de vorm van modules. Azure IoT Edge-modules zijn de kleinste berekening eenheden die worden beheerd door IoT Edge en Microsoft-services (zoals Azure Stream Analytics), 3rd derden services of uw eigen oplossingsspecifieke code kunnen bevatten. Zie voor meer informatie over IoT Edge-modules, [inzicht in Azure IoT Edge-modules](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**Wat is het verschil tussen een type van de aanbieding Container en een IoT Edge module-aanbiedingtype?**

Het type aanbieding van IoT Edge-module is een specifiek type container die wordt uitgevoerd op een IoT Edge-apparaat. Het wordt geleverd met standaardconfiguratie-instellingen om uit te voeren in de context van IoT Edge en (optioneel) maakt gebruik van de IoT Edge-module SDK kan worden geïntegreerd met de IoT Edge-runtime.

## <a name="publishing-your-iot-edge-module"></a>Publiceren van uw IoT Edge-module

**De juiste webwinkel selecteren**

IoT Edge-Modules alleen worden gepubliceerd naar de Azure Marketplace, AppSource is niet van toepassing.  Zie voor meer informatie over de verschillen en de doelgroep voor storefronts [bepalen de publicatieoptie voor uw oplossing](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Factureringsopties voor**

De marketplace ondersteunt momenteel **gratis** en **doen om uw eigen licentie (BYOL)** opties voor IoT Edge-modules voor facturering.
 
**Publicatieopties**

In alle gevallen IoT Edge-modules moeten selecteren de **Transact** optie publiceren.  Zie [Kies een optie voor publiceren](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) voor meer informatie over het publiceren van opties.  

## <a name="eligibility-criteria"></a>Criteria voor in aanmerking komt

De voorwaarden van de Microsoft Azure Marketplace-overeenkomsten en het beleid van toepassing op IoT Edge-module biedt.  Bovendien, vereisten en technische vereisten voor IoT Edge-modules.  

**Vereisten**

Als u wilt een IoT Edge-module in de Azure Marketplace publiceren, moet u voldoen aan de volgende vereisten:

- Toegang tot de Cloud Partner-Portal (CPP). Zie voor meer informatie, [Azure Marketplace en AppSource publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Host uw IoT Edge-module in een Azure Container Registry. 
- Uw IoT Edge module metagegevens die klaar zijn, zoals (niet-uitputtende lijst) hebben: 
    - Een titel
    - Een beschrijving (in HTML-indeling)
    - Een installatiekopie van het logo (PNG-indeling en vaste grootte zoals 40x40px, 90x90px, 115x115px, 255x115px)
    - Een termijn van en het privacybeleid
    - Standaardconfiguratie van module (route, apparaatdubbel-gewenste eigenschappen, createOptions, omgevingsvariabelen)
    - Documentatie
    - Contactpersonen voor ondersteuning

**Technische vereisten**

De primaire technische vereisten voor een IoT Edge-Module, om deze gecertificeerd en gepubliceerd in de Azure Marketplace zijn uiteengezet in de [voorbereiden van uw IoT Edge-module technische zaken](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets).  

## <a name="documentation-and-resources"></a>Documentatie en informatiebronnen

[Maak een aanbieding van IoT Edge module](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-offer) -– de stappen voor het publiceren van een nieuwe IoT Edge-module bieden met de Cloud-Portal voor publiceren.

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan,

- Registreren in de [Microsoft Partner Network](https://partner.microsoft.com/membership).
- Maak een [Microsoft-Account](https://account.microsoft.com/account/) (vereist voor Azure Marketplace biedt transact; aanbevolen voor anderen).
- Dien de [Marketplace registratieformulier](https://azuremarketplace.microsoft.com/sell/signup).

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- Aanmelden bij [Cloud Partner-Portal](https://cloudpartner.azure.com/) maken of uitvoeren van uw aanbieding.
- Zie [IoT Edge module aanbieding publiceren overzicht](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) voor meer informatie over hoe u een IoT Edge-module-aanbieding publiceert.
