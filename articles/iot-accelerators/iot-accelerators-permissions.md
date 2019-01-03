---
title: Gebruik de Azure IoT-oplossingen-site - Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u de AzureIoTSolutions.com-website met de oplossingsversnellers implementeren.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dobett
ms.openlocfilehash: 87f6b9cef50e4b8c388be835b2aa7bed8177ac4b
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601080"
---
# <a name="use-the-azureiotsolutionscom-site-to-deploy-your-solution-accelerator"></a>Gebruik de site azureiotsolutions.com aan uw oplossingsversnellers implementeren

U kunt Azure IoT-oplossingsversnellers implementeren voor uw Azure-abonnement van [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators). AzureIoTSolutions.com als host fungeert voor de Microsoft open source- en partner Oplossingsverbeteringen. Deze Oplossingsverbeteringen zijn afgestemd op de [Azure IoT Reference Architecture](https://aka.ms/iotrefarchitecture). De site kunt u snel een oplossingsverbetering als een demo's of productie-omgeving te implementeren.

![AzureIoTSolutions.com](media/iot-accelerators-permissions/iotsolutionscom.png)

> [!TIP]
> Als u meer controle over het implementatieproces nodig hebt, kunt u de [CLI voor het implementeren van een oplossingsversnellers](iot-accelerators-remote-monitoring-deploy-cli.md).

U kunt de oplossingsversnellers in de volgende configuraties implementeren:

* **Standard**: Een uitgebreide Infrastructuurimplementatie voor het ontwikkelen van een productie-omgeving. De Azure Container Service implementeert de microservices in diverse virtuele Azure-machines. Kubernetes deelt de Docker-containers in die de afzonderlijke microservices hosten.
* **Basic**: Een lagere kosten-versie voor een demonstratie of om een implementatie te testen. Alle microservices worden geïmplementeerd op een enkele virtuele Azure-machine.
* **Lokale**: Een implementatie van de lokale computer voor het testen en ontwikkeling. Deze benadering implementeert de microservices naar een lokale Docker-container en verbinding maakt met de IoT Hub, Azure Cosmos DB en Azure storage-services in de cloud.

Elk van de oplossingsversnellers maakt gebruik van een andere combinatie van Azure-services zoals IoT Hub, Azure Stream Analytics en Cosmos DB. Ga voor meer informatie naar [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators) en selecteert u een oplossingsverbetering.

## <a name="sign-in-at-azureiotsolutionscom"></a>Meld u aan bij azureiotsolutions.com

Voordat u een oplossingsverbetering implementeren kunt, moet u zich aanmelden bij AzureIoTSolutions.com met behulp van referenties die zijn gekoppeld aan een Azure-abonnement. Als uw account is gekoppeld aan meer dan één tenant van Microsoft Azure Active Directory (AD), kunt u de **Account selectie vervolgkeuzelijst** kiezen van de map te gebruiken.

Uw machtigingen oplossingsversnellers implementeren, het beheren van gebruikers en het beheren van Azure-services, is afhankelijk van uw rol in de geselecteerde map. Algemene Azure AD-rollen die zijn gekoppeld aan de oplossingsversnellers zijn onder andere:

* **Globale beheerder**: Er kunnen zich veel [globale beheerders](../active-directory/users-groups-roles/directory-assign-admin-roles.md) per Azure AD-tenant:

  * Wanneer u een Azure AD-tenant maakt, bent u standaard de globale beheerder van deze tenant.
  * De globale beheerder kunt basic en standard-oplossingsversnellers implementeren.

* **Domeingebruiker**: Er zijn veel domeingebruikers per Azure AD-tenant. Een domeingebruiker kan een eenvoudige oplossingsversnellers implementeren.

* **Gastgebruiker**: Er zijn veel gastgebruikers ook kunnen per Azure AD-tenant. Gastgebruikers ook kunnen implementeren niet van een oplossingsversnellers in de Azure AD-tenant.

Zie de volgende bronnen voor meer informatie over gebruikers en rollen in Azure AD:

* [Gebruikers maken in Azure Active Directory](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
* [Gebruikers toewijzen aan apps](../active-directory/manage-apps/assign-user-or-group-access-portal.md)

## <a name="choose-your-device"></a>Kies uw apparaat

De site AzureIoTSolutions.com is gekoppeld aan de [Azure Certified voor IoT-apparaatcatalogus](https://catalog.azureiotsolutions.com/).

De catalogus bevat honderden certified IoT-apparaten die kunt u verbinding met uw Oplossingsverbeteringen om te beginnen het bouwen van uw IoT-oplossing.

![Apparaatcatalogus](media/iot-accelerators-permissions/devicecatalog.png)

Als u een hardwarefabrikant bent, klikt u op **een partner** voor meer informatie over de samenwerking met Microsoft op de Certified for IoT-programma.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de quickstarts om een van de oplossingsverbeteringen uit te proberen:

* [Een oplossing voor externe controle uitproberen](quickstart-remote-monitoring-deploy.md)
* [Een oplossing voor verbonden factory uitproberen](quickstart-connected-factory-deploy.md)
* [Een oplossing voor predictief onderhoud uitproberen](quickstart-predictive-maintenance-deploy.md)
* [Een oplossing voor apparaatsimulatie uitproberen](quickstart-device-simulation-deploy.md)
