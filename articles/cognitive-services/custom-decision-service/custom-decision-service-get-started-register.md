---
title: Uw toepassing - cognitieve Azure-Services registreren | Microsoft Docs
description: Een stapsgewijze handleiding voor het registreren van een nieuwe app met Azure aangepaste besluit Service
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo;alekh;marossi
ms.openlocfilehash: 2aa8fbe77c11df4434eefa4c92d8529d5ca1d885
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345452"
---
# <a name="register-your-application"></a>Uw toepassing registreren

Voor het gebruik van aangepaste besluit Service voor uw toepassing, moet u het registreren in de portal. Dit artikel wordt uitgelegd hoe.

1. Ga naar de [voorpagina](https://ds.microsoft.com/) van aangepaste besluit Service. Klik op het lint op **mijn Portal**, als gemarkeerd in de afbeelding:

    ![Mijn Portal](./media/portal.png)

    Als u nog niet bent aangemeld, de portal wordt u gevraagd om te ondertekenen met uw [Microsoft-account](https://account.microsoft.com/account). Nadat u zich hebt aangemeld, wordt uw Microsoft-account in de portal weergegeven in de rechterbovenhoek van de pagina.

2. Voor het registreren van uw toepassing, klikt u op de **nieuwe App** knop.

3. Kies een App-ID voor uw toepassing in het dialoogvenster. Aangepaste besluit Service vereist een unieke ID voor elke toepassing. Als iemand anders deze ID al heeft genomen, wordt u gevraagd u kiest een andere naam.

4. Geef een actie Set API. Deze instelling is een RSS- of Atom-feed die de inhoud beschikbaar voor uw toepassing met aangepaste besluit Service communiceert. Voer een naam voor de feed en voer de URL waaruit deze in behandeling is genomen. U kunt deze stap later doen, klikt u op de **Feeds** knop en klik vervolgens op de **nieuwe feed** knop. Een voorbeeld van een RSS-feed maakt wordt verderop beschreven.

5. Voor het registreren van uw toepassing, selecteer de **aangepaste App** selectievakje in de linkerbenedenhoek. Voer een [verbindingsreeks](../../storage/common/storage-configure-connection-string.md) voor de Azure-opslagaccount waarin de gegevens van uw toepassing wordt geregistreerd. Zie voor meer informatie over het maken van een opslagaccount [maken, beheren of verwijderen van een opslagaccount](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Volgende stappen

* Aan de slag te optimaliseren [een webpagina](custom-decision-service-get-started-browser.md) of [een smartphone-app](custom-decision-service-get-started-app.md).
* Werken via een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een uitgebreidere.
* Raadpleeg de [API-referentiemateriaal](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit voor opgegeven.