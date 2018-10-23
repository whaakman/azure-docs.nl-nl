---
title: Aangepaste metrische gegevens voor een Linux-VM met de InfluxData Telegraf Agent verzamelen
description: Aangepaste metrische gegevens voor een Linux-VM met de InfluxData Telegraf Agent verzamelen
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990702"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Aangepaste metrische gegevens voor een Linux-VM met de InfluxData Telegraf Agent verzamelen

Azure Monitor kunt u aangepaste metrische gegevens verzamelen via de telemetriegegevens van uw toepassing, een agent die wordt uitgevoerd op uw Azure-resources of zelfs buitenaf in bewakingssystemen en het verzenden van ze rechtstreeks naar Azure Monitor. In dit artikel richt zich op die instructies biedt over het implementeren van de [InfluxData](https://www.influxdata.com/) Telegraf-agent op een Linux-VM in Azure en configureer de agent voor het publiceren van metrische gegevens naar Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf Agent 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) is een invoegtoepassing gebaseerde agent die kunt u het verzamelen van metrische gegevens uit meer dan 150 verschillende bronnen. Afhankelijk van wat workloads worden uitgevoerd op de virtuele machine (ex.) MySQL, NGINX, Apache, enz.) u kunt de agent als u wilt gebruikmaken van gespecialiseerde invoer Plug-ins voor het verzamelen van metrische gegevens configureren. Invoegtoepassingen uitvoer en schakelt u de agent te schrijven naar bestemmingen van uw keuze. De agent Telegraf is rechtstreeks geïntegreerd met de Azure Monitor aangepaste metrische gegevens REST-API, en biedt ondersteuning voor een 'Azure Monitor-uitvoer-invoegtoepassing'. Hierdoor wordt de agent voor het verzamelen van specifieke metrische gegevens voor een werkbelasting op uw Linux-VM en deze als aangepaste metrische gegevens naar Azure Monitor te verzenden. 

 ![Overzicht van de Agent telegraaf](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Aangepaste metrische gegevens verzenden 

In deze zelfstudie implementeren we een Linux-VM met het besturingssysteem Ubuntu 16.04 LTS. De agent Telegraf wordt ondersteund voor de meeste Linux-besturingssystemen. Debian- en RPM-pakketten zijn beschikbaar, samen met niet-ingepakte Linux-binaire bestanden in de portal voor het downloaden van InfluxData. Zie deze installatiehandleiding voor extra Telegraf installatie-instructies en opties. 

Meld u aan bij de [Azure-Portal](https://portal.azure.com)

Een nieuwe Linux-VM maken: 

1. Klik op de **een resource maken** optie in het navigatiedeelvenster links. 
1. Zoeken naar *virtuele Machine*  
1. Selecteer *Ubuntu 16.04 LTS* en klikt u op **maken** 
1. Geef een VM-naam, zoals *MyTelegrafVM*.  
1. Laat u het schijftype als **SSD**, geeft u een **gebruikersnaam**, zoals *azureuser*. 
1. Voor *verificatietype*, selecteer **wachtwoord**, typt u in een wachtwoord dat u later SSH in deze virtuele machine gebruikt. 
1. Kies aan **nieuwe resourcegroep maken**, geeft u een naam, zoals *myResourceGroup*.  Kies de gewenste locatie, en selecteer vervolgens **OK**. 

     ![Ubuntu-VM maken](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. Selecteer een grootte voor de VM. U kunt bijvoorbeeld filteren op type Compute of schijf. 

     ![Grootte van de virtuele Machine Telegraph Agent-overzicht](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. Op de **instellingenpagina**in **netwerk** > **Network Security Group** > ** Openbare binnenkomende poorten selecteren**, selecteer *HTTP* en *SSH (22)*. Laat de rest van de standaardwaarden en selecteer **OK**. 

1. Selecteer op de pagina overzicht maken om de implementatie van de virtuele machine te starten. 

1. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend. 

1. Navigeer in de VM-blade naar de **identiteit** tabblad en zorg ervoor dat uw virtuele machine heeft een systeem toegewezen identiteit *op*. 
 
![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine 

Maak een SSH-verbinding met de VM. Selecteer de knop verbinding maken op de overzichtspagina voor uw virtuele machine. 

![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

In het verbinding maken met virtuele machine-pagina, behouden de standaardopties verbinding maken met de DNS-naam via poort 22. Bij aanmelden met behulp van de virtuele machine wordt lokaal account een verbindingsopdracht weergegeven. Klik op de knop om de opdracht te kopiëren. Het volgende voorbeeld laat zien hoe de SSH-verbindingsopdracht eruitziet: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Plak de SSH-verbinding in een shell, zoals de Azure Cloud Shell, Bash in Ubuntu in Windows, de opdracht of een SSH-client van uw keuze gebruiken om de verbinding te maken. 

## <a name="install-and-configure-telegraf"></a>Installeren en configureren van Telegraf 

Voer de volgende opdrachten uit de SSH-sessie voor het installeren van de Telegraf Debian-pakket naar de virtuele machine: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Het configuratiebestand van de Telegraf definieert Telegraf van bewerkingen. Een voorbeeld-configuratiebestand is standaard geïnstalleerd in het pad ' / etc/telegraf/telegraf.conf '. Het configuratiebestand van het voorbeeld van een lijst met alle mogelijke invoer en uitvoer van invoegtoepassingen. Echter, gaan we een aangepaste configuratie-bestand maken en de agent door het uitvoeren van de volgende opdrachten gebruiken 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> De bovenstaande alleen kan twee invoer invoegtoepassingen 'processor' en "geh", gerust om toe te voegen meer invoer invoegtoepassingen (Docker, MySQL, NGINX, enzovoort), afhankelijk van de werkbelasting wordt uitgevoerd op uw computer. Een volledige lijst van invoer invoegtoepassingen vindt u hier. 

Ten slotte, als u wilt dat de agent moet worden gestart met behulp van de nieuwe configuratie wordt de agent stoppen en starten door het uitvoeren van de volgende opdrachten gedwongen 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
De agent wordt nu metrische gegevens verzamelen uit elk van de opgegeven invoer-invoegtoepassingen en ze verzenden naar Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Tekenen van uw Telegraf metrische gegevens in Azure portal 

1. Open de [Azure-portal](https://portal.azure.com) 

1. Navigeer naar het nieuwe tabblad Monitor en selecteer vervolgens **metrische gegevens**.  
     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. Selecteer de virtuele machine in de Resourcekiezer

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. Selecteer de *Telegraf/CPU* naamruimte, en selecteer de *usage_system* metrische gegevens. U kunt filteren op de dimensies op deze metrische gegevens kiezen of splitsen op deze.  

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Aanvullende configuratie 

De bovenstaande procedure bevat informatie over het configureren van de agent Telegraf voor het verzamelen van metrische gegevens van een paar eenvoudige invoer invoegtoepassingen. De agent Telegraf biedt ondersteuning voor meer dan 150 invoer invoegtoepassingen, met enkele ondersteunende aanvullende configuratie-opties. InfluxData beschikbaar heeft gesteld een [lijst met ondersteunde Plug-ins](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) en instructies over [hoe u ze configureert](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

In dit scenario heeft bovendien mag dat u de agent Telegraf gebruiken om te verzenden van metrische gegevens over de virtuele machine op de agent is geïmplementeerd. De agent Telegraf kan ook worden gebruikt als een collector en -doorstuurserver van metrische gegevens voor andere resources. Zie voor informatie over het configureren van de agent voor het verzenden van metrische gegevens voor andere Azure-resources, [uitvoer van Azure Monitor aangepaste metrische gegevens voor Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Resources opschonen 

Wanneer u de VM niet meer nodig hebt, kunt u de resourcegroep, de machine zelf én alle gerelateerde resources verwijderen. Om dit te doen, selecteert u de resourcegroep voor de virtuele machine, selecteer verwijderen en controleer de naam van de resourcegroep te verwijderen. 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).


