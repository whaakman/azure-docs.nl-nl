---
title: OpenShift op Azure na de implementatie-taken | Microsoft Docs
description: Extra taken voor na een OpenShift-cluster is geïmplementeerd.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/09/2018
ms.author: haroldw
ms.openlocfilehash: 39febceff58127fb9777ace6e3063fbe41605b79
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426444"
---
# <a name="post-deployment-tasks"></a>Taken na de implementatie

Nadat u een OpenShift-cluster implementeert, kunt u extra items configureren. In dit artikel bevat informatie over het volgende:

- Het configureren van eenmalige aanmelding met behulp van Azure Active Directory (Azure AD)
- Het configureren van Log Analytics voor het controleren van OpenShift
- Metrische gegevens en logboekregistratie configureren

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Eenmalige aanmelding configureren met behulp van Azure Active Directory

Voor het gebruik van Azure Active Directory voor verificatie, moet u eerst de registratie van een Azure AD-app maken. Dit proces bestaat uit twee stappen: het maken van de app-registratie en het configureren van machtigingen.

### <a name="create-an-app-registration"></a>Maken van een app-registratie

Deze stappen wordt de Azure CLI gebruiken om de app-registratie en de grafische gebruikersinterface (portal) om in te stellen de machtigingen te maken. Voor het maken van de app-registratie, moet u de volgende vijf stukjes informatie:

- Weergavenaam: naam van App-registratie (bijvoorbeeld OCPAzureAD)
- Startpagina: OpenShift console URL (bijvoorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Id-URI: OpenShift-console-URL (bijvoorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Antwoord-URL: Openbare URL en de naam van de app-registratie (bijvoorbeeld Master https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Wachtwoord: Beveiligd wachtwoord (gebruik een sterk wachtwoord)

Het volgende voorbeeld wordt een app-registratie met behulp van de voorgaande informatie:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Als de opdracht geslaagd is, krijgt u een JSON-uitvoer die vergelijkbaar is met:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
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

6.  Klik op stap 1: Selecteer API en klik vervolgens op **Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Klik op **Selecteer** aan de onderkant.

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
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
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

De tenant-ID vinden met behulp van de volgende CLI-opdracht: ```az account show```

Start opnieuw op de master OpenShift-services op alle hoofdknooppunten:

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OpenShift Container Platform (OCP) met meerdere modellen**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform met één master**

```bash
sudo systemctl restart atomic-openshift-master
```

In de console van OpenShift ziet u nu twee opties voor verificatie: htpasswd_auth en [App-registratie].

## <a name="monitor-openshift-with-log-analytics"></a>OpenShift bewaken met Log Analytics

Voor het controleren van OpenShift met Log Analytics, kunt u een van twee opties: installatie van de Log Analytics-agent op de VM-host of Log Analytics-container. In dit artikel vindt u instructies voor het implementeren van de Log Analytics-container.

## <a name="create-an-openshift-project-for-log-analytics-and-set-user-access"></a>Een OpenShift-project maken voor Log Analytics en gebruikerstoegang instellen

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-a-daemon-set-yaml-file"></a>Maak een daemon set yaml-bestand

Maak een bestand met de naam ocp-omsagent.yml:

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-a-secret-yaml-file"></a>Een geheim yaml-bestand maken

Voor het maken van de geheime yaml-bestand, moet u twee soorten informatie: Log Analytics-werkruimte-ID en de gedeelde sleutel van Log Analytics-werkruimte. 

Hier volgt een voorbeeld ocp-secret.yml-bestand: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Vervang wsid_data met de met Base64 gecodeerde Log Analytics-werkruimte-ID. Vervang vervolgens key_data door de met Base64 gecodeerde Log Analytics-werkruimte gedeelde sleutel.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-the-secret-and-daemon-set"></a>Het geheim en de daemon-set maken

Het geheime bestand implementeren:

```bash
oc create -f ocp-secret.yml
```

De daemon-set van Log Analytics-Agent implementeren:

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Metrische gegevens en logboekregistratie configureren

De Azure Resource Manager-sjabloon voor OpenShift Container Platform biedt invoerparameters die zijn opgegeven voor het inschakelen van metrische gegevens en logboekregistratie. De OpenShift Container Platform Marketplace-aanbieding en de OpenShift Origin-Resource Manager-sjabloon niet.

Als u de OCP Resource Manager-sjabloon en de metrische gegevens gebruikt en logboekregistratie zijn niet ingeschakeld tijdens de installatie of als u de OCP Marketplace-aanbieding hebt gebruikt, kunt u eenvoudig kunt zich schakelt u deze na de gebeurtenis. Als u de OpenShift Origin-Resource Manager-sjabloon gebruikt, zijn werk dat vooraf is vereist.

### <a name="openshift-origin-template-pre-work"></a>OpenShift Origin sjabloon vooraf werk

1. SSH naar het eerste hoofdknooppunt met behulp van poort 2200.

   Voorbeeld:

   ```bash
   ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
   ```

2. Bewerk het bestand /etc/ansible/hosts en voeg de volgende regels toe na de sectie id-Provider (# HTPasswdPasswordIdentityProvider inschakelen):

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

3. $ROUTING vervangen door de tekenreeks die wordt gebruikt voor de optie openshift_master_default_subdomain in hetzelfde bestand /etc/ansible/hosts.

### <a name="azure-cloud-provider-in-use"></a>Azure Cloud-Provider in gebruik

Op de eerste hoofdknooppunt (oorsprong) of bastionhost knooppunt (OCP), SSH met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht uit:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure Cloud-Provider niet in gebruik

Op de eerste hoofdknooppunt (oorsprong) of bastionhost knooppunt (OCP), SSH met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht uit:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [Aan de slag met OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)
