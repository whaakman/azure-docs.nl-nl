---
title: Veelgestelde vragen over Azure-bestanden | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure-bestanden.
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/13/2017
ms.author: renash
ms.openlocfilehash: 91c46ea0897f83811cfa5c1a8b67677caff14569
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Veelgestelde vragen over Azure-bestanden
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via de industriestandaard [protocol Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (ook wel Common Internet File System of CIFS). Azure-bestandsshares kunnen gelijktijdig worden gekoppeld door on-premises of cloudimplementaties van Windows, Linux en macOS. Bovendien Azure-bestandsshares kunnen worden opgeslagen in de cache op Windows-Servers met het synchroniseren van Azure-bestand (Preview) voor snelle toegang in de buurt van waar de gegevens wordt gebruikt.

Dit artikel wordt uitgelegd dat sommige van de veelgestelde vragen over Azure-bestanden functies en functionaliteit, waaronder Azure File-synchronisatie. Als u het antwoord op uw vraag hier niet ziet, altijd bereiken ons via de volgende kanalen (in volgorde escaleren):

1. In de sectie met opmerkingen van dit artikel.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Voor het maken van een nieuwe ondersteuningsaanvraag gaat u naar 'Help + ondersteuning' tabblad in de Azure portal en klik op 'Nieuw ondersteuningsverzoek'.

## <a name="general"></a>Algemeen
* <a id="why-files-useful"></a>**Waarom is de Azure-bestanden nuttig?**  
   Azure Files kunt u bestandsshares in de cloud maken zonder de overhead van een fysieke server of het apparaat/apparaat beheren. Dit betekent dat u minder tijd OS-updates worden toegepast en vervangen beschadigde schijven kunt besteden - we al die monotone werk voor u doen. Zie voor meer informatie over de scenario's met Azure-bestanden kan helpen, [waarom Azure Files is nuttig](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>**Wat zijn verschillende manieren toegang krijgen tot bestanden in de Azure-bestanden?**  
    U kunt de bestandsshare koppelen op uw lokale machine met behulp van SMB 3.0-protocol of gebruik hulpprogramma's zoals [Opslagverkenner](http://storageexplorer.com/) toegang krijgen tot bestanden in de bestandsshare. Van uw toepassing, kunt u opslagclientbibliotheken, REST-API's, PowerShell of CLI om toegang tot uw bestanden in Azure-bestandsshare.

* <a id="what-is-afs"></a>**Wat is Azure bestand synchronisatie?**  
    Azure File-synchronisatie kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder geeft de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

* <a id="files-versus-blobs"></a>**Waarom zou ik een Azure-bestandsshare versus Azure Blob storage gebruiken voor mijn gegevens?**  
    Azure Files en Azure Blob storage zowel bieden een manier voor het opslaan van grote hoeveelheden gegevens in de cloud, maar zijn handig voor verschillende doeleinden. Azure Blob-opslag is nuttig voor grootschalige en cloud-systeemeigen toepassingen die nodig zijn voor het opslaan van ongestructureerde gegevens. Azure Blob-opslag is om te maximaliseren prestaties en schaalbaarheid, een eenvoudigere opslag abstractie dan een waar bestandssysteem. Azure Blob storage kan bovendien alleen worden benaderd via REST gebaseerde clientbibliotheken (of rechtstreeks via het protocol op basis van REST).

    Azure Files aan de andere kant specifiek zoekt moet een bestandssysteem, met alle van de bestand samenvattingen u bekend zijn en veel van jaren van lokale besturingssystemen. Azure Files biedt een REST-interface en REST gebaseerde clientbibliotheken zoals Azure Blob-opslag, maar in tegenstelling tot Azure Blob storage, biedt Azure-bestanden ook SMB-toegang tot Azure-bestandsshares. Dit betekent dat u kunt rechtstreeks koppelen een Azure-bestandsshare op Windows, Linux of Mac OS, on-premises of in de cloud VM's, zonder een code te schrijven of eventuele speciale stuurprogramma's toevoegen aan het bestandssysteem. Bovendien Azure-bestandsshares kunnen worden opgeslagen in de cache op lokale bestandsservers met behulp van Azure-Sync-bestand voor snelle toegang in de buurt van waar de gegevens wordt gebruikt. 
   
    Zie voor een diepgaandere bespreking van de verschillen tussen Azure-bestanden en Azure Blob-opslag, [beslist het gebruik van Azure Blob storage, Azure-bestanden of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure Blob storage, [Inleiding tot Blob storage](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Waarom zou ik een Azure-bestandsshare ten opzichte van de Azure-schijven gebruiken?**  
    Een Azure-schijf is zojuist die een schijf. De schijf van een zelfstandige zelfstandig gebruikt, is niet erg nuttig - waarde buiten een Azure-schijf op te halen, moet u koppelen aan virtuele machine in Azure wordt uitgevoerd. Azure-schijven kunnen worden gebruikt voor alles en alles zou u een schijf voor op een on-premises server: als een besturingssysteemschijf system wisselruimte voor een besturingssysteem, of als toegewezen opslag voor een toepassing. Een interessante gebruiken voor Azure-schijven is het maken van een bestandsserver in de cloud voor gebruik in exact dezelfde u plaatst mogelijk gebruik van een Azure-bestandsshare. Implementeren van een bestandsserver in Azure VM's is een fantastische en krachtige manier om opslag van bestanden in Azure wanneer u de implementatie-opties die momenteel niet ondersteund door Azure-bestanden (zoals NFS-protocol ondersteuning of premium-opslag) vereist. 

    Aan de andere kant wordt met een bestandsserver met Azure-schijven als back-end opslag doorgaans veel duurder dan het gebruik van een Azure-bestandsshare om verschillende redenen. Eerst naast betaalt voor schijfopslag, moet u ook betalen voor de kosten van het uitvoeren van een of meer virtuele Azure-machines. Ten tweede, moet u ook de virtuele machines gebruikt voor het uitvoeren van de bestandsserver, zoals die verantwoordelijk is voor upgrades voor het besturingssysteem, enzovoort beheren. Ten slotte, als u uiteindelijk vereist dat gegevens in de lokale cache, het is aan u instellen en beheren van replicatietechnologieën (zoals Distributed File System Replication) zelf doen.

    Één interessante benadering van het beste op zowel de Azure-bestanden en de bestandsserver gehost in Azure VM's met Azure-schijven als back-end-opslag, is het installeren van Azure bestand synchronisatie op de cloud VM gehost bestandsserver. Als de Azure-bestandsshare in dezelfde regio bevinden als de bestandsserver is, kunt u cloud tiering en volume vrije ruimte percentage ingesteld op maximum (99%). Dit zorgt ervoor minimale dubbele gegevens terwijl u met de gewenste toepassingen op uw bestandsservers dergelijke iets vereisen van NFS-protocol ondersteunen.

    Zie voor informatie over een manier voor het instellen van een hoge prestaties en een maximaal beschikbare bestandsserver in Azure, [implementeren IaaS VM-Gastclusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Zie voor een uitgebreidere bespreking van de verschillen tussen Azure-bestanden en de Azure-schijven, [beslist het gebruik van Azure Blob storage, Azure-bestanden of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure-schijven, [overzicht van Azure schijven beheerd](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Hoe ga ik aan de slag met Azure-bestanden?**  
    Aan de slag met Azure Files is eenvoudig: gewoon [een bestandsshare maken](storage-how-to-create-file-share.md) en koppel in uw favoriete besturingssysteem: 

    - [Koppelen aan Windows](storage-how-to-use-files-windows.md)
    - [Koppel op Linux](storage-how-to-use-files-linux.md)
    - [Op Mac OS koppelen](storage-how-to-use-files-mac.md)

    Zie voor een uitgebreidere handleiding voor het implementeren van een Azure-bestandsshare ter vervanging van productie-bestandsshares in uw organisatie, [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md).

* <a id="redundancy-options"></a>**Welke opties voor opslag redundantie worden ondersteund door Azure Files?**  
    Azure Files ondersteunt momenteel alleen lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS) nu. We wilt ondersteunen zone-redundante opslag (ZRS) en geografisch redundante opslag met leestoegang (RA-GRS) in de toekomst, maar geen tijdlijnen te delen op dit moment.

* <a id="tier-options"></a>**Welke opslaglagen worden momenteel ondersteund door Azure Files?**  
    Azure Files ondersteunt momenteel alleen de laag standard-opslag. Er momenteel geen tijdlijnen delen voor premium- en cool ondersteuning. Houd er rekening mee dat u geen Azure-bestandsshares van alleen-Blob storage-accounts of van Premium storage-accounts maken.

* <a id="give-us-feedback"></a>**Ik wil graag Zie *x* functie toegevoegd aan Azure-bestanden, kunt u deze toevoegen?**  
    Absoluut, is het team van Azure Files geïnteresseerd in het horen alle feedback hebt over onze service. Neem stemmen op functieaanvragen op de [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! Denk eens doorsturen naar u verwennen met veel nieuwe functies.

## <a name="azure-file-sync"></a>Azure File-synchronisatie
* <a id="afs-region-availability"></a>**Welke regio's worden momenteel ondersteund voor Azure File-synchronisatie (preview)?**  
    Azure File-synchronisatie is momenteel beschikbaar in VS-West, West-Europa, Australië-Oost en Zuidoost-Azië. Er wordt ondersteuning toevoegen voor extra gebieden zoals we aan de algemene beschikbaarheid werken. Zie voor meer informatie, [beschikbaarheid in regio's](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**Kan ik verbonden met het domein en niet van het domein gekoppelde servers in de groep voor synchronisatie met dezelfde hebben?**  
    Ja, kan een groep voor synchronisatie met de Server-eindpunten die verschillende Active Directory-lidmaatschap hebben, inclusief wordt niet verbonden met het domein bevatten. Terwijl configuration technisch werkt, raadzaam niet deze als een normale configuratie ACL's die zijn gedefinieerd voor bestanden/mappen op één server mogelijk niet worden afgedwongen door andere servers in de groep voor synchronisatie. U wordt aangeraden synchroniseren tussen beide servers in de dezelfde Active Directory-forest, -servers in verschillende Active Directory-forests met ingestelde vertrouwensrelaties of -servers die niet in een domein, maar niet een combinatie van alle bovenstaande voor de beste resultaten.

* <a id="afs-change-detection"></a>**Ik heb een bestand rechtstreeks in mijn Azure-bestandsshare via SMB of via de portal gemaakt. Hoe lang duurt het tot het bestand is gesynchroniseerd met de servers in de groep voor synchronisatie?** 
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Wat gebeurt er wanneer hetzelfde bestand is gewijzigd op twee servers tegelijkertijd ongeveer?**  
    Een strategie voor een eenvoudige conflict oplossing maakt gebruik van Azure File-synchronisatie: we beide wijzigingen behouden. De meest recent geschreven houdt de oorspronkelijke bestandsnaam. De oudere bestand heeft de bronmachine en het aantal conflicten is toegevoegd aan de naam met deze taxonomie: 
   
    `<FileNameWithoutExtension>-<MachineName>[-#].<ext>`  

    Bijvoorbeeld de eerste conflicterende `CompanyReport.docx` zou `CompanyReport-CentralServer.docx` als `CentralServer` is waar het oudere schrijven zich heeft voorgedaan. Het tweede conflict zou worden geïdentificeerd als `CompanyReport-CentralServer-1.docx`.

* <a id="afs-storage-redundancy"></a>**Is geografisch redundante opslag (GRS) voor Azure bestand synchronisatie ondersteund?**  
    Ja, zowel lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS) worden ondersteund door Azure File-synchronisatie. U wordt aangeraden de nieuwe regio behandelen als een back-up van gegevens alleen in het geval van een failover GRS tussen gekoppelde regio's. Azure File-synchronisatie wordt niet automatisch wordt gestart met de nieuwe primaire regio worden gesynchroniseerd. 

* <a id="sizeondisk-versus-size"></a>**Waarom niet de *ruimte op de schijf* eigenschap voor een bestand overeen met de *grootte* eigenschap na gebruik van Azure bestand synchronisatie?**  
    Windows Verkenner beschrijft de twee eigenschappen **grootte** en **ruimte op de schijf** vertegenwoordigt de grootte van een bestand. Deze eigenschappen verschillen enigszins in de zin; **Grootte** vertegenwoordigt de volledige grootte van het bestand, terwijl **ruimte op de schijf** vertegenwoordigt de grootte van de bestandsstroom daadwerkelijk worden opgeslagen op schijf. Deze kunnen afwijken voor een aantal redenen, zoals compressie, gebruik van Gegevensontdubbeling of in ons geval, cloud tiering met het synchroniseren van Azure-bestand. Als een bestand is gelaagd naar een Azure-bestandsshare, de grootte op schijf worden nul omdat de bestandsstroom is niet in uw Azure-bestandsshare opgeslagen op schijf. Het is ook mogelijk om een bestand te worden gedeeltelijk gelaagde (of gedeeltelijk ingetrokken), wat betekent dat deel van het bestand is op schijf. Dit kan gebeuren wanneer bestanden gedeeltelijk door toepassingen zoals multimedia spelers of hulpprogramma's voor het comprimeren gelezen worden. 

* <a id="is-my-file-tiered"></a>**Hoe kan ik zien als een bestand is gelaagd?**  
    Er zijn verschillende manieren om te controleren als een bestand naar uw Azure-bestand is gelaagde delen:
    
    1. **Controleer de bestandskenmerken op het bestand.** U doet dit door met de rechtermuisknop op een bestand, gaat u naar **Details** en schuif omlaag naar de **kenmerken** eigenschap. Een gelaagde bestand heeft de volgende kenmerken instellen:     
        
        | Kenmerk letter | Kenmerk | Definitie |
        |:----------------:|-----------|------------|
        | A | Archiveren | Hiermee wordt aangegeven dat het bestand moet een back-up door back-upsoftware. Dit kenmerk is altijd ingesteld ongeacht of het bestand is gelaagd of volledig opgeslagen op schijf. |
        | P | Verspreid bestand | Geeft aan dat het bestand een sparse-bestand een speciaal type bestand die NTFS biedt op efficiënte wijze gebruiken is als van het bestand op schijf stroom voornamelijk leeg is. Azure File-synchronisatie maakt gebruik van verspreide bestanden omdat een bestand is niet volledig lagen, wat betekent dat de bestandsstroom alleen wordt opgeslagen in de cloud of gedeeltelijk ingetrokken, dit houdt die deel van het bestand is al op de schijf. Als een bestand volledig is ingetrokken op schijf Azure File-synchronisatie wordt converteren van een sparse-bestand naar een reguliere-bestand. |
        | L | Reparsepunt | Geeft aan dat het bestand een reparsepunt een speciale aanwijzer voor gebruik door een bestandssysteemfilter is heeft. Azure File-synchronisatie reparsepunten gebruikt om met het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys) te definiëren waar het bestand wordt opgeslagen in de cloud. Hierdoor kan naadloze toegang zonder uw eindgebruikers zelfs hoeven weten dat Azure File-synchronisatie wordt gebruikt of hoe ze toegang krijgen tot het bestand in uw Azure-bestandsshare. Wanneer een bestand volledig worden teruggehaald, verwijdert Azure bestand Sync het reparsepunt uit het bestand. |
        | O | Off line | Hiermee wordt aangegeven dat bepaalde of alle inhoud van het bestand niet is opgeslagen op schijf. Wanneer een bestand volledig worden teruggehaald, verwijdert Azure bestand Sync dit kenmerk. |

        ![Het dialoogvenster Eigenschappen voor een bestand met het tabblad met Details geselecteerd](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Het is ook mogelijk om te zien van de kenmerken voor alle bestanden in een map door toe te voegen de **kenmerken** veld bij het weergeven van de tabel van de Verkenner-bestand. U doet dit door met de rechtermuisknop op een bestaande kolom (bijvoorbeeld grootte), selecteer **meer** en selecteer **kenmerken** uit de vervolgkeuzelijst.
        
    2. **Gebruik `fsutil` om te controleren op Reparse-punten op een bestand.** Zoals vermeld in de vorige optie, hebben een gelaagde bestand altijd een reparsepunt of een speciale verwijzing voor het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys) ingesteld. U kunt controleren of een bestand een reparsepunt heeft met de `fsutil` hulpprogramma op een opdrachtprompt met verhoogde bevoegdheid of PowerShell-sessie:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Als het bestand een reparsepunt heeft, ziet u **tagwaarde Reparse: 0x8000001e**. Deze hexadecimale waarde is het reparsepunt puntwaarde eigendom van Azure File-synchronisatie. De uitvoer bevat ook het reparsepunt delen van gegevens die het pad naar het bestand in uw Azure-bestand vertegenwoordigt.

        > [!Warning]  
        > De `fsutil reparsepoint` hulpprogramma opdracht bevat ook de mogelijkheid om te verwijderen van een reparsepunt. Met deze opdracht niet uitgevoerd tenzij aan het technische team van Azure bestand Sync - doen gegevensverlies kan leiden. 

* <a id="afs-recall-file"></a>**Een bestand dat ik wil gebruiken is gelaagde... Hoe kan ik deze naar de schijf voor gebruik lokaal intrekken?**  
    De eenvoudigste manier om een bestand naar de schijf intrekken is om deze te openen. De Azure-bestand Sync bestandssysteemfilter (StorageSync.sys) download het bestand van uw Azure-bestandsshare naadloos zonder dat u hoeft geen werk te doen. Voor de bestandstypen die gedeeltelijk kunnen worden resulteert gelezen in, zoals multimedia of zip-bestanden openen van een bestand niet in het downloaden van het hele bestand.

    Het is ook mogelijk force een bestand aan herinnerd met behulp van PowerShell. Dit kan bijvoorbeeld handig als u wilt intrekken veel bestanden tegelijk (zoals alle bestanden in een map) zijn. Open een PowerShell-sessie in het knooppunt van de server waarop Azure File-synchronisatie is geïnstalleerd en voer de volgende PowerShell-opdrachten:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Hoe kan ik afdwingen dat een bestand of map tiers worden verdeeld?**  
    Wanneer ingeschakeld, de functie Lagen automatisch bestanden op basis van laatste toegang servicetier en tijden wijzigen met het oog op de percentage vrije ruimte op volume dat is opgegeven op het cloudeindpunt, maar soms wilt u wellicht een bestand handmatig trapsgewijs. Dit kan bijvoorbeeld zijn handig als u een groot bestand dat u opnieuw gebruiken gedurende een lange periode en de vrije ruimte op het volume nu voor andere bestanden/mappen niet wilt opslaan. U kunt afdwingen lagen met de volgende PowerShell-opdrachten:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Welke bestanden of mappen worden automatisch uitgesloten door Azure bestand synchronisatie?**
    Standaard omvat Azure bestand synchronisatie niet de volgende bestanden:
    
    - Desktop.ini
    - Thumbs.DB
    - ehthumbs.DB
    - ~$\*.\*
    - \*ACCDB
    - \*tmp
    - 635D02A9D91C401B97884B82B3BCDAEA.\*

    De volgende mappen worden ook standaard uitgesloten:

    - \System volume Information
    - \$RECYCLE. OPSLAGLOCATIE
    - \SyncShareState

* <a id="afs-os-support"></a>**Ik wil Azure bestand Sync gebruiken met Windows Server 2008 R2 met Linux, of met mijn NAS-apparaat - ondersteuning biedt voor Azure bestand Sync die?**
    Vandaag de dag Azure bestand Sync biedt alleen ondersteuning voor Windows Server 2016 en Windows Server 2012 R2. Op dit moment geen andere schema's die wilt delen, maar we zijn geopend en geïnteresseerd om extra platforms op basis van vraag van klanten te ondersteunen. Laat ons weten met welke platforms die u opnemen wilt voor de ondersteuning op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

## <a name="security-authentication-and-access-control"></a>Beveiliging, verificatie en toegangsbeheer
* <a id="ad-support"></a>**Active Directory gebaseerde verificatie en toegangsbeheer wordt ondersteund door Azure Files is?**  
    Azure-bestanden kunt u voor het beheren van toegangsbeheer op twee manieren:

    - Met SAS, kunt u tokens met specifieke machtigingen die geldig voor een opgegeven tijdsinterval zijn genereren. U kunt bijvoorbeeld een token met alleen-lezen toegang tot een bepaald bestand 10 minuten verloopdatum genereren. Iedereen die dit token beschikt wanneer deze geldig is heeft alleen-lezen toegang tot dit bestand die 10 minuten. SAS-sleutels worden alleen ondersteund via de REST-API of clientbibliotheken vandaag, moet u de Azure-bestandsshare via SMB koppelen met de opslagaccountsleutels.

    - Azure File-synchronisatie wordt behouden en alle ACL's (op basis van AD of lokale) repliceren naar alle server-eindpunten waarmee wordt gesynchroniseerd. Omdat Windows Server al met Active Directory verifiëren kan, Azure bestand synchronisatie kan een uitstekende stop hiaat meting bieden tot volledige ondersteuning voor verificatie op basis van AD en ACL-ondersteuning binnenkomt.

    Azure Files biedt geen ondersteuning voor Active Directory rechtstreeks op dit moment.

* <a id="encryption-at-rest"></a>**Hoe zorgt dat mijn Azure-bestandsshare is versleuteld in rust?**  
    Codering in rust wordt momenteel wordt standaard ingeschakeld in alle regio's. Voor deze regio's er geen te ondernemen om versleuteling te schakelen. Raadpleeg voor andere regio's, [serverversleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Hoe kan ik toegang bieden tot een specifiek bestand met een webbrowser?**  
    Met SAS, kunt u tokens met specifieke machtigingen die geldig voor een opgegeven tijdsinterval zijn genereren. U kunt bijvoorbeeld een token genereren met alleen-lezen toegang tot een bepaald bestand voor een bepaalde periode. Iedereen die deze url heeft toegang tot het bestand rechtstreeks via elke webbrowser wanneer deze geldig is. SAS-sleutels kunnen eenvoudig worden gegenereerd vanuit een gebruikersinterface zoals Opslagverkenner.

* <a id="file-level-permissions"></a>**Is het mogelijk om op te geven alleen-lezen of alleen-schrijven machtigingen op de mappen in de share?**  
    Dit niveau van controle over machtigingen is niet mogelijk wanneer u de bestandsshare koppelt via SMB. U kunt dit echter wel bereiken door een Shared Access Signature (SAS) te maken via de REST API of clientbibliotheken.

* <a id="ip-restrictions"></a>**Kan ik het IP-beperkingen voor een Azure-bestandsshare implementeren?**  
    Toegang tot uw Azure-bestandsshare kan Ja, worden beperkt op een niveau storage-account. Zie voor meer informatie Zie [Azure Storage-Firewalls configureren en virtuele netwerken](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Wat zijn de gegevens nalevingsbeleid voor Azure-bestanden ondersteund?**  
   Azure Files wordt uitgevoerd boven op de dezelfde opslagarchitectuur als andere storage-services in Azure Storage en past de beleidsregels voor naleving van dezelfde gegevens. Meer informatie over de naleving van Azure Storage-gegevens, die u kunt downloaden en verwijzen naar [Microsoft Azure Data Protection document](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409) en de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Toegang tot on-Premises
* <a id="expressroute-not-required"></a>**Heb ik Azure ExpressRoute gebruiken voor verbinding met Azure-bestanden of voor het gebruik van Azure bestand Sync on-premises?**  
    Nee, ExpressRoute is niet vereist voor toegang tot een Azure-bestandsshare. Als u een Azure-bestandsshare rechtstreeks bent koppelen is on-premises alle die nodig dat u poort 445 (TCP uitgaand) geopend voor toegang tot Internet (dit is de poort die SMB communiceert). Als u Azure File-synchronisatie, is dat poort 443 (TCP uitgaand) is vereist voor HTTPS-toegang (geen SMB vereist). ExpressRoute kan echter worden gebruikt met beide opties toegang, indien gewenst.

* <a id="mount-locally"></a>**Hoe kan ik een Azure-bestandsshare op mijn lokale computer koppelen?**  
    U kunt de bestandsshare koppelt via het SMB-protocol, zolang poort 445 (TCP uitgaand) geopend is en de client het SMB 3.0-protocol ondersteunt (bijvoorbeeld, u Windows 10 of Windows Server 2016). Als poort 445 wordt geblokkeerd door uw organisatie beleid of door uw Internetprovider, kunt u Azure bestand Sync kunt gebruiken voor toegang tot uw Azure-bestandsshare.

## <a name="backup"></a>Back-up
* <a id="backup-share"></a>**Hoe kan ik back-up van mijn Azure-bestand delen?**  
    U kunt periodieke [momentopnamen (preview) delen](storage-how-to-use-files-snapshots.md) ter bescherming tegen onopzettelijk verwijderen. U kunt gebruikmaken van AzCopy, RoboCopy, of een 3rd partij back-hulpprogramma dat u kunt back-up een gekoppelde bestandsshare. 

## <a name="share-snapshots"></a>Delen van momentopnamen
### <a name="share-snapshots---general"></a>Delen van momentopnamen - algemeen
* <a id="what-are-snaphots"></a>**Wat is er een momentopname van bestanden delen?**  
    Azure File share momentopnamen kunt u een alleen-lezen-versies van uw bestandsshares maken. U kunt er ook een oudere versie van de back-inhoud kopiëren naar dezelfde share of een alternatieve locatie in Azure of on-premises voor verdere wijzigingen. Zie voor meer informatie over de momentopname van de share op [delen momentopname overzicht](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**Waar kan ik mijn momentopnamen share opgeslagen?**  
    Share momentopnamen worden opgeslagen in hetzelfde opslagaccount als de bestandsshare.

* <a id="snapshot-perf-impact"></a>**Zijn er geen gevolgen voor prestaties?**  
    Share momentopnamen hoeft niet elke prestatieoverhead.

* <a id="snapshot-consistency"></a>**Toepassing van momentopnamen share consistent zijn?**  
    Nee. Share momentopnamen zijn niet van toepassing consistente. Gebruiker heeft de schrijfbewerkingen leegmaken van de toepassing naar de share voordat het maken van de momentopname delen.

* <a id="snapshot-limits"></a>**Zijn er beperkingen met betrekking tot het aantal momentopnamen van de share?**  
    Er is een limiet van 200 share momentopnamen die Azure-bestanden kunt behouden. Momentopnamen van de share meetellen niet voor het quotum voor de bestandsshare zodat er geen per share limiet voor de totale ruimte die door alle momentopnamen van de bestandsshare gebruikt. Limieten voor opslagaccounts nog steeds toepassen. Oudere momentopnamen moeten worden verwijderd om te kunnen maken van nieuwe momentopnamen die share na 200 share momentopnamen.

### <a name="create-share-snapshots"></a>Momentopnamen van de share maken
* <a id="file-snaphsots"></a>**Kan ik een momentopname van de share van afzonderlijke bestanden maken?**  
    Share momentopnamen worden gemaakt op het bestandsshareniveau. U kunt afzonderlijke bestanden herstellen met de momentopname van de bestandsshare, maar u bestandsniveau share momentopnamen kan niet maken. Echter, als u een momentopname van een share niveau share hebt gemaakt en u wilt weergeven van de share momentopnamen waarbij een bepaald bestand is gewijzigd, u kunt doen 'Vorige versies' ervaring op een gekoppelde Windows-share van. Laat ons weten als u de noodzaak van een functie van de momentopname bestand hebben op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>**Kan ik share momentopnamen van versleutelde bestandsshare maken?**  
    U kunt een momentopname delen van Azure-bestandsshares waarvoor versleuteling in rust ingeschakeld. U kunt bestanden terugzetten vanuit een momentopname van de share naar een versleutelde bestandsshare. Als de share is versleuteld, wordt uw momentopname share eveneens versleuteld.

* <a id="geo-redundant-snaphsots"></a>**Zijn de momentopnamen van mijn share geografisch redundante?**
    Momentopname van de share hebben de dezelfde redundantie als de Azure-bestandsshare waarvoor ze zijn genomen. Als u geografisch redundante opslag (GRS) hebt geselecteerd voor uw account worden uw momentopname share ook toch toe opgeslagen in de gekoppelde regio.

### <a name="manage-share-snapshots"></a>Share momentopnamen beheren
* <a id="browse-snapshots-linux"></a>**Kan ik mijn share momentopnamen van Linux bladeren?**  
    U kunt Azure CLI 2.0 gebruiken om te maken, weergeven, bladeren en terugzetten op Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>**Kan ik de momentopnamen van de share kopiëren naar een ander opslagaccount**  
    U kunt bestanden van momentopnamen van de share kopiëren naar een andere locatie, maar niet naar de share momentopnamen zelf.

### <a name="restore-data-from-share-snapshots"></a>Gegevens terugzetten van momentopnamen van de share
* <a id="promote-share-snapshot"></a>**Kan ik een momentopname van een share met de share base promoveren?**  
    U kunt alleen gegevens vanuit een momentopname van de share gekopieerd naar elke gewenste bestemming. U kunt een momentopname van een share met de share base echter kan niet promoveren.

* <a id="restore-snapshotted-file-to-other-share"></a>**Kan ik gegevens terugzetten vanuit Mijn momentopname share naar een ander opslagaccount?**  
    Ja. Bestanden vanuit een momentopname van de share kunnen worden gekopieerd naar de oorspronkelijke of een alternatieve locatie waaronder dezelfde/ander opslagaccount in dezelfde of verschillende regio's. U kunt ook bestanden kopiëren naar de on-premises of een andere cloud.    
  
### <a name="cleanup-share-snapshot"></a>Opruimen share momentopname
* <a id="delete-share-keep-snapshots"></a>**Kan ik mijn share verwijderen, maar niet delen momentopnamen?**
    Niet mogelijk uw share verwijderen als er actieve share momentopnamen op de share. Er is een API beschikbaar voor het verwijderen van de share momentopnamen samen met de share en u kunt dezelfde vanuit Azure portal ook bereiken.

* <a id="delete-share-with-snapshots"></a>**Wat gebeurt er met mijn momentopnamen van de share als ik mijn Storage-Account verwijderen?**
    Als u uw storage-account verwijdert, wordt de share momentopnamen ook verwijderd.

## <a name="billing-and-pricing"></a>Facturering en prijzen
* <a id="vm-file-share-network-traffic"></a>**Telt het netwerkverkeer tussen een virtuele machine van Azure en een Azure-bestandsshare mee als externe bandbreedte die wordt verrekend met het abonnement?**  
    Als de bestandsshare en de virtuele machine zich in dezelfde Azure-regio, wordt het verkeer tussen hen is gratis. Als ze zich in verschillende regio's, wordt het verkeer tussen hen als externe bandbreedte in rekening gebracht.

* <a id="share-snapshot-price"></a>**Hoeveel moet momentopnamen kosten delen?**
     Tijdens de Preview-share momentopname capaciteit geen afgeschreven. Standaard opslag uitgaande en transactie kosten vensterbank toepassen. Na algemene beschikbaarheid zowel de capaciteit en de transacties in momentopname van de share wordt in rekening gebracht.
     
     Share momentopnamen zijn incrementele aard. De momentopname base share is de bestandsshare zelf. Alle volgende share momentopnamen zijn incrementele en worden alleen de diff uit de vorige momentopname van de bestandsshare opgeslagen. U wordt gefactureerd alleen voor de gewijzigde inhoud. Als u een share met 100 GiB van gegevens hebt, maar slechts 5 GiB is gewijzigd sinds de laatste momentopname van de share, de share momentopname wordt verbruikt slechts 5 aanvullende GiB en kunt u gefactureerd alleen 105 GiB. Zie [prijzen pagina](https://azure.microsoft.com/pricing/details/storage/files/) voor meer informatie over transactie en kosten voor uitgaande standaard.

## <a name="scale-and-performance"></a>Schaal en prestaties
* <a id="files-scale-limits"></a>**Wat zijn de schaallimieten van Azure-bestanden?**  
    Zie voor informatie over de schaalbaarheids- en prestatiedoelen van Azure-bestanden, [Azure Files schaalbaarheids- en prestatiedoelen](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Moet ik een grotere bestandsshare dan Azure-bestanden op dit moment biedt... kan ik het formaat van mijn Azure-bestandsshare verhogen?**  
    Nee, is de maximale grootte van een Azure-bestandsshare 5 TiB. Dit is momenteel een vaste limiet die we niet aanpassen. We werken aan een oplossing voor het verhogen van de grootte van de share op 100 TiBs, maar geen tijdlijnen te delen op dit moment.

* <a id="open-handles-quota"></a>**Hoeveel clients kunnen tegelijkertijd toegang hebben tot hetzelfde bestand?**  
    Er is een open ingangen 2000 quotum op één bestand. Wanneer u 2000 open ingangen hebt, ontvangt u een fout die quotum is bereikt.

* <a id="zip-slow-performance"></a>**Mijn prestaties waren slecht tijdens het uitpakken van bestanden naar Azure-bestanden. Wat moet ik doen?**  
    Om grote aantallen bestanden overdragen naar Azure-bestanden, wordt u aangeraden dat u AzCopy (Windows, Preview voor Linux/Unix) of Azure Powershell gebruiken als u deze hulpprogramma's zijn geoptimaliseerd voor netwerkoverdracht.

* <a id="slow-perf-windows-81-2012r2"></a>**Ik heb mijn Azure-bestandsshare gekoppeld op Windows Server 2012 R2 of Windows 8.1 en Mijn is traag - waarom?**  
    Er is een bekend probleem bij het koppelen van een Azure-bestandsshare op Windows Server 2012 R2 en Windows 8.1 die in de cumulatieve update van April 2014 voor Windows 8.1 en Windows Server 2012 R2 is hersteld. Zorg ervoor dat alle exemplaren van Windows Server 2012 R2 en Windows 8.1 deze patch toegepast voor optimale prestaties hebben (natuurlijk altijd wordt aangeraden alle Windows-patches via Windows Update duurt). Voor meer informatie, Controleer of het bijbehorende KB-artikel [prestaties afnemen wanneer u Azure-bestanden vanuit Windows 8.1 of Server 2012 R2 opent](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Functies en -interoperabiliteit met andere services
* <a id="cluster-witness"></a>**Kan ik mijn Azure-bestandsshare als gebruiken een *bestandssharewitness* voor mijn Windows Server Failover Cluster?**  
    Dit wordt momenteel niet ondersteund voor een Azure-bestandsshare. Zie voor meer informatie over hoe u dit instelt voor Azure-blobopslag [een Witness Cloud implementeren voor een failovercluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**Kan ik een Azure-bestandsshare op een exemplaar van Azure Container koppelen?** ?  
    Ja, Azure-bestandsshares zijn ideaal optie voor het persistent maken van gegevens buiten de levensduur van een exemplaar van de container. Zie voor meer informatie [koppelen van een Azure-bestandsshare met Azure Containerexemplaren](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**Is er een naamswijziging in de REST-API?**  
    Momenteel niet.

* <a id="nested-shares"></a>**Zijn geneste shares, met andere woorden, een share onder een share?**  
    Nee. De bestandsshare is het virtuele stuurprogramma dat u kunt koppelen. Geneste shares worden dus niet ondersteund.

* <a id="ibm-mq"></a>**Met behulp van Azure-bestanden met IBM MQ**  
    IBM heeft een document IBM MQ-klanten te helpen bij het configureren van Azure-bestanden met hun service uitgebracht. Voor meer informatie raadpleegt u [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service) (IBM MQ-wachtrijbeheer voor meerdere instanties instellen met Microsoft Azure File-service.

## <a name="see-also"></a>Zie ook
* [Oplossen van problemen in Windows Azure-bestanden](storage-troubleshoot-windows-file-connection-problems.md)
* [Problemen met Azure-bestanden in Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Problemen met Azure File-synchronisatie (preview)](storage-sync-files-troubleshoot.md)