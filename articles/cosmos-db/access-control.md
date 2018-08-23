---
title: Toegangsbeheer instellen in Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van toegangsbeheer in Azure Cosmos DB.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: rafats
ms.openlocfilehash: 0d4595bcf8a9f009dce928d3f3cbc442328ec39b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057501"
---
# <a name="access-control-in-azure-cosmos-db"></a>Toegangsbeheer in Azure Cosmos DB

Toegang tot de lezer van de Azure Cosmos DB-account toevoegen aan uw gebruikersaccount, hebt u de eigenaar van een abonnement op de volgende stappen uitvoeren in Azure portal.

1. Open de Azure-portal en selecteer uw Azure Cosmos DB-account.
2. Klik op de **toegangsbeheer (IAM)** tabblad en klik vervolgens op **+ toevoegen**.
3. In de **machtigingen toevoegen** deelvenster in de **rol** Schakel **Cosmos DB Account Reader-rol**.
4. In de **toegang toewijzen tot vak**, selecteer **Azure AD-gebruiker, groep of toepassing**.
5. Selecteer de gebruiker, groep of toepassing in de map waarnaar u wilt om toegang te verlenen.  U kunt zoeken naar de map met de naam, e-mailadres of object-id's.
    De geselecteerde gebruiker, groep of toepassing wordt weergegeven in de lijst met geselecteerde leden.
6. Klik op **Opslaan**.

De entiteit kan nu lezen voor Azure Cosmos DB-resources.
