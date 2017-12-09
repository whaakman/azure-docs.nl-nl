---
title: Azure stapelen snel starten - Portal van de virtuele machine maken
description: Azure Stack Quick Start - Maak een Linux-VM met de portal
services: azure-stack
cloud: azure-stack
author: vhorne
manager: byronr
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a97372002052570ccad2bb8bc442eb0dd1ca76d9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-linux-virtual-machine-with-the-azure-stack-portal"></a>Een virtuele Linux-machine maken met de Stack van Azure-portal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Stack van virtuele machines in Azure kunnen worden gemaakt via de Stack van Azure-portal. Deze methode biedt een gebruikersinterface voor het maken en configureren van een virtuele machine op basis van een browser en alle gerelateerde resources. Deze snelstartgids wordt beschreven hoe u snel een virtuele Linux-machine maken en een webserver installeren op deze.

## <a name="prerequisites"></a>Vereisten

* **Een Linux-installatiekopie in de Stack van Azure marketplace**

   De Stack van Azure marketplace bevat geen standaard een Linux-installatiekopie. Dus voordat u een virtuele Linux-machine maken kunt, moet u controleren of de Azure-Stack-operator heeft gedownload de **Ubuntu Server 16.04 LTS** installatiekopie met behulp van de stappen in de [downloaden marketplace-items van Azure naar Azure Stack](../azure-stack-download-azure-marketplace-item.md) onderwerp.

* **Toegang tot een SSH-client**

   Als u gebruikmaakt van Azure Stack Development Kit (ASDK), u mogelijk geen toegang tot een SSH-client in uw omgeving. Als dit het geval is, kunt u kiezen uit verschillende pakketten met een SSH-client. U kunt bijvoorbeeld PuTTY met een SSH-client en de SSH-codegenerator (puttygen.exe) installeren. Zie de volgende gerelateerde Azure-artikel voor meer informatie over mogelijke opties: [hoe SSH gebruiken sleutels met Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Deze snelstartgids gebruikt de PuTTY SSH-sleutels genereren en verbinding maken met de virtuele Linux-machine. Als u wilt downloaden en installeren van PuTTY, gaat u naar [http://www.putty.org/](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken

U moet een SSH-sleutelpaar voltooien van deze snelstartgids. Als u een bestaand SSH-sleutelpaar hebt, kunt u deze stap overslaan.

1. Navigeer naar de installatiemap PuTTY (de standaardlocatie is ```C:\Program Files\PuTTY```) en voer ```puttygen.exe```.
2. In het venster PuTTY sleutel Generator Zorg ervoor dat de **Type sleutel voor het genereren van** is ingesteld op **RSA**, en de **aantal bits in een gegenereerde sleutel** is ingesteld op **2048**. Wanneer u klaar bent, klikt u op **genereren**.

   ![puttygen.exe](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Houd de muisaanwijzer binnen het venster Generator voor PuTTY-sleutel voor het voltooien van het proces van het genereren van sleutels.
4. Wanneer het genereren van sleutels is voltooid, klikt u op **openbare sleutel opslaan** en **persoonlijke sleutel opslaan** uw openbare en persoonlijke sleutels opslaan naar bestanden.

   ![PuTTY-sleutels](media/azure-stack-quick-linux-portal/Putty02.PNG)



## <a name="sign-in-to-the-azure-stack-portal"></a>Aanmelden bij de Azure-Stack-portal

Meld u aan de Stack van Azure-portal. Het adres van de Stack van Azure-portal is afhankelijk van welk Azure-Stack-product u een verbinding maakt:

* Voor Azure Stack Development Kit (ASDK) gaat u naar: https://portal.local.azurestack.external.
* Ga naar de URL die uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

1. Klik op de **nieuw** knop gevonden in de linkerbovenhoek van de Stack van Azure-portal.

2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Klik op **Create**.

4. Typ de informatie van de virtuele machine. Bij **Verificatietype** selecteert u **Openbare SSH-sleutel**. Bij het plakken in uw openbare SSH-sleutel (die u eerder naar een bestand opgeslagen), zorgt een witruimte voorloop- of volgspaties verwijderen. Na het voltooien klikt u op **OK**.

   ![Basisprincipes van de virtuele machine](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Selecteer **D1_V2** voor de virtuele machine.

   ![Grootte van de machine](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Op de **instellingen** pagina, behoud de standaardinstellingen en klik op **OK**.

7. Op de **samenvatting** pagina, klikt u op **OK** implementatie van virtuele machine te starten.


## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Klik op **Connect** op de pagina van de virtuele machine. U ziet nu een SSH-verbindingsreeks die verbinding maken met de virtuele machine kan worden gebruikt.

   ![Verbinding maken met virtuele machine](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Open PuTTY.
3. Op de **PuTTY-configuratie** scherm onder **categorie**, vouw **SSH** en klik vervolgens op **Auth**. Klik op **Bladeren** en selecteer het persoonlijke sleutelbestand dat u eerder hebt opgeslagen.

   ![PuTTY persoonlijke sleutel](media/azure-stack-quick-linux-portal/Putty03.PNG)
4. Onder **categorie**, Blader omhoog en klik op **sessie**.
5. In de **hostnaam (of IP-adres)** vak, plak de verbindingsreeks van de Stack van Azure-portal die u eerder hebt gezien. In dit voorbeeld wordt de tekenreeks is ```asadmin@192.168.102.34```.
 
   ![PuTTY-sessie](media/azure-stack-quick-linux-portal/Putty04.PNG)
6. Klik op **openen** een sessie aan de virtuele machine te openen.

   ![Linus sessie](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-nginx"></a>NGINX installeren

Het volgende bash-script gebruiken om te werken pakket bronnen en het meest recente NGINX-pakket installeren op de virtuele machine. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Wanneer u klaar bent, sluit u de SSH-sessie en retourneert de pagina overzicht van virtuele machine in de Stack van Azure-portal.


## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Een netwerkbeveiligingsgroep (NSG) beveiligt binnenkomend en uitgaand verkeer. Wanneer een virtuele machine uit de Stack van Azure-portal is gemaakt, wordt een inkomende regel gemaakt op poort 22 voor SSH-verbindingen. Omdat deze virtuele machine fungeert als host voor een webserver, moet een NSG-regel worden gemaakt voor poort 80.

1. Op de virtuele machine **overzicht** pagina, klikt u op de naam van de **resourcegroep**.
2. Selecteer de **netwerkbeveiligingsgroep** voor de virtuele machine. De NSG kan worden geïdentificeerd met behulp van de kolom **Type**. 
3. Klik in het menu links onder **instellingen**, klikt u op **inkomende beveiligingsregels**.
4. Klik op **Add**.
5. Typ bij **Naam** **http**. Zorg ervoor dat het **poortbereik** is ingesteld op 80 en **Actie** is ingesteld op **Toestaan**. 
6. Klik op **OK**.


## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven

Met NGINX geïnstalleerd, en poort 80 is geopend op de virtuele machine, zijn de webserver nu toegankelijk op het openbare IP-adres van de virtuele machine. Het openbare IP-adres, kunt u vinden op de pagina overzicht van de virtuele machine in de Stack van Azure-portal.

Open een webbrowser en blader naar ```http://<public IP address>```.

![Standaardsite van NGINX](media/azure-stack-quick-linux-portal/linux-04.PNG)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. Om dit te doen, selecteert u de resourcegroep van de pagina van de virtuele machine en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, hebt u een eenvoudige virtuele Linux-machine, een groep netwerkbeveiligingsregel, geïmplementeerd en geïnstalleerd een webserver. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

