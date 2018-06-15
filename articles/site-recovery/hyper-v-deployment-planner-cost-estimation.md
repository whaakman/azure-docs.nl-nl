---
title: Azure Site Recovery implementatie Planner schatting details kosten voor Hyper-V-Azure | Microsoft Docs
description: Dit artikel wordt beschreven kosten schatting details van een rapport dat is gegenereerd met behulp van Azure Site Recovery-implementatie plannen voor een Hyper-V op Azure scenario.
services: site-recovery
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: nisoneji
ms.openlocfilehash: 31461e70e81f0f48a8d67e31b98cfae2dd627a54
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
ms.locfileid: "29467175"
---
# <a name="cost-estimation-report-by-azure-site-recovery-deployment-planner"></a>Kosten schatting rapport door Azure Site Recovery-implementatie plannen 

Rapport van de Planner met implementatie van Azure Site Recovery biedt de kosten van samenvatting in schatting [aanbevelingen](hyper-v-deployment-planner-analyze-report.md#recommendations) bladen en gedetailleerde kostenanalyse in het eigenschappenvenster van de schatting van de kosten. Hier vindt u de uitgebreide kostenanalyse per VM. 

### <a name="cost-estimation-summary"></a>Samenvatting kostenramingen 
De grafiek toont de weergave Samenvatting van de kosten van geschatte totale disaster recovery (DR) naar Azure van uw doelregio gekozen en de valuta die u hebt opgegeven voor het genereren van rapporten.

![Samenvatting kostenramingen](media/hyper-v-azure-deployment-planner-cost-estimation/cost-estimation-summary-h2a.png)

De samenvatting helpt u bij het begrijpen van de kosten die u nodig hebt om te betalen voor opslag, compute, netwerk en licentie wanneer u uw compatibel VM's beveiligen met behulp van Azure Site Recovery. De kosten worden berekend voor compatibele virtuele machines en niet op de profiled virtuele machines. 
 
U kunt de kosten maandelijks of jaarlijks weergeven. Meer informatie over [ondersteunde doelregio's](./hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) en [ondersteunde valuta's](./hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Kosten per onderdelen**: de totale kosten voor Noodherstel is onderverdeeld in vier onderdelen: berekenings-, opslag-, netwerk- en Site Recovery-licentie kosten. De kosten wordt berekend op basis van het verbruik dat tijdens de replicatie en klik op Details voor Dr-tijd is ontstaan. COMPUTE, storage (premium en standard), de ExpressRoute/VPN-verbinding die is geconfigureerd tussen de lokale site en Azure en de Site Recovery-licentie worden gebruikt voor de berekeningen.

**Kosten per statussen**: de categorie totale disaster recovery (DR) kosten is gebaseerd op twee verschillende statussen: replicatie en details voor Dr. 

**Replicatie kosten**: de kosten die tijdens de replicatie is ontstaan. Deze heeft de kosten voor opslag, netwerk en de Site Recovery-licentie. 

**Details voor Dr-kosten**: de kosten die tijdens de testfailovers is ontstaan. Site Recovery draait virtuele machines tijdens de testfailover. De details voor Dr-kosten heeft betrekking op de actieve VM's berekenings- en kosten. 

**Azure-opslagkosten per maand-jaar**: de totale opslagkosten die voor premium en standard-opslag voor replicatie en details voor Dr is ontstaan.

## <a name="detailed-cost-analysis"></a>Gedetailleerde kostenanalyse
Azure prijzen voor berekening, opslag en netwerk verschillen voor verschillende Azure-regio's. U kunt een rapport van de schatting kosten genereren met de meest recente Azure prijzen op basis van uw abonnement, de aanbieding die zijn gekoppeld aan uw abonnement en het opgegeven doel-Azure-regio in een bepaalde valuta. Het hulpprogramma gebruikt standaard de Azure-regio VS - west 2 en als valuta de Amerikaanse dollar (USD). Als u alle andere regio en valuta, de volgende keer dat u een rapport zonder abonnements-ID, ID van de aanbieding doelregio en valuta genereren gebruikt, het hulpprogramma maakt gebruik van de prijzen van de laatst gebruikte doelregio en valuta voor kosten schatting.

Deze sectie ziet u het abonnement-ID en aanbieding-ID die u voor het genereren van rapporten gebruikt. Als ze niet gebruikt, is leeg.

In het hele rapport zijn de cellen die grijs is gemarkeerd als alleen-lezen. Cellen in wit kunnen worden gewijzigd volgens uw vereisten.

![Kosten schatting details](media/hyper-v-azure-deployment-planner-cost-estimation/cost-estimation1-h2a.png)

### <a name="overall-dr-costs-by-components"></a>Totale kosten voor Noodherstel door onderdelen
In de eerste sectie ziet u de totale DR-kosten per onderdelen en per statussen. 

**COMPUTE**: de kosten van IaaS VM's die worden uitgevoerd op Azure voor DR-behoeften. Virtuele machines die zijn gemaakt door Site Recovery tijdens DR zoomt (testfailovers) bevat. Dit omvat ook virtuele machines die worden uitgevoerd op Azure, zoals SQL Server met AlwaysOn-beschikbaarheidsgroepen en domeincontrollers of servers in de naam van het domein.

**Opslag**: de kosten van Azure storage-verbruik voor DR-behoeften. Deze omvatten de kosten voor het opslagverbruik voor replicatie en tijdens DR-oefeningen.

**Netwerk**: ExpressRoute en site-naar-site VPN-kosten voor DR-behoeften. 

**ASR licentie**: de Site Recovery-licentie kosten voor alle compatibele virtuele machines. Als u handmatig een virtuele machine in de tabel met gedetailleerde kosten analyse hebt ingevoerd, is de Site Recovery-licentie kosten ook opgenomen voor die VM.

### <a name="overall-dr-costs-by-states"></a>Totale kosten voor Noodherstel door statussen
De totale kosten voor Noodherstel is ingedeeld op basis van twee verschillende statussen: replicatie en details voor Dr.

**Replicatie**: de kosten die zijn gemaakt op het moment van replicatie. Deze heeft de kosten voor opslag, netwerk en de Site Recovery-licentie. 

**Details voor Dr**: de kosten die zijn gemaakt op het moment van DR oefeningen. Site Recovery draait virtuele machines tijdens DR zoomt. De DR-herstelanalyse kosten dekt berekenings- en kosten van de actieve virtuele machines.

* Totaal aantal DR-herstelanalyse duur in een jaar = het aantal DR zoomt x elke DR inzoomen duur (dagen)
* Gemiddelde DR-herstelanalyse kosten (per maand) = totaalkosten DR-herstelanalyse / 12

### <a name="storage-cost-table"></a>De tabel kosten opslag
Deze tabel ziet premium en standard-opslag-kosten voor replicatie en zoomt DR met en zonder kortingen.

### <a name="site-to-azure-network"></a>Site-naar-Azure-netwerk
Selecteer de gewenste instelling volgens uw vereisten. 

**ExpressRoute**: het hulpprogramma selecteert standaard het ExpressRoute-abonnement dat het meest overeenkomt met de vereiste netwerkbandbreedte voor replicatie van verschillen. U kunt het plan wijzigen volgens uw vereisten.

**Type VPN-Gateway**: Selecteer de Azure VPN-Gateway als u die in uw omgeving hebt. Dit is standaard niet van toepassing.

**Doelregio**: opgegeven Azure-regio voor herstel na Noodgevallen. De prijs die in het rapport wordt gebruikt voor het berekenen, de opslag, het netwerk en de licentie, is gebaseerd op de Azure-prijzen voor die regio. 

### <a name="vm-running-on-azure"></a>VM uitgevoerd op Azure
Mogelijk hebt u een domeincontroller of DNS VM of SQL Server-VM met AlwaysOn-beschikbaarheidsgroepen uitgevoerd op Azure voor herstel na Noodgevallen. U kunt opgeven dat het aantal virtuele machines en de grootte van de rekening houden met hun computer kosten van de totale kosten voor Noodherstel. 

### <a name="apply-overall-discount-if-applicable"></a>Algemene korting toepassen, indien van toepassing
Als u een Azure-partner of een klant en het recht om eventuele kortingen voor algemene Azure prijsinformatie, kunt u dit veld. Het hulpprogramma is de korting (in procenten) van toepassing op alle onderdelen.

### <a name="number-of-virtual-machines-type-and-compute-cost-per-year"></a>Aantal typen virtuele machines en de rekenkosten (per jaar)
Deze tabel ziet u het aantal vensters en niet - Windows virtuele machines en de DR-herstelanalyse kosten voor deze berekenen.

### <a name="settings"></a>Instellingen 
**Met beheerde schijf**: deze instelling bepaalt u of een beheerde schijf wordt gebruikt op het moment van DR oefeningen. De standaardwaarde is **Ja**. Als u instelt **- UseManagedDisks** naar **Nee**, de prijs onbeheerde schijf wordt gebruikt voor de berekening van de kosten.

**Valuta**: de valuta die voor het te genereren rapport wordt gebruikt.

**Duur van de kosten**: kunt u alle kosten voor de maand of voor het hele jaar weergeven. 

## <a name="detailed-cost-analysis-table"></a>Tabel Gedetailleerde kostenanalyse
![Gedetailleerde kostenanalyse](media/hyper-v-azure-deployment-planner-cost-estimation/detailed-cost-analysis-h2a.png)

De tabel bevat de verdeling van de kosten voor elke VM die compatibel is. U kunt deze tabel gebruiken om op te halen van de geschatte kosten van Azure DR nonprofiled VM's handmatig toevoegen aan virtuele machines. Deze informatie is nuttig in gevallen waarin u schatten Azure kosten voor een nieuwe DR-implementatie wilt zonder gedetailleerde profilering.

Handmatig VM's toevoegen:

1. Selecteer **rij invoegen** invoegen van een nieuwe rij tussen de **Start** en **End** rijen.

2. Vul in de volgende kolommen op basis van VM-grootte en het aantal virtuele machines die overeenkomen met deze configuratie: 

    a. **Aantal virtuele machines**

    b. **De grootte van IaaS (uw selectie)**

    c. **Opslagtype Standard/Premium**

    d. **Grootte van de totale opslagruimte VM (GB)**

    e. **Aantal DR-zoomt in een jaar**

    f. **De duur van elke DR-Herstelanalyse (dagen)**

    g. Type besturingssysteem

    h. **Gegevensredundantie**

    i. **Azure hybride gebruik Benefit**

3. U kunt dezelfde waarde voor alle VM's in de tabel toepassen door te selecteren **toepassen op alle** voor **nummer van DR-zoomt in een jaar**, **elke DR-Herstelanalyse duur (dagen)**, **gegevens redundantie**, en **Azure hybride gebruik voordeel**.

4. Selecteer **opnieuw berekenen kosten** bijwerken van de kosten.

**VM-naam**: de naam van de virtuele machine.

**Aantal VM's**: het aantal virtuele machines dat overeenkomt met de configuratie. Als een vergelijkbare configuratie van virtuele machines niet profiel maar beveiligd, kunt u het aantal bestaande virtuele machines bijwerken.

**De grootte van IaaS (aanbevolen)**: grootte van de VM-rol van de compatibel VM waarop het wordt aanbevolen. 

**De grootte van IaaS (uw selectie)**: de grootte is standaard hetzelfde zijn als de aanbevolen grootte van de VM-rol. U kunt de rol aanpassen aan uw behoefte. De kosten voor het rekenen zijn gebaseerd op de geselecteerde grootte van de VM-rol.

**Opslagtype**: het type opslag dat wordt gebruikt door de virtuele machine. Het is standaard of premium-opslag.

**De grootte van de totale VM-opslagruimte (GB)**: de totale opslag van de virtuele machine.

**Aantal DR-zoomt in een jaar**: het aantal keren uitvoeren van DR ingezoomd in een jaar. Het is standaard vier keer in een jaar. U kunt de periode voor specifieke virtuele machines te wijzigen of toepassen van de nieuwe waarde voor alle VM's. Voer de nieuwe waarde in de bovenste rij en selecteer **toepassen op alle**. Op basis van het aantal DR zoomt in een jaar en elke DR-herstelanalyse periode, wordt de totale kosten voor DR-herstelanalyse berekend. 

**De duur van elke DR-Herstelanalyse (dagen)**: de duur van elke details voor Dr. Standaard is 7 dagen om de 90 dagen volgens de [Disaster Recovery Software Assurance-voordeel](https://azure.microsoft.com/en-in/pricing/details/site-recovery). U kunt de periode voor specifieke virtuele machines wijzigen of u een nieuwe waarde kunt toepassen op alle virtuele machines. Voer een nieuwe waarde in de bovenste rij en selecteer **toepassen op alle**. De totale kosten voor DR-herstelanalyse wordt berekend op basis van het aantal DR zoomt in een jaar en elke DR-herstelanalyse periode.
 
**Type besturingssysteem**: het type besturingssysteem (OS) van de virtuele machine. Het is Windows of Linux. Als het type besturingssysteem Windows is, kan het voordeel van Azure hybride gebruik worden toegepast op die virtuele machine. 

**Gegevensredundantie**: kan het zijn lokaal redundante opslag, geografisch redundante opslag of geografisch redundante opslag met leestoegang. De standaardwaarde is lokaal redundante opslag. U kunt het type op basis van uw storage-account voor een specifieke virtuele machines wijzigen of kunt u het nieuwe type toepassen op alle virtuele machines. Het type van de bovenste rij wijzigen en selecteer **toepassen op alle**. De opslagkosten voor replicatie wordt berekend op basis van de prijs van de redundantie van de gegevens die u hebt geselecteerd. 

**Azure hybride gebruik voordeel**: kunt u het voordeel van Azure hybride gebruik toepassen op Windows-VM's, indien van toepassing. De standaardwaarde is **Ja**. U kunt de instelling voor specifieke virtuele machines wijzigen of u alle virtuele machines kunt bijwerken. Selecteer **toepassen op alle**.

**Totaal aantal Azure-verbruik**: de berekenings-, opslag- en Site Recovery-licentie kosten voor het herstel na Noodgevallen. Op basis van uw selectie, bevat het de kosten maandelijks of jaarlijks.

**Actieve status replicatie kosten**: de opslagkosten voor replicatie.

**Totale kosten voor DR-Herstelanalyse (gemiddeld)**: de berekenings- en kosten voor DR oefeningen.

**ASR licentie kosten**: de Site Recovery-licentie kosten.

## <a name="supported-target-regions"></a>Ondersteunde doelregio's
Site Recovery implementatie Planner biedt schatting van de kosten voor de volgende Azure-regio's. Als uw regio hier niet is vermeld, kunt u een van de volgende regio's waarvan prijzen die het dichtst bij uw regio is:

eastus, eastus2, westus, centralus, northcentralus, southcentralus, northeurope, westeurope, eastasia, southeastasia, japaneast, japanwest, australiaeast, australiasoutheast, brazilsouth, southindia, centralindia, westindia, canadacentral, canadaeast, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth 

## <a name="supported-currencies"></a>Ondersteunde valuta's
Site Recovery implementatie Planner kunt het kostenrapport met een van de volgende valuta's genereren.

|Valuta|Naam||Valuta|Naam||Valuta|Naam|
|---|---|---|---|---|---|---|---|
|ARS|Argentijnse peso ($)||AUD|Australische dollar ($)||BRL|Braziliaanse real (R$)|
|CAD|Canadese dollar ($)||CHF|Zwitserland frank (chf)||DKK|Deens kroon (kr)|
|EUR|Euro (€)||GBP|Engelse pond (£)||HKD|Hongkong dollar (HK$)|
|IDR|Indonesische roepia (Rp)||INR|Indiase roepia (₹)||JPY|Dividenddatum (¥)|
|KRW|Koreaans gewonnen (₩)||MXN|Mexico peso (MX$)||MYR|Maleisische ringgit (RM$)|
|NOK|Noors kroon (kr)||NZD|Nieuw-Zeeland dollar ($)||RUB|Russische roebel (руб)|
|SAR|Saudi riyal (SR)||SEK|Zweeds kroon (kr)||TWD|Taiwan dollar (NT$)|
|TRY|Turkse lira (TL)||USD| Amerikaanse dollar ($)||ZAR|Zuid-Afrika RNG (R)|

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het beveiligen van [Hyper-V-machines naar Azure met behulp van Site Recovery](hyper-v-azure-tutorial.md).
