---
title: Aanmelden voor de Tekstanalyse-API
titleSuffix: Azure Cognitive Services
description: Instructies voor hoe u zich registreert voor het gebruik van tekstanalyse en gebruik binnen limieten.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: heidist
ms.openlocfilehash: 1461682e01d25e1b076ec2be130b9c3844aa0884
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208360"
---
# <a name="how-to-sign-up-for-the-text-analytics-api"></a>Hoe u zich registreert voor de Tekstanalyse-API

Text Analytics-resources zijn 24-7 beschikbaar in de cloud. Voordat u uw inhoud kunt uploaden voor analyse, moet u zich registreren om een toegangssleutel te verkrijgen. Voor elke aanroep naar de API is een toegangssleutel in de aanvraag vereist.

+ Begin met een Azure-abonnement. U kunt een [gratis account](https://azure.microsoft.com/free/) maken als u kosteloos wilt experimenteren.

+ Maak een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) door de **Text Analytics-API** te kiezen. De sleutel wordt gegenereerd wanneer u zich registreert.

Voor Text Analytics is er een gratis laag voor verkenning en evaluatie, en zijn er factureerbare lagen voor productieworkloads. U kunt meerdere registraties hebben in elk abonnement: een gratis laag, één betaalde laag, enzovoort. U kunt overschakelen naar een laag die meer transacties aanbiedt als uw aanvraagvolume toeneemt.

Er is geen serviceovereenkomst voor services in de Preview- of gratis laag. Voor meer informatie raadpleegt u [SLA voor Cognitive Services](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/)

## <a name="how-to-change-tiers"></a>Lagen wijzigen

Begin met een gratis laag en schakel vervolgens over op een factureerbare laag voor productieworkloads. Standaardfacturering wordt aangeboden op gestaffelde niveaus. U kunt tussen lagen schakelen met behoud van hetzelfde eindpunt en toegangssleutels.

1. Registreer u bij [Azure Portal](https://portal.azure.com) en [zoek uw service](text-analytics-how-to-access-key.md).

2. Klik op **Prijscategorie**.

   ![Prijscategorieopdracht in het linkernavigatiemenu](../media/portal-pricing-tier.png)

3. Kies een categorie en klik op **Selecteren**.  De nieuwe limieten gaan van kracht zodra de selectie wordt verwerkt. 

   ![Tegels en de knop Selecteren op de pagina voor het selecteren van de prijscategorie](../media/portal-choose-tier.png)

## <a name="how-billing-works"></a>Werking van facturering

Facturering is gebaseerd op het aantal transacties. U kunt een blok transacties in een specifieke prijscategorie kopen in een maandelijkse factureringscyclus. Als u daar overheen gaat, wordt er per transactie een klein overschrijdingsbedrag in rekening gebracht. Als u de maximumlimiet regelmatig overschrijdt, kunt u misschien beter overschakelen naar een hogere laag.

Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) voor meer informatie.

### <a name="what-constitutes-a-transaction-in-the-text-analytics-api"></a>Waaruit bestaat een transactie in de Text Analytics-API?
Elke aantekening bij een document telt als een transactie. Bij de batchscore-aanroepen wordt ook rekening gehouden met het aantal documenten waarvoor een score moet worden bepaald in die transactie. Als er dus bijvoorbeeld in één API-aanroep 1000 documenten worden verzonden voor sentimentanalyse, telt dit voor 1000 transacties.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een toegangssleutel opvragen](text-analytics-how-to-access-key.md)
