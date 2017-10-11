---
title: Beheren van een Azure DB die Cosmos-account via Azure Portal | Microsoft Docs
description: "Informatie over het beheren van uw Azure DB die Cosmos-account via Azure Portal. Een handleiding voor het gebruik van de Azure Portal weergeven, kopiëren, verwijderen en toegang tot accounts vinden."
keywords: Azure-Portal, documentdb, azure, Microsoft azure
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Het beheren van een Azure DB die Cosmos-account
Informatie over het globale consistentie instellen, in combinatie met sleutels en verwijderen van een Azure DB die Cosmos-account in de Azure-portal.

## <a id="consistency"></a>Azure Cosmos DB consistentie instellingen beheren
Het consistentieniveau van de juiste te selecteren, is afhankelijk van de semantiek van uw toepassing. U moet vertrouwd raken met de beschikbare consistentieniveaus in Azure Cosmos DB door te lezen [consistentieniveaus gebruiken om te maximaliseren, beschikbaarheid en prestaties in Azure Cosmos DB][consistency]. Azure Cosmos DB biedt consistentie, beschikbaarheid en prestaties garanties, op elk consistentieniveau beschikbaar voor het account van uw database. Configureren van uw databaseaccount met een sterke consistentie vereist dat uw gegevens beperkt aan één Azure-regio en niet algemeen beschikbaar. Anderzijds, de beperkte consistentieniveaus - gebonden veroudering, session of uiteindelijke inschakelen u een willekeurig aantal Azure-regio's koppelen aan uw databaseaccount. De volgende eenvoudige stappen ziet u hoe u selecteert het standaardniveau voor consistentie voor het account van uw database. 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>De standaardconsistentie voor een Azure DB die Cosmos-account opgeven
1. In de [Azure-portal](https://portal.azure.com/), toegang tot uw Azure DB die Cosmos-account.
2. Klik op de accountblade **consistentie standaard**.
3. In de **standaard consistentie** blade, selecteer de nieuwe consistentieniveau en klik op **opslaan**.
    ![Standaard consistentie sessie][5]

## <a id="keys"></a>Weergeven, kopiëren en opnieuw genereren toegangstoetsen
Wanneer u een Azure DB die Cosmos-account maakt, genereert de service twee master toegangstoetsen die kunnen worden gebruikt voor verificatie wanneer het account voor Azure Cosmos DB wordt geopend. Dankzij twee toegangssleutels, kunt u opnieuw genereren van de sleutels zonder onderbreking naar uw Azure DB die Cosmos-account Azure Cosmos DB. 

In de [Azure-portal](https://portal.azure.com/), toegang tot de **sleutels** blade vanuit het menu van de resource op de **Azure DB die Cosmos-account** blade weergeven, kopiëren en opnieuw genereren van de sneltoetsen die worden gebruikt voor toegang tot uw Azure DB die Cosmos-account.

![Azure Portal schermafdruk is de blade sleutels](./media/manage-account/keys.png)

> [!NOTE]
> De **sleutels** blade bevat ook de primaire en secundaire verbindingsreeksen die kunnen worden gebruikt voor verbinding met uw account uit de [hulpprogramma voor gegevensmigratie](import-data.md).
> 
> 

Alleen-lezen sleutels zijn ook beschikbaar in deze blade. Lees- en query's zijn alleen-lezen-bewerkingen tijdens maakt, verwijderen, en vervangt niet.

### <a name="copy-an-access-key-in-the-azure-portal"></a>Kopieer een toegangssleutel in de Azure Portal
Op de **sleutels** blade, klikt u op de **kopie** knop aan de rechterkant van de sleutel die u wilt kopiëren.

![Bekijken en kopiëren van een toegangssleutel in de Azure-Portal blade sleutels](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Toegangstoetsen genereren
Aan uw account voor Azure Cosmos DB regelmatig om te voorkomen dat uw verbindingen beter te beveiligen, moet u de toegangssleutels wijzigen. Twee toegangssleutels zijn zodat u verbindingen met de Azure DB die Cosmos-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereert onderhouden toegewezen.

> [!WARNING]
> Is van invloed op alle toepassingen die afhankelijk van de huidige sleutel zijn opnieuw genereren van uw toegangssleutels. Alle clients die gebruikmaken van de toegangssleutel voor toegang tot de Azure DB die Cosmos-account moeten worden bijgewerkt om de nieuwe sleutel te gebruiken.
> 
> 

Als u toepassingen of cloudservices met de Azure DB die Cosmos-account hebt, wordt de verbinding verbroken als u opnieuw sleutels genereert, tenzij u uw sleutels rouleert. De volgende stappen wordt beschreven hoe die zijn betrokken bij het implementeren van uw sleutels.

1. De toegangssleutel in uw toepassingscode om te verwijzen naar de secundaire toegangssleutel van het Azure DB die Cosmos-account bijwerken.
2. Opnieuw genereren van de primaire toegangssleutel voor uw Azure DB die Cosmos-account. In de [Azure Portal](https://portal.azure.com/), toegang tot uw Azure DB die Cosmos-account.
3. In de **Azure Cosmos-DB Account** blade, klikt u op **sleutels**.
4. Op de **sleutels** blade, klik op de knop opnieuw genereren en klik vervolgens op **Ok** om te bevestigen dat u wilt een nieuwe sleutel te genereren.
    ![Toegangstoetsen genereren](./media/manage-account/regenerate-keys.png)
5. Nadat u hebt gecontroleerd dat de nieuwe sleutel beschikbaar voor gebruik (ongeveer 5 minuten na het opnieuw genereren is), worden de toegangssleutel in uw toepassingscode om te verwijzen naar de nieuwe primaire toegangssleutel bijgewerkt.
6. Genereer de secundaire toegangssleutel opnieuw.
   
    ![Toegangstoetsen genereren](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Het kan enkele minuten duren voordat een zojuist gegenereerde sleutel kan worden gebruikt voor toegang tot uw Azure DB die Cosmos-account.
> 
> 

## <a name="get-the--connection-string"></a>De verbindingsreeks ophalen
Voor het ophalen van de verbindingsreeks, het volgende doen: 

1. In de [Azure-portal](https://portal.azure.com), toegang tot uw Azure DB die Cosmos-account.
2. Klik in het menu resource **sleutels**.
3. Klik op de **kopie** naast de **primaire verbindingsreeks** of **secundaire verbindingsreeks** vak. 

Als u de verbindingsreeks in de [hulpprogramma voor migratie van Azure Cosmos DB Database](import-data.md), naam van de database toevoegen aan het einde van de verbindingsreeks. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Een Azure DB die Cosmos-account verwijderen
Als u wilt verwijderen een Cosmos-DB Azure-account van de Azure Portal die u niet langer gebruikt, met de rechtermuisknop op de accountnaam en klik op **account verwijderen**.

![Het verwijderen van een Azure DB die Cosmos-account in de Azure Portal](./media/manage-account/deleteaccount.png)

1. In de [Azure-portal](https://portal.azure.com/), toegang tot de Azure DB die Cosmos-account dat u wilt verwijderen.
2. Op de **Azure Cosmos DB account** blade met de rechtermuisknop op het account en klik vervolgens op **Account verwijderen**. 
3. Typ de naam van de Azure DB die Cosmos om te bevestigen dat u wilt verwijderen van het account op de blade resulterende bevestiging.
4. Klik op de **verwijderen** knop.

![Het verwijderen van een Azure DB die Cosmos-account in de Azure Portal](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Volgende stappen
Meer informatie over hoe [aan de slag met uw account voor Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
