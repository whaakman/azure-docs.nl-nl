---
title: Oplossen van uw implementatie naar Kubernetes met Azure Stack | Microsoft Docs
description: Informatie over het oplossen van uw implementatie naar Kubernetes met Azure Stack.
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
ms.date: 10/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: e9eb3cc029e60acd18fc6611ca14817488a2d983
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266550"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Oplossen van uw implementatie naar Kubernetes met Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview.

Het volgende artikel kijkt naar het oplossen van uw Kubernetes-cluster. U kunt de implementatie-waarschuwing bekijken en de status van uw implementatie controleren door de elementen die zijn vereist voor de implementatie. Mogelijk moet u de implementatielogboeken te verzamelen van Azure Stack of de Linux-VM's die als host fungeren voor Kubernetes. Mogelijk moet u ook samen met de beheerder van uw Azure Stack logboeken ophalen van een administratieve eindpunt.

## <a name="overview-of-deployment"></a>Overzicht van de implementatie

Voordat u het oplossen van uw cluster, is het raadzaam om te controleren van het implementatieproces van de Azure Stack Kubernetes-cluster. De implementatie maakt gebruik van een Azure Resource Manager-oplossingssjabloon te maken van de virtuele machines en de ACS-Engine voor uw cluster te installeren.

### <a name="deployment-workflow"></a>Implementatiewerkstroom

Het volgende diagram ziet u het algemene proces voor het implementeren van het cluster.

![Kubernetes-implementatieproces](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Implementatiestappen

1. Invoerparameters verzamelen over de marketplace-item.

    Voer de waarden die u nodig hebt voor het instellen van het Kubernetes-cluster, met inbegrip van:
    -  **Gebruikersnaam**: De gebruikersnaam voor de virtuele Linux-machines die deel van de Kubernetes-cluster en DVM uitmaken.
    -  **Openbare SSH-sleutel**: De sleutel die wordt gebruikt voor de autorisatie van alle Linux-machines die zijn gemaakt als onderdeel van de Kubernetes-cluster en DVM.
    -  **Service-Principal**: De ID die wordt gebruikt door de Kubernetes Azure-cloud-provider. De client-ID die wordt geïdentificeerd als de toepassings-ID bij het maken van uw service-principal. 
    -  **Clientgeheim**: Deze sleutel u hebt gemaakt tijdens het maken van uw service-principal.

2. De implementatie van virtuele machine maken en de extensie voor aangepaste scripts.
    -  De implementatie van Linux-VM maken met behulp van de Linux-installatiekopie marketplace **Ubuntu Server 16.04-LTS**.
    -  Downloaden en uitvoeren van de klant-script-extensie uit de marketplace. Het script is **aangepast Script voor Linux 2.0**.
    -  Voer het aangepaste script DVM. Het script heeft de volgende taken:
        1. Hiermee haalt u het eindpunt van de galerie van het eindpunt van de Azure Resource Manager-metagegevens.
        2. Hiermee haalt u de active directory-resource-ID van het eindpunt van de Azure Resource Manager-metagegevens.
        3. Laadt het API-model voor de ACS-Engine.
        4. De ACS-Engine implementeert op het Kubernetes-cluster en het Azure Stack-cloud-profiel moet worden opgeslagen `/etc/kubernetes/azurestackcloud.json`.
3. De master-VM's maken.

4. Downloaden en uitvoeren van de klant script-extensies.

5. Voer het script master.

    Het script heeft de volgende taken:
    - Installeert etcd, Docker en Kubernetes-resources, zoals kubelet. etcd is een gedistribueerde sleutel waardearchief dat een manier biedt voor het opslaan van gegevens in een computercluster. Docker biedt ondersteuning voor internetverkoop besturingssysteem-niveau virtualizations bekend als containers. Kubelet is het knooppuntagent die wordt uitgevoerd op elk knooppunt Kubernetes.
    - Stelt u de etcd-service.
    - Stelt u de kubelet-service.
    - Kubelet begint. Deze taak omvat de volgende stappen uit:
        1. De API-service wordt gestart.
        2. Start de controllerservice.
        3. De scheduler-service wordt gestart.
6. Agent-VM's maken.

7. Downloaden en uitvoeren van de klant-script-extensie.

7. Voer het script van de agent. Het aangepaste script van de agent heeft de volgende taken:
    - Etcd installeert
    - Stelt u de kubelet-service
    - Lid wordt van het Kubernetes-cluster

## <a name="steps-for-troubleshooting"></a>Stappen voor het oplossen van problemen

U kunt Logboeken verzamelen op de virtuele machines die ondersteuning bieden voor uw Kubernetes-cluster. U kunt ook het implementatielogbestand bekijken. U moet mogelijk om te communiceren met de Azure Stack-beheerder om te controleren of de versie van Azure-Stack die u nodig hebt om te gebruiken en logboeken ophalen uit de Azure-Stack die gerelateerd zijn aan uw implementatie.

1. Controleer de [Implementatiestatus](#review-deployment-status) en [de logboeken kunt ophalen](#get-logs-from-a-vm) van het hoofdknooppunt in uw Kubernetes-cluster.
2. Zorg ervoor dat u de meest recente versie van Azure Stack. Als u niet zeker weet welke versie u gebruikt, moet u contact op met uw Azure Stack-beheerder. De Kubernetes-cluster marketplace tijd 0.3.0 is Azure Stack-versie 1808 of hoger vereist.
3.  Controleer uw virtuele machine maken van bestanden. U kunt de volgende problemen had:  
    - De openbare sleutel is mogelijk ongeldig. Bekijk de sleutel die u hebt gemaakt.  
    - Maken van VM mogelijk een interne fout hebben geactiveerd of een fout bij het maken gestart. Een aantal factoren kan fouten veroorzaken, met inbegrip van de beperkingen van de capaciteit voor uw Azure Stack-abonnement.
    - Zorg ervoor dat de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machine wordt gestart met een dubbele voorvoegsel.
4.  Als de virtuele machine **OK**, klikt u vervolgens de DVM evalueren. Als de DVM een foutbericht weergegeven heeft:

    - De openbare sleutel is mogelijk ongeldig. Bekijk de sleutel die u hebt gemaakt.  
    - Zo moet u contact op met uw Azure Stack-beheerder om op te halen van de logboeken voor Azure Stack met behulp van de bevoegde eindpunten. Zie voor meer informatie, [diagnostische hulpprogramma's voor Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Als u een vraag over uw implementatie hebt, kunt u boeken of zien als iemand de vraag in al is beantwoord de [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>De implementatiestatus controleren

Wanneer u uw Kubernetes-cluster implementeert, kunt u de status van de implementatie om te controleren op eventuele problemen bekijken.

1. Open de [Azure Stack-portal](https://portal.local.azurestack.external).
2. Selecteer **resourcegroepen**, en selecteer vervolgens de naam van de resourcegroep die u hebt gebruikt bij het implementeren van het Kubernetes-cluster.
3. Selecteer **implementaties**, en selecteer vervolgens de **implementatienaam**.

    ![Problemen oplossen](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Raadpleeg het venster voor het oplossen van problemen. Elke geïmplementeerde resources bevat de volgende informatie:
    
    | Eigenschap | Description |
    | ----     | ----        |
    | Resource | De naam van de resource. |
    | Type | De resourceprovider en het type resource. |
    | Status | De status van het item. |
    | Tijdstempel | De UTC-timestamp van de tijd. |
    | Bewerkingsdetails | De details van de bewerking, zoals de resourceprovider die bij de bewerking, het eindpunt van de resource en de naam van de resource betrokken is. |

    Elk item heeft een pictogram voor de status van de groene of rode.

## <a name="get-logs-from-a-vm"></a>Logboeken van een virtuele machine ophalen

Voor het genereren van de logboeken, moet u verbinding maken met de hoofd-VM's voor uw cluster, open een bash-prompt, en vervolgens een script uitvoeren. Het model virtuele machine kunt u vinden in de resourcegroep van uw cluster en de naam `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Vereisten

U moet een bash-prompt op de computer die u gebruikt voor het beheren van Azure Stack. Bash gebruiken om uit te voeren van de scripts die toegang hebben tot de logboeken. U kunt de bash-prompt die geïnstalleerd met Git gebruiken op een Windows-machine. Als u de meest recente versie van git, raadpleegt u [Git downloads](https://git-scm.com/downloads).

### <a name="get-logs"></a>Logboeken ophalen

Als u zich aanmeldt, moet u de volgende stappen uitvoeren:

1. Open een bash-prompt. Als u Git op een Windows-machine gebruikt, kunt u een bash-prompt openen vanaf het volgende pad: `c:\programfiles\git\bin\bash.exe`.
2. Voer de volgende bash-opdrachten:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Op Windows, moet u niet uitvoeren `sudo`. In plaats daarvan kunt u alleen gebruiken `chmod 744 getkuberneteslogs.sh`.

3. Voer de volgende opdracht met de parameters bijgewerkt zodat deze overeenkomt met uw omgeving in de dezelfde sessie:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

4. Controleer de parameters en stel de waarden op basis van uw omgeving.
    | Parameter           | Description                                                                                                      | Voorbeeld                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,-identiteits-bestand | De RSA bestand met persoonlijke sleutel om de Kubernetes hoofd-VM verbinding te maken. De sleutel moeten beginnen met `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | h-,--host          | Het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van de hoofd-VM's van de cluster in Kubernetes. De naam van de virtuele machine wordt gestart met `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u-,--gebruiker          | De gebruikersnaam van de Kubernetes-cluster master VM. U stelt deze naam wanneer u de marketplace-item configureren.                                                                    | azureuser                                                                     |
    | -d--vmdhost       | Het openbare IP-adres of de FQDN-naam van de DVM. De naam van de virtuele machine wordt gestart met `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Wanneer u uw parameterwaarden hebt toegevoegd, ziet deze er ongeveer als de volgende code uit:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Een geslaagde uitvoering Hiermee maakt u de logboeken.

    ![Gegenereerde logboeken](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. De logboeken in de mappen die zijn gemaakt door de opdracht kunt ophalen. De opdracht maakt u nieuwe mappen en tijdstempels ze.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Volgende stappen

[Kubernetes op Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md)

[Een Kubernetes-cluster toevoegen aan de Marketplace (voor de Azure Stack-operator)](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
