---
title: Uw toepassing - Custom Decision Service registreren
titlesuffix: Azure Cognitive Services
description: Een stapsgewijze handleiding over het registreren van een nieuwe app met Azure Custom Decision Service.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo
ms.openlocfilehash: 0771c417a526c17d1bef7b0d40405efd94333929
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55869990"
---
# <a name="register-your-application"></a>Uw toepassing registreren

Voor het gebruik van Custom Decision Service voor uw toepassing, moet u het registreren van de portal. Dit artikel wordt uitgelegd hoe.

1. Ga naar de [voorpagina](https://ds.microsoft.com/) van Custom Decision Service. Klik op het lint **mijn Portal**, zoals gemarkeerd in de afbeelding:

    ![De Portal](./media/portal.png)

    Als u nog niet bent aangemeld, de portal vraagt u zich aanmelden met uw [Microsoft-account](https://account.microsoft.com/account). Nadat u zich hebt aangemeld, wordt uw Microsoft-account in de portal weergegeven in de rechterbovenhoek van de pagina.

2. Voor het registreren van uw toepassing, klikt u op de **nieuwe App** knop.

3. Kies een App-ID voor uw toepassing in het dialoogvenster. Custom Decision Service moet een unieke ID voor elke toepassing. Als iemand anders heeft deze ID al in gebruik, wordt u gevraagd u aan een andere naam kiezen.

4. Een actie Set API opgeven. Deze instelling is een RSS- of Atom-feed die de inhoud beschikbaar voor uw toepassing Custom Decision Service communiceert. Voer een naam voor de feed en voer de URL waaruit deze in behandeling is genomen. Als u wilt deze stap later doen, klikt u op de **Feeds** knop en klik vervolgens op de **nieuwe feed** knop. Een voorbeeld waarin wordt gemaakt van een RSS-feed wordt later beschreven.

5. Voor het registreren van uw toepassing, selecteert u de **aangepaste App** het selectievakje in de linkerbenedenhoek. Voer een [verbindingsreeks](../../storage/common/storage-configure-connection-string.md) voor de Azure storage-account waar de gegevens van uw toepassing is geregistreerd. Zie voor meer informatie over het maken van een storage-account [maken, beheren of verwijderen van een storage-account](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Volgende stappen

* Ga aan de slag met het optimaliseren van [een webpagina](custom-decision-service-get-started-browser.md) of [een smartphone-app](custom-decision-service-get-started-app.md).
* Doorloop een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een meer diepgaande.
* Raadpleeg de [API-naslag](custom-decision-service-api-reference.md) voor meer informatie over de geleverde functionaliteit.
