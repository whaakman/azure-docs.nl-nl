---
title: Een aangepaste domeinnaam voor uw exemplaar van Azure API Management configureren | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u een aangepaste domeinnaam voor uw exemplaar van Azure API Management configureren.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 07/01/2019
ms.author: apimpm
ms.openlocfilehash: 59b44dcc9ec3a1f7c274f426a19aa8ed2258db3e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509306"
---
# <a name="configure-a-custom-domain-name"></a>Een aangepaste domeinnaam configureren

Wanneer u een Azure API Management service-exemplaar maakt, Azure wijst deze toe een subdomein van azure-api.net (bijvoorbeeld `apim-service-name.azure-api.net`). U kunt echter uw API Management-eindpunten met behulp van de naam van uw eigen aangepaste domein, zoals weergeven **contoso.com**. Deze zelfstudie leert u hoe u kunt een bestaande aangepaste DNS-naam toewijzen aan eindpunten die worden weergegeven door een exemplaar van API Management.

> [!WARNING]
> Klanten die gebruiken certificaat vast te maken willen voor het verbeteren van de beveiliging van hun toepassingen moeten gebruiken een aangepaste domeinnaam > en het certificaat die ze beheren, niet de standaardcertificaat. Klanten die het standaardcertificaat vastmaken in plaats daarvan worden > waarbij een vaste afhankelijkheid van de eigenschappen van het certificaat dat ze niet worden beheerd, dit is geen aanbevolen.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

-   Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Exemplaar van API Management. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).
-   Een aangepaste domeinnaam die eigendom is van u. De aangepaste domeinnaam die u wilt gebruiken, moet worden afzonderlijk aangeschaft en die worden gehost op een DNS-server. In dit onderwerp geeft geen instructies voor het hosten van een aangepaste domeinnaam.
-   U moet een geldig certificaat met een openbare en persoonlijke sleutel hebben (. (PFX). Onderwerpnaam of alternatieve naam voor onderwerp (SAN) moet overeenkomen met de naam van het domein (Hiermee kunt u API Management-exemplaar URL's, veilig kunt blootstellen via SSL).

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Gebruik de Azure portal voor het instellen van een aangepaste domeinnaam

1. Navigeer naar uw API Management-exemplaar in de [Azure-portal](https://portal.azure.com/).
1. Selecteer **aangepaste domeinen en SSL**.

    Er zijn een aantal eindpunten waarop u een aangepaste domeinnaam kunt toewijzen. De volgende eindpunten zijn momenteel beschikbaar:

    - **Proxy** (standaard: `<apim-service-name>.azure-api.net`),
    - **Portal** (standaard: `<apim-service-name>.portal.azure-api.net`),
    - **Management** (standaard: `<apim-service-name>.management.azure-api.net`),
    - **SCM** (standaard: `<apim-service-name>.scm.azure-api.net`).

    > [!NOTE]
    > U kunt alle van de eindpunten of een deel van deze bijwerken. Normaal gesproken klanten bijwerken **Proxy** (deze URL wordt gebruikt voor het aanroepen van de API die toegankelijk is via API Management) en **Portal** (de ontwikkelaarsportal URL). **Management** en **SCM** eindpunten wordt intern gebruikt door de API Management-exemplaar eigenaren alleen en dus minder vaak een aangepaste domeinnaam toegewezen. In de meeste gevallen kan alleen een één aangepaste domeinnaam voor een bepaald eindpunt worden ingesteld. Echter, de **Premium** laag ondersteunt het instellen van meerdere hostnamen voor de **Proxy** eindpunt.

1. Selecteer het eindpunt dat u wilt bijwerken.
1. Klik in het venster aan de rechterkant, **aangepaste**.

    - In de **aangepaste domeinnaam**, geef de naam die u wilt gebruiken. Bijvoorbeeld `api.contoso.com`. Domeinnamen van het jokerteken (bijvoorbeeld \*. domein.com) worden ook ondersteund.
    - In de **certificaat**, selecteert u een certificaat uit Key Vault. U kunt ook een geldige uploaden. PFX-bestand en geef de **wachtwoord**, als het certificaat is beveiligd met een wachtwoord.

    > [!TIP]
    > Het is raadzaam om met behulp van Azure Key Vault voor het beheren van certificaten en het instellen van deze naar autorotate.
    > Als u Azure Key Vault gebruikt voor het beheren van het SSL-certificaat van het aangepaste domein, controleert u of het certificaat wordt ingevoegd in Key Vault [als een _certificaat_](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate), niet een _geheim_.
    >
    > Om op te halen van een SSL-certificaat, moet API Management de lijst met een get-geheimen machtigingen hebben op de Azure Key Vault met het certificaat. Wanneer u Azure portal wordt automatisch de benodigde configuratiestappen worden voltooid. Wanneer u vanaf de opdrachtregel-hulpprogramma's of API-beheer, moeten deze machtigingen handmatig worden verleend. Dit wordt gedaan in twee stappen. Gebruik eerst beheerde identiteiten pagina op uw exemplaar van API Management en zorg ervoor dat de beheerde identiteit is ingeschakeld en noteer de principal-id weergegeven op de pagina. Ten tweede lijst met machtigingen geven en ophalen van geheimen machtigingen voor deze principal-id in de Azure Key Vault met het certificaat.
    >
    > Als het certificaat is ingesteld op autorotate, haalt API Management de meest recente versie automatisch zonder uitvaltijd voor de service (als uw API Management-laag is de SLA - dat wil zeggen in alle lagen met uitzondering van de Developer-laag).

1. Klik op toepassen.

    > [!NOTE]
    > Het proces van het toewijzen van het certificaat duurt 15 minuten of langer, afhankelijk van de grootte van de implementatie. Developer SKU heeft downtime, Basic en hoger SKU's zijn geen downtime.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Volgende stappen

[Bijwerken en schalen van uw service](upgrade-and-scale.md)
