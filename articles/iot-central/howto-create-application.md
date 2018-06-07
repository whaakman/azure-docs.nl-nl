---
title: Maken van een Azure IoT centrale toepassing | Microsoft Docs
description: Als een beheerder, het maken van een Azure IoT centrale toepassing.
author: tbhagwat3
ms.author: tanmayb
ms.date: 03/20/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 39992a1cc36d00a64ee6430cad5f24af3e1e1157
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629283"
---
# <a name="create-your-azure-iot-central-application"></a>Uw Azure IoT centraal-toepassing maken

Maken van uw Microsoft Azure IoT centrale toepassing van de [toepassing maken](https://apps.microsoftiotcentral.com/create) pagina. Een Azure IoT centrale toepassing maken, moet u Vul de velden op deze pagina en kies vervolgens **maken**. In dit artikel bevat meer informatie over elk van de velden.

![Toepassingspagina maken](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Betaling plannen

U kunt een proefabonnement of een betaald toepassing maken. Meer informatie over proefabonnementen en betaalde toepassingen op deze pagina.

## <a name="application-name"></a>Toepassingsnaam

De naam van uw toepassing wordt weergegeven op de **Toepassingsbeheer** pagina en in elke Azure IoT centrale toepassing. U kunt een naam voor uw Azure IoT centrale toepassing. Kies een naam die zinvol voor u en anderen in uw organisatie.

## <a name="application-url"></a>Toepassings-URL

De URL van de toepassing is de koppeling naar uw toepassing. U kunt een bladwijzer aan opslaan in uw browser of met anderen delen.

Wanneer u de naam voor uw toepassing invoert, wordt de URL van uw toepassing is automatisch gegenereerd. Als u liever, kunt u een andere URL voor uw toepassing. Elke centrale Azure IoT-URL moet uniek zijn. U ziet een foutbericht weergegeven als de URL die u kiest is al in gebruik.

## <a name="directory"></a>Directory

Alleen in betaalde toepassingen.

Kies een Azure Active Directory-tenant maken van een Azure IoT centrale toepassing. Een Azure Active Directory-tenant bevat de gebruikers-id's, referenties en andere organisatiegegevens. Meerdere Azure-abonnementen kunnen worden gekoppeld aan één Azure Active Directory-tenant.

Als u een Azure Active Directory-tenant hebt, is een gemaakt voor u wanneer u een Azure-abonnement maakt.

Zie voor meer informatie, [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure-abonnement

Een Azure-abonnement kunt u exemplaren van Azure-services maken. Azure IoT centraal automatisch zoeken naar alle Azure-abonnementen die u toegang tot hebt en weergegeven in een vervolgkeuzelijst op de **toepassing maken** pagina. Kies een Azure-abonnement maken van een nieuwe Azure IoT centrale toepassing.

Als u geen Azure-abonnement hebt, kunt u een op deze pagina kunt maken. Nadat u het Azure-abonnement hebt gemaakt, gaat u terug naar de **toepassing maken** pagina. Het nieuwe abonnement wordt weergegeven in de **Azure-abonnement** vervolgkeuzelijst.

Zie voor meer informatie, [Azure-abonnementen](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Regio

Alleen in betaalde toepassingen.

Kies de regio waarin u wilt uw centrale Azure IoT-toepassing maken. Normaal gesproken moet u de regio die het dichtst fysiek op uw apparaten om optimale prestaties te behalen.

Zie voor meer informatie, [Azure-regio's](https://docs.microsoft.com/en-us/azure/guides/developer/azure-developer-guide#azure-regions).

U ziet de regio's waarin Azure IoT centrale beschikbaar is op de [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/) pagina.

> [!Note]
> Wanneer u een regio kiest, kunt u uw toepassing in andere regio niet later verplaatsen.

## <a name="application-template"></a>Sjabloon voor toepassingen

U kunt een van de beschikbare toepassingsaanvragen sjablonen voor uw nieuwe Azure IoT centrale toepassing. Een toepassingssjabloon vooraf gedefinieerde items zoals apparaat sjablonen kan bevatten en dashboards om u te helpen aan de slag:

| Sjabloon voor toepassingen | Beschrijving |
| -------------------- | ----------- |
| Aangepaste toepassing   | Maakt een lege toepassing voor u te vullen met uw eigen apparaat sjablonen en apparaten. |
| Voorbeeld Contoso       | Maakt een toepassing die gebruikmaakt van een apparaat-sjabloon voor een eenvoudige aangesloten apparaat. Met deze sjabloon kunt u aan de slag met Azure IoT centrale. |
| Voorbeeld Devkits       | Maakt een toepassing met sjablonen apparaat gereed voor een MXChip of frambozen Pi apparaat verbinding kunt maken. Deze sjabloon gebruiken als u een apparaat-ontwikkelaar experimenteren met code op een van deze apparaten. |

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een Azure IoT centrale toepassing maakt, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beheren van uw toepassing](howto-administer.md)