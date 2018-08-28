---
title: Azure Data Lake Analytics beheren met behulp van Azure portal
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het beheren van Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 1d49403ec04b2ec35291869385c316cb5ab3b0da
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044905"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Data Lake Analytics met Azure portal beheren
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

In dit artikel wordt beschreven hoe u Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken beheren met behulp van de Azure-portal.


<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics-accounts beheren

### <a name="create-an-account"></a>Een account maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **een resource maken** > **Intelligence en analyse** > **Data Lake Analytics**.
3. Selecteer waarden voor de volgende items: 
   1. **Naam**: de naam van de Data Lake Analytics-account.
   2. **Abonnement**: het Azure-abonnement gebruikt voor het account.
   3. **Resourcegroep**: de Azure-resourcegroep waarin u wilt maken van het account. 
   4. **Locatie**: de Azure-datacenter voor het Data Lake Analytics-account. 
   5. **Data Lake Store**: het standaardarchief van moet worden gebruikt voor het Data Lake Analytics-account. Het Azure Data Lake Store-account en de Data Lake Analytics-account moeten zich op dezelfde locatie.
4. Klik op **Create**. 

### <a name="delete-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account verwijderen

Voordat u een Data Lake Analytics-account verwijdert, verwijdert u de Data Lake Store-standaardaccount.

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **Verwijderen**.
3. Typ de naam van het account.
4. Klik op **Verwijderen**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Gegevensbronnen beheren

Data Lake Analytics ondersteunt de volgende gegevensbronnen:

* Data Lake Store
* Azure Storage

U kunt Data Explorer gebruiken om te bladeren in gegevensbronnen en beheerbewerkingen basisopdrachten voor bestandsbeheer. 

### <a name="add-a-data-source"></a>Een gegevensbron toevoegen

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **gegevensbronnen**.
3. Klik op **gegevensbron toevoegen**.
    
   * Als u wilt toevoegen van een Data Lake Store-account, moet u de accountnaam en de toegang tot het account moet op deze een query uitgevoerd.
   * Als u wilt toevoegen in Azure Blob-opslag, moet u het opslagaccount en de accountsleutel. Als u wilt vinden, gaat u naar het opslagaccount in de portal.

## <a name="set-up-firewall-rules"></a>Firewallregels instellen

U kunt Data Lake Analytics verdere vergrendelen van toegang aan uw Data Lake Analytics-account op het niveau van het netwerk. U kunt een firewall inschakelen, Geef een IP-adres of een IP-adresbereik voor de vertrouwde clients definiëren. Nadat u deze maatregelen hebt ingeschakeld, worden alleen clients met de IP-adressen binnen het gedefinieerde bereik kunnen verbinden met de store.

Als andere Azure-services zoals Azure Data Factory of virtuele machines, verbinding met de Data Lake Analytics-account, zorg ervoor dat **Azure-Services toestaan** is ingeschakeld **op**. 

### <a name="set-up-a-firewall-rule"></a>Een firewallregel instellen

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op het menu aan de linkerkant, **Firewall**.

## <a name="add-a-new-user"></a>Een nieuwe gebruiker toevoegen

U kunt de **Wizard gebruiker toevoegen** voor het eenvoudig inrichten van nieuwe Data Lake-gebruikers.

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Aan de linkerkant, onder **aan de slag**, klikt u op **Wizard gebruiker toevoegen**.
3. Selecteer een gebruiker en klik vervolgens op **Selecteer**.
4. Selecteer een rol en klik vervolgens op **Selecteer**. Als u een nieuwe ontwikkelaar instelt voor het gebruik van Azure Data Lake, selecteer de **Data Lake Analytics-ontwikkelaar** rol.
5. Selecteer de toegangsbeheerlijsten (ACL's) voor de U-SQL-databases. Wanneer u tevreden met uw keuzes bent, klikt u op **Selecteer**.
6. Selecteer de ACL's voor bestanden. Voor het standaardarchief niet te wijzigen de ACL's voor de hoofdmap '/' en voor de map/System. Klik op **Selecteren**.
7. Controleer uw geselecteerde wijzigingen, en klik vervolgens op **uitvoeren**.
8. Wanneer de wizard voltooid is, klikt u op **gedaan**.

## <a name="manage-role-based-access-control"></a>Toegangsbeheer op basis van rollen beheren

Net als andere Azure-services, kunt u rollen gebaseerd toegangsbeheer (RBAC) om te bepalen hoe gebruikers interacteren met de service.

De standaard RBAC-rollen hebben de volgende mogelijkheden:
* **De eigenaar van**: kunt verzenden van taken, taken controleren, annuleren taken van elke gebruiker en het account configureren.
* **Inzender**: kunt verzenden van taken, taken controleren, annuleren taken van elke gebruiker en het account configureren.
* **Lezer**: taken kunt bewaken.

Gebruik de functie Data Lake Analytics-ontwikkelaar zodat U-SQL-ontwikkelaars kunnen gebruikmaken van de Data Lake Analytics-service. U kunt de rol van Data Lake Analytics-ontwikkelaar te gebruiken:
* Verzenden van taken.
* Monitor de status van taak en de voortgang van taken die worden ingediend door een gebruiker.
* Zie de U-SQL-scripts van taken die worden ingediend door een gebruiker.
* Alleen uw eigen taken annuleren.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Gebruikers of beveiligingsgroepen toevoegen aan een Data Lake Analytics-account

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **toegangsbeheer (IAM)** > **toevoegen**.
3. Selecteer een rol.
4. Een gebruiker toevoegen.
5. Klik op **OK**.

>[!NOTE]
>Als een gebruiker of een beveiligingsgroep verzenden van taken moet, moeten ze ook zijn gemachtigd in de store-account. Zie voor meer informatie, [beveiligen van gegevens die zijn opgeslagen in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Taken beheren

### <a name="submit-a-job"></a>Een taak verzenden

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.

2. Klik op **nieuwe taak**. Voor elke taak configureren:

    1. **Taaknaam**: de naam van de taak.
    2. **Prioriteit**: lagere getallen een hogere prioriteit hebben. Als u twee taken in de wachtrij, met lagere prioriteit eerst wordt uitgevoerd.
    3. **Parallelle uitvoering**: het maximum aantal rekenprocessen te reserveren voor deze taak.

3. Klik op **Taak verzenden**.

### <a name="monitor-jobs"></a>Taken controleren

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-account.
2. Klik op **alle taken weergeven**. Een lijst van alle actieve en laatst voltooide taken in het account wordt weergegeven.
3. Klik desgewenst op **Filter** om te vinden van de taken op **tijdsbereik**, **taaknaam**, en **auteur** waarden. 

### <a name="monitoring-pipeline-jobs"></a>Taken van de pijplijn bewaken
Taken die deel van een pijplijn uitmaken werken samen, meestal sequentieel worden verwerkt om uit te voeren van een specifiek scenario. Bijvoorbeeld, kunt u een pijplijn die wist, worden geëxtraheerd, transformeert, aggregeert gebruik voor customer insights hebt. Pijplijninzichten worden aangeduid met behulp van de eigenschap "Pipeline" wanneer de taak is verzonden. Taken gepland met ADF V2 hebben automatisch deze eigenschap ingevuld. 

Een lijst met U-SQL-taken die deel van pijplijnen uitmaken weergeven: 

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-accounts.
2. Klik op **taak Insights**. Het tabblad 'Alle taken' worden opgehaald, waarin een lijst die wordt uitgevoerd, wordt weergegeven in de wachtrij geplaatst en taken beëindigd.
3. Klik op de **Pijplijninzichten** tabblad. Een lijst met taken van de pijplijn worden weergegeven, samen met samengevoegde statistieken voor elke pijplijn.

### <a name="monitoring-recurring-jobs"></a>Terugkerende taken bewaken
Een terugkerende taak is een die dezelfde zakelijke logica, maar maakt gebruik van verschillende invoergegevens telkens wanneer deze wordt uitgevoerd. In het ideale geval moeten terugkerende taken altijd slagen en hebben relatief stabiele uitvoeringstijd; bewaking van deze problemen kunnen Controleer of dat de taak is in orde. Terugkerende taken worden aangeduid met de eigenschap "Terugkeerpatroon". Taken gepland met ADF V2 hebben automatisch deze eigenschap ingevuld.

Een lijst met U-SQL-taken die worden periodiek weergeven: 

1. In de Azure-portal, gaat u naar uw Data Lake Analytics-accounts.
2. Klik op **taak Insights**. Het tabblad 'Alle taken' worden opgehaald, waarin een lijst die wordt uitgevoerd, wordt weergegeven in de wachtrij geplaatst en taken beëindigd.
3. Klik op de **terugkerende taken** tabblad. Een lijst van terugkerende taken worden weergegeven, samen met samengevoegde statistieken voor elke terugkerende taak.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Azure Data Lake Analytics met behulp van beleid beheren](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-policies)
