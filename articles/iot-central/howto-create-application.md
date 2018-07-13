---
title: Een Azure IoT Central-toepassing maken | Microsoft Docs
description: Als een beheerder over het maken van een Azure IoT Central-toepassing.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/09/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 1fbe3ea142e1dd738cd341f57d2b8f48b539ac75
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003763"
---
# <a name="create-your-azure-iot-central-application"></a>Uw Azure IoT Central-toepassing maken

Maken van uw Microsoft Azure IoT Central-toepassing vanuit de [toepassing maken](https://apps.microsoftiotcentral.com/create) pagina. Voor het maken van een Azure IoT Central-toepassing, moet u Vul de velden op deze pagina en kies vervolgens **maken**. U vindt meer informatie over elk van de onderstaande velden heeft.

![Toepassingspagina maken](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Betalingsschema

U kunt een proefversie of een betaalde toepassing maken. Meer informatie over proefabonnementen en betaalde toepassingen op [Azure IoT Central pagina met prijzen](https://azure.microsoft.com/pricing/details/iot-central/)...

## <a name="application-name"></a>Toepassingsnaam

De naam van uw toepassing wordt weergegeven op de **Toepassingsbeheer** pagina en in elke Azure IoT Central-toepassing. U kunt een willekeurige naam voor uw Azure IoT Central-toepassing. Kies een naam die zinvol voor u en anderen in uw organisatie.

## <a name="application-url"></a>Toepassings-URL

De URL van de toepassing is de koppeling naar uw toepassing. U kunt een bladwijzer toe opslaan in uw browser of met anderen delen.

Wanneer u de naam voor uw toepassing invoert, wordt de URL van uw toepassing wordt automatisch gegenereerd. Als u liever, kunt u een andere URL voor uw toepassing. Elke Azure IoT Central-URL moet uniek zijn binnen Azure IoT Central. U ziet een foutbericht weergegeven als de URL die u kiest, wordt al gebruikt.

## <a name="directory"></a>Directory

Alleen in betaalde toepassingen.

Kies een Azure Active Directory-tenant te maken van een Azure IoT Central-toepassing. Een Azure Active Directory-tenant bevat gebruikers-id's, referenties en andere gegevens van de organisatie. Meerdere Azure-abonnementen kunnen worden gekoppeld aan één Azure Active Directory-tenant.

Als u een Azure Active Directory-tenant hebt, is een gemaakt voor u bij het maken van een Azure-abonnement.

Zie voor meer informatie, [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure-abonnement

Een Azure-abonnement kunt u exemplaren maken van Azure-services. Azure IoT Central automatisch alle Azure-abonnementen die u toegang tot hebt vindt, en geeft deze weer in een vervolgkeuzelijst op de **toepassing maken** pagina. Kies een Azure-abonnement te maken van een nieuwe Azure IoT Central-toepassing.

Als u geen Azure-abonnement hebt, kunt u er een maken op de [Azure registratiepagina](https://aka.ms/createazuresubscription). Nadat u het Azure-abonnement maakt, gaat u terug naar de **toepassing maken** pagina. Uw nieuwe abonnement wordt weergegeven in de **Azure-abonnement** vervolgkeuzelijst.

Zie voor meer informatie, [Azure-abonnementen](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Regio

Alleen in betaalde toepassingen.

Kies de regio waar u wilt maken van uw Azure IoT Central-toepassing. Normaal gesproken kiest u de regio die zich het dichtst bij uw apparaten om de optimale prestaties fysiek.

Zie voor meer informatie, [Azure-regio's](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Ziet u de regio's waar Azure IoT Central is beschikbaar op de [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) pagina.

> [!Note]
> Als u een regio hebt gekozen, kunt u uw toepassing naar een andere regio niet later verplaatsen.

## <a name="application-template"></a>De sjabloon voor toepassingen

U kunt een van de beschikbare sjablonen voor uw nieuwe Azure IoT Central-toepassing. Een toepassingssjabloon mag vooraf gedefinieerde items zoals apparaatsjablonen en dashboards om u te helpen aan de slag.

| De sjabloon voor toepassingen | Beschrijving |
| -------------------- | ----------- |
| Aangepaste toepassing   | Hiermee maakt u een lege toepassing voor u om in te vullen met uw eigen apparaatsjablonen en apparaten. |
| Voorbeeld van Contoso       | Hiermee maakt u een toepassing die gebruikmaakt van een apparaat-sjabloon voor een eenvoudige aangesloten apparaat. Gebruik deze sjabloon om te beginnen met het verkennen van Azure IoT Central. |
| Voorbeeld Devkits       | Hiermee maakt een toepassing met apparaatsjablonen kunt u verbinding maken met een MXChip of Raspberry Pi-apparaat. Gebruik deze sjabloon als u een ontwikkelaar van het apparaat te experimenteren met code op een van deze apparaten. |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe een Azure IoT Central-toepassing maken, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)