---
title: Maken en beheren van Azure IoT Central toepassingen als een CSP | Microsoft Docs
description: Als een CSP over het maken van een Azure IoT Central toepassing namens uw klant.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/29/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: dbc72e040f3d68ca77c036c41612f57616d1e74e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016825"
---
# <a name="as-a-csp-create-and-manage-an-azure-iot-central-application-on-behalf-of-your-customer"></a>Als een CSP, maken en beheren van een Azure IoT Central-toepassing namens uw klant 

De Microsoft Cloud Solution Provider (CSP) is een Microsoft-Reseller-programma. De bedoeling is het bieden van onze kanaalpartners met een centrale programma tot wederverkoop van alle commerciële onlineservices van Microsoft. Meer informatie over de [programma Cloud Solution Provider](https://partner.microsoft.com/cloud-solution-provider).

Als een CSP, kunt u maken en beheren van Microsoft Azure IoT Central toepassingen namens uw klanten via de [Microsoft Partner Center](https://partnercenter.microsoft.com/partner/home). Wanneer Azure IoT Central toepassingen worden gemaakt namens klanten met CSP's, net als bij andere CSP beheerde Azure-services, CSP's beheren de facturering voor klanten. Een post voor Azure IoT Central verschijnt in uw totale factuur in het Microsoft Partner Center.

Als u wilt beginnen, aanmelden bij uw account op de Microsoft-Partner-Portal en selecteer een klant voor wie u wilt maken van een Azure IoT Central-toepassing. Ga naar Service management voor de klant in het linkernavigatievenster.

![Microsoft Partner Center, aangepaste weergave](media\howto-create-application-asCSP\image1.png)

Azure IoT Central vermeld als een service die beschikbaar zijn om te beheren. Klik op de Azure IoT Central-koppeling op de pagina om nieuwe toepassingen maakt of bestaande toepassingen voor de klant beheren.

![Azure IoT Central beschikbaar voor het beheren](media\howto-create-application-asCSP\image2.png)

U komt terecht op de pagina Azure IoT Central Application Manager. Azure IoT Central houdt context of u afkomstig van het Microsoft Partner Center is en of u voor het beheren van die specifieke klant afkomstig is. U ziet dit bevestigd in de koptekst van de pagina Toepassingsbeheer. Hier kunt u ofwel navigeren naar een bestaande toepassing eerder voor de klant om te beheren of een nieuwe toepassing maken voor de klant die u hebt gemaakt.

![Manager voor CSP's maken](media\howto-create-application-asCSP\image3.png)

Voor het maken van een toepassing met Azure IoT Central, klikt u op de **nieuwe toepassing** tegel. Hiermee wordt de pagina voor het maken van de toepassing geladen. U moet Vul de velden op deze pagina en kies vervolgens **maken**. Vindt u meer informatie over elk van de velden hieronder in.

![Toepassingspagina voor CSP's maken](media\howto-create-application-asCSP\image4.png)

![Toepassingspagina voor CSP's maken](media\howto-create-application-asCSP\image4-1.png)

## <a name="payment-plan"></a>Betalingsschema

U kunt alleen betalen per gebruik-toepassingen maken als een CSP. Ter promotie van Azure IoT Central naar de klant, kunt u een proefversie afzonderlijk maken. Meer informatie over de proef- en betalen per gebruik-toepassingen in de [Azure IoT Central pagina met prijzen](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="application-name"></a>Toepassingsnaam

De naam van uw toepassing wordt weergegeven op de **Toepassingsbeheer** pagina en in elke Azure IoT Central-toepassing. U kunt een willekeurige naam voor uw Azure IoT Central-toepassing. Kies een naam die zinvol voor u en anderen in uw organisatie.

## <a name="application-url"></a>Toepassings-URL

De URL van de toepassing is de koppeling naar uw toepassing. U kunt een bladwijzer toe opslaan in uw browser of met anderen delen.

Wanneer u de naam voor uw toepassing invoert, is de URL van uw toepassing automatisch gegenereerd. Als u liever, kunt u een andere URL voor uw toepassing. Elke Azure IoT Central-URL moet uniek zijn binnen Azure IoT Central. U ziet een foutbericht weergegeven als de URL die u kiest, wordt al gebruikt.

## <a name="directory"></a>Directory

Aangezien Azure IoT Central is een context die u voor het beheren van de klant die u hebt geselecteerd in de Microsoft-Partner-Portal is geleverd, ziet u alleen de Azure Active Directory-tenant voor die klant in het veld Directory. 

Een Azure Active Directory-tenant bevat gebruikers-id's, referenties en andere gegevens van de organisatie. Meerdere Azure-abonnementen kunnen worden gekoppeld aan één Azure Active Directory-tenant.

Zie voor meer informatie, [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure-abonnement

Een Azure-abonnement kunt u exemplaren maken van Azure-services. Azure IoT Central automatisch toegang voor alle Azure-abonnementen van de klant die u hebt gevonden, en geeft deze weer in een vervolgkeuzelijst op de **toepassing maken** pagina. Kies een Azure-abonnement te maken van een nieuwe Azure IoT Central-toepassing.

Als u geen Azure-abonnement hebt, kunt u in het Microsoft Partner Center maken. Nadat u het Azure-abonnement maakt, gaat u terug naar de **toepassing maken** pagina. Uw nieuwe abonnement wordt weergegeven in de **Azure-abonnement** vervolgkeuzelijst.

Zie voor meer informatie, [Azure-abonnementen](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Regio

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

U hebt geleerd over het maken van een toepassing met Azure IoT Central als een CSP, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)