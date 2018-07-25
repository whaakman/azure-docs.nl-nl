---
title: Veelgestelde vragen over Microsoft Azure Data Box Disk | Microsoft Docs in gegevens
description: Bevat veelgestelde vragen en antwoorden voor Azure Data Box Disk, een cloudoplossing waarmee u grote hoeveelheden gegevens naar Azure kunt overdragen
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/17/2018
ms.author: alkohli
ms.openlocfilehash: 5288e9900c75eae7601b84f7366edf9ac739d5da
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125800"
---
# <a name="what-is-azure-data-box-disk-preview"></a>Wat is Azure Data Box Disk? (Preview)

Met de cloudoplossing van Microsoft Azure Data Box Disk kunt u terabytes aan gegevens op een snelle, goedkope en betrouwbare manier naar Azure verzenden. Hier vindt u wellicht antwoorden op uw vragen over het gebruik van Data Box-schijven in Azure Portal. 

De vragen en antwoorden zijn in de volgende categorieën onderverdeeld:

- Over de service
- Configureren en verbinding maken 
- Status bijhouden
- Gegevens migreren 
- Gegevens verifiëren en uploaden 

> [!IMPORTANT]
> Data Box Disk is in de preview-fase. Lees de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.

## <a name="about-the-service"></a>Over de service

### <a name="q-what-is-azure-data-box-service"></a>V. Wat is de Azure Data Box-service? 
A.  De service Azure Data Box is bedoeld voor offlinegegevensopname. Met deze service wordt een reeks producten beheerd die geschikt zijn voor gegevenstransport bij verschillende opslagcapaciteiten. 

### <a name="q-what-are-azure-data-box-disks"></a>V. Wat is Azure Data Box Disks?
A. Met Azure Data Box Disks kunt u terabytes aan gegevens snel, goedkoop en veilig naar en vanuit Azure overdragen. U ontvangt maximaal vijf schijven, met een maximale opslagcapaciteit van 35 TB. U kunt deze schijven makkelijk configureren, verbinden en ontgrendelen via de Data Box-service in Azure Portal.  

Schijven worden versleuteld met Microsoft BitLocker-stationsversleuteling en uw encryptiesleutels worden beheerd in Azure Portal. Vervolgens kopieert u de gegevens vanuit de servers van de klant. In het datacenter worden uw gegevens naar de cloud gemigreerd met behulp van een snelle uploadkoppeling via een privénetwerk en naar Azure geüpload.

### <a name="q-when-should-i-use-data-box-disks"></a>V. Wanneer moet ik Data Box Disks gebruiken?
A. Als u 35 TB (of minder) aan gegevens naar Azure wilt overdragen, hebt u baat bij het gebruik van Data Box Disks.

### <a name="q-what-is-the-price-of-data-box-disks"></a>V. Wat kost Data Box Disks?
A. Tijdens de previewfase is de Data Box Disks kosteloos. De verzending van de schijven is ook gratis, maar er zijn wel kosten verbonden aan de Azure-opslag.

### <a name="q-how-do-i-get-data-box-disks"></a>V. Hoe verkrijg ik Azure Data Box Disks? 
A.  U registreert u eerst voor de [previewversie van Data Box Disk](http://aka.ms/AzureDataBox) als u Azure Data Box-schijven wilt hebben. Vervolgens meldt u zich aan bij Azure Portal en plaatst u een Data Box-bestelling voor schijven. Geef uw contactgegevens en overige informatie op. Zodra u een bestelling hebt geplaatst, worden de schijven op basis van beschikbaarheid binnen tien dagen verzonden.   

### <a name="q-what-is-the-maximum-amount-of-data-i-can-transfer-with-data-box-disks-in-one-instance"></a>V. Wat is de maximale hoeveelheid gegevens die ik met Data Box Disks in één keer kan overdragen?
A. Bij vijf schijven van 8 TB (7 TB aan bruikbare capaciteit), bedraagt de maximaal bruikbare capaciteit 35 TB. U kunt dus 35 TB aan gegevens in één keer overdragen.  Als u meer gegevens wilt overdragen, dient u meer schijven te bestellen.

### <a name="q-how-can-i-check-if-data-box-disks-are-available-in-my-region"></a>V. Hoe kan ik controleren of Data Box Disks in mijn regio beschikbaar is? 
A.  Data Box Disks is tijdens de previewfase beschikbaar in de V.S. en alle landen in de Europese Unie.  

### <a name="q-which-regions-can-i-store-data-in-with-data-box-disks"></a>V. In welke regio's kan ik gegevens opslaan met Data Box Disks?
A. Data Box Disk wordt voor preview ondersteund in alle regio's binnen de V.S. en West- en Noord-Europa. Alleen de Azure-regio's met een openbare cloud worden ondersteund. Azure Government of andere onafhankelijke clouds worden niet ondersteund.

### <a name="q-whom-should-i-contact-if-i-encounter-any-issues--with-data-box-disks"></a>V. Met wie moet ik contact opnemen als ik een probleem krijg met Data Box-schijven?
A. Bij problemen met Data Box-schijven neemt u contact op met [Data Box Disk Support](mailto:expresspodsupport@microsoft.com).

## <a name="configure-and-connect"></a>Configureren en verbinding maken
 
### <a name="q-can-i-specify-the-number-of-data-box-disks-in-the-order"></a>V. Kan ik het aantal Data Box-schijven in de bestelling opgeven?
A.  Nee. Het aantal schijven van 8 TB (maximaal vijf) dat u ontvangt, is afhankelijk van uw gegevensgrootte en de beschikbaarheid van de schijven.  

### <a name="q-how-do-i-unlock-the-data-box-disks"></a>V. Hoe ontgrendel ik de Data Box-schijven? 
A.  Ga in Azure Portal naar uw Data Box Disk-bestelling en vervolgens naar **Apparaatdetails**. Kopieer de wachtwoordsleutel. Download en extraheer het Data Box Disk-ontgrendelingsprogramma in Azure Portal en voer *DataBoxDiskUnlock.exe* uit op de computer met de gegevens die u naar de schijven wilt kopiëren. Geef de wachtwoordsleutel op om de schijven te ontgrendelen. U kunt alle schijven met dezelfde wachtwoordsleutel ontgrendelen.

### <a name="q-can-i-use-a-linux-host-computer-to-connect-and-copy-the-data-on-to-the-data-box-disks"></a>V. Kan ik een Linux-hostcomputer gebruiken om verbinding te maken met de gegevens en deze naar de Data Box-schijven te kopiëren?
A.  Nee. Alleen Windows-computers worden ondersteund. Ga naar de lijst met [Ondersteunde besturingssystemen](data-box-disk-system-requirements.md) voor uw hostcomputer voor meer informatie.

### <a name="q-my-disks-are-dispatched-but-now-i-want-to-cancel-this-order-why-is-the-cancel-button-not-available"></a>V. Mijn schijven zijn verzonden, maar ik wil de bestelling annuleren. Waarom is er geen knop Annuleren?
A.  U kunt de bestelling alleen annuleren nadat de schijven zijn besteld en voordat ze worden verzonden. Zodra de schijven zijn verzonden, kunt u de bestelling niet meer annuleren. Tijdens de previewfase kunt u de schijven kosteloos terugsturen, maar dat kan waarschijnlijk niet meer als deze oplossing algemeen beschikbaar is. 

### <a name="q-can-i-connect-multiple-data-box-disks-at-the-same-to-the-host-computer-to-transfer-data"></a>V. Kan de hostcomputer verbinding maken met meerdere Data Box-schijven tegelijk om gegevens over te dragen?
A. Ja. Er kunnen meerdere Data Box-schijven verbinding maken met dezelfde hostcomputer om gegevens over te dragen en er kunnen meerdere kopieertaken parallel worden uitgevoerd.

## <a name="track-status"></a>Status bijhouden

### <a name="q-how-do-i-track-the-disks-from-when-i-placed-the-order-to-shipping-the-disks-back"></a>V. Hoe kan ik de bestelstatus van de schijven volgen vanaf het moment dat de bestelling is geplaatst als ik de schijven terug wil sturen? 
A.  U kunt de status van de Data Box Disk-bestelling volgen in Azure Portal. Als u de bestelling plaatst, wordt u ook gevraagd een e-mailadres voor meldingen op te geven. Als u dat hebt gedaan, dan krijgt u elke keer per e-mail een melding als er een statuswijziging heeft plaatsgevonden. Meer informatie over het [configureren van e-mailmeldingen](data-box-portal-ui-admin.md#edit-notification-details).

### <a name="q-how-do-i-return-the-disks"></a>V. Hoe kan ik de schijven terugsturen? 
A.  In de verpakking van de Data Box-schijven zit een verzendlabel. Bevestig het label aan de doos met de schijven en geef de verzegelde verpakking af bij de verzender. Als het label beschadigd is of verloren is gegaan, gaat u naar **Overzicht > Verzendlabel downloaden** en downloadt u een nieuw verzendlabel.

## <a name="migrate-data"></a>Gegevens migreren

### <a name="q-what-is-the-maximum-data-size-that-can-be-used-with-data-box-disks"></a>V. Wat is de maximale hoeveelheid gegevens die voor Data Box-schijven kan worden gebruikt?  
A.  Data Box Disk kan uit maximaal vijf schijven bestaan met een maximale gebruikscapaciteit van 35 TB. De schijven zelf zijn elk 8 TB groot (waarvan 7 TB voor gebruik). 

### <a name="q-what-are-the-maximum-block-blob-and-page-blob-sizes-supported-by-data-box-disks"></a>V. Wat zijn de maximale groottes voor blok-blob en paginablob die door Data Box-schijven worden ondersteund? 
A.  De maximale groottes worden bepaald door Azure Storage-limieten. De maximale blok-blob is ongeveer 4,768 TiB en de maximale blobgrootte is 8 TiB. Ga naar [Schaalbaarheids- en prestatiedoelen in Azure Storage](../storage/common/storage-scalability-targets.md) voor meer informatie. 

### <a name="q-what-is-the-data-transfer-speed-for-data-box-disks"></a>V. Wat is de snelheid van gegevensoverdracht voor Data Box-schijven?
A. Bij een test met schijven die verbonden waren via USB 3.0 was de prestatie van de schijven maximaal 430 MB/s. De werkelijke waarden variëren, afhankelijk van de gebruikte bestandsgrootte. Voor kleinere bestanden zijn de prestaties lager.

### <a name="q-how-do-i-know-that-my-data-is-secure-during-transit"></a>V. Hoe weet ik of mijn gegevens veilig zijn tijdens de overdracht? 
A.  Data Box-schijven zijn versleuteld met BitLocker AES-128-bitsversleuteling; de wachtwoordsleutel is alleen beschikbaar via Azure Portal. Meld u aan bij Azure Portal met uw accountreferenties om de wachtwoordsleutel op te halen. Geef deze wachtwoordsleutel op als u het Data Box Disk-ontgrendelingsprogramma wilt uitvoeren.

### <a name="q-how-do-i-copy-the-data-to-the-data-box-disks"></a>V. Hoe kan ik de gegevens naar de Data Box-schijven kopiëren? 
A.  Gebruik een SMB-kopieerprogramma als Robocopy of Diskboss als u gegevens naar schijven wilt kopiëren. U kunt ook slepen vanuit Windows Verkenner. 

### <a name="q-are-there-any-tips-to-speed-up-the-data-copy"></a>V. Zijn er tips om het kopiëren van gegevens te versnellen?
A.  U kunt het kopieerproces als volgt versnellen:

- Gebruik meerdere gegevensstromen. In bijvoorbeeld Robocopy kunt u de optie voor meerdere threads gebruiken. Ga naar [Tutorial: Copy data to Azure Data Box Disk and verify](data-box-disk-deploy-copy-data.md#copy-data-to-disks) (Zelfstudie: Gegevens naar Azure data Box kopiëren en verifiëren) voor meer informatie over de opdracht die u dient te gebruiken.
- Gebruik meerdere sessies.
- In plaats van te kopiëren via een netwerkshare (waarbij de netwerksnelheid beperkend kan zijn), zorgt u ervoor dat de gegevens lokaal aanwezig zijn op de computer waarmee de schijven zijn verbonden.
- Zorg ervoor dat u USB 3.0 of hoger gebruikt gedurende het hele kopieerproces. Download en gebruik de [USBView tool](https://docs.microsoft.com/windows-hardware/drivers/debugger/usbview) (USBView-hulpprogramma) om de USB-controllers en de USB-apparaten te identificeren die met de computer zijn verbonden.
- Voer een benchmark-test uit van de prestaties van de computer die wordt gebruikt om de gegevens te kopiëren. Download en gebruik de [Bluestop FIO tool](https://bluestop.org/fio/) (Bluestop FIO-hulpprogramma) om een benchmark-test uit te voeren van de prestaties van de serverhardware.

### <a name="q-how-to-speed-up-the-data-if-the-source-data-has-small-files-kbs-or-few-mbs"></a>V. Hoe kan ik het kopiëren van de gegevens versnellen als de brongegevens uit kleine bestanden (kB's of enkele MB's) bestaan?
A.  U kunt het kopieerproces als volgt versnellen:

- Maak een lokale VHDx in snelle opslag of maak een lege VHD op de HDD/SSD (langzamer).
- Koppel deze aan een VM.
- Kopieer de bestanden naar de schijf van de VM.


### <a name="q-can-i-use-multiple-storage-accounts-with-data-box-disks"></a>V. Kan ik meerdere opslagaccounts gebruiken voor Data Box Disks?
A.  Nee. Er wordt momenteel slechts één (algemeen of klassiek) opslagaccount ondersteund voor Data Box Disks. Zowel dynamische als statische blob wordt ondersteund. Tijdens de preview worden alleen de opslagaccounts in de V.S. en in West- en Noord-Europa in de openbare Azure-cloud ondersteund.

## <a name="verify-and-upload"></a>Verifiëren en uploaden

### <a name="q-how-soon-can-i-access-my-data-in-azure-once-ive-shipped-the-disks-back"></a>V. Wanneer heb ik weer toegang tot mijn gegevens in Azure als ik de schijven terug heb gestuurd? 
A.  Zodra de bestellingsstatus voor Gegevens kopiëren als voltooid is aangemerkt, hebt u weer toegang tot uw gegevens.

### <a name="q-where-is-my-data-located-in-azure-after-the-upload"></a>V. Waar bevinden zich mijn gegevens in Azure na het uploaden?
A.  Als u de gegevens onder de bestanden *BlockBlob* en *PageBlob* op uw schijf kopieert, wordt er voor elke submap onder de mappen *BlockBlob* en *PageBlob* een container gemaakt in het Azure Storage-account. Als u de bestanden onder de mappen *BlockBlob* en *PageBlob* rechtstreeks hebt gekopieerd, bevinden deze zich in de standaardcontainer *$root* onder het Azure Storage-account. 

### <a name="q-i-just-noticed-that-i-did-not-follow-the-azure-naming-requirements-for-my-containers-will-my-data-fail-to-upload-to-azure"></a>V. I heb gemerkt dat ik de naamgevingsvereisten voor Azure niet voor mijn containers heb gevolgd. Worden mijn gegevens niet naar Azure geüpload?
A. Als de containernamen hoofdletters bevatten, worden deze automatisch naar kleine letters omgezet. Als de namen anderszins niet aan de vereisten voldoen (speciale tekens, andere taal, enzovoort), dan worden er geen gegevens geüpload.

### <a name="q-how-do-i-verify-the-data-i-copied-onto-multiple-data-box-disks"></a>V. Hoe controleer ik de gegevens die ik naar meerdere Data Box-schijven heb gekopieerd?
A.  Als het kopiëren van de gegevens is voltooid, kunt u `AzureExpressDiskService.cmd` uitvoeren (in de map *AzureImportExport*) voor het genereren van controlesommen voor validatie. Als u meerdere schijven hebt, dient u per schijf een opdrachtvenster te openen en deze opdracht uit te voeren. Houd er rekening mee dat deze bewerking lang kan duren (uren), afhankelijk van de grootte van de gegevens.

### <a name="q-what-happens-to-my-data-after-i-have-returned-the-disks"></a>V. Wat gebeurt er met de gegevens nadat ik de schijven heb teruggestuurd?
A.  Als de gegevens naar Azure zijn gekopieerd, worden de gegevens van de schijven veilig gewist volgens de richtlijnen van NIST SP 800-88 Revision 1.  

### <a name="q-how-is-my-data-protected-during-transit"></a>V. Hoe worden mijn gegevens tijdens het verzenden beveiligd? 
A.  De Data Box-schijven zijn versleuteld met AES-128 Microsoft BitLocker-stationsversleuteling. Hiervoor is één wachtwoordsleutel vereist om alle schijven en toegangsgegevens te ontgrendelen.

### <a name="q-do-i-need-to-rerun-checksum-validation-if-i-add-more-data-to-the-data-box-disks"></a>V. Moet ik de controlesom opnieuw valideren als ik meer gegevens aan de Data Box-schijven toevoeg?
A. Ja. Als u besluit uw gegevens te valideren (aanbevolen), dan dient u de validatie opnieuw uit te voeren als u meer gegevens aan de schijven hebt toegevoegd.

### <a name="q-i-used-all-my-disks-to-transfer-data-and-need-to-order-more-disks-is-there-a-way-to-quickly-place-the-order"></a>V. Ik heb al mijn schijven gebruikt om gegevens over te dragen. Ik wil meer schijven bestellen. Kan ik dat op een snelle manier doen?
A. U kunt een kloon maken van uw vorige bestelling. Hierdoor maakt u dezelfde bestelling als eerst en kunt u de details van de bestelling bewerken zonder dat u opnieuw uw adres, contactgegevens en meldingsinformatie hoeft te typen. 



## <a name="next-steps"></a>Volgende stappen

- De [Systeemvereisten voor Data Box](data-box-disk-system-requirements.md) lezen.
- Informatie over de [Limieten voor Data Box](data-box-disk-limits.md).
- Snel [Azure Data Box Disk](data-box-disk-quickstart-portal.md) in Azure Portal implementeren.
