---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 30b5ae499d29b8b78b5852074362841ac1ceb49f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134103"
---
1. Meld u aan bij [Azure Portal].
2. Selecteer **+ nieuw** > **Web en mobiel** > **mobiele App**, en geef vervolgens een naam voor uw Mobile Apps-back-end.
3. Voor **resourcegroep**, selecteer een bestaande resourcegroep of maak een nieuwe (met behulp van dezelfde naam als uw app). 
4. Voor **App Service-plan**, het standaardabonnement (in de [Standard-laag](https://azure.microsoft.com/pricing/details/app-service/)) is geselecteerd. U kunt ook een ander abonnement selecteren of [een nieuw abonnement maken](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   Instellingen voor de App Service-plan bepalen de [locatie, functies, kosten en rekenresources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. Voor meer informatie over App Service-abonnementen en hoe u een nieuw plan maken in een andere prijscategorie laag en in de gewenste locatie, Zie [gedetailleerd overzicht van Azure App Service-plannen](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
5. Selecteer **Maken**. Deze stap maakt u de back-end voor mobiele Apps. 
6. In de **instellingen** deelvenster voor het nieuwe Mobile Apps-back-end, selecteer **Quick start** > uw clientplatform app > **verbinding maken met een database**. 
   
   ![Selecties voor het verbinden van een database](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. In de **gegevensverbinding toevoegen** venster **SQL-Database** > **een nieuwe database maken**. Voer de databasenaam in, kies een prijscategorie en selecteer vervolgens **Server**. U kunt deze nieuwe database opnieuw gebruiken. Als u al een database op dezelfde locatie hebt, kunt u in plaats daarvan **Een bestaande database gebruiken** kiezen. Het gebruik van een database in een andere locatie, vanwege bandbreedtekosten en hogere latentie wordt niet aanbevolen.
   
   ![Een database selecteren](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. In de **nieuwe server** deelvenster, voer een unieke naam in de **servernaam** Geef een gebruikersnaam en wachtwoord, selecteer **kunnen Azure-services tot server**, en selecteer  **OK**. Deze stap maakt u de nieuwe database.
9. Klik in de **gegevensverbinding toevoegen** venster **Connection string**, voer de waarden bij en het wachtwoord voor uw database en selecteer **OK**. 

   Wacht een paar minuten voor de database is geïmplementeerd voordat u doorgaat.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
