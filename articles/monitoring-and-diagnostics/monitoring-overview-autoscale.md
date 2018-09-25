---
title: Overzicht van automatisch schalen in virtuele Machines, Cloud Services en Web-Apps
description: Als u automatisch schalen in een Microsoft Azure. Van toepassing op virtuele Machines, Virtual machine Scale sets, Cloud Services en Web-Apps.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: autoscale
ms.openlocfilehash: fe63ce931da9fbe94b47d00805820affddfb1bc1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946983"
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Overzicht van automatisch schalen in een Microsoft Azure Virtual Machines, Cloud Services en Web-Apps
Dit artikel wordt beschreven welke Microsoft Azure voor automatisch schalen is, de voordelen ervan, en hoe u aan de slag met behulp van deze.  

Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

> [!NOTE]
> Azure heeft twee methoden voor het automatisch schalen. Een oudere versie van automatisch schalen is van toepassing op virtuele Machines (beschikbaarheidssets). Deze functie heeft beperkte ondersteuning en het is raadzaam migreren naar virtuele-machineschaalsets voor ondersteuning sneller en betrouwbaarder voor automatisch schalen. Een koppeling voor het gebruik van de oudere technologie is opgenomen in dit artikel.  
>
>

## <a name="what-is-autoscale"></a>Wat is er voor automatisch schalen?
Automatisch schalen kunt u de juiste hoeveelheid resources die worden uitgevoerd voor het afhandelen van de belasting van uw toepassing. Hiermee kunt u resources voor het verwerken van de belasting toeneemt en bespaart ook geld door het verwijderen van resources die zijn zit toevoegen niet actief. U een minimum en maximum aantal exemplaren worden uitgevoerd en toevoegen of verwijderen van virtuele machines automatisch op basis van een set regels. Met een minimale maakt ervoor dat uw toepassing altijd wordt uitgevoerd zelfs zonder enige belasting. Met een beperkt mogelijk van uw totale kosten per uur. Automatisch schalen tussen deze twee extreme met behulp van regels die u maakt.

 ![Automatisch schalen is beschreven. VM's toevoegen en verwijderen](./media/monitoring-overview-autoscale/AutoscaleConcept.png)

Wanneer de regelvoorwaarden wordt voldaan, kan een of meer acties voor automatisch schalen worden geactiveerd. U kunt toevoegen en verwijderen van virtuele machines of andere acties worden uitgevoerd. De volgende conceptuele diagram ziet u dit proces.  

 ![Stroomdiagram voor automatisch schalen](./media/monitoring-overview-autoscale/Autoscale_Overview_v4.png)

De volgende uitleg geldt voor de onderdelen van het vorige diagram.   

## <a name="resource-metrics"></a>Metrische gegevens voor resources
Resources metrische gegevens verzenden, deze metrische gegevens later door regels worden verwerkt. Metrische gegevens worden geleverd via verschillende methoden.
Virtuele-machineschaalsets gebruik telemetriegegevens van Azure diagnostics-agents dat telemetrie voor Web-apps en cloudservices afkomstig rechtstreeks van de Azure-infrastructuur is. Enkele veelgebruikte statistische gegevens omvatten CPU-gebruik, geheugengebruik, aantallen threads, lengte van de wachtrij en schijfgebruik. Zie voor een lijst van welke telemetrische gegevens die u kunt gebruiken, [algemene metrische gegevens voor automatisch schalen](insights-autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Aangepaste metrische gegevens
U kunt ook gebruikmaken van uw eigen aangepaste metrische gegevens die uw toepassingen kunnen genereren. Als u uw toepassingen metrische gegevens verzenden naar Application Insights kunt u gebruikmaken van deze metrische gegevens om beslissingen te maken op al dan niet schalen of niet hebt geconfigureerd.

## <a name="time"></a>Time
Regels op basis van een schema zijn gebaseerd op UTC. U moet uw tijdzone correct ingesteld bij het instellen van uw regels.  

## <a name="rules"></a>Regels
Het diagram ziet u slechts één regel voor automatisch schalen, maar u kunt veel van deze hebben. U kunt complexe overlappende regels maken die nodig zijn voor uw situatie.  Regeltypen opnemen  

* **Op basis van metrische gegevens** -bijvoorbeeld doen met deze actie wanneer CPU-gebruik hoger dan 50 is %.
* **Op basis van tijd** - bijvoorbeeld: elke 8 uur op zaterdag in een bepaalde tijdzone van een webhook-trigger.

Regels op basis van metrische gegevens laden meten en toevoegen of verwijderen van virtuele machines op basis van die belasting. Regels op basis van een planning kunnen u om te schalen wanneer u tijd patronen in uw load en wilt aanpassen voordat u een verhoging van het mogelijke load of afname optreedt.  

## <a name="actions-and-automation"></a>Acties en automatisering
Regels kunnen een of meer soorten acties activeren.

* **Schaal** -schaal VM's in- of uitschalen
* **E-mailbericht** -e-mailbericht verzenden naar abonnementsbeheerders, co-beheerders en/of aanvullende e-mailadres die u opgeeft
* **Automatiseren via webhooks** -aanroepen van webhooks, die meerdere complexe acties binnen of buiten Azure kan activeren. In Azure, kunt u een Azure Automation-runbook, Azure-functie of logische App van Azure starten. Voorbeeld-URL van derden buiten Azure omvatten, zoals Slack en Twilio.

## <a name="autoscale-settings"></a>Instellingen voor automatisch schalen
Automatisch schalen gebruik de volgende terminologie en de structuur.

- Een **instelling voor automatisch schalen** wordt gelezen door de engine voor automatisch schalen om te bepalen of omhoog of omlaag schalen. Het bevat een of meer profielen, informatie over de doelresource en instellingen voor meldingen.

    - Een **profiel voor automatisch schalen** is een combinatie van a:

        - **capaciteit**, wat aangeeft dat het minimum, maximum en standaardwaarden voor het aantal exemplaren.
        - **verzameling van regels**, die elk een trigger (tijd of metrische gegevens) en een schaalactie (omhoog of omlaag) bevat.
        - **terugkeerpatroon**, waarmee wordt aangegeven wanneer automatisch schalen moet dit profiel tot stand is gebracht.

        U kunt meerdere profielen, waarmee u regelt verschillende overlappende vereisten hebben. U kunt profielen voor verschillende automatisch schalen voor verschillende tijdstippen van de dag of dagen van de week, bijvoorbeeld hebben.

    - Een **meldingsinstelling** wordt gedefinieerd welke meldingen moeten worden uitgevoerd als een gebeurtenis voor automatisch schalen op basis van die voldoet aan de criteria van een van de instelling voor automatisch schalen van profielen. Automatisch schalen kan een of meer e-mailadressen in kennis of aanroepen naar een of meer webhooks.


![Instelling voor automatisch schalen van Azure, het profiel en regelstructuur](./media/monitoring-overview-autoscale/AzureResourceManagerRuleStructure3.png)

De volledige lijst met velden kunnen worden geconfigureerd en beschrijvingen is beschikbaar in de [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931928.aspx).

Zie voor codevoorbeelden van

* [Geavanceerde configuratie van de voor automatisch schalen met behulp van Resource Manager-sjablonen voor VM-Schaalsets](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Horizontaal en verticaal schalen
Automatisch schalen alleen kan worden geschaald horizontaal, dit is een toename ('uit') of te verlagen ('in') in het aantal VM-exemplaren.  Horizontale biedt meer flexibiliteit in het geval van een cloud omdat hiermee om uit te voeren mogelijk duizenden VM's om belasting te verwerken.

Daarentegen, verschilt verticaal schalen. Het houdt het aantal virtuele machines die dezelfde, maar zorgt ervoor dat de virtuele machines ("u") meer of minder ('down') krachtige. Power wordt gemeten in het geheugen, CPU-snelheid, schijfruimte, enz.  Verticaal schalen heeft meer beperkingen. Dit is afhankelijk van de beschikbaarheid van grotere hardware die snel komt binnen via een bovenlimiet en per regio kan verschillen. Verticaal schalen, moet ook meestal een virtuele machine te stoppen en opnieuw starten.

Zie voor meer informatie, [verticaal schalen met Azure Automation virtuele Azure-machine](../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Methoden voor toegang
U kunt automatisch schalen via instellen

* [Azure Portal](insights-how-to-scale.md)
* [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
* [Platformonafhankelijke opdrachtregelinterface (CLI)](insights-cli-samples.md#autoscale)
* [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Ondersteunde services voor automatisch schalen
| Service | Schema en documenten |
| --- | --- |
| Web Apps |[Schalen van Web-Apps](insights-how-to-scale.md) |
| Cloud Services |[Automatisch schalen van een Service in de Cloud](../cloud-services/cloud-services-how-to-scale-portal.md) |
| Virtuele Machines: klassieke |[Beschikbaarheidssets voor klassieke virtuele machines schalen](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuele Machines: Windows-Schaalsets voor |[Schalen van virtuele-machineschaalset Hiermee stelt u in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Virtuele Machines: Schaalsets voor Linux |[Schalen van virtuele-machineschaalset Hiermee stelt u in Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuele Machines: Voorbeeld van de Windows |[Geavanceerde configuratie van de voor automatisch schalen met behulp van Resource Manager-sjablonen voor VM-Schaalsets](insights-advanced-autoscale-virtual-machine-scale-sets.md) |
| API Management-service|[Automatisch schalen van een Azure API Management-exemplaar](https://docs.microsoft.com/azure/api-management/api-management-howto-autoscale)

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over automatisch schalen, gebruik de Autoscale Walkthroughs eerder is vermeld, of raadpleegt u de volgende bronnen:

* [Azure Monitor voor automatisch schalen de algemene metrische gegevens](insights-autoscale-common-metrics.md)
* [Aanbevolen procedures voor automatisch schalen van Azure Monitor](insights-autoscale-best-practices.md)
* [Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook waarschuwingsmeldingen](insights-autoscale-to-webhook-email.md)
* [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931953.aspx)
* [Oplossen van problemen met Virtual Machine Scale Sets automatisch schalen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
