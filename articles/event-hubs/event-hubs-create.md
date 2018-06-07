---
title: Een Azure event hub maken | Microsoft Docs
description: Een Azure Event Hubs-naamruimte en een event hub met de Azure portal maken
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: sethm
ms.openlocfilehash: 9b466d4e727c1511ca2318c0da3ec2807a965a5d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625539"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Een Event Hubs-naamruimte en een event hub met de Azure portal maken

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

1. Meld u aan bij de [Azure-portal][Azure portal], en klik op **maken van een resource** op linksboven op het scherm.
2. Klik op **Internet der dingen**, en klik vervolgens op **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. In **naamruimte maken**, voer de naam van een naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.  

4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic of Standard). Kies ook een Azure-abonnement, resourcegroep en locatie voor het maken van de resource.
 
5. Klik op **Maken** om de naamruimte te maken. U moet een paar minuten wachten voordat het systeem de resources volledig heeft ingericht.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Klik in de portal lijst met naamruimten op de zojuist gemaakte naamruimte.

7. Klik op **gedeeld toegangsbeleid**, en klik vervolgens op **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. Klik op de knop voor kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord te kopiëren. Sla deze verbindingsreeks in een tijdelijke locatie, zoals Kladblok voor later gebruik.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Een Event Hub maken

1. Klik op de zojuist gemaakte naamruimte in de lijst van de naamruimte Event Hubs.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Klik op de blade Naamruimte op **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Klik boven aan de blade op **+ Event Hub**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Typ een naam voor uw event hub en klik vervolgens op **maken**. 

Uw event hub is nu gemaakt en u hebt de verbindingsreeksen die u wilt verzenden en ontvangen van gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs, gaat u naar deze koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Event Hubs-API-overzicht](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/