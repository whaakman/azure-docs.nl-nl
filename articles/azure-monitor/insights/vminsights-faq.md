---
title: 'Azure Monitor voor virtuele machines (preview): veelgestelde vragen | Microsoft Docs'
description: Azure Monitor voor virtuele machines is een oplossing in Azure en combineert status en prestaties bewaken van het besturingssysteem van de virtuele machine van Azure, evenals automatisch detecteren van onderdelen van de toepassing en afhankelijkheden met andere resources en de communicatie tussen de kaarten ze. In dit artikel vindt u antwoorden op veelgestelde vragen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: magoedte
ms.openlocfilehash: f553a938c6329e21388ca68dea480b008e4e1363
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191561"
---
# <a name="azure-monitor-for-vms-preview-frequently-asked-questions"></a>Azure Monitor voor virtuele machines (preview): veelgestelde vragen
Dit Microsoft-FAQ is een lijst met veelgestelde vragen over Azure Monitor voor virtuele machines. Als u aanvullende vragen over de oplossing hebt, gaat u naar de [discussieforum](https://feedback.azure.com/forums/34192--general-feedback) en plaats uw vraag. Wanneer u een vraag is vaak wordt gevraagd, toevoegen we deze aan dit artikel zodat snel en eenvoudig kunnen worden gevonden.

## <a name="can-i-onboard-to-an-existing-workspace"></a>Kan ik toevoegen aan een bestaande werkruimte?
Als uw virtuele machines al met een Log Analytics-werkruimte verbonden zijn, u kunt blijven gebruiken die werkruimte wanneer het voorbereiden op Azure-Monitor voor virtuele machines, mits deze is in een van de ondersteunde regio's die worden vermeld [hier](vminsights-onboard.md#prerequisites).

Wanneer onboarding, we configureert prestatiemeteritems voor de werkruimte die zorgt ervoor dat alle virtuele machines waarvoor gegevens zijn gerapporteerd aan de werkruimte om te beginnen met het verzamelen van deze informatie voor het weergeven en analyseren in Azure Monitor voor virtuele machines.  Als gevolg hiervan, ziet u de prestatiegegevens van alle virtuele machines die zijn verbonden met de geselecteerde werkruimte.  De status en kaart-functies zijn alleen ingeschakeld voor de virtuele machines die u hebt opgegeven om te onboarden.

Voor meer informatie over welke prestaties prestatiemeteritems zijn ingeschakeld, raadpleegt u onze [onboarding](vminsights-onboard.md) artikel.

## <a name="can-i-onboard-to-a-new-workspace"></a>Kan ik toevoegen aan een nieuwe werkruimte? 
Als uw VM's momenteel niet met een bestaande Log Analytics-werkruimte verbonden zijn, moet u een nieuwe werkruimte maken voor het opslaan van uw gegevens.  Het maken van een nieuwe standaardwerkruimte gebeurt automatisch als u één Azure-VM voor Azure Monitor voor VM's via Azure portal configureren.

Als u ervoor kiest om de methode op basis van een script te gebruiken, als volgt worden behandeld in de [onboarding](vminsights-onboard.md) artikel. 

## <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Wat moet ik doen als mijn virtuele machine is al aan een bestaande werkruimte rapporteren?
Als u al gegevens van uw virtuele machines verzamelen, u mogelijk al hebt geconfigureerd het rapport gegevens naar een bestaande Log Analytics-werkruimte.  Als deze werkruimte zich in een van onze ondersteunde regio's, kunt u Azure Monitor inschakelen voor virtuele machines aan die vooraf bestaande werkruimte.  Als de werkruimte die u al gebruikt zich niet in een van onze ondersteunde regio's, kunt u zich niet voor Onboarding van Azure Monitor voor virtuele machines op dit moment.  We werken actief ter ondersteuning van extra regio's.

>[!NOTE]
>We configureert prestatiemeteritems voor de werkruimte die van invloed is op alle VM's die aan de werkruimte rapporteren, ongeacht of u hebt gekozen voor de onboarding ze naar Azure Monitor voor virtuele machines of niet. Raadpleeg voor meer informatie over hoe de prestatiemeteritems zijn geconfigureerd voor de werkruimte, onze [documentatie](../../azure-monitor/platform/data-sources-performance-counters.md). Raadpleeg voor informatie over de items die zijn geconfigureerd voor Azure Monitor voor virtuele machines, onze [inleidende documentatie](vminsights-onboard.md#performance-counters-enabled).  

## <a name="why-did-my-vm-fail-to-onboard"></a>Waarom is mijn virtuele machine voor onboarding mislukt?
Wanneer onboarding van een Azure-VM vanuit de Azure-portal, de volgende stappen plaats:

* Een standaard Log Analytics-werkruimte is gemaakt, als die optie hebt geselecteerd.
* De prestatiemeteritems zijn geconfigureerd voor de geselecteerde werkruimte. Als deze stap mislukt, ziet u dat sommige van de van prestatiegrafieken en tabellen gegevens worden niet voor de virtuele machine weergegeven onboarding. U kunt dit oplossen door het uitvoeren van het PowerShell-script beschreven [hier](vminsights-onboard.md#enable-with-powershell).
* De Log Analytics-agent is geïnstalleerd op virtuele Azure-machines met behulp van een VM-extensie, als dat nodig is vastgesteld.  
* De Azure-Monitor voor agent voor afhankelijkheden voor virtuele machines-kaart is geïnstalleerd op virtuele Azure-machines met de extensie, als dat nodig is vastgesteld.  
* Azure Monitor-onderdelen ondersteuning van de Health-functie zijn geconfigureerd, indien nodig, en de virtuele machine is geconfigureerd voor de gezondheid van rapportgegevens.

We controleren tijdens het onboarding-proces voor de status van elk van de bovenstaande om terug te keren van de meldingsstatus van een aan u in de portal. Configuratie van de werkruimte en de installatie van de agent duurt doorgaans 5 tot 10 minuten. Weergeven en de status van gegevens in de portal voor duren een aanvullende 5 tot 10 minuten.  

Als u onboarding hebt gestart en er berichten die aangeven dat de virtuele machine moet worden uitgevoerd, wacht u tot 30 minuten voor de virtuele machine om het proces te voltooien. 

## <a name="i-only-enabled-azure-monitor-for-vms-why-do-i-see-all-my-vms-monitored-by-the-health-feature"></a>Ik Azure Monitor alleen voor VM's, waarom zie ik alle mijn virtuele machines gecontroleerd door de Health-functie ingeschakeld?
De Health-functie is ingeschakeld voor alle virtuele machines die zijn verbonden met de Log Analytics-werkruimte, zelfs wanneer de actie voor een enkele virtuele machine wordt gestart.

## <a name="can-i-modify-the-schedule-for-when-health-criteria-evaluates-a-condition"></a>Kan ik de planning voor wanneer een voorwaarde wordt geëvalueerd in criteria voor beveiligingsstatus wijzigen?
Nee, de periode en de frequentie van criteria voor beveiligingsstatus kunnen niet worden gewijzigd met deze release. 

## <a name="can-i-disable-health-criteria-for-a-condition-i-dont-need-to-monitor"></a>Kan ik de van gezondheidscriteria voor een voorwaarde die ik niet nodig voor het bewaken van uitschakelen?
Criteria voor beveiligingsstatus kunnen niet worden uitgeschakeld in deze release.

## <a name="are-the-health-alert-severities-configurable"></a>De status waarschuwing ernstcategorieën configureerbare zijn?  
Status van de ernst van waarschuwing kan niet worden gewijzigd, ze kunnen alleen worden ingeschakeld of uitgeschakeld. Bovendien enkele dagen per week update waarschuwing gebaseerd op de status van de van gezondheidscriteria. 

## <a name="if-i-reconfigure-the-settings-of-a-particular-health-criteria-can-it-be-scoped-to-a-specific-instance"></a>Als ik de instellingen van een bepaalde status criteria configureren, kan deze worden toegewezen aan een specifiek exemplaar?  
Als u alle instellingen van een health criterium-instantie wijzigt, worden alle exemplaren van de gezondheid van criteria van hetzelfde type op de Azure VM gewijzigd. Bijvoorbeeld, als de drempelwaarde van de status van de vrije ruimte criterium schijfexemplaar die overeenkomt met de logische schijf C: wordt gewijzigd, geldt deze drempelwaarde voor alle andere logische schijven die zijn gedetecteerd en bewaakt voor dezelfde virtuele machine.

## <a name="does-the-health-feature-monitor-logical-processors-and-cores"></a>Controleert de status-functie logische processors en kernen?
Nee, afzonderlijke processor en logische processor op de gezondheid van criteria niet is opgenomen voor een Windows, totale CPU-gebruik wordt gecontroleerd om te evalueren effectief CPU druk op basis van het totale aantal logische CPU's beschikbaar voor de virtuele machine van Azure. 

## <a name="are-all-health-criteria-thresholds-configurable"></a>Zijn alle health criteria drempelwaarden kunnen worden geconfigureerd?  
Drempelwaarden voor de health-criteria die zijn gericht op een Windows-VM zijn gewijzigd, niet omdat hun statussen zijn ingesteld op *met* of *beschikbaar*. Wanneer u de status van een query de [Workload Monitor API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components), wordt de *vergelijkingsoperator* waarde van **LessThan** of **groter dan** met een *drempelwaarde* waarde van **4** voor de service of de entiteit als:
   - DNS-Client-Service Health-Service niet actief is. 
   - DHCP-client-service health-Service niet actief is. 
   - RPC-Service Health-Service niet actief is. 
   - Windows firewall-service health-Service niet actief is.
   - Windows-gebeurtenislogboek service health-Service niet actief is. 
   - Server-service health-Service niet actief is. 
   - Windows remote management-service health-Service wordt niet uitgevoerd. 
   - Bestandssysteemfout of beschadiging – logische schijf is niet beschikbaar.

Drempelwaarden voor de volgende criteria voor Linux-status zijn gewijzigd, niet omdat hun status is al ingesteld op *waar*. De status wordt weergegeven de *vergelijkingsoperator* met een waarde **LessThan** en *drempelwaarde* waarde van **1** wanneer opgevraagd uit de Workload bewakings-API voor de entiteit, afhankelijk van de context:
   - Status van logische schijf – logische schijf is niet online / beschikbaar
   - De Status van de schijf-schijf is niet online / beschikbaar
   - Status van Network Adapter - netwerkadapter is uitgeschakeld

## <a name="how-do-i-modify-alerts-that-are-included-with-the-health-feature"></a>Hoe wijzig ik waarschuwingen die opgenomen in de functie voor Health zijn?
Regels voor waarschuwingen die zijn gedefinieerd voor elk criterium status worden niet weergegeven in de Azure-portal. U kunt inschakelen of uitschakelen van een statuswaarschuwing voor de regel alleen in de [Workload Monitor API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). Bovendien kunt u niet toewijzen een [Azure Monitor actiegroep](../../azure-monitor/platform/action-groups.md) voor de gezondheid van waarschuwingen in Azure portal. U kunt alleen de melding instelling-API gebruiken voor het configureren van een actiegroep wordt geactiveerd wanneer een statuswaarschuwing wordt geactiveerd. Op dit moment kunt u actiegroepen op basis van een virtuele machine toewijzen zodat alle *statusmeldingen* dezelfde actiegroepen op basis van de VM-trigger wordt geactiveerd. In tegenstelling tot traditionele Azure-waarschuwingen is er geen concept van een aparte actiegroep voor elke status waarschuwingsregel. Bovendien worden alleen actiegroepen die zijn geconfigureerd voor e-mailadres of SMS-berichten ondersteund wanneer de gezondheid van waarschuwingen worden geactiveerd. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Ik zie niet sommige of alle gegevens in de prestatiegrafieken voor mijn virtuele machine
Als er geen prestatiegegevens in de tabel schijf of in enkele van de van prestatiegrafieken en vervolgens uw prestatiemeteritems kunnen niet worden geconfigureerd in de werkruimte. Als u wilt oplossen, voert u de volgende [PowerShell-script](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Hoe verschilt Azure Monitor voor de functie voor toewijzing van virtuele machines van Serviceoverzicht?
De Azure-Monitor voor de functie voor toewijzing van virtuele machines is gebaseerd op Service Map, maar heeft de volgende verschillen:

* De kaartweergave zijn toegankelijk vanaf de VM-beheerblade en van Azure Monitor voor virtuele machines in Azure Monitor.
* De verbindingen in de kaart zijn nu klikbaar en een weergave van de metrische gegevens van de verbinding in het deelvenster aan clientzijde voor de geselecteerde verbinding.
* Er is een nieuwe API die wordt gebruikt voor de toewijzingen voor betere ondersteuning van meer complexe kaarten maken.
* Bewaakte VM's zijn nu opgenomen in het knooppunt voor de client en het ringdiagram toont het deel van de bewaakte vs niet-bewaakte virtuele machines in de groep.  Het kan ook worden gebruikt voor het filteren van de lijst met computers wanneer de groep is uitgevouwen.
* Bewaakte virtuele machines zijn nu opgenomen in de knooppunten poort groep en het ringdiagram toont het deel van de bewaakte vs niet-gecontroleerde computers in de groep.  Het kan ook worden gebruikt voor het filteren van de lijst met computers wanneer de groep is uitgevouwen.
* De stijl van de kaart is meer consistent zijn met App-kaart van Application insights bijgewerkt.
* De deelvensters aan clientzijde zijn bijgewerkt, maar nog niet de volledige set met integratie van die werden ondersteund in de Serviceoverzicht - updatebeheer, bijhouden, beveiliging en -servicedesk. 
* De optie voor het kiezen van groepen en computers toe te wijzen, is bijgewerkt en biedt nu ondersteuning voor abonnementen, resourcegroepen, schaalsets van virtuele Azure-machine en cloudservices.
* U kunt nieuwe Serviceoverzicht machine-groepen maken in de Azure-Monitor voor de functie voor virtuele machines toewijzen.  

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Waarom kunnen mijn prestatiegrafieken stippellijnen weergeven?
Dit kan gebeuren een aantal oorzaken hebben.  In gevallen waarbij er een onderbreking in het verzamelen van gegevens wordt de stippellijn met regels weer.  Als u de samplingfrequentie gegevens voor de prestatiemeteritems ingeschakeld hebt gewijzigd (de standaardinstelling is het verzamelen van gegevens elke 60 seconden), ziet u de stippellijn in de grafiek als u een smal tijdsbereik voor de grafiek en de samplingfrequentie is kleiner dan de Bucketgrootte in de grafiek gebruikt (bijvoorbeeld de samplingfrequentie is elke 10 minuten en elke bucket op de grafiek is 5 minuten).  Het kiezen van een breder scala van de tijd om weer te geven, zou de regels van de grafiek wordt weergegeven als ononderbroken lijnen in plaats van punten in dit geval veroorzaken.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Worden groepen ondersteund met Azure Monitor voor virtuele machines?
Ja, zodra u de agent voor afhankelijkheden we gegevens verzamelen van de virtuele machines om weer te geven van de groepen op basis van abonnement, resourcegroep, virtuele machine schaalsets, en cloudservices.  Als u hebt gebruikt Serviceoverzicht en machine-groepen hebt gemaakt, worden deze ook weergegeven.  Computergroepen wordt ook weergegeven in het filter groepen als u deze hebt gemaakt voor de werkruimte die u bekijkt. 

## <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Hoe zien de details voor wat het 95e percentiel vraagt om een lijndiagram in het aggregaat prestaties?
Standaard wordt de lijst om weer te geven u de virtuele machines waarvoor de hoogste waarde voor het 95e percentiel voor de geselecteerde metrische gegevens, met uitzondering van de grafiek beschikbaar geheugen, waarin de machines met de laagste waarde van 5% van de gevallen gesorteerd.  Te klikken op de grafiek wordt geopend de **Top N lijst** weergeven met de juiste metriek geselecteerd.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>Hoe wordt de functie voor toewijzing dubbele IP-adressen in verschillende vnets en subnetten verwerkt?
Als u IP-adresbereiken dupliceert met virtuele machines of virtuele machine van Azure schaalsets in subnetten en vnets, kan dit leiden tot Azure-Monitor voor virtuele machines toewijzen om onjuiste gegevens weer te geven. Dit is een bekend probleem en we bekijken de opties om deze ervaring te verbeteren.

## <a name="does-map-feature-support-ipv6"></a>Ondersteuning voor IPv6-functies worden toegewezen?
Map-functie ondersteunt momenteel alleen IPv4 en we bekijken de ondersteuning voor IPv6. We ondersteunen ook IPv4 die wordt toegepast in IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Wanneer ik een toewijzing voor een resourcegroep of andere grote groep laadt is de kaart het moeilijk om weer te geven
Terwijl we verbeteringen in kaart aangebracht hebben voor het afhandelen van grote en complexe configuraties, realiseren we ons dat een kaart kan hebben een groot aantal knooppunten, verbindingen en knooppunt werkt als een cluster.  We zijn om verder te verbeteren van ondersteuning voor meer schaalbaarheid.   

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>Waarom ziet de grafiek netwerk op het tabblad prestaties er anders dan de grafiek netwerk op de pagina overzicht van Azure-VM?

De overzichtspagina voor een Azure-VM wordt weergegeven voor diagrammen op basis van de meting van de host van de activiteit in de Gast-VM.  Voor op de Azure VM-overzicht van de grafiek netwerk alleen wordt weergegeven netwerkverkeer dat wordt in rekening gebracht.  Dit omvat geen inter-vnet-verkeer.  De gegevens en grafieken weergegeven voor Azure Monitor voor virtuele machines is gebaseerd op gegevens van de Gast-VM en de netwerk-grafiek wordt weergegeven voor alle TCP/IP-verkeer dat binnenkomend en uitgaand naar die virtuele machine, met inbegrip van inter-vnet.

## <a name="are-their-limitations-if-i-am-on-the-log-analytics-free-pricing-plan"></a>Zijn de beperkingen ervan als ik op de Log Analytics gratis prijsstelling?
Als u Azure Monitor hebt geconfigureerd met een Log Analytics-werkruimte met de *gratis* prijscategorie, Azure Monitor voor de functie voor toewijzing van virtuele machines ondersteunen alleen vijf verbonden machines verbonden met de werkruimte. Als u vijf virtuele machines die zijn verbonden met een gratis werkruimte hebt, u verbinding te verbreken van de virtuele machines en later verbindt vervolgens een nieuwe virtuele machine, de nieuwe virtuele machine wordt niet bewaakt en doorgevoerd op de pagina overzicht.  

In dit geval wordt er een bericht met de **nu proberen** wanneer u de virtuele machine openen en selecteer de optie **inzichten (preview)** in het linkervenster, zelfs nadat deze is al is geïnstalleerd op de virtuele machine.  Echter, u niet gevraagd met opties als normaal gesproken optreden zou als deze virtuele machine niet toegevoegd aan Azure-Monitor voor virtuele machines zijn. 

## <a name="next-steps"></a>Volgende stappen
Beoordeling [ingebouwde Azure-Monitor voor virtuele machines](vminsights-onboard.md) voor informatie over vereisten en methoden voor bewaking van uw virtuele machines wilt inschakelen.
