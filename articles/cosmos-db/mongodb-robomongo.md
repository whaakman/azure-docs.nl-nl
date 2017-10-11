---
title: Robomongo gebruiken voor Azure Cosmos DB | Microsoft Docs
description: 'Informatie over het gebruik van Robomongo met een Cosmos Azure DB: API voor MongoDB-account'
keywords: robomongo
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 8983594776a1bbe413a6d7cf2cd518f0e327648a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Robomongo gebruiken met een Cosmos Azure DB: API voor MongoDB-account
Verbinding maken met een Cosmos Azure DB: de API voor Robomongo met MongoDB-account, moet u:

* Download en installeer [Robomongo](https://robomongo.org/)
* Uw Azure-Cosmos-DB hebben:-API voor MongoDB account [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="connect-using-robomongo"></a>Verbinding maken met behulp van Robomongo
Toevoegen van uw Azure-Cosmos-DB: API voor MongoDB-account voor de Robomongo MongoDB-verbindingen, de volgende stappen uitvoeren.

1. Ophalen van de Cosmos Azure DB:-API voor MongoDB verbinding accountgegevens van de instructies [hier](connect-mongodb-account.md).

    ![Schermopname van de connection string-blade](./media/mongodb-robomongo/connectionstringblade.png)
2. Voer *Robomongo.exe*

3. Klik op de verbindingsknop onder **bestand** voor het beheren van uw verbindingen. Klik vervolgens op **maken** in de **MongoDB verbindingen** venster wordt geopend de **verbindingsinstellingen** venster.

4. In de **verbindingsinstellingen** venster, kies een naam. Zoek vervolgens de **Host** en **poort** van de verbindingsinformatie in stap 1 en voert u deze in **adres** en **poort**respectievelijk.

    ![Schermopname van de verbindingen Robomongo beheren](./media/mongodb-robomongo/manageconnections.png)
5. Op de **verificatie** tabblad **verificatie uitvoeren**. Voer vervolgens de Database (standaardwaarde is *Admin*), **gebruikersnaam** en **wachtwoord**.
Beide **gebruikersnaam** en **wachtwoord** vindt u in de verbindingsinformatie in stap 1.

    ![Schermopname van het tabblad Robomongo verificatie](./media/mongodb-robomongo/authentication.png)
6. Op de **SSL** tabblad selectievakje **Gebruik SSL-protocol**, wijzigt u vervolgens de **verificatiemethode** naar **zelfondertekende certificaten**.

    ![Schermopname van het SSL-tabblad Robomongo](./media/mongodb-robomongo/SSL.png)
7. Tot slot op **Test** om te controleren of u verbinding maken, klikt u vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen
* Verkennen Azure Cosmos DB: API voor MongoDB [voorbeelden](mongodb-samples.md).
