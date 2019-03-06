---
title: Migreren van gegevens in Azure File Sync met behulp van Azure Data Box en andere methoden
description: Migreren van grote hoeveelheden gegevens op een manier die compatibel is met Azure File Sync.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 3b286bbe2c246345bf6acd84a4fc0c400451c706
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445344"
---
# <a name="migrate-bulk-data-to-azure-file-sync"></a>Grote hoeveelheden gegevens migreren naar Azure File Sync
U kunt grote hoeveelheden gegevens migreren naar Azure File Sync op twee manieren:

* **Uw bestanden uploaden met behulp van Azure File Sync.** Dit is de eenvoudigste methode. Verplaats de bestanden lokaal naar Windows Server 2012 R2 of hoger en de Azure File Sync-agent installeren. Nadat u de synchronisatie hebt ingesteld, wordt uw bestanden van de server worden geüpload. (Onze klanten ondervinden momenteel een gemiddelde uploadsnelheid van 1 TiB over elke twee dagen.) Om ervoor te zorgen dat de server niet wordt gebruikt te veel van de bandbreedte voor uw datacenter, kunt u voor het instellen van een [planning voor bandbreedteregeling](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter).
* **Het overbrengen van uw bestanden offline.** Als u niet voldoende bandbreedte hebt, kunt u mogelijk geen bestanden uploaden naar Azure in een redelijk tijdsbestek. De uitdaging is de initiële synchronisatie van de hele set bestanden. Gebruiken om te strijden tegen deze uitdaging, offline bulk-hulpprogramma's voor migratie, zoals de [Azure Data Box-familie](https://azure.microsoft.com/services/storage/databox). 

In dit artikel wordt uitgelegd hoe u bestanden op een manier die compatibel is met Azure File Sync migreren. Volg deze instructies om te voorkomen van conflicten en uw bestand en map toegangsbeheerlijsten (ACL's) en tijdstempels behouden nadat u de synchronisatie hebt ingeschakeld.

## <a name="online-migration-tools"></a>Hulpprogramma's voor online migratie
Het proces wordt beschreven in dit artikel werkt niet alleen voor de Data Box, maar ook voor andere hulpprogramma's voor offline migratie. Het werkt ook voor online hulpprogramma's zoals AzCopy, Robocopy, of hulpprogramma's en services. Maar het overwinnen van de eerste uitdaging te uploaden, volg de stappen in dit artikel voor het gebruik van deze hulpprogramma's op een manier die compatibel is met Azure File Sync.


## <a name="benefits-of-using-a-tool-to-transfer-data-offline"></a>Voordelen van het gebruik van een hulpprogramma voor het overbrengen van gegevens offline
Hier volgen de belangrijkste voordelen van het gebruik van een hulpprogramma voor bestandsoverdracht, zoals Data Box voor offline migratie:

- U hebt geen al uw bestanden uploaden via het netwerk. Voor grote naamruimten, kan het opslaan van dit hulpprogramma aanzienlijke netwerkbandbreedte en -tijd.
- Wanneer u Azure File Sync gebruikt, ongeacht welke hulpprogramma voor bestandsoverdracht die u gebruikt (Data Box, Azure Import/Export-service, enzovoort), alleen de bestanden die wijzigen nadat u de gegevens naar Azure verplaatst door uw live-server wordt geüpload.
- Azure File Sync worden gesynchroniseerd met uw bestanden en mappen ACL's, zelfs als het hulpprogramma voor migratie van offline bulksgewijs niet vervoer van ACL's.
- Data Box en Azure File Sync hiervoor is geen uitvaltijd vereist. Wanneer u gegevens over te dragen naar Azure Data Box, kunt u efficiënt gebruik van netwerkbandbreedte en de kwaliteit van het bestand behouden. U ook houden uw naamruimte up-to-date te door alleen de bestanden die wijzigen nadat u de gegevens naar Azure verplaatst te uploaden.

## <a name="prerequisites-for-the-offline-data-transfer"></a>Vereisten voor de offline-gegevensoverdracht
Voordat u begint met de offline gegevensoverdracht:

- Voordat u de synchronisatie met Azure File Sync inschakelt, moet u uw gegevens bulksgewijs migreren naar een of meerdere Azure-bestandsshares.
- Als u van plan bent te gebruiken van Data Box voor uw migratie bulksgewijs, raadpleegt u de [implementatievereisten voor de Data Box](../../databox/data-box-deploy-ordered.md#prerequisites).
- Plan uw uiteindelijke Azure File Sync-topologie. Zie voor meer informatie, [plannen voor de implementatie van Azure File Sync](storage-sync-files-planning.md).
- Selecteer het Azure Storage-account of de accounts die u synchroniseren wilt met bestandsshares. Migreer uw gegevens bulksgewijs naar tijdelijke staging bestandsshares die zich in hetzelfde opslagaccount of accounts. U kunt alleen een definitieve share- en een staging-share die zich in hetzelfde opslagaccount gebruiken.
- Maak een nieuwe synchronisatierelatie met locatie van de server. U kunt een bestaande synchronisatierelatie niet gebruiken om grote hoeveelheden gegevens te migreren.

## <a name="process-for-offline-data-transfer"></a>Proces voor de overdracht van offline
Dit is hoe u Azure File Sync instelt op een manier die compatibel is met het hulpprogramma voor migratie van grote hoeveelheden, zoals Azure Data Box:

![Diagram waarin wordt getoond hoe u Azure File Sync kunt instellen](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Stap | Detail |
|---|---------------------------------------------------------------------------------------|
| ![Stap 1](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Uw Data Box bestellen](../../databox/data-box-deploy-ordered.md). De Data Box-familie aanbiedingen [verschillende producten](https://azure.microsoft.com/services/storage/databox/data) om te voldoen aan uw behoeften. Wanneer u uw Data Box ontvangt, volgt u de [documentatie om uw gegevens te kopiëren](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box) naar deze UNC-pad op de Data Box: *\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>*. Hier *ShareName* is de naam van de staging-share. Data Box terug naar Azure verzenden. |
| ![Stap 2](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Wacht totdat de bestanden weergegeven in de Azure-bestandsshares die u hebt gekozen als tijdelijke staging shares. *Schakel geen worden gesynchroniseerd met deze shares.* |
| ![Stap 3](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Maak een nieuwe lege share voor elke bestandsshare die Data Box voor u gemaakt. Deze nieuwe share moet zich in hetzelfde opslagaccount als de Data Box-share. [Over het maken van een nieuwe Azure-bestandsshare](storage-how-to-create-file-share.md). |
| ![Stap 4](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Maak een synchronisatiegroep](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) in een opslagsynchronisatieservice. Verwijzen naar de share leeg zijn als een cloudeindpunt. Herhaal deze stap voor elke bestandsshare Data Box. [Instellen van Azure File Sync](storage-sync-files-deployment-guide.md). |
| ![Stap 5](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [De map van uw live-server toevoegen als een servereindpunt](storage-sync-files-deployment-guide.md#create-a-server-endpoint). In het proces opgeven dat u de bestanden worden verplaatst naar Azure en verwijzen naar de staging-shares. U kunt inschakelen of uitschakelen van cloudopslaglagen indien nodig. Tijdens het maken van een servereindpunt op uw live-server, verwijzen naar de staging-share. Op de **servereindpunt toevoegen** blade onder **Offline gegevensoverdracht**, selecteer **ingeschakeld**, en selecteer vervolgens de staging-share die u moet zich in hetzelfde opslagaccount als de cloud het eindpunt. De lijst met beschikbare shares wordt hier, gefilterd op storage-account en -shares die al zijn niet gesynchroniseerd. |

![Schermafbeelding van de Azure portal-gebruikersinterface, offline gegevensoverdracht tijdens het maken van een nieuw servereindpunt inschakelen](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>De share synchroniseren
Nadat u uw eindpunt hebt gemaakt, wordt de synchronisatie wordt gestart. Het synchronisatieproces wordt bepaald of elk bestand op de server ook voorkomt in de staging-share waar de bestanden in Data Box worden gebracht. Als het bestand bestaat, wordt in het synchronisatieproces het bestand opgehaald uit de staging-share in plaats van het uploaden van de server. Als het bestand bestaat niet in de staging-share, of als een nieuwere versie beschikbaar is op de lokale server, wordt het bestand van de lokale server door het synchronisatieproces geüpload.

> [!IMPORTANT]
> Alleen vast als u een servereindpunt maakt, kunt u de modus van de migratie bulksgewijs inschakelen. Nadat u een servereindpunt hebt ingesteld, kunt u gegevens bulksgewijs gemigreerd van een reeds gesynchroniseerd server niet integreren in de naamruimte.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL's en -timestamps voor bestanden en mappen
Azure File Sync zorgt ervoor dat bestands- en ACL's van de live-server worden gesynchroniseerd, zelfs als het hulpprogramma voor de migratie van grote hoeveelheden die u hebt gebruikt niet in eerste instantie vervoer van ACL's. Als gevolg hiervan moet de staging-share niet alle ACL's voor bestanden en mappen bevatten. Wanneer u de migratiefunctie voor offlinesynchronisatie van gegevens bij het maken van een nieuw servereindpunt inschakelt, worden alle ACL's voor bestanden worden gesynchroniseerd op de server. Zojuist gemaakt en gewijzigde tijdstempels worden ook gesynchroniseerd.

## <a name="shape-of-the-namespace"></a>Vorm van de naamruimte
Wanneer u de synchronisatie inschakelt, bepalen de inhoud van de server in de vorm van de naamruimte. Als bestanden zijn verwijderd uit de lokale server nadat de momentopname van de Data Box en de migratie hebt voltooid, worden deze bestanden niet verplaatsen in de naamruimte die live, synchroniseert. Ze blijven in de staging-share, maar ze zijn niet gekopieerd. Dit is nodig omdat de synchronisatie de naamruimte op basis van de live-server houdt. Data Box *momentopname* is slechts een gefaseerde installatie volledig is ontwikkeld voor het kopiëren van een doeltreffende bestand. Het is niet de instantie voor de vorm van de live-naamruimte.

## <a name="cleaning-up-after-bulk-migration"></a>Na de migratie van grote hoeveelheden opschonen 
Als de server is voltooid voor de initiële synchronisatie van de naamruimte, gebruik de Data Box-bestanden bulksgewijs gemigreerd de staging-bestandsshare. Op de **eigenschappen van servereindpunt** -blade in Azure portal in de **Offline gegevensoverdracht** sectie, de status verandert van **In voortgang** naar **voltooid** . 

![Schermafbeelding van het tabblad Eigenschappen van servereindpunt, waar de besturingselementen voor status- en uitschakelen voor de overdracht van offline zich bevinden](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

U kunt nu de staging-share om kosten te besparen opschonen:

1. Op de **eigenschappen van servereindpunt** blade, wanneer de status **voltooid**, selecteer **uitschakelen offline gegevensoverdracht**.
2. Houd rekening met het verwijderen van de staging-share om kosten te besparen. De staging-share bevat waarschijnlijk geen bestanden en mappen ACL's, dus het is niet erg nuttig. Voor back-updoeleinden point-in-time, maakt u een reëel getal [momentopname van de synchroniseert Azure-bestandsshare](storage-snapshots-files.md). U kunt [Azure Backup instellen om momentopnamen te]( ../../backup/backup-azure-files.md) volgens een schema.

Uitschakelen van de offline modus voor gegevensoverdracht, alleen wanneer de status is **voltooid** of als u wilt annuleren vanwege een onjuiste configuratie. Als u de modus tijdens de implementatie van een uitschakelt, worden bestanden wordt gestart voor het uploaden van de server, zelfs als uw staging bestandsshare nog steeds beschikbaar is.

> [!IMPORTANT]
> Nadat u de offline modus voor gegevensoverdracht uitschakelt, kan niet u deze opnieuw inschakelen, zelfs als de staging-share van de migratie bulksgewijs nog steeds beschikbaar is.

## <a name="next-steps"></a>Volgende stappen
- [Een Azure File Sync-implementatie plannen](storage-sync-files-planning.md)
- [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
