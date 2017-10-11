---
title: Azure beveiligingsfuncties gebruikt met virtuele machines in Azure | Microsoft Docs
description: " Een overzicht van de functies voor het Azure-beveiliging van core die kan worden gebruikt met virtuele machines in Azure. Virtuele machines in Azure bieden u de flexibiliteit van virtualisatie zonder te kopen en beheren van de fysieke hardware die de virtuele machine wordt uitgevoerd. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: terrylan
ms.openlocfilehash: f1fb7f876c7dc010c03f01a4f6698ddc18da1100
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Overzicht van de beveiliging Azure virtuele Machines
Met Azure Virtual Machines kunt u een brede reeks computeroplossingen op flexibele wijze inzetten. Met ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk Services kunt u elke workload en elke taal implementeren op vrijwel elk besturingssysteem.

Een virtuele machine in Azure biedt u de flexibiliteit van virtualisatie zonder dat u de fysieke hardware hoeft te kopen en te beheren waarop de virtuele machine wordt uitgevoerd.  U kunt bouwen en implementeren van uw toepassingen met de zekerheid dat de gegevens beveiligd en veilige de maximaal beveiligde datacenters zijn.

Met Azure, kunt u die verbeterde beveiliging, conform oplossingen bouwen:

* Uw virtuele machines beschermen tegen virussen en malware
* Gevoelige gegevens versleutelen
* Netwerkverkeer beveiligen
* Bedreigingen identificeren en detecteren
* Voldoen aan nalevingsvereisten

Het doel van dit artikel is een overzicht van de kern van het Azure-beveiliging om functies te leveren die kunnen worden gebruikt met virtuele machines. We bieden ook koppelingen naar artikelen waarmee details van elk onderdeel, zodat u meer informatie.  

De basisbeveiligingsmogelijkheden voor Azure virtuele Machine die moet worden opgenomen in dit artikel:

* Antimalware
* Hardware Security Module
* Virtuele machine schijfversleuteling
* Back-up van virtuele machine
* Azure Site Recovery
* Virtuele netwerken
* Beheer van beveiligingsbeleid en rapportage
* Naleving

## <a name="antimalware"></a>Antimalware
Met Azure, kunt u antimalware-software van beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro en Kaspersky uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere dreigingen. Zie de sectie meer informatie hieronder om te zoeken naar artikelen over partner oplossingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een functie real-timebeveiliging die helpt bepalen en virussen, spyware en andere schadelijke software verwijderen.  Microsoft Antimalware biedt configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

Microsoft Antimalware is een single-agent-oplossing voor toepassingen en tenant-omgevingen, die is ontworpen om uit te voeren op de achtergrond, zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van uw toepassing werkbelastingen, met een eenvoudige secure--standaard of aangepaste configuratie, met inbegrip van antimalware monitoring Geavanceerd implementeren.

Wanneer u implementeert en Microsoft Antimalware inschakelt, zijn de volgende core functies beschikbaar:

* Real-timebeveiliging - monitors activiteit in de Cloud Services en op virtuele Machines om te detecteren en blokkeren van schadelijke software worden uitgevoerd.
* Geplande scan - voert periodiek gerichte scannen om te detecteren van schadelijke software, inclusief actieve programma's.
* Oplossen van malware - neemt actie automatisch op de gedetecteerde malware, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en schadelijke registervermeldingen opruimen.
* Updates van de handtekening - automatisch worden geïnstalleerd, de meest recente definities (virusdefinities) ter bescherming is op de hoogte van een vooraf bepaald frequentie.
* Antimalware-Engine-updates – automatisch de Microsoft Antimalware-engine-updates.
* Antimalware-Platform updates – automatisch updates van het Microsoft Antimalware-platform.
* Actieve beveiliging - rapporten naar Azure telemetrie metagegevens over gedetecteerde bedreigingen en verdachte bronnen om ervoor te zorgen snelle respons en realtime synchrone handtekening levering via de Microsoft Active Protection System (MAPS) maakt.
* Voorbeelden voor rapportage - biedt en voorbeelden rapporten naar Microsoft Antimalware-service voor het verfijnen van de service en het oplossen van problemen inschakelen.
* Uitsluitingen – kunnen toepassing en servicebeheerders voor het configureren van bepaalde bestanden, processen, en ze uitsluiten van beveiliging en scannen op de prestaties en de andere stations.
* Gebeurtenissen verzamelen van Antimalware - registreert de status van antimalware-service, verdachte activiteiten en herstelacties genomen in het gebeurtenislogboek van het besturingssysteem en verzamelt ze in Azure Storage-account van de klant.

Meer informatie: Zie voor meer informatie over de antimalware-software voor het beveiligen van uw virtuele machines:

* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Het installeren en Trend Micro grondige Security configureren als een Service op een virtuele machine van Windows](../virtual-machines/windows/classic/install-trend.md)
* [Het installeren en configureren van Symantec Endpoint Protection op een virtuele machine van Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Oplossingen voor webbeveiliging in Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware security Module
Versleuteling en authenticatie beveiligingen kunnen worden uitgebreid door sleutelbeveiliging te verbeteren. Door deze te slaan in Azure Sleutelkluis kunt u het beheer en beveiliging van uw kritieke geheimen en sleutels vereenvoudigen. Azure Key Vault biedt u de mogelijkheid om uw sleutels op te slaan in Hardware Security Modules (HSM's) die zijn gecertificeerd voor de standaarden van FIPS 140-2 Level 2. De versleutelingssleutels SQL Server voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen alle worden opgeslagen in de Sleutelkluis met alle sleutels of geheimen van uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Meer informatie:

* [Wat is Azure Sleutelkluis?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Sleutelkluis](../key-vault/key-vault-get-started.md)
* [Azure Sleutelkluis-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuele machine schijfversleuteling
Azure Disk Encryption is een nieuwe functie waarmee u de schijven voor Windows en Linux Azure Virtual machines versleutelen. Azure Disk Encryption maakt gebruik van het industrie-initiatief [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) functie van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven.

De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijf-sleutels en geheimen in uw abonnement sleutelkluis, terwijl u ervoor zorgt dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag.

Meer informatie:

* [Azure Disk Encryption for Windows and Linux IaaS VM 's](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption voor Linux en Windows virtuele Machines](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Versleutelen van een virtuele machine](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Back-up van virtuele machine
Azure Backup is een schaalbare oplossing die uw toepassingsgegevens beveiligt zonder enige kapitaalinvestering en tegen minimale bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen. Uw virtuele machines met Windows en Linux zijn beveiligd met Azure Backup.

Meer informatie:

* [Wat is Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Leertraject voor Azure Backup](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup-Service - Veelgestelde vragen](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Ervoor zorgen dat zakelijke workloads en apps actief blijven tijdens geplande en niet-geplande uitval is een belangrijk onderdeel van de BCDR-strategie van uw organisatie. Azure Site Recovery kunnen indelen van replicatie, failovers en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

Site Recovery:

* **Uw BCDR-strategie vereenvoudigt** : Site Recovery kunt u gemakkelijk verwerken van replicatie, failovers en herstel van meerdere zakelijke workloads en apps vanaf één locatie. Site Recovery regelt de replicatie en failovers, maar onderschept uw toepassingsgegevens niet en heeft er ook geen informatie over.
* **Biedt flexibele replicatie** : met behulp van Site Recovery kunt u workloads die worden uitgevoerd op Hyper-V virtuele machines, virtuele VMware-machines en fysieke Windows of Linux-servers repliceren.
* **Biedt ondersteuning voor failover en herstel** : Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen zonder productieomgevingen. Bovendien kunt u bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). Na het uitvoeren van een failover kunt u weer een failback uitvoeren naar uw primaire sites. De herstelplannen van Site Recovery kunnen scripts en Azure Automation-werkmappen bevatten, zodat u failovers en het herstel van toepassingen met meerdere lagen naar behoefte kunt aanpassen.
* **Secundair datacenter elimineert** — u kunt repliceren naar een secundaire on-premises site of naar Azure. Met behulp van Azure als bestemming voor herstel na noodgevallen elimineert de kosten en complexiteit van het onderhouden van een secundaire site. Gerepliceerde gegevens worden opgeslagen in Azure Storage.
* **Kan worden geïntegreerd met bestaande BCDR-technologieën** : Site Recovery werkt samen met andere toepassing BCDR-functies. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Dit omvat systeemeigen ondersteuning voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen.

Meer informatie:

* [Wat is Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Hoe werkt Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Wat werkbelastingen zijn beveiligd door Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuele netwerken
Verbinding met het netwerk, moeten virtuele machines. Azure vereist ter ondersteuning van dit vereiste, virtuele machines worden verbonden met een virtueel netwerk van Azure. Een Azure-netwerk is een logische constructie die is gebouwd op de fysieke netwerk van Azure-infrastructuur. Elke logische Azure Virtual Network is geïsoleerd van alle andere virtuele netwerken van Azure. Deze isolatie kunt zorgen dat het netwerkverkeer in uw implementaties is niet toegankelijk voor andere Microsoft Azure-klanten.

Meer informatie:

* [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)
* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Netwerkfuncties en samenwerking voor zakelijke scenario 's](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapportage
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen en biedt dat u grotere zichtbaarheid van, en controle over, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Azure Security Center helpt u te optimaliseren en beveiliging van de virtuele machine door te controleren:

* Virtuele machine biedt [beveiligingsaanbevelingen](../security-center/security-center-recommendations.md) zoals toepassing systeemupdates, eindpunten ACL's configureren, antimalware inschakelen netwerkbeveiligingsgroepen inschakelen en schijfversleuteling van toepassing.
* Bewaking van de status van uw virtuele machines

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)
* [Veelgestelde vragen over Azure Security Center](../security-center/security-center-faq.md)
* [Azure Security Center plannen en bewerkingen](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Naleving
Virtuele Machines in Azure is gecertificeerd voor FISMA, FedRAMP HIPAA, PCI DSS-niveau 1 en andere belangrijke naleving programma's. Deze certificeringsinstantie gemakkelijker voor uw eigen Azure-toepassingen om te voldoen aan nalevingsvereisten en voor uw bedrijf voor het oplossen van een breed scala aan nationale en internationale voorschriften.

Meer informatie:

* [Microsoft Trust Center: naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Vertrouwde Cloud: Microsoft Azure-beveiliging, Privacy en naleving](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
