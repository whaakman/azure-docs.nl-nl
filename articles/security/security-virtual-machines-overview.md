---
title: Azure beveiligingsfuncties gebruikt met virtuele machines in Azure | Microsoft Docs
description: In dit artikel biedt een overzicht van de kern van het Azure-beveiligingsfuncties die kunnen worden gebruikt met Azure Virtual Machines.
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
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 7d31a434d4ead6dd8f8a13a08d368389904b5b4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-virtual-machines-security-overview"></a>Overzicht van de beveiliging Azure virtuele Machines
Virtuele Machines van Azure kunt u een breed scala aan computing oplossingen implementeren in een flexibele manier. De service ondersteunt Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk Services. U kunt dus elke werkbelasting en een andere taal op bijna elk besturingssysteem implementeren.

Een virtuele machine in Azure biedt u de flexibiliteit van virtualisatie zonder dat u de fysieke hardware hoeft te kopen en te beheren waarop de virtuele machine wordt uitgevoerd. U kunt bouwen en implementeren van uw toepassingen met de zekerheid dat de gegevens beveiligd en veilige maximaal beveiligde datacenters zijn.

Met Azure, kunt u die verbeterde beveiliging, conform oplossingen bouwen:

* Uw virtuele machines beschermen tegen virussen en kwaadaardige software.
* Codeer uw vertrouwelijke gegevens.
* Beveiligde-netwerkverkeer.
* Het identificeren en bedreigingen te detecteren.
* Voldoen aan nalevingsvereisten.

Het doel van dit artikel is een overzicht van de kern van het Azure-beveiliging om functies te leveren die kunnen worden gebruikt met virtuele machines. Koppelingen naar artikelen geven details van elk onderdeel, zodat u meer informatie.  

## <a name="antimalware"></a>Antimalware
Met Azure, kunt u van beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro en Kaspersky antimalware-software. Deze software kunt u uw virtuele machines beveiligen tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een functie real-timebeveiliging die helpt bepalen en virussen, spyware en andere schadelijke software verwijderen.  Microsoft Antimalware voor Azure biedt configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

Microsoft Antimalware voor Azure is een single-agent-oplossing voor toepassingen en tenant-omgevingen. Het is ontworpen om uit te voeren op de achtergrond, zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van uw toepassing werkbelastingen, met een eenvoudige secure--standaard of aangepaste configuratie, met inbegrip van antimalware monitoring Geavanceerd implementeren.

Wanneer u implementeert en Microsoft Antimalware voor Azure inschakelen, moet de volgende core-functies zijn beschikbaar:

* **Real-timebeveiliging**: controleert de activiteit in de Cloud Services en op virtuele Machines om te detecteren en blokkeren van schadelijke software worden uitgevoerd.
* **Geplande scan**: periodiek voert gerichte scannen om te detecteren van schadelijke software, inclusief actieve programma's.
* **Oplossen van malware**: neemt automatisch actie op de gedetecteerde malware, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en schadelijke registervermeldingen opruimen.
* **Handtekening updates**: installeert automatisch de meest recente definities (virusdefinities) om ervoor te zorgen dat beveiliging op de hoogte van een vooraf bepaald frequentie.
* **Antimalware-engine-updates**: automatisch bijgewerkt met de Microsoft Antimalware voor Azure-engine.
* **Updates voor anti-malware-platform**: automatisch bijgewerkt met de Microsoft Antimalware voor Azure-platform.
* **Actieve beveiliging**: metagegevens naar Azure telemetrie over gedetecteerde bedreigingen en verdachte bronnen om ervoor te zorgen snelle respons rapporten. Hiermee schakelt u realtime synchrone handtekening levering via de Microsoft Active Protection System (MAPS).
* **Voorbeelden reporting**: biedt en voorbeelden wordt doorgegeven aan de Microsoft Antimalware voor Azure-service voor het verfijnen van de service en het oplossen van problemen inschakelen.
* **Uitsluitingen**: kunnen toepassing en servicebeheerders voor het configureren van bepaalde bestanden, processen, en ze uitsluiten van beveiliging en scannen op de prestaties en de andere stations.
* **Antimalware-gebeurtenisverzameling**: registreert antimalware-servicestatus, verdachte activiteiten en herstelacties genomen in het gebeurtenislogboek van het besturingssysteem en verzamelt u ze in uw Azure storage-account.

Meer informatie over de antimalware-software voor het beveiligen van uw virtuele machines:

* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Het installeren en Trend Micro grondige Security configureren als een service op een virtuele machine van Windows](../virtual-machines/windows/classic/install-trend.md)
* [Het installeren en configureren van Symantec Endpoint Protection op een virtuele machine van Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Oplossingen voor webbeveiliging in Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware security module
Verbetering van de sleutelbeveiliging kan versleuteling en authenticatie beveiligingen verbeteren. Door deze te slaan in Azure Sleutelkluis kunt u het beheer en beveiliging van uw kritieke geheimen en sleutels vereenvoudigen. 

Azure Key Vault biedt u de mogelijkheid om uw sleutels op te slaan in Hardware Security Modules (HSM's) die zijn gecertificeerd voor de standaarden van FIPS 140-2 Level 2. De versleutelingssleutels SQL Server voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen alle worden opgeslagen in de Sleutelkluis met alle sleutels of geheimen van uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Meer informatie:

* [Wat is Azure Sleutelkluis?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Sleutelkluis](../key-vault/key-vault-get-started.md)
* [Azure Sleutelkluis-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuele machine schijfversleuteling
Azure Disk Encryption is een nieuwe functie voor het versleutelen van uw Windows- en Linux-schijven voor virtuele machine. Azure Disk Encryption maakt gebruik van de industriestandaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) functie van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven.

De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijf versleutelingssleutels en geheimen in uw abonnement sleutelkluis. Hiermee zorgt u ervoor dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in Azure Storage.

Meer informatie:

* [Azure Disk Encryption for Windows and Linux IaaS VM 's](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption voor Linux en Windows virtuele Machines](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Versleutelen van een virtuele machine](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Back-up van virtuele machine
Azure Backup is een schaalbare oplossing waarmee Bescherm uw toepassingsgegevens met nul investeringen en met minimale operationele kosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen. Uw virtuele machines met Windows en Linux zijn beveiligd met Azure Backup.

Meer informatie:

* [Wat is Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Leertraject voor Azure Backup](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Veelgestelde vragen over van Azure Backup-service](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Een belangrijk onderdeel van de BCDR-strategie van uw organisatie is na te gaan hoe u zakelijke workloads en apps die worden uitgevoerd wanneer de geplande en ongeplande storingen optreden. Azure Site Recovery kunnen indelen van replicatie, failovers en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

Site Recovery:

* **Uw BCDR-strategie vereenvoudigt**: Site Recovery kunt u gemakkelijk verwerken van replicatie, failovers en herstel van meerdere zakelijke workloads en apps vanaf één locatie. Site Recovery regelt de replicatie en failover, maar niet onderschept uw toepassingsgegevens of hebt u geen informatie over.
* **Biedt flexibele replicatie**: met behulp van Site Recovery kunt u workloads die worden uitgevoerd op Hyper-V virtuele machines, virtuele VMware-machines en fysieke Windows of Linux-servers repliceren.
* **Biedt ondersteuning voor failover en herstel**: Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen zonder productieomgevingen. Bovendien kunt u bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). Na een failover, kunt u schakelt terug naar uw primaire sites. De herstelplannen van Site Recovery kunnen scripts en Azure Automation-werkmappen bevatten, zodat u failovers en het herstel van toepassingen met meerdere lagen naar behoefte kunt aanpassen.
* **Secundaire datacenters elimineert**: U kunt repliceren naar een secundaire on-premises site of naar Azure. Met behulp van Azure als bestemming voor herstel na noodgevallen elimineert de kosten en complexiteit van het onderhouden van een secundaire site. Gerepliceerde gegevens worden opgeslagen in Azure Storage.
* **Kan worden geïntegreerd met bestaande BCDR-technologieën**: Site Recovery werkt samen met BCDR-functies van andere toepassingen. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Dit omvat systeemeigen ondersteuning voor SQL Server Always On voor het beheren van de failovers van beschikbaarheidsgroepen.

Meer informatie:

* [Wat is Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Hoe werkt Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Welke workloads worden beveiligd door Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtueel netwerk
Verbinding met het netwerk, moeten virtuele machines. Azure vereist ter ondersteuning van dit vereiste, virtuele machines worden verbonden met een Azure-netwerk. 

Een Azure-netwerk is een logische constructie die is gebouwd op de fysieke netwerk van Azure-infrastructuur. Elke logische virtuele Azure-netwerk is geïsoleerd van andere Azure virtuele netwerken. Deze isolatie kunt zorgen dat het netwerkverkeer in uw implementaties is niet toegankelijk voor andere Microsoft Azure-klanten.

Meer informatie:

* [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)
* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Netwerkfuncties en samenwerking voor zakelijke scenario 's](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapportage
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen. Security Center biedt u grotere zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Helpt het detecteren van bedreigingen die anders onopgemerkt en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center helpt u te optimaliseren en de beveiliging van uw virtuele machines door te controleren:

* Bieden [beveiligingsaanbevelingen](../security-center/security-center-recommendations.md) voor de virtuele machines. Voorbeeld van de aanbevelingen omvatten: systeemupdates toe te passen, eindpunten ACL's configureren, antimalware inschakelen, netwerkbeveiligingsgroepen inschakelen en schijfversleuteling van toepassing.
* Bewaken van de status van uw virtuele machines.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)
* [Veelgestelde vragen over Azure Security Center](../security-center/security-center-faq.md)
* [Azure Security Center plannen en bewerkingen](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Naleving
Virtuele Machines in Azure is gecertificeerd voor FISMA, FedRAMP HIPAA, PCI DSS-niveau 1 en andere belangrijke naleving programma's. Deze certificeringsinstantie gemakkelijker voor uw eigen Azure-toepassingen om te voldoen aan nalevingsvereisten en voor uw bedrijf voor het oplossen van een breed scala aan nationale en internationale voorschriften.

Meer informatie:

* [Microsoft Trust Center: naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Vertrouwde Cloud: Microsoft Azure-beveiliging, Privacy en naleving](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
