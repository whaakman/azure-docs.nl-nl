---
title: 'Snelstart: Een virtuele Linux-machine maken op de Azure Portal | Microsoft Docs'
description: In deze snelstartgids leert u hoe u de Azure Portal  gebruikt om een virtuele Linux-machine te maken
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 18ac0291bff2c0fbfffdd5dfa3097f8a6acb561f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012880"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Snelstart: Een virtuele Linux-machine maken op de Azure Portal

Virtuele Azure-machines (VM's)kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken van VM's en van alle verwante resources. In deze snelstart wordt beschreven hoe u de Azure Portal gebruikt voor het implementeren van een virtuele Linux-machine (VM) in Azure waarop Ubuntu wordt uitgevoerd. Wanner u uw virtuele machine in actie wilt zien, voert u SSH voor de virtuele machine uit en installeert u de NGINX-webserver.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-ssh-key-pair"></a>Een SSH-sleutelpaar maken

U hebt een SSH-sleutelpaar nodig om deze snelstart te volgen. Als u een bestaand SSH-sleutelpaar hebt, kunt u deze stap overslaan.

Wanneer u een SSH-sleutelpaar wilt maken en u wilt aanmelden bij Linux-VM’s, voert u de volgende opdracht uit vanaf een Bash-shell en volgt u de aanwijzingen op het scherm. U kunt bijvoorbeeld de [Azure Cloud Shell](../../cloud-shell/overview.md) of het [Windows-subsysteem voor Linux](/windows/wsl/install-win10) gebruiken. De uitvoer van de opdracht bevat de bestandsnaam van het bestand met de openbare sleutel. Kopieer de inhoud van het bestand met de openbare sleutel (`cat ~/.ssh/id_rsa.pub`) naar het klembord:

```bash
ssh-keygen -t rsa -b 2048
```

Zie [SSH-sleutels gebruiken met Windows](ssh-from-windows.md) voor gedetailleerde informatie over het maken van SSH-sleutelparen, waaronder het gebruik van PuTTy.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op http://portal.azure.com

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Kies in de linkerbovenhoek van Azure Portal **Een resource maken**.

2. Zoek via het zoekvak boven de lijst met Azure Marketplace-resources naar **Ubuntu Server 16.04 LTS by Canonical**, selecteer deze en kies **Maken**.

3. Geef een VM-naam op, zoals *myVM*, laat het schijftype als *SSD* en geef dan een gebruikersnaam op, zoals *azuregebruiker*.

4. . Als **Verificatietype** selecteert u **Openbare SSH-sleutel**. Plak dan uw openbare sleutel in het tekstvak. Zorg date er geen voorloop- en volgspaties in uw openbare sleutel staan.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/quick-create-portal/create-vm-portal-basic-blade.png)

5. Kies voor het maken van een **nieuwe** resourcegroep en geef vervolgens een naam op, zoals *myResourceGroup*. Kies de gewenste **locatie** en selecteer dan **OK**.

4. Selecteer een grootte voor de VM. U kunt bijvoorbeeld filteren op *Rekentype* of *Schijftype*. Een voorgestelde VM-grootte is *D2s_v3*.

    ![Schermopname van VM-grootten](./media/quick-create-portal/create-linux-vm-portal-sizes.png)

5. Laat onder **Instellingen** alle standaardwaarden staan en selecteer **OK**.

6. Selecteer **Maken** op de overzichtspagina om de implementatie van de VM te starten.

7. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend.

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Maak een SSH-verbinding met de VM.

1. Selecteer de knop **Verbinden** op de overzichtspagina van uw VM. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. Laat op de pagina **Verbinding maken met virtuele machine** de standaardopties staan om verbinding te maken met de DNS-naam via poort 22. In **Aanmelden met lokaal VM-account** wordt een verbindingsopdracht weergegeven. Klik op de knop om de opdracht te kopiëren. Het volgende voorbeeld laat zien hoe de SSH-verbindingsopdracht eruitziet:

    ```bash
    ssh azureuser@myvm-123abc.eastus.cloudapp.azure.com
    ```

3. Plak de SSH-verbindingsopdracht in een shell, zoals de Azure Cloud Shell of Bash op Ubuntu in Windows om de verbinding te maken. 

## <a name="install-web-server"></a>Webserver installeren

Als u uw VM in actie wilt zien, installeert u de NGINX-webserver. Gebruik de volgende opdrachten vanuit uw SSH-sessie om pakketbronnen bij te werken en het meest recente NGINX-pakket te installeren:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Wanneer u klaar bent, `exit`sluit u de SSH-sessie af en geeft u de VM-eigenschappen op in de Azure Portal.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Een netwerkbeveiligingsgroep (NSG) beveiligt binnenkomend en uitgaand verkeer. Wanneer een VM wordt gemaakt in de Azure Portal, wordt een inkomende regel gemaakt op poort 22 voor SSH-verbindingen. Omdat deze VM fungeert als host voor een webserver, moet een NSG-regel worden gemaakt voor poort 80.

1. Selecteer **Netwerken** op de overzichtspagina van de VM.
2. De lijst met bestaande regels voor binnenkomend en uitgaand verkeer worden weergegeven. Kies **Regel voor binnenkomende poort toevoegen**.
3. Selecteer de optie **Basic** bovenaan en kies vervolgens *HTTP* in de lijst met beschikbare services. Poort 80, een prioriteit en een naam worden voor u ingevuld.
4. Selecteer **Toevoegen** om de regel te maken.

## <a name="view-the-web-server-in-action"></a>De webserver in actie zien

Als NGINX is geïnstalleerd en poort 80 is geopend voor de VM, is de webserver nu toegankelijk vanaf internet. Open een webbrowser en voer het openbare IP-adres van de VM in. Het openbare IP-adres kan worden gevonden op de overzichtspagina van de VM of bovenaan de pagina *Netwerken*, waar u de regel toevoegt voor de binnenkomende poort.

![Standaardsite van NGINX](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de VM niet meer nodig hebt, kunt u de resourcegroep, de machine zelf én alle gerelateerde resources verwijderen. Wanneer u dit wilt doen, selecteert u de resourcegroep voor de virtuele machine en selecteert u **Verwijderen**. Vervolgens bevestigt u de naam van de resourcegroep die u wilt verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkbeveiligingsgroep en -regel gemaakt en een eenvoudige webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Linux-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Linux-machines](./tutorial-manage-vm.md)
