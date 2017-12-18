---
title: Een Azure API Management-exemplaar maken | Microsoft Docs
description: Volg de stappen in deze zelfstudie om een nieuw Azure API Management-exemplaar te maken.
services: api-management
documentationcenter: 
author: juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: b7ea43419cbbb5f5d38f8ad721d564650c3681a6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Een nieuw exemplaar van de API Management-service maken

APIM (API Management) helpt organisaties bij het publiceren van API's naar externe en interne ontwikkelaars, en naar partnerontwikkelaars om alle mogelijkheden van hun gegevens en services te ontsluiten. API Management beschikt over de competenties die belangrijk zijn voor een geslaagd API-programma via ontwikkelaarsbetrokkenheid, zakelijke inzichten, analytische gegevens, beveiliging en bescherming. Met APIM kunt u moderne API-gateways maken en beheren voor bestaande back-endservices die op elke willekeurige locatie worden gehost. Bekijk het [Overzicht](api-management-key-concepts.md) voor meer informatie.

In deze quickstart worden de stappen beschreven voor het maken van een nieuw API Management-exemplaar met behulp van Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

![nieuw exemplaar](./media/get-started-create-service-instance/get-started-create-service-instance-created.png)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-new-service"></a>Een nieuwe service maken

1. Selecteer in [Azure Portal](https://portal.azure.com/) achtereenvolgens **Nieuw** > **Bedrijfsintegratie** > **API Management**.

    U kunt ook **Nieuw** kiezen, `API management` typen in het zoekvak en op Enter drukken. Klik op **Create**.

2. Voer in het venster **API Management-service** de instellingen in.

    ![nieuw exemplaar](./media/get-started-create-service-instance/get-started-create-service-instance-create-new.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving              |
    | ------------ |  ------- | ---------------------------------|
    |**Naam**|Een unieke naam voor de API Management-service| De naam kan later niet meer worden gewijzigd. De servicenaam wordt gebruikt om een standaarddomeinnaam te genereren in de notatie: *{name}.azure-api.net.* Zie [Een aangepast domein configureren](configure-custom-domain.md) als u een aangepaste domeinnaam wilt gebruiken. <br/> De servicenaam wordt gebruikt om naar de service en de bijbehorende Azure-resource te verwijzen.|
    |**Abonnement**|Uw abonnement | Het abonnement waarin dit nieuwe service-exemplaar wordt gemaakt. U kunt het abonnement selecteren in een van de verschillende Azure-abonnementen waartoe u toegang hebt.|
    |**Resourcegroep**|*apimResourceGroup*|U kunt een nieuwe of bestaande resource selecteren. Een resourcegroep is een verzameling resources met dezelfde levenscyclus, dezelfde machtigingen en hetzelfde beleid. Klik [hier](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.|
    |**Locatie**|*VS - west*|Selecteer de geografische regio bij u in de buurt. Alleen de beschikbare regio’s voor de API Management-service worden weergegeven in de vervolgkeuzelijst. |
    |**Naam van de organisatie**|De naam van uw organisatie|Deze naam wordt op een aantal plekken gebruikt, onder andere in de titel van de ontwikkelaarsportal en als afzender bij e-mailmeldingen.|
    |**E-mailadres van de beheerder**|*admin@org.com*|Vast e-mailadres waarnaar alle meldingen afkomstig van **API Management** worden verzonden.|
    |**Prijscategorie**|*Developer*|Vaste **Developer**-laag om de service te evalueren. Deze laag is niet bedoeld voor productie. Zie [bijwerken en schalen](upgrade-and-scale.md) voor meer informatie over het schalen van API Management-lagen.|
3. Kies **Maken**.

    > [!TIP]
    > Het duurt meestal tussen de 20 en 30 minuten om een API Management-service te maken. Als u **Vastmaken aan dashboard** selecteert, kunt u een nieuwe service gemakkelijker terugvinden.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer de resourcegroep niet meer nodig is, kunt u deze stappen volgen om de resourcegroep en alle gerelateerde resources te verwijderen:


1. In Azure Portal selecteert u ![pijl](./media/get-started-create-service-instance/arrow.png).
2. Selecteer **Resourcegroepen**.
3. Ga naar uw resourcegroep.
4. Klik op . . . en verwijder de groep.

![opruimen](./media/get-started-create-service-instance/cleanup.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eerste API importeren en publiceren](import-and-publish.md)
