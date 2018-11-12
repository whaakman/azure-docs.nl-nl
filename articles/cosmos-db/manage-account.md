---
title: Een Azure Cosmos DB-account via de Azure Portal beheren | Microsoft Docs
description: Informatie over het beheren van uw Azure Cosmos DB-account via de Azure Portal. Een handleiding over het gebruik van de Azure-Portal weergeven, kopiëren, verwijderen en toegang tot accounts zoeken.
keywords: Azure Portal, azure, Microsoft azure
services: cosmos-db
author: kirillg
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: kirillg
ms.openlocfilehash: abcf51c6bd196c2ffb0bb35e2df161531a53972d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229387"
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Een Azure Cosmos DB-account beheren
Informatie over het instellen van globale consistentie, werken met sleutels en verwijderen van een Azure Cosmos DB-account in Azure portal.

## <a id="consistency"></a>Azure Cosmos DB consistentie-instellingen beheren
Selecteren van de juiste consistentieniveau, is afhankelijk van de semantiek van uw toepassing. Raken met de beschikbare consistentieniveaus in Azure Cosmos DB door te lezen [consistentieniveaus voor een maximale beschikbaarheid en prestaties in Azure Cosmos DB met][consistency]. Azure Cosmos DB biedt consistentie, beschikbaarheid en prestatiegaranties, op elk consistentieniveau beschikbaar voor het account van uw database. Configureren van uw databaseaccount met een sterke consistentie is vereist dat uw gegevens beperkt naar één Azure-regio en niet algemeen beschikbaar is. Anderzijds, de beperkte consistentieniveaus - gebonden veroudering, sessie en uiteindelijk inschakelen u een willekeurig aantal Azure-regio's aan uw databaseaccount koppelen. De volgende eenvoudige stappen laten zien hoe u het standaardconsistentieniveau voor uw databaseaccount selecteren.

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>De standaardconsistentie voor een Azure Cosmos DB-account opgeven
1. In de [Azure-portal](https://portal.azure.com/), toegang tot uw Azure Cosmos DB-account.
2. Klik in de accountpagina op **Standaardconsistentie**.
3. In de **Standaardconsistentie** pagina, selecteert u de nieuwe consistentieniveau en op **opslaan**.
    ![Standaard consistentie sessie][5]

## <a id="keys"></a>Weergeven, kopiëren en opnieuw genereren van toegangssleutels en wachtwoorden
Wanneer u een Azure Cosmos DB-account maakt, de service genereert twee master toegangssleutels (of twee wachtwoorden voor accounts van de MongoDB-API) die kan worden gebruikt voor verificatie wanneer het Azure Cosmos DB-account wordt geopend. Door twee toegangssleutels, kunt Azure Cosmos DB u de sleutels zonder onderbreking naar uw Azure Cosmos DB-account opnieuw genereren. 

In de [Azure-portal](https://portal.azure.com/), toegang tot de **sleutels** pagina in het resourcemenu op de **Azure Cosmos DB-account** pagina weergeven, kopiëren en de toegang tot sleutels die gebruikt voor het worden opnieuw genereren toegang tot uw Azure Cosmos DB-account. Voor MongoDB-API-accounts toegang krijgen tot de **Connection String** pagina in het resourcemenu weergeven, kopiëren en opnieuw genereren van de wachtwoorden die worden gebruikt voor toegang tot uw account.

![Azure portal schermafbeelding, op de pagina sleutels](./media/manage-account/keys.png)

> [!NOTE]
> De **sleutels** pagina bevat tevens primaire en secundaire verbindingsreeksen die kunnen worden gebruikt om verbinding maken met uw account uit de [hulpprogramma voor gegevensmigratie](import-data.md).
> 
> 

Alleen-lezensleutels zijn ook beschikbaar op deze pagina. Leesbewerkingen en query's zijn alleen-lezen bewerkingen, terwijl wordt gemaakt, verwijderen, en vervangt niet.

### <a name="copy-an-access-key-or-password-in-the-azure-portal"></a>Een toegangssleutel of een wachtwoord in de Azure-portal kopiëren
Op de **sleutels** pagina (of **Connection string** pagina voor MongoDB-API-accounts), klikt u op de **kopie** knop aan de rechterkant van de sleutel of het wachtwoord die u wilt kopiëren.

![Een toegangssleutel in Azure Portal bekijken en kopiëren op de pagina Sleutels](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys-and-passwords"></a>Opnieuw genereren van toegangssleutels en wachtwoorden
U moet de toegangssleutels (en wachtwoorden wijzigen voor MongoDB-API-accounts) aan uw Azure Cosmos DB-account periodiek om ervoor te zorgen dat uw verbindingen beter te beveiligen. Twee sleutels/wachtwoorden voor toegang worden toegewezen om te kunnen beheren van verbindingen met de Azure Cosmos DB-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereren.

> [!WARNING]
> Is van invloed op alle toepassingen die afhankelijk van de huidige sleutel zijn opnieuw genereren van uw toegangssleutels. Alle clients die gebruikmaken van de toegangssleutel voor toegang tot de Azure Cosmos DB-account moeten worden bijgewerkt voor het gebruik van de nieuwe sleutel.
> 
> 

Als u toepassingen of cloudservices met behulp van de Azure Cosmos DB-account hebt, wordt u de verbinding verbroken als u opnieuw sleutels genereert, tenzij u uw sleutels rouleert. De volgende stappen beschrijven het proces die betrokken zijn bij het implementeren van uw sleutels/wachtwoorden.

1. De toegangssleutel in de code van uw toepassing om te verwijzen naar de secundaire toegangssleutel van het Azure Cosmos DB-account bijwerken.
2. Genereer de primaire toegangssleutel voor uw Azure Cosmos DB-account opnieuw. In de [Azure-portal](https://portal.azure.com/), toegang tot uw Azure Cosmos DB-account.
3. In de **Azure Cosmos DB-Account** pagina, klikt u op **sleutels** (of **Connection String** voor MongoDB-accounts **).
4. Op de **sleutels**/**Connection String** pagina, klikt u op de knop opnieuw genereren, en klik vervolgens op **Ok** te bevestigen dat u wilt om een nieuwe sleutel te genereren.
    ![Opnieuw genereren van toegangssleutels](./media/manage-account/regenerate-keys.png)
5. Nadat u hebt gecontroleerd dat de nieuwe sleutel beschikbaar voor gebruik (ongeveer vijf minuten na het opnieuw genereren is), moet u de toegangssleutel in de code van uw toepassing om te verwijzen naar de nieuwe primaire toegangssleutel bijwerken.
6. Genereer de secundaire toegangssleutel opnieuw.
   
    ![Opnieuw genereren van toegangssleutels](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Het kan enkele minuten duren voordat een zojuist gegenereerde sleutel kan worden gebruikt voor toegang tot uw Azure Cosmos DB-account.
> 
> 

## <a name="get-the-connection-string"></a>De verbindingsreeks ophalen
Als u wilt ophalen van de verbindingsreeks, het volgende doen: 

1. In de [Azure-portal](https://portal.azure.com), toegang tot uw Azure Cosmos DB-account.
2. Klik in het resourcemenu op **sleutels** (of **Connection String** voor MongoDB-API-accounts).
3. Klik op de **kopie** naast de **Primary Connection String** of **secundaire verbindingsreeks** vak. 

Als u de verbindingsreeks in de [hulpprogramma voor migratie van Azure Cosmos DB-Database](import-data.md), naam van de database toevoegen aan het einde van de verbindingsreeks. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a> Een Azure Cosmos DB-account verwijderen
Als u wilt verwijderen van een Azure Cosmos DB-account van de Azure portal die u niet langer gebruikt, met de rechtermuisknop op de accountnaam en klik op **-account verwijderen**.

![Het verwijderen van een Azure Cosmos DB-account in Azure portal](./media/manage-account/deleteaccount.png)

1. In de [Azure-portal](https://portal.azure.com/), toegang tot het Azure Cosmos DB-account dat u wilt verwijderen.
2. Op de **Azure Cosmos DB-account** pagina, met de rechtermuisknop op het account en klik vervolgens op **Account verwijderen**. 
3. Typ op de resulterende bevestigingspagina de naam van de Azure Cosmos DB-account om te bevestigen dat u wilt verwijderen van het account.
4. Klik op de **verwijderen** knop.

![Het verwijderen van een Azure Cosmos DB-account in Azure portal](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Volgende stappen
Meer informatie over het [aan de slag met uw Azure Cosmos DB-account](https://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
