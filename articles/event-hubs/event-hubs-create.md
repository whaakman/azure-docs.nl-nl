---
title: Een Azure event hub maken | Microsoft Docs
description: Maak een Azure Event Hubs-naamruimte en een event hub met behulp van de Azure portal
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: shvija
ms.openlocfilehash: fb2020f7bfc8521e141adb4eefbc227e8123a269
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40002617"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Maak een Event Hubs-naamruimte en een event hub met behulp van de Azure portal

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

1. Meld u aan bij de [Azure-portal][Azure portal], en klikt u op **een resource maken** op linksboven in het scherm.
2. Klik op **Internet of Things** en vervolgens op **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. Voer in **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.  

4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic of Standard). Kies ook een Azure-abonnement, resourcegroep en locatie voor het maken van de resource.
 
5. Klik op **Maken** om de naamruimte te maken. U moet een paar minuten wachten voordat het systeem de resources volledig heeft ingericht.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Klik in de portallijst met naamruimten op de zojuist gemaakte naamruimte.

7. Klik op **Beleid voor gedeelde toegang** en vervolgens op **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. Klik op de knop voor kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord te kopiëren. Bewaar deze verbindingsreeks op een tijdelijke locatie, zoals Kladblok, om later te gebruiken.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Een Event Hub maken

1. Klik in de lijst met naamruimten van Event Hubs op de zojuist gemaakte naamruimte.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Klik op de blade Naamruimte op **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Aan de bovenkant van de blade, klikt u op **+ Event Hub**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Typ een naam voor uw Event Hub en klik vervolgens op **Maken**. 

Uw event hub is nu gemaakt en u hebt de verbindingsreeksen die u wilt verzenden en ontvangen van gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Event Hubs:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Event Hubs-API-overzicht](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/