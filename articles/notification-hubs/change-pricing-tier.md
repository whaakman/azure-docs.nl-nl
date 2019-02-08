---
title: Wijzig de prijscategorie van Notification Hubs-naamruimte | Microsoft Docs
description: Informatie over het wijzigen van de prijscategorie van een Azure Notification Hubs-naamruimte.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/28/2019
ms.author: jowargo
ms.openlocfilehash: c572f64bdcb8846b5f73a0fee34f11c9729ee45d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885052"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Wijzig de prijscategorie van een Azure notification hubs-naamruimte
Notification Hubs wordt aangeboden in drie lagen: **gratis**, **basic**, en **standard**. Dit artikel leest u hoe u de prijscategorie voor een Azure Notification Hubs-naamruimte. 

## <a name="overview"></a>Overzicht
In de Azure Notification Hubs, een **hub** is de kleinste resource/entiteit. Het algemeen is toegewezen aan een toepassing en kan er een bevatten één certificaat voor elk Platform Notification System voor de app wordt ondersteund. De toepassing kan worden een hybride of een systeemeigen en platformoverschrijdende toepassingen.

Een **naamruimte** is een verzameling van notification hubs. Elke naamruimte bestaat meestal uit hubs die zijn gerelateerd en gebruikt voor een specifiek doel. Bijvoorbeeld, kan u drie verschillende naamruimten voor ontwikkeling, testen en productie doeleinden respectievelijk hebben. 

U kunt een prijscategorie op het niveau van de naamruimte koppelen. Notification Hubs ook ondersteuning voor drie lagen: **gratis**, **basic**, en **standard**. U kunt de laag gebruiken voor een naamruimte die tegemoetkomt aan uw vereisten. De volgende secties laten zien hoe u de prijscategorie van een Notification Hubs-naamruimte wijzigen. 

## <a name="use-azure-portal"></a>Azure Portal gebruiken 
Wanneer u Azure portal gebruikt, kunt u de prijscategorie voor een naamruimte op de pagina van de naamruimte of een hub-pagina.  Wanneer u deze op een hub-pagina wijzigt, wijzigt u het daadwerkelijk op het niveau van de naamruimte. De prijscategorie voor de naamruimte en alle hubs in de naamruimte worden gewijzigd. 

### <a name="change-tier-on-the-namespace-page"></a>Laag wijzigt voor de naamruimte-pagina
De volgende procedure bevat stappen voor het wijzigen van de prijscategorie voor een naamruimte op de pagina van de naamruimte. Wanneer u de laag voor een naamruimte wijzigt, is dit van toepassing op alle hubs in de naamruimte.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** in het menu links. 
3. Selecteer **Notification Hub-naamruimten** in de **Internet of Things** sectie. Als u een ster (`*`) naast de tekst, wordt deze toegevoegd aan de linkernavigatiebalk onder **Favorieten**. Dit helpt u bij het openen van de pagina naamruimten sneller de volgende keer en hoger. Nadat u deze aan Favorieten toevoegen, selecteert u **Notification Hub-naamruimten**. 

    ![Alle services -> Notification Hub-naamruimten](./media/change-pricing-tier/all-services-nhub.png)
1. Op de **Notification Hub-naamruimten** pagina, selecteert u de naamruimte die u wilt de prijscategorie wijzigen. 
2. Op de **Notification Hub Namespace** pagina voor de naamruimte, ziet u de huidige prijscategorie voor de naamruimte in de **Essentials** sectie. In de volgende afbeelding ziet u dat de prijscategorie van de naamruimte is **gratis**. 

    ![Huidige prijscategorie op de pagina van de naamruimte](./media/change-pricing-tier/pricing-tier-before.png)
1. Op de **Notification Hub Namespace** pagina voor de naamruimte, selecteer **prijscategorie** onder **beheren** sectie. 

    ![Prijscategorie op de pagina naamruimte selecteren](./media/change-pricing-tier/namespace-select-pricing-menu.png)
6. Uw prijscategorie wijzigen en klik op de **Selecteer** knop.    
7. U ziet de status van de laag wijzigen van de actie in de **waarschuwingen**. 
8. Schakel over naar de **overzicht** pagina. Bevestig dat de nieuwe laag wordt weergegeven voor de **prijscategorie** veld in de **Essentials** sectie.     
1. Deze stap is optioneel. Selecteer een hub in de naamruimte. Bevestig dat de dezelfde prijzen laag in de **Essentials** sectie. Hier ziet u de dezelfde prijscategorie voor alle hubs in de naamruimte. 

### <a name="change-tier-on-the-hub-page"></a>Laag wijzigt voor de hub-pagina
De volgende procedure bevat stappen voor het wijzigen van de prijscategorie voor een naamruimte op de pagina van de hub. Hoewel u deze stappen vanaf de pagina hub doet, wordt de prijscategorie voor de naamruimte en alle hubs in de naamruimte daadwerkelijk wijzigen. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** in het menu links.
3. Selecteer **Notification Hubs** in de **Internet of Things** sectie. 
4. Selecteer uw bericht **hub**. 
5. Selecteer **prijscategorie** in het menu links. 
6. Wijzig de prijscategorie en klikt u op de **Selecteer** knop. Hiermee wijzigt u de prijzen laag-instelling voor de naamruimte waarin de hub. Dus, ziet u de nieuwe prijscategorie op de pagina van de naamruimte en in alle hub-pagina's. 

## <a name="use-rest-api"></a>REST API gebruiken
U kunt de volgende Resource Provider REST-API's gebruiken voor het ophalen van de huidige prijscategorie en deze bijwerken. 

### <a name="get-current-pricing-tier-for-a-namespace"></a>Huidige prijscategorie voor een naamruimte ophalen
Aan de **huidige naamruimte laag**, een GET-opdracht verzenden, zoals wordt weergegeven in het volgende voorbeeld: 

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Prijscategorie voor een naamruimte bijwerken
Naar **bijwerken van de naamruimte-laag**, een PUT-opdracht verzenden, zoals wordt weergegeven in het volgende voorbeeld: 

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```



## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over deze lagen en prijzen, Zie [prijzen van Notification Hubs](https://azure.microsoft.com/pricing/details/notification-hubs/).
