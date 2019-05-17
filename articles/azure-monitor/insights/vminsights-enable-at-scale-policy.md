---
title: Azure Monitor inschakelen voor virtuele machines met behulp van Azure Policy | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Monitor inschakelen voor VM's voor meerdere virtuele Azure-machines of virtuele-machineschaalsets met behulp van Azure Policy.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: magoedte
ms.openlocfilehash: 9664ad5272ffeb55bd85e3d4c4f4b70d05e97702
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524143"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-policy"></a>Azure Monitor inschakelen voor virtuele machines (preview) met behulp van Azure Policy

In dit artikel wordt uitgelegd hoe u Azure Monitor voor virtuele machines (preview) inschakelen voor Azure virtual machines of virtuele-machineschaalsets met behulp van Azure Policy. Aan het einde van dit proces, u is geconfigureerd voor het inschakelen van de Log Analytics en de afhankelijkheid agents, en virtuele machines geïdentificeerd die niet compatibel zijn.

Als u wilt detecteren en beheren en Azure Monitor inschakelen voor virtuele machines voor al uw Azure virtual machines of virtuele-machineschaalsets, kunt u Azure Policy of Azure PowerShell. Azure Policy is de aanbevolen methode omdat u beleidsdefinities in uw abonnementen om ervoor te zorgen consistent naleving voor effectief beheren en automatisch inschakelen kunt van de nieuwe VM's ingericht. Deze beleidsdefinities:

* Implementeer de Log Analytics-agent en de agent voor afhankelijkheden.
* Rapport over nalevingsresultaten.
* Herstel voor niet-compatibele virtuele machines.

Als u geïnteresseerd bent in bereiken met Azure PowerShell of Azure Resource Manager-sjabloon, Zie [inschakelen met behulp van Azure PowerShell of Resource Manager-sjabloon](vminsights-enable-at-scale-powershell.md). 

## <a name="manage-policy-coverage-feature-overview"></a>Overzicht van Beleidsdekking-functies beheren

De ervaring met Azure-beleid voor het beheren en implementeren van de beleidsdefinities voor Azure Monitor voor virtuele machines is oorspronkelijk uitgevoerd uitsluitend uit Azure Policy. Met de **Beleidsdekking beheren** functie, het maakt het eenvoudiger en gemakkelijker te detecteren en beheren en op schaal inschakelen de **Azure Monitor inschakelen voor virtuele machines** initiatief, waaronder de beleidsdefinities eerder is vermeld. U kunt toegang tot deze nieuwe functie van de **aan de slag** tabblad in Azure Monitor voor virtuele machines door het selecteren van **Beleidsdekking beheren**. De virtuele machines Beleidsdekking-pagina wordt geopend. 

![Azure Monitor van het tabblad virtuele machines aan de slag](./media/vminsights-enable-at-scale-policy/get-started-page-01.png)

Hier u kunt controleren en dekking voor het initiatief in uw beheergroepen en abonnementen beheren, evenals informatie over het aantal virtuele machines aanwezig zijn in elk van de beheergroepen, abonnementen en hun status van naleving.   

![Azure Monitor voor beleid voor virtuele machines beheren pagina](./media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Deze informatie is nuttig om te plannen en uitvoeren van uw scenario governance voor Azure Monitor voor virtuele machines vanaf één centrale locatie. Azure Policy biedt een weergave naleving wanneer een beleid/initiatief is toegewezen aan een bereik, kunt u met deze nieuwe pagina ontdekken waar het beleid/initiatief is niet toegewezen en deze in-place toewijzen. Alle acties (toewijzen, weergeven, bewerken) rechtstreeks naar Azure Policy omleiden. Azure Monitor voor virtuele machines Beleidsdekking pagina is een uitgebreide en geïntegreerde ervaring voor alleen het initiatief **Azure Monitor inschakelen voor virtuele machines**. 

Op deze pagina kunt u ook configureren werkruimte voor logboekanalyse voor Azure Monitor voor virtuele machines, die wordt uitgevoerd de volgende:

- Hiermee installeert de installatie van Service Map- en Infrastructure Insights-oplossingen
- Hiermee kunt de prestatiemeteritems van het besturingssysteem die worden gebruikt door de van prestatiegrafieken, werkmappen en uw aangepaste logboeken-query's en waarschuwingen.

![Azure Monitor voor virtuele machines werkruimte configureren](./media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Deze optie niet gerelateerd is aan alle acties en is beschikbaar om op een gemakkelijke manier om te voldoen aan de [vereisten](vminsights-enable-overview.md) vereist voor het inschakelen van Azure Monitor voor virtuele machines.  

### <a name="what-information-is-available-on-this-page"></a>Welke informatie is beschikbaar op deze pagina?
De volgende tabel bevat een verdeling van welke informatie wordt weergegeven in de pagina beleid dekking en hoe worden begrepen.

| Function | Description | 
|----------|-------------| 
| **Bereik** | Beheergroep en abonnementen die u hebt of overgenomen toegang tot de groep met de mogelijkheid om in te zoomen via de-hiërarchie.|
| **Rol** | Uw rol aan het bereik kan worden reader-eigenaar of Bijdrager. In sommige gevallen deze kan leeg worden weergegeven om aan te geven dat u toegang tot abonnement hebt mogelijk maar niet aan de beheergroep hoort bij. Gegevens in andere kolommen variëren afhankelijk van uw rol als sleutel bij het bepalen van welke gegevens u kunt zien en de acties die u uitvoeren kunt in termen van initiatief/beleid (eigenaar) toewijzen, bewerken of weergeven van naleving. |
| **Totale aantal virtuele machines** | Aantal virtuele machines in dat bereik. Voor een beheergroep is de som van virtuele machines die worden genest onder de abonnementen en/of de onderliggende beheergroep. |
| **Dekking voor toewijzing** | Percentage van virtuele machines die worden gedekt door initiatief/beleid. |
| **Toewijzingsstatus** | In deze kolom, vindt u informatie over de status van uw beleid / initiatief toewijzing. |
| **Compatibele VM 's** | Het aantal virtuele machines die compatibel zijn onder het beleid/initiatief. Voor het initiatief **Azure Monitor inschakelen voor virtuele machines** dit is het aantal virtuele machines met zowel de Log Analytics-agent en de agent voor afhankelijkheden. In sommige gevallen deze kan leeg worden weergegeven vanwege geen toewijzing, of geen virtuele machines of onvoldoende machtigingen. Informatie wordt verstrekt onder Nalevingsstatus. |
| **Naleving** | Het nummer van de algemene compatibiliteit is de som van afzonderlijke resources die compatibele gedeeld zijn door de som van alle afzonderlijke resources. |
| **Nalevingsstatus** | Meer informatie over de nalevingsstatus voor uw beleid / initiatief toewijzing.|

Wanneer u het initiatief/beleid toewijst, kan het bereik dat is geselecteerd in de toewijzing van het bereik dat wordt vermeld of een subset van deze worden. Bijvoorbeeld, hebt u gemaakt een toewijzing voor een abonnement (beleidsbereik) en niet een beheergroep (dekking scope). In dit geval wordt de waarde van **toewijzing dekking** aangeeft dat het bereik van virtuele machines in het beleid (of initiatief) gedeeld door virtuele machines binnen het bereik van de dekking. In andere gevallen moet u mogelijk hebt uitgesloten enkele VM's, resourcegroepen of een abonnement van beleidsbereik. Als de waarde leeg is, betekent dit dat het beleid/initiatief niet bestaat of u bent niet gemachtigd (informatie wordt verstrekt in de Status toewijzing).

## <a name="enable-using-azure-policy"></a>Inschakelen met Azure Policy

Azure Monitor inschakelen voor virtuele machines met behulp van Azure Policy in uw tenant:

- Het initiatief toewijzen aan een bereik: beheergroep, abonnement of resourcegroep
- Controleer en nalevingsresultaten herstellen

Zie voor meer informatie over het toewijzen van Azure Policy [overzicht van Azure Policy](../../governance/policy/overview.md#policy-assignment) en bekijk de [overzicht van beheergroepen](../../governance/management-groups/index.md) voordat u doorgaat.

### <a name="policies-for-azure-vms"></a>Beleid voor virtuele machines in Azure

De beleidsdefinities voor een Azure-VM worden vermeld in de volgende tabel:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Azure Monitor voor virtuele machines inschakelen |Azure Monitor inschakelen voor de virtuele Machines (VM's) in het opgegeven bereik (beheergroep, abonnement of resourcegroep). Log Analytics-werkruimte wordt gebruikt als parameter. |Initiatief |
|[Preview]: Implementatie van de afhankelijkheid Agent-VM-installatiekopie (OS) niet-vermelde controleren |Rapporten VM's als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Implementatie van Log Analytics-Agent: VM-installatiekopie (OS) niet-vermelde controleren |Rapporten VM's als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor virtuele Linux-machines implementeren |Agent voor afhankelijkheden implementeren voor Linux-VM's als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor Windows-VM's implementeren |Afhankelijkheid Agent voor de Windows VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-Agent voor Linux-VM's implementeren |Implementeer Log Analytics-Agent voor Linux-VM's als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-Agent voor Windows-VM's implementeren |Log Analytics-Agent voor Windows-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Beleid voor Azure-machine-schaalsets

De beleidsdefinities voor een schaalset voor virtuele machine van Azure worden vermeld in de volgende tabel:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Azure Monitor voor Schaalsets van virtuele machines (VMSS) inschakelen |Azure Monitor inschakelen voor de virtuele-machineschaalsets in het opgegeven bereik (Management groep, abonnement of resourcegroep). Log Analytics-werkruimte wordt als parameter. Opmerking: als uw scale set upgradePolicy is ingesteld op handmatig, moet u de extensie is toegepast op alle virtuele machines in de set door het aanroepen van upgrade op deze. Dit zou az vmss update-exemplaren zijn in de CLI. |Initiatief |
|[Preview]: Implementatie van de Agent in VMSS: VM-installatiekopie (OS) niet-vermelde afhankelijkheden controleren |Rapporten virtuele-machineschaalset als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Audit Log Analytics-Agent-implementatie in VMSS: VM-installatiekopie (OS) niet-vermelde |Rapporten virtuele-machineschaalset als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor virtuele Linux-machine-Schaalsets (VMSS) implementeren |Agent voor afhankelijkheden implementeren voor Linux virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor Windows VM-Schaalsets (VMSS) implementeren |Implementeer afhankelijkheid Agent voor Windows virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-Agent voor Linux VM-Schaalsets (VMSS) implementeren |Implementeer Log Analytics-Agent voor Linux virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-Agent voor Windows VM-Schaalsets (VMSS) implementeren |Implementeer Log Analytics-Agent voor Windows virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |

Beleid voor zelfstandige (niet opgenomen in het initiatief) wordt hier beschreven:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Audit Log Analytics-werkruimte voor VM - rapport verschil |Rapporteren als niet-compatibele VM's als ze worden niet van de Log Analytics-werkruimte die is opgegeven in de toewijzing van beleid/initiatief registreren. |Beleid |

### <a name="assign-the-azure-monitor-initiative"></a>De Azure Monitor-initiatief toewijzen
Voor het maken van de beleidstoewijzing van de Azure-Monitor voor virtuele machines Beleidsdekking pagina, moet u de volgende stappen uitvoeren. Zie voor meer informatie over hoe u deze stappen hebt voltooid, [een beleidstoewijzing maken vanuit Azure portal](../../governance/policy/assign-policy-portal.md).

Wanneer u het initiatief/beleid toewijst, kan het bereik dat is geselecteerd in de toewijzing worden de scope die hier worden vermeld of een subset van deze. Bijvoorbeeld, hebt u gemaakt toewijzing voor het abonnement (beleidsbereik) en niet de beheergroep (dekking bereik) waarin case dekking %, dit bereik van virtuele machines in het beleid (of initiatief duidt) gedeeld door virtuele machines binnen het bereik van de dekking. In sommige andere gevallen moet u mogelijk hebt uitgesloten sommige virtuele machines of RGs of abonnement van beleidsbereik.  Als deze leeg is, betekent dit dat het beleid voor beide / initiatief bestaat niet of u bent niet gemachtigd (gegevens die zijn opgegeven in de status van Clienttoewijzingen)  

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in de Azure portal, **Monitor**. 

3. Kies **virtuele Machines (preview)** in de **Insights** sectie.
 
4. Selecteer de **aan de slag** tabblad, en selecteer op de pagina **Beleidsdekking beheren**.

5. Selecteer een beheergroep of een abonnement uit de tabel en selecteer **bereik** door te klikken op de ellips (...).      In ons voorbeeld wordt een scope beperkt de beleidstoewijzing naar een groepering van virtuele machines voor afdwinging.

6. Op de **Azure Policy-toewijzing** pagina wordt dit vooraf ingevuld met het initiatief **Azure Monitor inschakelen voor virtuele machines**. 
    De **opdrachtnaam** vak wordt automatisch gevuld met de naam van het initiatief, maar u kunt deze wijzigen. U kunt ook een optionele beschrijving toevoegen. De **toegewezen door** vakje wordt automatisch ingevuld op basis van die zich heeft aangemeld, en deze waarde is optioneel.

7. (Optioneel) Selecteer een of meer als resources wilt verwijderen uit het bereik, **uitsluitingen**.

8. In de **Log Analytics-werkruimte** vervolgkeuzelijst lijst voor de ondersteunde regio, selecteer een werkruimte.

   > [!NOTE]
   > Als de werkruimte buiten het bereik van de toewijzing valt, verleent *Inzender van Log Analytics* machtigingen voor de beleidstoewijzing Principal-ID. Als u dit niet doet, ziet u mogelijk een implementatiefout zoals: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` Om toegang te verlenen, Bekijk [het handmatig configureren van de beheerde identiteit](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  De **beheerde identiteit** selectievakje is ingeschakeld, omdat het initiatief wordt toegewezen een beleid met omvat de *deployIfNotExists* effect.
    
9. In de **identiteit beheren locatie** vervolgkeuzelijst, selecteert u de juiste regio.

10. Selecteer **Toewijzen**.

Als u de toewijzing maakt, worden de Azure-Monitor voor virtuele machines Beleidsdekking pagina toewijzing dekking, Status van Clienttoewijzingen, naleving VM's en Nalevingsstatus als gevolg van de wijzigingen bijgewerkt. 

De volgende matrix wijst elke mogelijke nalevingsstatus van het initiatief.  

| Nalevingsstatus | Description | 
|------------------|-------------|
| **Voldoen aan het beleid** | Alle virtuele machines binnen het bereik hebben de Log Analytics- en Afhankelijkheidsmonitors agents die aan hen is geïmplementeerd.|
| **Niet-compatibel** | Niet alle virtuele machines binnen het bereik hebben de met Log Analytics en afhankelijkheid agents geïmplementeerd en herstel mogelijk.|
| **Niet gestart** | Een nieuwe toewijzing is toegevoegd. |
| **Lock** | U hebt niet voldoende bevoegdheden voor de beheergroep. <sup>1</sup> | 
| **Leeg** | Er is geen beleid is toegewezen. | 

<sup>1</sup> als u geen toegang tot de beheergroep, vraagt u een eigenaar toegang verlenen of compatibiliteit en toewijzingen door het onderliggende beheergroepen of abonnementen beheren. 

De volgende tabel wordt de status van elke mogelijke toewijzing van het initiatief toegewezen.

| Toewijzingsstatus | Description | 
|------------------|-------------|
| **Geslaagd** | Alle virtuele machines binnen het bereik hebben de Log Analytics- en Afhankelijkheidsmonitors agents die aan hen is geïmplementeerd.|
| **Waarschuwing** | Het abonnement is niet onder een beheergroep.|
| **Niet gestart** | Een nieuwe toewijzing is toegevoegd. |
| **Lock** | U hebt niet voldoende bevoegdheden voor de beheergroep. <sup>1</sup> | 
| **Leeg** | Geen virtuele machines bestaat of niet-toegewezen beleid. | 
| **Actie** | Toewijzing van een beleid toewijzen of bewerken | 

<sup>1</sup> als u geen toegang tot de beheergroep, vraagt u een eigenaar toegang verlenen of compatibiliteit en toewijzingen door het onderliggende beheergroepen of abonnementen beheren. 

## <a name="review-and-remediate-the-compliance-results"></a>Controleren en herstellen van de compliantieresultaten

Het volgende voorbeeld is voor een Azure-VM, maar ook van toepassing op virtuele-machineschaalsets. U kunt leren hoe u nalevingsresultaten controleren door te lezen [identificeren van niet-naleving resultaten](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Op de Azure-Monitor voor virtuele machines Beleidsdekking pagina, selecteert u een beheergroep of een abonnement in de tabel en selecteer **naleving weergeven** door te klikken op de ellips (...).   

![Naleving van het beleid voor virtuele Azure-machines](./media/vminsights-enable-at-scale-policy/policy-view-compliance-01.png)

Op basis van de resultaten van de beleidsregels die zijn opgenomen in het initiatief, worden VM's gerapporteerd als niet-compatibel in de volgende scenario's:

* Log Analytics of de agent voor afhankelijkheden is niet geïmplementeerd.  
    Dit scenario is gebruikelijk om een bereik met bestaande virtuele machines. Om te beperken het, implementeert u de vereiste agents per [het maken van herstel taken](../../governance/policy/how-to/remediate-resources.md) op een beleid voor niet-compatibel.  
    - [Preview]: Deploy Dependency Agent for Linux VMs
    - [Preview]: Deploy Dependency Agent for Windows VMs
    - [Preview]: Deploy Log Analytics Agent for Linux VMs
    - [Preview]: Deploy Log Analytics Agent for Windows VMs

* VM-installatiekopie (OS) wordt niet aangegeven in de beleidsdefinitie.  
    De criteria van het implementatiebeleid voor zijn alleen de VM's die zijn geïmplementeerd vanuit een bekende Azure VM-installatiekopieën. Raadpleeg de documentatie om te zien of het besturingssysteem van de virtuele machine wordt ondersteund. Als dit wordt niet ondersteund, dupliceren van de update en van implementatiebeleid of te wijzigen zodat de installatiekopie van het voldoen aan het beleid.  
    - [Preview]: Implementatie van de afhankelijkheid Agent-VM-installatiekopie (OS) niet-vermelde controleren
    - [Preview]: Implementatie van Log Analytics-Agent: VM-installatiekopie (OS) niet-vermelde controleren

* Virtuele machines zijn niet aangemeld bij de opgegeven Log Analytics-werkruimte.  
    Het is mogelijk dat sommige virtuele machines binnen het initiatief bereik zijn aangemeld bij een Log Analytics-werkruimte dan de naam die opgegeven in de beleidstoewijzing. Dit beleid is een hulpprogramma om te bepalen uit welke VM's rapporteren aan een niet-compatibele werkruimte.  
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch

## <a name="edit-initiative-assignment"></a>Initiatieftoewijzing bewerken

Op elk gewenst moment na een initiatief toewijzen aan een beheergroep of -abonnement, kunt u deze voor het wijzigen van de volgende eigenschappen bewerken:

- Toewijzingsnaam
- Description
- Toegewezen door
- Log Analytics-werkruimte
- Uitzonderingen

## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machines, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines. Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines Health](vminsights-health.md). Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). Voor het identificeren van knelpunten en het algehele gebruik met de prestaties van uw VM's, Zie [weergave Azure VM-prestaties](vminsights-performance.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).