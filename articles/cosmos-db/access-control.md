---
title: Instellen van toegangsbeheer in Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van toegangsbeheer in Azure Cosmos DB.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 740f1ca560207ada95dd03fbecc7f9940ee7b2a0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Toegangsbeheer in Azure Cosmos-DB

Om toe te voegen Azure Cosmos DB account leestoegang hebben tot uw gebruikersaccount, hebt u de eigenaar van een abonnement dat de volgende stappen uitvoeren in de Azure portal.

1. De Azure portal openen en selecteert u uw Azure DB die Cosmos-account.
2. Klik op de **toegangsbeheer (IAM)** tabblad en klik vervolgens op **+ toevoegen**.
3. In de **machtigingen toevoegen** deelvenster, in de **rol** de optie **Account Cosmos-lezer databaserol**.
4. In de **toegang toewijzen aan vak**, selecteer **Azure AD-gebruiker, groep of toepassing**.
5. Selecteer de gebruiker, groep of toepassing in uw directory waaraan u toegang wilt verlenen.  U kunt de map met de weergavenaam, e-mailadres of object-id's op te zoeken.
    De geselecteerde gebruiker, groep of toepassing wordt weergegeven in de lijst geselecteerde leden.
6. Klik op **Opslaan**.

De entiteit kan nu Azure Cosmos DB bronnen lezen.
