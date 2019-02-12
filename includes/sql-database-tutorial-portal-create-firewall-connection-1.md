---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: ab31ee82e8035fe888fa70b5796aef2c2b2939b2
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008522"
---
## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/)

## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken

Een Azure SQL-database bestaat in een gedefinieerde set [reken- en opslagresources](../articles/sql-database/sql-database-service-tiers-dtu.md). De database werkt onder een [Azure-resourcegroep](../articles/azure-resource-manager/resource-group-overview.md) en een [logische Azure SQL Database-server](../articles/sql-database/sql-database-features.md).

Volg deze stappen om een lege SQL-database te maken.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

1. Selecteer op de pagina **Nieuw** de optie **Databases** > **SQL-database**.

   ![lege database maken](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

1. Typ of selecteer de volgende waarden in het deelvenster **SQL Database**:

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Databasenaam** | *yourDatabase* | Zie [Database-id's](/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. |
   | **Abonnement** | *yourSubscription*  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | *yourResourceGroup* | Zie [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Bron selecteren** | Lege database | Hiermee geeft u aan dat er een lege database moet worden gemaakt. |

   ![database maken](../articles/sql-database/media/sql-database-design-first-database/create-database.png)

   1. Selecteer **Server** om een server voor de nieuwe database te configureren. Typ of selecteer vervolgens de volgende waarden:

      | Instelling       | Voorgestelde waarde | Beschrijving |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. |
      | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen.|
      | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet uit ten minste 8 tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
      | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

      Kies **Selecteren**.

      ![database-server maken](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

   1. Selecteer **Prijscategorie** om de servicelaag, het aantal DTU's en de hoeveelheid opslag op te geven. Bekijk de opties voor de DTU's en opslag die voor elke servicelaag beschikbaar zijn.

      Als u de servicelaag, het aantal DTU's en de hoeveelheid opslagruimte hebt geselecteerd, selecteert u **Toepassen**.

   1. Voer een **sortering** in voor de lege database (gebruik de standaardwaarde in deze zelfstudie). Zie [Collations](/sql/t-sql/statements/collations) (Sorteringen) voor meer informatie over sorteringen.

1. Nu u het **SQL Database**-formulier hebt ingevuld, selecteert u **Maken** om de database te maken. Het uitvoeren van deze stap kan tot anderhalve minuut duren.

1. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

     ![melding](../articles/sql-database/media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Een firewallregel maken

Met de SQL-databaseservice wordt een firewall op serverniveau gemaakt waarmee wordt voorkomen dat externe toepassingen en hulpprogramma’s verbinding maken met de server of databases op de server. Volg deze stappen om een [firewallregel op serverniveau voor SQL Database](../articles/sql-database/sql-database-firewall-configure.md) te maken voor het IP-adres van uw client. Met behulp van dit proces is alleen externe connectiviteit via de firewall van de SQL-database voor uw IP-adres mogelijk.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met de Azure SQL Database-server, tenzij de beheerder poort 1433 openstelt.

1. Wanneer de implementatie is voltooid, kiest u **SQL Databases** in het menu aan de linkerkant. Selecteer vervolgens op de pagina **SQL Databases** de optie *yourDatabase*. De overzichtspagina voor de database wordt geopend, met de volledig gekwalificeerde **servernaam** (bijvoorbeeld *uwserver.database.windows.net*) en opties voor verdere configuratie.

1. Kopieer de volledig gekwalificeerde servernaam om in verdere stappen verbinding te maken met de server en de bijbehorende databases.

   ![servernaam](../articles/sql-database/media/sql-database-design-first-database/server-name.png)

1. Selecteer **Serverfirewall instellen** op de werkbalk. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

   ![serverfirewallregel](../articles/sql-database/media/sql-database-design-first-database/server-firewall-rule.png)

   1. Selecteer **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

   1. Kies **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

   1. Selecteer **OK** en sluit de pagina **Firewallinstellingen**.

Uw IP-adres heeft nu toegang via de firewall en u kunt nu verbinding maken met de SQL-databaseserver en de bijbehorende databases met behulp van SSMS of een ander hulpprogramma naar keuze. Gebruik het beheerdersaccount voor de server dat u eerder hebt gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL-databasefirewall standaard ingeschakeld. Kies **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.
