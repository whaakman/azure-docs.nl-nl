---
title: Beheer en ontwikkeling takenlijst in BizTalk Services | Microsoft Docs
description: Plannings- en taak steun voor het implementeren van Azure BizTalk Services.
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 6b8e0fea73dc24f7e680482be1f06fba6d702804
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916237"
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Beheer en ontwikkeling Task List in BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="getting-started"></a>Aan de slag
Als u werkt met Microsoft Azure BizTalk Services, zijn er verschillende on-premises en cloud-gebaseerde onderdelen om te overwegen. Als u wilt beginnen, houd rekening met het volgende proces dat plaatsvindt:  

| Stap | Wie er verantwoordelijk is | Taak | Verwante koppelingen |
| --- | --- | --- | --- |
| 1. |Beheerder |De Microsoft Azure-abonnement met behulp van een Microsoft-account of een organisatie-account maken |[Azure Portal](https://portal.azure.com) |
| 2. |Beheerder |Maken of een BizTalk Service inricht. |[Een BizTalk Service maken](/previous-versions/azure/reference/dn232347(v=azure.100)) |
| 3. |Beheerder |Registreer u of de implementatie van de BizTalk Services van uw bedrijf |[Registreren en bijwerken van een BizTalk Service-implementatie op de BizTalk Services-Portal](/previous-versions/azure/hh689837(v=azure.100)) |
| 4. |Beheerder |Is van toepassing als de toepassing gebruikmaakt van BizTalk Adapter Service verbinding maken met een Line-of-Business (LOB) on-premises systeem of een wachtrij of onderwerp doel gebruikt.  De Azure Service Bus-Namespace maken. Deze naamruimte, Service Bus-Verlenernaam en sleutel van Service Bus-verlener waarden geven aan de ontwikkelaar. |[Procedure: Maken of wijzigen van een Service Bus-Service Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) en [waarden voor naam verkrijgen en sleutel van verlener](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Installeer de SDK en de BizTalk Service-project maken in Visual Studio. |[Azure BizTalk Services SDK installeren](/previous-versions/azure/hh689760(v=azure.100)) en [eindpunten voor uitgebreide berichten maken in Azure](/previous-versions/azure/hh689766(v=azure.100)) |
| 6. |Developer |Implementeer uw BizTalk Service project naar uw BizTalk Service die wordt gehost op Azure. |[Implementatie en het vernieuwen van het BizTalk Services-Project](/previous-versions/azure/hh689881(v=azure.100)) |
| 7. |Beheerder |Is van toepassing als u van EDI gebruikmaakt.  U kunt toevoegen, Partners en overeenkomsten maken op de Microsoft Azure BizTalk Services-Portal. Wanneer u een overeenkomst maakt, kunt u de bridge en/of de transformaties die zijn gemaakt door de ontwikkelaar in de instellingen van de overeenkomst kunt toevoegen. |[EDI, AS2 en EDIFACT op BizTalk Services-Portal configureren](/previous-versions/azure/hh689853(v=azure.100)) |
| 8. |Beheerder |Met behulp van [REST](/previous-versions/azure/reference/dn232347(v=azure.100)), Controleer de status van uw BizTalk Service, met inbegrip van metrische gegevens voor prestaties. |[BizTalk Services: Tabbladen dashboard, bewaken en schalen](https://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Beheerder |Met behulp van de Microsoft Azure BizTalk Services-Portal, de artefacten die wordt gebruikt door BizTalk Services en -berichten bijhouden terwijl ze worden verwerkt door de bridge-bestanden beheren. |[De BizTalk Services-portal gebruiken](/previous-versions/azure/dn874043(v=azure.100)) |
| 10. |Beheerder |Maak een back-upplan back-up van de BizTalk Service. |[Zakelijke continuïteit en herstel na noodgevallen in BizTalk Services](/previous-versions/azure/dn509557(v=azure.100)) |

## <a name="next-steps"></a>Volgende stappen
[Zelfstudie en voorbeelden](/previous-versions/azure/hh689895(v=azure.100))

[Het project in Visual Studio maken](/previous-versions/azure/hh689811(v=azure.100))

[Install Azure BizTalk Services SDK](/previous-versions/azure/hh689760(v=azure.100))

## <a name="concepts"></a>Concepten
[Het project in Visual Studio maken](/previous-versions/azure/hh689811(v=azure.100))  
[EDI, AS2 en EDIFACT-berichten (Business to Business)](/previous-versions/azure/hh689898(v=azure.100))  

## <a name="other-resources"></a>Meer informatie
[Bron en bestemming Bridge Messaging eindpunten toevoegen](/previous-versions/azure/hh689877(v=azure.100))  
[Meer informatie en bericht Maps en transformaties te maken](/previous-versions/azure/hh689905(v=azure.100))  
[Met behulp van de BizTalk Adapterservice (BAS)](/previous-versions/azure/hh689889(v=azure.100))  
[Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=303664)

