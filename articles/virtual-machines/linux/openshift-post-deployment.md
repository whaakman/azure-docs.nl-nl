---
title: OpenShift op Azure na de implementatie-taken | Microsoft Docs
description: Extra taken voor na een OpenShift-cluster is geïmplementeerd.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 8436b530ac01f03e071604e5023b50f8de6989fd
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034804"
---
# <a name="post-deployment-tasks"></a>Taken na de implementatie

Nadat u een OpenShift-cluster implementeert, kunt u extra items configureren. In dit artikel bevat informatie over:

- Het configureren van eenmalige aanmelding met behulp van Azure Active Directory (Azure AD)
- Het configureren van Log Analytics voor het controleren van OpenShift
- Metrische gegevens en logboekregistratie configureren
- Het installeren van Open Service Broker for Azure (OSBA)

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Eenmalige aanmelding configureren met behulp van Azure Active Directory

Voor het gebruik van Azure Active Directory voor verificatie, moet u eerst de registratie van een Azure AD-app maken. Dit proces bestaat uit twee stappen: het maken van de app-registratie en het configureren van machtigingen.

### <a name="create-an-app-registration"></a>Maken van een app-registratie

Deze stappen wordt de Azure CLI gebruiken om de app-registratie en de grafische gebruikersinterface (portal) om in te stellen de machtigingen te maken. Voor het maken van de app-registratie, moet u de volgende vijf stukjes informatie:

- Weergavenaam: naam van App-registratie (bijvoorbeeld OCPAzureAD)
- Startpagina: OpenShift console URL (bijvoorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Id-URI: OpenShift-console-URL (bijvoorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Antwoord-URL: Openbare URL en de naam van de app-registratie (bijvoorbeeld Master https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Wachtwoord: Beveiligd wachtwoord (gebruik een sterk wachtwoord)

Het volgende voorbeeld wordt een app-registratie met behulp van de voorgaande informatie:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com/console --password {Strong Password}
```

Als de opdracht geslaagd is, krijgt u een JSON-uitvoer die vergelijkbaar is met:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Noteer de toepassings-id-eigenschap geretourneerd door de opdracht voor een latere stap.

In Azure Portal:

1.  Selecteer **Azure Active Directory** > **App-registratie**.
2.  Zoeken naar uw app-registratie (bijvoorbeeld OCPAzureAD).
3.  Klik in de resultaten op de app-registratie.
4.  Onder **instellingen**, selecteer **vereiste machtigingen**.
5.  Onder **vereiste machtigingen**, selecteer **toevoegen**.

  ![App-registratie](media/openshift-post-deployment/app-registration.png)

6.  Klik op stap 1: Selecteer API en klik vervolgens op **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Klik op **Selecteer** aan de onderkant.

  ![Selecteer API App-registratie](media/openshift-post-deployment/app-registration-select-api.png)

7.  In stap 2: Machtigingen selecteert, selecteert u **aanmelden en gebruikersprofiel lezen** onder **gedelegeerde machtigingen**, en klik vervolgens op **Selecteer**.

  ![Toegang tot de App-registratie](media/openshift-post-deployment/app-registration-access.png)

8.  Selecteer **Done**.

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift configureren voor Azure AD-verificatie

Voor het configureren van OpenShift voor het gebruik van Azure AD als een verificatieprovider, moet het bestand /etc/origin/master/master-config.yaml op alle hoofdknooppunten worden bewerkt.

De tenant-ID vinden met behulp van de volgende CLI-opdracht:

```azurecli
az account show
```

In het yaml-bestand, vinden de volgende regels:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Voeg de volgende regels onmiddellijk na de vorige regels:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Zorg ervoor dat de tekst wordt uitgelijnd correct onder identityProviders. De tenant-ID vinden met behulp van de volgende CLI-opdracht: ```az account show```

Start opnieuw op de master OpenShift-services op alle hoofdknooppunten:

**OpenShift Container Platform (OCP) met meerdere modellen**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform met één master**

```bash
sudo systemctl restart atomic-openshift-master
```

**OKD met meerdere modellen**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OKD met één master**

```bash
sudo systemctl restart origin-master
```

In de console van OpenShift ziet u nu twee opties voor verificatie: htpasswd_auth en [App-registratie].

## <a name="monitor-openshift-with-log-analytics"></a>OpenShift bewaken met Log Analytics

Er zijn drie manieren om toe te voegen van de Log Analytics-agent voor OpenShift.
- De Log Analytics-agent voor Linux installeren rechtstreeks op elk knooppunt van OpenShift
- Log Analytics VM-extensie inschakelen op elk knooppunt van OpenShift
- De Log Analytics-agent installeren als een OpenShift-daemon-set

De volledige instructies vindt u hier: https://docs.microsoft.com/azure/log-analytics/log-analytics-containers#configure-a-log-analytics-agent-for-red-hat-openshift.

## <a name="configure-metrics-and-logging"></a>Metrische gegevens en logboekregistratie configureren

Op basis van de vertakking, kunnen de Azure Resource Manager-sjablonen voor OpenShift Container Platform en OKD invoerparameters die zijn opgegeven voor het inschakelen van metrische gegevens en logboekregistratie als onderdeel van de installatie bieden.

De OpenShift Container Platform Marketplace-aanbieding biedt ook een optie voor het inschakelen van metrische gegevens en logboekregistratie tijdens de clusterinstallatie van het.

Als metrische gegevens / logboekregistratie niet is ingeschakeld tijdens de installatie van het cluster, ze eenvoudig achteraf kunnen worden ingeschakeld.

### <a name="ansible-inventory-pre-work"></a>Ansible voorraad vooraf werk

Controleer of het ansible-inventarisatiebestand (/ ansible/etc/hosts) heeft de betreffende variabelen voor metrische gegevens / logboekregistratie. Het inventarisatiebestand kan worden gevonden op verschillende hosts op basis van de sjabloon die wordt gebruikt.

Voor de sjabloon van OpenShift Container en de Marketplace-aanbieding, worden de inventarisatiebestand bevindt zich op bastionhost. Voor de sjabloon OKD de inventarisatiebestand bevindt zich op de master-0-host of het bastionhost op basis van de vertakking in gebruik.

1. Bewerk het bestand /etc/ansible/hosts en voeg de volgende regels toe na de sectie id-Provider (# HTPasswdPasswordIdentityProvider inschakelen). Als deze regels al aanwezig zijn zijn, niet deze opnieuw toevoegen.

   OpenShift / OKD versies 3,9 en lager

   ```yaml
   # Setup metrics
   openshift_hosted_metrics_deploy=false
   openshift_metrics_cassandra_storage_type=dynamic
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"type":"infra"}
   openshift_metrics_cassandra_nodeselector={"type":"infra"}
   openshift_metrics_heapster_nodeselector={"type":"infra"}
   openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

   # Setup logging
   openshift_hosted_logging_deploy=false
   openshift_hosted_logging_storage_kind=dynamic
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"type":"infra"}
   openshift_logging_kibana_nodeselector={"type":"infra"}
   openshift_logging_curator_nodeselector={"type":"infra"}
   openshift_master_logging_public_url=https://kibana.$ROUTING
   ```

   OpenShift / OKD versies 3.10 en hoger

   ```yaml
   # Setup metrics
   openshift_metrics_install_metrics=false
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_cassandra_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_heapster_nodeselector={"node-role.kubernetes.io/infra":"true"}

   # Setup logging
   openshift_logging_install_logging=false
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_kibana_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_curator_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_master_public_url=https://kibana.$ROUTING
   ```

3. $ROUTING vervangen door de tekenreeks die wordt gebruikt voor de optie openshift_master_default_subdomain in hetzelfde bestand /etc/ansible/hosts.

### <a name="azure-cloud-provider-in-use"></a>Azure Cloud-Provider in gebruik

SSH naar de bastionomgeving knooppunt of het eerste hoofdknooppunt (op basis van sjabloon en de vertakking in gebruik) met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht uit:

**OpenShift Containerplatform 3.7 en lager**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OpenShift Containerplatform 3,9 en hoger**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

**OKD 3.7 en lager**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OKD 3,9 en hoger**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure Cloud-Provider niet in gebruik

SSH naar de bastionomgeving knooppunt of het eerste hoofdknooppunt (op basis van sjabloon en de vertakking in gebruik) met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht uit:


**OpenShift Containerplatform 3.7 en lager**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OpenShift Containerplatform 3,9 en hoger**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

**OKD 3.7 en lager**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OKD 3,9 en hoger**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True
ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

## <a name="install-open-service-broker-for-azure-osba"></a>Open Service Broker for Azure (OSBA) installeren

Open Service Broker voor Azure of OSBA, kunt u Azure Cloud Services rechtstreeks vanuit OpenShift inrichten. OSBA in een implementatie van Open Service Broker-API voor Azure. De Open Service Broker-API is een specificatie die een gemeenschappelijke taal voor cloud-providers die systeemeigen cloudtoepassingen gebruiken kunnen voor het beheren van cloudservices zonder de vergrendeling in definieert.

Als u wilt installeren OSBA op OpenShift, volgt u de instructies die u hier: https://github.com/Azure/open-service-broker-azure#openshift-project-template. 

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform)
- [Aan de slag met OKD](https://docs.okd.io/latest)
