---
title: 'Zelfstudie: toegang op rollen gebaseerd beheer (RBAC) rollen configureren in Azure Kubernetes Service (AKS) wanneer u Ansible gebruikt | Microsoft Docs'
description: Leer hoe u Ansible gebruikt om RBAC te configureren in Azure Kubernetes Service(AKS) cluster
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, aks, kubernetes, azure active directory, rbac
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: eae23806ee1b4e2dac1d3410e32c3242e89d4be8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719813"
---
# <a name="tutorial-configure-role-based-access-control-rbac-roles-in-azure-kubernetes-service-aks-using-ansible"></a>Zelfstudie: Op basis van rollen (RBAC) toegangsbeheerrollen in Azure Kubernetes Service (AKS) met behulp van Ansible configureren

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS kan worden geconfigureerd voor het gebruik van [Azure Active Directory (AD)](/azure/active-directory/) voor gebruikersverificatie. Nadat geconfigureerd, gebruikt u uw Azure AD-verificatietoken aan te melden bij het AKS-cluster. De RBAC kan worden gebaseerd op de identiteit van een gebruiker of groepslidmaatschap van een directory.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een Azure AD-functionaliteit AKS-cluster maken
> * Een RBAC-rol in het cluster configureren

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Installeer de bibliotheek RedHat OpenShift** - `pip install openshift`

## <a name="configure-azure-ad-for-aks-authentication"></a>Azure AD voor de AKS-verificatie configureren

Wanneer u Azure AD voor de AKS-verificatie configureert, worden twee Azure AD-toepassingen zijn geconfigureerd. Deze bewerking moet worden voltooid door de beheerder van de Azure-tenant. Zie voor meer informatie, [Integreer Azure Active Directory met AKS](/azure/aks/aad-integration#create-the-server-application). 

Ophalen van de Azure-tenant-beheerder de volgende waarden:

- Server app-geheim
- Server app-ID
- Client-app-ID 
- Tenant-id

Deze waarden zijn nodig om de voorbeeld-playbook uitvoeren.  

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

In deze sectie maakt u een AKS met de [Azure AD-toepassing](#configure-azure-ad-for-aks-authentication).

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- De playbook wordt geladen `ssh_key` van `~/.ssh/id_rsa.pub`. Als u deze wijzigen, gebruikt u de indeling met één regel - beginnen met 'ssh-rsa' (zonder de aanhalingstekens).
- De `client_id` en `client_secret` waarden die worden geladen uit `~/.azure/credentials`, dit is de standaard-referentie-bestand. U kunt deze waarden ingesteld op uw service principal of deze waarden van omgevingsvariabelen laden:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

Sla het volgende playbook op als `aks-create.yml`:

```yml
- name: Create resource group
  azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

- name: List supported kubernetes version from Azure
  azure_rm_aksversion_facts:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with RBAC enabled
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      enable_rbac: yes
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      aad_profile:
          client_app_id: "{{ client_app_id }}"
          server_app_id: "{{ server_app_id }}"
          server_app_secret: "{{ server_app_secret }}"
          tenant_id: "{{ app_tenant_id }}"
  register: aks

- name: Save cluster user config
  copy:
      content: "{{ aks.kube_config }}"
      dest: "aks-{{ name }}-kubeconfig-user"

- name: Get admin config of AKS
  azure_rm_aks_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      show_kubeconfig: admin
  register: aks

- name: Save the kubeconfig
  copy:
      content: "{{ aks.aks[0].kube_config }}"
      dest: "aks-{{ name }}-kubeconfig"
```

## <a name="get-the-azure-ad-object-id"></a>De Azure AD-Object-ID ophalen

Als u wilt een RBAC-binding maken, moet u eerst om op te halen van de Azure AD-Object-ID. 

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Voer in het zoekveld aan de bovenkant van de pagina `Azure Active Directory`. 

1. Klik op `Enter`.

1. In de **beheren** in het menu **gebruikers**.

1. Zoeken in het naamveld voor uw account.

1. In de **naam** kolom, selecteer de koppeling naar uw account.

1. In de **identiteit** sectie, Kopieer de **Object-ID**.

    ![Kopiëren van de Azure AD-Object-ID.](./media/ansible-aks-configure-rbac/ansible-aad-object-id.png)

## <a name="create-rbac-binding"></a>RBAC-binding maken

In deze sectie maakt u een binding van de rol of functie van de binding van cluster in AKS. 

Sla het volgende playbook op als `kube-role.yml`:

```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: <your-aad-account>
```

Vervang de `&lt;your-aad-account>` tijdelijke aanduiding met uw Azure AD-tenant [Object-ID](#get-the-azure-ad-object-id).

De volgende playbook - die implementeert uw nieuwe functie in AKS - opslaan als `aks-kube-deploy.yml`:

```yml
- name: Apply role to AKS
  k8s:
      src: kube-role.yml
      kubeconfig: "aks-{{ name }}-kubeconfig"
```

## <a name="run-the-sample-playbook"></a>De voorbeeld-playbook uitvoeren

Deze sectie vindt u het volledige voorbeeld playbook die de taken maken in dit artikel worden aangeroepen. 

Sla het volgende playbook op als `aks-rbac.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exist
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create AKS
       vars:
           client_app_id: <client id>
           server_app_id: <server id>
           server_app_secret: <server secret>
           app_tenant_id: <tenant id>
       include_tasks: aks-create.yml

     - name: Enable RBAC
       include_tasks: aks-kube-deploy.yml
```

In de `vars` sectie, vervang de volgende tijdelijke aanduidingen door uw Azure AD-gegevens:

- `<client id>`
- `<server id>`
- `<server secret>`
- `<tenant id>`

Voer de volledige playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook aks-rbac.yml
```

## <a name="verify-the-results"></a>De resultaten controleren

In deze sectie maakt u kubectl lijst met de knooppunten maken in dit artikel.

Voer de volgende opdracht in een terminalvenster:

```bash
kubectl --kubeconfig aks-aksansibletest-kubeconfig-user get nodes
```

De opdracht wordt u doorgeleid naar een verificatiepagina. Meld u aan met uw Azure-account.

Eenmaal is geverifieerd, ziet u kubectl de knooppunten op een soortgelijke wijze aan de volgende resultaten:

```txt
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-33413200-0   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-1   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-2   Ready    agent   49m   v1.12.6
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

Sla de volgende code als `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      name: aksansibletest
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
      - name: Remove kubeconfig
        file:
            state: absent
            path: "aks-{{ name }}-kubeconfig"
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ansible in Azure](/azure/ansible/)
