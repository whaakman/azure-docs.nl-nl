---
title: Azure stapelen snel starten - Portal van de virtuele machine maken
description: Azure Stack Quick Start - Maak een Linux-VM met de portal
services: azure-stack
cloud: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 2ea07f04d4c566c0add39d75cad3d3a4ed81c6c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32152217"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-with-the-azure-stack-portal"></a>Snelstartgids: een virtuele Linux-server-machine maken met de Stack van Azure-portal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Ubuntu Server 16.04 TNS virtuele machine maken met behulp van de Stack van Azure-portal. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine. In dit artikel hebt u ook de stappen voor het:

* Verbinding maken met de virtuele machine met een externe client.
* Installeer een NGINX-webserver.
* Opschonen van uw resources.

## <a name="prerequisites"></a>Vereisten

* **Een Linux-installatiekopie in de Stack van Azure marketplace**

   De Stack van Azure marketplace bevat geen standaard een Linux-installatiekopie. Voordat u een virtuele Linux-server-machine maken kunt, zorgt u ervoor dat de Azure-Stack-operator geeft de **Ubuntu Server 16.04 LTS** u moet de installatiekopie. De operator kunt de stappen in de [downloaden marketplace-items van Azure naar Azure Stack](../azure-stack-download-azure-marketplace-item.md) artikel.

* **Toegang tot een SSH-client**

   Als u Azure Stack Development Kit (ASDK) gebruikt, u mogelijk geen toegang tot een SSH-client. Als u een client moet, zijn er verschillende pakketten met een SSH-client. PuTTY bevat bijvoorbeeld een SSH-client en een SSH-codegenerator (puttygen.exe). Lees voor meer informatie over beschikbare pakketten, het volgende Azure artikel: [hoe SSH gebruiken sleutels met Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Deze snelstartgids gebruikt de PuTTY SSH-sleutels genereren en verbinding maken met de virtuele machine voor de Linux-server. Als u wilt downloaden en installeren van PuTTY, gaat u naar [ http://www.putty.org/ ](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken

U moet een SSH-sleutelpaar Voltooi de stappen in dit artikel. Als u een bestaande SSH-sleutelpaar hebt, kunt u deze stap overslaan.

1. Navigeer naar de installatiemap PuTTY (de standaardlocatie is ```C:\Program Files\PuTTY```) en voer ```puttygen.exe```.
2. In het venster PuTTY sleutel Generator Zorg ervoor dat de **Type sleutel voor het genereren van** is ingesteld op **RSA**, en de **aantal bits in een gegenereerde sleutel** is ingesteld op **2048**. Wanneer u klaar bent, klikt u op **genereren**.

   ![PuTTY-configuratie van de sleutel Generator](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Houd de muisaanwijzer willekeurig in het venster Generator voor PuTTY-sleutel voor het genereren van een sleutel.
4. Wanneer het genereren van sleutels is voltooid, klikt u op **openbare sleutel opslaan** en klik vervolgens op **persoonlijke sleutel opslaan** uw sleutels opslaan naar bestanden.

   ![PuTTY-sleutel Generator resultaten](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Aanmelden bij de Azure-Stack-portal

Meld u aan de Stack van Azure-portal. Het adres van de Stack van Azure-portal is afhankelijk van welk Azure-Stack-product u een verbinding maakt:

* Voor Azure Stack Development Kit (ASDK) gaat u naar: https://portal.local.azurestack.external.
* Ga naar de URL die uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

1. Klik op **maken van een resource** in de linkerbovenhoek van de Stack van Azure-portal.

2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Klik op **Create**.

4. Typ de informatie van de virtuele machine. Bij **Verificatietype** selecteert u **Openbare SSH-sleutel**. Plakken in de openbare SSH-sleutel die u opgeslagen en klik vervolgens op **OK**.

   >[!NOTE]
 Zorg ervoor dat u eventuele witruimte voorloop- of volgspaties verwijderen voor deze sleutel.

   ![Basisbeginselen van het deelvenster - virtuele machine configureren](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Selecteer **D1_V2** voor de virtuele machine.

   ![Het formaat van Configuratiescherm - de grootte van een virtuele machine kiezen](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Op de **instellingen** pagina, behoud de standaardinstellingen en klik op **OK**.

7. Op de **samenvatting** pagina, klikt u op **OK** implementatie van virtuele machine te starten.

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Klik op **Connect** op de pagina van de virtuele machine. U ziet nu een SSH-verbindingsreeks die u moet verbinding maken met de virtuele machine.

   ![Verbinding maken met virtuele machine](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Open PuTTY.
3. Op de **PuTTY-configuratie** scherm u gebruikt de **categorie** venster omhoog of omlaag schuiven. Schuif omlaag naar **SSH**, vouw **SSH**, en klik vervolgens op **Auth**. Klik op **Bladeren** en kies het persoonlijke sleutelbestand dat u hebt opgeslagen.

   ![PuTTY persoonlijke sleutel selecteren](media/azure-stack-quick-linux-portal/Putty03.PNG)

4. Blader omhoog de **categorie** venster en klik vervolgens op **sessie**.
5. In de **hostnaam (of IP-adres)** vak, plak de verbindingsreeks die wordt weergegeven in de Stack van Azure-portal. In dit voorbeeld wordt de tekenreeks is ```asadmin@192.168.102.34```.

   ![PuTTY-configuratie-verbindingsreeks](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Klik op **openen** een sessie voor de virtuele machine te openen.

   ![Linux-sessie](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>De NGINX-webserver installeren

Gebruik de volgende bash-opdrachten om te werken pakket bronnen en het meest recente NGINX-pakket installeren op de virtuele machine.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Wanneer u klaar bent met het installeren van NGINX de SSH-sessie sluit en open de pagina overzicht van de virtuele machine in de Stack van Azure-portal.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Een netwerkbeveiligingsgroep (NSG) beveiligt binnenkomend en uitgaand verkeer. Wanneer een virtuele machine in de Stack van Azure-portal is gemaakt, wordt een inkomende regel gemaakt op poort 22 voor SSH-verbindingen. Omdat deze virtuele machine fungeert als host voor een webserver, moet een NSG-regel worden gemaakt voor webverkeer op poort 80.

1. Op de virtuele machine **overzicht** pagina, klikt u op de naam van de **resourcegroep**.
2. Selecteer de **netwerkbeveiligingsgroep** voor de virtuele machine. De NSG kan worden geïdentificeerd met behulp van de kolom **Type**.
3. Klik in het menu links onder **instellingen**, klikt u op **inkomende beveiligingsregels**.
4. Klik op **Add**.
5. Typ bij **Naam** **http**. Zorg ervoor dat het **poortbereik** is ingesteld op 80 en **Actie** is ingesteld op **Toestaan**.
6. Klik op **OK**

## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven

Met NGINX geïnstalleerd, en poort 80 is geopend op de virtuele machine, kunt u toegang tot de webserver met het openbare IP-adres van de virtuele machine. (Het openbare IP-adres wordt weergegeven op de pagina overzicht van de virtuele machine).

Open een webbrowser en blader naar ```http://<public IP address>```.

![NGINX web server welkomstpagina](media/azure-stack-quick-linux-portal/linux-04.PNG)

## <a name="clean-up-resources"></a>Resources opschonen

Opschonen van de resources die u niet langer nodig. Selecteer de resourcegroep op de pagina van de virtuele machine voor het verwijderen van de virtuele machine en de bijbehorende bronnen, en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, moet u een basic virtuele machine van een Linux-server met een webserver geïmplementeerd. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).
