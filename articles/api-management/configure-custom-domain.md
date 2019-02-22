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
ms.date: 12/14/2017
ms.author: apimpm
ms.openlocfilehash: cb7ccc665cdf9867232580fd8b687b344e43116d
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56587273"
---
# <a name="configure-a-custom-domain-name"></a>Een aangepaste domeinnaam configureren 

Wanneer u een exemplaar van API Management (APIM) maakt, Azure toegewezen aan een subdomein van azure-api.net (bijvoorbeeld `apim-service-name.azure-api.net`). U kunt echter uw APIM-eindpunten met behulp van uw eigen domeinnaam, zoals weergeven **contoso.com**. Deze zelfstudie leert u hoe u kunt een bestaande aangepaste DNS-naam toewijzen aan eindpunten die worden weergegeven door een Azure API Management-exemplaar.

> [!WARNING]
> Klanten die gebruiken certificaat vast te maken willen voor het verbeteren van de beveiliging van hun toepassingen moeten gebruiken een aangepaste domeinnaam > en het certificaat die ze beheren, niet de standaardcertificaat. Klanten die het standaardcertificaat vastmaken in plaats daarvan worden > waarbij een vaste afhankelijkheid van de eigenschappen van het certificaat dat ze niet worden beheerd, dit is geen aanbevolen.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een APIM-instantie. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).
+ Een aangepaste domeinnaam die eigendom is van u. De aangepaste domeinnaam die u wilt gebruiken, moet worden afzonderlijk aangeschaft en die worden gehost op een DNS-server. In dit onderwerp geeft geen instructies voor het hosten van een aangepaste domeinnaam.
+ U moet een geldig certificaat met een openbare en persoonlijke sleutel hebben (. (PFX). Onderwerpnaam of alternatieve naam voor onderwerp (SAN) moet overeenkomen met de naam van het domein (Hiermee kunt u APIM URL's, veilig kunt blootstellen via SSL).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Gebruik de Azure portal voor het instellen van een aangepaste domeinnaam

1. Navigeer naar de APIM-instantie in de [Azure-portal](https://portal.azure.com/).
1. Selecteer **aangepaste domeinen en SSL**.
    
    Er zijn een aantal eindpunten waarop u een aangepaste domeinnaam kunt toewijzen. De volgende eindpunten zijn momenteel beschikbaar: 
    + **Proxy** (standaard: `<apim-service-name>.azure-api.net`), 
    + **Portal** (standaard: `<apim-service-name>.portal.azure-api.net`),     
    + **Management** (standaard: `<apim-service-name>.management.azure-api.net`), 
    + **SCM** (standaard: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > U kunt alle van de eindpunten of een deel van deze bijwerken. Normaal gesproken klanten bijwerken **Proxy** (deze URL wordt gebruikt voor het aanroepen van de API die toegankelijk is via API Management) en **Portal** (de ontwikkelaarsportal URL). **Management** en **SCM** eindpunten wordt intern gebruikt door APIM-klanten en dus minder vaak een aangepaste domeinnaam toegewezen.

1. Selecteer het eindpunt dat u wilt bijwerken. 
1. Klik in het venster aan de rechterkant, **aangepaste**.

    + In de **aangepaste domeinnaam**, geef de naam die u wilt gebruiken. Bijvoorbeeld `api.contoso.com`. Domeinnamen van het jokerteken (bijvoorbeeld *. domein.com) worden ook ondersteund.
    + In de **certificaat**, selecteert u een certificaat uit Key Vault. U kunt ook een geldige uploaden. PFX-bestand en geef de **wachtwoord**, als het certificaat is beveiligd met een wachtwoord.

    > [!TIP]
    > Als u Azure Key Vault gebruikt voor het beheren van het SSL-certificaat van het aangepaste domein, controleert u of het certificaat wordt ingevoegd in Key Vault [als een *certificaat*](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate), niet een *geheim*. Als het certificaat is ingesteld op autorotate, pikken API Management de nieuwste versie automatisch.

1. Klik op toepassen.

    >[!NOTE]
    >Het proces van het toewijzen van het certificaat duurt 15 minuten of langer, afhankelijk van de grootte van de implementatie. Developer SKU heeft downtime, Basic en hoger SKU's zijn geen downtime.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Volgende stappen

[Bijwerken en schalen van uw service](upgrade-and-scale.md)
