---
title: Verbinding maken met andere services bij toezicht houden op inhoud - Content Moderator
titlesuffix: Azure Cognitive Services
description: Leer hoe u toegang tot andere API's voor uw Content Moderator-werkstromen met behulp van connectors.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 3ee582e2541e4eb55e5ea1424782df132ecf3575
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58116551"
---
# <a name="connect-to-other-cognitive-services"></a>Verbinding maken met andere cognitive services

Azure Content Moderator-werkstromen kunnen andere API's, naast de Content Moderator API's gebruiken. U toegang tot andere API's met behulp van een connector in Content Moderator. De connector biedt een koppeling naar de andere API's.

Content Moderator bevat deze connectors standaard:

* Emotion-API
* Face-API
* PhotoDNA Cloud Service

![Content Moderator beschikbare connectors](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Uw referenties controleren 

Voordat u een werkstroom hebt gedefinieerd, moet u controleren dat u hebt geldige referenties voor de connector-API die u wilt gebruiken:

1. Selecteer op het beoordelingsprogramma Dashboard **instellingen** > **Connectors**.

   ![Content Moderator Selecteer Connectors](images/connectors-2.png)

2. Selecteer de **bewerken** symbool naast de connector die u wilt controleren of de referenties voor.

   ![Content Moderator selecteert u het symbool bewerken](images/connectors-3.png)

3. De abonnementssleutel wordt weergegeven. Als u wijzigingen aanbrengt, selecteert u **opslaan** wanneer u klaar bent.

   ![Pagina met Content Moderator bewerken Connectors](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Een connector toevoegen

1. Voordat u een connector toevoegt, moet u een abonnementssleutel. Selecteer op het beoordelingsprogramma Dashboard **instellingen** > **referenties**. Selecteer en kopieer de waarde in de **Ocp-Admin-Subscription-Key** vak.

2. Selecteer **Connectors**. Selecteer een van de beschikbare connectors die worden weergegeven op het beoordelingsprogramma Dashboard. Selecteer **Connect**. 

   ![Content Moderator toevoegen Connector-pagina](images/connectors-5.png)

3. In de **Ocp-Admin-Subscription-Key** vak, plak de sleutel die u hebt gekopieerd. Selecteer vervolgens **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

* Informatie over het gebruik van connectors voor [definiëren van aangepaste werkstromen](workflows.md).
