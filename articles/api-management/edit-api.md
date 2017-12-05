---
title: Bewerken van een API met de Azure portal | Microsoft Docs
description: Deze zelfstudie ziet u het gebruik van API Management (APIM) voor het bewerken van een API.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 362c36181da706e3fe0a27cc5ab262271c2a1e57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="edit-an-api"></a>Een API bewerken

De stappen in deze zelfstudie ziet u het gebruik van API Management (APIM) voor het bewerken van een API. 

+ U kunt dit doen door toevoegen, verwijderen, wijzigen van bewerkingen in het exemplaar APIM. 
+ U kunt uw API swagger bewerken.

## <a name="prerequisites"></a>Vereisten

+ [Exemplaar van Azure API Management maken](get-started-create-service-instance.md)
+ [Importeren en publiceren van uw eerste API](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Een API in APIM bewerken

![Een api bewerken](./media/edit-api/edit-api001.png)

1. Klik op de **API's** tabblad.
2. Selecteer een van de API's die u eerder hebt geïmporteerd.
3. Selecteer de **ontwerp** tabblad.
4. Selecteer een bewerking die u wilt bewerken.
5. Wijzig de naam van de bewerking, selecteer een **pen** in de **Frontend** venster.

## <a name="update-the-swagger"></a>Bijwerken van de swagger

U kunt uw backbend API van de Azure-portal bijwerken met de volgende stappen:

1. Selecteer **alle bewerkingen**
2. Klik op pen in de **Frontend** venster.

    ![Een api bewerken](./media/edit-api/edit-api002.png)

    Uw API swagger wordt weergegeven.

    ![Een api bewerken](./media/edit-api/edit-api003.png)

3. De swagger bijwerken.
4. Druk op **opslaan**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voorbeelden van APIM beleid](policy-samples.md)
> [transformeren en een gepubliceerde API beveiligen](transform-api.md)