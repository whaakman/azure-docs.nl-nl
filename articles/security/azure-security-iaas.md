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
ms.date: 06/14/2018
ms.author: barclayn
ms.openlocfilehash: 7c28459aa04c67db8abda54d9f14eb417bd8ed60
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618594"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Aanbevolen beveiligingsprocedures voor IaaS-workloads in Azure

In de meeste infrastructuur als een service (IaaS)-scenario's, [virtuele Azure-machines (VM's)](https://docs.microsoft.com/azure/virtual-machines/) zijn de belangrijkste werkbelasting voor organisaties die gebruikmaken van cloud computing. Dit is vooral duidelijk in [hybride scenario's](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) waar organisaties willen langzaam workloads migreren naar de cloud. In dergelijke scenario's, voert u de [algemene beveiligingsoverwegingen voor IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), en aanbevolen procedures voor beveiliging van toepassing op alle virtuele machines.

Uw eigen verantwoordelijkheid voor beveiliging is gebaseerd op het type van de service in de cloud. De volgende tabel geeft een overzicht van het saldo van de verantwoordelijkheid voor zowel Microsoft als u:

![Verantwoordelijkheidsgebieden ten opzichte van](./media/azure-security-iaas/sec-cloudstack-new.png)

Vereisten voor beveiliging zijn afhankelijk van een aantal factoren, waaronder verschillende typen workloads. Niet een van deze aanbevolen procedures beveiligen op zichzelf uw systemen. Als iets anders in beveiliging, die u moet kiest u de gewenste opties en Zie hoe de oplossingen kunnen vullen elkaar door hiaten invullen.

Dit artikel worden besproken verschillende VM best practices voor beveiliging, elk afgeleid van de klant en directe ervaringen met VM's van Microsoft.

De best practices zijn gebaseerd op een consensus van advies, en ze werken met de huidige mogelijkheden van Azure-platform en functie ingesteld. Omdat de meningen en technologieën na verloop van tijd wijzigen kunnen, in dit artikel wordt bijgewerkt die wijzigingen.

## <a name="use-privileged-access-workstations"></a>Privileged Access Workstations gebruiken

Organisaties misbruik vaak vallen cyberaanvallen omdat beheerders acties uitvoeren tijdens het gebruik van accounts met verhoogde rechten. Hoewel dit mogelijk niet het resultaat van schadelijke activiteiten, dit gebeurt omdat dit is toegestaan voor bestaande configuratie en -processen. De meeste van deze gebruikers het risico van de volgende acties uit conceptueel oogpunt begrijpt, maar nog steeds voor kiezen om te doen ze.

Uitvoeren van handelingen zoals het controleren van e-mail en surfen op Internet onschuldige genoeg lijken. Maar ze mogelijk beschikbaar kunt maken met verhoogde bevoegdheid accounts binnendringen door kwaadwillende actoren. Bladeren door activiteiten, speciaal e-mailberichten of andere methoden kan worden gebruikt voor toegang tot uw onderneming. Wij raden het gebruik van beveiligde beheerwerkstations (saw) voor alle Azure-beheertaken uitvoeren. Saw zijn een manier om blootstelling aan per ongeluk inbreuk te verminderen.

Privileged Access Workstations (paw's) bieden een speciaal besturingssysteem voor gevoelige taken, die is beveiligd tegen aanvallen via Internet en dreigingsvectoren. Het scheiden van gevoelige taken en accounts van het dagelijkse gebruik voor werkstations en apparaten biedt sterke beveiliging. Dankzij deze scheiding beperkt de gevolgen van phishing-aanvallen, toepassing en OS beveiligingsproblemen, verschillende imitatie-aanvallen en referentie diefstal aanvallen. (toetsaanslagen logboekregistratie, Pass-the-Hash en Pass-the-Ticket)

De PAW-benadering is een uitbreiding van de goede en aanbevolen procedure voor het gebruik van een afzonderlijk toegewezen beheerdersaccount. De Administrator-account is gescheiden van een standaardgebruikersaccount. Een PAW biedt een betrouwbaar werkstation voor deze gevoelige accounts.

Zie voor meer informatie en de implementatie-instructies, [Privileged Access Workstations](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Multi-factor Authentication gebruiken

In het verleden is de netwerkperimeter gebruikt om toegang tot zakelijke gegevens te beheren. In een wereld cloud en mobiliteit identiteit van de controlelaag is: U deze gebruiken voor toegang tot IaaS-services beheren vanaf elk apparaat. U deze ook gebruiken om op te halen van zichtbaarheid en inzicht in waar en hoe uw gegevens wordt gebruikt. Beveiligen van de digitale identiteit van uw Azure-gebruikers, is de hoeksteen van het beveiligen van uw abonnementen van diefstal van identiteiten en andere cybercrimes.

Een van de meest zinvol stappen die u ondernemen kunt om een account te beveiligen is het inschakelen van verificatie met twee factoren. Verificatie met twee factoren is een manier om verificatie met behulp van iets naast een wachtwoord. Het helpt het risico van toegang door iemand die u beheert om een wachtwoord van iemand anders te krijgen.

[Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) om de toegang tot gegevens en toepassingen beveiligen terwijl aan de wensen van gebruikers voor een eenvoudige aanmeldprocedure. Het biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties--telefoongesprek, tekstbericht of mobiele app-meldingen. Gebruikers de methode kiezen die ze de voorkeur geven.

De eenvoudigste manier om het gebruik van multi-factor Authentication is de mobiele Microsoft Authenticator-app die kan worden gebruikt op mobiele apparaten met Windows, iOS en Android. Met de meest recente versie van Windows 10 en de integratie van on-premises Active Directory met Azure Active Directory (Azure AD), [Windows Hello voor bedrijven](../active-directory/active-directory-azureadjoin-passport-deployment.md) kan worden gebruikt voor naadloze eenmalige aanmelding tot Azure-resources. In dit geval wordt de Windows 10-apparaat gebruikt als de tweede factor voor verificatie.

Voor accounts die voor het beheren van uw Azure-abonnement en voor accounts die kunnen zich aanmelden bij virtuele machines, biedt met behulp van multi-factor Authentication u een veel hoger beveiligingsniveau dan het gebruik van alleen een wachtwoord. Andere vormen van tweeledige verificatie werkt net zo goed mogelijk, maar ze kan ingewikkeld zijn als ze niet al in de productieomgeving.

De volgende schermafbeelding ziet u enkele van de beschikbare opties voor Azure multi-factor Authentication:

![Meervoudige verificatie-opties](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Beperken van toegang op beheerdersniveau te beperken

Het is belangrijk voor het beveiligen van de accounts die uw Azure-abonnement kunnen beheren. De inbreuk op een van deze accounts genegeerd de waarde van alle andere stappen die u nemen kunt om te controleren of de vertrouwelijkheid en integriteit van uw gegevens. Als onlangs geïllustreerd door het [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) interne aanvallen vormen een enorme bedreiging voor de algehele beveiliging van elke organisatie.

Personen voor beheerdersrechten evalueren door de volgende criteria die vergelijkbaar is met deze:

- Zijn ze uitvoeren van taken waarvoor administratorbevoegdheden?
- Hoe vaak de taken worden uitgevoerd?
- Is er een specifieke reden waarom de taken kunnen niet worden uitgevoerd door een andere beheerder namens hen?

Documenteer alle andere bekende alternatieve methoden voor het toekennen van de bevoegdheden en waarom deze niet geschikt.

Just-in-time-beheer wordt voorkomen dat de onnodige sprake is van accounts met verhoogde rechten tijdens perioden wanneer deze rechten niet nodig zijn. Accounts hebben verhoogde bevoegdheden voor een beperkte periode zodat administrators hun werk kunnen doen. En vervolgens deze rechten aan het einde van een verschuiving zijn verwijderd of wanneer een taak is voltooid.

U kunt [Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md) om te beheren, bewaking en beheer toegang in uw organisatie. Hiermee kunt u blijven op de hoogte van de acties die personen in uw organisatie uitvoeren. Het biedt ook just-in-time-beheer met Azure AD door de introductie van het concept van in aanmerking komende beheerders. Dit zijn personen met een account met de mogelijkheid beheerdersrechten worden verleend. Deze typen gebruikers een activeringsproces kunnen doorlopen en beheerdersrechten worden verleend voor een beperkte periode.

## <a name="use-devtest-labs"></a>Gebruik DevTest Labs

Azure voor labs en ontwikkelomgevingen gaat onmiddellijk de vertragingen introductie van een inkoopanalyse hardware. Hierdoor kunnen organisaties flexibiliteit bij het testen en ontwikkelen. Aan de andere kant leiden een gebrek aan bekend zijn met Azure of een wens om u te helpen bij de acceptatie versnellen de beheerder kan worden te soepele met toewijzing van gebruiksrecht. Dit risico kan per ongeluk de organisatie interne aanvallen worden blootgesteld. Sommige gebruikers mogelijk veel meer toegang dan ze moeten worden verleend.

De [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) service maakt gebruik van [toegangsbeheer](../role-based-access-control/overview.md) (RBAC). Met behulp van RBAC, kunt u taken scheiden binnen uw team in rollen die alleen het niveau van toegang die nodig zijn voor gebruikers om hun werkzaamheden te verlenen. RBAC wordt geleverd met vooraf gedefinieerde rollen (eigenaar, lab-gebruiker en Inzender). U kunt deze rollen zelfs gebruiken om rechten toewijzen aan externe partners en samenwerking aanzienlijk te vereenvoudigen.

Omdat DevTest Labs RBAC gebruikt, is het mogelijk te maken als u meer, [aangepaste rollen](../lab-services/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs niet alleen vereenvoudigt het beheer van machtigingen, het vereenvoudigt het proces van het ophalen van omgevingen ingericht. Ook helpt u om te gaan met andere typische uitdagingen van teams die op de ontwikkel- en testomgevingen. Deze voorbereiding vereist, maar op de lange termijn, deze wordt eenvoudiger voor uw team.

Azure DevTest Labs-functies zijn onder andere:

- Administratieve controle over de opties die beschikbaar zijn voor gebruikers. De beheerder kan bijvoorbeeld toegestane VM-grootten, maximum aantal virtuele machines, en wanneer virtuele machines worden gestart en afsluiten centraal beheren.
- Automatisering van lab-omgeving maken.
- Kosten bijhouden.
- Vereenvoudigde verdeling van virtuele machines van tijdelijke mensen samenwerken.
- Self-service waarmee gebruikers hun labs inrichten met behulp van sjablonen.
- Het beheren en verbruik te beperken.

![DevTest Labs](./media/azure-security-iaas/devtestlabs.png)

Er zijn geen extra kosten is gekoppeld aan het gebruik van DevTest Labs. Het maken van labs, beleid, sjablonen en artefacten is gratis. U betaalt alleen de Azure-resources die in uw labs, zoals virtuele machines, opslagaccounts en virtuele netwerken worden gebruikt.

## <a name="control-and-limit-endpoint-access"></a>Controle en limiet endpoint-toegang

Het hosten van labs of productiesystemen in Azure betekent dat uw systemen moeten toegankelijk zijn vanaf Internet. Standaard een nieuwe virtuele machine voor Windows is de RDP-poort die toegankelijk is vanaf het Internet en een virtuele Linux-machine heeft de SSH-poort openen. Stappen naar limiet blootgesteld eindpunten is nodig om het minimaliseren van de kans op onbevoegde toegang.

Met behulp van technologieën in Azure kunnen u beperken de toegang tot deze administratieve eindpunten. In Azure, kunt u [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) (nsg's). Wanneer u Azure Resource Manager voor implementatie gebruikt, beperkt nsg's de toegang via alle netwerken met alleen de beheereindpunten (RDP of SSH). Wanneer u nsg's denkt, denkt u router ACL's. U kunt deze gebruiken voor het beheren van nauw de netwerkcommunicatie tussen verschillende segmenten van uw Azure-netwerken. Dit is vergelijkbaar met het maken van netwerken in perimeternetwerken of andere geïsoleerde netwerken. Ze niet dan het gegevensverkeer inspecteren, maar ze met segmentatie helpen.

Een meer dynamische manier van het beperken van toegang tot virtuele machines is het gebruik van Azure Security center [Just-in-time-beheer](../security-center/security-center-just-in-time.md). Security center kunt vergrendelen van uw Azure VM's en biedt toegang wanneer dat nodig is. Het proces werkt doordat de toegang tot een gebruiker die deze aanvragen nadat u hebt gecontroleerd die op basis van hun [Role-Based Access control](../role-based-access-control/role-assignments-portal.md) (RBAC) hebben de vereiste machtigingen. Azure Security center zal Breng de benodigde Netwerkbeveiligingsgroepen (nsg's) naar binnenkomend verkeer toestaan.

### <a name="site-to-site-vpnvpn-gatewayvpn-gateway-howto-site-to-site-resource-manager-portalmd"></a>[Site-naar-site-VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Een site-naar-site-VPN-verbinding breidt uw on-premises netwerk naar de cloud. Dit biedt u een andere mogelijkheid u nsg's, omdat u kunt ook de nsg's niet om toegang te verlenen vanaf elke locatie dan het lokale netwerk wijzigen. Vervolgens kunt u vereisen dat worden beheerd via een eerste verbinding met het Azure-netwerk via VPN.

Het is mogelijk dat de site-naar-site VPN-optie meest aantrekkelijk in gevallen waarin u productiesystemen die nauw worden geïntegreerd met uw on-premises resources in Azure worden gehost.

### <a name="point-to-sitevpn-gatewayvpn-gateway-howto-point-to-site-rm-psmd"></a>[Punt-naar-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

In situaties waar u voor het beheren van systemen die niet nodig voor toegang tot on-premises bronnen hebt. Deze systemen kunnen worden geïsoleerd in hun eigen Azure-netwerk. Beheerders kunnen VPN in Azure gehoste omgeving van hun werkstation voor beheer.

>[!NOTE]
>U kunt een van de VPN-opties gebruiken opnieuw configureren van de ACL's op de nsg's niet om toegang te verlenen aan de eindpunten voor beheer van Internet.

### <a name="remote-desktop-gatewayactive-directoryauthenticationhowto-mfaserver-nps-rdgmd"></a>[Extern bureaublad-gateway](../active-directory/authentication/howto-mfaserver-nps-rdg.md)

Extern bureaublad-Gateway kunt u een beveiligde verbinding met extern bureaublad-servers via HTTPS, terwijl de gedetailleerdere besturingselementen toe te passen op deze verbindingen.

Functies die u toegang toe hebt als u wilt opnemen:

- Opties voor beheerders om te beperken van verbindingen op aanvragen van specifieke systemen.
- Smart card-verificatie of Azure multi-factor Authentication.
- Bepalen via welke systemen iemand kunt verbinden via de gateway.
- Omleiding van het apparaat en de schijf kunt beheren.

### <a name="vm-availability"></a>VM-beschikbaarheid

Als een virtuele machine wordt uitgevoerd voor kritieke toepassingen die u wilt een hoge beschikbaarheid hebben, is het raadzaam dat meerdere virtuele machines worden gebruikt. Voor betere beschikbaarheid, maakt u ten minste twee virtuele machines in de [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure Load Balancer](../load-balancer/load-balancer-overview.md) vereist ook dat netwerktaakverdeling VM's deel uitmaken van dezelfde beschikbaarheidsset. Als deze virtuele machines moeten worden geopend via Internet, moet u een [internetgerichte load balancer](../load-balancer/load-balancer-internet-overview.md).

## <a name="use-a-key-management-solution"></a>Gebruik van een oplossing voor sleutelbeheer

Veilig sleutelbeheer is essentieel voor de bescherming van gegevens in de cloud. Met [Azure Key Vault](../key-vault/key-vault-whatis.md), kunt u versleutelingssleutels veilig opslaan en kleine geheimen zoals wachtwoorden in hardware security modules (HSM's). Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's.

De verwerkt Microsoft uw sleutels in FIPS 140-2 niveau 2 gevalideerde HSM's (hardware en firmware). Beheer en controleer sleutelgebruik met Azure-Logboeken: kunt Logboeken openen in Azure of uw Security Information and Event Management (SIEM) systeem voor aanvullende analyse en detectie van bedreigingen.

Iedereen met een Azure-abonnement kunt maken en gebruiken van sleutelkluizen. Hoewel Key Vault voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan worden geïmplementeerd en beheerd door een beheerder die verantwoordelijk is voor het beheren van Azure-services in een organisatie.

## <a name="encrypt-virtual-disks-and-disk-storage"></a>Virtuele schijven en opslag op de schijf versleutelen

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) adressen van de bedreiging van gegevensdiefstal of blootstelling tegen onbevoegde toegang, die wordt bereikt door het verplaatsen van een schijf. De schijf kan worden gekoppeld aan een ander systeem als een manier om andere beveiligingsmechanismen overslaan. Schijf maakt gebruik van versleuteling [BitLocker](https://technet.microsoft.com/library/hh831713) in Windows en DM-Crypt in Linux-besturingssysteem en gegevensstations versleutelen. Azure Disk Encryption kan worden geïntegreerd met Key Vault om te controleren en beheren van de versleutelingssleutels. Het is beschikbaar voor standard VM's en VM's met premium storage.

Zie voor meer informatie, [Azure Disk Encryption in Windows en Linux IaaS-VM's](azure-security-disk-encryption.md).

[Azure Storage-Serviceversleuteling](../storage/common/storage-service-encryption.md) beschermt gegevens in rust. Het ingeschakeld op het niveau van de storage-account. Gegevens worden gecodeerd als ze worden geschreven in onze datacenters en deze automatisch ontsleuteld en dan als u toegang krijgt. Het ondersteunt de volgende scenario's:

- Versleuteling van blok-blobs, toevoeg-blobs en pagina-blobs
- Versleuteling van gearchiveerde VHD's en sjablonen die zijn overgebracht naar Azure vanaf on-premises
- Versleuteling van het onderliggende besturingssysteem en gegevensschijven voor IaaS-VM's die u hebt gemaakt met behulp van uw VHD 's

Voordat u met het Azure Storage-versleuteling verdergaat, worden op de hoogte van twee beperkingen:

- Het is niet beschikbaar voor klassieke opslagaccounts.
- Deze versleutelt alleen de gegevens die zijn geschreven als versleuteling is ingeschakeld.

## <a name="use-a-centralized-security-management-system"></a>Gebruik een beheersysteem gecentraliseerde beveiliging

Uw servers moeten worden bewaakt voor toepassen van patches, configuratie, gebeurtenissen en activiteiten die mogelijk problemen met de beveiliging worden beschouwd. Om deze problemen op te lossen, kunt u [Security Center](https://azure.microsoft.com/services/security-center/) en [Operations Management Suite Security and Compliance](https://azure.microsoft.com/services/security-center/). Beide opties verder gaan dan de configuratie van het besturingssysteem. Ze bieden ook bewaking van de configuratie van de onderliggende infrastructuur, zoals de configuratie van het netwerk en het gebruik van virtueel apparaat.

## <a name="manage-operating-systems"></a>Besturingssystemen beheren

In een IaaS-implementatie bent u nog steeds zelf verantwoordelijk voor het beheer van de systemen die u implementeert, net als bij een andere server of werkstation in uw omgeving. Toepassen van patches, beveiliging, rechten toewijzingen en een willekeurige andere activiteit met betrekking tot het onderhoud van het systeem nog steeds uw eigen verantwoordelijkheid. Voor systemen die zijn geïntegreerd met uw on-premises resources, kunt u dezelfde hulpprogramma's en procedures die u on-premises voor zaken zoals virussen, anti-malware gebruikt, patches en back-up gebruiken.

### <a name="harden-systems"></a>Beveiliging van systemen

Alle virtuele machines in Azure IaaS moet worden beveiligd zodat ze beschikbaar maakt alleen service-eindpunten die vereist zijn voor de toepassingen die zijn geïnstalleerd. Voor Windows virtual machines, volgt u de aanbevelingen die Microsoft publiceert als basislijnen voor de [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) oplossing.

Security Compliance Manager is een gratis hulpprogramma. U kunt deze gebruiken om snel te configureren en beheren van uw bureaubladen, traditionele datacenter en private en openbare cloud met behulp van Groepsbeleid en System Center Configuration Manager.

Security Compliance Manager biedt ready-to-deploy beleidsregels en Desired Configuration Management-configuratiepakketten die zijn getest. Deze basislijnen zijn gebaseerd op [Microsoft-beveiligingsrichtlijnen](https://technet.microsoft.com/library/cc184906.aspx) aanbevelingen en branche aanbevolen procedures. Ze helpen u configuratie drift, adres nalevingsvereisten, beheren en te verminderen van beveiligingsrisico's.

Security Compliance Manager kunt u de huidige configuratie van uw computers importeren met behulp van twee verschillende methoden. Ten eerste kunt u Groepsbeleid op basis van Active Directory importeren. Ten tweede kunt u de configuratie van een 'golden master' importeren referentiecomputer met behulp van de [LocalGPO hulpprogramma](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) back-up van het lokale Groepsbeleid. U kunt het lokale Groepsbeleid vervolgens importeren in Security Compliance Manager.

Vergelijk uw standaarden branche aanbevolen procedures, ze aanpassen en nieuwe beleidsregels en Desired Configuration Management-configuratiepakketten maken. Basislijnen zijn gepubliceerd voor alle ondersteunde besturingssystemen, met inbegrip van Windows 10 Verjaardag Update en Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Installeren en beheren van antimalware

Voor omgevingen die afzonderlijk worden gehost in uw productieomgeving, kunt u een anti-malware-extensie om u te helpen bij het beveiligen van uw virtuele machines en cloudservices. Het kan worden geïntegreerd met [Azure Security Center](../security-center/security-center-intro.md).

[Microsoft Antimalware](azure-security-antimalware.md) bevat functies zoals realtime-beveiliging, geplande scan, oplossen van malware, handtekeningupdates, engine-updates, rapportage, uitsluitingsverzameling gebeurtenis, voorbeelden en [PowerShell-ondersteuning](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureserviceantimalwareextension).

![Azure anti-malware](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>Installeer de meest recente beveiligingsupdates 

Sommige van de eerste werkbelastingen die klanten naar Azure verplaatsen zijn labs en externe systemen. Als uw Azure gehoste virtuele machines host toepassingen of services die toegankelijk zijn met het Internet moeten zijn, worden waakzaam over patches. Patch dan het besturingssysteem. Niet-gepatchte kwetsbaarheden van toepassingen van derden kunnen ook leiden tot problemen die kunnen worden voorkomen als goed patchbeheer aanwezig is.

### <a name="deploy-and-test-a-backup-solution"></a>Implementeren en testen van een back-upoplossing

Net als bij beveiligingsupdates moet een back-up worden afgehandeld dat u een andere bewerking verwerken. Dit geldt voor systemen die deel uitmaken van uw productie-omgeving uitbreiden naar de cloud. Testen en ontwikkelen systemen moeten voldoen aan back-upstrategieën restore-mogelijkheden die vergelijkbaar zijn met wat gebruikers gewend zijn, op basis van hun ervaring met on-premises omgevingen.

Productie-workloads naar Azure verplaatst moeten integreren met bestaande back-upoplossingen indien mogelijk. Of u kunt [Azure Backup](../backup/backup-azure-arm-vms.md) bij het oplossen van uw back-upvereisten.

## <a name="monitor"></a>Controleren

### <a name="security-centersecurity-centersecurity-center-intromd"></a>[Security Center](../security-center/security-center-intro.md)

Security center biedt continue evaluatie van de beveiligingsstatus van uw Azure-resources om mogelijke beveiligingsproblemen te identificeren. Een lijst met aanbevelingen begeleidt u bij het configureren van benodigde besturingselementen.

Voorbeelden zijn:

- Inrichting van antimalware om te identificeren en verwijderen van schadelijke software.
- Netwerkbeveiligingsgroepen en regels voor het verkeer beheren om virtuele machines te configureren.
- Web application firewalls om te beschermen tegen aanvallen die zijn gericht inrichten van uw webtoepassingen.
- Ontbrekende systeemupdates implementeren.
- Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen.

De volgende afbeelding ziet u enkele van de opties die u in Security Center kunt inschakelen.

![Azure Security Center-beleid](./media/azure-security-iaas/security-center-policies.png)

### <a name="operations-management-suiteoperations-management-suiteoperations-management-suite-overviewmd"></a>[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) 

Operations Management Suite is een Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u kunt beheren en beveiligen van uw on-premises en cloudinfrastructuur. Omdat Operations Management Suite als een cloud-gebaseerde service is geïmplementeerd, kunnen worden geïmplementeerd snel en met minimale investeringen in infrastructuurresources.

Nieuwe functies worden automatisch geleverd, waardoor u vanuit het voortdurende onderhoud en upgrades van kosten. Operations Management Suite is ook geïntegreerd met System Center Operations Manager. Deze verschillende onderdelen kunt u uw Azure-workloads, met inbegrip van beter te beheren is een [beveiliging en naleving](../operations-management-suite/oms-security-getting-started.md) module.

U kunt de functies voor beveiliging en naleving in Operations Management Suite gebruiken om informatie over uw resources weer te geven. De informatie is onderverdeeld in vier hoofdcategorieën:

- **Beveiligingsdomeinen**: nader bestuderen beveiligingsrecords in de loop van de tijd. Toegang tot malware-evaluatie, evaluatie, netwerkgegevens voor beveiliging, identiteit en toegang tot gegevens en computers met beveiligingsgebeurtenissen bijwerken. Profiteer van snelle toegang tot het Azure Security Center-dashboard.
- **Problemen die aandacht vereisen**: snel het aantal actieve problemen en de ernst van deze problemen kunt identificeren.
- **Detecties (preview)**: aanval identificeren patronen door beveiligingswaarschuwingen te visualiseren wanneer deze zich op basis van uw resources voordoen.
- **Bedreigingsinformatie**: aanval identificeren patronen door te visualiseren van het totale aantal servers met schadelijk uitgaand IP-verkeer, het type schadelijke dreigingen en een kaart waarop wordt aangegeven waar deze IP-adressen afkomstig zijn.
- **Algemene Beveiligingsquery's**: een overzicht van de meest voorkomende Beveiligingsquery's die u gebruiken kunt voor het bewaken van uw omgeving. Wanneer u een van deze query's, klikt u op de **zoeken** blade wordt geopend en toont de resultaten voor deze query.

De volgende Schermafbeelding toont een voorbeeld van de informatie die Operations Management Suite kunt weergeven.

![Operations Management Suite security basislijnen](./media/azure-security-iaas/oms-security-baseline.png)

### <a name="monitor-vm-performance"></a>VM-prestaties bewaken

Misbruik van de resource is een probleem wanneer VM processen meer bronnen dan ze moeten gebruiken. Prestatieproblemen met een virtuele machine kunnen leiden tot een onderbreking van de service, die in strijd is met de beveiligingsprincipal van beschikbaarheid. Daarom is het belangrijk om het VM-toegang niet alleen reactief bewaken terwijl er een probleem optreedt, maar ook proactief tegen basislijnprestaties gemeten tijdens normale werking.

Door te analyseren [Azure diagnostische logboekbestanden](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), kunt u uw VM-resources controleren en identificeren van mogelijke problemen die prestaties en beschikbaarheid beschadigen mogelijk. De Azure Diagnostics-extensie biedt mogelijkheden voor controle en diagnostische gegevens op Windows gebaseerde virtuele machines. U kunt deze mogelijkheden inschakelen met de extensie opnemen als onderdeel van de [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md).

U kunt ook [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) te krijgen van inzicht in de status van uw resources.

Organisaties die geen VM-prestaties controleren zijn kan niet bepalen of bepaalde wijzigingen in prestatiepatronen normale of abnormale zijn. Als de virtuele machine van de meer bronnen dan normaal gebruikmaakt al, kan deze een anomalie duiden op een mogelijke aanval vanaf een externe resource of een verdachte proces wordt uitgevoerd in de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* [Blog van het Azure-beveiligingsteam](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md)
