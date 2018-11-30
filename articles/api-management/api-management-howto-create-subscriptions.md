---
title: Over het maken van abonnementen in Azure API Management | Microsoft Docs
description: Informatie over het maken van abonnementen in Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 3f4e113125ec9644aac974e47996afe290e57cee
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621796"
---
# <a name="how-to-create-subscriptions-in-azure-api-management"></a>Over het maken van abonnementen in Azure API Management

Bij het publiceren van API's via Azure API Management (APIM), is de eenvoudigste en meest voorkomende manier voor het beveiligen van toegang tot deze API's met behulp van Abonnementssleutels. Met andere woorden, moeten clienttoepassingen die gebruikmaken van de gepubliceerde API's moeten een geldige Subscription-Key opnemen in HTTP-aanvragen bij het maken van deze API's aanroepen. Als u wilt een Abonnementssleutel voor toegang tot API's, is een abonnement vereist. Zie voor meer informatie over abonnementen [abonnementen in Azure API Management](api-management-subscriptions.md)

In dit artikel worden de stappen voor het maken van abonnementen in Azure portal.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in dit artikel hebt voltooid, moet u naar:

+ [Een APIM-instantie maken](get-started-create-service-instance.md)
+ Inzicht in [abonnementen in APIM](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Maak een nieuw abonnement

1. Klik op **abonnementen** in het menu aan de linkerkant
2. Klik op **abonnement toevoegen**
3. Geef een naam op van het abonnement en selecteert u het bereik
4. Klik op **Opslaan**.

![Flexibele abonnementen](./media/api-management-subscriptions/flexible-subscription.png)

Nadat het abonnement is gemaakt, wordt een combinatie van API-sleutel (primaire en secundaire) worden ingericht voor toegang tot de API's.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over API Management:

+ Informatie over andere [concepten](api-management-terminology.md) in API Management
+ Volg onze [zelfstudies](import-and-publish.md) voor meer informatie over API Management
+ Controleer onze [pagina met veelgestelde vragen](api-management-faq.md) voor veelgestelde vragen