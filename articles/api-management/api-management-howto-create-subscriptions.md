---
title: Maken van abonnementen in Azure API Management | Microsoft Docs
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
ms.openlocfilehash: bc791fea1dfd184749e84cb7b7a912972c6a9f12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657604"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Maken van abonnementen in Azure API Management

Wanneer u API's via Azure API Management publiceert, is het eenvoudig en voor beveiligde toegang tot deze API's met behulp van abonnementssleutels. Clienttoepassingen die gebruikmaken van de gepubliceerde API's moeten moeten een geldig abonnement-sleutel opnemen in de HTTP-aanvragen wanneer ze deze API's aanroepen. Als u een abonnementssleutel voor toegang tot API's, is een abonnement vereist. Zie voor meer informatie over abonnementen [abonnementen in Azure API Management](api-management-subscriptions.md).

In dit artikel worden de stappen voor het maken van abonnementen in Azure portal.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, zijn de vereisten als volgt uit:

+ [Maak een exemplaar van API Management](get-started-create-service-instance.md).
+ Inzicht in [abonnementen in API Management](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Maak een nieuw abonnement

1. Selecteer **abonnementen** in het menu aan de linkerkant.
2. Selecteer **abonnement toevoegen**.
3. Geef een naam op van het abonnement en selecteert u het bereik.
4. (Optioneel) Kies als het abonnement gekoppeld aan een gebruiker worden moet.
5. Selecteer **Opslaan**.

![Flexibele abonnementen](./media/api-management-subscriptions/flexible-subscription.png)

Nadat u het abonnement hebt gemaakt, vindt u twee API-sleutels voor toegang tot de API's. Een sleutel is primaire en secundaire is. 

## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over API Management:

+ Informatie over andere [concepten](api-management-terminology.md) in API Management.
+ Volg onze [zelfstudies](import-and-publish.md) voor meer informatie over API Management.
+ Controleer onze [pagina met veelgestelde vragen](api-management-faq.md) voor veelgestelde vragen.