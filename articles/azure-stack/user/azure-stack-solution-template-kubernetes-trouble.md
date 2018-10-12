---
title: Oplossen van uw implementatie naar Kubernetes (K8) naar Azure Stack | Microsoft Docs
description: Informatie over het oplossen van uw implementatie naar Kubernetes (K8) naar Azure Stack.
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
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116909"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Oplossen van uw implementatie naar Kubernetes met Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview.

Het volgende artikel kijkt naar het oplossen van uw Kubernetes-cluster. U kunt de implementatie-waarschuwing bekijken en de status van uw implementatie controleren door de elementen die zijn vereist voor de implementatie. Mogelijk moet u de implementatielogboeken te verzamelen uit uw Azure Stack of de Linux-VM's die als host fungeren voor Kubernetes. U moet bovendien samen met de beheerder van uw Azure Stack logboeken ophalen van een administratieve eindpunt.

## <a name="overview-of-deployment"></a>Overzicht van de implementatie

Voordat u naar de stappen voor het oplossen van uw cluster, kunt u om te controleren van het implementatieproces van de Azure Stack Kubernetes-cluster. De implementatie maakt gebruik van een Azure Resource Manager-oplossingssjabloon te maken van de virtuele machines en installeert de ACS-Engine voor uw cluster.

### <a name="deployment-workflow"></a>Implementatiewerkstroom

Het volgende diagram ziet u het algemene proces voor het implementeren van het cluster.

![Kubernetes-implementatieproces](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Implementatiestappen

1. Verzamelt de invoerparameters van het marketplace-item.

    Voer de waarden die u nodig hebt voor het instellen van de Kubernetes cluster, waaronder:
    -  **Gebruikersnaam** gebruikersnaam voor de virtuele Linux-Machines die deel van het Kubernetes-cluster uitmaken en DVM.
    -  **Openbare SSH-sleutel** de sleutel die wordt gebruikt voor autorisatie voor alle Linux-machines gemaakt als onderdeel van de Kubernetes-cluster en DVM
    -  **Service-Principal** de ID die wordt gebruikt door de Kubernetes Azure-cloud-provider. De Client-ID die wordt geïdentificeerd als de toepassings-ID tijdens het maken van uw service-principal. 
    -  **Clientgeheim** ze sleutel u hebt gemaakt bij het maken van uw service-principal.

2. Implementatie van virtuele machine maakt en de extensie voor aangepaste scripts.
    -  Hiermee maakt u de implementatie van Linux-VM met de marketplace-Linux-installatiekopie, **Ubuntu Server 16.04-LTS**.
    -  Downloaden en uitvoeren voor klantenscripts vanuit de marketplace. Het script is de **aangepast Script voor Linux 2.0**.
    -  Voert het aangepaste script DVM. Het script:
        1. Hiermee haalt u het eindpunt van de galerie van Azure Resource Manager-metagegevenseindpunt.
        2. Hiermee haalt u de resource-ID van het active directory van Azure Resource Manager-metagegevenseindpunt.
        3. Laadt het API-model voor de ACS-Engine.
        4. De ACS-Engine implementeert op het Kubernetes-cluster en het Azure Stack-cloud-profiel moet worden opgeslagen `/etc/kubernetes/azurestackcloud.json`.
3. Master-VM's gemaakt.

    Gedownload en uitgevoerd voor klantenscripts.

4. De master script wordt uitgevoerd.

    Het script:
    - Installeert etcd, Docker en Kubernetes-resources, zoals kubelet. etcd is een gedistribueerde sleutel waardearchief dat een manier biedt voor het opslaan van gegevens in een computercluster. Docker biedt ondersteuning voor internetverkoop besturingssysteem niveau virtualizations bekend als containers. Kubelet is het knooppuntagent die wordt uitgevoerd op elk knooppunt Kubernetes.
    - Stelt u de etcd-service.
    - Stelt u Kubelet-service.
    - Kubelet begint. Dit omvat het volgende:
        1. API-Service wordt gestart.
        2. Controller-service wordt gestart.
        3. Scheduler-service wordt gestart.
5. Agent-VM's gemaakt.

    Gedownload en uitgevoerd van de klant-script-extensie.

6. Agent-script wordt uitgevoerd. Het aangepaste script van de agent:
    - Etcd installeren.
    - Kubelet-service instellen.
    - Lid wordt van het Kubernetes-cluster.

## <a name="steps-for-troubleshooting"></a>Stappen voor het oplossen van problemen

U kunt Logboeken verzamelen op de virtuele machines met de ondersteuning van uw Kubernetes-cluster. U kunt ook het implementatielogbestand bekijken. U moet mogelijk ook om te communiceren met uw Azure Stack-beheerder om te controleren of de versie van Azure Stack u gebruikmaakt van en naar Logboeken ophalen uit Azure Stack met betrekking tot uw implementatie.

1. Controleer de [Implementatiestatus](#review-deployment-status) en de [de logboeken kunt ophalen](#get-logs-from-a-vm) van het hoofdknooppunt in uw Kubernetes-cluster.
2. U moet de meest recente versie van Azure Stack gebruiken. Als u niet zeker van uw versie van Azure Stack, moet u contact op met uw Azure Stack-beheerder. De Kubernetes-Cluster marketplace tijd 0.3.0 is Azure Stack-versie 1808 of hoger vereist.
3.  Controleer uw virtuele machine maken van bestanden. Mogelijk hebt u de volgende problemen aangetroffen:  
    - De openbare sleutel is mogelijk ongeldig. Bekijk de sleutel die u hebt gemaakt.  
    - Het maken van VM kan een interne fout hebben geactiveerd of veroorzaakt een fout bij het maken. Fouten kunnen worden veroorzaakt door een aantal factoren, waaronder de beperkingen van de capaciteit voor uw Azure Stack-abonnement.
    - Begint de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machine met een dubbele voorvoegsel?
4.  Als de virtuele machine **OK**, en de DVM geëvalueerd. Als de DVM een foutbericht weergegeven heeft:

    - De openbare sleutel is mogelijk ongeldig. Bekijk de sleutel die u hebt gemaakt.  
     - U moet contact opnemen met uw Azure Stack-beheerder om op te halen van de logboeken voor Azure Stack met behulp van de bevoegde eindpunten. Zie voor meer informatie, [diagnostische hulpprogramma's voor Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Als u vragen over uw implementatie hebt, kunt u uw vragen of zien als iemand de vraag in al is beantwoord de [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>De implementatiestatus controleren

U kunt de implementatiestatus controleren wanneer u implementeert uw Kubernetes-cluster om eventuele problemen te bekijken.

1. Open de [Azure Stack-portal](https://portal.local.azurestack.external).
2. Selecteer **resourcegroepen**, en selecteer de naam van de resourcegroep die wordt gebruikt wanneer het Kubernetes-cluster implementeren.
3. Selecteer **implementaties** en vervolgens de **implementatienaam**.

    ![Problemen oplossen](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Raadpleeg het venster voor het oplossen van problemen. Elke geïmplementeerde resources bevat de volgende informatie.
    
    | Eigenschap | Beschrijving |
    | ----     | ----        |
    | Resource | De naam van de resource. |
    | Type | De resourceprovider en het type resource. |
    | Status | De status van het item. |
    | Tijdstempel | De UTC-timestamp van de tijd. |
    | Bewerkingsdetails | De details van de bewerking, zoals de resourceprovider van die betrokken zijn bij de bewerking, het eindpunt van de resource en de naam van de resource. |

    Elk item heeft een pictogram voor de status van de groene of rode.

## <a name="get-logs-from-a-vm"></a>Logboeken van een virtuele machine ophalen

U moet verbinding maken met de hoofd-VM's voor uw cluster, open een bash-prompt, en uitvoeren van een script voor het genereren van de logboeken. Het model kunt u vinden in de resourcegroep van uw cluster en de naam `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Vereisten

U moet een bash vragen op de computer van uw gebruik voor het beheren van Azure Stack. Bash gebruiken om uit te voeren van de scripts die toegang hebben tot de logboeken. Op een Windows-machine, kunt u de geïnstalleerd met Git bash-prompt. Als u de meest recente versie van git, raadpleegt u [git downloads](https://git-scm.com/downloads).

### <a name="get-logs"></a>Logboeken ophalen

1. Open een bash-prompt. Als u git op een Windows-machine gebruikt, kunt u een bash-prompt openen vanaf het volgende pad: `c:\programfiles\git\bin\bash.exe`.
2. Voer de volgende bash-opdrachten:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Op Windows, moet u niet uitvoeren `sudo` en kunt u gewoon gebruik `chmod 744 getkuberneteslogs.sh`.

3. Voer de volgende opdracht met de parameters bijgewerkt zodat deze overeenkomt met uw omgeving in de dezelfde sessie.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    Controleer de parameters en stel de waarden op basis van uw omgeving.
    | Parameter           | Beschrijving                                                                                                      | Voorbeeld                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,-identiteits-bestand | Het bestand RSA persoonlijke sleutel om de kubernetes-hoofd-VM's verbinding te maken. De sleutel moeten beginnen met `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.PEM                                                        |
    | h-,--host          | Het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van de hoofd-VM's van de cluster in Kubernetes. De naam van de virtuele machine wordt gestart met `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN-naam: k8s-12345.local.cloudapp.azurestack.external      |
    | -u-,--gebruiker          | De gebruikersnaam van de Kubernetes-cluster master VM. U kunt deze naam instellen bij het configureren van de marketplace-item.                                                                    | azureuser                                                                     |
    | -d--vmdhost       | Het openbare IP-adres of de FQDN-naam van de DVM. De naam van de virtuele machine wordt gestart met `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Wanneer u de parameterwaarden in de toevoegt, deze kan als volgt uitzien:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Een geslaagde uitvoering Hiermee maakt u de logboeken.

    ![Gegenereerde logboeken](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. De logboeken in de mappen die zijn gemaakt door de opdracht kunt ophalen. De opdracht maakt u een nieuwe map en tijdstempels deze.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Volgende stappen

[Kubernetes op Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md).

[Een Kubernetes (voor de Azure Stack-operator) toevoegen aan de Marketplace](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
