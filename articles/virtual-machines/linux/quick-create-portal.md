---
title: Azure Quick Start - Een VM-portal maken | Microsoft Docs
description: Azure Quick Start - Een VM-portal maken
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 25ffd1c5bf289fa98a4b350eb916b386eee7f05c
ms.lasthandoff: 04/06/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Een virtuele Linux-machine maken met Azure Portal

Virtuele Azure-machines kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken en configureren van virtuele machines en alle verwante resources. In deze Quick Start gaat u een virtuele machine maken via Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-ssh-key-pair"></a>Een SSH-sleutelpaar maken

U hebt een SSH-sleutelpaar nodig om deze Quick Start te volgen. Als u een bestaand SSH-sleutelpaar hebt, kunt u deze stap overslaan. Als u een Windows-machine gebruikt, volgt u de instructies [hier](ssh-from-windows.md). 

In een Bash-shell voert u deze opdracht uit en volgt u de aanwijzingen op het scherm. De uitvoer van de opdracht bevat de bestandsnaam van het bestand met de openbare sleutel. De inhoud van dit bestand is nodig om de virtuele machine te maken.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Compute** op de blade **Nieuw**. Selecteer **Ubuntu-server 16.04 LTS** op de blade **Compute** en klik daarna op **Maken**.

3. Vul het formulier **Basisinformatie** in voor de virtuele machine. Bij **Verificatietype** selecteert u **SSH**. Wanneer u uw **openbare SSH-sleutel** plakt, verwijdert u alle voorloop- en volgspaties. Selecteer een **resourcegroep** of maak een nieuwe. Een resourcegroep is een logische container waarin Azure-resources worden gemaakt en waarin ze collectief worden beheerd. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/quick-create-portal/create-vm-portal-basic-blade.png)  

4. Kies een grootte voor de virtuele machine en klik op **Selecteren**. 

    ![Selecteer een grootte voor de virtuele machine in de portal blade](./media/quick-create-portal/create-vm-portal-size-blade.png)

5. Op de blade Instellingen selecteert u **Ja** onder **Managed Disks gebruiken**. Laat voor de rest de standaardinstellingen staan en klik op **OK**.

6. Klik op de pagina Overzicht op **OK** om de implementatie van de virtuele machine te starten.

7. Voor het bewaken van de implementatiestatus klikt u op de virtuele machine. U vindt de virtuele machine op het Azure Portal-dashboard, of door **Virtuele machines** te selecteren in het menu links. Wanneer de virtuele machine is gemaakt, verandert de status van **Implementeren** in **In uitvoering**.


## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Standaard worden alleen SSH-verbindingen toegestaan naar virtuele Linux-machines die zijn geïmplementeerd in Azure. Als deze virtuele machine wordt gebruikt als een webserver, moet u poort 80 openen voor webverkeer. Deze stap helpt u bij het maken van een regel voor een netwerkbeveiligingsgroep (NSG) om binnenkomende verbindingen op poort 80 toe te staan.

1. Ga op de blade van de virtuele machine naar de sectie **Essentials** en klik op de naam van de **resourcegroep**.
2. Klik op de blade van de resourcegroep op de **netwerkbeveiligingsgroep** in de lijst met resources. De naam van de NSG moet bestaan uit de naam van de virtuele machine, met -nsg toegevoegd aan het einde.
3. Klik op de kop **Binnenkomende beveiligingsregel** om de lijst met regels voor binnenkomende verbindingen te openen. De lijst moet al een regel voor RDP bevatten.
4. Klik op **+ Toevoegen** om de blade **Inkomende beveiligingsregel toevoegen** te openen.
5. Typ **nginx** bij **Naam** en zorg ervoor dat **Poortbereik** is ingesteld op 80 en **Actie** is ingesteld op **Toestaan** en klik vervolgens op **OK**.


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u een SSH-verbinding met de virtuele machine.

1. Klik op de knop **Verbinden** op de blade van de virtuele machine. Op de knop Verbinden wordt een SSH-verbindingsreeks weergegeven die u kunt gebruiken om verbinding te maken met de virtuele machine.

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Voer de volgende opdracht uit om een SSH-sessie te starten. Vervang de verbindingsreeks door de reeks die u uit Azure Portal hebt gekopieerd.

```bash 
ssh <replace with IP address>
```

## <a name="install-nginx"></a>NGINX installeren

Gebruik het volgende bash-script om pakketbronnen bij te werken en het meest recente NGINX-pakket te installeren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>De welkomstpagina van NGIX weergeven

Nu NGINX is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van NGINX weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het `publicIpAddress` dat u hierboven hebt gedocumenteerd. 

![Standaardsite van NGINX](./media/quick-create-cli/nginx.png) 
## <a name="delete-virtual-machine"></a>De virtuele machine verwijderen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. Hiervoor selecteert u de resourcegroep op de blade van de virtuele machine en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor het maken van virtuele machines met een hoge beschikbaarheid](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[CLI-voorbeelden voor VM-implementatie verkennen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

