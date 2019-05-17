---
title: Veelgestelde vragen over Azure NetApp-bestanden | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure NetApp bestanden.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: 6f1ca3398678b59a81e5c22b51b36a1f5505d4c2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806384"
---
# <a name="faqs-about-azure-netapp-files"></a>Veelgestelde vragen over Azure NetApp bestanden

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure NetApp Files. 

## <a name="networking-faqs"></a>Veelgestelde vragen over netwerken

### <a name="does-the-nfs-data-path-go-over-the-internet"></a>Gaat het gegevenspad NFS via Internet?  

Nee. Het pad van de gegevens NFS loopt niet via Internet. Azure NetApp bestanden is een Azure-eigen service die wordt geïmplementeerd in de Azure Virtual Network (VNet) waar de service beschikbaar is. Azure NetApp bestanden maakt gebruik van een gedelegeerde subnet en een netwerkinterface rechtstreeks op het VNet wordt ingericht. 

Zie [richtlijnen voor Azure NetApp Files netwerk planning](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) voor meer informatie.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>Kan ik een VNet dat ik heb al gemaakt verbinden met de Azure NetApp Files-service?

Ja, u kunt VNets verbinden die u hebt gemaakt naar de service. 

Zie [richtlijnen voor Azure NetApp Files netwerk planning](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) voor meer informatie.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>Kan ik een NFS-volume van Azure met behulp van DNS FQDN-naam NetApp-bestanden koppelen?

Ja, u kunt, als u de vereiste DNS-vermeldingen te maken. Azure Files met NetApp levert het service-IP-adres voor het ingerichte volume. 

> [!NOTE] 
> Azure NetApp bestanden kunnen extra IP-adressen voor de service implementeren, indien nodig.  DNS-vermeldingen moet mogelijk worden regelmatig bijgewerkt.

## <a name="security-faqs"></a>Veelgestelde vragen over Security

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>Kan het netwerkverkeer tussen de Azure-VM en de opslag worden versleuteld?

Gegevensverkeer (verkeer van de NFSv3 of SMBv3-client naar Azure NetApp Files volumes) is niet versleuteld. Het verkeer van een virtuele machine van Azure (een NFS- of SMB-client) naar Azure NetApp bestanden is echter zo veilig als andere Azure-VM-VM-verkeer. Dit verkeer is lokaal op het netwerk van Azure Datacenter. 

### <a name="can-the-storage-be-encrypted-at-rest"></a>Kan de opslag in rust worden versleuteld?

Alle Azure NetApp Files volumes zijn versleuteld met behulp van de FIPS 140-2-standaard. Alle sleutels worden beheerd door de service Azure NetApp bestanden. 

### <a name="how-are-encryption-keys-managed"></a>Hoe worden de versleutelingssleutels beheerd? 

Sleutelbeheer voor Azure NetApp bestanden is verwerkt door de service.  Gebruikers beheerde sleutels (uw eigen sleutels Bring) worden momenteel niet ondersteund.

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>Kan ik de regels van het NFS exporteren voor het beheren van toegang tot het koppeldoel Azure NetApp Files-service configureren?


Ja, kunt u maximaal vijf regels configureren in een enkele beleidsregel van de NFS-uitvoer.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>Biedt ondersteuning voor Azure NetApp Files Netwerkbeveiligingsgroepen?

Nee, momenteel u kan niet van toepassing Netwerkbeveiligingsgroepen op de gedelegeerde subnet van Azure NetApp bestanden of de netwerkinterfaces die zijn gemaakt door de service.

### <a name="can-i-use-azure-iam-with-azure-netapp-files"></a>Kan ik Azure IAM gebruiken met Azure NetApp Files?

Ja, ondersteunt Azure NetApp Files RBAC-functies met Azure IAM.

## <a name="performance-faqs"></a>Veelgestelde vragen over de prestaties

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>Wat moet ik doen als u wilt optimaliseren of Azure NetApp Files prestaties afstemmen

U kunt de volgende acties per de prestatie-eisen uitvoeren: 
- Zorg ervoor dat de virtuele Machine op de juiste wijze is aangepast.
- Versneld netwerken voor de virtuele machine inschakelen.
- Selecteer het niveau van de gewenste service en de grootte voor de capaciteit van toepassingen.
- Maak een volume met de quotumgrootte van de gewenste voor de capaciteit en prestaties.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>Hoe kan ik serviceniveaus op basis van de doorvoer van Azure NetApp-bestanden converteren naar IOPS?

U kunt omzetten in MB/s IOP's met behulp van de volgende formule:  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>Hoe kan ik het serviceniveau van een volume wijzigen?

Wijzigen van de service van een volume is momenteel niet ondersteund.

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>Hoe controleer ik Azure NetApp Files prestaties?

Azure Files met NetApp biedt maatstaven voor prestaties van volume. U kunt ook Azure Monitor gebruiken voor de bewaking van metrische gegevens over gebruik van Azure NetApp bestanden.  Zie [metrische gegevens voor Azure NetApp Files](azure-netapp-files-metrics.md) voor een overzicht van metrische gegevens voor prestaties voor Azure NetApp-bestanden.

## <a name="nfs-faqs"></a>Veelgestelde vragen over NFS

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Ik wil een volume automatisch gekoppeld als een virtuele Azure-machine wordt gestart of opnieuw opgestart.  Hoe configureer ik mijn host voor permanente NFS-volumes?

Voor een NFS-volume automatisch koppelen op de virtuele machine starten of opnieuw opstarten, Voeg een vermelding aan de `/etc/fstab` bestand op de host. 

Bijvoorbeeld: `$ANFIP:/$FILEPATH      /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

- $ANFIP  
    Het IP-adres van het Azure NetApp Files-volume aangetroffen in de eigenschappenblade volume
- $FILEPATH  
    Het pad voor exporteren van het Azure NetApp Files-volume
- $MOUNTPOINT  
    De map die is gemaakt op de Linux-host gebruikt voor het koppelen van de NFS-export

### <a name="why-does-the-df-command-on-nfs-client-not-show-the-provisioned-volume-size"></a>Waarom wordt de opdracht DF op NFS-client de ingerichte volumegrootte niet weergegeven?

De grootte van het volume in DF gerapporteerd is de maximale grootte van die het volume Azure NetApp bestanden kan groeien. De grootte van het volume Azure NetApp Files in DF-opdracht is niet bij reflectieve van het quotum of de grootte van het volume.  U kunt de volumegrootte Azure NetApp bestanden of quotum ophalen via de Azure portal of de API.

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>Welke NFS versie biedt ondersteuning voor Azure NetApp Files?

Azure NetApp Files ondersteunt momenteel NFSv3.

### <a name="how-do-i-enable-root-squashing"></a>Hoe schakel ik hoofdmap vervorming.?

Hoofdmap vervorming wordt momenteel niet ondersteund.

## <a name="smb-faqs"></a>Veelgestelde vragen over SMB

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>NetApp-bestanden van Azure biedt ondersteuning voor Azure Active Directory?

Nee, deze wordt momenteel niet ondersteund.  Azure Files met NetApp ondersteuning voor Active Directory Domain Services (Breng uw eigen AD), dat bestaande Active Directory-domeincontrollers met Azure NetApp-bestanden gebruiken kunt. Domeincontrollers kunnen zich bevinden in Azure als virtuele machines of on-premises via ExpressRoute.

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>Is een Active Directory-verbinding vereist is voor SMB-toegang? 

Ja, moet u een verbinding met Active Directory voordat u implementeert een SMB-volume maken. De opgegeven domeincontrollers moet toegankelijk zijn via de gedelegeerde subnet van Azure NetApp bestanden voor de verbinding is geslaagd.  Zie [maken van een SMB-volume](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes#create-an-smb-volume) voor meer informatie. 

### <a name="how-many-active-directory-connections-are-supported"></a>Het aantal Active Directory-verbindingen worden ondersteund?

Azure NetApp Files ondersteunt momenteel één Active Directory-verbinding per abonnement. Ook is de verbinding met Active Directory specifiek voor één account NetApp; het wordt niet gedeeld in accounts. 

### <a name="what-versions-of-windows-ad-are-supported"></a>Welke versies van Windows AD worden ondersteund?

Versie van Windows Server 2008r2SP1 2016 van Active Directory Domain Services biedt ondersteuning voor Azure NetApp-bestanden.

## <a name="capacity-management-faqs"></a>Veelgestelde vragen over de capaciteitsbeheer

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>Hoe controleer ik gebruik voor de capaciteit van toepassingen en het volume van Azure NetApp bestanden? 

Azure Files met NetApp biedt capaciteit van toepassingen en het volume metrische gegevens over gebruik. U kunt ook Azure Monitor gebruiken om het gebruik van Azure NetApp bestanden bewaken. Zie [metrische gegevens voor Azure NetApp Files](azure-netapp-files-metrics.md) voor meer informatie. 

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>Kan ik Azure NetApp bestanden via Azure Storage Explorer beheren?

Nee. Azure NetApp-bestanden wordt niet ondersteund door Azure Storage Explorer.

## <a name="data-migration-and-protection-faqs"></a>Veelgestelde vragen migratie en de bescherming van gegevens

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>Hoe Migreer ik gegevens naar Azure NetApp Files?
Azure Files met NetApp biedt NFS en de SMB-volumes.  U kunt een hulpprogramma voor kopiëren op basis van bestanden gebruiken om gegevens te migreren naar de service. 

NetApp biedt een oplossing op basis van SaaS [NetApp Cloudsynchronisatie](https://cloud.netapp.com/cloud-sync-service).  De oplossing kunt u NFS repliceren of SMB-gegevens met de uitvoer van Azure NetApp bestanden NFS- of SMB-shares. 

U kunt ook een groot aantal gratis hulpprogramma's gebruiken om gegevens te kopiëren. Voor NFS, kunt u workloads hulpprogramma's zoals [rsync](https://rsync.samba.org/examples.html) om te kopiëren en synchroniseren van gegevens in een Azure NetApp Files-volume. Voor SMB, kunt u workloads [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) op dezelfde manier.  Deze hulpprogramma's kunnen ook machtigingen voor bestanden of map repliceren. 

De vereisten voor migratie van gegevens van on-premises naar Azure NetApp bestanden zijn als volgt: 

- Zorg ervoor dat Azure NetApp bestanden is beschikbaar in de doel-Azure-regio.
- Valideer de netwerkverbinding tussen de bron en het IP-adres van Azure NetApp Files doel volume. Gegevensoverdracht tussen locatie en de Azure NetApp Files-service wordt ondersteund via ExpressRoute.
- Maak de doel-Azure NetApp Files-volume.
- De brongegevens naar het doelvolume overbrengen met behulp van uw programma voor het gewenste bestand kopiëren.

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>Hoe maak ik een kopie van een Azure NetApp Files-volume in een andere Azure-regio?
    
Azure Files met NetApp biedt NFS en de SMB-volumes.  Elk bestand op basis van kopie hulpprogramma kan worden gebruikt om gegevens te repliceren tussen Azure-regio's. 

NetApp biedt een oplossing op basis van SaaS [NetApp Cloudsynchronisatie](https://cloud.netapp.com/cloud-sync-service).  De oplossing kunt u NFS repliceren of SMB-gegevens met de uitvoer van Azure NetApp bestanden NFS- of SMB-shares. 

U kunt ook een groot aantal gratis hulpprogramma's gebruiken om gegevens te kopiëren. Voor NFS, kunt u workloads hulpprogramma's zoals [rsync](https://rsync.samba.org/examples.html) om te kopiëren en synchroniseren van gegevens in een Azure NetApp Files-volume. Voor SMB, kunt u workloads [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) op dezelfde manier.  Deze hulpprogramma's kunnen ook machtigingen voor bestanden of map repliceren. 

De vereisten voor het repliceren van een Azure NetApp Files-volume naar een andere Azure-regio zijn als volgt: 
- Zorg ervoor dat Azure NetApp bestanden is beschikbaar in de doel-Azure-regio.
- Valideer de netwerkverbinding tussen de VNets in elke regio. Wereldwijde peering tussen VNets wordt momenteel niet ondersteund.  U kunt de verbinding tussen VNets te koppelen met een ExpressRoute-circuit of met behulp van een S2S-VPN-verbinding maken. 
- Maak de doel-Azure NetApp Files-volume.
- De brongegevens naar het doelvolume overbrengen met behulp van uw programma voor het gewenste bestand kopiëren.

### <a name="is-migration-with-azure-data-box-supported"></a>Migratie met Azure Data Box wordt ondersteund is?

Nee. Azure Data Box wordt Azure NetApp bestanden niet op dit moment ondersteund. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>Is de migratie met Azure Import/Export-service ondersteund?

Nee. Azure Import/Export-service ondersteunt geen Azure NetApp bestanden op dat moment.

## <a name="next-steps"></a>Volgende stappen  

- [Veelgestelde vragen over de Microsoft Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)
- [Veelgestelde vragen over Microsoft Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq)
- [Een ondersteuningsaanvraag maken voor Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)
- [Azure Data Box](https://docs.microsoft.com/azure/databox-family/)