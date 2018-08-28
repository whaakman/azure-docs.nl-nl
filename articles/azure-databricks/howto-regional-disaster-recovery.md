---
title: Regionaal herstel na noodgevallen voor Azure Databricks
description: Dit artikel beschrijft een benadering tot het uitvoeren van herstel na noodgevallen in Azure Databricks.
services: azure-databricks
author: jasonwhowell
ms.author: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/27/2018
ms.openlocfilehash: 46cb9eaee1d56a96801065ae0a349aa0b1be85e0
ms.sourcegitcommit: baed5a8884cb998138787a6ecfff46de07b8473d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43115235"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Regionaal herstel na noodgevallen voor Azure Databricks-clusters

In dit artikel beschrijft een disaster recovery-architectuur geschikt voor Azure Databricks-clusters en de stappen om uit te voeren dat ontwerp.

## <a name="control-plan-architecture"></a>Besturingselement plan-architectuur

Op hoog niveau, wanneer u een Azure Databricks-werkruimte via Azure portal maakt een [beheerde apparaat](../managed-applications/overview.md) wordt geïmplementeerd als een Azure-resource in uw abonnement, in de gekozen Azure-regio (bijvoorbeeld VS-West). Dit apparaat is geïmplementeerd in een [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) met een [Network Security Group](../virtual-network/manage-network-security-group.md) en een Azure Storage-account, beschikbaar in uw abonnement. Het virtuele netwerk biedt niveau perimeterbeveiliging naar de Databricks-werkruimte en wordt beveiligd via de netwerkbeveiligingsgroep. U kunt de Databricks-cluster (s) door te geven van de werknemer en VM-type-stuurprogramma en Databricks-runtimeversie maken in de werkruimte. De persistente gegevens zijn beschikbaar in uw storage-account, Azure Blob Storage of Azure Data Lake Store. Zodra het cluster is gemaakt, kunt u taken uitvoeren via notebooks, REST-API's, ODBC-/ JDBC eindpunten door ze te koppelen aan een specifieke cluster.

De controlelaag Databricks beheert en bewaakt de omgeving van het Databricks-werkruimte. Een bewerking voor het beheer, zoals cluster maken vanuit de besturingselement vlak wordt gestart. Alle metagegevens, zoals geplande taken worden opgeslagen in een Azure-Database met geo-replicatie voor fouttolerantie.

![Databricks besturingselement vlak-architectuur](media/howto-regional-disaster-recovery/databricks-control-plane.png)

Een van de voordelen van deze architectuur is dat gebruikers verbinding maken met Azure Databricks met elke opslagbron in hun account. Een belangrijk voordeel is dat beide (Azure Databricks) compute en opslag onafhankelijk van elkaar kan worden geschaald.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Over het maken van een regionale disaster recovery-topologie

Zoals u in de bovenstaande beschrijving van de architectuur ziet, er zijn een aantal onderdelen die worden gebruikt voor een Big Data-pijplijn met Azure Databricks: Azure Storage, Azure-Database en andere gegevensbronnen. Azure Databricks is de *compute* pijplijn voor het Big Data. Het is *kortstondige* aard is, wat betekent dat terwijl uw gegevens nog steeds beschikbaar in Azure Storage, is de *compute* (Azure Databricks-cluster) kan worden beëindigd, zodat u niet hoeft te betalen voor compute wanneer u deze niet nodig. De *compute* (Azure Databricks) en opslagbronnen moeten zich in dezelfde regio bevinden zodat taken niet hoge latentie ondervindt.  

Voor het maken van de topologie van uw eigen regionale disaster recovery, de volgende vereisten voldoen:

   1. Inrichten van meerdere Azure Databricks-werkruimten in afzonderlijke Azure-regio's. Bijvoorbeeld, de primaire Azure Databricks-werkruimte maken in VS-Oost 2. De secundaire noodherstel Azure Databricks-werkruimte maken in een apart gebied, zoals VS-West.

   2. Gebruik [geografisch redundante opslag](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage). De gegevens die gekoppeld Azure Databricks standaard in Azure Storage opgeslagen is. De resultaten van de Databricks-taken worden ook opgeslagen in Azure Blob-opslag, zodat de verwerkte gegevens duurzaam en maximaal beschikbaar blijft nadat het cluster wordt beëindigd. Als de opslag en Databricks-cluster worden geplaatst, moet u geografisch redundante opslag gebruiken, zodat gegevens kunnen worden geopend in de secundaire regio als de primaire regio niet meer toegankelijk.

   3. Nadat de secundaire regio is gemaakt, moet u migreert van gebruikers, gebruikersmappen, -laptops, clusterconfiguratie, configuratie van taken, -bibliotheken, opslag, init scripts en toegangsbeheer configureren. Aanvullende details worden beschreven in de volgende sectie.

## <a name="detailed-migration-steps"></a>Gedetailleerde migratiestappen

1. **De Databricks-opdrachtregelinterface op uw computer instellen**

   In dit artikel bevat een aantal voorbeelden van code die gebruikmaken van de opdrachtregelinterface voor het merendeel van de geautomatiseerde stappen brengen, omdat het een eenvoudig-voor-user-wrapper via REST-API van Azure Databricks.

   Voordat u alle stappen van de migratie, door de databricks-cli te installeren op uw desktopcomputer of een virtuele machine waar u van plan bent om het werk te doen. Zie voor meer informatie, [Databricks CLI installeren](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Een python-scripts vindt u in dit artikel worden verwacht om te werken met Python 2.7 + < 3.x.

2. **Configureer twee profielen.**

   Configureer één voor de primaire werkruimte en een andere naam voor de secundaire werkruimte:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   De codeblokken in dit artikel schakelen tussen profielen in de volgende stap met behulp van de bijbehorende werkruimte-opdracht. Zorg ervoor dat de namen van de profielen die u hebt gemaakt in elk codeblok worden vervangen.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   U kunt handmatig overschakelen op de opdrachtregel, indien nodig:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Azure Active Directory-gebruikers migreren**

   Dezelfde Azure Active Directory-gebruikers handmatig toevoegen aan de secundaire werkruimte die aanwezig zijn in de primaire werkruimte.

4. **Migreer de gebruikersmappen en -laptops**

   Gebruik de volgende python-code voor het migreren van de sandbox gebruiker-omgevingen, waaronder de geneste mapstructuur en -laptops per gebruiker.

   > [!NOTE]
   > Bibliotheken zijn niet gekopieerd in deze stap, terwijl de onderliggende API biedt geen ondersteuning voor de.

   Kopiëren en sla het volgende pythonscript naar een bestand en de App uitvoeren op uw Databricks-opdrachtregel. Bijvoorbeeld `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **De configuraties van clusters migreren**

   Zodra laptops zijn gemigreerd, kunt u eventueel de configuraties van clusters migreren naar de nieuwe werkruimte. Het is bijna een volledig geautomatiseerde stap met databricks-cli, tenzij u wilt doen selectief config-clustermigratie in plaats van voor alle.

   > [!NOTE]
   > Er is helaas geen eindpunt voor config cluster maken en dit script probeert elk cluster om meteen te maken. Als er niet voldoende kernen beschikbaar zijn in uw abonnement, kan het maken van een cluster mislukken. De fout kan worden genegeerd, zolang de configuratie zijn overgedragen.

   Het volgende script dat is opgegeven voor het afdrukken van een toewijzing van de oude naar het nieuwe cluster-id's die kan worden gebruikt voor het migreren van de taak later (voor taken die zijn geconfigureerd voor het gebruik van bestaande clusters).

   Kopiëren en sla het volgende pythonscript naar een bestand en de App uitvoeren op uw Databricks-opdrachtregel. Bijvoorbeeld `python scriptname.py`.

   ```python
   from subprocess import call, check_output import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace 
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE]) clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids 
   clusters_list = [] for cluster_info in clusters_info_list:   clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements 
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf"," node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","sp ark_env_vars","autotermination_minutes","enable_elastic_disk"]

   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {} for cluster in clusters_list:   print "Trying to migrate cluster " + cluster

   cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
   print "Got cluster config from old workspace"

   # Remove extra content from the config, as we need to build create request with allowed elements only
   cluster_req_json = json.loads(cluster_get_out)    
   cluster_json_keys = cluster_req_json.keys()   

   for key in cluster_json_keys:     
      if key not in cluster_req_elems:       
         cluster_req_json.pop(key, None)
  
   # Create the cluster, and store the mapping from old to new cluster ids
   cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE]) 
   cluster_create_out_json = json.loads(cluster_create_out)   
   cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

   print "Sent cluster create request to new workspace successfully"

   print "Cluster mappings: " + json.dumps(cluster_old_new_mappings)
   print "All done"
   ```

6. **Migratie van de configuratie van taken**

   Als u de configuraties van clusters in de vorige stap hebt gemigreerd, kunt u zich voor het migreren van configuraties van de taak naar de nieuwe werkruimte. Er is een volledig geautomatiseerde stap met behulp van databricks-cli, tenzij u wilt doen selectief taak config migratie in plaats van deze voor alle taken uitvoert.

   > [!NOTE]
   > De configuratie voor een geplande taak bevat ook informatie 'schema', zodat die standaard volgens de geconfigureerde time-out werken gestart wordt zodra deze is gemigreerd. Daarom kan het volgende codeblok Hiermee verwijdert u alle schema-informatie tijdens de migratie (om te voorkomen dat dubbele wordt uitgevoerd in oude en nieuwe werkruimten). De schema's voor dergelijke taken configureren nadat u bent gereed voor cutover.

   De taakconfiguratie vereist-instellingen voor een nieuwe of een bestaand cluster. Als bestaande cluster, probeert het onderstaande script-/code u het oude cluster-ID vervangt door nieuwe cluster-ID.

   Kopieer en bewaar de volgende python script naar een bestand. Vervang de waarde voor `old_cluster_id` en `new_cluster_id`, met de uitvoer van een cluster migreren, doet u in de vorige stap. Voer het uit in uw databricks-cli vanaf de opdrachtregel, bijvoorbeeld `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Migreren van bibliotheken**

   Er is momenteel geen eenvoudige manier om te migreren van bibliotheken van een werkruimte naar een andere. Installeer deze bibliotheken in de nieuwe werkruimte. Deze stap is daarom voornamelijk handmatig. Dit is mogelijk om te automatiseren met behulp van de combinatie van [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples) aangepaste bibliotheken uploaden naar de werkruimte en [bibliotheken CLI](https://github.com/databricks/databricks-cli#libraries-cli).

8. **Migreren van Azure blob storage en Azure Data Lake Store-koppelingen**

   Alle handmatig koppelen [Azure Blob-opslag](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html) en [Azure Data Lake Store (Gen 1)](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html) koppelpunten met behulp van een oplossing op basis van een laptop. De storage-resources zouden in de primaire werkruimte is gekoppeld en die moet worden herhaald in de secundaire werkruimte. Er is geen externe API voor koppelingen.

9. **Cluster init scripts migreren**

   Cluster-initialisatiescripts kunnen worden gemigreerd van de oude naar het nieuwe werkruimte met behulp de [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples). Kopieer eerst de benodigde scripts "dbfs: / dat abricks/init /.. ' naar het lokale bureaublad of de virtuele machine. Kopieer deze scripts vervolgens naar de nieuwe werkruimte op hetzelfde pad.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **Handmatig opnieuw configureren en toepassen van toegangsbeheer.**

   Als uw bestaande primaire werkruimte is geconfigureerd voor het gebruik van de Premium-laag (SKU), is het waarschijnlijk u ook gebruikmaakt van de [Access Control-functie](https://docs.azuredatabricks.net/administration-guide/admin-settings/index.html#manage-access-control).

   Als u de Access Control-functie gebruikt, moet u handmatig het toegangsbeheer toepassen op de resources (Notebooks, Clusters, taken, tabellen).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [documentatie voor Azure Databricks](https://docs.azuredatabricks.net/user-guide/index.html).
