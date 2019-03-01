---
title: Aanbevolen procedures voor beveiliging voor IaaS-workloads in Azure | Microsoft Docs
description: " De migratie van werkbelastingen naar Azure IaaS brengt kansen te beoordelen van onze ontwerpen "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: barclayn
ms.openlocfilehash: de89e0a30f39ba97379b4d55914338702aef5c32
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990405"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Aanbevolen beveiligingsprocedures voor IaaS-workloads in Azure

In de meeste infrastructuur als een service (IaaS)-scenario's, [virtuele Azure-machines (VM's)](https://docs.microsoft.com/azure/virtual-machines/) zijn de belangrijkste werkbelasting voor organisaties die gebruikmaken van cloud computing. Dit is duidelijk in [hybride scenario's](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) waar organisaties willen langzaam workloads migreren naar de cloud. In dergelijke scenario's, voert u de [algemene beveiligingsoverwegingen voor IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), en aanbevolen procedures voor beveiliging van toepassing op alle virtuele machines.

Uw eigen verantwoordelijkheid voor beveiliging is gebaseerd op het type van de service in de cloud. De volgende tabel geeft een overzicht van het saldo van de verantwoordelijkheid voor zowel Microsoft als u:

![Verantwoordelijkheidsgebieden ten opzichte van](./media/azure-security-iaas/sec-cloudstack-new.png)

Vereisten voor beveiliging zijn afhankelijk van een aantal factoren, waaronder verschillende typen workloads. Niet een van deze aanbevolen procedures beveiligen op zichzelf uw systemen. Als iets anders in beveiliging, die u moet kiest u de gewenste opties en Zie hoe de oplossingen kunnen vullen elkaar door hiaten invullen.

Dit artikel wordt beschreven aanbevolen procedures voor beveiliging voor virtuele machines en besturingssystemen.

De best practices zijn gebaseerd op een consensus van advies, en ze werken met de huidige mogelijkheden van Azure-platform en functie ingesteld. Omdat de meningen en technologieën na verloop van tijd wijzigen kunnen, wordt in dit artikel wordt bijgewerkt om deze wijzigingen weer te geven.

## <a name="protect-vms-by-using-authentication-and-access-control"></a>Virtuele machines beveiligen met behulp van verificatie en toegangsbeheer
De eerste stap bij het beschermen van uw virtuele machines is om ervoor te zorgen dat alleen gemachtigde gebruikers kunnen instellen van nieuwe virtuele machines en toegang tot virtuele machines.

**Beste**: De VM-toegang beheren.   
**Details**: Gebruik [Azure beleid](../azure-policy/azure-policy-introduction.md) conventies voor resources in uw organisatie vast te stellen en aangepaste beleidsregels maken. Dit beleid van toepassing op resources, zoals [resourcegroepen](../azure-resource-manager/resource-group-overview.md). Virtuele machines die deel uitmaken van een resourcegroep worden overgenomen voorgeschreven beleid.

Als uw organisatie veel abonnementen heeft, moet u mogelijk een manier om efficiënt beheer van toegang, beleid en naleving voor deze abonnementen. [Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md) een bepaald niveau van bereik hierboven abonnementen. U ordenen van abonnementen in beheergroepen (containers) en de voorwaarden van uw beheeracties toepassen op deze groepen. Alle abonnementen in een beheergroep worden automatisch de voorwaarden die aan de groep overgenomen. Beheergroepen bieden u beheer van bedrijfskwaliteit op grote schaal, ongeacht de typen abonnementen die u hebt.

**Beste**: Minder variabiliteit bij de installatie en implementatie van virtuele machines.   
**Details**: Gebruik [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sjablonen versterking van uw implementatieopties en maken het gemakkelijker om te begrijpen en inventariseren van de virtuele machines in uw omgeving.

**Beste**: Beveiligde uitgebreide toegang.   
**Details**: Gebruik een [benadering van minimale bevoegdheden](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) en ingebouwde Azure-rollen om gebruikers toegang tot en het instellen van virtuele machines:

- [Inzender voor virtuele machines](../role-based-access-control/built-in-roles.md#virtual-machine-contributor): Kan beheren VM's, maar niet op het virtuele netwerk of opslag account waaraan ze zijn verbonden.
- [Inzender voor klassieke virtuele machines](../role-based-access-control/built-in-roles.md#classic-virtual-machine-contributor): Kan virtuele machines beheren die zijn gemaakt met behulp van het klassieke implementatiemodel, maar niet op het virtuele netwerk of opslag account waarmee de virtuele machines zijn verbonden.
- [Beveiligingsbeheerder](../role-based-access-control/built-in-roles.md#security-admin): In Security Center: Kan weergeven beveiligingsbeleid, security-status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen.
- [DevTest Labs User](../role-based-access-control/built-in-roles.md#devtest-labs-user): Kan alles weergeven en verbinding maken, starten, opnieuw opstarten en VM's af.

Uw abonnementsbeheerders en coadmins kunt deze instelling, waardoor ze beheerders van alle virtuele machines in een abonnement wijzigen. Zorg ervoor dat u al uw abonnementsbeheerders en coadmins aanmelden bij een van uw machines vertrouwen.

> [!NOTE]
> Het is raadzaam dat u virtuele machines met dezelfde levenscyclus in dezelfde resourcegroep bevinden samenvoegen. Met behulp van resourcegroepen, kunt u implementeren, bewaken en factureringskosten voor uw resources.
>
>

Organisaties die VM-toegang en instellingen beheren verbeteren de algehele beveiliging van de virtuele machine.

## <a name="use-multiple-vms-for-better-availability"></a>Meerdere virtuele machines gebruiken voor betere beschikbaarheid
Als uw virtuele machine wordt uitgevoerd voor kritieke toepassingen die u wilt een hoge beschikbaarheid hebben, wordt aangeraden dat u meerdere virtuele machines. Gebruik voor betere beschikbaarheid, een [beschikbaarheidsset](../virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).

Een beschikbaarheidsset is een logische groep die u in Azure gebruiken kunt om ervoor te zorgen dat de VM-resources die u erin geïsoleerd van elkaar zijn wanneer deze zijn geïmplementeerd in een Azure-datacenter. Azure zorgt ervoor dat de virtuele machines die u in een beschikbaarheidsset plaatst uitvoeren voor meerdere fysieke servers, compute rekken, opslageenheden en netwerkswitches instellen. Als er een hardware- of softwarestoring in Azure optreedt, alleen een subset van uw virtuele machines zijn getroffen en blijft uw totale toepassing beschikbaar zijn voor uw klanten. Beschikbaarheidssets zijn essentieel wanneer u betrouwbare cloudoplossingen bouwen.

## <a name="protect-against-malware"></a>Bescherming tegen malware
Bescherming tegen malware om te helpen identificeren en virussen, spyware en andere schadelijke software verwijderen, moet u installeren. U kunt installeren [Microsoft Antimalware](azure-security-antimalware.md) of oplossing voor eindpuntbeveiliging is een Microsoft-partner ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/search/result.aspx?q=Windows+defender+endpoint+protection), en [System Center Endpoint Protection](https://www.microsoft.com/search/result.aspx?q=System+Center+endpoint+protection)).

Microsoft Antimalware bevat functies zoals realtime-beveiliging, geplande scannen, oplossen van malware, handtekeningupdates, engine-updates, rapportage-voorbeelden en uitsluitingsverzameling gebeurtenis. Voor omgevingen die afzonderlijk worden gehost in uw productieomgeving, kunt u een anti-malware-extensie voor het beveiligen van uw virtuele machines en cloudservices.

U kunt oplossingen van Microsoft Antimalware en partners met integreren [Azure Security Center](https://docs.microsoft.com/azure/security-center/) voor een eenvoudige implementatie en ingebouwde detecties (waarschuwingen en incidenten).

**Beste**: Installeer een antimalwareoplossing om u te beschermen tegen malware.   
**Details**: [Een oplossing van Microsoft-partner of Microsoft Antimalware installeren](../security-center/security-center-install-endpoint-protection.md)

**Beste**: De antimalwareoplossing integreren met Security Center voor het bewaken van de status van uw beveiliging.   
**Details**: [Problemen met endpoint protection met Security Center beheren](../security-center/security-center-partner-integration.md)

## <a name="manage-your-vm-updates"></a>Uw virtuele machine-updates beheren
Azure VM's, zoals alle on-premises VM's zijn bedoeld om te worden door gebruiker beheerd. Azure biedt geen Windows-updates pushen naar deze. U moet uw virtuele machine-updates wilt beheren.

**Beste**: Uw virtuele machines actueel te houden.   
**Details**: Gebruik de [updatebeheer](../automation/automation-update-management.md) oplossing in Azure Automation voor het beheren van updates voor uw Windows- en Linux-computers die zijn geïmplementeerd in Azure, voor het besturingssysteem in on-premises omgevingen, of in andere cloud-providers. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en de procedure voor het installeren van vereiste updates voor servers beheren.

Computers die worden beheerd door de Update Management gebruiken de volgende configuraties voor het uitvoeren van de evaluatie en update-implementaties:

- Microsoft Monitoring Agent (MMA) voor Windows of Linux
- PowerShell Desired State Configuration (DSC) voor Linux
- Automation Hybrid Runbook Worker
- Microsoft Update of Windows Server Update Services (WSUS) voor Windows-computers

Als u Windows Update gebruikt, laat u de instelling voor automatisch Windows Update ingeschakeld.

**Beste**: Zorg ervoor dat tijdens de implementatie dat installatiekopieën die u hebt gemaakt, de meest recente ronde van Windows-updates bevatten.   
**Details**: Controleer en alle Windows-updates te installeren als een eerste stap van elke implementatie. Deze meting is vooral belangrijk om toe te passen wanneer u de installatiekopieën die afkomstig van de door u of uw eigen bibliotheek zijn implementeert. Hoewel installatiekopieën uit de Azure Marketplace automatisch door standaard bijgewerkt worden, kan dit worden veroorzaakt door een vertraging (maximaal een paar weken) na een openbare versie.

**Beste**: Periodiek opnieuw uw VM's om af te dwingen een nieuwe versie van het besturingssysteem implementeren.   
**Details**: Definieer uw virtuele machine met een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) , zodat u eenvoudig kunt implementeren. Met behulp van een sjabloon biedt u een virtuele machine bijwerken en beveiligen wanneer u ze nodig hebt.

**Beste**: Installeer de meest recente beveiligingsupdates.   
**Details**: Sommige van de eerste werkbelastingen die klanten naar Azure verplaatsen zijn labs en externe systemen. Als uw Azure VM's hosten toepassingen of services die toegankelijk zijn met het internet moeten zijn, worden waakzaam over patches. Patch dan het besturingssysteem. Niet-gepatchte beveiligingsproblemen op partnertoepassingen kunnen ook leiden tot problemen die kunnen worden voorkomen als goed patchbeheer aanwezig is.

**Beste**: Implementeren en testen van een back-upoplossing.   
**Details**: Een back-up moet worden afgehandeld dat u een andere bewerking verwerken. Dit geldt voor systemen die deel uitmaken van uw productie-omgeving uitbreiden naar de cloud.

Testen en ontwikkelen systemen moeten voldoen aan back-upstrategieën restore-mogelijkheden die vergelijkbaar zijn met wat gebruikers gewend zijn, op basis van hun ervaring met on-premises omgevingen. Productie-workloads naar Azure verplaatst moeten integreren met bestaande back-upoplossingen indien mogelijk. Of u kunt [Azure Backup](../backup/backup-azure-vms-first-look-arm.md) bij het oplossen van uw back-upvereisten.

Organisaties die geen van beleid voor software-update afdwingen worden meer blootgesteld aan aanvallen die gebruikmaken van bekende, eerder vaste beveiligingsproblemen. Om te voldoen aan regelgeving vanuit de sector, moeten bedrijven bewijzen dat ze zijn toegewijd met behulp van de juiste beveiligingscontroles om te zorgen voor de beveiliging van hun workloads zich in de cloud.

Software-update aanbevolen procedures voor een traditionele datacenter en Azure IaaS hebben veel overeenkomsten. We raden u aan uw huidige beleid voor software-update om op te nemen van virtuele machines die zich in Azure.

## <a name="manage-your-vm-security-posture"></a>De beveiligingsstatus van uw virtuele machine beheren
Cyberdreigingen zijn nog in ontwikkeling. Bescherming van uw virtuele machines vereist een bewakingsmogelijkheid die u kunt snel bedreigingen detecteren, voorkomt ongeoorloofde toegang tot uw resources, waarschuwing is geactiveerd en fout-positieven.

Voor het bewaken van de beveiligingsstatus van uw [Windows](../security-center/security-center-virtual-machine.md) en [virtuele Linux-machines](../security-center/security-center-linux-virtual-machine.md), gebruikt u [Azure Security Center](../security-center/security-center-intro.md). In Security Center, door uw virtuele machines te beschermen door te profiteren van de volgende mogelijkheden:

- OS-beveiligingsinstellingen met aanbevolen configuratieregels toepassen.
- Identificeren en downloaden voor systeembeveiliging en essentiële updates die ontbreken mogelijk.
- Implementeer aanbevelingen voor het eindpunt van antimalwarebescherming.
- Schijfversleuteling valideren.
- Beoordelen en herstellen van beveiligingsproblemen.
- Detecteren van bedreigingen.

Security Center actief voor bedreigingen kunt bewaken en mogelijke bedreigingen in beveiligingswaarschuwingen worden blootgesteld. Gecorreleerde bedreigingen worden samengevoegd in één weergave met de naam een beveiligingsincident.

Security Center slaat gegevens op in [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics biedt een query taal en analytics-engine waarmee u inzicht in de werking van uw toepassingen en resources. Gegevens ook worden verzameld van [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md), oplossingen voor het beheer en de agents zijn geïnstalleerd op virtuele machines in de cloud of on-premises. Deze gedeelde functionaliteit helpt u een volledig overzicht van uw omgeving te vormen.

Organisaties die geen sterke beveiliging voor hun virtuele machines afdwingen blijven niet op de hoogte van mogelijke pogingen door onbevoegde gebruikers besturingselementen voor de beveiliging te omzeilen.

## <a name="monitor-vm-performance"></a>VM-prestaties bewaken
Misbruik van de resource is een probleem wanneer VM processen meer bronnen dan ze moeten gebruiken. Prestatieproblemen met een virtuele machine kunnen leiden tot een onderbreking van de service, die in strijd is met de beveiligingsprincipal van beschikbaarheid. Dit is vooral belangrijk voor virtuele machines die worden gehost, IIS of andere webservers, omdat intensief gebruik van CPU of geheugen op een denial of service (DoS) aanval duiden kan. Het is belangrijk om niet alleen reactief VM-toegang bewaken terwijl er een probleem optreedt, maar ook proactief tegen basislijnprestaties gemeten tijdens normale werking.

Het is raadzaam dat u [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) te krijgen van inzicht in de status van uw resource. Azure Monitor-functies:

- [Diagnostische logboeken bronbestanden](../azure-monitor/platform/diagnostic-logs-overview.md): Uw VM-resources bewaakt en identificeert mogelijke problemen die prestaties en beschikbaarheid beschadigen mogelijk.
- [Azure Diagnostics-extensie](../azure-monitor/platform/diagnostics-extension-overview.md): Biedt mogelijkheden voor controle en diagnostische gegevens op Windows-VM's. U kunt deze mogelijkheden inschakelen met de extensie opnemen als onderdeel van de [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md).

Organisaties die geen VM-prestaties controleren kunnen niet bepalen of bepaalde wijzigingen in prestatiepatronen normale of abnormale zijn. Een virtuele machine die wordt verbruikt meer bronnen dan normaal kan wijzen op een aanval van een externe resource of een verdachte proces wordt uitgevoerd in de virtuele machine.

## <a name="encrypt-your-virtual-hard-disk-files"></a>Versleutelen van uw virtuele harde schijf-bestanden
Het is raadzaam dat u uw virtuele harde schijven (VHD's) om uw opstartvolume en gegevensvolumes in rust in de opslag, te beschermen, samen met uw versleutelingssleutels en geheimen versleutelen.

[Azure Disk Encryption](azure-security-disk-encryption-overview.md) helpt u bij het versleutelen van de schijven van uw Windows en Linux IaaS virtuele machines. Azure Disk Encryption gebruikt de industriestandaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw key vault-abonnement. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in Azure Storage.

Hieronder vindt u aanbevolen procedures voor het gebruik van Azure Disk Encryption:

**Beste**: Hiermee schakelt u versleuteling op virtuele machines.   
**Details**: Azure Disk Encryption genereert en schrijft de versleutelingssleutels in uw key vault. Beheer van versleutelingssleutels in uw key vault, vereist Azure AD-verificatie. Maak een Azure AD-toepassing voor dit doel. Voor verificatiedoeleinden wordt gebruikt, kunt u een van beide verificaties client op basis van een geheim of [Azure AD op basis van certificaten clientverificatie](../active-directory/active-directory-certificate-based-authentication-get-started.md).

**Beste**: Gebruik een sleutel van versleutelingssleutel (KEK-sleutel) voor een extra beveiligingslaag voor versleutelingssleutels. Een KEK-sleutel toevoegen aan uw key vault.   
**Details**: Gebruik de [toevoegen AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azurekeyvaultkey) cmdlet voor het maken van een sleutel van versleutelingssleutel in de key vault. U kunt ook een KEK importeren uit uw on-premises hardware security module (HSM) voor sleutelbeheer. Zie voor meer informatie de [Key Vault-documentatie](../key-vault/key-vault-hsm-protected-keys.md). Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. Bewaren van een escrow-kopie van deze sleutel in een on-premises biedt Sleutelbeheer HSM extra beveiliging tegen per ongeluk verwijderen van sleutels.

**Beste**: Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up voordat de schijven worden versleuteld. Back-ups bieden een optie voor siteherstel als het geval is een onverwachte fout opgetreden tijdens het versleutelen.   
**Details**: Virtuele machines met beheerde schijven moeten u een back-up voordat versleuteling plaatsvindt. Nadat een back-up is gemaakt, kunt u de **Set AzVMDiskEncryptionExtension** cmdlet voor het versleutelen van beheerde schijven door op te geven de *- skipVmBackup* parameter. Zie voor meer informatie over hoe u een back-up en herstel van versleutelde virtuele machines, de [Azure Backup](../backup/backup-azure-vms-encryption.md) artikel.

**Beste**: Als u wilt controleren of dat de geheimen van de versleuteling niet overschreden door de regionale grenzen, moet Azure Disk Encryption de sleutelkluis en de VM's die zich in dezelfde regio bevinden.   
**Details**: Maak en gebruik van een key vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld.

Wanneer u Azure Disk Encryption toepast, kunt u voldoen aan de volgende zaken dat vereisen:

- IaaS-VM's worden beveiligd in rust via het industriestandaard versleutelingstechnologie om de organisatorische vereisten voor beveiliging en naleving.
- IaaS-VM's starten onder de klant beheerde sleutels en beleidsregels en u kunt gebruik ervan controleren in uw key vault.

## <a name="next-steps"></a>Volgende stappen
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com
