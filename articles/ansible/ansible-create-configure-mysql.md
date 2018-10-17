---
title: Een Azure Database for MySQL-server maken en configureren met behulp van Ansible (preview)
description: Ontdek hoe u Ansible gebruikt om een Azure Database for MySQL-server te maken en configureren
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, mysql, database
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 508274d11a9693d28a9b3a01bd6ebbd7198e8711
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586663"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-using-ansible-preview"></a>Een Azure Database for MySQL-server maken en configureren met behulp van Ansible (preview)
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) is een beheerde service waarmee u MySQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. In deze snelstart ontdekt u hoe u in ongeveer vijf minuten een Azure Database for MySQL-server maakt in Azure Portal. 

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Ansible gebruikt om binnen vijf minuten een Azure Database for MySQL-server te maken en de firewallregels te configureren. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren. U kunt de RC-versie van Ansible 2.7 installeren door `sudo pip install ansible[azure]==2.7.0rc2` uit te voeren. Ansible 2.7 wordt uitgebracht in oktober 2018. Daarna hoeft u hier geen versie op te geven omdat 2.7 de standaardversie is.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.  

In het volgende voorbeeld wordt een resourcegroep met de naam **myResourceGroup** gemaakt op de locatie **VS - oost**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Sla het bovenstaande playbook op als *rg.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook rg.yml
```

## <a name="create-mysql-server-and-database"></a>Een MySQL-server en -database maken
In het volgende voorbeeld wordt een MySQL-server met de naam **mysqlserveransible** gemaakt, en wordt een Azure Database for MySQL gemaakt met de naam **mysqldbansible**. Dit is een Gen 5-server voor algemeen gebruik met één vCore. De waarde van **mysqlserver_name** moet uniek zijn. Raadpleeg de documentatie over [prijscategorieën](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers) om de geldige waarden per regio en categorie te bekijken. 

Uw eigen `<server_admin_password>` invoeren:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus
    mysqlserver_name: mysqlserveransible
    mysqldb_name: mysqldbansible
    admin_username: mysqladmin
    admin_password: <server_admin_password> 
  tasks:
    - name: Create MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        sku:
          name: B_Gen5_1
          tier: Basic
        location: "{{ location }}"
        version: 5.6
        enforce_ssl: True
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        storage_mb: 51200
    - name: Create instance of MySQL Database
      azure_rm_mysqldatabase:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
        name: "{{ mysqldb_name }}"
```

Sla het bovenstaande playbook op als *mysql_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-firewall-rule"></a>Firewallregel configureren
Een firewallregel op serverniveau stelt een externe toepassing, zoals het opdrachtregelprogramma **mysql** of MySQL Workbench, in staat om verbinding te maken met uw server via de firewall van de MySQL-service van Azure. In het volgende voorbeeld wordt een firewallregel met de naam **extenalaccess** gemaakt, die verbindingen van een specifiek extern IP-adres toestaat. 

Voer uw eigen **startIpAddress** en **endIpAddress** uit met een reeks IP-adressen die overeenkomen met het IP-adres waarmee u verbinding gaat maken: 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
  - name: Open firewall to access MySQL Server from outside
    azure_rm_resource:
      api_version: '2017-12-01'
      resource_group: "{{ resource_group }}"
      provider: dbformysql
      resource_type: servers
      resource_name: "{{ mysqlserver_name }}"
      subresource:
        - type: firewallrules
          name: externalaccess
      body:
        properties:
          startIpAddress: "0.0.0.0"
          endIpAddress: "255.255.255.255"
```

> [!NOTE]
> Verbindingen met Azure Database voor MySQL communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.
> 

Hier wordt de module **azure_rm_resource** gebruikt om de taak uit te voeren. De REST API kan dan direct worden gebruikt.

Sla het bovenstaande playbook op als *mysql_firewall.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-using-command-line-tool"></a>Maak verbinding met de server met behulp van het opdrachtregelprogramma
U kunt MySQL [hier](https://dev.mysql.com/downloads/) downloaden en vervolgens op uw computer installeren. In plaats daarvan kunt u ook op de knop **Try It** klikken in codevoorbeelden, of op `>_` op de werkbalk rechtsboven in Azure Portal, en de **Azure Cloud Shell** starten.

Typ de volgende opdrachten: 

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```azurecli-interactive
 mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
```

2. Bekijk de status van de server:
```sql
 mysql> status
```

Als alles goed gaat, geeft het opdrachtregelprogramma de volgende tekst weer:

```
demo@Azure:~$ mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65233
Server version: 5.6.39.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64) using  EditLine wrapper

Connection id:          65233
Current database:
Current user:           mysqladmin@13.76.42.93
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mysqlserveransible.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 36 min 21 sec

Threads: 5  Questions: 559  Slow queries: 0  Opens: 96  Flush tables: 3  Open tables: 10  Queries per second avg: 0.256
--------------
```

## <a name="using-facts-to-query-mysql-servers"></a>Feiten gebruiken om query's te verzenden naar MySQL-servers
In het volgende voorbeeld wordt er een query gestuurd naar MySQL-servers in **myResourceGroup** en vervolgens naar alle databases op de server:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Query MySQL Servers in current resource group
      azure_rm_mysqlserver_facts:
        resource_group: "{{ resource_group }}"
      register: mysqlserverfacts

    - name: Dump MySQL Server facts
      debug:
        var: mysqlserverfacts

    - name: Query MySQL Databases
      azure_rm_mysqldatabase_facts:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
      register: mysqldatabasefacts

    - name: Dump MySQL Database Facts
      debug:
        var: mysqldatabasefacts
```

Sla het bovenstaande playbook op als *mysql_query.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook mysql_query.yml
```

Vervolgens ziet u deze uitvoer voor MySQL-server: 
```json
"servers": [
    {
        "admin_username": "mysqladmin",
        "enforce_ssl": false,
        "fully_qualified_domain_name": "mysqlserveransible.mysql.database.azure.com",
        "id": "/subscriptions/685ba005-af8d-4b04-8f16-a7bf38b2eb5a/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysqlserveransible",
        "location": "eastus",
        "name": "mysqlserveransible",
        "resource_group": "myResourceGroup",
        "sku": {
            "capacity": 1,
            "family": "Gen5",
            "name": "B_Gen5_1",
            "tier": "Basic"
        },
        "storage_mb": 5120,
        "user_visible_state": "Ready",
        "version": "5.6"
    }
]
```

U ziet ook de volgende uitvoer voor de MySQL-database:
```json
"databases": [
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "information_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysql",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransibler"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysqldbansible",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "performance_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet nodig hebt, kunt u ze verwijderen door onderstaand voorbeeld uit te voeren. Hiermee verwijdert u een resourcegroep met de naam **myResourceGroup**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Sla het bovenstaande playbook op als *rg_delete*.yml. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook rg_delete.yml
```

Als u alleen de zojuist gemaakte MySQL-server wilt verwijderen, kunt u deze verwijderen door het onderstaande voorbeeld uit te voeren:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Delete MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        state: absent
```

Sla het bovenstaande playbook op als *mysql_delete.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)