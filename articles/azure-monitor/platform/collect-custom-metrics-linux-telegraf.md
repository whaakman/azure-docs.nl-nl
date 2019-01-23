---
title: Aangepaste metrische gegevens verzamelen voor een Linux-VM met de agent InfluxData Telegraf
description: Aangepaste metrische gegevens verzamelen voor een Linux-VM met de agent InfluxData Telegraf
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 6944eb922250ac23aaae28997465af5a7c1e6032
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474724"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Aangepaste metrische gegevens verzamelen voor een Linux-VM met de agent InfluxData Telegraf

U kunt aangepaste metrische gegevens via de telemetriegegevens van uw toepassing, een agent die wordt uitgevoerd op uw Azure-resources of zelfs buitenaf in bewakingssystemen verzamelen met behulp van Azure Monitor. U kunt deze vervolgens verzenden rechtstreeks naar Azure Monitor. In dit artikel vindt u instructies voor het implementeren van de [InfluxData](https://www.influxdata.com/) Telegraf-agent op een Linux-VM in Azure en configureer de agent voor het publiceren van metrische gegevens naar Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf agent 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) is een plug-in-driven agent die kunt u het verzamelen van metrische gegevens uit meer dan 150 verschillende bronnen. Afhankelijk van wat workloads worden uitgevoerd op de virtuele machine, kunt u de agent als u wilt gebruikmaken van gespecialiseerde invoer plug-ins voor het verzamelen van metrische gegevens configureren. Voorbeelden zijn MySQL, NGINX en Apache. Met behulp van uitvoer-invoegtoepassingen, schrijven de agent vervolgens naar bestemmingen die u kiest. De agent Telegraf is rechtstreeks geïntegreerd met de Azure Monitor aangepaste metrische gegevens REST-API. Deze biedt ondersteuning voor een invoegtoepassing Azure Monitor-uitvoer. Met behulp van deze invoegtoepassing de agent kunt workload-specifieke metrische gegevens verzamelen over uw Linux-VM en legt deze als aangepaste metrische gegevens naar Azure Monitor. 

 ![Overzicht van de agent telegraaf](./media/collect-custom-metrics-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Aangepaste metrische gegevens verzenden 

Voor deze zelfstudie implementeren we een Linux-VM die het besturingssysteem Ubuntu 16.04 LTS wordt uitgevoerd. De agent Telegraf wordt ondersteund voor de meeste Linux-besturingssystemen. Debian- en RPM-pakketten zijn beschikbaar, samen met niet-ingepakte Linux-binaire bestanden op de [InfluxData downloadportal](https://portal.influxdata.com/downloads). Raadpleeg deze [Telegraf installatiehandleiding](https://docs.influxdata.com/telegraf/v1.8/introduction/installation/) voor aanvullende installatie-instructies en opties. 

Meld u aan bij [Azure Portal](https://portal.azure.com).

Maak een nieuwe Linux-VM: 

1. Selecteer de **een resource maken** optie in het navigatiedeelvenster links. 
1. Zoeken naar **virtuele Machine**.  
1. Selecteer **Ubuntu 16.04 LTS** en selecteer **maken**. 
1. Geef een VM-naam, zoals **MyTelegrafVM**.  
1. Laat u het schijftype als **SSD**. Geef vervolgens een **gebruikersnaam**, zoals **azureuser**. 
1. Voor **verificatietype**, selecteer **wachtwoord**. Voer een wachtwoord u gebruikt later SSH in deze virtuele machine. 
1. Kies aan **nieuwe resourcegroep maken**. Geef een naam, zoals **myResourceGroup**. Kies uw **locatie**. Selecteer vervolgens **OK**. 

    ![Maken van een Ubuntu-VM](./media/collect-custom-metrics-linux-telegraf/create-vm.png)

1. Selecteer een grootte voor de VM. U kunt filteren op **Rekentype** of **schijftype**, bijvoorbeeld. 

    ![Overzicht van virtuele machines grootte telegraaf agent](./media/collect-custom-metrics-linux-telegraf/vm-size.png)

1. Op de **instellingen** pagina **netwerk** > **Network Security Group**   >  ** Openbare binnenkomende poorten selecteren**, selecteer **HTTP** en **SSH (22)**. Laat de rest van de standaardwaarden en selecteer **OK**. 

1. Selecteer op de pagina overzicht **maken** de implementatie van de virtuele machine te starten. 

1. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Nadat de implementatie is voltooid, de samenvatting van de VM automatisch geopend. 

1. In het deelvenster van de virtuele machine, gaat u naar de **identiteit** tabblad. Zorg ervoor dat uw virtuele machine een systeem toegewezen identiteit ingesteld heeft op **op**. 
 
    ![Preview-versie van Telegraf VM identiteit](./media/collect-custom-metrics-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine 

Maak een SSH-verbinding met de VM. Selecteer de **Connect** knop op de overzichtspagina voor uw virtuele machine. 

![De pagina overzicht Telegraf VM](./media/collect-custom-metrics-linux-telegraf/connect-VM-button2.png)

In de **verbinding maken met virtuele machine** pagina, bewaart de standaardopties verbinding maken met de DNS-naam via poort 22. In **aanmelden met lokaal VM-account**, een verbindingsopdracht wordt weergegeven. Selecteer de knop kopiëren van de opdracht. Het volgende voorbeeld laat zien hoe de SSH-verbindingsopdracht eruitziet: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Plak de SSH-verbinding-opdracht in een shell, zoals Azure Cloud Shell of Bash in Ubuntu in Windows, of een SSH-client van uw keuze gebruiken om de verbinding te maken. 

## <a name="install-and-configure-telegraf"></a>Installeren en configureren van Telegraf 

Voer de volgende opdrachten uit de SSH-sessie voor het installeren van de Telegraf Debian-pakket naar de virtuele machine: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Het configuratiebestand van de Telegraf definieert Telegraf van bewerkingen. Een voorbeeld-configuratiebestand is standaard geïnstalleerd in het pad **/etc/telegraf/telegraf.conf**. Het voorbeeldconfiguratiebestand geeft een lijst van alle mogelijke invoer en uitvoer van invoegtoepassingen. Echter, we een aangepaste configuratie-bestand maken en de agent door het uitvoeren van de volgende opdrachten gebruiken: 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]  
> De bovenstaande code kunt u slechts twee invoegtoepassingen invoer: **cpu** en **geh**. U kunt meer invoer-invoegtoepassingen, afhankelijk van de werkbelasting die wordt uitgevoerd op uw computer kunt toevoegen. Voorbeelden zijn Docker, MySQL en NGINX. Zie voor een volledige lijst van de invoer-invoegtoepassingen, de **aanvullende configuratie** sectie. 

Ten slotte, als u wilt dat de agent met behulp van de nieuwe configuratie, wordt de agent gedwongen stoppen en starten door het uitvoeren van de volgende opdrachten: 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
De agent wordt nu metrische gegevens van elk van de invoer plug-ins opgegeven verzamelen en ze verzenden naar Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Tekenen van uw Telegraf metrische gegevens in Azure portal 

1. Open de [Azure Portal](https://portal.azure.com). 

1. Navigeer naar de nieuwe **Monitor** tabblad. Selecteer vervolgens **metrische gegevens**.  

     ![Controleren - metrische gegevens (preview)](./media/collect-custom-metrics-linux-telegraf/metrics.png)

1. Selecteer de virtuele machine in de lijst met resources.

     ![Grafiek met metrische gegevens](./media/collect-custom-metrics-linux-telegraf/metric-chart.png)

1. Selecteer de **Telegraf/CPU** naamruimte, en selecteer de **usage_system** metrische gegevens. U kunt kiezen om te filteren op de dimensies voor deze metrische gegevens of splitsen op deze.  

     ![Selecteer de naamruimte en metrische gegevens](./media/collect-custom-metrics-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Aanvullende configuratie 

De voorgaande procedure bevat informatie over het configureren van de agent Telegraf voor het verzamelen van metrische gegevens van een paar eenvoudige invoer-invoegtoepassingen. De agent Telegraf biedt ondersteuning voor meer dan 150 invoer-invoegtoepassingen, met enkele ondersteunende aanvullende configuratie-opties. InfluxData beschikbaar heeft gesteld een [lijst met ondersteunde Plug-ins](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) en instructies over [hoe u ze configureert](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Daarnaast wordt in dit scenario gebruikt u de agent Telegraf om te verzenden van metrische gegevens over de virtuele machine de agent wordt geïmplementeerd op. De agent Telegraf kan ook worden gebruikt als een collector en -doorstuurserver van metrische gegevens voor andere resources. Zie voor informatie over het configureren van de agent voor het verzenden van metrische gegevens voor andere Azure-resources, [uitvoer van Azure Monitor aangepaste metrische gegevens voor Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Resources opschonen 

Wanneer ze niet meer nodig zijn, kunt u de resourcegroep, de virtuele machine en alle gerelateerde resources verwijderen. Om dit te doen, selecteert u de resourcegroep voor de virtuele machine en selecteer **verwijderen**. Controleer vervolgens of de naam van de resourcegroep te verwijderen. 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).



