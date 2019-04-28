---
title: Zelfstudie - databases configureren in Azure Database voor MySQL met behulp van Ansible | Microsoft Docs
description: Ontdek hoe u Ansible gebruikt om een Azure Database for MySQL-server te maken en configureren
keywords: ansible, azure, devops, bash, playbook, mysql, database
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 7238e993ebd812734b3b08f57b7a4c2f080a7384
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764069"
---
# <a name="tutorial-configure-databases-in-azure-database-for-mysql-using-ansible"></a>Zelfstudie: Databases in Azure Database voor MySQL met behulp van Ansible configureren

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[Azure Database voor MySQL](/azure/mysql/overview) is een relationele databaseservice op basis van de MySQL Community-versie. Azure Database voor MySQL kunt u voor het beheren van de MySQL-databases in uw web-apps.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een MySql-server maken
> * Een MySql-database maken
> * Een regel filewall zo configureren dat een externe app verbinding met uw server maken kan
> * Verbinding maken met uw MySql-server via de Azure cloudshell
> * Query uitvoeren op uw beschikbare MySQL-servers
> * Lijst van alle databases in uw verbonden servers

## <a name="prerequisites"></a>Vereisten

* [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
* [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-resource-group"></a>Een resourcegroep maken

De code playbook in deze sectie maakt u een Azure-resourcegroep. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.  

Sla het volgende playbook op als `rg.yml`:

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

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

* Een resourcegroep met de naam `myResourceGroup` wordt gemaakt.
* De resourcegroep wordt gemaakt in de `eastus` locatie:

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook rg.yml
```

## <a name="create-a-mysql-server-and-database"></a>Een MySQL-server en -database maken

De code playbook in deze sectie maakt u een MySQL-server en een Azure Database for MySQL-exemplaar. De nieuwe MySQL-server is een Gen 5 Basic doel-server met één vCore en de naam `mysqlserveransible`. De database-instantie met de naam `mysqldbansible`.

Zie voor meer informatie over Prijscategorieën [Azure Database for MySQL Prijscategorieën](/azure/mysql/concepts-pricing-tiers). 

Sla het volgende playbook op als `mysql_create.yml`:

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

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

* In de `vars` sectie, de waarde van `mysqlserver_name` moet uniek zijn.
* In de `vars` sectie, Vervang `<server_admin_password>` met een wachtwoord.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-a-firewall-rule"></a>Een firewallregel configureren

Een firewallregel op serverniveau kan een externe app verbinding maken met uw server via de firewall van de Azure MySQL-service. Voorbeelden van externe apps zijn de `mysql` opdrachtregel-hulpprogramma en de MySQL Workbench.

De code playbook in deze sectie maakt u een firewall-regel met de naam `extenalaccess` die verbindingen van externe IP-adressen toestaat. 

Sla het volgende playbook op als `mysql_firewall.yml`:

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

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

* Vervang in de sectie variabelen `startIpAddress` en `endIpAddress`. Het bereik van IP-adressen die overeenkomen met het bereik van waaruit u kunt verbinding gebruiken.
* Verbindingen met Azure Database voor MySQL communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.
* Maakt gebruik van de playbook de `azure_rm_resource` -module, waarmee u direct gebruik van de REST-API.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server"></a>Verbinding maken met de server

In deze sectie gebruikt u de Azure cloudshell verbinding maken met de server die u eerder hebt gemaakt.

1. Selecteer de **uitproberen** knop in de volgende code:

    ```azurecli-interactive
    mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
    ```

1. Voer de volgende opdracht om op te vragen van de status van de server bij de opdrachtprompt:

    ```sql
    mysql> status
    ```
    
    Als alles goed gaat, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:
    
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
    
## <a name="query-mysql-servers"></a>Query MySQL-servers

MySQL-servers in een query uitgevoerd de playbook-code in deze sectie `myResourceGroup` en geeft een lijst van de databases op de servers gevonden.

Sla het volgende playbook op als `mysql_query.yml`:

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

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook mysql_query.yml
```

Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

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

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

Sla het volgende playbook op als `cleanup.yml`:

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

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Ansible in Azure](/azure/ansible/)