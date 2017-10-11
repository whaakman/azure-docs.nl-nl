---
title: Een cloudservice automatisch schalen in de klassieke portal | Microsoft Docs
description: (klassiek) Informatie over het configureren van regels voor automatisch schalen voor een cloud service-web-rol of functie worker in Azure met behulp van de klassieke portal.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a>Het automatisch schalen voor een Cloudservice in de klassieke portal configureren
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Klassieke Azure Portal](cloud-services-how-to-scale.md)

U kunt instellingen voor automatische voor uw Webrol of functie worker configureren op de pagina schaal van de klassieke Azure portal. U kunt ook handmatig schalen in plaats van automatisch schalen op basis van regels.

> [!NOTE]
> Dit artikel is gericht op Cloud Service-web- en werkrollen rollen. Wanneer u een virtuele machine (klassiek) rechtstreeks maakt, wordt deze gehost in een cloudservice. Sommige van deze informatie geldt voor deze typen virtuele machines. Schalen van een beschikbaarheidsset van virtuele machines wordt alleen afgesloten ze in- of uitschakelen op basis van de scale-regels die u configureert. Zie voor meer informatie over virtuele Machines en beschikbaarheidssets [de beschikbaarheid van virtuele Machines beheren](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

U kunt de volgende informatie voordat u configureert voor uw toepassing schalen:

* Schalen wordt beïnvloed door de belangrijkste informatie over het gebruik.

    Grotere rolinstanties gebruik meer kernen. U kunt een toepassing alleen binnen de limiet van kernen schalen voor uw abonnement. Bijvoorbeeld, Stel dat uw abonnement heeft een limiet van 20 kernen. Als u een toepassing met twee middelgrote cloudservices (in totaal 4 kernen) uitvoert, kunt u alleen opschalen andere cloud service-implementaties in uw abonnement door de resterende 16 kernen. Zie voor meer informatie over grootten [Cloud Service Sizes](cloud-services-sizes-specs.md).

* U moet een wachtrij maken en deze koppelen aan een rol voordat u een toepassing op basis van een bericht drempelwaarde kunt schalen. Zie voor meer informatie [het gebruik van de Queue Storage-Service](../storage/queues/storage-dotnet-how-to-use-queues.md).

* U kunt de resources die zijn gekoppeld aan uw cloudservice schalen. Zie voor meer informatie over het koppelen van resources [procedure: een bron koppelen aan een cloudservice](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* Om hoge beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze wordt geïmplementeerd met twee of meer rolinstanties. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Planning schalen
Standaard alle rollen niet op een specifiek schema. Daarom instellingen gewijzigd van toepassing op alle tijden en alle dagen in het jaar. Als u wilt, kunt u de installatie handmatig of automatisch schalen voor een van de volgende modi:

* Weekdagen
* Tijdens het weekend
* Week nachten
* Uitvoeren van de week
* Specifieke datums
* Specifiek datumbereik

De instelling van de planning is geconfigureerd in de [klassieke Azure-portal](https://manage.windowsazure.com/) op de  
**Cloudservices** > **\[uw cloudservice\]** > **Scale** > **\[productie of Staging\]**  pagina.

Klik op de **instellen van tijden waarop planning** knop voor elke rol die u wilt wijzigen.

![Cloudservice automatisch schalen op basis van een planning][scale_schedules]

## <a name="manual-scale"></a>Handmatig schalen
Op de **Scale** pagina, u kunt handmatig vergroten of verkleinen het aantal exemplaren in een cloudservice wordt uitgevoerd. Deze instelling is geconfigureerd voor elke planning die u hebt gemaakt of aan alle tijd als u een planning niet hebt gemaakt.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**, en klik vervolgens op de naam van de cloudservice om het dashboard te openen.
   
   > [!TIP]
   > Als u de cloudservice niet ziet, moet u mogelijk gewijzigd van **productie** naar **fasering** of vice versa.

2. Klik op **Scale**.
3. Selecteer de planning die u wilt schalen opties voor het wijzigen. *Geen geplande tijden* is de standaardwaarde als er geen schema's gedefinieerd.
4. Zoeken naar de **schaal op metriek** sectie en selecteer ****. Dit is de standaardinstelling voor alle functies.
5. Elke rol in de cloudservice heeft een schuifregelaar voor het wijzigen van het aantal exemplaren moet worden gebruikt.
   
    ![De functie van een cloud-service handmatig schalen][manual_scale]
   
    Als u meer exemplaren nodig hebt, moet u mogelijk wijzigen de [cloud van de grootte van de virtuele machine service](cloud-services-sizes-specs.md).
6. Klik op **Opslaan**.  
   Rolinstanties worden toegevoegd of verwijderd op basis van uw selecties.

> [!TIP]
> Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp over welke specifieke instelling komt.

## <a name="automatic-scale---cpu"></a>Automatische scale - CPU
Deze modus wordt geschaald als het gemiddelde percentage van CPU-gebruik boven of onder de opgegeven drempelwaarden gaat. Rolinstanties zijn gemaakt of verwijderd als dit gebeurt.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**, en klik vervolgens op de naam van de cloudservice om het dashboard te openen.
   
   > [!TIP]
   > Als u de cloudservice niet ziet, moet u mogelijk gewijzigd van **productie** naar **fasering** of vice versa.

2. Klik op **Scale**.
3. Selecteer de planning die u wilt schalen opties voor het wijzigen. *Geen geplande tijden* is de standaardwaarde als er geen schema's gedefinieerd.
4. Zoeken naar de **schaal op metriek** sectie en selecteer **CPU**.
5. U kunt nu een minimum en maximum aantal rollen instanties, het doel CPU-gebruik (voor het activeren van een schaal van) en hoeveel exemplaren omhoog en omlaag schalen door configureren.

![De functie van een cloud-service schalen door cpu-belasting][cpu_scale]

> [!TIP]
> Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp over welke specifieke instelling komt.

## <a name="automatic-scale---queue"></a>Automatische scale - wachtrij
Deze modus wordt automatisch geschaald als het aantal berichten in een wachtrij boven of onder een bepaalde drempelwaarde vallen gaat. Rolinstanties zijn gemaakt of verwijderd als dit gebeurt.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**, en klik vervolgens op de naam van de cloudservice om het dashboard te openen.
   
   > [!TIP]
   > Als u de cloudservice niet ziet, moet u mogelijk gewijzigd van **productie** naar **fasering** of vice versa.

2. Klik op **Scale**.
3. Zoeken naar de **schaal op metriek** sectie en selecteer **wachtrij**.
4. U kunt nu een minimum en maximum aantal exemplaren van de functies, de wachtrij en het aantal Wachtrijberichten voor het verwerken voor elk exemplaar en hoeveel exemplaren omhoog en omlaag schalen door configureren.

![De functie van een cloud-service schalen door een berichtenwachtrij][queue_scale]

> [!TIP]
> Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp over welke specifieke instelling komt.

## <a name="scale-linked-resources"></a>Gekoppelde resources schalen
Vaak wanneer u een rol schalen, is het nuttig voor het schalen van de database die de toepassing ook wordt gebruikt. Als u de database een koppeling naar de cloudservice, kunt u de instellingen voor schalen voor die bron openen door te klikken op de koppeling.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**, en klik vervolgens op de naam van de cloudservice om het dashboard te openen.
   
   > [!TIP]
   > Als u de cloudservice niet ziet, moet u mogelijk gewijzigd van **productie** naar **fasering** of vice versa.

2. Klik op **Scale**.
3. Zoeken naar de **gekoppelde resources** sectie en klik op **scale voor deze database beheren**.
   
   > [!NOTE]
   > Als er geen een **gekoppelde resources** sectie u waarschijnlijk geen gekoppelde resources.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
