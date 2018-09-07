---
title: Veelgestelde vragen over de Azure Import/Export service | Microsoft Docs
description: Lees de antwoorden op veelgestelde vragen over Azure Import Export-service.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: edaff86531a9c40064d25a046bbbb70f48b75c84
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027126"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Azure Import/Export-service: veelgestelde vragen 
Hier volgen vragen en antwoorden die u hebt wanneer u uw Azure Import/Export-service voor het overbrengen van gegevens naar Azure storage. De vragen en antwoorden zijn in de volgende categorieën onderverdeeld:

- Over Import/Export-service
- De schijven voorbereiden voor importeren/exporteren
- Import-/ exporttaken
- Verzending van schijven
- Diversen 

## <a name="about-importexport-service"></a>Over Import/Export-service

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Kan ik Azure File storage met behulp van de Azure Import/Export-service kopiëren?

Ja. De Azure Import/Export-service ondersteunt importeren in Azure File Storage. Exporteren van Azure Files op dit moment worden niet ondersteund.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>Is de Azure Import/Export-service beschikbaar voor CSP-abonnementen?

Ja. Azure Import/Export-service biedt ondersteuning voor Cloud Solution Providers (CSP)-abonnementen.

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>Kan ik de Azure Import/Export-service gebruiken om te kopiëren van PST-postvakken en SharePoint-gegevens naar Office 365?

Ja. Ga voor meer informatie naar [importeren PST-bestanden of SharePoint-gegevens op Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Kan ik de Azure Import/Export-service gebruiken om te kopiëren offline Mijn back-ups naar de Azure Backup-Service?

Ja. Ga voor meer informatie naar [Offlineback-upwerkstroom in Azure Backup](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Kan ik schijven voor import/export-taken in Microsoft kopen?

Nee. U moet de verzending van uw eigen schijven voor het importeren en exporteren van taken.


## <a name="preparing-disks-for-importexport"></a>Schijven voorbereiden voor importeren/exporteren

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>Kan ik de voorbereiding van het station voor een importtaak overslaan? Kan ik een station voorbereiden zonder te kopiëren

Nee. Elk station gebruikt voor het importeren van gegevens moet worden voorbereid met het hulpprogramma Azure WAImportExport. Gebruik het hulpprogramma ook om gegevens te kopiëren naar het station.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>Heb ik nodig om uit te voeren van de schijfvoorbereiding van een bij het maken van een exporttaak bekijken?

Nee. Sommige apparaatupdates worden aanbevolen. Om te controleren of het aantal schijven dat vereist is, van het hulpprogramma WAImportExport PreviewExport opdracht te gebruiken. Zie voor meer informatie, [is beschikbaar als Preview-station gebruik voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx). De opdracht kunt u een voorbeeld van schijfgebruik voor de geselecteerde blobs, op basis van de grootte van de stations die u wilt gebruiken. Controleer ook of u kunt lezen en schrijven naar de harde schijf die wordt geleverd voor de taak voor het exporteren.

## <a name="importexport-jobs"></a>Import-/ exporttaken

### <a name="can-i-cancel-my-job"></a>Kan ik mijn taak annuleren?
Ja. U kunt een taak annuleren als de status ervan **maken** of **verzending**. Taak kan niet worden geannuleerd en gaat door totdat de laatste fase buiten deze fasen.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Hoe lang kan ik de status van voltooide taken bekijken in Azure portal?
U kunt de status voor voltooide taken bekijken voor maximaal 90 dagen. Voltooide taken worden na 90 dagen verwijderd.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>Wat moet ik doen als ik wilt importeren of exporteren van meer dan 10 stations?
Invoer of exporttaak kan verwijzen naar alleen 10 schijven in één taak. Als u wilt meer dan 10 schijven verzenden, moet u meerdere taken te maken. Schijven die zijn gekoppeld aan dezelfde taak moeten samen worden verzonden in hetzelfde pakket. Voor meer informatie over en richtlijnen wanneer gegevenscapaciteit meerdere schijf omvat importeert taken, neem dan contact op met Microsoft op bulkimport@microsoft.com.                                                              

## <a name="shipping-disks"></a>Verzending van schijven

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>Wat is het maximum aantal harde schijven voor in een verzending?
Er is geen limiet voor het aantal harde schijven in een verzending. Als de schijven tot meerdere taken behoren, raden we u: 
- schijven met een bijbehorende taaknamen van label.
- bijwerken van de taken met een volgnummer voorafgegaan door -1,-2 enzovoort.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>Moet ik iets anders dan de harde schijf in mijn pakket opnemen?
Alleen uw harde schijven in het pakket verzending verzenden. Geen items zoals power supply kabels of USB-kabels.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>Heb ik mijn schijven met behulp van FedEx of DHL verzenden?
U kunt schijven in de Azure-datacenter met behulp van alle bekende mobiele provider, zoals FedEx, DHL, UPS of ons postservice verzenden. Echter, voor verzending van stations voor u van het datacenter, moet u opgeven:

- Een getal van de account FedEx in de Verenigde Staten en de Europese Unie, of
- Een DHL nummer in de regio Azië en Australië.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Zijn er beperkingen met mijn station internationaal verzenden?
Houd er rekening mee dat de fysieke media die u moet mogelijk cross-internationale grenzen heen. U bent verantwoordelijk om ervoor te zorgen dat uw fysieke media en de gegevens zijn geïmporteerd en/of geëxporteerd in overeenstemming met de van toepassing zijnde wetten. Voordat u de fysieke media levert, contact op met uw adviseurs om te controleren of dat uw media en de gegevens legaal naar het geïdentificeerde Datacenter kunnen worden verzonden. Dit helpt om ervoor te zorgen dat het Microsoft tijdig bereikt.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Wanneer u een taak maakt, is het verzendadres een locatie die verschilt van de locatie van het opslagaccount. Wat moet ik doen?

Sommige opslaglocaties account worden toegewezen aan de verzending van alternatieve locaties. Eerder kunnen beschikbare verzending locaties ook worden tijdelijk toegewezen naar alternatieve locaties. Controleer altijd het verzendadres opgegeven tijdens het maken van de taak voor het verzenden van uw schijven.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Tijdens het verzenden van het station, vraagt de provider die de data center contact op met adres en het telefoonnummer op te geven. Wat moet ik bieden?

Het telefoonnummer en DC adres wordt geleverd als onderdeel van de taak te maken.


## <a name="miscellaneous"></a>Diversen

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>Wat gebeurt er als ik per ongeluk een harde schijf die niet voldoet aan de vereisten voor de ondersteunde hebt verzonden?

De Azure-Datacenter wordt het station dat niet voldoet aan de vereisten voor de ondersteunde om u te retourneren. Als er slechts enkele van de schijven in het pakket te voldoen aan de vereisten van het ondersteuningsteam, wordt deze schijven worden verwerkt en de stations die niet voldoen aan de vereisten worden geretourneerd aan u.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>De service formatteren van de stations alvorens ze?

Nee. Alle stations zijn versleuteld met BitLocker.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Hoe krijg ik toegang tot gegevens die zijn geïmporteerd door deze service?

Gebruik de Azure-Portal of [Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) voor toegang tot de gegevens onder uw Azure storage-account.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Nadat het importeren voltooid is, wat mijn gegevens uitzien als in de storage-account? Mijn directory-hiërarchie bewaard?

Bij het maken van een harde schijf voor een importtaak, is het doel is opgegeven in het veld DstBlobPathOrPrefix in de gegevensset CSV. Dit is de doelcontainer in het opslagaccount waarnaar de gegevens van de vaste schijf worden gekopieerd. In deze bestemmingscontainer virtuele mappen worden gemaakt voor mappen van de vaste schijf en blobs zijn gemaakt voor bestanden. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Als een station bestanden die al bestaan in mijn storage-account heeft, de service wilt overschrijven bestaande blobs of -bestanden?

Hangt ervan af. Wanneer het station is voorbereid, kunt u opgeven of de doel-bestanden moeten worden overschreven of genegeerd met het veld in gegevensset CSV-bestand met de naam toestand: < naam | niet overschrijven | overschrijven >. Standaard de service wijzigt de naam van de nieuwe bestanden in plaats bestaande blobs of bestanden overschrijven.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>Het hulpprogramma WAImportExport compatibel is met 32-bits besturingssystemen?
Nee. Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssystemen. Voor een volledige lijst van ondersteunde OS, gaat u naar [Supported Operating Systems](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>Wat is de maximale blok-Blob en het formaat van pagina-Blob ondersteund door Azure Import/Export?

Blok-Blob voor maximale grootte is ongeveer 4.768TB of 5.000.000 MB.
Max Page Blob-grootte is 1TB.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>Azure Import/Export biedt ondersteuning voor AES-256-codering?
Azure Import/Export-service maakt standaard gebruik van AES-128 bitlocker-versleuteling. U kunt dit wijzigen naar AES-256 door het handmatig versleutelen met bitlocker voordat de gegevens worden gekopieerd. 

- Als u [WAImportExport V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), hieronder volgt een voorbeeldopdracht
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Als u [WAImportExport V2](https://www.microsoft.com/en-us/download/details.aspx?id=55280) "AlreadyEncrypted" opgeven en de sleutel in de driveset CSV opgeven.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure Import/Export?](storage-import-export-service.md)


