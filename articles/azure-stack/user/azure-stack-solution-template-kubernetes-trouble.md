---
title: Problemen met uw Kubernetes-implementatie naar Azure Stack oplossen | Microsoft Docs
description: Informatie over het oplossen van uw Kubernetes-implementatie naar Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.author: mabvrigg
ms.date: 03/20/2019
ms.reviewer: waltero
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: 01a9405c98160149782ab2cf248f64818d631dde
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58293784"
---
# <a name="troubleshoot-your-kubernetes-deployment-to-azure-stack"></a>Problemen met uw Kubernetes-implementatie naar Azure Stack oplossen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie.

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
    -  **Service-principal**: De ID die wordt gebruikt door de Kubernetes Azure-cloud-provider. De client-ID die wordt geïdentificeerd als de toepassings-ID bij het maken van uw service-principal. 
    -  **Clientgeheim**: Deze sleutel u hebt gemaakt tijdens het maken van uw service-principal.

2. De implementatie van virtuele machine maken en de extensie voor aangepaste scripts.
    -  De implementatie van Linux-VM maken met behulp van de Linux-installatiekopie marketplace **Ubuntu Server 16.04-LTS**.
    -  Downloaden en uitvoeren van de aangepaste scriptextensie vanuit de marketplace. Het script is **aangepast Script voor Linux 2.0**.
    -  Voer het aangepaste script DVM. Het script heeft de volgende taken:
        1. Hiermee haalt u het eindpunt van de galerie van het eindpunt van de Azure Resource Manager-metagegevens.
        2. Hiermee haalt u de active directory-resource-ID van het eindpunt van de Azure Resource Manager-metagegevens.
        3. Laadt het API-model voor de ACS-Engine.
        4. De ACS-Engine implementeert op het Kubernetes-cluster en het Azure Stack-cloud-profiel moet worden opgeslagen `/etc/kubernetes/azurestackcloud.json`.
3. De master-VM's maken.

4. Downloaden en uitvoeren van aangepaste scriptextensies.

5. Voer het script master.

    Het script heeft de volgende taken:
    - Installeert etcd, Docker en Kubernetes-resources, zoals kubelet. etcd is een gedistribueerde sleutel waardearchief dat een manier biedt voor het opslaan van gegevens in een computercluster. Docker biedt ondersteuning voor internetverkoop besturingssysteem-niveau virtualizations bekend als containers. Kubelet is het knooppuntagent die wordt uitgevoerd op elk knooppunt Kubernetes.
    - Stelt u de **etcd** service.
    - Stelt u de **kubelet** service.
    - Kubelet begint. Deze taak omvat de volgende stappen uit:
        1. De API-service wordt gestart.
        2. Start de controllerservice.
        3. De scheduler-service wordt gestart.
6. Agent-VM's maken.

7. Downloaden en uitvoeren van de extensie voor aangepaste scripts.

7. Voer het script van de agent. Het aangepaste script van de agent heeft de volgende taken:
    - Installeert **etcd**.
    - Stelt u de **kubelet** service.
    - Lid wordt van het Kubernetes-cluster.

## <a name="steps-for-troubleshooting"></a>Stappen voor het oplossen van problemen

U kunt Logboeken verzamelen op de virtuele machines die ondersteuning bieden voor uw Kubernetes-cluster. U kunt ook het implementatielogbestand bekijken. U moet mogelijk om te communiceren met de Azure Stack-beheerder om te controleren of de versie van Azure-Stack die u nodig hebt om te gebruiken en logboeken ophalen uit de Azure-Stack die gerelateerd zijn aan uw implementatie.

1. Controleer de [Implementatiestatus](#review-deployment-status) en [de logboeken kunt ophalen](#get-logs-from-a-vm) van het hoofdknooppunt in uw Kubernetes-cluster.
2. Zorg ervoor dat u de meest recente versie van Azure Stack. Als u niet zeker weet welke versie u gebruikt, moet u contact op met uw Azure Stack-beheerder.
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

## <a name="review-deployment-logs"></a>Logboeken van de implementatie bekijken

Als de Azure Stack-portal geen voldoende informatie voor u oplossen of implementatiefouten oplossen biedt, wordt de volgende stap is om u te verdiepen in de clusterlogboeken. Als u wilt de implementatielogboeken handmatig kunt ophalen, moet u meestal verbinding maken met een van de master virtuele machines van het cluster. Een eenvoudigere alternatieve methode is om te downloaden en voer de volgende [Bash-script-](https://aka.ms/AzsK8sLogCollectorScript) die door het team van Azure Stack. Met dit script maakt verbinding met de DVM en van het cluster virtuele machines, systeem- en clusterlogboeken worden verzameld en downloadt deze terug naar uw werkstation.

### <a name="prerequisites"></a>Vereisten

U moet een Bash-prompt op de computer die u gebruiken voor het beheren van Azure Stack. Op een Windows-machine, krijgt u een Bash-prompt door het installeren van [Git voor Windows](https://git-scm.com/downloads). Wanneer geïnstalleerd, zoekt _Git Bash_ in het startmenu.

### <a name="retrieving-the-logs"></a>Bij het ophalen van de logboeken

Volg deze stappen om te verzamelen en de clusterlogboeken downloaden:

1. Open een Bash-prompt. Open vanaf een Windows-machine _Git Bash_ of uit te voeren: `C:\Program Files\Git\git-bash.exe`.

2. Het logboek collector-script downloaden door te voeren van de volgende opdrachten in de Bash-prompt:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    chmod 744 getkuberneteslogs.sh
    ```

3. Zoek naar de informatie die wordt vereist door het script en voer deze uit:

    | Parameter           | Description                                                                                                      | Voorbeeld                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -d--vmd-host      | Het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van de DVM. De naam van de virtuele machine wordt gestart met `vmd-`. | IP: 192.168.102.38<br>DNS: vmd-myk8s.local.cloudapp.azurestack.external |
    | -h,--Help-informatie  | Opdrachtgebruik afdrukken. | |
    | -i,-identiteits-bestand | Het persoonlijke sleutelbestand van RSA doorgegeven aan de marketplace-item bij het maken van het Kubernetes-cluster. Nodig voor het extern in naar de Kubernetes-knooppunten. | C:\data\id_rsa.PEM (Putty)<br>~/.SSH/id_rsa (SSH)
    | -m-,--host van master   | Het openbare IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van een Kubernetes-hoofdknooppunt. De naam van de virtuele machine wordt gestart met `k8s-master-`. | IP: 192.168.102.37<br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u-,--gebruiker          | Naam van de gebruiker wordt doorgegeven aan de marketplace-item bij het maken van het Kubernetes-cluster. Die nodig zijn voor extern in naar de Kubernetes-knooppunten | azureuser (standaardwaarde) |


   Wanneer u uw parameterwaarden hebt toegevoegd, ziet er ongeveer als volgt door de opdracht uit:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\id_rsa.pem" --user azureuser --vmd-host 192.168.102.37
     ```

4. Na een paar minuten, het script de verzamelde logboeken naar een map met de naam wordt uitvoer `KubernetesLogs_{{time-stamp}}`. U vindt er een map voor elke virtuele machine die deel uitmaakt van het cluster.

    Het logboek collector script ook zoeken naar fouten in de logboekbestanden en nemen stappen voor probleemoplossing als dit het geval is een bekend probleem vinden. Zorg ervoor dat u de meest recente versie van het script te verhogen kans om te zoeken naar bekende problemen worden uitgevoerd.

> [!Note]  
> Bekijk deze GitHub [opslagplaats](https://github.com/msazurestackworkloads/azurestack-gallery/tree/master/diagnosis) voor meer informatie over het logboek collector-script.

## <a name="next-steps"></a>Volgende stappen

[Kubernetes op Azure Stack implementeren](azure-stack-solution-template-kubernetes-deploy.md)

[Een Kubernetes-cluster toevoegen aan de Marketplace (voor de Azure Stack-operator)](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
