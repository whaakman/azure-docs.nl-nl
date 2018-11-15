---
title: In deze zelfstudie maakt u een Azure Stack-VM met behulp van een sjabloon | Microsoft Docs
description: Beschrijft hoe u de ASDK gebruiken voor het maken van een virtuele machine met behulp van een vooraf gedefinieerde sjabloon en een aangepaste sjabloon voor GitHub.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/13/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 7780cfde52cb429503e57216e2543807d3fb1d1d
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632455"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Zelfstudie: een virtuele machine met behulp van een communitysjabloon maken

Als een Azure Stack-operators of de gebruiker, kunt u een virtuele machine met [aangepaste snelstartsjablonen van GitHub](https://github.com/Azure/AzureStack-QuickStart-Templates) in plaats van het implementeren van een handmatig vanuit de Azure Stack marketplace.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure Stack-quickstart-sjablonen gebruiken 
> * Een virtuele machine met een aangepaste GitHub-sjabloon maken
> * Start minikube en installeren van een toepassing

## <a name="azure-stack-quickstart-templates"></a>Snelstartsjablonen voor Azure Stack

Azure Stack-quickstart-sjablonen worden opgeslagen in de [openbare opslagplaats voor Azure Stack-QuickStart-sjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates) op GitHub. Deze opslagplaats bevat sjablonen van Azure Resource Manager-implementatie die zijn getest met de Microsoft Azure Stack Development Kit (ASDK). U kunt ze maken het gemakkelijker voor u om te evalueren van Azure Stack en gebruik de ASDK-omgeving. 

Na verloop van tijd hebben veel GitHub-gebruikers bijgedragen tot de opslagplaats, wat resulteert in een verzameling van meer dan 400 sjablonen voor implementatie. Deze opslagplaats is een goed startpunt om een beter begrip van hoe u verschillende soorten omgevingen op Azure Stack implementeren kunt. 

>[!IMPORTANT]
> Sommige van deze sjablonen zijn gemaakt door leden van de community en niet door Microsoft. Elke sjabloon is in licentie gegeven onder een licentieovereenkomst van de eigenaar, niet van Microsoft. Microsoft is niet verantwoordelijk voor deze sjablonen en voert geen controle veiligheid, compatibiliteit of prestaties. Communitysjablonen worden niet ondersteund Microsoft ondersteuningsprogramma of-service, en worden beschikbaar gesteld 'In de huidige staat' zonder garantie van welke aard dan ook.

Als u wilt om bij te dragen van uw Azure Resource Manager-sjablonen naar GitHub, moet u uw bijdrage aan de [AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates) opslagplaats. Zie voor meer informatie over deze opslagplaats en het bijdragen aan de [Leesmij-bestand](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 

## <a name="create-a-vm-using-a-custom-github-template"></a>Een virtuele machine met een aangepaste GitHub-sjabloon maken

In deze zelfstudie voorbeeld de [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure Stack-quickstart-sjabloon wordt gebruikt voor het implementeren van een Ubuntu 16.04 virtuele machine op Azure Stack Minikube voor het beheren van een Kubernetes-cluster uitgevoerd.

Minikube is een hulpprogramma waarmee u eenvoudig Kubernetes lokaal uitvoeren. Minikube wordt uitgevoerd een Kubernetes-cluster van één knooppunt in een virtuele machine, zodat u kunt uitproberen Kubernetes of ontwikkelen met het dagelijkse. Een eenvoudige, één knooppunt Kubernetes-cluster die worden uitgevoerd op een Linux-VM ondersteunt. Minikube is de snelste en eenvoudigste manier om een volledig werkend Kubernetes-cluster uitgevoerd. Hiermee kunnen ontwikkelaars ontwikkelen en testen van hun op basis van een Kubernetes-toepassingsimplementaties op hun lokale machines. Qua, de VM Minikube hoofd- en knooppunt onderdelen van de Agent wordt lokaal uitgevoerd:

- Knooppunt van de master-onderdelen, zoals API-Server, Scheduler, en [etcd Server](https://coreos.com/etcd/) worden uitgevoerd in een enkel Linux-proces LocalKube genoemd.
- Knooppunt onderdelen van de agent worden uitgevoerd in dockercontainers, precies zoals ze zou worden uitgevoerd op een normale Agent-knooppunt. Vanuit het oogpunt van een implementatie van toepassing is het geen verschil tussen wanneer de toepassing wordt geïmplementeerd op een Minikube of in een reguliere Kubernetes-cluster.

Deze sjabloon wordt de volgende onderdelen geïnstalleerd:

- Ubuntu 16.04 LTS-VM
- [Docker CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> De Ubuntu-VM-installatiekopie (Ubuntu Server 16.04 LTS in dit voorbeeld) moet al zijn toegevoegd aan de Azure Stack marketplace voordat u deze stappen uitvoert.

1.  Selecteer **+ een resource maken**, klikt u vervolgens **aangepaste**, klikt u vervolgens **sjabloonimplementatie**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Selecteer **template bewerken**.

    ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Selecteer **Quickstart-sjabloon**.

    ![](media/azure-stack-create-vm-template/3.PNG)

4. Selecteer **101-vm-linux-minikube** van de beschikbare sjablonen met behulp van de **selecteert u een sjabloon** vervolgkeuzelijst lijst en klik vervolgens op **OK**.  

    ![](media/azure-stack-create-vm-template/4.PNG)

5. Als u wilt wijzigingen aanbrengen in de JSON-sjabloon kunt u dat doen. Als dat niet, of als u klaar bent, selecteert u **opslaan** sluiten de **template bewerken** dialoogvenster.

    ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Selecteer **Parameters**, vult u of de beschikbare velden Wijzig indien nodig, en klik op **OK**. Kies het abonnement te gebruiken, maken of kies een bestaande Resourcegroepnaam en selecteer vervolgens **maken** aan de sjabloonimplementatie van een initiëren.

    ![](media/azure-stack-create-vm-template/6.PNG)

7. Kies het abonnement te gebruiken, maken of kies een bestaande Resourcegroepnaam en selecteer vervolgens **maken** aan de sjabloonimplementatie van een initiëren.

    ![](media/azure-stack-create-vm-template/7.PNG)

8. De implementatie van resourcegroep duurt enkele minuten om de aangepaste VM op basis van een sjabloon te maken. U kunt de status van de installatie via meldingen en van de eigenschappen van de resourcegroep kunt bewaken. 

    ![](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > De virtuele machine zal worden uitgevoerd wanneer de implementatie is voltooid. 

## <a name="start-minikube-and-install-an-application"></a>Start Minikube en installeren van een toepassing

Nu dat de Linux-VM is gemaakt, kunt u zich kunt aanmelden bij Minikube start en een toepassing te installeren. 

1. Nadat de implementatie is voltooid, selecteert u **Connect** om het openbare IP-adres dat wordt gebruikt om verbinding maken met de Linux-VM weer te geven. 

    ![](media/azure-stack-create-vm-template/9.PNG)

2. Voer vanuit een opdrachtprompt met verhoogde bevoegdheid **mstsc.exe** verbinding met extern bureaublad openen en verbinding maken met het openbare IP-adres van Linux-VM, gedetecteerd in de vorige stap. Wanneer u hierom wordt gevraagd om aan te melden bij xRDP, gebruikt u de referenties die u hebt opgegeven bij het maken van de virtuele machine.

    ![](media/azure-stack-create-vm-template/10.PNG)

3. Open de Terminal Emulator en voert u de volgende opdrachten uit om te beginnen Minikube:

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![](media/azure-stack-create-vm-template/11.PNG)

4. Open de webbrowser en Ga naar het adres van de Kubernetes-dashboard. Gefeliciteerd, u hebt nu een volledig werken Kubernetes-installatie Minikube!

    ![](media/azure-stack-create-vm-template/12.PNG)

5. Voor het implementeren van een voorbeeldtoepassing, Ga naar de pagina officiële documentatie van Kubernetes en de sectie 'Minikube Cluster maken' overslaan als u al hebt gemaakt een hierboven. Ga naar de sectie 'Uw Node.js-toepassing maken' op https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Meer informatie over Azure Stack-quickstart-sjablonen 
> * Een virtuele machine met een aangepaste GitHub-sjabloon maken
> * Start minikube en installeren van een toepassing

