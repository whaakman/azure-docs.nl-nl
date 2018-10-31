---
title: Functies van Azure-beveiliging gebruikt met Azure virtual machines | Microsoft Docs
description: Dit artikel bevat een overzicht van de belangrijkste functies van Azure-beveiliging die kunnen worden gebruikt met Azure Virtual Machines.
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
ms.date: 10/30/2018
ms.author: terrylan
ms.openlocfilehash: 631fe91bc72093cf5ad87779853df4901a27a068
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249111"
---
# <a name="azure-virtual-machines-security-overview"></a>Overzicht van beveiliging van Azure Virtual Machines

U kunt Azure Virtual Machines gebruiken voor het implementeren van een breed scala aan computeroplossingen op flexibele wijze inzetten. De service biedt ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk Services. U kunt dus elke workload en elke taal op vrijwel elk besturingssysteem implementeren.

Een virtuele machine in Azure biedt u de flexibiliteit van virtualisatie zonder dat u de fysieke hardware hoeft te kopen en te beheren waarop de virtuele machine wordt uitgevoerd. U kunt bouwen en implementeren van uw toepassingen in de wetenschap dat uw gegevens veilig en beschermd zijn in zwaar beveiligde datacenters.

Met Azure, kunt u dat verbeterde beveiliging en compatibele oplossingen bouwen:

* Bescherm uw virtuele machines tegen virussen en malware.
* Gevoelige gegevens versleutelen.
* Netwerkverkeer beveiligen.
* Identificeren en bedreigingen te detecteren.
* Voldoen aan nalevingsvereisten.

Het doel van dit artikel is voor een overzicht van de belangrijkste functies van Azure-beveiliging die kunnen worden gebruikt met virtuele machines. Koppelingen naar artikelen Geef details van elke functie zodat u kunt meer informatie.  

## <a name="antimalware"></a>Antimalware

Met Azure, kunt u antimalware-software van leveranciers van beveiligingsoplossingen, zoals Microsoft, Symantec, Trend Micro en Kaspersky. Deze software beschermt uw virtuele machines tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een realtime-beveiliging-functie waarmee u kunt herkennen en verwijderen van virussen, spyware en andere schadelijke software.  Microsoft Antimalware voor Azure biedt configureerbare meldingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

Microsoft Antimalware voor Azure is een één-agent-oplossing voor toepassingen en tenant-omgevingen. Het is ontworpen om uit te voeren op de achtergrond zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van uw werkbelastingen van toepassingen, met een standaard secure standaard of aangepaste configuratie, met inbegrip van antimalware monitoring geavanceerde implementeren.

Wanneer u implementeert en inschakelen van Microsoft Antimalware voor Azure, zijn de volgende belangrijkste functies beschikbaar:

* **Real-timebeveiliging**: controleert de activiteit in Cloud Services en op virtuele Machines te detecteren en blokkeren van schadelijke software worden uitgevoerd.
* **Geplande scan**: periodiek voert gerichte scannen voor het detecteren van malware, met inbegrip van programma's actief wordt uitgevoerd.
* **Oplossen van malware**: automatisch actie onderneemt met gedetecteerde schadelijke software, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en opschonen van schadelijke registervermeldingen.
* **Handtekeningupdates**: installeert automatisch de meest recente protection handtekeningen (virusdefinities) om ervoor te zorgen dat de beveiliging op de hoogte van een vooraf bepaald frequentie.
* **Antimalware-engine-updates**: automatisch bijgewerkt met de Microsoft Antimalware voor Azure-engine.
* **Updates voor anti-malware-platform**: automatisch bijgewerkt met de Microsoft Antimalware voor Azure-platform.
* **Actieve beveiliging**: rapporten telemetrie metagegevens naar Azure over gedetecteerde bedreigingen en verdachte resources om te controleren of een snelle respons. Hiermee kunt realtime synchrone handtekening levering via de Microsoft Active Protection System (MAPS).
* **Voorbeelden van reporting**: biedt en voorbeelden van rapporten naar de Microsoft Antimalware voor Azure-service om u te helpen bij het verfijnen van de service en het oplossen van inschakelen.
* **Uitsluitingen**: kunnen toepassingen en servicebeheerders bepaalde bestanden, processen, configureren en ze uitsluiten van beveiliging en scannen op prestaties en de andere stations.
* **Antimalware-gebeurtenisverzameling**: registreert antimalware-servicestatus, verdachte activiteiten en herstelacties die zijn uitgevoerd in het gebeurtenislogboek van het besturingssysteem en verzamelt deze gegevens in uw Azure storage-account.

Meer informatie over de antimalware-software voor beveiliging van uw virtuele machines:

* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Installeren en configureren van Trend Micro Deep Security als een service op een Windows-VM](../virtual-machines/windows/classic/install-trend.md)
* [Installeren en configureren van Symantec Endpoint Protection op een Windows-VM](../virtual-machines/windows/classic/install-symantec.md)
* [Beveiligingsoplossingen in Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

Voor nog krachtiger beveiliging, kunt u overwegen [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection). Met Windows Defender ATP profiteert u van:

* [Kwetsbaarheid voor aanvallen verminderen](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)  
* [Volgende generatie beveiliging](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)  
* [Endpoint protection en de reactie](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-endpoint-detection-response)
* [Geautomatiseerde onderzoek en herstel](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/automated-investigations-windows-defender-advanced-threat-protection)
* [beveiligen van score](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-secure-score-windows-defender-advanced-threat-protection)
* [Geavanceerd zoeken](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-hunting-windows-defender-advanced-threat-protection)
* [Beheer en API 's](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/management-apis)
* [Microsoft Threat Protection](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/threat-protection-integration)

Meer informatie: 

* [Aan de slag met WDATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/get-started)  
* [Overzicht van WDATP mogelijkheden](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview)  

## <a name="hardware-security-module"></a>Hardware security module

Verbetering van de sleutelbeveiliging kan versleuteling en authenticatie beveiligingen verbeteren. U kunt het beheer en beveiliging van uw essentiële geheimen en sleutels vereenvoudigen doordat ze in Azure Key Vault. 

Azure Key Vault biedt u de mogelijkheid om uw sleutels op te slaan in Hardware Security Modules (HSM's) die zijn gecertificeerd voor de standaarden van FIPS 140-2 Level 2. Uw SQL Server-versleutelingssleutels voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen allemaal worden opgeslagen in Key Vault met alle sleutels of geheimen voor uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Meer informatie:

* [Wat is Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Azure Key Vault-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Versleuteling van de virtuele machine-schijf

Azure Disk Encryption is een nieuwe mogelijkheid voor het versleutelen van uw virtuele-machineschijven van Windows en Linux. Azure Disk Encryption gebruikt de industriestandaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden.

De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de sleutels en geheimen in uw key vault-abonnement. Het zorgt ervoor dat alle gegevens in de virtuele-machineschijven zijn versleuteld in rust in Azure Storage.

Meer informatie:

* [Azure Disk Encryption voor IaaS-VM 's](../security/azure-security-disk-encryption-overview.md)
* [Snelstartgids: Een virtuele Windows IaaS-machine met Azure PowerShell versleutelen](../security/quick-encrypt-vm-powershell.md)

## <a name="virtual-machine-backup"></a>Back-up van virtuele machine

Azure Backup is een schaalbare oplossing die beschermt uw toepassingsgegevens met enige kapitaalinvestering en tegen minimale bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen. Met Azure Backup, worden uw virtuele machines met Windows en Linux beveiligd.

Meer informatie:

* [Wat is Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Leertraject voor Azure Backup](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Veelgestelde vragen over van Azure Backup-service](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery

Een belangrijk onderdeel van uw organisatie BCDR-strategie is essentieel dat u weet hoe u zakelijke workloads en apps uitvoeren wanneer geplande en ongeplande storingen optreden. Azure Site Recovery kunt indelen van replicatie, failover en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie als uw primaire locatie uitvalt.

Site Recovery:

* **Uw BCDR-strategie vereenvoudigt**: Site Recovery maakt het eenvoudig om replicatie, failover en herstel van meerdere zakelijke workloads en apps vanaf één locatie. Site Recovery deelt replicatie en failover, maar niet onderschept uw toepassingsgegevens of geen informatie erover.
* **Flexibele replicatie biedt**: met Site Recovery kunt u workloads die worden uitgevoerd op Hyper-V virtuele machines, virtuele VMware-machines en fysieke Windows/Linux-servers kunt repliceren.
* **Biedt ondersteuning voor failover en herstel**: Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen zonder gevolgen voor productie-omgevingen. Bovendien kunt u bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). Na een failover, kunt u een failback naar uw primaire sites. De herstelplannen van Site Recovery kunnen scripts en Azure Automation-werkmappen bevatten, zodat u failovers en het herstel van toepassingen met meerdere lagen naar behoefte kunt aanpassen.
* **Secundaire datacenters elimineert**: U kunt repliceren naar een secundaire on-premises site, of naar Azure. Met behulp van Azure als bestemming voor herstel na noodgevallen elimineert de kosten en complexiteit van het onderhouden van een secundaire site. Gerepliceerde gegevens worden opgeslagen in Azure Storage.
* **Kan worden geïntegreerd met bestaande BCDR-technologieën**: Site Recovery werkt samen met BCDR-functies voor andere toepassingen. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Dit omvat ingebouwde ondersteuning voor SQL Server Always On voor het beheren van failovers van beschikbaarheidsgroepen.

Meer informatie:

* [Wat is Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Hoe werkt Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Welke workloads worden beveiligd door Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtueel netwerk

Virtuele machines moet verbinding met het netwerk. Voor de ondersteuning die vereiste vereist Azure virtuele machines worden verbonden met een Azure-netwerk. 

Een Azure-netwerk is een logische constructie die is gebaseerd op de fysieke netwerk van Azure-infrastructuur. Elke logische Azure-netwerk is geïsoleerd van andere Azure-netwerken. Deze isolatie helpt ervoor gezorgd dat het netwerkverkeer in uw implementaties niet toegankelijk voor andere Microsoft Azure-klanten is.

Meer informatie:

* [Overzicht van de beveiliging van Azure-netwerk](security-network-overview.md)
* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Netwerkfuncties en relaties voor zakelijke scenario 's](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapportage

Azure Security Center helpt u bij het voorkomen, detecteren en direct reageren op bedreigingen. Security Center biedt u meer inzicht in, en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Het helpt bedreigingen detecteren die anders onopgemerkt en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center helpt u te optimaliseren en de beveiliging van uw virtuele machines door te controleren:

* Biedt [beveiligingsaanbevelingen](../security-center/security-center-recommendations.md) voor de virtuele machines. Voorbeeld van de aanbevelingen zijn onder andere: systeemupdates toepassen, ACL's eindpunten configureren voor het inschakelen van anti-malware, netwerkbeveiligingsgroepen inschakelen en schijfversleuteling toepassen.
* Bewaking van de status van uw virtuele machines.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)
* [Veelgestelde vragen over Azure Security Center](../security-center/security-center-faq.md)
* [Azure Security Center planning en bewerkingen](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Naleving

Virtuele Machines van Azure is gecertificeerd voor FISMA, FedRAMP, HIPAA, PCI DSS Level 1 en andere belangrijke nalevingsprogramma's. Deze certificering maakt het gemakkelijker voor uw eigen Azure-toepassingen om te voldoen aan nalevingsvereisten voldoet en uw bedrijf te voldoen aan een breed bereik van binnenlandse en internationale regelgeving.

Meer informatie:

* [Vertrouwenscentrum van Microsoft: naleving](https://www.microsoft.com/en-us/trustcenter/compliance)
* [Vertrouwde Cloud: Microsoft Azure-beveiliging, Privacy en naleving](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)

## <a name="confidential-computing"></a>Vertrouwelijke Computing

Terwijl een vertrouwelijk computing is geen technisch onderdeel van de beveiliging van virtuele machines, wordt het onderwerp van de beveiliging van virtuele machines behoort tot het onderwerp op een hoger niveau van beveiliging 'compute'. In de categorie 'compute' beveiliging behoort vertrouwelijke computing. 

Vertrouwelijke computing zorgt ervoor dat als gegevens 'in de wissen", die is vereist voor efficiënte verwerking, de gegevens wordt beveiligd in een vertrouwde omgeving https://en.wikipedia.org/wiki/Trusted_execution_environment (t - ook wel bekend als een enclave), een voorbeeld van die in de afbeelding hieronder wordt weergegeven .  

Grote zorg ervoor dat er is geen manier om gegevens of de bewerkingen binnen van buiten en zelfs met een foutopsporingsprogramma weer te geven. Zelfs ervoor dat alleen geautoriseerde code is toegestaan voor toegang tot gegevens. Als de code is gewijzigd of gewijzigd, de bewerkingen zijn geweigerd en de omgeving is uitgeschakeld. De t dwingt deze beveiligingen gedurende de uitvoering van code in het af. 

Meer informatie:

* [Maak kennis met Azure vertrouwelijke computing](https://azure.microsoft.com/blog/introducing-azure-confidential-computing/)  
* [Azure vertrouwelijke computing](https://azure.microsoft.com/blog/azure-confidential-computing/)  

