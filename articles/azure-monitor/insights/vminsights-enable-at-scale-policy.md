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
ms.openlocfilehash: fda79a7ea361a6b44798d18b79ffd763055087a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122608"
---
# <a name="enable-azure-monitor-for-vms-preview-by-using-azure-policy"></a>Azure Monitor inschakelen voor virtuele machines (preview) met behulp van Azure Policy

Dit artikel wordt uitgelegd hoe u Azure Monitor voor virtuele machines (preview) inschakelen voor Azure virtual machines of virtuele-machineschaalsets met behulp van Azure Policy. Aan het einde van dit proces, u is geconfigureerd voor het inschakelen van de Log Analytics en de afhankelijkheid agents en virtuele machines geïdentificeerd die niet compatibel zijn.

Als u wilt detecteren en beheren en Azure Monitor inschakelen voor virtuele machines voor al uw Azure virtual machines of virtuele-machineschaalsets, kunt u Azure Policy of Azure PowerShell. Azure Policy is de methode die wordt aangeraden omdat u beleidsdefinities in uw abonnementen om ervoor te zorgen consistent naleving voor effectief beheren en automatisch inschakelen kunt van de nieuwe VM's ingericht. Deze beleidsdefinities:

* Implementeer de Log Analytics-agent en de agent voor afhankelijkheden.
* Rapport over nalevingsresultaten.
* Herstel voor niet-compatibele virtuele machines.

Als u geïnteresseerd bent in het uitvoeren van deze taken met Azure PowerShell of een Azure Resource Manager-sjabloon, Zie [Azure Monitor inschakelen voor virtuele machines (preview) met behulp van Azure PowerShell of Azure Resource Manager-sjablonen](vminsights-enable-at-scale-powershell.md).

## <a name="manage-policy-coverage-feature-overview"></a>Overzicht van Beleidsdekking-functies beheren

De ervaring met Azure-beleid voor het beheren en implementeren van de beleidsdefinities voor Azure Monitor voor virtuele machines is oorspronkelijk is gedaan uitsluitend uit Azure Policy. De functie Beleidsdekking beheren maakt het eenvoudiger en gemakkelijker te detecteren en beheren en op schaal inschakelen de **Azure Monitor inschakelen voor virtuele machines** initiatief, waaronder de beleidsdefinities die eerder vermeld. U kunt toegang tot deze nieuwe functie van de **aan de slag** tabblad in Azure Monitor voor virtuele machines. Selecteer **Beleidsdekking beheren** openen de **Azure Monitor voor virtuele machines Beleidsdekking** pagina.

![Azure Monitor van het tabblad virtuele machines aan de slag](./media/vminsights-enable-at-scale-policy/get-started-page-01.png)

Hier kunt u controleren en beheren van dekking voor het initiatief in uw beheergroepen en abonnementen. U kunt begrijpen hoeveel virtuele machines aanwezig zijn in elk van de beheergroepen, abonnementen en hun status van naleving.

![Azure Monitor voor beleid voor virtuele machines beheren pagina](./media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Deze informatie is nuttig om te plannen en uitvoeren van uw scenario governance voor Azure Monitor voor virtuele machines vanaf één centrale locatie. Azure Policy biedt een weergave naleving wanneer een beleid of initiatief is toegewezen aan een bereik, kunt u met deze nieuwe pagina ontdekken waar het beleid of initiatief is niet toegewezen en toewijzen in plaats. Alle acties, zoals toewijzen, weergeven en omleiden naar Azure Policy rechtstreeks bewerken. De **Azure Monitor voor virtuele machines Beleidsdekking** pagina is een uitgebreide en geïntegreerde ervaring voor alleen het initiatief **Azure Monitor inschakelen voor virtuele machines**.

Op deze pagina ook kunt u uw werkruimte voor logboekanalyse voor Azure Monitor voor virtuele machines, die:

- Hiermee installeert u de installatie van Service Map- en Infrastructure Insights-oplossingen.
- Hiermee kunt de prestatiemeteritems van het besturingssysteem die worden gebruikt door de van prestatiegrafieken, werkmappen en uw aangepaste logboeken-query's en waarschuwingen.

![Azure Monitor voor virtuele machines werkruimte configureren](./media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Deze optie is niet gerelateerd aan alle beleidsacties. Het is beschikbaar om op een gemakkelijke manier om te voldoen aan de [vereisten](vminsights-enable-overview.md) vereist voor het inschakelen van Azure Monitor voor virtuele machines.  

### <a name="what-information-is-available-on-this-page"></a>Welke informatie is beschikbaar op deze pagina?
De volgende tabel bevat een verdeling van de informatie die wordt weergegeven op de pagina beleid dekking en hoe worden begrepen.

| Function | Description | 
|----------|-------------| 
| **Bereik** | Beheergroep en abonnementen die u hebt of toegang tot met de mogelijkheid om in te zoomen via de beheerhiërarchie van de groep overgenomen.|
| **Rol** | Uw rol aan het bereik, die mogelijk lezer, de eigenaar of Inzender. In sommige gevallen deze kan leeg worden weergegeven om aan te geven die u toegang tot het abonnement hebt mogelijk maar niet aan de beheergroep hoort bij. Gegevens in andere kolommen, is afhankelijk van uw rol. De rol is essentieel bij het bepalen van welke gegevens u kunt zien en de acties die u uitvoeren kunt in termen van toewijzen van beleid of initiatieven (eigenaar), bewerken of weergeven van naleving. |
| **Totale aantal virtuele machines** | Aantal virtuele machines in dat bereik. Voor een beheergroep is de som van virtuele machines die worden genest onder de abonnementen of de onderliggende beheergroep. |
| **Dekking voor toewijzing** | Percentage van virtuele machines die worden gedekt door het beleid of initiatief. |
| **Toewijzingsstatus** | Informatie over de status van de toewijzing van beleid of initiatief. |
| **Compatibele VM 's** | Het aantal virtuele machines die compatibel zijn onder het beleid of initiatief. Voor het initiatief **Azure Monitor inschakelen voor virtuele machines**, dit is het aantal virtuele machines met zowel de Log Analytics-agent en de agent voor afhankelijkheden. In sommige gevallen kan deze mogelijk weergegeven leeg vanwege geen toewijzing, geen virtuele machines of onvoldoende machtigingen. Informatie wordt verstrekt onder **Nalevingsstatus**. |
| **Naleving** | Het nummer van de algemene compatibiliteit is de som van afzonderlijke resources die compatibel gedeeld zijn door de som van alle afzonderlijke resources. |
| **Nalevingsstatus** | Informatie over de nalevingsstatus voor de toewijzing van beleid of initiatief.|

Wanneer u het beleid of initiatief toewijst, kan het bereik dat is geselecteerd in de toewijzing van het bereik dat wordt vermeld of een subset van deze worden. Bijvoorbeeld, hebt u gemaakt een toewijzing voor een abonnement (beleidsbereik) en niet een beheergroep (dekking scope). In dit geval wordt de waarde van **toewijzing dekking** geeft aan dat de virtuele machines in het beleid of initiatief bereik gedeeld door de virtuele machines binnen het bereik van de dekking. In andere gevallen moet u mogelijk hebt uitgesloten enkele VM's, resourcegroepen of een abonnement van beleidsbereik. Als de waarde leeg is, betekent dit dat het beleid of initiatief bestaat niet of u bent niet gemachtigd. Informatie wordt verstrekt onder **toewijzingsstatus**.

## <a name="enable-by-using-azure-policy"></a>Inschakelen met behulp van Azure Policy

Azure Monitor inschakelen voor virtuele machines met behulp van Azure Policy in uw tenant:

- Het initiatief toewijzen aan een bereik: beheergroep, abonnement of resourcegroep.
- Bekijk en nalevingsresultaten herstellen.

Zie voor meer informatie over het toewijzen van Azure Policy [overzicht van Azure Policy](../../governance/policy/overview.md#policy-assignment) en bekijk de [overzicht van beheergroepen](../../governance/management-groups/index.md) voordat u doorgaat.

### <a name="policies-for-azure-vms"></a>Beleid voor virtuele machines in Azure

De beleidsdefinities voor een Azure-VM worden in de volgende tabel weergegeven.

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Azure Monitor voor virtuele machines inschakelen |Azure Monitor inschakelen voor de virtuele machines in het opgegeven bereik (beheergroep, abonnement of resourcegroep). Log Analytics-werkruimte wordt gebruikt als parameter. |Initiatief |
|[Preview]: Implementatie van de afhankelijkheid agent-VM-installatiekopie (OS) niet-vermelde controleren |Gerapporteerd als niet-compatibele VM's als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Implementatie van Log Analytics-agent: VM-installatiekopie (OS) niet-vermelde controleren |Gerapporteerd als niet-compatibele VM's als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor virtuele Linux-machines implementeren |Agent voor afhankelijkheden voor Linux-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor Windows-VM's implementeren |Agent voor afhankelijkheden voor Windows-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-agent voor Linux-VM's implementeren |Log Analytics-agent voor Linux-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-agent voor Windows-VM's implementeren |Log Analytics-agent voor Windows-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Beleid voor Azure-machine-schaalsets

De beleidsdefinities voor een schaalset voor virtuele Azure-machine worden in de volgende tabel weergegeven.

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Azure Monitor inschakelen voor virtuele-machineschaalsets |Azure Monitor inschakelen voor de virtuele-machineschaalsets in het opgegeven bereik (beheergroep, abonnement of resourcegroep). Log Analytics-werkruimte wordt gebruikt als parameter. Opmerking: Als uw scale set-Upgradebeleid is ingesteld op handmatig, de extensie is toegepast op alle virtuele machines in de set door het aanroepen van upgrade op deze. Dit is az vmss update-exemplaren in de CLI. |Initiatief |
|[Preview]: Afhankelijkheid implementatie van de agent in virtuele-machineschaalsets: VM-installatiekopie (OS) niet-vermelde controleren |Rapporten van virtuele-machineschaalset als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-agent implementeren in schaalsets voor virtuele machines – VM-installatiekopie (OS) niet-vermelde controleren |Rapporten van virtuele-machineschaalset als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor Linux VM-schaalsets implementeren |Implementeer de agent voor afhankelijkheden voor Linux virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Agent voor afhankelijkheden voor Windows VM-schaalsets implementeren |Implementeer de agent voor afhankelijkheden voor Windows-schaalsets voor virtuele machine als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-agent voor Linux VM-schaalsets implementeren |Implementeer Log Analytics-agent voor Linux virtuele-machineschaalsets als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |
|[Preview]: Log Analytics-agent voor Windows VM-schaalsets implementeren |Implementeer Log Analytics-agent voor Windows-schaalsets voor virtuele machine als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |

Beleid voor zelfstandige (niet opgenomen in het initiatief) wordt hier beschreven:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Log Analytics-werkruimte controleren voor VM-rapport verschil |Rapporteren als niet-compatibele VM's als ze worden niet van de Log Analytics-werkruimte die is opgegeven in de toewijzing van beleid of initiatief registreren. |Beleid |

### <a name="assign-the-azure-monitor-initiative"></a>De Azure Monitor-initiatief toewijzen
Het maken van de beleidstoewijzing van de **Azure Monitor voor virtuele machines Beleidsdekking** pagina, volgt u deze stappen. Zie voor meer informatie over hoe u deze stappen hebt voltooid, [een beleidstoewijzing maken vanuit Azure portal](../../governance/policy/assign-policy-portal.md).

Wanneer u het beleid of initiatief toewijst, kan het bereik dat is geselecteerd in de toewijzing worden de scope die hier worden vermeld of een subset van deze. Bijvoorbeeld, hebt u gemaakt een toewijzing voor het abonnement (beleidsbereik) en niet de beheergroep (dekking scope). Het percentage dekking zou in dit geval geven de VM's in het beleid of initiatief bereik wordt gedeeld door de virtuele machines in het bereik van de dekking. In andere gevallen moet u mogelijk hebt uitgesloten enkele VM's, of resourcegroepen of een abonnement van het beleidsbereik. Als deze leeg is, betekent dit dat het beleid of initiatief bestaat niet of u hebt geen machtigingen. Informatie wordt verstrekt onder **toewijzingsstatus**.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in de Azure portal, **Monitor**. 

3. Kies **virtuele Machines (preview)** in de **Insights** sectie.
 
4. Selecteer de **aan de slag** tabblad. Selecteer op de pagina **Beleidsdekking beheren**.

5. Selecteer een beheergroep of een abonnement uit de tabel. Selecteer **bereik** door het beletselteken (...) te selecteren. In het voorbeeld wordt een scope beperkt de beleidstoewijzing naar een groepering van virtuele machines voor afdwinging.

6. Op de **Azure Policy-toewijzing** pagina, dit is vooraf ingevuld met het initiatief **Azure Monitor inschakelen voor virtuele machines**. 
    De **opdrachtnaam** vak wordt automatisch gevuld met de naam van het initiatief, maar u kunt deze wijzigen. U kunt ook een optionele beschrijving toevoegen. De **toegewezen door** vakje wordt automatisch ingevuld op basis van die zich heeft aangemeld. Deze waarde is optioneel.

7. (Optioneel) Selecteer een of meer als resources wilt verwijderen uit het bereik, **uitsluitingen**.

8. In de **Log Analytics-werkruimte** vervolgkeuzelijst lijst voor de ondersteunde regio, selecteer een werkruimte.

   > [!NOTE]
   > Als de werkruimte buiten het bereik van de toewijzing valt, verleent *Inzender van Log Analytics* machtigingen voor de beleidstoewijzing Principal-ID. Als u dit niet doet, ziet u mogelijk een implementatiefout zoals `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` om toegang te verlenen, Bekijk [het handmatig configureren van de beheerde identiteit](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  De **beheerde identiteit** selectievakje is ingeschakeld, omdat het initiatief wordt toegewezen een beleid met omvat de *deployIfNotExists* effect.
    
9. In de **identiteit beheren locatie** vervolgkeuzelijst, selecteert u de juiste regio.

10. Selecteer **Toewijzen**.

Nadat u de toewijzing maakt de **Azure Monitor voor virtuele machines Beleidsdekking** pagina updates **toewijzing dekking**, **toewijzingsstatus**, **compatibel Virtuele machines**, en **Nalevingsstatus** om de wijzigingen weer te geven. 

De volgende matrix wijst elke mogelijke nalevingsstatus van het initiatief.  

| Nalevingsstatus | Description | 
|------------------|-------------|
| **Voldoen aan het beleid** | Alle virtuele machines binnen het bereik hebben de Log Analytics- en Afhankelijkheidsmonitors agents die aan hen is geïmplementeerd.|
| **Niet-compatibel** | Niet alle virtuele machines binnen het bereik hebben de met Log Analytics en afhankelijkheid agents geïmplementeerd en herstel mogelijk.|
| **Niet gestart** | Een nieuwe toewijzing is toegevoegd. |
| **Lock** | U hebt geen machtigingen aan de beheergroep. <sup>1</sup> | 
| **Leeg** | Er is geen beleid is toegewezen. | 

<sup>1</sup> als u geen toegang tot de beheergroep, vraagt u een eigenaar om toegang te bieden. Of, compatibiliteit en toewijzingen door het onderliggende beheergroepen of abonnementen beheren. 

De volgende tabel wordt de status van elke mogelijke toewijzing van het initiatief toegewezen.

| Toewijzingsstatus | Description | 
|------------------|-------------|
| **Geslaagd** | Alle virtuele machines binnen het bereik hebben de Log Analytics- en Afhankelijkheidsmonitors agents die aan hen is geïmplementeerd.|
| **Waarschuwing** | Het abonnement is niet onder een beheergroep.|
| **Niet gestart** | Een nieuwe toewijzing is toegevoegd. |
| **Lock** | U hebt geen machtigingen aan de beheergroep. <sup>1</sup> | 
| **Leeg** | Er zijn geen virtuele machines bestaat of een beleid is niet toegewezen. | 
| **Actie** | Een beleid toewijzen of bewerken van een toewijzing. | 

<sup>1</sup> als u geen toegang tot de beheergroep, vraagt u een eigenaar om toegang te bieden. Of, compatibiliteit en toewijzingen door het onderliggende beheergroepen of abonnementen beheren.

## <a name="review-and-remediate-the-compliance-results"></a>Controleren en herstellen van de compliantieresultaten

Het volgende voorbeeld is voor een Azure-VM, maar dit geldt ook voor virtuele-machineschaalsets. Zie voor meer informatie over het bekijken van nalevingsresultaten, [identificeren van niet-naleving resultaten](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Op de **Azure Monitor voor virtuele machines Beleidsdekking** pagina, selecteert u een beheergroep of een abonnement in de tabel. Selecteer **naleving weergeven** door het beletselteken (...) te selecteren.   

![Naleving van het beleid voor virtuele Azure-machines](./media/vminsights-enable-at-scale-policy/policy-view-compliance-01.png)

Op basis van de resultaten van de beleidsregels die zijn opgenomen in het initiatief, worden VM's gerapporteerd als niet-compatibel in de volgende scenario's:

* Log Analytics-agent of de agent voor afhankelijkheden is niet geïmplementeerd.  
    Dit scenario is gebruikelijk om een bereik met bestaande virtuele machines. Om te beperken het, implementeert u de vereiste agents per [het maken van herstel taken](../../governance/policy/how-to/remediate-resources.md) op een niet-compatibele beleid.  
    - [Preview]: Deploy Dependency agent for Linux VMs
    - [Preview]: Deploy Dependency agent for Windows VMs
    - [Preview]: Deploy Log Analytics agent for Linux VMs
    - [Preview]: Deploy Log Analytics agent for Windows VMs

* VM-installatiekopie (OS) wordt niet aangegeven in de beleidsdefinitie.  
    De criteria van het implementatiebeleid voor zijn alleen de VM's die zijn geïmplementeerd vanuit een bekende Azure VM-installatiekopieën. Raadpleeg de documentatie om te zien of het besturingssysteem van de virtuele machine wordt ondersteund. Als dit wordt niet ondersteund, dupliceren van de update en van implementatiebeleid of te wijzigen zodat de installatiekopie van het voldoen aan het beleid.  
    - [Preview]: Implementatie van de afhankelijkheid agent-VM-installatiekopie (OS) niet-vermelde controleren
    - [Preview]: Implementatie van Log Analytics-agent: VM-installatiekopie (OS) niet-vermelde controleren

* Virtuele machines zijn niet aangemeld bij de opgegeven Log Analytics-werkruimte.  
    Het is mogelijk dat sommige virtuele machines binnen het initiatief bereik zijn aangemeld bij een Log Analytics-werkruimte dan de naam die opgegeven in de beleidstoewijzing. Dit beleid is een hulpprogramma om te bepalen uit welke VM's rapporteren aan een niet-compatibele werkruimte.  
    - [Preview]: Audit Log Analytics workspace for VM – Report mismatch

## <a name="edit-an-initiative-assignment"></a>Een initiatieftoewijzing bewerken

Op elk gewenst moment nadat u een initiatief aan een beheergroep of het abonnement toewijzen, kunt u deze bewerken voor het wijzigen van de volgende eigenschappen:

- De naam van toewijzing
- Description
- Toegewezen door
- Log Analytics-werkruimte
- Uitzonderingen

## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machines, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines. 

- Zie voor meer informatie over het gebruik van de health-functie, [weergave Azure Monitor voor virtuele machines health](vminsights-health.md). 
- Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). 
- Zie voor het identificeren van knelpunten en het algehele gebruik met de prestaties van de virtuele machine, [weergave Azure VM-prestaties](vminsights-performance.md). 
- Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).
