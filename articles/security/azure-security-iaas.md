---
title: Security best practices voor IaaS werkbelastingen in Azure | Microsoft Docs
description: " De migratie van werkbelastingen naar Azure IaaS brengt verkoopkansen te beoordelen onze ontwerpen "
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
ms.date: 11/21/2017
ms.author: barclayn
ms.openlocfilehash: 376a3e47e5099aa4d74732e0b6ed14ed9af14091
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Aanbevolen beveiligingsprocedures voor IaaS-workloads in Azure

Als u nadenken over het verplaatsen van werkbelastingen naar Azure-infrastructuur als een service (IaaS) hebt gestart, gerealiseerde u waarschijnlijk dat er enkele overwegingen bekend bent. U hebt mogelijk al ervaring voor het beveiligen van virtuele omgevingen. Wanneer u Azure IaaS naar, kunt u uw kennis in het beveiligen van virtuele omgevingen van toepassing en een nieuwe set opties gebruiken om u te helpen beveiligen uw assets.

Laten we beginnen door te zeggen dat mag niet naar verwachting lokale bronnen als een-op-een meenemen naar Azure. De nieuwe uitdagingen en de nieuwe opties brengt een kans om de bestaande Herzie deigns, hulpprogramma's voor databaseontwikkeling en verwerkt.

Uw verantwoordelijkheid voor beveiliging is gebaseerd op het type van de cloudservice. Het volgende diagram ziet u het saldo van de verantwoordelijkheid voor zowel Microsoft als u:


![Verantwoordelijkheidsgebieden](./media/azure-security-iaas/sec-cloudstack-new.png)


Gaan we enkele van de beschikbare opties in Azure kunt u voldoen aan de beveiligingsvereisten van uw organisatie. Houd er rekening mee dat de beveiligingsvereisten voor verschillende typen werkbelastingen kunnen verschillen. Niet een van deze best practices kan zelfstandig Beveilig uw systemen. U hebt zoals iets anders in de beveiliging, kies de gewenste opties en Zie hoe de oplossingen kunnen elkaar aanvullen door hiaten.

## <a name="use-privileged-access-workstations"></a>Privileged Access Workstations gebruiken

Organisaties handig vaak vallen voor cyberattacks omdat beheerders acties uitvoeren tijdens het gebruik van accounts met verhoogde rechten. Meestal dit met kwaadaardige bedoelingen wordt niet uitgevoerd, maar omdat de bestaande configuratie en processen toe te staan. De meeste van deze gebruikers het risico van de volgende acties uit conceptueel oogpunt begrijpt, maar nog steeds ervoor kiezen om het uitvoeren van deze.

Opdrachten, zoals het controleren van e-mail en surfen op Internet onschuldige genoeg lijken uit te voeren. Maar verhoogde accounts binnendringen door schadelijke actoren kan worden blootgesteld. Bladeren door activiteiten, speciaal e-mailberichten of andere technieken kan worden gebruikt voor toegang tot uw onderneming. We raden het gebruik van beveiligde beheerwerkstations (zagen) voor de uitvoering van alle beheertaken voor Azure. Zagen vormen een van de blootstelling aan onbedoeld inbreuk te verminderen.

Privileged Access Workstations (poten) bieden een toegewijde besturingssysteem voor gevoelige taken--één die is beveiligd tegen aanvallen via Internet en bedreigingen met zich mee. Scheiden deze gevoelige taken en de accounts van het dagelijks gebruik werkstations en apparaten biedt sterke beveiliging. Deze scheiding beperkt de gevolgen van phishingaanvallen, toepassing en OS beveiligingsproblemen, verschillende imitatie aanvallen en credential theft aanvallen. (toetsaanslagen aan te melden, Pass-the-Hash en Pass-the-Ticket)

De PAW aanpak is een uitbreiding van de praktijk gangbaar zijn en aanbevolen een afzonderlijk toegewezen Administrator-account gebruiken. De Administrator-account is gescheiden van een standaardgebruikersaccount. Een PAW biedt een betrouwbare werkstation voor die gevoelige accounts.

Zie voor meer informatie en de implementatie-instructies, [Privileged Access Workstations](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Meervoudige verificatie gebruiken

In het verleden uw netwerkverbinding gebruikt om toegang tot zakelijke gegevens te beheren. In een wereld cloud eerste, mobile eerste identiteit is het besturingselement vlak: U deze gebruiken voor toegang tot IaaS services beheren vanaf elk apparaat. U deze ook gebruiken om op te halen zichtbaarheid en inzicht in hoe en waar uw gegevens wordt gebruikt. Beveiligen van de digitale identiteit van uw Azure-gebruikers, vormt de basis van het beveiligen van uw abonnementen van identiteitsdiefstal en andere cybercrimes.

Een van de meeste zin stappen die u ondernemen kunt om een account te beveiligen is tweeledige verificatie in te schakelen. Tweeledige verificatie is een manier te verifiëren met behulp van iets naast een wachtwoord. Het helpt het risico van toegang door iemand die u beheert om een wachtwoord van iemand anders te achterhalen.

[Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) u toegang tot gegevens en toepassingen beveiligt en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Sterke verificatie met een bereik van eenvoudige verificatie-opties--telefoongesprek, tekstbericht of mobiele-appmelding levert. Gebruikers de methode kiezen die ze de voorkeur geven.

De eenvoudigste manier om meervoudige verificatie gebruiken is de mobiele app van Microsoft Authenticator die kan worden gebruikt op mobiele apparaten met Windows, iOS en Android. Met de meest recente versie van Windows 10 en de integratie van de lokale Active Directory met Azure Active Directory (Azure AD), [Windows Hello voor bedrijven](../active-directory/active-directory-azureadjoin-passport-deployment.md) kan worden gebruikt voor naadloze eenmalige aanmelding voor Azure-resources. In dit geval wordt wordt het apparaat met Windows 10 gebruikt als tweede factor voor verificatie.

Voor accounts die uw Azure-abonnement beheren en voor accounts die aan virtuele machines kunnen aanmelden hebt met multi-factor Authentication u een veel hoger niveau van beveiliging dan het gebruik van alleen een wachtwoord. Andere vormen van tweeledige verificatie werken net zo goed mogelijk, maar deze kunnen mogelijk ingewikkeld als ze nog niet in de productieomgeving.

De volgende schermafbeelding ziet u enkele van de opties die beschikbaar zijn voor Azure multi-factor Authentication:

![Opties voor meervoudige verificatie](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Beperk de beheerderstoegang te beperken

Het is zeer belangrijk voor het beveiligen van de accounts die uw Azure-abonnement kunnen beheren. De inbreuk op een van deze accounts genegeerd de waarde van alle andere stappen die u ondernemen kunt om te controleren of de vertrouwelijkheid en integriteit van uw gegevens. Als onlangs geïllustreerd door het [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) interne aanvallen bedreigen grote de algehele beveiliging van elke organisatie.

Personen voor beheerdersrechten evalueren door de volgende criteria zoals de volgende:

- Presteren ze taken die u hebt u beheerdersbevoegdheden nodig?
- Hoe vaak worden de taken uitgevoerd?
- Is er een specifieke reden waarom de taken kunnen niet worden uitgevoerd door een andere beheerder namens hen?

Documenteer alle andere bekende alternatieve benaderingen aan het toekennen van de bevoegdheden en waarom elk kan niet worden geaccepteerd.

Het gebruik van just in time-beheer wordt voorkomen dat de onnodige aanwezigheid van accounts met verhoogde rechten tijdens perioden wanneer deze rechten niet nodig zijn. Accounts wel verhoogde bevoegdheden hebben voor een beperkte tijd zodat beheerders kunnen hun werk. Deze rechten worden vervolgens verwijderd aan het einde van een verschuiving of wanneer een taak is voltooid.

U kunt [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) om te beheren, bewaking en beheer openen in uw organisatie. U kunt blijven op de hoogte van de acties die personen in uw organisatie uitvoeren. Ook biedt door de introductie van het concept van in aanmerking komende beheerders just in time-beheer voor het Azure AD. Dit zijn personen die een account met de mogelijkheid om te worden toegekend-beheerdersrechten. Deze typen van gebruikers kunnen doorlopen die een proces voor het activeren en beheerdersrechten worden verleend voor een beperkte periode.


## <a name="use-devtest-labs"></a>Gebruik DevTest Labs

Met behulp van Azure voor labs en ontwikkelomgevingen kan organisaties om te testen en ontwikkeling van de flexibiliteit door de vertragingen die hardware inkoop introduceert Blijf krijgen. Een gebrek aan bekend bent met Azure of wil dragen bij aan de overstap kan helaas leiden dat de beheerder moet te soepele met toewijzing van gebruiksrecht. Dit risico kan de organisatie om interne aanvallen onbedoeld worden blootgesteld. Sommige gebruikers mogelijk veel meer toegang dan ze moeten worden verleend.

De [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) service gebruikt [rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-what-is.md) (RBAC). Met behulp van RBAC kunt u taken scheiden binnen uw team in rollen die verlenen alleen het niveau van toegang nodig zijn voor gebruikers om hun werk te doen. RBAC wordt geleverd met vooraf gedefinieerde functies (eigenaar, lab-gebruiker en Inzender). U kunt zelfs deze rollen toewijzen aan externe partners en samenwerking aanzienlijk te vereenvoudigen.

Omdat DevTest Labs RBAC gebruikt, is het mogelijk te maken als u meer, [aangepaste rollen](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs niet alleen vereenvoudigt het beheer van machtigingen, het vereenvoudigt het proces van het ophalen van omgevingen ingericht. Hiermee kunt u ook andere typische uitdagingen van teams die op een ontwikkel- en testomgevingen werkt behandelt. Voorbereiding is vereist, maar de lange termijn, deze wordt gemakkelijker voor uw team.

Azure DevTest Labs functies:

- Beheerrechten voor de opties die beschikbaar zijn voor gebruikers. De beheerder kan bijvoorbeeld toegestane VM-grootten, het maximale aantal van virtuele machines, en wanneer virtuele machines worden gestart en afsluiten centraal beheren.
- Automatisering van lab maken.
- Kosten bijhouden.
- Vereenvoudigde verdeling van virtuele machines tijdelijke samenwerkingsmogelijkheden.
- Self-service waarmee gebruikers hun labs inrichten met behulp van sjablonen.
- Beheren en beperken van verbruik.

![Een lab maken via een DevTest Labs](./media/azure-security-iaas/devtestlabs.png)

Kan zonder extra kosten is gekoppeld aan het gebruik van DevTest Labs. Het maken van labs, beleid, sjablonen en artefacten is gratis. U betaalt voor alleen de Azure-resources in uw labs, zoals virtuele machines, opslagaccounts en virtuele netwerken gebruikt.



## <a name="control-and-limit-endpoint-access"></a>Controle- en limit endpoint-toegang

Labs of productiesystemen in Azure hosten, betekent dat uw systemen moeten toegankelijk zijn vanaf Internet. Standaard een nieuwe Windows virtuele machine heeft de RDP-poort die toegankelijk is vanaf het Internet en een virtuele Linux-machine heeft de SSH-poort openen. Doen om met limiet blootgesteld eindpunten is nodig om te voorkomen dat onbevoegde toegang.

Technologieën in Azure kunt u de toegang tot deze administratieve eindpunten te beperken. In Azure, kunt u [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) (nsg's). Wanneer u Azure Resource Manager voor implementatie gebruikt, beperkt nsg's de toegang van alle netwerken tot alleen de eindpunten voor beheer (RDP of SSH). Als u nsg's nadenkt, beschouw router ACL's. U kunt ze beheren de netwerkcommunicatie tussen verschillende segmenten van uw Azure-netwerken. Dit is vergelijkbaar met het maken van netwerken in het perimeternetwerk of andere geïsoleerde netwerken. Ze doen het verkeer niet controleren, maar ze helpen bij netwerksegmentering.


In Azure, configureert u een [site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) van uw on-premises netwerk. Een site-naar-site VPN breidt uw on-premises netwerk naar de cloud. Hierdoor kunt u een andere u NSGs, omdat u kunt ook het nsg's niet om toegang te verlenen vanaf ergens anders dan het lokale netwerk wijzigen. Vervolgens kunt u vereisen dat het beheer wordt uitgevoerd door de eerste verbinding te maken met het Azure-netwerk via VPN.

Mogelijk is de optie site-naar-site-VPN-meest aantrekkelijk in gevallen waarin u productiesystemen die nauw worden geïntegreerd met uw lokale resources in Azure worden gehost.

U kunt ook de [punt-naar-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) optie in situaties waar u het beheer van systemen die geen toegang nodig tot lokale bronnen. Deze systemen kunnen worden geïsoleerd in hun eigen virtuele Azure-netwerk. Beheerders kunnen VPN in Azure gehoste omgeving van hun werkstation voor beheer.

>[!NOTE]
>U kunt beide opties voor VPN-opnieuw configureren van de ACL's voor het nsg's op geen toegang tot eindpunten voor beheer van het Internet toestaat.

Een andere optie waard is een [extern bureaublad-Gateway](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md) implementatie. U kunt deze implementatie veilig verbinding kunnen maken met extern bureaublad-servers via HTTPS, tijdens het toepassen van gedetailleerdere besturingselementen op deze verbindingen gebruiken.

Functies die u toegang zou hebben om op te nemen:

- Beheerder opties voor het aantal verbindingen beperken op aanvragen van specifieke systemen.
- Smart card-verificatie of Azure multi-factor Authentication.
- Bepalen via welke systemen iemand kan maken via de gateway.
- De controle over apparaten en schijf-omleiding.

## <a name="use-a-key-management-solution"></a>Gebruik een oplossing voor sleutelbeheer

Veilig sleutelbeheer is essentieel voor de bescherming van gegevens in de cloud. Met [Azure Key Vault](../key-vault/key-vault-whatis.md), kunt u veilig versleutelingssleutels opslaan en klein geheimen zoals wachtwoorden in hardware security modules (HSM's). Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's.

De processen Microsoft uw sleutels in FIPS 140-2 Level 2 gevalideerde HSM (hardware en firmware). Monitor en audit sleutelgebruik met logboekregistratie van Azure: pipe-Logboeken in Azure of uw Security Information en Event Management (SIEM) systeem voor extra analyse en detectie van bedreigingen.

Iedereen met een Azure-abonnement kunt maken en gebruiken van sleutelkluizen. Hoewel Sleutelkluis voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan deze worden geïmplementeerd en beheerd door een beheerder die verantwoordelijk is voor het beheren van Azure-services in een organisatie.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Virtuele schijven en schijfopslag versleutelen

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) adressen van de bedreiging van gegevensdiefstal of blootstelling tegen onbevoegde toegang, die wordt bereikt door het verplaatsen van een schijf. De schijf kan worden gekoppeld aan een ander systeem bij wijze van andere beveiligingsmechanismen omzeilen. Schijf wordt gebruikt voor versleuteling [BitLocker](https://technet.microsoft.com/library/hh831713) in Windows en DM-Crypt in Linux-besturingssysteem en gegevensstations versleutelen. Azure Disk Encryption kan worden geïntegreerd met Sleutelkluis en te controleren en beheren van de versleutelingssleutels. Is beschikbaar voor de standaard virtuele machines en virtuele machines met premium-opslag.

Zie voor meer informatie [Azure Disk Encryption in Windows en Linux IaaS VM's](azure-security-disk-encryption.md).

[Azure Storage-Service: versleuteling](../storage/common/storage-service-encryption.md) beschermt uw gegevens in rust. Het ingeschakeld op het niveau van de storage-account. Gegevens worden gecodeerd als ze de datacenters worden geschreven en wordt dit automatisch ontsleuteld wanneer u toegang hebt. Ondersteunt de volgende scenario's:

- Versleuteling van blok-blobs, toevoeg-blobs en pagina-blobs
- Versleuteling van gearchiveerde VHD's en sjablonen in Azure worden gebracht van on-premises
- Versleuteling van het onderliggende besturingssysteem en gegevensschijven voor IaaS VM's die u hebt gemaakt met behulp van uw VHD 's

Voordat u met Azure Storage Encryption doorgaat, worden op de hoogte van twee beperkingen:

- Het is niet beschikbaar op klassieke opslagaccounts.
- Alleen de gegevens die zijn geschreven nadat de codering is ingeschakeld, worden versleuteld.

## <a name="use-a-centralized-security-management-system"></a>Gebruik een beheersysteem voor gecentraliseerde beveiliging

Uw servers moeten worden gecontroleerd voor patchen, configuratie, gebeurtenissen en activiteiten die als beveiligingsproblemen worden beschouwd. Om deze problemen op te lossen, kunt u [Security Center](https://azure.microsoft.com/services/security-center/) en [Operations Management Suite-beveiliging en naleving](https://azure.microsoft.com/services/security-center/). Beide opties verder dan de configuratie in het besturingssysteem. Ze bieden ook bewaking van de configuratie van de onderliggende infrastructuur, zoals configuratie en het gebruik van virtueel apparaat.

## <a name="manage-operating-systems"></a>Besturingssystemen beheren

In een implementatie met IaaS bent u nog steeds verantwoordelijk voor het beheer van de systemen die u implementeert, net als een andere server of werkstation in uw omgeving. Patchen, te beperken, toegewezen rechten en eventuele andere activiteiten met betrekking tot het onderhoud van het systeem nog steeds zelf verantwoordelijk. Voor systemen die zijn geïntegreerd met uw lokale bronnen, wilt u mogelijk dezelfde hulpprogramma's en procedures die u hebt lokale gebruikt voor items zoals antivirus, anti-malware, patchen en back-up gebruiken.

### <a name="harden-systems"></a>Systemen beperken
Alle virtuele machines in Azure IaaS moet worden gehard zodat ze openbaren alleen service-eindpunten die vereist zijn voor de toepassingen die zijn geïnstalleerd. Voor Windows virtuele machines, volgt u de aanbevelingen die Microsoft publiceert als basislijnen voor de [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) oplossing.

Security Compliance Manager is een gratis hulpprogramma. U kunt deze gebruiken om snel te configureren en beheren van uw bureaubladen, traditionele datacenter en private en openbare cloud met behulp van Groepsbeleid en System Center Configuration Manager.

Security Compliance Manager biedt gereed te implementeren beleid en Desired Configuration Management-configuratiepakketten die zijn getest. Deze basislijnen zijn gebaseerd op [Microsoft-beveiligingsrichtlijnen](https://technet.microsoft.com/en-us/library/cc184906.aspx) aanbevelingen en andere aanbevolen procedures. Ze helpen u configuratie afwijking, adres nalevingsvereisten, beheren en beveiligingsrisico's verkleinen.

U kunt Security Compliance Manager de huidige configuratie van uw computers importeren met behulp van twee verschillende methoden gebruiken. Ten eerste kunt u Groepsbeleid op basis van Active Directory importeren. Ten tweede kunt u de configuratie van de 'gouden master' importeren referentiecomputer met behulp van de [LocalGPO hulpprogramma](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) back-up van het lokale Groepsbeleid. U kunt het lokale Groepsbeleid vervolgens importeren in Security Compliance Manager.

Vergelijk uw standaarden voor de beste praktijken, pas ze aan en nieuw beleid en Desired Configuration Management-configuratiepakketten maken. Basislijnen zijn gepubliceerd voor alle ondersteunde besturingssystemen, met inbegrip van Windows 10 Verjaardag en Update voor Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Installeren en beheren van antimalware

In omgevingen die afzonderlijk van uw productieomgeving worden gehost, kunt u een anti-malware-extensie voor het beveiligen van uw virtuele machines en cloudservices. Het is geïntegreerd met [Azure Security Center](../security-center/security-center-intro.md).


[Microsoft Antimalware](azure-security-antimalware.md) bevat functies zoals realtime-beveiliging, geplande scan, oplossen van malware, handtekening updates, engine-updates en rapportage, uitsluiting gebeurtenissen verzamelen, voorbeelden en [PowerShell-ondersteuning](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure Antimalware](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>Installeer de meest recente beveiligingsupdates
Sommige van de eerste werklasten die klanten naar Azure verplaatsen zijn labs en externe systemen. Als uw Azure gehoste virtuele machines host toepassingen of services die toegankelijk zijn met het Internet moeten zijn, worden bedacht patchen. Patch dan het besturingssysteem. Niet-gepatchte beveiligingsproblemen op toepassingen van derden kunnen ook leiden tot problemen die kunnen worden voorkomen als goed patch management aanwezig is.

### <a name="deploy-and-test-a-backup-solution"></a>Implementeren en testen van een back-upoplossing

Net als beveiligingsupdates moet een back-up dat u een andere bewerking afgehandeld dezelfde manier worden verwerkt. Dit geldt voor systemen die deel uitmaken van uw productieomgeving uitbreiden naar de cloud. Test-en Developer moeten volgen op back-strategieën die mogelijkheden voor terugzetten die vergelijkbaar zijn bieden met wat gebruikers gewend zijn, op basis van hun ervaringen met on-premises omgevingen.

Productie-workloads naar Azure verplaatst moeten worden geïntegreerd met bestaande back-upoplossingen indien mogelijk. Of u kunt [Azure Backup](../backup/backup-azure-arm-vms.md) bij het oplossen van de vereisten van uw back-up.


## <a name="monitor"></a>Bewaken

[Security Center](../security-center/security-center-intro.md) biedt continue evaluatie van de beveiligingsstatus van uw Azure-resources voor het identificeren van mogelijke beveiligingsproblemen. Een lijst met aanbevelingen begeleidt u bij het configureren van benodigde besturingselementen.

Voorbeelden zijn:

- Inrichting van antimalware om te identificeren en verwijderen van schadelijke software.
- Configureren van netwerkbeveiligingsgroepen en regels voor het verkeer voor beheer voor virtuele machines.
- Inrichting web application firewalls om te beschermen tegen aanvallen die zijn gericht op uw webtoepassingen.
- Ontbrekende systeemupdates implementeren.
- Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen.

De volgende afbeelding ziet u enkele van de opties die u in Security Center kunt inschakelen.

![Azure Security Center-beleid](./media/azure-security-iaas/security-center-policies.png)

[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) is een Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Omdat de Operations Management Suite is geïmplementeerd als een cloudservice, kan worden geïmplementeerd snel en met minimale investeringen in infrastructuurresources.

Nieuwe functies worden automatisch geleverd zodat u van voortdurend onderhoud en kosten upgrade. Operations Management Suite is ook worden geïntegreerd met System Center Operations Manager. Andere onderdelen kunt u beter beheer van uw Azure-werkbelastingen, waaronder een [beveiliging en naleving](../operations-management-suite/oms-security-getting-started.md) module.

U kunt functies van de beveiliging en naleving in Operations Management Suite gebruiken om informatie te bekijken over uw resources. Deze informatie is onderverdeeld in vier hoofdcategorieën:

- **Beveiligingsdomeinen**: verder verkennen beveiligingsrecord gedurende een bepaalde periode. Toegang tot de evaluatie van schadelijke software, beoordeling, netwerkgegevens voor beveiliging, informatie over identiteit en toegang en computers met beveiligingsgebeurtenissen bijwerken. Profiteren van snelle toegang tot het Azure Security Center-dashboard.
- **Problemen die aandacht vereisen**: het aantal actieve problemen en de ernst van deze problemen snel te identificeren.
- **Detecties (preview)**: aanval worden geïdentificeerd patronen door beveiligingswaarschuwingen visualiseren meteen op basis van uw resources.
- **Dreiging intelligence**: aanval worden geïdentificeerd patronen door het totale aantal servers met uitgaand schadelijk IP-verkeer, het type schadelijke threat en die laat waar deze IP-adressen afkomstig zijn zien van een-toewijzing te visualiseren.
- **Algemene query's voor beveiliging**: een overzicht van de meest voorkomende beveiliging query's die u gebruiken kunt voor het bewaken van uw omgeving. Wanneer u op een van de query's, de **Search** blade wordt geopend en toont de resultaten voor deze query.

De volgende Schermafbeelding toont een voorbeeld van de informatie die Operations Management Suite kunt weergeven.

![Operations Management Suite-beveiligingsbasislijnen](./media/azure-security-iaas/oms-security-baseline.png)

## <a name="next-steps"></a>Volgende stappen

* [Blog van het Azure-beveiligingsteam](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md)
