---
title: Veelgestelde vragen (FAQ) voor Azure Files | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure Files.
services: storage
author: RenaShahMSFT
ms.service: storage
ms.date: 10/04/2018
ms.author: renash
ms.component: files
ms.openlocfilehash: b3aca33bea8f7847b7069bba3f2a6dcd143cf29c
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079122"
---
# <a name="frequently-asked-questions-faq-about-azure-files"></a>Veelgestelde vragen (FAQ) over Azure Files
[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard [Server Message Block (SMB)-protocol](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx). U kunt Azure-bestandsshares gelijktijdig koppelen in de cloud of on-premises implementaties van Windows, Linux en macOS. U kunt ook Azure-bestandsshares op Windows Server-machines cache met behulp van Azure File Sync voor snelle toegang dicht bij waar de gegevens wordt gebruikt.

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Files-functies en functionaliteit, waaronder het gebruik van Azure File Sync met Azure Files. Als u het antwoord op uw vraag niet ziet, u kunt contact met ons opnemen via de volgende kanalen (in steeds sneller groeiende volgorde):

1. Het gedeelte met opmerkingen van dit artikel.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure-bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft Ondersteuning. Maken van een nieuwe ondersteuningsaanvraag in de Azure-portal op de **Help** tabblad de **Help en ondersteuning** knop en selecteer vervolgens **nieuwe ondersteuningsaanvraag**.

## <a name="general"></a>Algemeen
* <a id="why-files-useful"></a>
**Wat is Azure Files handig?**  
   U kunt Azure Files gebruiken om bestandsshares te maken in de cloud, zonder dat verantwoordelijk is voor het beheren van de overhead van een fysieke server, een apparaat of een apparaat. Wij doen het monotone werk voor u, met inbegrip van het toepassen van updates van het besturingssysteem en beschadigde schijven vervangen. Zie voor meer informatie over de scenario's voor Azure Files u met kunt [waarom Azure Files handig is](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>
**Wat zijn verschillende manieren toegang krijgen tot bestanden in Azure Files?**  
    U kunt de bestandsshare koppelen op uw lokale computer met behulp van het SMB 3.0-protocol of kunt u hulpprogramma's zoals [Opslagverkenner](http://storageexplorer.com/) toegang krijgen tot bestanden in de bestandsshare. Vanuit uw toepassing, kunt u clientbibliotheken, REST-API's, PowerShell of Azure CLI kunt gebruiken voor toegang tot uw bestanden in de Azure-bestandsshare.

* <a id="what-is-afs"></a>
**Wat is Azure File Sync?**  
    U kunt Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert uw Windows Server-machines naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, Network File System (NFS) en File Transfer Protocol-Service (FTPS) gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

* <a id="files-versus-blobs"></a>
**Waarom zou ik een Azure-bestandsshare ten opzichte van Azure Blob-opslag gebruiken voor mijn gegevens?**  
    Azure Files en Azure Blob storage bieden beide manieren om grote hoeveelheden gegevens opslaan in de cloud, maar ze zijn nuttig voor verschillende doeleinden. 
    
    Azure Blob-opslag is handig voor zeer grote schaal, cloud-eigen toepassingen die nodig hebt voor het opslaan van ongestructureerde gegevens. Azure Blob-opslag is voor maximale prestaties en schaalbaarheid, een eenvoudigere opslag abstractie dan een echt bestandssysteem. U kunt toegang krijgen tot de Azure Blob-opslag alleen via op REST-gebaseerde clientbibliotheken (of rechtstreeks via het protocol op basis van REST).

    Azure Files is specifiek voor een bestandssysteem. Azure Files heeft alle samenvattingen van bestand die u kent en waardeert van jarenlange samenwerking met on-premises-besturingssystemen. Azure Files biedt, zoals Azure Blob storage, een REST-interface en op basis van REST-clientbibliotheken. In tegenstelling tot Azure Blob-opslag biedt Azure Files SMB-toegang tot Azure-bestandsshares. U kunt een Azure-bestandsshare rechtstreeks in Windows, Linux of macOS, on-premises of in virtuele machines, cloud zonder code te schrijven of eventuele speciale stuurprogramma's te koppelen aan het bestandssysteem koppelen via SMB. U kunt ook Azure-bestandsshares op on-premises bestandsservers cache met behulp van Azure File Sync voor snelle toegang, dicht bij waar de gegevens wordt gebruikt. 
   
    Zie voor een gedetailleerdere beschrijving van de verschillen tussen Azure Files en Azure Blob-opslag, [beslissen wanneer u het gebruik van Azure Blob-opslag, Azure Files of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure Blob-opslag, [Inleiding tot Blob-opslag](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Waarom zou ik een Azure-bestandsshare in plaats van Azure-schijven gebruiken?**  
    Een diskette in Azure-schijven is gewoon een schijf. Als u de waarde van Azure-schijven, moet u een schijf koppelen aan een virtuele machine die wordt uitgevoerd in Azure. Azure-schijven kunnen worden gebruikt voor alles wat zou u een schijf voor op een on-premises server. U kunt deze gebruiken als een besturingssysteemschijf systeem, als wisselruimte voor een besturingssysteem, of als toegewezen opslag voor een toepassing. Een interessante gebruiken voor Azure-schijven is het maken van een bestandsserver in de cloud te gebruiken op de dezelfde plaatsen waar u een Azure-bestandsshare kunt. Implementeren van een bestandsserver in Azure Virtual Machines is een krachtige manier om opslag van bestanden in Azure wanneer u de implementatie-opties die op dit moment niet worden ondersteund door Azure-bestanden (zoals NFS-protocol ondersteunings- of premium-opslag) vereist. 

    Uitvoeren van een bestandsserver met Azure-schijven als opslag voor back-end doorgaans is echter veel duurder dan het gebruik van een Azure-bestandsshare, een aantal oorzaken hebben. Eerst, naast de facturatie voor schijfopslag, betaalt u ook moet voor de kosten van het uitvoeren van een of meer virtuele machines van Azure. Ten tweede u kan ook moet de virtuele machines die worden gebruikt voor het uitvoeren van de bestandsserver beheren. Bijvoorbeeld, bent u verantwoordelijk voor het besturingssysteem. Ten slotte, als u gegevens in de cache on-premises moet worden uiteindelijk vereist, het is aan u instellen en beheren van replicatietechnologieën, zoals Distributed File System Replication (DFSR), aan u dit kunt doen.

    Eén mogelijke benadering voor het ophalen van het beste van zowel Azure Files en een bestandsserver die in Azure Virtual Machines wordt gehost (naast het gebruik van Azure-schijven als opslag voor back-end) is het installeren van Azure File Sync op een bestandsserver die wordt gehost op een cloud-VM. Als de Azure-bestandsshare in dezelfde regio als de bestandsserver is, kunt u in de cloud tiering en instellen van het volume van het percentage vrije ruimte tot maximum (99%). Dit zorgt ervoor minimale duplicatie van gegevens. U kunt ook alle toepassingen die u met uw bestandsservers, wilt zoals ondersteuning voor toepassingen waarvoor NFS-protocol gebruiken.

    Zie voor meer informatie over een optie voor het instellen van een krachtige en maximaal beschikbare bestandsserver in Azure [implementeren IaaS VM-gastclusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Zie voor een gedetailleerdere beschrijving van de verschillen tussen Azure Files en Azure-schijven, [beslissen wanneer u het gebruik van Azure Blob-opslag, Azure Files of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure-schijven, [overzicht Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>
**Hoe ga ik aan de slag met Azure Files?**  
   Aan de slag met Azure Files is heel eenvoudig. Eerste, [een bestandsshare maken](storage-how-to-create-file-share.md), en koppel deze vervolgens in uw favoriete besturingssysteem: 

    * [Koppelen aan Windows](storage-how-to-use-files-windows.md)
    * [Koppelen in Linux](storage-how-to-use-files-linux.md)
    * [Koppelen in macOS](storage-how-to-use-files-mac.md)

   Zie voor een uitgebreidere handleiding over het implementeren van een Azure-bestandsshare voor het vervangen van productie-bestandsshares in uw organisatie, [Planning voor de implementatie van Azure Files](storage-files-planning.md).

* <a id="redundancy-options"></a>
**Welke opties voor opslagredundantie worden ondersteund door Azure Files?**  
    Azure Files ondersteunt momenteel lokaal redundante opslag (LRS), zone-redundante opslag (ZRS) en geografisch redundante opslag (GRS). Wij van plan bent te leestoegang geografisch redundante opslag met (RA-GRS) in de toekomst, maar er zijn geen tijdlijnen om te delen op dit moment.

* <a id="tier-options"></a>
**Welke opslaglagen worden ondersteund in Azure Files?**  
    Azure Files ondersteunt momenteel alleen de laag standard-opslag. Er zijn geen tijdlijnen voor premium storage en cool storage-ondersteuning op dit moment delen. 
    
    > [!NOTE]
    > U kunt geen Azure-bestandsshares maken van alleen-blob storage-accounts of van premium storage-accounts.

* <a id="give-us-feedback"></a>
**Ik wil om te zien van een specifieke functie die is toegevoegd aan Azure-bestanden. Kunt u deze toevoegen?**  
    Het team van Azure Files is geïnteresseerd in alle feedback hebt over onze service horen. Neem stemmen op de functieaanvragen bij een [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! We willen doorsturen naar u verwennen met veel nieuwe functies.

## <a name="azure-file-sync"></a>Azure File Sync

* <a id="afs-region-availability"></a>
**Welke regio's worden ondersteund voor Azure File Sync?**  
    De lijst met beschikbare regio's te vinden in de [beschikbaarheid in regio](storage-sync-files-planning.md#region-availability) sectie van de planning van de Azure File Sync begeleiden. Er wordt continu ondersteuning voor extra regio's, inclusief niet-openbare regio's toevoegen.

* <a id="cross-domain-sync"></a>
**Kan ik in de groep voor synchronisatie met hetzelfde domein en niet-domein-servers hebben?**  
    Ja. Een synchronisatiegroep mag servereindpunten waarvoor verschillende lidmaatschappen voor Active Directory, zelfs als ze niet domein zijn. Hoewel deze configuratie technisch werkt, niet aangeraden dit als een typische configuratie omdat toegangsbeheerlijsten (ACL's) die zijn gedefinieerd voor bestanden en mappen op één server mogelijk niet worden afgedwongen door andere servers in de groep voor synchronisatie. Voor optimale resultaten, wordt u aangeraden synchroniseren tussen de servers die zich in hetzelfde Active Directory-forest, tussen servers die zich in verschillende Active Directory-forests, maar die ingestelde vertrouwensrelaties, of tussen servers die zich niet in een domein. Het is raadzaam om te voorkomen dat met behulp van een combinatie van deze configuraties.

* <a id="afs-change-detection"></a>
**Kan ik een bestand rechtstreeks in mijn Azure-bestandsshare gemaakt met behulp van SMB of in de portal. Hoe lang duurt het om het bestand te synchroniseren met de servers in de groep voor synchronisatie?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Wat gebeurt er als hetzelfde bestand is gewijzigd op twee servers op ongeveer hetzelfde moment?**  
    Azure File Sync maakt gebruik van een strategie voor een eenvoudige conflictoplossing: we beide wijzigingen in bestanden die zijn gewijzigd op twee servers op hetzelfde moment behouden. De meest recent schriftelijke wijziging houdt u de naam van het oorspronkelijke bestand. Het oudere bestand heeft de 'bron'-machine en het aantal conflicten is toegevoegd aan de naam. Deze taxonomie volgt: 
   
    \<FileNameWithoutExtension\>-\<MachineName\>\[-#\].\< Ext\>  

    De eerste conflicterende CompanyReport.docx zou bijvoorbeeld CompanyReport CentralServer.docx worden als CentralServer is waar de oudere schrijven is opgetreden. De naam van het tweede conflict zou CompanyReport-CentralServer-1.docx.

* <a id="afs-storage-redundancy"></a>
**Geografisch redundante opslag is wordt ondersteund voor Azure File Sync?**  
    Ja, Azure Files ondersteunt zowel lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS). Als een GRS-failover tussen twee gekoppelde regio's is uitgevoerd, raden wij u aan de nieuwe regio te behandelen als een back-up van gegevens. Azure File Sync begint niet automatisch gesynchroniseerd met de nieuwe primaire regio. 

* <a id="sizeondisk-versus-size"></a>
**Waarom niet de *ruimte op de schijf* eigenschap voor een bestand overeen met de *grootte* eigenschap na het gebruik van Azure File Sync?**  
 Zie [inzicht in Cloud-Opslaglagen](storage-sync-cloud-tiering.md#sizeondisk-versus-size).

* <a id="is-my-file-tiered"></a>
**Hoe weet ik of een bestand is gelaagd?**  
 Zie [inzicht in Cloud-Opslaglagen](storage-sync-cloud-tiering.md#is-my-file-tiered).

* <a id="afs-recall-file"></a>**Een bestand dat ik wil gebruiken gelaagde. Hoe kan ik het bestand op schijf om lokaal gebruiken intrekken?**  
 Zie [inzicht in Cloud-Opslaglagen](storage-sync-cloud-tiering.md#afs-recall-file).


* <a id="afs-force-tiering"></a>
**Hoe Dwing ik een bestand of map in tiers worden verdeeld?**  
 Zie [inzicht in Cloud-Opslaglagen](storage-sync-cloud-tiering.md#afs-force-tiering).

* <a id="afs-effective-vfs"></a>
**Hoe wordt *vrije ruimte op volume* geïnterpreteerd wanneer ik meerdere servereindpunten op een volume heb?**  
 Zie [inzicht in Cloud-Opslaglagen](storage-sync-cloud-tiering.md#afs-effective-vfs).

* <a id="afs-files-excluded"></a>
**Welke bestanden of mappen worden automatisch uitgesloten door Azure File Sync?**  
    Azure File Sync niet standaard van toepassing op de volgende bestanden:
    * desktop.ini
    * Thumbs.DB
    * ehthumbs.DB
    * ~$\*.\*
    * \*.laccdb
    * \*.tmp
    * 635D02A9D91C401B97884B82B3BCDAEA.\*

    De volgende mappen worden standaard ook uitgesloten:

    * \System volume Information
    * \$PRULLENBAK. OPSLAGLOCATIE
    * \SyncShareState

* <a id="afs-os-support"></a>
**Kan ik Azure File Sync gebruiken met Windows Server 2008 R2, Linux of mijn apparaat network-attached storage (NAS)?**  
    Azure File Sync ondersteunt momenteel alleen Windows Server 2016 en Windows Server 2012 R2. Op dit moment is er geen andere plannen die we kunt delen, maar we zijn open op de ondersteuning van extra platforms op basis van vraag van klanten. Laat het ons weten op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) welke platforms u graag voor de ondersteuning.

* <a id="afs-tiered-files-out-of-endpoint"></a>
**Waarom bestaan er gelaagde bestanden buiten de server eindpunt-naamruimte?**  
    Azure File Sync geblokkeerd voordat u Azure File Sync-agentversie 3, de verplaatsing van gelaagde bestanden buiten het servereindpunt, maar op hetzelfde volume als het servereindpunt. Kopieerbewerkingen, verplaatst van niet-gelaagde bestanden en wordt verplaatst van gelaagde in andere volumes zijn niet beïnvloed. De reden voor dit gedrag is de impliciete veronderstelling dat bestand in Bestandenverkenner en andere Windows-API's hebben die bewerkingen op hetzelfde volume zijn (bijna) instanenous Wijzig de naam van bewerkingen worden verplaatst. Dit betekent verplaatst maakt Verkenner of andere verplaatsingsmethoden (zoals vanaf de opdrachtregel of PowerShell) wordt niet meer reageert geopend terwijl Azure File Sync de gegevens vanuit de cloud roept. Beginnen met [versie van Azure File Sync-agent 3.0.12.0](storage-files-release-notes.md#supported-versions), Azure File Sync kunt u een gelaagd bestand buiten het servereindpunt verplaatsen. We te voorkomen dat de negatieve effecten die eerder is vermeld dat het gelaagde bestand bestaat als een gelaagd bestand buiten het servereindpunt en vervolgens terughalen van het bestand op de achtergrond. Dit betekent dat verplaatst op hetzelfde volume zijn instaneous en we hebben al het werk om in te trekken van het bestand op schijf na de verplaatsing is voltooid. 

* <a id="afs-do-not-delete-server-endpoint"></a>
**Ik ondervind een probleem met Azure File Sync op mijn server (sync, cloud cloudlagen, enzovoort). Moet ik verwijderen en opnieuw maken van mijn servereindpunt?**  
    [!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]
    
* <a id="afs-resource-move"></a>
**Kan ik de opslagsynchronisatieservice en/of de storage-account verplaatsen naar een andere resourcegroep of abonnement?**  
   Ja, de opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.
    
* <a id="afs-ntfs-acls"></a>
**Behoudt Azure File Sync directory/bestandsniveau NTFS ACL's samen met de gegevens die zijn opgeslagen in Azure Files?**

    NTFS ACL's uitgevoerd vanuit on-premises bestand servers door Azure File Sync als metagegevens zijn opgeslagen. Azure Files biedt geen ondersteuning voor verificatie met Azure AD-referenties voor toegang tot bestandsshares die worden beheerd door de Azure File Sync-service.
    
## <a name="security-authentication-and-access-control"></a>Beveiliging, verificatie en toegangsbeheer
* <a id="ad-support"></a>
**Active Directory gebaseerde verificatie en toegangsbeheer die worden ondersteund door Azure Files is?**  
    
    Ja, op basis van identiteit verificatie en toegangsbeheer met Azure Active Directory (Azure AD) biedt ondersteuning voor Azure Files (Preview). Azure AD-verificatie via SMB voor Azure Files maakt gebruik van Azure Active Directory Domain Services om in te schakelen domein-VM's voor toegang tot shares, mappen en bestanden met behulp van Azure AD-referenties. Zie voor meer informatie, [overzicht van Azure Active Directory-verificatie voor Azure Files (Preview) via SMB](storage-files-active-directory-overview.md). 

    Azure Files biedt twee extra manieren voor het beheren van toegangsbeheer:

    - U kunt handtekeningen voor gedeelde toegang (SAS) gebruiken voor het genereren van tokens met specifieke machtigingen, en die geldig zijn voor een opgegeven tijdsinterval. Bijvoorbeeld, kunt u een token genereren met alleen-lezen toegang tot een specifiek bestand met een vervaldatum 10 minuten. Iedereen die het token beschikt terwijl het token geldig is heeft alleen-lezen toegang tot dat bestand voor de 10 minuten. Handtekening voor gedeelde toegangssleutels worden momenteel ondersteund alleen via de REST API of clientbibliotheken. U moet de Azure-bestandsshare koppelen via SMB met behulp van de sleutels voor het opslagaccount.

    - Azure File Sync blijven behouden en gerepliceerd van alle discretionaire ACL's, of DACL's (of op basis van Active Directory of lokaal) naar alle servereindpunten die deze wordt gesynchroniseerd met. Omdat Windows Server al met Active Directory verifiëren kan, Azure File Sync is een effectieve optie stop-hiaat tot en met volledige ondersteuning voor verificatie op basis van Active Directory en ACL-ondersteuning ontvangen.

* <a id="ad-support-regions"></a>
**Is de Preview-versie van Azure AD via SMB voor Azure Files beschikbaar in alle Azure-regio's?**

    De Preview-versie is beschikbaar in alle openbare regio's met uitzondering van: VS-West, VS Zuid-centraal, VS-midden, West-Europa, Noord-Europa.

* <a id="ad-support-on-premises"></a>
**Azure AD-verificatie via SMB voor Azure Files (Preview) biedt ondersteuning voor verificatie met behulp van Azure AD vanaf on-premises machines?**

    Nee, Azure Files ondersteunt geen verificatie met Azure AD vanaf on-premises machines in de preview-versie.

* <a id="ad-support-devices"></a>
**Wordt Azure AD-verificatie via SMB voor Azure Files (Preview) ondersteuning voor SMB-toegang met behulp van Azure AD-referenties van de apparaten lid zijn van of geregistreerd bij Azure AD?**

    Nee, dit scenario wordt niet ondersteund.

* <a id="ad-support-rest-apis"></a>
**Zijn er REST-API's ter ondersteuning van Get/Set/kopiëren map/bestand NTFS ACL's?**

    De preview-versie geen ondersteuning voor REST-API's voor het ophalen, instellen of NTFS ACL's voor mappen of bestanden kopiëren.

* <a id="ad-vm-subscription"></a>
**Kan ik toegang krijgen tot Azure Files met Azure AD-referenties van een virtuele machine met een ander abonnement?**

    Als het abonnement waarin de bestandsshare is geïmplementeerd, gekoppeld aan dezelfde Azure AD-tenant als de Azure AD Domain Services-deploymnet waarop de virtuele machine is het domein is, wordt u hebben vervolgens toegang tot Azure Files met dezelfde Azure AD-referenties. De beperking niet van het abonnement wordt opgelegd, maar op de bijbehorende Azure AD-tenant.    
    
* <a id="ad-support-subscription"></a>
**Kan ik inschakelen Azure AD-verificatie via SMB voor Azure Files met een Azure AD-tenant die verschilt van de primaire tenant die aan de bestandsshare is dat is gekoppeld aan?**

    Nee, Azure Files ondersteunt alleen Azure AD-integratie met een Azure AD-tenant die zich in hetzelfde abonnement als de bestandsshare bevinden. Slechts één abonnement kan worden gekoppeld aan een Azure AD-tenant.

* <a id="ad-linux-vms"></a>
**Azure AD-verificatie via SMB voor Azure Files (Preview) biedt ondersteuning voor Linux-VM's?**

    Nee, verificatie van virtuele Linux-machines wordt niet ondersteund in de preview-versie.

* <a id="ad-aad-smb-afs"></a>
**Kan ik Azure AD-verificatie gebruiken voor SMB-mogelijkheden op bestandsshares die worden beheerd door Azure File Sync?**

    Azure Files ondersteunt Nee, geen NTFS ACL's op bestandsshares die worden beheerd door Azure File Sync te behouden. Het bestand ACL's die worden uitgevoerd vanaf on-premises bestandsservers zijn opgeslagen door Azure File Sync. Een NTFS-ACL's standaard geconfigureerd voor de Azure-bestanden worden overschreven door de Azure File Sync-service. Azure Files biedt bovendien geen ondersteuning voor verificatie met Azure AD-referenties voor toegang tot bestandsshares die worden beheerd door de Azure File Sync-service.

* <a id="encryption-at-rest"></a>
**Hoe kan ik ervoor zorgen dat mijn Azure-bestandsshare in rust worden versleuteld?**  

    Azure Storage-Serviceversleuteling wordt momenteel wordt standaard ingeschakeld in alle regio's. Voor deze regio's moet u niet alle acties ondernemen voor het inschakelen van versleuteling. Zie voor andere regio's, [serverzijde versleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>
**Hoe kan ik toegang bieden tot een specifiek bestand via een webbrowser?**  

    U kunt handtekeningen voor gedeelde toegang gebruiken voor het genereren van tokens met specifieke machtigingen, en die geldig zijn voor een opgegeven tijdsinterval. U kunt bijvoorbeeld een token met alleen-lezen toegang tot een specifiek bestand, gedurende een bepaalde periode genereren. Iedereen die de URL heeft toegang tot het bestand rechtstreeks via elke webbrowser terwijl het token geldig is. U kunt eenvoudig een gedeelde toegangssleutel voor de handtekening van een gebruikersinterface zoals Opslagverkenner genereren.

* <a id="file-level-permissions"></a>
**Is het mogelijk om op te geven alleen-lezen of alleen-schrijven machtigingen voor mappen in de share?**  

    Als u de bestandsshare koppelt via SMB, hebt u geen mapniveau controle over machtigingen. Als u een shared access signature maken met behulp van de REST-API of clientbibliotheken, kunt u echter alleen-lezen of alleen-schrijven machtigingen opgeven voor mappen in de share.

* <a id="ip-restrictions"></a>
**Kan ik het IP-beperkingen voor een Azure-bestandsshare implementeren?**  

    Ja. Toegang tot uw Azure-bestandsshare kan worden beperkt op het niveau van de storage-account. Zie voor meer informatie, [configureren van Azure Storage-Firewalls en virtuele netwerken](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>
**Welke gegevens nalevingsbeleid biedt ondersteuning voor Azure Files?**  

   Azure Files wordt uitgevoerd boven op de dezelfde opslagarchitectuur die wordt gebruikt in andere storage-services in Azure Storage. Azure Files is van toepassing de dezelfde nalevingsbeleid voor gegevens die worden gebruikt in andere Azure storage-services. Voor meer informatie over de naleving van Azure Storage-gegevens, kunt u verwijzen naar [compliance-aanbiedingen voor Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-compliance-offerings), en Ga naar de [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx).

## <a name="on-premises-access"></a>Toegang tot on-premises
* <a id="expressroute-not-required"></a>
**Heb ik Azure ExpressRoute gebruiken voor het verbinding maken met Azure Files of voor het gebruik van Azure File Sync on-premises?**  

    Nee. ExpressRoute is niet vereist voor toegang tot een Azure-bestandsshare. Als u wilt koppelen wordt een Azure-bestandsshare rechtstreeks on-premises alle die vereist is dat poort 445 (TCP uitgaand) openen voor toegang tot internet (dit is de poort die SMB gebruikt om te communiceren). Als u Azure File Sync, dat u nodig hebt, is poort 443 (TCP uitgaand) voor HTTPS-toegang (geen SMB vereist). Echter, u *kunt* ExpressRoute gebruiken met een van deze toegangsopties.

* <a id="mount-locally"></a>
**Hoe kan ik een Azure-bestandsshare koppelen op mijn lokale computer?**  

    U kunt de bestandsshare koppelt via het SMB-protocol als poort 445 (TCP uitgaand) geopend is en de client het SMB 3.0-protocol ondersteunt (bijvoorbeeld, als u Windows 10 of Windows Server 2016). Als poort 445 wordt geblokkeerd door het beleid van uw organisatie of door uw Internetprovider, kunt u Azure File Sync kunt gebruiken voor toegang tot uw Azure-bestandsshare.

## <a name="backup"></a>Backup
* <a id="backup-share"></a>
**Hoe kan ik maak een back-up van mijn Azure-bestand delen?**  
    U kunt periodieke [momentopnamen van bestandsshares](storage-snapshots-files.md) voor bescherming tegen onbedoeld verwijderen. Ook kunt u AzCopy, Robocopy of een back-uphulpprogramma van derden die back-up van een gekoppelde bestandsshare. Azure Backup biedt back-up van Azure Files. Meer informatie over [maakt u een back-up van Azure-bestandsshares door Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-files).

## <a name="share-snapshots"></a>Momentopnamen van shares

### <a name="share-snapshots-general"></a>Momentopnamen van bestandsshares: Algemeen
* <a id="what-are-snaphots"></a>
**Wat zijn de momentopnamen van bestandsshares?**  
    U kunt momentopnamen van Azure-bestandsshares gebruiken om te maken van een alleen-lezen versie van uw bestandsshares. U kunt ook Azure Files gebruiken om te kopiëren van een eerdere versie van de back-inhoud naar dezelfde share, naar een alternatieve locatie in Azure of on-premises voor meer wijzigingen. Zie voor meer informatie over momentopnamen van shares, de [overzicht van de momentopname delen](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>
**Waar worden mijn momentopnamen van shares opgeslagen?**  
    Momentopnamen van bestandsshares worden opgeslagen in hetzelfde opslagaccount als de bestandsshare.

* <a id="snapshot-perf-impact"></a>
**Zijn er geen gevolgen voor prestaties voor het gebruik van momentopnamen van shares?**  
    Momentopnamen van shares beschikt niet over een prestatieoverhead.

* <a id="snapshot-consistency"></a>
**Momentopnamen van shares toepassingsconsistent zijn?**  
    Nee, zijn er geen momentopnamen van shares toepassingsconsistente. De gebruiker moet de schrijfbewerkingen leegmaken van de toepassing naar de share voordat u een momentopname van de share.

* <a id="snapshot-limits"></a>
**Zijn er beperkingen met betrekking tot het aantal momentopnamen van bestandsshares die kan ik gebruiken?**  
    Ja. Azure Files kunnen maximaal 200 momentopnamen bewaren. Momentopnamen van shares meetellen niet voor het quotum voor de bestandsshare, zodat er geen limiet per share voor de totale ruimte die wordt gebruikt door alle momentopnamen van shares. Opslagaccountlimieten nog steeds van toepassing. Na 200 momentopnamen, moet u oudere momentopnamen voor het maken van nieuwe momentopnamen van shares verwijderen.

* <a id="snapshot-cost"></a>
**Hoeveel u momentopnamen kosten mag delen?**  
    Standard transactie- en standard-opslagkosten wordt toegepast op de momentopname. Momentopnamen van zijn incrementele van aard. De base momentopname is de bestandsshare zelf. De volgende momentopnamen worden incrementele en worden alleen de verschillen van de vorige momentopname opgeslagen. Dit betekent dat de wijzigingen die in de factuur is zichtbaar minimale als uw werkbelastingsverloop minimaal is. Zie [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/files/) voor standaard Azure-bestanden die informatie over prijzen. Vandaag de manier om te kijken naar de grootte die worden gebruikt door een momentopname van bestandsshare is door het vergelijken van de capaciteit worden gefactureerd met capaciteit gebruikt. We werken op hulpprogramma's voor het verbeteren van de rapportage.

* <a id="ntfs-acls-snaphsots"></a>
**NTFS ACL's op mappen en bestanden persistent in momentopnamen van shares?**
    NTFS ACL's op mappen en bestanden zijn opgeslagen in de momentopnamen van shares.

### <a name="create-share-snapshots"></a>Momentopnamen van shares maken
* <a id="file-snaphsots"></a>
**Kan ik een share-momentopname van afzonderlijke bestanden maken?**  
    Momentopnamen van shares worden gemaakt op het niveau van delen. Kunt u afzonderlijke bestanden herstellen vanuit de momentopname van bestandsshare, maar u kunt geen momentopnamen van shares op bestandsniveau maken. Echter, als u een momentopname van een share-niveau share hebt gemaakt en u wilt weergeven van momentopnamen waarbij een specifiek bestand is gewijzigd, u kunt dit doen onder **vorige versies** op een share Windows gekoppeld. 
    
    Als u een functie van de momentopname van bestand nodig hebt, laat het ons weten op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>
**Kan ik momentopnamen van shares van een versleutelde-bestandsshare maken?**  
    U kunt een momentopname van een share van de Azure-bestandsshares waarvoor versleuteling ' at rest ' ingeschakeld op te nemen. U kunt bestanden vanuit een share-momentopname herstellen naar een versleutelde bestandsshare. Als uw bestandsshare is versleuteld, worden een momentopname van de share ook versleuteld.

* <a id="geo-redundant-snaphsots"></a>
**Zijn mijn momentopnamen van shares geografisch redundante?**  
    Momentopnamen van shares hebben de dezelfde redundantie als de Azure-bestandsshare waarvoor ze zijn genomen. Als u geografisch redundante opslag voor uw account hebt geselecteerd, wordt uw ook een momentopname van bestandsshare toch toe opgeslagen in de gekoppelde regio.

### <a name="manage-share-snapshots"></a>Momentopnamen van shares beheren
* <a id="browse-snapshots-linux"></a>
**Kan ik mijn momentopnamen in Linux bladeren?**  
    U kunt Azure CLI gebruiken om te maken, weergeven, bladeren en terugzetten van momentopnamen in Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>
**Kan ik de momentopnamen van shares kopiëren naar een ander opslagaccount?**  
    U kunt bestanden van momentopnamen van shares kopiëren naar een andere locatie, maar u kunt de momentopnamen van shares zelf niet kopiëren.

### <a name="restore-data-from-share-snapshots"></a>Gegevens terugzetten van momentopnamen van shares
* <a id="promote-share-snapshot"></a>
**Kan ik bevordering van de momentopname van een share naar de basis-share?**  
    U kunt gegevens kopiëren vanuit een share-momentopname naar een andere bestemming. U kunt een momentopname van een share naar de basis-share kan niet promoveren.

* <a id="restore-snapshotted-file-to-other-share"></a>
**Kan ik gegevens herstellen vanuit Mijn share-momentopname naar een ander opslagaccount?**  
    Ja. Bestanden vanuit een share-momentopname kunnen worden gekopieerd naar de oorspronkelijke locatie of naar een alternatieve locatie die hetzelfde opslagaccount of een ander opslagaccount in dezelfde regio of in verschillende regio's bevat. U kunt ook bestanden kopiëren naar een on-premises locatie of naar een andere cloud.    
  
### <a name="clean-up-share-snapshots"></a>Opschonen van de momentopnamen van shares
* <a id="delete-share-keep-snapshots"></a>
**Kan ik mijn share verwijderen, maar niet mijn momentopnamen van shares verwijderen?**  
    Als u actieve momentopnamen op de share hebt, kunt u uw bestandsshare niet verwijderen. Een API kunt u momentopnamen van shares, samen met de share verwijderen. Ook kunt u zowel de share in de Azure-portal als de momentopnamen van shares verwijderen.

* <a id="delete-share-with-snapshots"></a>
**Wat gebeurt er met mijn momentopnamen van shares als ik mijn storage-account verwijderen?**  
    Als u uw storage-account verwijdert, worden de share-momentopnamen ook verwijderd.

## <a name="billing-and-pricing"></a>Facturering en prijzen
* <a id="vm-file-share-network-traffic"></a>
**Telt het netwerkverkeer tussen een Azure-VM en een Azure bestandsshare mee als externe bandbreedte die wordt verrekend met het abonnement?**  
    Als de bestandsshare en de VM zich in dezelfde Azure-regio, is er geen extra kosten voor het verkeer tussen de bestandsshare en de virtuele machine. Als de bestandsshare en de virtuele machine zich in verschillende regio's, het verkeer tussen hen in rekening worden gebracht als externe bandbreedte.

* <a id="share-snapshot-price"></a>
**Hoeveel u momentopnamen kosten mag delen?**  
     Tijdens de preview is er geen kosten in rekening gebracht voor de share snapshot-capaciteit. Standaard uitgaand verkeer en transactie opslagkosten van toepassing. Na algemene beschikbaarheid, wordt abonnementen gefactureerd voor capaciteit en transacties op momentopnamen van shares.
     
     Momentopnamen van shares zijn incrementele van aard. De basis-share-momentopname is de share zelf. Alle volgende momentopnamen worden incrementele en op te slaan alleen het verschil van de vorige momentopname. U wordt gefactureerd voor de gewijzigde inhoud. Als u een share met 100 GiB gegevens hebt, maar alleen 5 GiB is gewijzigd sinds uw laatste share-momentopname, de share-momentopname verbruikt slechts 5 extra GiB en worden in rekening gebracht voor 105 GiB. Zie voor meer informatie over transactie en de kosten voor standaard uitgaand verkeer, de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Schaal en prestaties
* <a id="files-scale-limits"></a>
**Wat zijn de schaallimieten van Azure Files?**  
    Zie voor meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Files [schaalbaarheids- en prestatiedoelen van Azure Files](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>
**Ik heb nodig een grotere bestandsshare dan Azure Files op dit moment biedt. Kan ik de grootte van mijn Azure-bestandsshare vergroten?**  
    Nee. De maximale grootte van een Azure-bestandsshare is 5 TiB. Dit is momenteel een vaste limiet die we niet aanpassen. Er wordt gewerkt aan een oplossing voor het verhogen van de grootte van de bestandsshare in 100 TiB, maar er zijn geen tijdlijnen om te delen op dit moment.

* <a id="open-handles-quota"></a>
**Hoeveel clients kunnen tegelijkertijd toegang krijgen tot hetzelfde bestand?**   
    Er is een quotum van 2000 open ingangen in één bestand. Wanneer u 2000 open ingangen hebt, wordt een foutbericht weergegeven met de tekst dat is bereikt.

* <a id="zip-slow-performance"></a>
**Mijn prestaties zijn traag wanneer ik bij het uitpakken van bestanden in Azure Files. Wat moet ik doen?**  
    Voor grote aantallen bestanden overbrengen naar Azure Files, raden wij aan dat u AzCopy (voor Windows, in Preview-versie voor Linux en UNIX) of Azure PowerShell. Deze hulpprogramma's zijn geoptimaliseerd voor netwerkoverdracht.

* <a id="slow-perf-windows-81-2012r2"></a>
**Waarom is mijn trage prestaties nadat ik mijn Azure-bestandsshare in Windows Server 2012 R2 of koppelen Windows 8.1?**  
    Er is een bekend probleem tijdens het koppelen van een Azure-bestandsshare op Windows Server 2012 R2 en Windows 8.1. Het probleem is gevuld in de cumulatieve update van April 2014 voor Windows 8.1 en Windows Server 2012 R2. Zorg ervoor dat alle exemplaren van Windows Server 2012 R2 en Windows 8.1 deze patch toegepast hebben voor optimale prestaties. (U moet altijd ontvangen de Windows-patches via Windows Update.) Zie voor meer informatie, de bijbehorende Microsoft Knowledge Base-artikel [trage prestaties wanneer u Azure Files vanuit Windows 8.1 of Server 2012 R2 opent](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Functies en -interoperabiliteit met andere services
* <a id="cluster-witness"></a>
**Kan ik mijn Azure-bestandsshare als gebruiken een *bestandsshare-Witness* voor mijn Windows Server Failover Cluster?**  
    Deze configuratie wordt momenteel niet ondersteund voor een Azure-bestandsshare. Zie voor meer informatie over hoe u deze optie instellen voor Azure Blob-opslag, [een Cloudwitness implementeren voor een failovercluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>
**Kan ik een Azure-bestandsshare koppelen op een Azure Container exemplaar?**  
    Ja, Azure-bestandsshares zijn een goede optie wanneer u wilt behouden van gegevens buiten de levensduur van een containerexemplaar. Zie voor meer informatie, [een Azure-bestandsshare koppelen met Azure Container instances](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>
**Is er een naamswijziging in de REST-API?**  
    Momenteel niet.

* <a id="nested-shares"></a>
**Kan ik geneste shares instellen? Met andere woorden, een share onder een share?**  
    Nee. De bestandsshare *is* het virtuele stuurprogramma dat u koppelen kunt, zodat de geneste shares worden niet ondersteund.

* <a id="ibm-mq"></a>
**Hoe gebruik ik Azure Files met IBM MQ?**  
    IBM heeft een document waarmee IBM MQ-klanten kunnen Azure Files configureren met de IBM-service worden uitgegeven. Zie voor meer informatie, [over het instellen van een IBM MQ-Wachtrijbeheer meerdere exemplaren met Microsoft Azure Files-service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Zie ook
* [Problemen met Azure Files in Windows oplossen](storage-troubleshoot-windows-file-connection-problems.md)
* [Problemen oplossen met Azure Files in Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure Files Sync oplossen](storage-sync-files-troubleshoot.md)
