---
title: Azure Kubernetes-cluster voor Windows | Microsoft Docs
description: Aan de slag met een Kubernetes-cluster voor Windows-containers en dit implementeren in Azure Container Service
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c43648dae95d90d0ee9f3d6b5bedfad7ab4889ca
ms.lasthandoff: 03/22/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Aan de slag met Kubernetes- en Windows-containers in Container Service


In dit artikel ziet u hoe u een Kubernetes-cluster maakt in Azure Container Service dat Windows-knooppunten bevat om Windows-containers uit te voeren. 

> [!NOTE]
> De ondersteuning voor Windows-containers met Kubernetes in Azure Container Service is momenteel beschikbaar als preview. Gebruik Azure Portal of een Resource Manager-sjabloon om een Kubernetes-cluster te maken met Windows-knooppunten. Deze functie wordt momenteel niet ondersteund door Azure CLI 2.0.
>



In de volgende afbeelding ziet u de architectuur van een Kubernetes-cluster in Azure Container Service met één Linux-hoofdknooppunt en twee Windows-agentknooppunten. 

* Het Linus-hoofdknooppunt dient voor de Kubernetes REST-API en is toegankelijk via SSH op poort 22 of `kubectl` op poort 443. 
* De Windows-agentknooppunten worden gegroepeerd in een Azure-beschikbaarheidsset en voeren uw containers uit. De Windows-knooppunten kunnen worden geopend via een RDP SSH-tunnel via het hoofdknooppunt. Azure Load Balancer-regels worden dynamisch toegevoegd aan het cluster, afhankelijk van de weergegeven services.


![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

Alle virtuele machines bevinden zich in hetzelfde virtuele privénetwerk en hebben volledige toegang tot elkaar. Alle virtuele machines zijn voorzien van een kubelet, een Docker en een proxy.

## <a name="prerequisites"></a>Vereisten


* **Openbare SSH RSA-sleutel**: wanneer u implementeert via de portal of een van de snelstartsjablonen van Azure, moet u een openbare SSH RSA-sleutel opgeven voor verificatie bij de virtuele machines van Azure Container Service. Als u SSH RSA-sleutels (Secure Shell) wilt maken, ziet u de richtlijnen voor [OS X en Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) of [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **Service-principal-client-id en -geheim**: zie [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor meer informatie over en richtlijnen.




## <a name="create-the-cluster"></a>Het cluster maken

U kunt Azure Portal gebruiken om [een Kubernetes-cluster te maken](container-service-deployment.md#create-a-cluster-by-using-the-azure-portal) met Windows-agentknooppunten. Let op de volgende instellingen bij het maken van het cluster:

* Selecteer in de blade **Basics** in **Orchestrator** de optie **Kubernetes**. 

  ![Orchestrator in Kubernetes selecteren](media/container-service-kubernetes-windows-walkthrough/portal-select-kubernetes.png)

* Voer in de blade **Master configuration** de gebruikersreferenties en service-principalreferenties voor de Linux-hoofdknooppunten in. Kies 1, 3 of 5 modellen.

* Selecteer in de blade **Agent configuration** in **Operating system** de optie **Windows (preview)**. Voer beheerdersreferenties voor de Windows-agentknooppunten in.

  ![Windows-agents selecteren](media/container-service-kubernetes-windows-walkthrough/portal-select-windows.png)

Zie [Een Azure Container Service-cluster implementeren](container-service-deployment.md) voor meer details.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Gebruik het opdrachtregelhulpprogramma `kubectl` om via uw lokale computer verbinding te maken met het hoofdknooppunt van het Kubernetes-cluster. Zie [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md#connect-to-a-kubernetes-cluster) voor meer informatie over het installeren en instellen van `kubectl`. U kunt `kubectl`-opdrachten gebruiken voor toegang tot de webgebruikersinterface van Kubernetes en om Windows-containerwerkbelastingen te maken en beheren.

## <a name="create-your-first-kubernetes-service"></a>Uw eerste Kubernetes-service maken

Wanneer u het cluster hebt gemaakt en u verbinding hebt gemaakt met `kubectl`, kunt u een eenvoudige Windows-web-app openen en deze beschikbaar maken op internet. In dit voorbeeld geeft u de containerresources op met een YAML-bestand en maakt u het met `kubctl apply`.

1. Typ `kubectl get nodes` om een lijst met alle knooppunten te bekijken. Typ het volgende als u alle gegevens over de knooppunten wilt bekijken:  

    ```
    kubectl get nodes -o yaml
    ```

2. Maak een bestand met de naam `simpleweb.yaml` en kopieer het volgende. Met dit bestand wordt een web-app ingesteld met de basisinstallatiekopie van het Windows Server 2016 Server Core-besturingssysteem uit de [Docker-hub](https://hub.docker.com/r/microsoft/windowsservercore/).  

```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: win-webserver
    labels:
      app: win-webserver
  spec:
    ports:
      # the port that this service should serve on
    - port: 80
      targetPort: 80
    selector:
      app: win-webserver
    type: LoadBalancer
  ---
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: win-webserver
    name: win-webserver
  spec:
    replicas: 1
    template:
      metadata:
        labels:
          app: win-webserver
        name: win-webserver
      spec:
        containers:
        - name: windowswebserver
          image: microsoft/windowsservercore
          command:
          - powershell.exe
          - -command
          - "<#code used from https://gist.github.com/wagnerandrade/5424431#> ; $$listener = New-Object System.Net.HttpListener ; $$listener.Prefixes.Add('http://*:80/') ; $$listener.Start() ; $$callerCounts = @{} ; Write-Host('Listening at http://*:80/') ; while ($$listener.IsListening) { ;$$context = $$listener.GetContext() ;$$requestUrl = $$context.Request.Url ;$$clientIP = $$context.Request.RemoteEndPoint.Address ;$$response = $$context.Response ;Write-Host '' ;Write-Host('> {0}' -f $$requestUrl) ;  ;$$count = 1 ;$$k=$$callerCounts.Get_Item($$clientIP) ;if ($$k -ne $$null) { $$count += $$k } ;$$callerCounts.Set_Item($$clientIP, $$count) ;$$header='<html><body><H1>Windows Container Web Server</H1>' ;$$callerCountsString='' ;$$callerCounts.Keys | % { $$callerCountsString+='<p>IP {0} callerCount {1} ' -f $$_,$$callerCounts.Item($$_) } ;$$footer='</body></html>' ;$$content='{0}{1}{2}' -f $$header,$$callerCountsString,$$footer ;Write-Output $$content ;$$buffer = [System.Text.Encoding]::UTF8.GetBytes($$content) ;$$response.ContentLength64 = $$buffer.Length ;$$response.OutputStream.Write($$buffer, 0, $$buffer.Length) ;$$response.Close() ;$$responseStatus = $$response.StatusCode ;Write-Host('< {0}' -f $$responseStatus)  } ; "
        nodeSelector:
          beta.kubernetes.io/os: windows
  ```

      
> [!NOTE] 
> De configuratie omvat `type: LoadBalancer`. Met deze instelling zorgt u ervoor dat de service beschikbaar wordt gemaakt op internet via een Azure Load Balancer. Zie voor meer informatie [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Taakverdeling instellen voor containers in een Kubernetes-cluster in Azure Container Service).
>

## <a name="start-the-application"></a>De toepassing starten

1. Typ het volgende om de toepassing te starten:  

    ```
    kubectl apply -f simpleweb.yaml
    ```  
  
  
2. Typ het volgende als u de implementatie van de service wilt controleren (dit duurt ongeveer 30 seconden):  

    ```
    kubectl get pods
    ```

3. Wanneer de service wordt uitgevoerd, typt u het volgende als u de interne en externe IP-adressen van de service wilt bekijken:

    ```
    kubectl get svc
    ``` 
  
    ![IP-adressen van de Windows-service](media/container-service-kubernetes-windows-walkthrough/externalipa.png)

    Het toevoegen van het externe IP-adres duurt enkele minuten. Voordat de Load Balancer het externe adres heeft geconfigureerd, wordt het weergegeven als `<pending>`.

4. Wanneer het externe IP-adres beschikbaar is gemaakt, kunt u de service via uw webbrowser bereiken.

    ![Windows Server-app in een browser](media/container-service-kubernetes-windows-walkthrough/wincontainerwebserver.png)


## <a name="access-the-windows-nodes"></a>Toegang tot de Windows-knooppunten
U kunt Windows-knooppunten openen via een lokale Windows-computer. Gebruik daarvoor Verbinding met extern bureaublad. Het wordt aangeraden om een RDP SSH-tunnel te gebruiken via het hoofdknooppunt. 

Er zijn meerdere opties voor het maken van SSH-tunnels in Windows. In deze sectie wordt beschreven hoe u PuTTY gebruikt om de tunnel te maken.

1. [Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) naar uw Windows-systeem.

2. Voer de toepassing uit.

3. Voer een hostnaam in die bestaat uit de naam van de clusterbeheerder en de openbare DNS-naam van het eerste hoofdknooppunt in het cluster. De **hostnaam** ziet er ongeveer zo uit: `adminuser@PublicDNSName`. Voer 22 in bij **Poort**.

  ![PuTTY-configuratie 1](media/container-service-kubernetes-windows-walkthrough/putty1.png)

4. Selecteer **SSH > Verificatie**. Voeg een pad aan uw persoonlijke-sleutelbestand (.ppk-indeling) toe voor verificatie. U kunt een hulpprogramma als [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) gebruiken om dit bestand te genereren vanuit de SSH-sleutel waarmee het cluster is gemaakt.

  ![PuTTY-configuratie 2](media/container-service-kubernetes-windows-walkthrough/putty2.png)

5. Selecteer **SSH > Tunnels** en configureer de doorgestuurde poorten. Omdat uw lokale Windows-machine al gebruikmaakt van poort 3389 wordt het aanbevolen om de volgende instellingen te gebruiken om Windows-knooppunt 0 en Windows-knooppunt 1 te bereiken. (Ga door met dit patroon voor aanvullende Windows-knooppunten.)

    **Windows-knooppunt 0**

    * **Bronpoort:** 3390
    * **Doel:** 10.240.245.5:3389

    **Windows-knooppunt 1**

    * **Bronpoort:** 3391
    * **Doel:** 10.240.245.6:3389

    ![Afbeelding van Windows RDP-tunnels](media/container-service-kubernetes-windows-walkthrough/rdptunnels.png)

6. Wanneer u klaar bent, klikt u op **Sessie > Opslaan** om de verbindingsconfiguratie op te slaan.

7. Klik op **Openen** als u verbinding wilt maken met de PuTTY-sessie. Maak verbinding met het hoofdknooppunt.

8. Maak verbinding met een extern bureaublad. Als u verbinding wilt maken met het eerste Windows-knooppunt, geeft u bij **Computer** `localhost:3390` op en klikt u op **Verbinden**. (Als verbinding wilt maken met de tweede, geeft u `localhost:3390` op, enz.) Voltooi het verbinden door het wachtwoord van de lokale Windows-beheerder op te geven dat u hebt ingesteld tijdens het implementeren.


## <a name="next-steps"></a>Volgende stappen

Hier volgen aanbevolen koppelingen om meer te weten te komen over Kubernetes:

* [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/index.html): hier leest u hoe u toepassingen in containers kunt implementeren, schalen en bijwerken, en hoe u fouten kunt opsporen.
* [Gebruikershandleiding voor Kubernetes](http://kubernetes.io/docs/user-guide/): bevat informatie over het uitvoeren van programma's in een bestaand Kubernetes-cluster.
* [Voorbeelden van Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples): biedt een aantal voorbeelden voor het uitvoeren van echte toepassingen met Kubernetes.
