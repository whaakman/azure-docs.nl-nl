---
title: Bekijk het rapport cost schatting in het Azure Site Recovery Deployment Planner | Microsoft Docs
description: In deze artikelen wordt beschreven hoe u het kosten ramings rapport in de Azure Site Recovery Deployment Planner voor VMware naar Azure nood herstel kunt controleren.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 7/29/2019
ms.author: mayg
ms.openlocfilehash: 1f825b67baf36c9a1a9187d555522f5a5955d1c7
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620071"
---
# <a name="review-the-cost-estimation-report-in-the-site-recovery-deployment-planner-for-vmware-disaster-recovery-to-azure"></a>Het kosten ramings rapport bekijken in de Site Recovery Deployment Planner voor VMware nood herstel naar Azure

De implementatieplanner geeft een samenvatting van de kostenramingen in werkbladen met [Aanbevelingen](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations). De details staan in het werkblad Kostenramingen. Hier vindt u de uitgebreide kostenanalyse per VM. 

>[!Note]
>De huidige versie van de Deployment planner tool v 2.5 biedt een schatting van de kosten voor het repliceren van Vm's naar Managed Disks.

### <a name="cost-estimation-summary"></a>Samenvatting kostenramingen 
De grafiek toont de weergave Samenvatting van de geschatte totale kosten voor noodherstel (DR) voor Azure van de gekozen doelregio en de valuta die u hebt opgegeven voor het genereren van rapporten.
Samenvatting kostenramingen

![Samenvatting kostenramingen](media/site-recovery-vmware-deployment-planner-analyze-report/cost-estimation-summary-v2a.png)

De samenvatting helpt u bij het begrijpen van de kosten die u nodig hebt om te betalen voor opslag, rekensnelheid, netwerk en licentie wanneer u alle compatibele virtuele machines naar Azure met Azure Site Recovery beveiligt. De kosten worden berekend op basis van compatibele virtuele machines en niet op basis van alle geprofileerde virtuele machines.  
 
U kunt de kosten maandelijks of jaarlijks weergeven. Meer informatie over [ondersteunde doelregio's](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-target-regions) en [ondersteunde valuta's](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-currencies).

**Kosten per onderdelen** De totale hoeveelheid DR-kosten is onderverdeeld in vier onderdelen: Kosten voor reken-, opslag-, netwerk-en Azure Site Recovery licenties. De kosten worden berekend op basis van het verbruik dat zal plaatsvinden tijdens de replicatie, en bij Noodherstel de tijd voor de rekensnelheid, opslag (premium en standard), de ExpressRoute/VPN die is geconfigureerd tussen de lokale site en Azure, en Azure Site Recovery-licentie.

**Kosten per statussen** De kosten voor een totaal noodherstel (DR) zijn is gebaseerd op twee verschillende statussen: Replicatie en Noodherstel. 

**Replicatie kosten**:  De kosten die worden gemaakt tijdens de replicatie. Dit dekt de kosten van opslag, netwerk en de Azure Site Recovery-licentie. 

**Dr-analyse kosten**: De kosten die worden gemaakt tijdens de testfailover. Azure Site Recovery laat virtuele machines draaien tijdens de testfailover. De details voor DR-kosten zijn de kosten voor de berekenings- en opslagkosten van de actieve virtuele machines. 

**Azure-opslagkosten per maand-jaar** Dit toont de totale opslagkosten die zullen worden gemaakt voor premium en standard-opslag voor replicatie en details voor DR.

## <a name="detailed-cost-analysis"></a>Gedetailleerde kostenanalyse
Azure-prijzen voor berekening, opslag, netwerk enz. variëren per Azure-regio. U kunt een kostenramingsrapport genereren met de meest recente Azure-prijzen op basis van uw abonnement, de aanbieding die aan uw abonnement is gekoppeld en voor de opgegeven Azure-doelregio in de opgegeven valuta. Het hulpprogramma gebruikt standaard de Azure-regio US - west 2 en als valuta de Amerikaanse dollar (USD). Als u een andere regio en valuta hebt gebruikt, worden de volgende keer wanneer u een rapport zonder abonnements-id, aanbiedings-id, doelregio en valuta genereert, de prijzen van de laatst gebruikte doelregio en valuta voor de raming van kosten gebruikt.
In deze sectie wordt het abonnements-ID en aanbiedings-ID weergegeven die u hebt gebruikt voor het genereren van rapporten.  Als u deze niet hebt gebruikt, is deze leeg.

Voor het hele rapport geldt dat de cellen die grijs zijn gemarkeerd, alleen-lezen zijn. Witte cellen kunnen worden gewijzigd overeenkomstig uw vereisten.

![Kostenraming - details1](media/site-recovery-hyper-v-deployment-planner-cost-estimation/cost-estimation1-h2a.png)

### <a name="overall-dr-cost-by-components"></a>Totale DR-kosten per onderdelen
In de eerste sectie ziet u de totale DR-kosten per onderdelen en per statussen. 

**Berekenen**: Kosten voor IaaS Vm's die op Azure worden uitgevoerd voor nood herstel. Hierbij inbegrepen zijn de kosten voor virtuele machines die zijn gemaakt door Azure Site Recovery tijdens DR-oefeningen (testfailovers) en voor virtuele machines die worden uitgevoerd op Azure, zoals SQL Server met AlwaysOn-beschikbaarheidsgroepen en domeincontrollers/DNS (Domain Name Servers).

**Opslag**: Kosten voor Azure-opslag verbruik voor DR-behoeften. Deze omvatten de kosten voor het opslagverbruik voor replicatie en tijdens DR-oefeningen.
Netwerk ExpressRoute en site-naar-site VPN-kosten voor DR-behoeften. 

**ASR-licentie**: Azure Site Recovery licentie kosten voor alle compatibele Vm's. Als u handmatig een virtuele machine in de tabel met de gedetailleerde kostenanalyse hebt ingevoerd, worden de kosten voor de licentie van Azure Site Recovery ook voor die VM inbegrepen.

### <a name="overall-dr-cost-by-states"></a>Totale DR-kosten per statussen
De total kosten voor DR zijn gebaseerd op twee verschillende statussen: replicatie en DR-oefening.

**Replicatie kosten**: De kosten worden berekend op het moment van replicatie. Dit dekt de kosten van opslag, netwerk en de Azure Site Recovery-licentie. 

**Dr-analyse kosten**: De kosten worden berekend op het moment van DR-oefeningen. Azure Site Recovery laat virtuele machines draaien tijdens DR-oefeningen. De kosten voor DR-oefeningen zijn de kosten voor de reken- en opslagkosten van de actieve virtuele machines.
Totaal duur van DR-oefeningen in = aantal DR-oefeningen x de duur van elke DR-oefening (dagen) Gemiddelde kosten DR-oefeningen (per maand) = totale kosten DR-oefeningen/12

### <a name="storage-cost-table"></a>Tabel met opslagkosten:
Deze tabel bevat de Premium- en Standard-opslagkosten die zijn gemoeid met replicatie en DR-oefeningen, met en zonder korting.

### <a name="site-to-azure-network"></a>Site-naar-Azure-netwerk
Selecteer de gewenste instelling overeenkomstig uw vereisten. 

**ExpressRoute**: Het hulp programma selecteert standaard het dichtstbijzijnde ExpressRoute-abonnement dat overeenkomt met de vereiste netwerk bandbreedte voor replicatie van verschillen. U kunt het abonnement wijzigen overeenkomstig uw vereisten.

**VPN gateway**: Selecteer de VPN Gateway als u in uw omgeving hebt. Dit is standaard niet van toepassing.

**Doel regio**: De Azure-regio voor DR is opgegeven. De prijs die in het rapport wordt gebruikt voor het berekenen, de opslag, het netwerk en de licentie, is gebaseerd op de Azure-prijzen voor die regio. 

### <a name="vm-running-on-azure"></a>VM uitgevoerd op Azure
Als u een domeincontroller of DNS-VM of SQL Server-VM met AlwaysOn-beschikbaarheidsgroepen hebt die op Azure voor DR worden uitgevoerd, kunt u het aantal virtuele machines en de grootte ervan opgeven, als u wilt dat de rekenkosten ervan worden meegenomen in de totale kosten voor DR. 

### <a name="apply-overall-discount-if-applicable"></a>Algemene korting toepassen, indien van toepassing
Als u een Azure-partner of -klant bent en recht hebt op een korting op algemene Azure-prijzen, kunt u dit veld gebruiken. Het hulpprogramma past de korting toe (in %) op alle componenten.

### <a name="number-of-virtual-machines-type-and-compute-cost-per-year"></a>Aantal typen virtuele machines en de rekenkosten (per jaar)
Deze tabel toont het aantal Windows- en niet-Windows-VM's, en voor elk ervan de rekenkosten voor de DR-oefening.

### <a name="settings"></a>Instellingen 

**Valuta**: De valuta waarin het rapport wordt gegenereerd. Duur van de kosten:  U kunt alle kosten voor de maand of voor het hele jaar weer geven. 

## <a name="detailed-cost-analysis-table"></a>Tabel Gedetailleerde kostenanalyse
![Gedetailleerde kostenanalyse](media/site-recovery-hyper-v-deployment-planner-cost-estimation/detailed-cost-analysis-h2a.png) In de tabel wordt de verdeling van de kosten over alle compatibele VM's weergegeven. Als u handmatig VM's toevoegt, kunt deze tabel ook gebruiken als u een kostenraming voor DR voor niet-geprofileerde VM's in Azure wilt zien. Dit is handig in gevallen waarin u Azure-kosten wilt ramen voor een nieuwe implementatie na een noodgeval, zonder dat er een gedetailleerde profilering wordt uitgevoerd.
Handmatig VM's toevoegen: 
1.  Klik op de knop Rij invoegen als u een nieuwe rij tussen de begin- en eindrijen wilt invoegen.

2.  Vul de volgende kolommen in op grond van de bij benadering bepaalde VM-grootte en het aantal VM's dat overeenkomt met de volgende configuratie: 

* Het aantal virtuele machines, de grootte van de IaaS (uw selectie)
* Het opslagtype (Standard/Premium)
* VM-totale opslag grootte (GB) van de bron machine
* Het aantal DR-oefeningen in een jaar 
* De duur van elke DR-oefening (dagen) 
* Het type besturingssysteem
* De gegevensredundantie 
* Azure Hybrid Benefit

1. U kunt dezelfde waarde toepassen op alle virtuele machines in de tabel door op de knop Op alles toepassen te klikken voor Aantal DR-oefeningen in een jaar, Duur van elke DR-oefening (dagen), Gegevensredundantie en Azure Hybrid Use Benefit.

1. Klik op Kosten opnieuw berekenen om de kosten bij te werken.

**VM-naam**: De naam van de virtuele machine.

**Aantal vm's**: Het aantal Vm's dat overeenkomt met de configuratie. U kunt het aantal bestaande virtuele machines bijwerken als vergelijkbare configuratie-VM's niet zijn geprofileerd maar toch worden beveiligd.

**Grootte van IaaS (aanbeveling)** : Het is de grootte van de VM-rol van de compatibele VM die door het hulp programma wordt aanbevolen. 

**Grootte van IaaS (uw selectie)** : Dit is standaard hetzelfde als de aanbevolen grootte van de VM-rol. U kunt de rol aanpassen aan uw behoefte. De kosten voor het rekenen zijn gebaseerd op de geselecteerde grootte van de VM-rol.

**Opslag type**: Het type opslag dat wordt gebruikt door de virtuele machine. Dit is Standard- of Premium-opslag.

**Totale opslag grootte VM (GB)** : De totale opslag van de bron-VM.

**Aantal Dr-oefeningen in een jaar**: Het aantal keren dat u DR-oefeningen in een jaar uitvoert. Dit is standaard vier keer per jaar. U kunt de periode voor specifieke virtuele machines wijzigen of op alle VM's toepassen door de nieuwe waarde in de bovenste rij in te voeren en op de knop Op alles toepassen te klikken. Op basis van het aantal DR-oefeningen in een jaar en de duur van elke DR-oefening worden de totale kosten van de DR-oefening berekend.  

**Elke duur van Dr-oefeningen (dagen)** : De duur van elke nood herstel analyse. Standaard is dit 7 dagen om de 90 dagen, overeenkomstig [Disaster Recovery Software Assurance](https://azure.microsoft.com/pricing/details/site-recovery). U kunt de periode voor specifieke virtuele machines wijzigen of op alle VM's toepassen door een nieuwe waarde in de bovenste rij in te voeren en op de knop 'Op alles toepassen' te klikken. De totale kosten voor DR-oefeningen worden berekend op basis van het aantal DR-oefeningen in een jaar en de duur van elke DR-oefening.
  
**Type besturings systeem**: Het type besturings systeem van de virtuele machine. Dit is Windows of Linux. Als het type besturingssysteem Windows is, kan Azure Hybrid Use Benefit op die VM worden toegepast. 

**Gegevens redundantie**: Dit kan een van de volgende zijn: lokaal redundante opslag (LRS), geografisch redundante opslag (GRS) of geografisch redundante opslag met lees toegang (RA-GRS). Standaard is LRS. U kunt het type wijzigen op basis van uw opslagaccount voor specifieke virtuele machines of u kunt het nieuwe type toepassen op alle virtuele machines door het type in de eerste rij te wijzigen en op de knop Op alles toepassen te klikken.  De opslagkosten voor replicatie worden berekend op basis van de prijs van de gegevensredundantie die u hebt geselecteerd. 

**Azure Hybrid Benefit**: U kunt Azure Hybrid Benefit Toep assen op Windows-Vm's, indien van toepassing.  Standaard is Ja. U kunt de instelling voor specifieke virtuele machines wijzigen of alle virtuele machines bijwerken door op de knop Op alles toepassen te klikken.

**Totaal Azure-verbruik**: Dit omvat reken-, opslag-en Azure Site Recovery licentie kosten voor uw DR. Afhankelijk van wat u hebt geselecteerd, worden hier de maandelijkse of jaarlijkse kosten weergegeven.

**Kosten voor stabiele status replicatie**: Dit omvat opslag kosten voor replicatie.

**Totale kosten voor Dr-analyse (gemiddeld)** : Het bevat reken-en opslag kosten voor DR-oefeningen.

**Kosten ASR-licentie**: Azure Site Recovery licentie kosten.

## <a name="supported-target-regions"></a>Ondersteunde doelregio's
De Azure Site Recovery-implementatieplanner biedt een schatting van de kosten voor de volgende Azure-regio's. Als uw regio hieronder niet wordt vermeld, kunt u een van de volgende regio's gebruiken waarvan de prijzen het dichtst bij die van uw regio liggen.

eastus, eastus2, westus, centralus, northcentralus, southcentralus, northeurope, westeurope, eastasia, southeastasia, japaneast, japanwest, australiaeast, australiasoutheast, brazilsouth, southindia, centralindia, westindia, canadacentral, canadaeast, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth 

## <a name="supported-currencies"></a>Ondersteunde valuta's
Met Azure Site Recovery Deployment Planner kunt u het kostenrapport genereren met een van de volgende valuta's.

|Currency|Name||Currency|Name||Currency|Name|
|---|---|---|---|---|---|---|---|
|ARS|Argentijnse peso ($)||AUD|Australische dollar ($)||BRL|Braziliaanse real (R$)|
|CAD|Canadese dollar ($)||CHF|Zwitserse frank. (chf)||DKK|Deense kroon (kr)|
|EUR|Euro (€)||GBP|Britse pond (£)||HKD|Hongkongse dollar (HK$)|
|IDR|Indonesische roepia (Rp)||INR|Indiase roepie (₹)||JPY|Japanse yen (¥)|
|KRW|Koreaanse won (₩)||MXN|Mexicaanse peso (MX$)||MYR|Maleisische ringgit (RM$)|
|NOK|Noorse kroon (kr)||NZD|Nieuw-Zeelandse dollar ($)||RUB|Russische roebel (руб)|
|SAR|Saudische riyal (SR)||SEK|Zweedse kroon (kr)||TWD|Taiwanese dollar (NT$)|
|TRY|Turkse lira (TL)||USD| Amerikaanse dollar ($)||ZAR|Zuid-Afrikaanse rand (R)|

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het beveiligen van [VMware-VM's naar Azure met Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-vmware-to-azure).
