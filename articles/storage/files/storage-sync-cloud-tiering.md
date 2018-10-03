---
title: Informatie over Azure File Sync Cloud-Opslaglagen | Microsoft Docs
description: Meer informatie over Azure File Sync-functie Cloud Tiering
services: storage
author: sikoo
ms.service: storage
ms.topic: article
ms.date: 09/21/2018
ms.author: sikoo
ms.component: files
ms.openlocfilehash: a11e0a1c20617f3065d5b3f8cf59d67cf7aa0179
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242647"
---
# <a name="cloud-tiering-overview"></a>Warmtemeting overzicht van cloud
Cloud tiering is een optionele functie van Azure File Sync waarin vaak gebruikte bestanden in de cache lokaal op de server opgeslagen terwijl alle andere bestanden naar Azure Files op basis van beleidsinstellingen worden geschakeld. Wanneer een bestand is gelaagd, vervangen het bestandssysteemfilter van Azure File Sync (StorageSync.sys) het bestand lokaal door een wijzer, of een reparsepunt. Het reparsepunt vertegenwoordigt een URL naar het bestand in Azure Files. Een gelaagd bestand is zowel het kenmerk 'offline' als het FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS-kenmerk in NTFS zo instellen dat toepassingen van derden kunnen gelaagde bestanden veilig kan identificeren.
 
Wanneer een gebruiker een gelaagd bestand opent, roept Azure File Sync naadloos de bestandsgegevens van Azure Files zonder de gebruiker hoeft te weten dat het bestand daadwerkelijk is opgeslagen in Azure. 
 
 > [!Important]  
    > Belangrijk: Cloud tiering wordt niet ondersteund voor servereindpunten op de volumes van Windows-systeem, en alleen bestanden die groter zijn dan 64 KiB in grootte kunnen worden lagen naar Azure Files.
    
Azure File Sync biedt geen ondersteuning voor cloudlagen bestanden die kleiner zijn dan 64 KiB zoals de noodzaak van de prestaties van opslaglagen en terughalen van dergelijke kleine bestanden zouden opwegen tegen de ruimtebesparing.

## <a name="cloud-tiering-faq"></a>Warmtemeting Veelgestelde vragen over de cloud

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>Hoe cloud cloudlagen werken?
Het filter van Azure File Sync-systeem een 'heatmap' van de naamruimte is gebaseerd op elk servereindpunt. Deze toegang krijgt tot (Lees- en schrijfbewerkingen) bewaakt na verloop van tijd en vervolgens, op basis van de frequentie en de recentheid van toegang, wijst een heatmap score aan elk bestand. Een veelgebruikte bestand die u onlangs hebt geopend wordt beschouwd als ' hot ', dat een bestand dat is nauwelijks aangeraakt en enige tijd niet is geopend, als ' Cool ' worden beschouwd. Wanneer het bestandsvolume op een server de drempelwaarde voor herstelpuntvolume vrije ruimte die u instelt overschrijdt, wordt het leukste bestanden naar Azure Files laag totdat het percentage vrije ruimte is voldaan.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>Hoe werkt de beleidsinstelling voor lagen volume ruimte?
Vrije ruimte op volume is de hoeveelheid vrije ruimte die u wilt reserveren op het volume waarop een servereindpunt bevindt. Bijvoorbeeld, als vrije ruimte op volume is ingesteld op 20% op een volume dat één servereindpunt heeft, u tot 80% van de ruimte op volume wordt wordt ingenomen door de meest recente gebruikte bestanden, met alle overige bestanden die niet in deze ruimte passen trapsgewijs geschakeld naar Azure. Vrije ruimte op volume is van toepassing op het volumeniveau van het in plaats van op het niveau van afzonderlijke mappen of synchronisatiegroepen. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>Hoe werkt de beleidsinstelling voor lagen volume ruimte met betrekking tot de nieuwe servereindpunten?
Wanneer een servereindpunt zojuist is ingericht en is verbonden met een Azure-bestandsshare, wordt de server eerst de naamruimte wordt opgehaald en vervolgens meest de werkelijke bestanden totdat deze de drempelwaarde voor herstelpuntvolume vrije ruimte raakt. Dit proces is ook wel bekend als fast noodherstel of snelle naamruimte herstellen.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>Hoe wordt er vrije ruimte op volume geïnterpreteerd wanneer ik meerdere servereindpunten op een volume?
Wanneer er meer dan één eindpunt op een volume, is de drempelwaarde van de vrije ruimte effectieve volume de grootste vrije ruimte op volume opgegeven op een servereindpunt op het desbetreffende volume. Bestanden worden in tiers worden verdeeld op basis van hun gebruikspatronen ongeacht welke servereindpunt waartoe ze behoren. Bijvoorbeeld, hebt u twee servereindpunten op een volume 1 en Endpoint2, waarbij 1 heeft de drempelwaarde voor een volume-vrije ruimte van 25% en Endpoint2 heeft de drempelwaarde voor een volume-vrije ruimte van 50%, de drempelwaarde voor herstelpuntvolume vrije ruimte voor zowel servereindpunten worden 50%. 

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>Hoe bepaal ik de juiste hoeveelheid vrije ruimte op volume?
De hoeveelheid gegevens die u lokale bewaren moet wordt bepaald door een aantal factoren: uw bandbreedte, toegangspatroon voor uw gegevensset weer en uw budget. Als u een verbinding met een lage bandbreedte hebt, kunt u meer van uw gegevens lokaal om ervoor te zorgen dat er minimaal vertragingstijd voor uw gebruikers te houden. Anders, u kunt baseren op het verloop gedurende een bepaalde periode. Bijvoorbeeld, als u dat ongeveer 10% van de wijzigingen van de gegevensset 1 TB weet of worden elke maand actief geopend en vervolgens kunt u 100 GB lokaal te houden zodat u zijn niet vaak terughalen van bestanden. Als het volume 2TB is, wordt u wilt behouden van 5% (of 100 GB) lokaal, wat betekent dat de resterende 95% wordt het percentage van de vrije ruimte volume. Echter, raden wij u aan een buffer voor het account voor perioden van hogere verloop – met andere woorden, beginnen met een lager volume ruimtepercentage en deze desgewenst later aanpassen. 

Meer gegevens lokaal bewaren betekent lagere kosten voor uitgaand als minder bestanden zal worden teruggehaald uit Azure, maar ook moet u een grotere hoeveelheid van het on-premises opslag, die wordt geleverd met een eigen kosten onderhouden. Zodra u een exemplaar van Azure File Sync is geïmplementeerd hebt, kunt u uw storage-account uitgaand verkeer om te meten ongeveer of uw instellingen van de vrije ruimte volume geschikt voor uw gebruik zijn bekijken. Ervan uitgaande dat het opslagaccount alleen uw Azure File Sync Cloud-eindpunt (dat wil zeggen, de synchronisatieshare) bevat, wordt hoge uitgaande betekent dat veel bestanden zijn worden teruggehaald uit de cloud, en u rekening moet houden met het verhogen van de lokale cache.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>Hoe weet ik of een bestand is gelaagd?
Er zijn verschillende manieren om te controleren of een bestand naar uw Azure-bestandsshare is gelaagde:
    
   *  **Controleer de bestandskenmerken op het bestand.**
     Met de rechtermuisknop op een bestand, gaat u naar **Details**, en schuif vervolgens omlaag naar de **kenmerken** eigenschap. Een gelaagd bestand heeft de volgende kenmerken instellen:     
        
        | Kenmerk letter | Kenmerk | Definitie |
        |:----------------:|-----------|------------|
        | A | Archiveren | Geeft aan dat het bestand met back-upsoftware moet worden gemaakt. Dit kenmerk is altijd ingesteld, ongeacht of het bestand is gelaagd of volledig op schijf opgeslagen. |
        | P | Sparse-bestand | Geeft aan dat het bestand een sparse-bestand. Een sparse-bestand is een speciaal type bestand dat NTFS biedt voor efficiënte gebruik wanneer het bestand op de schijf-stream grotendeels leeg is. Azure File Sync maakt gebruik van verspreide bestanden omdat een bestand wordt doorverbonden volledig of gedeeltelijk ingetrokken. De bestandsstroom is in een volledig gelaagd bestand opgeslagen in de cloud. In een gedeeltelijk ingetrokken bestand, dat deel uitmaakt van het bestand al op de schijf is. Als een bestand volledig is op schijf ingetrokken, Azure File Sync geconverteerd van een sparse-bestand naar een reguliere-bestand. |
        | L | Reparsepunt | Hiermee wordt aangegeven dat het bestand een reparsepunt bevat. Een reparsepunt is een speciale aanwijzer voor gebruik door een bestandssysteemfilter. Azure File Sync maakt gebruik van reparsepunten voor het definiëren van naar het bestandssysteemfilter van Azure File Sync (StorageSync.sys) de cloud-locatie waar het bestand is opgeslagen. Dit biedt ondersteuning voor naadloze toegang. Gebruikers hoeft niet te weten dat Azure File Sync wordt gebruikt of over het verkrijgen van toegang tot het bestand in uw Azure-bestandsshare. Wanneer een bestand volledig ingetrokken is, wordt het reparsepunt in Azure File Sync verwijderd uit het bestand. |
        | O | Offline | Geeft aan dat sommige of alle van de inhoud van het bestand niet is opgeslagen op schijf. Wanneer een bestand volledig ingetrokken is, wordt dit kenmerk in Azure File Sync verwijderd. |

        ![Het dialoogvenster Eigenschappen voor een bestand met het tabblad met Details geselecteerd](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        U kunt de kenmerken voor alle bestanden in een map zien door toe te voegen de **kenmerken** veld aan de tabel-weergave van de Bestandenverkenner. U doet dit door met de rechtermuisknop op een bestaande kolom (bijvoorbeeld **grootte**), selecteer **meer**, en selecteer vervolgens **kenmerken** uit de vervolgkeuzelijst.
        
   * **Gebruik `fsutil` om te controleren op reparse-punten op een bestand.**
       Zoals beschreven in de vorige optie, heeft een gelaagd bestand altijd een reparsepunt instellen. Een reparsepunt aanwijzer is een speciale aanwijzer voor de Azure File Sync-bestandssysteemfilter (StorageSync.sys). Als u wilt controleren of een bestand in een verhoogde opdrachtprompt of PowerShell-venster een reparsepunt bevat, voer de `fsutil` hulpprogramma:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Als het bestand een reparsepunt heeft, u kunt verwachten **tagwaarde Reparse: 0x8000001e**. Deze hexadecimale waarde is de point-waarde voor het reparsepunt dat eigendom is van Azure File Sync. De uitvoer bevat ook de reparsegegevens die staat voor het pad naar uw bestand op uw Azure-bestandsshare.

        > [!WARNING]  
        > De `fsutil reparsepoint` hulpprogramma opdracht heeft ook de mogelijkheid om te verwijderen van een reparsepunt. Voer deze opdracht niet uit, tenzij het technische team van Azure File Sync wordt gevraagd u aan. Met deze opdracht kan leiden tot verlies van gegevens. 

<a id="afs-recall-file"></a>

### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>Een bestand dat ik wil gebruiken gelaagde. Hoe kan ik het bestand op schijf om lokaal gebruiken intrekken?
De eenvoudigste manier om een bestand naar de schijf intrekken is het bestand te openen. Het bestand downloadt het bestandssysteemfilter van Azure File Sync (StorageSync.sys) naadloos van uw Azure-bestandsshare zonder werk aan uw kant. Voor bestandstypen die gedeeltelijk worden kunnen downloaden niet lezen uit, zoals multimedia- en ZIP-bestanden, een bestand openen het hele bestand.

U kunt ook PowerShell gebruiken om af te dwingen een bestand dat moet worden ingetrokken. Deze optie kan nuttig als u wilt dat aan de hand van meerdere bestanden tegelijk, zoals alle bestanden in een map zijn. Open een PowerShell-sessie met de server-knooppunt waarop de Azure File Sync is geïnstalleerd en voer de volgende PowerShell-opdrachten:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Waarom niet de *ruimte op de schijf* eigenschap voor een bestand overeen met de *grootte* eigenschap na het gebruik van Azure File Sync? 
Windows Verkenner beschrijft de twee eigenschappen voor de grootte van een bestand: **grootte** en **ruimte op de schijf**. Deze eigenschappen verschillen enigszins in betekenis. **Grootte** vertegenwoordigt de volledige grootte van het bestand. **Ruimte op de schijf** Hiermee geeft u de grootte van de stream van het bestand dat opgeslagen op de schijf. De waarden voor deze eigenschappen kunnen verschillen voor verschillende redenen, zoals compressie, het gebruik van Gegevensontdubbeling of cloud-opslaglagen met Azure File Sync. Als een bestand naar een Azure-bestandsshare is gelaagd, is de grootte van de schijf nul, omdat de bestandsstroom is opgeslagen in uw Azure-bestandsshare en niet op de schijf. Het is ook mogelijk dat een bestand te zijn, gedeeltelijk gelaagde (of gedeeltelijk ingetrokken). In een gedeeltelijk gelaagd bestand is onderdeel van het bestand op schijf. Dit kan gebeuren wanneer bestanden gedeeltelijk door andere toepassingen, zoals multimedia spelers gelezen worden of zip-hulpprogramma's. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>Hoe Dwing ik een bestand of map in tiers worden verdeeld?
Wanneer de functie cloudlagen is ingeschakeld, wordt in cloud-opslaglagen automatisch lagen bestanden op basis van laatste toegang en wijzigen van de tijd om de percentage vrije ruimte op volume dat is opgegeven op het cloudeindpunt. Soms echter raadzaam om af te dwingen handmatig een bestand naar tier. Dit kan zijn handig als u een groot bestand dat u niet opnieuw gebruiken gedurende een lange periode wilt opslaan, en u wilt dat de vrije ruimte op het volume nu gebruiken voor andere bestanden en mappen. U kunt afdwingen dat meerdere lagen met behulp van de volgende PowerShell-opdrachten:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

## <a name="next-steps"></a>Volgende stappen
* [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
