---
title: Toegang tot het Kubernetes-Dashboard in Azure Stack | Microsoft Docs
description: Leer hoe u toegang tot het Kubernetes-Dashboard in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: d6428fe17dabd4eb6c0d68a56e3b44cdf3b3f8b1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58085300"
---
# <a name="access-the-kubernetes-dashboard-in-azure-stack"></a>Toegang tot het Kubernetes-Dashboard in Azure Stack 

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit* 
> [!Note]   
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie. 

Kubernetes biedt een webdashboard dat u voor eenvoudige beheerbewerkingen gebruiken kunt. Dit dashboard kunt u de status van de basisbewaking van statussen en metrische gegevens voor uw toepassingen weergeven, maken en implementeren van services en bewerken van bestaande toepassingen. Dit artikel leest u hoe het instellen van het Kubernetes-dashboard in Azure Stack.

## <a name="prerequisites-for-kubernetes-dashboard"></a>Vereisten voor het Kubernetes-Dashboard

* Azure Stack-Kubernetes-cluster

    U moet een Kubernetes-cluster hebt geïmplementeerd op Azure Stack. Zie voor meer informatie, [implementeren Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

* SSH-client

    U moet een SSH-client beveiliging verbinding maken met het hoofdknooppunt in het cluster. Als u Windows gebruikt, kunt u [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm). U moet de persoonlijke sleutel gebruikt wanneer u uw Kubernetes-cluster geïmplementeerd.

* FTP (PSCP)

    U moet mogelijk ook een FTP-client die ondersteuning biedt voor SSH- en SSH File Transfer Protocol om over te dragen van de certificaten van het hoofdknooppunt op uw computer voor het beheer van Azure Stack. U kunt [FileZilla](https://filezilla-project.org/download.php?type=client). U moet de persoonlijke sleutel gebruikt wanneer u uw Kubernetes-cluster geïmplementeerd.

## <a name="overview-of-steps-to-enable-dashboard"></a>Overzicht van stappen voor het inschakelen van dashboard

1.  De Kubernetes-certificaten van het hoofdknooppunt in het cluster exporteren. 
2.  De certificaten importeren in Azure Stack uw beheercomputer.
2.  Open het Kubernetes-web-dashboard. 

## <a name="export-certificate-from-the-master"></a>Certificaat exporteren van het model 

U kunt de URL voor het dashboard ophalen uit het hoofdknooppunt in uw cluster.

1. Het openbare IP-adres en gebruikersnaam voor uw cluster-master ophalen vanuit het dashboard Azure Stack. Deze informatie ophalen:

    - Aanmelden bij de [Azure Stack-portal](https://portal.local.azurestack.external/)
    - Selecteer **alle services** > **alle resources**. Het model niet vinden in de resourcegroep van uw cluster. Het model is met de naam `k8s-master-<sequence-of-numbers>`. 

2. Open het hoofdknooppunt in de portal. Kopieer de **openbaar IP-adres** adres. Klik op **Connect** om uw gebruikersnaam in de **aanmelden met lokaal VM-account** vak. Dit is de dezelfde gebruikersnaam die u hebt ingesteld tijdens het maken van uw cluster. Gebruik het openbare IP-adres in plaats van het particuliere IP-adres die worden vermeld in de connect-blade.

3.  Open een SSH-client verbinding maken met de master. Als u op Windows werkt, kunt u [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm) om de verbinding te maken. U gebruikt het openbare IP-adres voor het hoofdknooppunt, de gebruikersnaam en de persoonlijke sleutel die u hebt gebruikt bij het maken van het cluster toevoegen.

4.  Wanneer de terminal verbinding maakt, typt u `kubectl` te openen, de Kubernetes-opdrachtregelclient.

5. Voer de volgende opdracht uit:

    ```Bash   
    kubectl cluster-info 
    ``` 
    Zoek de URL voor het dashboard. Bijvoorbeeld:`https://k8-1258.local.cloudapp.azurestack.external/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy`

6.  Het zelfondertekende certificaat ophalen en deze converteren naar het PFX-indeling. Voer de volgende opdracht uit:

    ```Bash  
    sudo su 
    openssl pkcs12 -export -out /etc/kubernetes/certs/client.pfx -inkey /etc/kubernetes/certs/client.key  -in /etc/kubernetes/certs/client.crt -certfile /etc/kubernetes/certs/ca.crt 
    ```

7.  De lijst met geheimen in de **kube-systeem** naamruimte. Voer de volgende opdracht uit:

    ```Bash  
    kubectl -n kube-system get secrets
    ```

    Noteer de kubernetes-dashboard-token -\<XXXXX > waarde. 

8.  Het token ophalen en opslaan. Update de `kubernetes-dashboard-token-<####>` de geheime waarde uit de vorige stap.

    ```Bash  
    kubectl -n kube-system describe secret kubernetes-dashboard-token-<####>| awk '$1=="token:"{print $2}' 
    ```

## <a name="import-the-certificate"></a>Het certificaat importeren

1. Open filezilla werkt en verbinding maken met het hoofdknooppunt. U moet de:

    - het openbare IP-hoofdknooppunt
    - de gebruikersnaam
    - het persoonlijke geheim
    - Gebruik **SFTP - SSH File Transfer Protocol**

2. Kopie `/etc/kubernetes/certs/client.pfx` en `/etc/kubernetes/certs/ca.crt` op uw computer voor het beheer van Azure Stack.

3. Noteer de bestandslocaties. Het script bijwerken met de locaties en opent u PowerShell met een opdrachtprompt. Voer het bijgewerkte script:  

    ```PowerShell   
    Import-Certificate -Filepath "ca.crt" -CertStoreLocation cert:\LocalMachine\Root 
    $pfxpwd = Get-Credential -UserName 'Enter password below' -Message 'Enter password below' 
    Import-PfxCertificate -Filepath "client.pfx" -CertStoreLocation cert:\CurrentUser\My -Password $pfxpwd.Password 
    ``` 

## <a name="open-the-kubernetes-dashboard"></a>Open het Kubernetes-dashboard 

1. Schakel de pop-upblokkering in uw webbrowser.

2. Uw browser naar de URL die u hebt genoteerd bij het uitvoeren van de opdracht punt `kubectl cluster-info`. Bijvoorbeeld: https://azurestackdomainnamefork8sdashboard/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy 
3. Selecteer het clientcertificaat.
4. Voer het token. 
5. Opnieuw verbinding maken met de bash-opdrachtregel op het hoofdknooppunt en machtigen voor `kubernetes-dashboard`. Voer de volgende opdracht uit:

    ```Bash  
    kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard 
    ``` 

    Het script geeft `kubernetes-dashboard` administrator-bevoegdheden in de Cloud. Zie voor meer informatie, [clusters voor RBAC-functionaliteit](https://docs.microsoft.com/azure/aks/kubernetes-dashboard).

U kunt het dashboard. Zie voor meer informatie over het Kubernetes-dashboard [Kubernetes Web UI-Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) 

![Azure Stack Kubernetes-Dashboard](media/azure-stack-solution-template-kubernetes-dashboard/azure-stack-kub-dashboard.png)

## <a name="next-steps"></a>Volgende stappen 

[Kubernetes op Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md)  

[Een Kubernetes-cluster toevoegen aan de Marketplace (voor de Azure Stack-operator)](../azure-stack-solution-template-kubernetes-cluster-add.md)  

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)  
