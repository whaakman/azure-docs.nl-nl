---
title: Azure Data Lake Analytics beheren met behulp van de Azure-portal
description: In dit artikel wordt beschreven hoe de Azure portal gebruiken voor het beheren van Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 1ccd4dd6b8d4ee15b7d9f14e7436ccd87392121e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623703"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Data Lake Analytics met Azure portal beheren
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dit artikel wordt beschreven hoe u Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken beheren via de Azure-portal.


<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics-accounts beheren

### <a name="create-an-account"></a>Een account maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **maken van een resource** > **Intelligence en analyse** > **Data Lake Analytics**.
3. Selecteer waarden voor de volgende items: 
   1. **Naam**: de naam van de Data Lake Analytics-account.
   2. **Abonnement**: de Azure-abonnement gebruikt voor het account.
   3. **Resourcegroep**: de Azure-resourcegroep in te maken van het account. 
   4. **Locatie**: de Azure-datacenter voor het Data Lake Analytics-account. 
   5. **Data Lake Store**: het standaardarchief moet worden gebruikt voor het Data Lake Analytics-account. De Azure Data Lake Store-account en de Data Lake Analytics-account, moet op dezelfde locatie bevinden.
4. Klik op **Create**. 

### <a name="delete-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account verwijderen

Voordat u een Data Lake Analytics-account verwijdert, verwijdert u de Data Lake Store-standaardaccount.

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Verwijderen**.
3. Typ de accountnaam.
4. Klik op **Verwijderen**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Gegevensbronnen beheren

Data Lake Analytics ondersteunt de volgende gegevensbronnen:

* Data Lake Store
* Azure Storage

U kunt Data Explorer gebruiken om te bladeren gegevensbronnen en basic bestand beheerbewerkingen uitvoeren. 

### <a name="add-a-data-source"></a>Een gegevensbron toevoegen

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **gegevensbronnen**.
3. Klik op **gegevensbron toevoegen**.
    
   * Als u wilt een Data Lake Store-account toevoegt, moet u de accountnaam en de toegang tot het account te kunnen deze query.
   * Als u wilt toevoegen in Azure Blob-opslag, moet u het opslagaccount en de accountsleutel. Als u wilt vinden, gaat u naar het opslagaccount in de portal.

## <a name="set-up-firewall-rules"></a>Firewallregels instellen

U kunt Data Lake Analytics verdere vergrendelen toegang tot uw Data Lake Analytics-account op het niveau. U kunt een firewall inschakelen, Geef een IP-adres of een IP-adresbereik voor de vertrouwde clients te definiëren. Nadat u deze maatregelen hebt ingeschakeld, worden alleen clients waarvoor de IP-adressen binnen het gedefinieerde bereik kunnen verbinden met de store.

Als andere Azure-services, zoals Azure Data Factory- of virtuele machines, verbinding met de Data Lake Analytics-account, zorg ervoor dat **Azure-Services toestaan** is ingeschakeld **op**. 

### <a name="set-up-a-firewall-rule"></a>Een firewallregel instellen

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op het menu aan de linkerkant **Firewall**.

## <a name="add-a-new-user"></a>Een nieuwe gebruiker toevoegen

U kunt de **Wizard gebruiker toevoegen** eenvoudig inrichten van nieuwe Data Lake-gebruikers.

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Aan de linkerkant onder **aan de slag**, klikt u op **Wizard gebruiker toevoegen**.
3. Selecteer een gebruiker en klik vervolgens op **Selecteer**.
4. Selecteer een rol en klik vervolgens op **Selecteer**. Als u een nieuwe ontwikkelaar gebruik van Azure Data Lake instelt, selecteert u de **Data Lake Analytics Developer** rol.
5. Selecteer de toegangsbeheerlijsten (ACL's) voor de U-SQL-databases. Wanneer u tevreden met uw keuzes bent, klikt u op **Selecteer**.
6. Selecteer de ACL's voor bestanden. Voor het standaardarchief niet te wijzigen de ACL's voor de hoofdmap (/) en voor de map/System. Klik op **Selecteren**.
7. Controleer de geselecteerde wijzigingen en klik vervolgens op **uitvoeren**.
8. Wanneer de wizard voltooid is, klikt u op **gedaan**.

## <a name="manage-role-based-access-control"></a>Toegangsbeheer op basis van rollen beheren

Net als andere Azure-services, kunt u rollen gebaseerd toegangsbeheer (RBAC) om te bepalen hoe gebruikers interacteren met de service.

De standaard RBAC-rollen hebben de volgende mogelijkheden:
* **Eigenaar**: kunt verzenden van taken, taken bewaken, taken van elke gebruiker annuleren en het account configureren.
* **Inzender**: kunt verzenden van taken, taken bewaken, taken van elke gebruiker annuleren en het account configureren.
* **Lezer**: taken kunt bewaken.

Gebruik de rol van de Data Lake Analytics Developer zodat U-SQL-ontwikkelaars kunnen het gebruik van de Data Lake Analytics-service. U kunt de Data Lake Analytics Developer-rol:
* Verzenden van taken.
* Taakstatus en de voortgang van de taken die worden aangeboden door een gebruiker bewaken.
* Zie de U-SQL-scripts uit taken die worden aangeboden door een gebruiker.
* Alleen uw eigen taken annuleren.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Gebruikers of beveiligingsgroepen toevoegen aan een Data Lake Analytics-account

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **toegangsbeheer (IAM)** > **toevoegen**.
3. Selecteer een rol.
4. Een gebruiker toevoegen.
5. Klik op **OK**.

>[!NOTE]
>Als een gebruiker of een beveiligingsgroep moet om taken te verzenden, moeten deze ook toegang tot de store-account. Zie voor meer informatie [beveiliging van gegevens die zijn opgeslagen in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Taken beheren

### <a name="submit-a-job"></a>Verzenden van een taak

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.

2. Klik op **nieuwe taak**. Voor elke taak configureren:

    1. **Taaknaam**: de naam van de taak.
    2. **Prioriteit**: lagere getallen hebben een hogere prioriteit. Als u twee taken in de wachtrij staan, met de lagere prioriteitswaarde eerste wordt uitgevoerd.
    3. **Parallelle uitvoering**: het maximum aantal compute-processen te reserveren voor deze taak.

3. Klik op **Taak verzenden**.

### <a name="monitor-jobs"></a>Taken controleren

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **alle taken weergeven**. Een lijst met alle actieve en onlangs voltooide taken in het account wordt weergegeven.
3. Klik desgewenst op **Filter** voor hulp bij het vinden van de taken op **tijdsbereik**, **taaknaam**, en **auteur** waarden. 

### <a name="monitoring-pipeline-jobs"></a>Taken van de pijplijn bewaken
Taken die deel van een pijplijn uitmaken werken samen, meestal opeenvolgend, voor het uitvoeren van een specifiek scenario. U kunt bijvoorbeeld een pijplijn die schoongemaakt, pakt, transformaties, gebruik voor de klant insights aggregeert hebben. Pipeline-taken worden aangeduid met behulp van de eigenschap "Pipeline" wanneer de taak is verzonden. Taken gepland met ADF V2 hebben automatisch deze eigenschap is ingevuld. 

Een lijst met U-SQL-taken die deel van pijplijnen uitmaken weergeven: 

1. In de Azure portal, gaat u naar uw Data Lake Analytics-accounts.
2. Klik op **taak Insights**. Het tabblad 'Alle taken' worden opgehaald, bevat een overzicht van die wordt uitgevoerd, in de wachtrij en taken beëindigd.
3. Klik op de **pijplijn taken** tabblad. Een lijst met taken van de pijplijn wordt weergegeven samen met de samengevoegde statistieken voor elke pijplijn.

### <a name="monitoring-recurring-jobs"></a>Terugkerende taken bewaken
Een terugkerende taak is een die dezelfde bedrijfslogica heeft maar maakt gebruik van verschillende invoergegevens telkens wanneer deze wordt uitgevoerd. In het ideale geval moeten terugkerende taken altijd slagen en hebben relatief stabiel uitvoeringstijd; bewaking van deze problemen kunt Zorg ervoor dat de taak is in orde. Terugkerende taken worden aangeduid met de eigenschap "Recurrence". Taken gepland met ADF V2 hebben automatisch deze eigenschap is ingevuld.

Een lijst met U-SQL-taken die zijn terugkerende weergeven: 

1. In de Azure portal, gaat u naar uw Data Lake Analytics-accounts.
2. Klik op **taak Insights**. Het tabblad 'Alle taken' worden opgehaald, bevat een overzicht van die wordt uitgevoerd, in de wachtrij en taken beëindigd.
3. Klik op de **terugkerende taken** tabblad. Een lijst met terugkerende taken wordt samen met de samengevoegde statistieken voor elke terugkerende taak weergegeven.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Azure Data Lake Analytics beheren met beleidsregels](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-policies)
