---
title: Veelgestelde vragen over Azure Monitor voor virtuele machines (preview) | Microsoft Docs
description: Azure Monitor voor virtuele machines (preview) is een oplossing in Azure en combineert statussen en prestaties van het besturingssysteem van de virtuele machine van Azure. Automatisch detecteert onderdelen van de toepassing en afhankelijkheden met andere resources en de communicatie tussen deze toewijst. In dit artikel vindt u antwoorden op veelgestelde vragen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2018
ms.author: magoedte
ms.openlocfilehash: a97a7be0eaa8438a4df27b610106ec6ab9f60d30
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184386"
---
# <a name="azure-monitor-for-vms-preview-faq"></a>Azure Monitor voor virtuele machines (preview) Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Monitor voor virtuele machines. Als u aanvullende vragen over de oplossing hebt, gaat u naar de [Azure-discussieforum](https://feedback.azure.com/forums/34192--general-feedback) en plaats uw vraag. Bij vragen vaak worden gesteld, toevoegen we ze aan dit artikel, zodat ze snel en eenvoudig kunnen worden gevonden.

## <a name="can-i-deploy-vms-to-an-existing-workspace"></a>Kan ik virtuele machines implementeren op een bestaande werkruimte?
Als uw virtuele machines al met een Log Analytics-werkruimte verbonden zijn, kunt u echter ook doorgaan met deze werkruimte wanneer u deze naar Azure Monitor voor virtuele machines implementeren. De werkruimte moet bestaan in een van de ondersteunde regio's die worden vermeld in de sectie "Vereisten" van [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md#prerequisites).

Tijdens de implementatie configureren we prestatiemeteritems voor de werkruimte. Deze actie zorgt ervoor dat de virtuele machines die gegevens naar de werkruimte om te beginnen met het verzamelen van de gegevens voor weergeven en -analyse in Azure Monitor voor virtuele machines. Als gevolg hiervan, ziet u de prestatiegegevens van alle virtuele machines die zijn verbonden met de geselecteerde werkruimte. De status en kaart-functies zijn alleen voor de virtuele machines die u hebt opgegeven voor de implementatie ingeschakeld.

Zie voor meer informatie over de prestaties van welke prestatiemeteritems zijn ingeschakeld, [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md).

## <a name="can-i-deploy-vms-to-a-new-workspace"></a>Kan ik virtuele machines implementeren op een nieuwe werkruimte? 
Als uw VM's zijn niet met een bestaande Log Analytics-werkruimte verbonden, moet u een nieuwe werkruimte maken voor het opslaan van uw gegevens. U kunt een automatisch door het configureren van een enkele virtuele machine voor Azure Monitor voor virtuele machines in Azure portal maken.

Als u ervoor kiest om de methode op basis van een script te gebruiken, Zie [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md). 

## <a name="what-can-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Wat moet ik doen als mijn virtuele machine is al aan een bestaande werkruimte rapporteren?
Als u bent al gegevens verzameld van uw virtuele machines, u mogelijk al hebt geconfigureerd ze tot rapportgegevens aan een bestaande Log Analytics-werkruimte. Zolang deze werkruimte zich in een van onze ondersteunde regio's, kunt u Azure Monitor inschakelen voor virtuele machines voor die vooraf bestaande werkruimte. We werken actief ter ondersteuning van extra regio's.

>[!NOTE]
>We configureert prestatiemeteritems voor de werkruimte, die invloed hebben op alle VM's die aan de werkruimte rapporteren, ongeacht of u hebt ervoor gekozen om ze te implementeren naar Azure Monitor voor virtuele machines of niet. Zie voor meer informatie over hoe de prestatiemeteritems zijn geconfigureerd voor de werkruimte, de sectie "Configureren van prestatiemeteritems" van [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md). Zie voor meer informatie over de items die zijn geconfigureerd voor Azure Monitor voor virtuele machines [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md). 

## <a name="why-did-my-vm-deployment-fail"></a>Waarom is mijn VM-implementatie mislukt?
Wanneer u een Azure-VM vanuit de Azure portal implementeert, gebeurt het volgende:

* Een standaard Log Analytics-werkruimte is gemaakt, als die optie hebt geselecteerd.
* De prestatiemeteritems zijn geconfigureerd voor de geselecteerde werkruimte. Als deze stap mislukt, weergegeven geen enkele van de van prestatiegrafieken en tabellen gegevens voor de virtuele machine die u hebt geïmplementeerd. U kunt dit probleem oplossen door het uitvoeren van de PowerShell-script dat wordt beschreven in de sectie 'Inschakelen met PowerShell' van [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md#enable-with-powershell).
* De Log Analytics-agent is geïnstalleerd op virtuele Azure-machines met een VM-extensie, als dit is vereist. 
* De Azure-Monitor voor agent voor afhankelijkheden voor virtuele machines-kaart is geïnstalleerd op virtuele Azure-machines met de extensie, als dit is vereist. 
* Azure Monitor-onderdelen die ondersteuning bieden voor de Health-functie zijn geconfigureerd, indien nodig, en de virtuele machine is geconfigureerd voor de gezondheid van rapportgegevens.

Tijdens de implementatie van de status controleren voor elk van de voorgaande stappen we en geeft de meldingsstatus van een aan u in de portal. Configuratie van de werkruimte en de installatie van de agent duurt doorgaans 5 tot 10 minuten. Weergeven en de status van gegevens in Azure portal duren een extra 5 tot 10 minuten. 

Als u de implementatie hebt gestart en er berichten die aangeven dat de virtuele machine moet worden geïmplementeerd, kunt u tot 30 minuten voor de virtuele machine om het proces te voltooien. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Ik zie niet sommige of alle gegevens in de prestatiegrafieken voor mijn virtuele machine
Als prestatiegegevens niet wordt weergegeven in de tabel schijf of de van prestatiegrafieken, kunnen de prestatiemeteritems niet worden geconfigureerd in de werkruimte. U lost dit probleem, voer het PowerShell-script dat wordt beschreven in de sectie 'Inschakelen met PowerShell' van [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-the-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Hoe verschilt de Azure-Monitor voor de functie voor toewijzing van virtuele machines van de Service Map?
De Azure-Monitor voor de functie voor toewijzing van virtuele machines is gebaseerd op Service Map, maar heeft de volgende verschillen:

* U kunt de weergave van de kaart in het deelvenster van de virtuele machine en van Azure Monitor openen voor virtuele machines in Azure Monitor.
* De verbindingen in de kaart zijn nu klikbaar en verbinding metrische gegevens weergeven in het paneel aan clientzijde.
* Een nieuwe API wordt gebruikt om de toewijzingen voor betere ondersteuning van meer complexe kaarten maken.
* Bewaakte VM's zijn nu in het knooppunt voor de client en de ring diagram de verhouding van bewaakte naar niet-bewaakte virtuele machines geeft. U kunt ook de lijst met computers filteren als de groep wordt uitgevouwen.
* Bewaakte virtuele machines zijn nu in de knooppunten poort groep en het ringdiagram geeft de verhouding van bewaakte naar niet-bewaakte computers. U kunt ook de lijst met computers filteren als de groep wordt uitgevouwen.
* De stijl van de kaart is meer consistent zijn met het Toepassingsoverzicht van Azure Application Insights bijgewerkt.
* De deelvensters aan clientzijde zijn bijgewerkt, maar de volledige set integraties gebruikt die werden ondersteund in Serviceoverzicht nog hebt: Updatebeheer, bijhouden van wijzigingen, beveiliging en -servicedesk. 
* De optie voor het kiezen van groepen en computers toe te wijzen, is bijgewerkt. Ondersteunt nu abonnementen, resourcegroepen, schaalsets van virtuele Azure-machine en cloudservices.
* U kunt nieuwe Serviceoverzicht machine-groepen maken in de Azure-Monitor voor de functie voor virtuele machines toewijzen. 

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Waarom kunnen mijn prestatiegrafieken stippellijnen weergeven?

Worden prestatiegrafieken weergegeven stippellijnen in plaats van effen regels voor een aantal redenen:
* Er is mogelijk een onderbreking in het verzamelen van gegevens. 

* De standaardinstelling voor steekproef nemen van gegevens is elke 60 seconden. U ziet stippellijn als u een smal tijdsbereik voor de grafiek en de samplingfrequentie kleiner dan de Bucketgrootte in de grafiek gebruikt is. Stel dat u hebt ervoor gekozen een samplingfrequentie van 10 minuten en elke bucket op de grafiek is 5 minuten. In dit geval kiezen van een breder scala van de tijd om weer te geven, zou de regels van de grafiek wordt weergegeven als ononderbroken lijnen in plaats van punten veroorzaken.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Worden groepen ondersteund met Azure Monitor voor virtuele machines?
Ja, nadat u de agent voor afhankelijkheden we gegevens verzamelen van de virtuele machines om weer te geven op basis van abonnement, resourcegroep, groepen hebt geïnstalleerd virtuele machine schaalsets, en cloudservices. Als u hebt gebruikt Serviceoverzicht en machine-groepen hebt gemaakt, wordt deze groepen worden ook weergegeven. Computergroepen wordt ook weergegeven in het filter groepen als u deze hebt gemaakt voor de werkruimte die u bekijkt. 

## <a name="how-can-i-display-the-details-about-whats-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Hoe kan ik de details weergegeven over wat het 95e percentiel vraagt om een lijndiagram in het aggregaat prestaties?
De lijst is gesorteerd standaard, om u de virtuele machines waarvoor de hoogste waarde voor het 95e percentiel voor de geselecteerde metrische gegevens weer te geven. Een uitzondering hierop is de **beschikbaar geheugen** grafiek waarin de machines met de laagste waarde van het vijfde percentiel. Selecteer de grafiek om te openen de **Top N lijst** weergeven met de juiste metriek geselecteerd.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-various-virtual-networks-and-subnets"></a>Hoe wordt de functie voor toewijzing dubbele IP-adressen in verschillende virtuele netwerken en subnetten verwerkt?
Als u van IP-bereiken dupliceren bent met behulp van een van beide VM's of Azure virtuele-machineschaalsets in subnetten en virtuele netwerken, kan dit leiden tot de Azure-Monitor voor de functie voor toewijzing van virtuele machines om onjuiste gegevens weer te geven. We op de hoogte van dit probleem en we bekijken de opties om de ervaring te verbeteren.

## <a name="does-the-map-feature-support-ipv6"></a>De kaart-functie biedt ondersteuning voor IPv6?
De functie voor toewijzing ondersteunt momenteel alleen IPv4 en we bekijken de ondersteuning voor IPv6. We ondersteunen ook IPv4 die wordt toegepast in IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-why-is-the-map-difficult-to-view"></a>Wanneer ik een toewijzing voor een resourcegroep of andere grote groep laadt, waarom is de kaart moeilijk om weer te geven?
Hoewel de functie van de kaart voor het afhandelen van grote en complexe configuraties zijn verbeterd, realiseren we ons dat een kaart kan hebben veel knooppunten, verbindingen en knooppunten die als een cluster. We zijn om verder te verbeteren van ondersteuning voor meer schaalbaarheid.  

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-from-the-network-chart-on-the-azure-vm-overview-page"></a>Waarom de netwerk-grafiek op het tabblad prestaties zien er anders uit de grafiek netwerk op de pagina overzicht van Azure-VM?

De overzichtspagina voor een Azure-VM wordt weergegeven voor diagrammen op basis van de meting van de host van de activiteit in de Gast-VM. Het netwerk op de pagina overzicht van Azure-VM worden weergegeven. alleen netwerkverkeer dat wordt in rekening gebracht. In deze weergave bevat geen verkeer tussen virtuele netwerken. De gegevens en grafieken die wordt weergegeven voor Azure Monitor voor virtuele machines is gebaseerd op gegevens van de Gast-VM en de netwerk-grafiek wordt weergegeven voor alle TCP/IP-verkeer dat is binnenkomend en uitgaand naar die virtuele machine, met inbegrip van verkeer tussen virtuele netwerken.

## <a name="what-are-the-limitations-of-the-log-analytics-free-pricing-plan"></a>Wat zijn de beperkingen van de gratis Log-Analytics is het prijsplan?
Als u Azure Monitor met een Log Analytics-werkruimte hebt geconfigureerd met behulp van de *gratis* prijscategorie, ondersteunt de Azure-Monitor voor de functie voor toewijzing van VM's verbinden slechts vijf machines naar de werkruimte. 

Stel dat u hebt vijf virtuele machines die zijn verbonden met een gratis werkruimte. Als u één virtuele machine te verbreken en vervolgens later opnieuw verbinding maken met een nieuwe, niet de nieuwe virtuele machine bewaakt en doorgevoerd op de pagina overzicht. In dit scenario is wanneer u de nieuwe virtuele machine opent u wordt gevraagd om te gebruiken de **nu proberen** optie en selecteer **inzichten (preview)** in het linkerdeelvenster, zelfs nadat deze is geïnstalleerd op de virtuele machine. Echter, u wordt niet gevraagd met opties zoals u normaal zijn zou als de virtuele machine niet zijn geïmplementeerd naar Azure Monitor voor virtuele machines. 

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over de vereisten en -methoden voor het inschakelen van bewaking van uw virtuele machines, [implementeert Azure Monitor voor virtuele machines (preview)](vminsights-onboard.md).
