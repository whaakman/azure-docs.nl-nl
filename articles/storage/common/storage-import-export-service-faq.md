---
title: Veelgestelde vragen over Azure Import/Export-service | Microsoft Docs
description: Lees de antwoorden op veelgestelde vragen over de service Azure Import exporteren.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: alkohli
ms.openlocfilehash: ed928452946b871ee9192bda82fcbf205b96e6e0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660810"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Azure Import/Export-service: veelgestelde vragen 
Hieronder vindt u vragen en antwoorden die u hebt wanneer u uw Azure Import/Export-service gebruikt mag gegevens overdragen naar Azure storage. Vragen en antwoorden worden gerangschikt in de volgende categorieën:

- Over Import/Export-service
- Voorbereiden van de schijven voor importeren/exporteren
- Taken voor importeren/exporteren
- Back-ups van schijven
- Diversen 

## <a name="about-importexport-service"></a>Over Import/Export-service

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Kan ik Azure File storage met behulp van de service Azure Import/Export kopiëren?

Ja. De Azure Import/Export-service ondersteunt importeren in Azure File Storage. Exporteren van het Azure-bestanden op dit moment ondersteunt niet.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>Is de Azure Import/Export-service beschikbaar voor abonnementen van de CSP?

Ja. Azure Import/Export-service biedt ondersteuning voor abonnementen van de Cloud Solution Providers (CSP).

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>Kan ik de Azure Import/Export-service PST postvakken en SharePoint-gegevens kopiëren naar O365 gebruiken?

Ja. Ga voor meer informatie naar [importeren PST-bestanden of SharePoint-gegevens op Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Kan ik de Azure Import/Export-service offline Mijn back-ups kopiëren naar de Azure Backup-Service gebruiken?

Ja. Ga voor meer informatie naar [Offlineback-upwerkstroom in Azure Backup](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Kan ik stations voor importeren/exporteren taken kopen van Microsoft?

Nee. U moet verzenden stations voor importeren en exporteren van taken.


## <a name="preparing-disks-for-importexport"></a>Schijven voorbereiden voor importeren/exporteren

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>Kan ik de stap ter voorbereiding van station voor een import-taak overslaan? Kan ik een station voorbereiden zonder te kopiëren

Nee. Een station dat wordt gebruikt voor het importeren van gegevens moet worden voorbereid met het hulpprogramma Azure WAImportExport. Gebruik het hulpprogramma ook om gegevens te kopiëren naar het station.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>Heb ik nodig om uit te voeren van de schijfvoorbereiding van een bij het maken van een taak voor het exporteren?

Nee. Sommige prechecks worden aanbevolen. Om te zien van het aantal schijven die zijn vereist, van het hulpprogramma WAImportExport PreviewExport opdracht te gebruiken. Zie voor meer informatie [station gebruik bekijken voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx). De opdracht kunt u een voorbeeld bekijken van schijfgebruik voor de geselecteerde blobs, op basis van de grootte van de stations die u gaat gebruiken. Ook gelezen uit en schrijven naar de harde schijf die wordt geleverd voor de exporttaak controleren.

## <a name="importexport-jobs"></a>Taken voor importeren/exporteren

### <a name="can-i-cancel-my-job"></a>Kan ik mijn taak annuleren?
Ja. U kunt een taak annuleren als de status **maken** of **back-upfunctie**. Afgezien van deze fasen taak kan niet worden geannuleerd en gaat door totdat de laatste fase.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Hoe lang kan ik de status van de voltooide taken weergeven in de Azure portal?
U kunt de status voor voltooide taken weergeven voor maximaal 90 dagen. Voltooide taken worden na 90 dagen verwijderd.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>Wat moet ik doen als ik wil importeren of exporteren van meer dan 10 schijven?
Invoer of exporttaak kan verwijzen naar alleen 10 schijven in een enkele taak. Als u wilt meer dan 10 schijven verzenden, moet u meerdere taken maken. Stations die zijn gekoppeld aan dezelfde taak moeten samen worden verzonden in hetzelfde pakket. Voor meer informatie en richtlijnen wanneer gegevenscapaciteit meerdere schijf omvat importtaken, neem dan contact op met Microsoft op bulkimport@microsoft.com.                                                              

## <a name="shipping-disks"></a>Back-ups van schijven

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>Wat is het maximum aantal harde schijven voor in één verzending?
Er is geen limiet aan het aantal harde schijven in één verzending. Als de schijven tot meerdere taken behoren, raden we u: 
- label van de schijven met namen van de bijbehorende taken.
- bijwerken van de taken met een volgnummer dat wordt voorafgegaan door -1,-2, enzovoort.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>Moet ik iets anders dan de harde schijf in mijn pakket opnemen?
Alleen de harde schijven in het pakket back-upfunctie voor verzenden. Neem geen items zoals power supply kabels of USB-kabels.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>Heb ik mijn schijven met FedEx of DHL verzenden?
U kunt schijven naar het Azure-datacenter met behulp van een bekende carrier zoals FedEx, DHL, UPS of ons opstuurt verzenden. Echter, voor verzending van stations voor u van het datacenter, moet u opgeven:

- Een getal FedEx account in de Verenigde Staten en de EU, of
- Een DHL nummer in de regio's Asia en Australië.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Zijn er beperkingen met mijn station internationaal verzenden?
Houd er rekening mee dat de fysieke media die u levert mogelijk moet cross internationale grenzen heen. U bent zelf verantwoordelijk voor het garanderen dat uw fysieke media en de gegevens zijn geïmporteerd en/of geëxporteerd in overeenstemming met het toepasselijk recht. Voordat de back-ups van de fysieke media Neem contact op met uw adviseurs om te controleren of uw media en de gegevens kan dit wettelijk naar het geïdentificeerde datacenter worden verzonden. Dit helpt ervoor te zorgen dat het Microsoft tijdig is bereikt.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Wanneer u een taak maakt, is het adres van de back-ups van een locatie die verschilt van de locatie van mijn storage-account. Wat moet ik doen?

Sommige opslaglocaties account zijn toegewezen aan back-upfunctie voor alternatieve locaties. Eerder kunnen beschikbare back-upfunctie locaties ook worden tijdelijk toegewezen naar alternatieve locaties. Controleer altijd het verzendadres opgegeven tijdens het maken van de taak vóór het verzenden van uw schijven.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Tijdens het verzenden van mijn station, wordt u gevraagd de provider voor de data center-mailadres en telefoonnummer telefoonnummer. Wat moet ik bieden?

Het telefoonnummer en de DC-adressen is beschikbaar als onderdeel van taak maken.


## <a name="miscellaneous"></a>Diversen

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>Wat gebeurt er als ik verstuurt per ongeluk een harde schijf die niet voldoet aan de vereisten voor ondersteunde?

De Azure-Datacenter wordt het station dat niet voldoet aan de ondersteunde eisen aan u geretourneerd. Als er slechts enkele van de stations in het pakket voldoen aan de ondersteuningsvereisten die stations worden verwerkt en de stations die niet voldoen aan de vereisten voor u worden geretourneerd.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>De service formatteren van de schijven voordat deze wordt teruggezonden?

Nee. Alle stations zijn versleuteld met BitLocker.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Hoe kan ik toegang tot gegevens die zijn geïmporteerd door deze service?

Gebruik de Azure-Portal of [Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) toegang tot de gegevens onder uw Azure storage-account.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Nadat het importeren voltooid is, hoe mijn gegevens eruit in de storage-account? Mijn mapstructuur behouden blijven?

Als u een harde schijf voorbereidt voor een import-taak, wordt de bestemming opgegeven in het veld DstBlobPathOrPrefix in de gegevensset CSV. Dit is de doelcontainer in het opslagaccount waarvoor gegevens van de vaste schijf worden gekopieerd. Virtuele mappen worden gemaakt voor mappen van de vaste schijf en blobs zijn gemaakt voor bestanden in deze bestemmingscontainer. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Als een station bestanden die al bestaan in mijn storage-account heeft, de service wilt overschrijven bestaande blobs of bestanden?

Is afhankelijk van. Bij het voorbereiden van het station kunt u opgeven of de doel-bestanden moeten worden overschreven of genegeerd met het veld in het CSV-bestand gegevensset toestand aangeroepen: < naam | Nee overschrijven | overschrijven >. Standaard de service wijzigt de naam van de nieuwe bestanden plaats van bestaande blobs of bestanden overschrijven.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>Het hulpprogramma WAImportExport compatibel is met 32-bits besturingssystemen?
Nee. Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssystemen. Voor een volledige lijst van het besturingssysteem wordt ondersteund, gaat u naar [besturingssystemen ondersteund](). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>Wat is de maximale blok-Blob en pagina-blobgrootte ondersteund door Azure Import/Export?

De grootte van de maximale blok-Blob is ongeveer 4.768TB of 5,000,000 MB.
Maximale Page Blob-grootte is 1TB.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>Azure Import/Export biedt ondersteuning voor AES-256-versleuteling?
Azure Import/Export-service maakt standaard gebruik van AES-128 bitlocker-versleuteling. U kunt dit wijzigen in AES-256 door handmatig te versleutelen met bitlocker voordat de gegevens worden gekopieerd. 

- Als u [WAImportExport V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), hieronder volgt een voorbeeld van een opdracht
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Als u [WAImportExport V2](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) opgeven 'AlreadyEncrypted' en de sleutel in de driveset CSV opgeven.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure Import/Export?](storage-import-export-service.md)


