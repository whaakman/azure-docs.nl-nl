---
title: Beheer en ontwikkeling taakoverzicht in BizTalk Services | Microsoft Docs
description: Planning en taak helpen voor het implementeren van Azure BizTalk Services.
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 7d4532daf5e4b8f45de94bbec230633978814a6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Beheer en ontwikkeling takenlijst in BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="getting-started"></a>Aan de slag
Als u werkt met Microsoft Azure BizTalk Services, zijn er verschillende on-premises en cloud-gebaseerde onderdelen in overweging moet nemen. Houd rekening met de volgende processtroom om te beginnen:  

| Stap | Wie is verantwoordelijk | Taak | Verwante koppelingen |
| --- | --- | --- | --- |
| 1. |Beheerder |De Microsoft Azure-abonnement met een Microsoft-account of een organisatie-account maken |[Klassieke Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) |
| 2. |Beheerder |Maken of een BizTalk Service inrichten. |[Een BizTalk Service met behulp van de klassieke Azure portal maken](http://go.microsoft.com/fwlink/p/?LinkID=302280) |
| 3. |Beheerder |Registreer u of uw bedrijf BizTalk Services-implementatie |[Registreren en een BizTalk Service-implementatie in de Portal van BizTalk Services bijwerken](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Beheerder |Van toepassing als de toepassing BizTalk Adapter Service gebruikt om verbinding met een on-premises Line-of-Business (LOB)-systeem of een wachtrij of onderwerp doel gebruikt.  De Azure Service Bus-Namespace maken. Deze naamruimte, naam van Service Bus verlener en sleutel van verlener van Service Bus waarden geven aan de ontwikkelaar. |[Hoe: maken of wijzigen van een Service Bus-Service Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) en [waarden certificaatverlener ophalen en sleutel van verlener](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Installeer de SDK en de BizTalk Service-project in Visual Studio maakt. |[Azure BizTalk Services SDK installeren](https://msdn.microsoft.com/library/azure/hh689760.aspx) en [uitgebreide berichten eindpunten in Azure maken](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Developer |Uw project met uw BizTalk Service wordt gehost op Azure BizTalk-Service implementeren. |[Implementeren en vernieuwen van het BizTalk Services-Project](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Beheerder |Als u van EDI gebruikmaakt geldt.  U kunt toevoegen van Partners en overeenkomsten maken op de Microsoft Azure BizTalk Services-Portal. Wanneer u een overeenkomst maakt, kunt u de brug en/of transformaties gemaakt door de ontwikkelaar in de instellingen van de overeenkomst kunt toevoegen. |[EDI en AS2, EDIFACT op BizTalk Services-Portal configureren](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Beheerder |Met de klassieke Azure Portal, de status van uw BizTalk Service, inclusief maatstaven voor prestaties bewaken. |[BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Beheerder |Met behulp van de Microsoft Azure BizTalk Services-Portal, de artefacten die wordt gebruikt door BizTalk Services en berichten bijhouden zoals ze worden verwerkt door de bestanden bridge beheren. |[Met behulp van de BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Beheerder |Maak een back-upplan back-up van de BizTalk Service. |[Zakelijke continuïteit en herstel na noodgevallen in BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Volgende stappen
[Zelfstudies en voorbeelden](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Het project in Visual Studio maken](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Azure BizTalk Services SDK installeren](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Concepten
[Het project in Visual Studio maken](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI AS2 en EDIFACT Messaging (Business-to-Business)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Meer informatie
[Bron en bestemming Bridge Messaging-eindpunten toevoegen](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Informatie over en het bericht Maps en transformaties maken](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Met behulp van de BizTalk Adapter Service (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)

