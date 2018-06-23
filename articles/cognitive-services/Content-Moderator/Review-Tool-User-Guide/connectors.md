---
title: Connectors in Azure inhoud beheerder gebruiken voor toegang tot andere API | Microsoft Docs
description: Informatie over het openen van andere API's voor uw werkstromen inhoud beheerder met behulp van connectors.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: d8114457e7079ca8772cab830bd011dcddf372f5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344481"
---
# <a name="connectors"></a>Connectors

Azure Content beheerder werkstromen kunnen andere API's, naast inhoud beheerder API's gebruiken. U toegang tot andere API met behulp van een connector in inhoud beheerder. De connector bevat een koppeling naar de andere API's.

Inhoud beheerder omvat deze standaard connectors:

* Emotion-API
* Face-API
* PhotoDNA Cloudservice

![Inhoud van de beschikbare connectors beheerder](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Controleer uw referenties 

Voordat u een werkstroom definiëren, zorg ervoor dat u hebt geldige referenties voor de connector-API die u wilt gebruiken:

1.  Selecteer op het Dashboard van het hulpprogramma revisie **instellingen** > **Connectors**.

  ![Inhoud beheerder Selecteer Connectors](images/connectors-2.png)

2.  Selecteer de **bewerken** naast de connector die u wilt controleren of de referenties voor.

  ![Inhoud beheerder selecteert u het symbool bewerken](images/connectors-3.png)

3.  De abonnementssleutel wordt weergegeven. Als u wijzigingen aanbrengt, selecteert u **opslaan** wanneer u klaar bent.

  ![De pagina inhoud beheerder bewerken Connectors](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Toevoegen van een connector

1.  Voordat u een connector toevoegen, moet u een abonnementssleutel. Selecteer op het Dashboard van het hulpprogramma revisie **instellingen** > **referenties**. Selecteer en kopieer de waarde die is in de **Ocp-Admin-Subscription-Key** vak.

2.  Selecteer **Connectors**. Selecteer een van de beschikbare connectors die worden weergegeven op het hulpprogramma voor beoordeling Dashboard. Selecteer **Connect**. 

  ![De pagina inhoud beheerder toevoegen-Connector](images/connectors-5.png)

3.  In de **Ocp-Admin-Subscription-Key** vak, plak de sleutel die u hebt gekopieerd. Selecteer vervolgens **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

* Informatie over het gebruik van connectors [aangepaste werkstromen definiëren](workflows.md).
