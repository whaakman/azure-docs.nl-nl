---
title: Vernieuwen met Logic Apps voor Azure Analysis Services-modellen | Microsoft Docs
description: Leer hoe u code asynchrone vernieuwing met behulp van Azure Logic Apps.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 6ffce339fe7b1a434c8f007b417ee81a42529dfc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66142546"
---
# <a name="refresh-with-logic-apps"></a>Vernieuwen met Logic Apps

U kunt met behulp van Logic Apps en REST-aanroepen, geautomatiseerde gegevensvernieuwingsbewerkingen op uw Azure Analysis modellen in tabelvorm, met inbegrip van synchronisatie van alleen-lezen replica's voor query's worden uitgeschaald uitvoeren.

Zie voor meer informatie over het gebruik van REST-API's met Azure Analysis Services, [asynchroon vernieuwen met de REST-API](analysis-services-async-refresh.md).

## <a name="authentication"></a>Verificatie

Alle-aanroepen moeten worden geverifieerd met een geldig token voor Azure Active Directory (OAuth 2).  De voorbeelden in dit artikel wordt een SPN (Service Principal) gebruiken om te verifiëren met Azure Analysis Services. Zie voor meer informatie, [een service-principal maken met behulp van Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="design-the-logic-app"></a>De logische App ontwerpen

> [!IMPORTANT]
> De volgende voorbeelden wordt ervan uitgegaan dat de Azure Analysis Services-firewall is uitgeschakeld.  Als de firewall is ingeschakeld, moet het openbare IP-adres van de aanvraag-initiator in de whitelist opgenomen in de Azure Analysis Services-firewall zijn. Zie voor meer informatie over logische App IP-adresbereiken per regio, [limieten en configuratie-informatie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses).

### <a name="prerequisites"></a>Vereisten

#### <a name="create-a-service-principal-spn"></a>Maken van een Service-Principal (SPN)

Zie voor meer informatie over het maken van een Service-Principal, [een service-principal maken met behulp van Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="configure-permissions-in-azure-analysis-services"></a>Machtigingen te configureren in Azure Analysis Services
 
De Service-Principal die u maakt, moet machtigingen voor serverbeheerders hebben op de server. Zie voor meer informatie, [een service-principal toevoegen aan de serverbeheerdersrol](analysis-services-addservprinc-admins.md).

### <a name="configure-the-logic-app"></a>Configureren van de logische App

In dit voorbeeld worden de logische App is ontworpen om te activeren wanneer een HTTP-aanvraag wordt ontvangen. Hiermee schakelt u het gebruik van een orchestration-hulpprogramma, zoals Azure Data Factory, voor het activeren van het vernieuwen van de Azure Analysis Services-model.

Als u een logische App hebt gemaakt:

1. Kies in de ontwerpfunctie voor logische App de eerste actie als **wanneer een HTTP-aanvraag wordt ontvangen**.

   ![HTTP-ontvangen activiteit toevoegen](./media/analysis-services-async-refresh-logic-app/1.png)

Deze stap wordt gevuld met de URL voor HTTP POST wanneer de logische App is opgeslagen.

2. Een nieuwe stap toevoegen en zoek naar de **HTTP**.  

   ![HTTP-activiteit toevoegen](./media/analysis-services-async-refresh-logic-app/9.png)

   ![HTTP-activiteit toevoegen](./media/analysis-services-async-refresh-logic-app/10.png)

3. Selecteer **HTTP** om toe te voegen met deze actie.

   ![HTTP-activiteit toevoegen](./media/analysis-services-async-refresh-logic-app/2.png)

De HTTP-activiteit als volgt configureren:

|Eigenschap  |Value  |
|---------|---------|
|**Methode**     |POST         |
|**URI**     | https://*de regio van uw server*/servers/*aas servernaam*/models/*de databasenaam van uw*/ <br /> <br /> Bijvoorbeeld: https:\//westus.asazure.windows.net/servers/myserver/models/AdventureWorks/|
|**Headers**     |   Content-Type, application/json <br /> <br />  ![Headers](./media/analysis-services-async-refresh-logic-app/6.png)    |
|**Hoofdtekst**     |   Zie voor meer informatie over de aanvraagtekst vormen, [asynchroon vernieuwen met de REST-API - POST /refreshes](analysis-services-async-refresh.md#post-refreshes). |
|**Verificatie**     |Active Directory OAuth         |
|**Tenant**     |Vul in uw Azure Active Directory-tenant-id         |
|**Doelgroep**     |https://*.asazure.windows.net         |
|**Client ID**     |Voer de ClientID van uw Service-Principal-naam         |
|**Referentietype**     |`Secret`         |
|**Geheim**     |Uw Service-Principal-naam van geheim invoeren         |

Voorbeeld:

![Voltooide HTTP-activiteit](./media/analysis-services-async-refresh-logic-app/7.png)

Test nu de logische App.  Klik in de ontwerpfunctie voor logische App op **uitvoeren**.

![De logische App testen](./media/analysis-services-async-refresh-logic-app/8.png)

## <a name="consume-the-logic-app-with-azure-data-factory"></a>De logische App met Azure Data Factory gebruiken

Wanneer de logische App is opgeslagen, Controleer de **wanneer een HTTP-aanvraag wordt ontvangen** activiteit en kopieer de **URL voor HTTP POST** die nu wordt gegenereerd.  Dit is de URL die kan worden gebruikt door Azure Data Factory om de asynchrone aanroep aan de logische App trigger te maken.

Hier volgt een voorbeeld van Azure Data Factory webactiviteit die door deze actie worden ondersteund.

![Data Factory-webactiviteit](./media/analysis-services-async-refresh-logic-app/11.png)

## <a name="use-a-self-contained-logic-app"></a>Een op zichzelf staand logica-App gebruiken

Als u niet van plan bent over het gebruik van een Orchestration-hulpprogramma, zoals Data Factory voor het activeren van het vernieuwen van het model, kunt u de logische app voor het activeren van de vernieuwing op basis van een planning instellen.

In het bovenstaande voorbeeld verwijderen van de eerste activiteit en vervang deze door een **planning** activiteit.

![Activiteit planning](./media/analysis-services-async-refresh-logic-app/12.png)

![Activiteit planning](./media/analysis-services-async-refresh-logic-app/13.png)

Dit voorbeeld gebruiken **terugkeerpatroon**.

Nadat de activiteit is toegevoegd, het Interval en frequentie, configureren en vervolgens een nieuwe parameter en kies **deze uren**.

![Activiteit planning](./media/analysis-services-async-refresh-logic-app/16.png)

Selecteer de gewenste uur.

![Activiteit planning](./media/analysis-services-async-refresh-logic-app/15.png)

Sla de logische App.

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
