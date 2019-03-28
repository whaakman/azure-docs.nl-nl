---
title: Replicatie van Azure naar Azure-architectuur in Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en architectuur die worden gebruikt bij het instellen van herstel na noodgevallen tussen Azure-regio's voor Azure-VM's met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 96873b5fdefc74893929f8150230118a162f195b
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540717"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Azure naar Azure disaster recovery-architectuur


In dit artikel beschrijft de architectuur, onderdelen en processen die worden gebruikt bij het implementeren van herstel na noodgevallen voor Azure virtual machines (VM's) met behulp van de [Azure Site Recovery](site-recovery-overview.md) service. Met herstel na noodgevallen instellen van repliceren Azure-VM's voortdurend vanaf naar een ander doel-regio. Als er een storing optreedt, kunt u virtuele machines een failover naar de secundaire regio, en ze van daaruit te openen. Wanneer alles wordt normaal gesproken nogmaals uit te voeren, kunt u een failback uitvoeren en blijven werken op de primaire locatie.



## <a name="architectural-components"></a>Architectuuronderdelen

De onderdelen die betrokken zijn bij herstel na noodgevallen voor Azure-VM's worden samengevat in de volgende tabel.

**Onderdeel** | **Vereisten**
--- | ---
**Virtuele machines in de regio van de gegevensbron** | Een van de meer Azure-VM's in een [bronregio ondersteund](azure-to-azure-support-matrix.md#region-support).<br/><br/> Virtuele machines kunnen worden uitgevoerd op elk [ondersteund besturingssysteem](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).
**Bron-VM-opslag** | Azure-VM's kunnen worden beheerd of niet-beheerde schijven verdeeld zijn over storage-accounts.<br/><br/>[Meer informatie over](azure-to-azure-support-matrix.md#replicated-machines---storage) Azure storage wordt ondersteund.
**Bron-VM-netwerken** | VM's kunnen zich bevinden in een of meer subnetten in een virtueel netwerk (VNet) in de bronregio. [Meer informatie](azure-to-azure-support-matrix.md#replicated-machines---networking) over netwerkvereisten.
**Cache-opslagaccount** | U moet een cache-opslagaccount in het Bronnetwerk. Tijdens de replicatie, worden wijzigingen van de virtuele machine in de cache opgeslagen voordat het wordt verzonden naar de doel-opslagaccount.<br/><br/> Met behulp van een cache zorgt ervoor dat de minimale gevolgen voor de productie-Apps die worden uitgevoerd op een virtuele machine.<br/><br/> [Meer informatie](azure-to-azure-support-matrix.md#cache-storage) over de vereisten voor cache-opslag. 
**Doelresources** | Doelresources worden gebruikt tijdens de replicatie, en wanneer er een failover optreedt. Site Recovery standaard doelresource kunt instellen, of u kunt maken/pas ze aan.<br/><br/> Controleer in de doelregio dat kunt u virtuele machines maken en uw abonnement heeft onvoldoende resources voor ondersteuning van VM-grootten die in de doelregio worden vereist. 

![Bron- en replicatie](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

## <a name="target-resources"></a>Doelresources

Wanneer u replicatie voor een virtuele machine inschakelt, biedt Site Recovery de mogelijkheid van de doelresources automatisch worden gemaakt. 

**Doelresource** | **Standaardinstelling**
--- | ---
**Doelabonnement** | Hetzelfde als de bronabonnement.
**Doelresourcegroep** | De resourcegroep waartoe virtuele machines na een failover behoren.<br/><br/> Kan het zijn in andere Azure-regio's met uitzondering van de bronregio.<br/><br/> Site Recovery maakt een nieuwe resourcegroep in de doelregio met het achtervoegsel 'asr'.<br/><br/>
**Doel-VNet** | Het virtuele netwerk (VNet) waarin gerepliceerde VM's zich na een failover. Een netwerktoewijzing wordt gemaakt tussen de bron- en virtuele netwerken, en vice versa.<br/><br/> Site Recovery maakt een nieuw VNet en subnet, met het achtervoegsel 'asr'.
**Doelopslagaccount** |  Als de virtuele machine niet een beheerde schijf gebruikt, is dit het opslagaccount waarvoor gegevens worden gerepliceerd.<br/><br/> Site Recovery maakt een nieuw opslagaccount in de doelregio voor het spiegelen van de bron-storage-account.
**Beheerde replicaschijven** | Als de virtuele machine gebruikmaakt van een beheerde schijf, is dit de beheerde schijven waarop gegevens worden gerepliceerd.<br/><br/> Site Recovery maakt beheerde replicaschijven in de storage-regio voor het spiegelen van de bron.
**Doelbeschikbaarheidssets** |  Beschikbaarheidsset in waarmee de replicerende virtuele machines bevinden zich na een failover.<br/><br/> Site Recovery maakt een beschikbaarheidsset voor virtuele machines die zich in een beschikbaarheidsset in de locatie van de in de doelregio met het achtervoegsel 'asr'. Als een beschikbaarheidsset bestaat, deze wordt gebruikt en een nieuw bestand is niet gemaakt.
**Doel-beschikbaarheidszones** | Als de doelregio ondersteuning voor beschikbaarheidszones biedt, toegewezen Site Recovery de dezelfde zone-nummer als die in de regio van de gegevensbron gebruikt.

### <a name="managing-target-resources"></a>Doelresources beheren

U kunt de doelresources zijn als volgt beheren:

- Als u replicatie inschakelt, kunt u doelinstellingen wijzigen.
- U kunt doelinstellingen wijzigen nadat replicatie is al werkt. De uitzondering hierop is het type beschikbaarheid (één exemplaar, set of zone). Als deze instelling wilt wijzigen, moet u replicatie uitschakelen, de instelling wijzigen en vervolgens weer inschakelen.



## <a name="replication-policy"></a>Beleid voor replicatie 

Wanneer u Azure VM-replicatie inschakelt, maakt Site Recovery standaard een nieuw replicatiebeleid met de standaardinstellingen in de tabel wordt samengevat.

**Beleidsinstelling** | **Details** | **Standaard**
--- | --- | ---
**Bewaarperiode voor herstelpunten** | Hiermee geeft u op hoe lang Site Recovery bewaart herstelpunten | 24 uur
**De frequentie van App-consistente momentopname** | Site Recovery maakt hoe vaak een app-consistente momentopname. | Om de 60 minuten.

### <a name="managing-replication-policies"></a>Replicatiebeleid beheren

U kunt beheren en wijzig de replicatie beleid standaardinstellingen als volgt te werk:
- Als u replicatie inschakelt, kunt u de instellingen wijzigen.
- U kunt een replicatiebeleid maken op elk gewenst moment en vervolgens toepassen wanneer u replicatie inschakelt.

### <a name="multi-vm-consistency"></a>Multi-VM-consistentie

Als u virtuele machines samen repliceren wilt, en u gedeelde crash-consistente en toepassingsconsistente na een failover herstelpunten, kunt u ze samen verzamelen in een replicatiegroep. Multi-VM-consistentie heeft gevolgen voor prestaties van de werkbelastingen en mag alleen worden gebruikt voor virtuele machines waarop werkbelastingen waarvoor consistentiecontrole voor alle machines worden uitgevoerd. 



## <a name="snapshots-and-recovery-points"></a>Momentopnamen en herstelpunten

Herstelpunten zijn gemaakt op basis van momentopnamen van VM-schijven die zijn uitgevoerd op een bepaald punt in tijd. Wanneer u een virtuele machine failover, gebruikt u een herstelpunt te herstellen van de virtuele machine op de doellocatie.

Wanneer Failover-overschakeling uitvoeren, willen we in het algemeen om ervoor te zorgen dat de virtuele machine wordt gestart zonder beschadiging of verlies van gegevens, en dat de VM-gegevens is consistent voor het besturingssysteem en voor apps die worden uitgevoerd op de virtuele machine. Dit is afhankelijk van het type van de momentopnamen die zijn gemaakt.

Site Recovery maakt momentopnamen als volgt:

1. Als u een frequentie voor hen opgeeft, site Recovery crash-consistente momentopnamen van gegevens wordt standaard en app-consistente momentopnamen.
2. Herstelpunten zijn gemaakt op basis van de momentopnamen en opgeslagen in overeenstemming met de instellingen voor het bewaren in het replicatiebeleid.

### <a name="consistency"></a>Consistentie

De volgende tabel beschrijft de verschillende typen consistentie.

### <a name="crash-consistent"></a>Crashconsistent

**Beschrijving** | **Details** | **Aanbeveling**
--- | --- | ---
Een crash-consistente momentopname bevat gegevens die op de schijf was toen de momentopname werd gemaakt. Deze bevat geen iets in het geheugen.<br/><br/> Het bevat het equivalent van de gegevens op de schijf die aanwezig zijn als de virtuele machine is vastgelopen of het netsnoer is opgehaald uit de server op het moment dat de momentopname werd gemaakt.<br/><br/> Een crash-consistente biedt geen garantie voor de consistentie van gegevens voor het besturingssysteem, of voor apps op de virtuele machine. | Site Recovery maakt crash-consistente herstelpunten om de vijf minuten standaard. Deze instelling kan niet worden gewijzigd.<br/><br/>  | De meeste apps kunnen vandaag, herstellen en van crash-consistente punten.<br/><br/> Crash-consistente herstelpunten zijn meestal voldoende zijn voor de replicatie van besturingssystemen en apps, zoals DHCP-servers en afdrukservers.

### <a name="app-consistent"></a>Toepassingsconsistent

**Beschrijving** | **Details** | **Aanbeveling**
--- | --- | ---
App-consistente herstelpunten zijn gemaakt op basis van app-consistente momentopnamen.<br/><br/> Een app-consistente momentopname bevatten alle gegevens in een crash-consistente momentopname, plus alle gegevens in het geheugen en transacties in uitvoering. | App-consistente momentopnamen gebruiken Volume Shadow Copy Service (VSS):<br/><br/>   (1) VSS uitvoeren als een momentopname wordt gestart, een bewerking van kopiëren bij schrijven (betreft) op het volume.<br/><br/>   2) voordat deze wordt uitgevoerd de betreft, informeert VSS elke app op de computer die de gegevens in het geheugen geladen leegmaken naar schijf nodig.<br/><br/>   3) de back-ups/noodherstel recovery-app kan VSS vervolgens (in dit geval Site Recovery) om te lezen van de momentopname van de gegevens en doorgaan. | App-consistente momentopnamen worden gemaakt in overeenstemming met de frequentie die u opgeeft. Deze frequentie moet altijd kleiner dan u herstelpunten worden bewaard. Bijvoorbeeld, als u met behulp van de standaardinstelling van 24 uur herstelpunten behoudt, moet u de frequentie instellen op minder dan 24 uur.<br/><br/>Ze meer complexe en het langer duren om dan crash-consistente momentopnamen.<br/><br/> Ze invloed op de prestaties van apps die worden uitgevoerd op een virtuele machine ingeschakeld voor replicatie. 

## <a name="replication-process"></a>Replicatieproces

Wanneer u replicatie voor een Azure-VM inschakelt, gebeurt het volgende:

1. De Site Recovery Mobility service-extensie wordt automatisch geïnstalleerd op de virtuele machine.
2. De extensie wordt de virtuele machine geregistreerd met Site Recovery.
3. Continue replicatie wordt gestart voor de virtuele machine.  Schrijfbewerkingen worden onmiddellijk overgebracht naar de cache-opslagaccount in de bronlocatie.
4. Site Recovery verwerkt de gegevens in de cache en verzendt ze naar het doelopslagaccount of op de replica beheerde schijven.
5. Nadat de gegevens zijn verwerkt, worden de crash-consistente herstelpunten om de vijf minuten gegenereerd. App-consistente herstelpunten worden gegenereerd op basis van de instelling die is opgegeven in het replicatiebeleid.

![Replicatieproces, stap 2 inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

**Replicatieproces**

## <a name="connectivity-requirements"></a>Vereisten voor connectiviteit

 De Azure VM's die u repliceren moet uitgaande connectiviteit. Site Recovery moet nooit binnenkomende verbindingen aan de virtuele machine. 

### <a name="outbound-connectivity-urls"></a>Uitgaande connectiviteit (URL's)

Als uitgaand verkeer voor virtuele machines worden beheerd met URL's, kunt u deze URL's.

| **URL** | **Details** |
| ------- | ----------- |
| *.blob.core.windows.net | Hiermee kunnen gegevens van de VM naar het cache-opslagaccount in de bronregio worden geschreven. |
| login.microsoftonline.com | Verzorgt autorisatie en authenticatie voor de URL’s van Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Maakt het de VM mogelijk te communiceren met de Site Recovery-service. |
| *.servicebus.windows.net | Maakt het de VM mogelijk bewakings- en diagnosegegevens van Site Recovery te schrijven. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande connectiviteit voor IP-adresbereiken

Voor het beheren van uitgaande connectiviteit voor virtuele machines met behulp van IP-adressen, moet u deze adressen toestaan.

#### <a name="source-region-rules"></a>Bron regio regels

**Regel** |  **Details** | **Servicetag**
--- | --- | --- 
Uitgaand verkeer toestaan van HTTPS: poort 443 | Bereiken die overeenkomen met de storage-accounts in de bronregio toestaan | Opslag. \<regio-name >.
Uitgaand verkeer toestaan van HTTPS: poort 443 | Sta de adresbereiken die overeenkomen met de Azure Active Directory (Azure AD) toe.<br/><br/> Als Azure AD-adressen in de toekomst worden toegevoegd die u wilt maken van nieuwe Netwerkbeveiligingsgroep (NSG)-regels.  | AzureActiveDirectory
Uitgaand verkeer toestaan van HTTPS: poort 443 | Toegang tot [Site Recovery-eindpunten](https://aka.ms/site-recovery-public-ips) die overeenkomen met de doellocatie. 

#### <a name="target-region-rules"></a>Doel regio regels

**Regel** |  **Details** | **Servicetag**
--- | --- | --- 
Uitgaand verkeer toestaan van HTTPS: poort 443 | Sta de adresbereiken die overeenkomen met de storage-accounts in de doelregio toe. | Opslag. \<regio-name >.
Uitgaand verkeer toestaan van HTTPS: poort 443 | Sta de adresbereiken die overeenkomen met de Azure AD toe.<br/><br/> Als Azure AD-adressen in de toekomst worden toegevoegd die u wilt maken van nieuwe NSG-regels.  | AzureActiveDirectory
Uitgaand verkeer toestaan van HTTPS: poort 443 | Toegang tot [Site Recovery-eindpunten](https://aka.ms/site-recovery-public-ips) die overeenkomen met de bronlocatie. 


#### <a name="control-access-with-nsg-rules"></a>Toegang beheren met NSG-regels

Als u VM-connectiviteit door het filteren van netwerkverkeer naar en van Azure met behulp van netwerken/subnetten beheren [NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview), houd er rekening mee de volgende vereisten:

- NSG-regels voor de Azure-regio van de bron moeten uitgaande toegang toestaan voor replicatieverkeer.
- U wordt aangeraden dat u regels maken in een testomgeving voordat u ze in productie plaatst.
- Gebruik [servicetags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) in plaats van afzonderlijke IP-adressen.
    - Service-tags vertegenwoordigt een groep met IP-adresvoorvoegsels die samen worden verzameld voor het minimaliseren van complexiteit bij het maken van beveiligingsregels.
    - Servicetags Microsoft automatisch bijgewerkt na verloop van tijd. 
 
Meer informatie over [uitgaande connectiviteit](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) voor Site Recovery, en [connectiviteit met nsg's beheren](concepts-network-security-group-with-site-recovery.md).


### <a name="connectivity-for-multi-vm-consistency"></a>Connectiviteit voor meerdere VM 's

Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004.
- Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd.
- Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.




## <a name="failover-process"></a>Failover-proces

Wanneer u een failover hebt gestart, worden de virtuele machines gemaakt in de doelresourcegroep, het virtuele doelnetwerk, het doelsubnet en in de doel-beschikbaarheid instellen. U kunt eender welk herstelpunt gebruiken tijdens een failover.

![Failover-proces](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM naar een secundaire regio.
