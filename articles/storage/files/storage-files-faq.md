---
title: Veelgestelde vragen over Azure-bestanden | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure-bestanden.
services: storage
documentationcenter: ''
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/04/2017
ms.author: renash
ms.openlocfilehash: ef8b5b30edaef61eca1be0cf80c5defd09c4dac2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="frequently-asked-questions-about-azure-files"></a>Veelgestelde vragen over Azure-bestanden
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via de industriestandaard [protocol Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (ook wel Common Internet File System of CIFS). U kunt Azure-bestandsshares gelijktijdig koppelen op de cloud of on-premises implementaties van Windows, Linux en Mac OS. U kunt Azure-bestandsshares op Windows Server-machines cache met behulp van Azure File-synchronisatie (preview) voor snelle toegang bijna waar de gegevens wordt gebruikt.

In dit artikel antwoorden op veelgestelde vragen over Azure-bestanden functies en functionaliteit, waaronder het gebruik van Azure File-synchronisatie met Azure-bestanden. Als u het antwoord op uw vraag niet ziet, u kunt contact met ons opnemen via de volgende kanalen (in groeiende volgorde):

1. Het gedeelte met opmerkingen van dit artikel.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft ondersteuning. Maken van een nieuw ondersteuningsverzoek in de Azure-portal op de **Help** tabblad de **Help + ondersteuning** knop en selecteer vervolgens **nieuw ondersteuningsverzoek**.

## <a name="general"></a>Algemeen
* <a id="why-files-useful"></a>
**Hoe wordt Azure Files nuttig?**  
   U kunt Azure-bestanden gebruiken om bestandsshares te maken in de cloud, zonder die verantwoordelijk is voor het beheren van de overhead van een fysieke server, een apparaat of een apparaat. We doen het monotone werk voor u, inclusief het toepassen van updates voor het besturingssysteem en beschadigde schijven vervangen. Zie voor meer informatie over de scenario's die Azure-bestanden u met kunt [waarom Azure Files is nuttig](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>
**Wat zijn verschillende manieren toegang krijgen tot bestanden in de Azure-bestanden?**  
    U kunt de bestandsshare koppelen op uw lokale machine via het SMB 3.0-protocol of kunt u hulpprogramma's zoals [Opslagverkenner](http://storageexplorer.com/) toegang krijgen tot bestanden in de bestandsshare. U kunt van uw toepassing opslagclientbibliotheken, REST-API's, PowerShell of Azure CLI gebruiken voor toegang tot uw bestanden in de Azure-bestandsshare.

* <a id="what-is-afs"></a>
**Wat is Azure bestand synchronisatie?**  
    Azure File-synchronisatie kunt u bestandsshares van uw organisatie in Azure-bestanden, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver centraliseren. Azure File-synchronisatie transformeert uw Windows Server-machines in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB en NFS Network File System () File Transfer Protocol-Service (FTPS) gebruiken. U kunt zoveel caches als u over de hele wereld nodig hebben.

* <a id="files-versus-blobs"></a>
**Waarom zou ik een Azure-bestandsshare versus Azure Blob storage gebruiken voor mijn gegevens?**  
    Azure Files en Azure Blob storage bieden manieren om grote hoeveelheden gegevens opslaan in de cloud, maar ze zijn nuttig voor verschillende doeleinden. 
    
    Azure Blob-opslag is nuttig voor grootschalige en cloud-systeemeigen toepassingen die nodig zijn voor het opslaan van ongestructureerde gegevens. Azure Blob-opslag is om te maximaliseren prestaties en schaalbaarheid, een eenvoudigere opslag abstractie dan een waar bestandssysteem. Alleen via REST gebaseerde clientbibliotheken (of rechtstreeks via het protocol op basis van REST), kunt u Azure Blob-opslag openen.

    Azure Files is specifiek voor een bestandssysteem. Azure Files heeft alle samenvattingen bestand dat u bekend zijn en veel van jaren van het werken met lokale-besturingssystemen. Zoals Azure Blob storage biedt Azure Files een REST-interface en REST gebaseerde clientbibliotheken. In tegenstelling tot Azure Blob storage biedt Azure Files SMB-toegang tot Azure-bestandsshares. U kunt een Azure-bestandsshare rechtstreeks op Windows, Linux of Mac OS, on-premises of in de cloud VM's, zonder de code te schrijven of eventuele speciale stuurprogramma's toevoegen aan het bestandssysteem koppelen via SMB. U kunt Azure-bestandsshares op bestandsservers lokale cache met behulp van Azure-Sync-bestand voor snelle toegang, bijna waar de gegevens wordt gebruikt. 
   
    Zie voor een gedetailleerdere beschrijving van de verschillen tussen Azure-bestanden en Azure Blob-opslag, [beslist het gebruik van Azure Blob storage, Azure-bestanden of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure Blob storage, [Inleiding tot Blob storage](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Waarom zou ik een Azure-bestandsshare gebruiken in plaats van Azure-schijven?**  
    Een schijf in de Azure-schijven is gewoon een schijf. Een zelfstandige schijf zelf is niet te nuttig. Als u de waarde van Azure-schijven, moet u een schijf koppelen aan een virtuele machine die wordt uitgevoerd in Azure. Azure-schijven kunnen worden gebruikt voor van alles dat u een schijf voor op een on-premises server gebruikt. U kunt deze gebruiken als een besturingssysteemschijf systeem, als de wisselruimte voor een besturingssysteem of als toegewezen opslag voor een toepassing. Een interessante gebruiken voor Azure-schijven is het maken van een bestandsserver in de cloud moet worden gebruikt in de dezelfde plaatsen waar u een Azure-bestandsshare kunt. Een bestandsserver in Azure Virtual Machines implementeert, is een krachtige manier om opslag van bestanden in Azure wanneer u de implementatie-opties die momenteel worden niet ondersteund door Azure-bestanden (zoals NFS-protocol ondersteuning of premium-opslag) vereist. 

    Met een bestandsserver doorgaans met Azure-schijven als back-end-opslag is echter veel duurder dan het gebruik van een Azure-bestandsshare, een aantal oorzaken hebben. Eerst naast betaalt voor schijfopslag, moet ook u betalen voor de kosten van het uitvoeren van een of meer virtuele Azure-machines. Ten tweede u kan ook moet de virtuele machines die worden gebruikt voor het uitvoeren van de bestandsserver beheren. Bijvoorbeeld, bent u verantwoordelijk voor upgrades voor het besturingssysteem. Ten slotte, als u gegevens in de cache on-premises worden uiteindelijk nodig, het is aan u instellen en beheren van replicatietechnologieën, zoals Distributed File System Replication (DFSR), zelf doen.

    Een aanpak voor het ophalen van het beste van zowel de Azure-bestanden en de bestandsserver die wordt gehost, in Azure Virtual Machines (in aanvulling op met de Azure-schijven als back-end-opslag) is het installeren van Azure bestand synchronisatie op een bestandsserver die wordt gehost op een VM-cloud. Als de Azure-bestandsshare in dezelfde regio bevinden als de bestandsserver is, kunt u cloud tiering en stelt u het volume van het percentage vrije ruimte op maximaal (99%) inschakelen. Dit zorgt ervoor minimale duplicatie van gegevens. U kunt ook alle toepassingen die u wilt dat met uw bestandsservers, zoals ondersteuning voor toepassingen waarvoor u de NFS-protocol gebruiken.

    Zie voor meer informatie over een optie voor het instellen van een hoge prestaties en maximaal beschikbare bestandsserver in Azure [implementeren IaaS VM-Gast-clusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Zie voor een gedetailleerdere beschrijving van de verschillen tussen Azure-bestanden en de Azure-schijven [beslist het gebruik van Azure Blob storage, Azure-bestanden of Azure-schijven](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Zie voor meer informatie over Azure-schijven, [Azure beheerd schijven overzicht](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>
**Hoe ga ik aan de slag met Azure-bestanden?**  
   Aan de slag met Azure Files is eenvoudig. Eerst [een bestandsshare maken](storage-how-to-create-file-share.md), en koppel vervolgens in het gewenste besturingssysteem: 

    * [Koppelen in Windows](storage-how-to-use-files-windows.md)
    * [Koppelen in Linux](storage-how-to-use-files-linux.md)
    * [Koppelen in Mac OS](storage-how-to-use-files-mac.md)

   Zie voor een uitgebreidere uitleg over het implementeren van een Azure-bestandsshare ter vervanging van productie-bestandsshares in uw organisatie, [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md).

* <a id="redundancy-options"></a>
**Welke opties voor opslag redundantie worden ondersteund door Azure Files?**  
    Op dit moment ondersteunt Azure-bestanden lokaal redundante opslag (LRS), zone-redundante opslag (ZRS) en geografisch redundante opslag (GRS). We willen leestoegang geografisch redundante opslag met (RA-GRS) in de toekomst ondersteunen, maar er zijn momenteel geen tijdlijnen te delen op dit moment.

* <a id="tier-options"></a>
**Welke opslaglagen worden ondersteund in Azure-bestanden?**  
    Azure-bestanden ondersteunt momenteel alleen de laag standard-opslag. Er zijn momenteel geen tijdlijnen voor premium-opslag- en cool storage ondersteuning op dit moment delen. 
    
    > [!NOTE]
    > U kunt Azure-bestandsshares van alleen-blob storage-accounts of van premium storage-accounts maken.

* <a id="give-us-feedback"></a>
**Ik wil een specifieke functie die is toegevoegd aan Azure-bestanden. Kunt u deze toevoegen?**  
    Het team van Azure Files is geïnteresseerd in horen alle feedback hebt over onze service. Neem stemmen over functieaanvragen op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! Denk eens doorsturen naar u verwennen met veel nieuwe functies.

## <a name="azure-file-sync"></a>Azure File Sync

* <a id="afs-region-availability"></a>
**Welke regio's worden ondersteund voor Azure File-synchronisatie (preview)?**  
    Azure File-synchronisatie is momenteel beschikbaar in Australië-Oost, Canada centraal, VS-Oost, Zuidoost-Azië, VK Zuid, West-Europa en VS-West. Ondersteuning voor meer regio's worden toegevoegd wanneer we voor algemene beschikbaarheid werken. Zie voor meer informatie [beschikbaarheid in regio's](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>
**Kan ik domein en niet-domein-servers in de groep voor synchronisatie met dezelfde hebben?**  
    Ja. Een groep voor synchronisatie mag server-eindpunten die verschillende Active Directory-lidmaatschap hebben, zelfs als ze geen lid van een domein zijn. Hoewel deze configuratie technisch werkt, raadzaam niet deze als een typische configuratie omdat toegangsbeheerlijsten (ACL's) die zijn gedefinieerd voor bestanden en mappen op één server mogelijk niet worden afgedwongen door andere servers in de groep voor synchronisatie. Voor de beste resultaten wordt u aangeraden synchroniseren tussen de servers die zich in hetzelfde Active Directory-forest, tussen servers die zich in verschillende Active Directory-forests, maar die ingestelde vertrouwensrelaties of tussen servers die zich niet in een domein. Het is raadzaam om te voorkomen dat een combinatie van deze configuraties gebruikt.

* <a id="afs-change-detection"></a>
**Ik heb een bestand rechtstreeks in mijn Azure-bestandsshare met behulp van SMB of in de portal gemaakt. Hoe lang duurt het voor het bestand wordt gesynchroniseerd met de servers in de groep voor synchronisatie?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Wat gebeurt er als hetzelfde bestand is gewijzigd op twee servers tegelijkertijd ongeveer?**  
    Een strategie voor een eenvoudige conflictoplossing maakt gebruik van Azure File-synchronisatie: we beide wijzigingen in bestanden die zijn gewijzigd op twee servers tegelijkertijd behouden. De meest recent geschreven wijziging blijft de oorspronkelijke bestandsnaam. Het oudere bestand heeft de machine 'bron' en het aantal conflicten is toegevoegd aan de naam. Deze taxonomie volgt: 
   
    \<FileNameWithoutExtension\>-\<MachineName\>\[-#\].\< Ext\>  

    Het eerste conflict van CompanyReport.docx zou bijvoorbeeld CompanyReport CentralServer.docx worden als CentralServer waar het oudere schrijven zich heeft voorgedaan. De naam van het tweede conflict zou CompanyReport-CentralServer-1.docx.

* <a id="afs-storage-redundancy"></a>
**Is geografisch redundante opslag voor Azure bestand synchronisatie ondersteund?**  
    Ja, ondersteunt Azure-bestanden lokaal redundante opslag (LRS) en via geografisch redundante opslag (GRS). Als een GRS failover tussen gekoppelde regio optreedt, raden wij u aan de nieuwe regio te behandelen als een back-up van gegevens alleen. Azure File-synchronisatie begint niet automatisch gesynchroniseerd met de nieuwe primaire regio. 

* <a id="sizeondisk-versus-size"></a>
**Waarom niet de *ruimte op de schijf* eigenschap voor een bestand overeen met de *grootte* eigenschap na gebruik van Azure bestand synchronisatie?**  
    Windows Verkenner beschrijft de twee eigenschappen staat voor de grootte van een bestand: **grootte** en **ruimte op de schijf**. Deze eigenschappen verschillen in de zin enigszins. **De grootte van** de volledige grootte van het bestand vertegenwoordigt. **Ruimte op de schijf** vertegenwoordigt de grootte van de bestandsstroom die zijn opgeslagen op de schijf. De waarden voor deze eigenschappen kunnen verschillen voor een aantal redenen, zoals compressie, het gebruik van Gegevensontdubbeling of cloud tiering met het synchroniseren van Azure-bestand. Als een bestand naar een Azure-bestandsshare is gelaagd, de grootte op de schijf is aan nul, omdat de bestandsstroom is opgeslagen in uw Azure-bestandsshare en niet op de schijf. Het is ook mogelijk dat een bestand moet gedeeltelijk gelaagde (of gedeeltelijk ingetrokken). In een gedeeltelijk gelaagde bestand is onderdeel van het bestand op schijf. Dit kan gebeuren wanneer bestanden door toepassingen zoals multimedia spelers gedeeltelijk gelezen worden of zip-hulpprogramma's. 

* <a id="is-my-file-tiered"></a>
**Hoe kan ik zien of een bestand is gelaagd?**  
    Er zijn verschillende manieren om te controleren of een bestand naar uw Azure-bestandsshare is gelaagde:
    
   *  **Controleer de bestandskenmerken op het bestand.**
     U doet dit door met de rechtermuisknop op een bestand, gaat u naar **Details**, en schuif vervolgens omlaag naar de **kenmerken** eigenschap. Een gelaagde bestand heeft de volgende kenmerken instellen:     
        
        | Kenmerk letter | Kenmerk | Definitie |
        |:----------------:|-----------|------------|
        | A | Archiveren | Hiermee wordt aangegeven dat het bestand moet een back-up door back-upsoftware. Dit kenmerk is altijd ingesteld, ongeacht of het bestand is gelaagd of volledig opgeslagen op schijf. |
        | P | Verspreid bestand | Geeft aan dat het bestand een sparse-bestand. Een sparse-bestand is een speciaal type bestand dat NTFS biedt voor efficiënt gebruik te maken als het bestand op de schijf stroom voornamelijk leeg is. Azure File-synchronisatie wordt gebruikgemaakt van verspreide bestanden omdat een bestand is gelaagd volledig of gedeeltelijk ingetrokken. De bestandsstroom is opgeslagen in een volledig gelaagde bestand in de cloud. In een gedeeltelijk teruggehaald bestand, die deel van het bestand al op schijf is. Als een bestand volledig is ingetrokken op schijf Azure bestand Sync geconverteerd van een sparse-bestand naar een reguliere-bestand. |
        | L | Reparsepunt | Hiermee wordt aangegeven dat het bestand een reparsepunt heeft. Een reparsepunt is een speciale aanwijzer voor gebruik door een bestandssysteemfilter. Azure File-synchronisatie reparsepunten gebruikt voor het definiëren van de Azure-bestand Sync bestandssysteemfilter (StorageSync.sys) de cloud-locatie waar het bestand is opgeslagen. Dit ondersteunt naadloze toegang. Gebruikers hoeft niet te weten dat Azure File-synchronisatie wordt gebruikt of hoe ze toegang krijgen tot het bestand in uw Azure-bestandsshare. Wanneer een bestand volledig worden teruggehaald, verwijdert Azure bestand Sync het reparsepunt uit het bestand. |
        | O | Offline | Hiermee wordt aangegeven dat bepaalde of alle inhoud van het bestand niet is opgeslagen op schijf. Wanneer een bestand volledig worden teruggehaald, verwijdert Azure bestand Sync dit kenmerk. |

        ![Het dialoogvenster Eigenschappen voor een bestand met het tabblad met Details geselecteerd](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        U kunt de kenmerken voor alle bestanden in een map zien door toe te voegen de **kenmerken** veld bij het weergeven van de tabel van Windows Verkenner. U doet dit door met de rechtermuisknop op een bestaande kolom (bijvoorbeeld **grootte**), selecteer **meer**, en selecteer vervolgens **kenmerken** uit de vervolgkeuzelijst.
        
   * **Gebruik `fsutil` om te controleren op reparse-punten op een bestand.**
       Zoals beschreven in de voorgaande optie, heeft een gelaagde bestand altijd een reparsepunt set. Een reparsepunt aanwijzer is een speciale aanwijzer voor het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys). Als u wilt controleren of een bestand in een verhoogde opdrachtprompt of PowerShell-venster een reparsepunt bevat, voer de `fsutil` hulpprogramma:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Als het bestand een reparsepunt heeft, u kunt verwachten **tagwaarde Reparse: 0x8000001e**. Deze hexadecimale waarde is de waarde voor het punt van reparsepunten dat eigendom is van Azure File-synchronisatie. De uitvoer bevat ook de reparsegegevens die staat voor het pad naar uw bestand op uw Azure-bestandsshare.

        > [!WARNING]  
        > De `fsutil reparsepoint` hulpprogramma opdracht heeft ook de mogelijkheid om te verwijderen van een reparsepunt. Met deze opdracht niet uitgevoerd tenzij het technische team van Azure File-synchronisatie u gevraagd te wordt. Met deze opdracht kan leiden tot verlies van gegevens. 

* <a id="afs-recall-file"></a>**Een bestand dat ik wil gebruiken gelaagde. Hoe kan ik het bestand naar schijf voor lokaal gebruik intrekken?**  
    De eenvoudigste manier om het intrekken van een bestand naar de schijf is het bestand te openen. Het bestand downloadt het bestandssysteemfilter van Azure File-synchronisatie (StorageSync.sys) naadloos van uw Azure-bestandsshare zonder werk van uw kant. Voor bestandstypen die gedeeltelijk worden kunnen downloaden gelezen in, zoals multimedia of ZIP-bestanden openen van een bestand niet van het hele bestand.

    U kunt PowerShell gebruiken om af te dwingen een bestand te worden teruggehaald. Deze optie is mogelijk handig als u wilt intrekken meerdere bestanden tegelijk, zoals de bestanden in een map. Open een PowerShell-sessie in het knooppunt van de server waarop Azure File-synchronisatie is geïnstalleerd en voer vervolgens de volgende PowerShell-opdrachten:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>
**Hoe ik afdwingen dat een bestand of map tiers worden verdeeld?**  
    Wanneer de functie lagen is ingeschakeld, wordt cloud tiering automatisch lagen bestanden op basis van laatste toegang en wijzigen van de tijd voor de percentage vrije ruimte op volume dat is opgegeven op het cloudeindpunt. Soms is echter mogelijk u handmatig een bestand naar de laag te forceren. Dit wordt mogelijk handig als u een groot bestand dat u niet opnieuw gebruiken gedurende een lange periode wilt opslaan en u wilt dat de vrije ruimte op het volume nu moet worden gebruikt voor andere bestanden en mappen. U kunt afdwingen lagen met behulp van de volgende PowerShell-opdrachten:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-effective-vfs"></a>
**Hoe wordt *vrije ruimte op volume* wanneer er meerdere server-eindpunten op een volume geïnterpreteerd?**  
    Wanneer er meer dan één servereindpunt op een volume, is de drempelwaarde voor vrije ruimte van effectieve volume de grootste vrije ruimte op volume opgegeven via een willekeurig servereindpunt op dat volume. Bestanden worden tiers worden verdeeld volgens hun gebruikspatronen ongeacht welke servereindpunt waartoe ze behoren. Bijvoorbeeld, als er twee eindpunten van de server op een volume 1 en Endpoint2, waarbij 1 is een volume vrije ruimte drempel van 25% en Endpoint2 heeft de drempelwaarde van een volume vrije ruimte van 50% de drempelwaarde voor herstelpuntvolume vrije ruimte is voor beide eindpunten van de server worden 50%.

* <a id="afs-files-excluded"></a>
**Welke bestanden of mappen worden automatisch uitgesloten door Azure bestand synchronisatie?**  
    Standaard omvat Azure bestand synchronisatie niet de volgende bestanden:
    * desktop.ini
    * Thumbs.DB
    * ehthumbs.DB
    * ~$\*.\*
    * \*.laccdb
    * \*tmp
    * 635D02A9D91C401B97884B82B3BCDAEA.\*

    De volgende mappen worden ook standaard uitgesloten:

    * \System volume Information
    * \$RECYCLE. OPSLAGLOCATIE
    * \SyncShareState

* <a id="afs-os-support"></a>
**Kan ik Azure bestand synchronisatie met Windows Server 2008 R2, Linux of mijn apparaat NAS-opslag (NAS) gebruiken?**  
    Op dit moment ondersteunt Azure bestand synchronisatie alleen Windows Server 2016 en Windows Server 2012 R2. Op dit moment geen andere schema's die wilt delen, maar we zijn geopend voor extra platforms op basis van vraag van klanten te ondersteunen. Laat ons weten op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) welke platforms u graag ondersteunen.

## <a name="security-authentication-and-access-control"></a>Beveiliging, verificatie en toegangsbeheer
* <a id="ad-support"></a>
**Active Directory gebaseerde verificatie en toegangsbeheer wordt ondersteund door Azure Files is?**  
    Azure Files biedt twee manieren voor het beheren van toegangsbeheer:

    - U kunt handtekeningen voor gedeelde toegang (SAS) gebruiken voor het genereren van tokens die specifieke machtigingen hebben en die geldig zijn voor een opgegeven tijdsinterval. U kunt bijvoorbeeld een token met alleen-lezen toegang tot een specifiek bestand met een vervaldatum 10 minuten genereren. Iedereen die het token in bezit terwijl het token geldig heeft alleen-lezen toegang tot dit bestand die 10 minuten. Shared access signature sleutels worden momenteel ondersteund alleen via de REST-API of clientbibliotheken. Met behulp van de opslagaccountsleutels, moet u de Azure-bestandsshare koppelen via SMB.

    - Azure File-synchronisatie worden bewaard en gerepliceerd van alle discretionaire ACL's, of DACL's (of op basis van Active Directory of lokaal) naar alle server-eindpunten die waarmee wordt gesynchroniseerd. Omdat Windows Server al met Active Directory verifiëren kan, Azure bestand Sync effectieve stop hiaat optie tot volledige ondersteuning voor verificatie op basis van Active Directory is en ACL-ondersteuning binnenkomt.

    Op dit moment kunnen ondersteunt Azure-bestanden geen Active Directory rechtstreeks.

* <a id="encryption-at-rest"></a>
**Hoe zorgt dat mijn Azure-bestandsshare in rust versleuteld?**  
    Azure Storage-Service: versleuteling is bezig te wordt standaard ingeschakeld in alle regio's. Voor deze regio's hoeft u niet alle acties versleuteling in te schakelen. Zie voor andere regio's, [serverzijde versleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>
**Hoe kan ik toegang bieden tot een specifiek bestand met een webbrowser?**  
    U kunt de handtekeningen voor gedeelde toegang gebruiken voor het genereren van tokens die specifieke machtigingen hebben en die geldig zijn voor een opgegeven tijdsinterval. U kunt bijvoorbeeld een token met alleen-lezen toegang tot een specifiek bestand gedurende een bepaalde tijd genereren. Iedereen die de URL in bezit toegang tot het bestand rechtstreeks via elke webbrowser terwijl het token ongeldig is. Eenvoudig kunt u een gedeelde toegangssleutel voor de handtekening van een gebruikersinterface zoals Opslagverkenner genereren.

* <a id="file-level-permissions"></a>
**Is het mogelijk om op te geven alleen-lezen of alleen-schrijven machtigingen op de mappen in de share?**  
    Als u de bestandsshare koppelt via SMB, hebt u geen controle over machtigingen niveau van de map. Als u een shared access signature met behulp van de REST-API of clientbibliotheken maakt, kunt u alleen-lezen of alleen-schrijven machtigingen opgeven op de mappen in de share.

* <a id="ip-restrictions"></a>
**Kan ik het IP-beperkingen voor een Azure-bestandsshare implementeren?**  
    Ja. Toegang tot uw Azure-bestandsshare kan op het niveau van de storage-account worden beperkt. Zie voor meer informatie [Azure Storage-Firewalls configureren en virtuele netwerken](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>
**Welke gegevens nalevingsbeleid biedt ondersteuning voor Azure Files?**  
   Azure Files wordt uitgevoerd boven op dezelfde opslagarchitectuur die wordt gebruikt in andere storage-services in Azure Storage. Azure Files is van toepassing de dezelfde nalevingsbeleid voor gegevens die worden gebruikt in andere Azure storage-services. Voor meer informatie over de naleving van Azure Storage-gegevens kunt u downloaden en verwijzen naar de [Microsoft Azure Data Protection document](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409), en Ga naar de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Lokale toegang
* <a id="expressroute-not-required"></a>
**Heb ik Azure ExpressRoute gebruiken voor verbinding met Azure-bestanden of voor het gebruik van Azure bestand Sync on-premises?**  
    Nee. ExpressRoute is niet vereist voor toegang tot een Azure-bestandsshare. Als u wilt koppelen wordt een Azure-bestandsshare direct on-premises alle die vereist is dat poort 445 (TCP uitgaand) openen voor toegang tot internet (dit is de poort die SMB gebruikt om te communiceren). Als u Azure File-synchronisatie, is alle benodigde poort 443 (TCP uitgaand) voor HTTPS-toegang (geen SMB vereist). Echter, u *kunt* ExpressRoute gebruiken met een van deze toegangsopties.

* <a id="mount-locally"></a>
**Hoe kan ik een Azure-bestandsshare op mijn lokale computer koppelen?**  
    U kunt de bestandsshare koppelt via het SMB-protocol als poort 445 (TCP uitgaand) geopend is en de client het SMB 3.0-protocol ondersteunt (bijvoorbeeld, als u Windows 10 of Windows Server 2016). Als poort 445 wordt geblokkeerd door uw organisatie beleid of door uw Internetprovider, kunt u Azure bestand Sync kunt gebruiken voor toegang tot uw Azure-bestandsshare.

## <a name="backup"></a>Back-up maken
* <a id="backup-share"></a>
**Hoe kan ik back-up van mijn Azure-bestand delen?**  
    U kunt periodieke [delen momentopnamen](storage-snapshots-files.md) ter bescherming tegen onbedoeld verwijderen. Ook kunt u AzCopy, Robocopy of een back-hulpprogramma van derden die u kunt back-up van een gekoppelde bestandsshare. 

## <a name="share-snapshots"></a>Delen van momentopnamen
### <a name="share-snapshots-general"></a>Delen van momentopnamen: Algemeen
* <a id="what-are-snaphots"></a>
**Wat zijn de momentopnamen van bestanden delen?**  
    Azure file share momentopnamen kunt u een alleen-lezen-versie van uw bestandsshares maken. U kunt Azure-bestanden gebruiken om te kopiëren van een eerdere versie van uw inhoud terug naar dezelfde share naar een alternatieve locatie in Azure of on-premises voor meer wijzigingen. Zie voor meer informatie over share momentopnamen, de [overzicht van de momentopname delen](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>
**Waar kan ik mijn momentopnamen share opgeslagen?**  
    Share momentopnamen worden opgeslagen in hetzelfde opslagaccount als de bestandsshare.

* <a id="snapshot-perf-impact"></a>
**Zijn er geen gevolgen voor prestaties voor het gebruik van momentopnamen van de share?**  
    Share momentopnamen hoeft niet elke prestatieoverhead.

* <a id="snapshot-consistency"></a>
**Share momentopnamen toepassingsconsistente zijn?**  
    Nee, zijn er geen share momentopnamen toepassingsconsistente. De gebruiker moet de schrijfbewerkingen leegmaken van de toepassing naar de share voor het maken van de momentopname van de share.

* <a id="snapshot-limits"></a>
**Zijn er beperkingen met betrekking tot het aantal momentopnamen van de share die kan worden gebruikt?**  
    Ja. Azure Files kunnen maximaal 200 share momentopnamen behouden. Momentopnamen van de share meetellen niet voor het quotum voor de bestandsshare, zodat er geen limiet per-share op de totale ruimte die wordt gebruikt door alle momentopnamen van de share. Limieten voor opslagaccounts nog steeds toepassen. Na 200 share momentopnamen, moet u de oudere momentopnamen voor het maken van nieuwe momentopnamen die share verwijderen.
* <a id="snapshot-cost"></a>
**Hoeveel deelt momentopname kosten?**  
    Standaard transactie en standaard opslagkosten geldt voor de momentopname. Momentopnamen zijn incrementele aard. De momentopname van het basistype is de share zelf. De volgende momentopnamen zijn incrementele en worden alleen de diff uit de vorige momentopname opgeslagen. Dit betekent dat de deltawijzigingen die zichtbaar in de factuur minimale als uw werkbelastingsverloop minimaal is. Zie [prijzen pagina](https://azure.microsoft.com/pricing/details/storage/files/) voor standaard Azure bestanden prijsinformatie. Vandaag de manier om te kijken naar grootte verbruikt door share momentopname is door het vergelijken van de gefactureerde capaciteit met gebruikt capaciteit. We werken aan tooling voor het verbeteren van de rapportage.


### <a name="create-share-snapshots"></a>Momentopnamen van de share maken
* <a id="file-snaphsots"></a>
**Kan ik een momentopname van de share van afzonderlijke bestanden maken?**  
    Share momentopnamen worden gemaakt op het niveau voor de bestandsshare. U kunt afzonderlijke bestanden herstellen met de momentopname van de bestandsshare, maar u bestandsniveau share momentopnamen kan niet maken. Echter, als u een momentopname van een share-niveau share hebt gemaakt en u wilt weergeven van de share momentopnamen waarbij een specifiek bestand is gewijzigd, u kunt dit doen onder **vorige versies** op een share Windows gekoppeld. 
    
    Als u een momentopname functie nodig hebt, laat ons weten op [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>
**Kan ik share momentopnamen van een gecodeerde bestandsshare maken?**  
    U kunt een share momentopname van de Azure-bestandsshares waarvoor versleuteling in rust ingeschakeld. U kunt bestanden terugzetten vanuit een momentopname van de share naar een versleutelde bestandsshare. Als de share is versleuteld, is uw momentopname share ook versleuteld.

* <a id="geo-redundant-snaphsots"></a>
**Zijn de momentopnamen van mijn share geografisch redundante?**  
    Momentopnamen van de share hebben de dezelfde redundantie als de Azure-bestandsshare waarvoor ze zijn genomen. Als u geografisch redundante opslag voor uw account hebt geselecteerd, wordt toch toe uw share ook een momentopname opgeslagen in de gekoppelde regio.

### <a name="manage-share-snapshots"></a>Share momentopnamen beheren
* <a id="browse-snapshots-linux"></a>
**Kan ik mijn share momentopnamen van Linux bladeren?**  
    U kunt Azure CLI 2.0 maken, weergeven, bladeren en terugzetten van momentopnamen van de share in Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>
**Kan ik de momentopnamen van de share kopiëren naar een ander opslagaccount**  
    U kunt bestanden van momentopnamen van de share naar een andere locatie kopiëren, maar de momentopnamen van de bestandsshare zelf kan niet worden gekopieerd.

### <a name="restore-data-from-share-snapshots"></a>Gegevens terugzetten van momentopnamen van de share
* <a id="promote-share-snapshot"></a>
**Kan ik een momentopname van een share met de share base promoveren?**  
    U kunt gegevens uit een momentopname van een share kopiëren naar een andere bestemming. U kunt een momentopname van een share met de share base kan niet promoveren.

* <a id="restore-snapshotted-file-to-other-share"></a>
**Kan ik gegevens terugzetten vanuit Mijn momentopname share naar een ander opslagaccount?**  
    Ja. Bestanden vanuit een momentopname van de share kunnen worden gekopieerd naar de oorspronkelijke locatie of naar een alternatieve locatie met hetzelfde opslagaccount of een ander opslagaccount in dezelfde regio of in verschillende regio's. U kunt ook bestanden kopiëren naar een on-premises locatie of naar een andere cloud.    
  
### <a name="clean-up-share-snapshots"></a>Opschonen van de share momentopnamen
* <a id="delete-share-keep-snapshots"></a>
**Kan ik mijn share verwijderen maar niet verwijderen van mijn momentopnamen share?**  
    Als u actieve share momentopnamen op uw hebt, kunt u de share niet verwijderen. U kunt een API gebruiken share momentopnamen, samen met de share wilt verwijderen. Ook kunt u zowel de momentopnamen van de share en de share in de Azure portal verwijderen.

* <a id="delete-share-with-snapshots"></a>
**Wat gebeurt er met mijn momentopnamen van de share als ik mijn storage-account verwijderen?**  
    Als u uw storage-account verwijdert, worden de share momentopnamen eveneens verwijderd.

## <a name="billing-and-pricing"></a>Facturering en prijzen
* <a id="vm-file-share-network-traffic"></a>
**Telt het netwerkverkeer tussen een virtuele machine van Azure en een Azure bestandsshare mee als externe bandbreedte die wordt verrekend met het abonnement?**  
    Als de bestandsshare en de virtuele machine zich in dezelfde Azure-regio, is er geen extra kosten voor het verkeer tussen de bestandsshare en de virtuele machine. Als de bestandsshare en de virtuele machine zich in verschillende regio's, het verkeer tussen hen in rekening worden gebracht als externe bandbreedte.

* <a id="share-snapshot-price"></a>
**Hoeveel moet momentopnamen kosten delen?**  
     Tijdens de preview zijn er geen kosten voor de share momentopname capaciteit. Standard-opslag uitgaande- en transactiekosten toepassen. Na algemene beschikbaarheid abonnementen wordt in rekening gebracht voor capaciteit en transacties op de share momentopnamen.
     
     Share momentopnamen zijn incrementele aard. De momentopname base share is de bestandsshare zelf. Alle volgende share momentopnamen zijn incrementele en alleen het verschil van de momentopname van de voorgaande share op te slaan. U wordt gefactureerd alleen voor de gewijzigde inhoud. Als u een share met 100 GiB van gegevens hebt, maar slechts 5 GiB is gewijzigd sinds de laatste momentopname van de share, de share momentopname slechts 5 aanvullende GiB verbruikt en u wordt gefactureerd voor 105 GiB. Zie voor meer informatie over transactie en kosten voor uitgaande standaard de [prijzen pagina](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Schaal en prestaties
* <a id="files-scale-limits"></a>
**Wat zijn de schaallimieten van Azure-bestanden?**  
    Zie voor meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Files [Azure Files schaalbaarheids- en prestatiedoelen](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>
**Ik moet een grotere bestandsshare dan Azure Files is momenteel biedt. Kan ik het formaat van mijn Azure-bestandsshare verhogen?**  
    Nee. De maximale grootte van een Azure-bestandsshare is 5 TiB. Dit is momenteel een vaste limiet die we niet aanpassen. We werken aan een oplossing voor het verhogen van de grootte van de share op 100 TiB, maar er zijn momenteel geen tijdlijnen te delen op dit moment.

* <a id="open-handles-quota"></a>
**Hoeveel clients kunnen tegelijkertijd toegang hebben tot hetzelfde bestand?**   
    Er is een quotum van 2.000 open ingangen in één bestand. Wanneer u 2.000 open ingangen hebt, wordt er een foutbericht weergegeven dat het quotum is bereikt.

* <a id="zip-slow-performance"></a>
**Mijn is traag bij het uitpakken van bestanden in de Azure-bestanden. Wat moet ik doen?**  
    Om grote aantallen bestanden overdraagt naar Azure-bestanden, wordt u aangeraden AzCopy (voor Windows; Preview-versie voor Linux en UNIX) of Azure PowerShell te gebruiken. Deze hulpprogramma's zijn geoptimaliseerd voor netwerkoverdracht.

* <a id="slow-perf-windows-81-2012r2"></a>
**Waarom wordt mijn trage prestaties nadat ik mijn Azure-bestandsshare op Windows Server 2012 R2 of Windows 8.1 koppelen?**  
    Er is een bekend probleem bij het koppelen van een Azure-bestandsshare op Windows Server 2012 R2 en Windows 8.1. Het probleem is hersteld in de cumulatieve update van April 2014 voor Windows 8.1 en Windows Server 2012 R2. Voor optimale prestaties, zorg ervoor dat alle exemplaren van Windows Server 2012 R2 en Windows 8.1 deze patch toegepast. (U moet altijd ontvangen de Windows-patches via Windows Update.) Zie voor meer informatie het bijbehorende Microsoft Knowledge Base-artikel [prestaties afnemen wanneer u Azure-bestanden vanuit Windows 8.1 of Server 2012 R2 opent](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Functies en -interoperabiliteit met andere services
* <a id="cluster-witness"></a>
**Kan ik mijn Azure-bestandsshare als gebruiken een *bestandssharewitness* voor mijn Windows Server Failover Cluster?**  
    Deze configuratie wordt momenteel niet ondersteund voor een Azure-bestandsshare. Zie voor meer informatie over hoe u dit instelt voor Azure-blobopslag [een Witness Cloud implementeren voor een failovercluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>
**Kan ik een Azure-bestandsshare op een Azure-Container-exemplaar koppelen?**  
    Ja, Azure-bestandsshares zijn een goede optie wanneer u wilt behouden van gegevens buiten de levensduur van een exemplaar van de container. Zie voor meer informatie [koppelen van een Azure-bestandsshare met exemplaren van de Azure-Container](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>
**Is er een naamswijziging in de REST-API?**  
    Momenteel niet.

* <a id="nested-shares"></a>
**Kan ik geneste shares instellen? Met andere woorden, een share onder een share?**  
    Nee. De bestandsshare *is* het virtuele stuurprogramma dat u koppelen kunt, zodat geneste shares worden niet ondersteund.

* <a id="ibm-mq"></a>
**Hoe gebruik ik Azure-bestanden met IBM MQ**  
    IBM heeft een document dat helpt IBM MQ-klanten configureren van Azure-bestanden met de service IBM uitgebracht. Zie voor meer informatie [het instellen van een IBM MQ-Wachtrijbeheer meerdere exemplaren met de service Microsoft Azure-bestanden](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Zie ook
* [Azure-bestanden in Windows oplossen](storage-troubleshoot-windows-file-connection-problems.md)
* [Azure-bestanden in Linux oplossen](storage-troubleshoot-linux-file-connection-problems.md)
* [Problemen met Azure File-synchronisatie (preview)](storage-sync-files-troubleshoot.md)
