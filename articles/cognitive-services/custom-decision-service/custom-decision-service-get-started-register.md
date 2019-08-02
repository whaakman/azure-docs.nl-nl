---
title: Uw toepassing registreren-Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Een stapsgewijze hand leiding voor het registreren van een nieuwe app met Azure Custom Decision Service.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo
ROBOTS: NOINDEX
ms.openlocfilehash: 4a8e4e91e768e3ad28ea9ff88c18515db4a40323
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704551"
---
# <a name="register-your-application"></a>Uw toepassing registreren

Als u Custom Decision Service voor uw toepassing wilt gebruiken, registreert u deze in de portal. In dit artikel wordt uitgelegd hoe.

1. Ga naar de [front page](https://portal.ds.microsoft.com/) van Custom decision service. Klik op het lint op **Mijn portal**, zoals is gemarkeerd in de afbeelding:

    ![Mijn portal](./media/portal.png)

    Als u nog niet bent aangemeld, wordt u door de portal gevraagd om u aan te melden met uw [Microsoft-account](https://account.microsoft.com/account). Nadat u zich hebt aangemeld, wordt in de Portal de Microsoft-account weer gegeven in de rechter bovenhoek van de pagina.

2. Als u uw toepassing wilt registreren, klikt u op de knop **nieuwe app** .

3. Kies in het dialoog venster een app-ID voor uw toepassing. Custom Decision Service vereist een unieke ID voor elke toepassing. Als iemand anders deze ID al heeft gebruikt, vraagt het systeem u een andere te kiezen.

4. Geef een Action set API op. Deze instelling is een RSS-of Atom-feed waarmee de beschik bare inhoud voor uw toepassing wordt gecommuniceerd naar Custom Decision Service. Voer een naam in voor de feed en voer de URL in van waaruit deze wordt geleverd. Als u deze stap later wilt uitvoeren, klikt u op de knop **feeds** en vervolgens op de knop **nieuwe feed** . Een voor beeld van het maken van een RSS-feed wordt later beschreven.

5. Als u uw toepassing wilt registreren, schakelt u het selectie vakje **aangepaste app** in de linkerbenedenhoek in. Voer een [Connection String](../../storage/common/storage-configure-connection-string.md) in voor het Azure Storage-account waar uw toepassings gegevens worden geregistreerd. Zie [een opslag account maken, beheren of verwijderen](../../storage/common/storage-create-storage-account.md)voor meer informatie over het maken van een opslag account.

### <a name="next-steps"></a>Volgende stappen

* Ga aan de slag met het optimaliseren van [een webpagina](custom-decision-service-get-started-browser.md) of [een smartphone-app](custom-decision-service-get-started-app.md).
* Gebruik een [zelf studie](custom-decision-service-tutorial-news.md) voor een uitgebreidere voor beeld.
* Raadpleeg de [API-naslag](custom-decision-service-api-reference.md) voor meer informatie over de geleverde functionaliteit.
