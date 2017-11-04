---
title: OpenShift op Azure Post implementatietaken | Microsoft Docs
description: OpenShift Post implementatietaken
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 12e6785358f5f412326418b0c64eeaeabdaa3b5f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="post-deployment-tasks"></a>Taken na de implementatie

Nadat het cluster OpenShift is geïmplementeerd, zijn er extra items zijn die kunnen worden geconfigureerd. In dit artikel wordt ingegaan op het volgende:

- Eenmalige aanmelding configureren met behulp van Azure Active Directory (AAD)
- OMS om te controleren OpenShift configureren
- Configureren van de metrische gegevens en logboekregistratie

## <a name="single-sign-on-using-aad"></a>Eenmalige aanmelding met behulp van AAD

Om te kunnen gebruiken AAD voor verificatie, moet u eerst een Azure AD-App-registratie gemaakt. Dit proces wordt gebruikgemaakt van twee stappen - maken van de registratie van de app en configureer vervolgens machtigingen.

### <a name="create-app-registration"></a>Registratie van de app maken

We gebruiken de Azure CLI voor het maken van de App-registratie en de grafische gebruikersinterface (Portal) als de machtigingen wilt instellen. Voor het maken van de registratie van de App vijf verschillende soorten informatie vereist.

- Weergavenaam: De naam van App-registratie (ex: OCPAzureAD)
- Startpagina: De URL van de Console OpenShift (voorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- URI-id: De URL van de Console OpenShift (voorbeeld: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- URL van antwoord: De hoofdsleutel van de openbare URL en de naam van de App-registratie (ex: oauth2callback-https://masterdns343khhde.westus.cloudapp.azure.com:8443/OCPAzureAD)
- Wachtwoord: Beveiligd wachtwoord (gebruik een sterk wachtwoord)

Het volgende voorbeeld maakt de registratie van een App met behulp van de gegevens van hierboven.

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Als de opdracht geslaagd is, ontvangt u een JSON-uitvoer lijkt op:

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
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD"
  ]
}
```

Let op de toepassings-id-eigenschap geretourneerd van de opdracht voor een latere stap.

In de **Azure-Portal**:

1.  Selecteer **Azure Active Directory** --> **App-registratie**
2.  Zoeken naar uw App-registratie (ex: OCPAzureAD)
3.  Klik in de resultaten op de App-registratie
4.  Selecteer in de blade beleidinstellingen **machtigingen vereist**
5.  Klik op de blade Required Permissions **toevoegen**

  ![App-registratie](media/openshift-post-deployment/app-registration.png)

6.  Klik op stap 1: Selecteer API en klik op **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)** en klik op **Selecteer** onderin

  ![App-registratie selectie-API](media/openshift-post-deployment/app-registration-select-api.png)

7.  In stap 2: Machtigingen selecteert, selecteert u **aanmelden en gebruikersprofiel lezen** onder **gedelegeerde machtigingen** en klik op **selecteren**

  ![Registratie van de App-toegang](media/openshift-post-deployment/app-registration-access.png)

8.  Klik op **gedaan**

### <a name="configure-openshift-for-azure-ad-authentication"></a>OpenShift configureren voor Azure AD-verificatie

Voor het configureren van OpenShift voor het gebruik van Azure AD als een verificatieprovider, de **/etc/origin/master/master-config.yaml** bestand op alle knooppunten van het model moet worden bewerkt.

De tenant-Id vindt u door met de volgende CLI-opdracht:

```azurecli
az account show
```

In het bestand yaml vinden de volgende regels:

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

Voeg de volgende regels onmiddellijk na de bovenstaande regels:

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

De tenant-Id vindt u door met de volgende CLI-opdracht:```az account show```

De OpenShift model-services op alle knooppunten van het model opnieuw starten

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OpenShift Container Platform met meerdere modellen**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform met één Master**

```bash
sudo systemctl restart atomic-openshift-master
```

In de OpenShift Console nu ziet u twee opties voor de verificatie - htpasswd_auth en **[App-registratie]**.

## <a name="monitor-openshift-with-oms"></a>Monitor OpenShift met OMS

Bewaking van OpenShift met OMS kan worden bereikt met een van twee opties - installatie van de OMS-Agent op de VM-host of OMS-Container. In dit artikel bevat instructies over het implementeren van de OMS-Container.

## <a name="create-an-openshift-project-for-oms-and-set-user-access"></a>Maken van een project OpenShift voor OMS en gebruikerstoegang instellen

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-daemon-set-yaml-file"></a>Daemon set yaml-bestand maken

Maak een bestand met de naam ocp-omsagent.yml.

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

## <a name="create-secret-yaml-file"></a>Geheime yaml-bestand maken

Om het geheim yaml-bestand maakt, worden twee soorten informatie vereist - OMS-werkruimte-ID en OMS werkruimte gedeelde sleutel. 

Ocp-secret.yml voorbeeldbestand 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Vervang wsid_data met de Base64-gecodeerd OMS-werkruimte-ID en key_data vervangen met de Base64-gecodeerd OMS werkruimte gedeelde sleutel.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-secret-and-daemon-set"></a>Geheim en de daemon set maken

Implementeer het geheim bestand

```bash
oc create -f ocp-secret.yml
```

De Daemon Set van OMS-Agent implementeren

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Metrische gegevens en logboekregistratie configureren

De sjabloon OpenShift Container Platform (OCP) Resource Manager biedt invoerparameters voor het inschakelen van metrische gegevens en logboekregistratie. De sjabloon OpenShift Container Platform Marketplace bieden en OpenShift Origin Resource Manager heeft geen.

Als de OCP Resource Manager-sjabloon gebruikt en metrische gegevens en logboekregistratie zijn niet ingeschakeld tijdens de installatie of de OCP Marketplace-aanbieding gebruikt, kunnen deze gemakkelijk achteraf ingeschakeld. Als de OpenShift Origin Resource Manager-sjabloon gebruikt, is enkele vooraf werk vereist.

### <a name="openshift-origin-template-pre-work"></a>OpenShift oorsprong sjabloon vooraf werk

SSH kunt uitvoeren naar de het eerste Master-knooppunt met behulp van poort 2200

Voorbeeld

```bash
ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
```

Bewerk de **/etc/ansible/hosts bestand** en voeg de volgende regels toe na de sectie Identity-Provider (# HTPasswdPasswordIdentityProvider inschakelen)

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

$ROUTING vervangen door de tekenreeks die wordt gebruikt voor **openshift_master_default_subdomain** optie in dezelfde **/etc/ansible/hosts** bestand.

### <a name="azure-cloud-provider-in-use"></a>Azure Cloud-Provider in gebruik

Op de eerste Master-knooppunt (oorsprong) of Bastionomgeving knooppunt (OCP), SSH met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure Cloud-Provider niet in gebruik

Op de eerste Master-knooppunt (oorsprong) of Bastionomgeving knooppunt (OCP), SSH met behulp van de referenties die zijn opgegeven tijdens de implementatie. Geef de volgende opdracht:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [Aan de slag met OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)