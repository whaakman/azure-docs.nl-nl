---
title: Azure Data Lake Analytics beheren met behulp van de Azure portal | Microsoft Docs
description: Informatie over het beheren van Data Lake Analytics acounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e49d1a0e0ccc6567d0a6841817667717ff5dba76
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a>Azure Data Lake Analytics beheren met behulp van de Azure-portal
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informatie over het beheren van Azure Data Lake Analytics-accounts, gegevensbronnen account, gebruikers en -taken via de Azure-portal. Klik op een tabblad boven aan de pagina overzicht management onderwerpen over het gebruik van andere hulpprogramma's.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics-accounts beheren

### <a name="create-an-account"></a>Een account maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** > **Intelligence + analyse** > **Data Lake Analytics**.
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

## <a name="manage-policies"></a>Beleid beheren

### <a name="account-level-policies"></a>Beleid account-niveau

Deze beleidsregels van toepassing op alle taken in een Data Lake Analytics-account.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximum aantal AUs in een Data Lake Analytics-account
Een beleid bepaalt het totale aantal Analytics-eenheden (AUs) uw Data Lake Analytics-account kunt gebruiken. De waarde is standaard ingesteld op 250. Bijvoorbeeld, als deze waarde is ingesteld op 250 AUs, u kunt één actieve taak hebben met 250 AUs toegewezen aan of 10 taken uitgevoerd met 25 AUs elke. Aanvullende taken die zijn ingediend in de wachtrij staan totdat de actieve taken zijn voltooid. Wanneer taken zijn voltooid, worden de AUs vrijgemaakt voor de taken in de wachtrij om uit te voeren.

Het aantal AUs voor uw Data Lake Analytics-account wijzigen:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **maximale AUs**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan de standaardwaarde (250) AUs, in de portal klikt u op **Help + ondersteuning** ondersteuning te vragen. Het aantal AUs beschikbaar in uw Data Lake Analytics-account kan worden verhoogd.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximum aantal taken dat tegelijkertijd kan worden uitgevoerd
Een beleid bepaalt hoeveel taken kunnen uitvoeren op hetzelfde moment. Deze waarde is standaard ingesteld op 20. Als uw Data Lake Analytics AUs beschikbaar heeft, worden nieuwe taken worden gepland onmiddellijk totdat de waarde van dit beleid wordt bereikt door het totale aantal actieve taken uitvoeren. Wanneer u het maximum aantal taken dat tegelijkertijd kan worden uitgevoerd bereikt, latere taken in de wachtrij staan in volgorde van prioriteit als een of meer actieve taken hebt voltooid (afhankelijk van de beschikbaarheid van Australië).

Wijzigen van het aantal taken dat tegelijkertijd kan worden uitgevoerd:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **Maximum aantal van een actief taken**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak. 
4. Klik op **Opslaan**.

> [!NOTE]
> Als u meer dan de standaardwaarde (20) aantal taken in de portal wordt uitgevoerd, klikt u op **Help + ondersteuning** ondersteuning te vragen. Het aantal taken dat tegelijkertijd kan worden uitgevoerd in uw Data Lake Analytics-account kan worden verhoogd.
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a>Hoe lang tot behouden taak metagegevens en -bronnen 
Wanneer uw gebruikers U-SQL-taken uitvoeren, behoudt de Data Lake Analytics-service alle gerelateerde bestanden. Verwante bestanden bevatten het U-SQL-script de dll-bestanden waarnaar wordt verwezen in de U-SQL-script, gecompileerde bronnen en statistieken. De bestanden zijn in de map /system/ van het Azure Data Lake Storage-standaardaccount. Dit beleid bepaalt hoe lang deze resources worden opgeslagen voordat ze automatisch worden verwijderd (de standaardwaarde is 30 dagen). U kunt deze bestanden gebruiken voor foutopsporing en prestaties afstemmen van taken die u in de toekomst opnieuw zult uitvoeren.

Wijzigen hoe lang om metagegevens van de taak en resources te houden:

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **dagen bewaren taak voert een query**, verplaats de schuifregelaar om een waarde te selecteren of Voer de waarde in het tekstvak.  
4. Klik op **Opslaan**.

### <a name="job-level-policies"></a>Beleid Job-niveau
U kunt de maximale AUs en de maximumprioriteit individuele gebruikers (of leden van bepaalde beveiligingsgroepen) kunnen worden ingesteld voor taken die ze verzenden beheren met beleid voor taak-niveau. Hiermee kunt u de kosten van gebruikers te beheren. U kunt ook beheer mogelijk hebt u het effect dat taken geplande hoge prioriteit productietaken die worden uitgevoerd in dezelfde Data Lake Analytics-account.

Data Lake Analytics heeft twee beleidsregels die u op het taakniveau van de instellen kunt:

* **Australië limiet is per taak**: gebruikers kunnen alleen taken die met dit aantal AUs indienen. Deze limiet is standaard hetzelfde zijn als de maximumlimiet Australië voor het account.
* **Prioriteit**: gebruikers kunnen alleen verzenden van taken met de prioriteit die lager is dan of gelijk zijn aan deze waarde. Houd er rekening mee dat een hoger getal een lagere prioriteit geeft. Standaard is dit ingesteld op 1, die de hoogst mogelijke prioriteit is.

Er is een standaardbeleid instellen voor elke account. Het standaardbeleid is van toepassing op alle gebruikers van het account. U kunt extra beleidsregels instellen voor specifieke gebruikers en groepen. 

> [!NOTE]
> Beleidsregels voor account-niveau en op jobniveau toepassing tegelijk.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Een beleid voor een specifieke gebruiker of groep toevoegen

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **taak verzending limieten**, klikt u op de **beleid toevoegen** knop. Vervolgens, selecteer of Voer de volgende instellingen:
    1. **De naam van beleid COMPUTE**: Voer een beleidsnaam van het doel van het beleid.
    2. **Gebruiker of groep selecteren**: Selecteer de gebruiker of groep die dit beleid is van toepassing op.
    3. **De limiet voor de taak Australië instellen**: limiet voor de instellen Australië die voor de geselecteerde gebruiker of groep geldt.
    4. **De limiet van de prioriteit instellen**: de limiet voor de prioriteit die voor de geselecteerde gebruiker of groep geldt instellen.

4. Klik op **OK**.

5. Het nieuwe beleid wordt weergegeven in de **standaard** beleid tabel onder **taak verzending limieten**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Verwijder of een bestaand beleid bewerken

1. Ga in de Azure-portal naar uw Data Lake Analytics-account.
2. Klik op **Eigenschappen**.
3. Onder **taak verzending limieten**, vinden van het beleid dat u wilt bewerken.
4.  Om te zien de **verwijderen** en **bewerken** opties in de rechterkolom van de tabel, klikt u op **...** .

### <a name="additional-resources-for-job-policies"></a>Aanvullende bronnen voor taak-beleid
* [Blogbericht beleid-overzicht](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Blogbericht beleid account-niveau](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Blogbericht beleid Job-niveau](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met behulp van de Azure-portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

