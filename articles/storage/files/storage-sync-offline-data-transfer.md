---
title: Data Box en andere methoden gebruiken voor offline opname in Azure File Sync.
description: Proces- en aanbevolen procedures voor het synchroniseren compatibel bulksgewijs migratie ondersteunen.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.component: files
ms.openlocfilehash: a184e0563d1ad26671c38cabe07f42d97cbe2885
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213355"
---
# <a name="migrate-to-azure-file-sync"></a>Migreren naar Azure File Sync
Er zijn verschillende opties om te verplaatsen naar Azure File Sync:

### <a name="uploading-files-via-azure-file-sync"></a>Uploaden van bestanden via Azure File Sync
De eenvoudigste optie is het verplaatsen van uw bestanden lokaal op een Windows Server 2012 R2 of hoger en de Azure File Sync-agent installeren. Nadat synchronisatie is geconfigureerd, wordt uw bestanden uploaden van de server. We zijn momenteel de uploadsnelheid van een gemiddelde geobserveerd in al onze klanten van 1 TB over elke twee dagen.
Houd rekening met een [planning voor bandbreedteregeling](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter) om te controleren of uw server is een goede burgers in uw datacenter.

### <a name="offline-bulk-transfer"></a>Offline bulksgewijs overdracht
Tijdens het uploaden zeker de eenvoudigste optie is, werkt het misschien niet voor u als de beschikbare bandbreedte wordt niet toegestaan voor het synchroniseren van uw bestanden naar Azure in een redelijk tijdsbestek. De uitdaging om te strijden tegen hier is de initiële synchronisatie van de hele set bestanden. Azure File Sync worden vervolgens alleen wijzigingen verplaatsen wanneer deze zich voordoen in de naamruimte, die doorgaans veel minder bandbreedte gebruikt.
Te strijden tegen deze initiële uploaden uitdaging, hulpprogramma's voor offline bulksgewijs migratie, zoals de Azure [Data Box-familie](https://azure.microsoft.com/services/storage/databox) kan worden gebruikt. Het volgende artikel richt zich op het proces dat u moet volgen om te profiteren van de offline migratie op een manier van Azure File Sync-compatibel. Het beschrijft de aanbevolen procedures waarmee u uw bestanden en mappen ACL's en -timestamps behouden nadat u synchronisatie inschakelt en conflictbestanden vermijden.

### <a name="online-migration-tools"></a>Hulpprogramma's voor online migratie
De onderstaande procedure werkt alleen niet voor de Data Box. Dit werkt voor alle offline migratie-hulpprogramma (zoals Data Box) of online hulpprogramma's, zoals AzCopy, Robocopy of externe hulpprogramma's en services. Dus ongeacht welke methode te strijden tegen de uitdaging initiële uploaden, is het wel belangrijk dat u volgt hieronder beschreven stappen voor het gebruik van deze hulpprogramma's in een synchronisatie compatibel manier.


## <a name="offline-data-transfer-benefits"></a>Voordelen van offline-gegevensoverdracht
De belangrijkste voordelen van offline migratie bij het gebruik van een Data Box zijn als volgt:

- Bij het migreren van bestanden naar Azure via een offline bulksgewijs overdracht-proces, zoals Data Box, bevindt u zich hoeft te uploaden van de bestanden van uw server via het netwerk. Voor grote naamruimten, kan dit betekenen dat aanzienlijke besparingen netwerkbandbreedte en-tijd.
- Als u Azure File Sync gebruiken, en vervolgens, ongeacht de modus van transport gebruikt (Data Box, Azure Import, enzovoort), uploadt uw live-server alleen de bestanden die zijn gewijzigd sinds u de gegevens naar Azure verzonden.
- Azure File Sync zorgt ervoor dat uw bestanden en mappen ACL's en - zijn gesynchroniseerd, zelfs als het product van de migratie offline bulksgewijs komt niet vervoer van ACL's.
- Wanneer u Azure Data Box en Azure File Sync, is er geen downtime. Gebruik Data Box voor het overbrengen van gegevens in Azure, kunt u efficiënt gebruik van netwerkbandbreedte behoud van de kwaliteit van het bestand. Daarnaast blijven zo uw naamruimte bijgewerkt door het uploaden van alleen de bestanden die zijn gewijzigd sinds de Data Box is verzonden.

## <a name="plan-your-offline-data-transfer"></a>Plan de overdracht van uw offline gegevens
Voordat u begint, controleert u de volgende informatie:

- Voltooi uw migratie bulksgewijs op een of meerdere Azure-bestandsshares voordat synchroniseren met Azure File Sync wordt ingeschakeld.
- Als u van plan bent te gebruiken van Data Box voor uw migratie bulksgewijs: Controleer de [implementatievereisten voor de Data Box](../../databox/data-box-deploy-ordered.md#prerequisites).
- Het plannen van de topologie van uw laatste Azure File Sync: [Een Azure File Sync-implementatie plannen](storage-sync-files-planning.md)
- Selecteer de Azure-opslagaccount (s) die u synchroniseren wilt met bestandsshares. Zorg ervoor dat de migratie van uw bulksgewijs gebeurt er met tijdelijke staging shares in hetzelfde Opslagaccount (s). Bulk-migratie kan alleen worden ingeschakeld met een laatste- en een staging-share die zich bevinden in hetzelfde opslagaccount.
- Een bulk-migratie kan alleen worden gebruikt wanneer u een nieuwe synchronisatierelatie met de locatie van de server maken. U kunt een bulk-migratie met een bestaande synchronisatierelatie niet inschakelen.

## <a name="offline-data-transfer-process"></a>Het overdrachtsproces offlinesynchronisatie van gegevens
In deze sectie beschrijft het proces van het instellen van Azure File Sync op een manier compatibel is met het hulpprogramma voor migratie van grote hoeveelheden, zoals Azure Data Box.

![Stappen die worden ook in detail uitgelegd in een volgende alinea visualiseren](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Stap | Detail |
|---|---------------------------------------------------------------------------------------|
| ![Stap 1 in het proces](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Uw Data Box bestellen](../../databox/data-box-deploy-ordered.md). Er zijn [verschillende aanbiedingen binnen de Data Box-familie](https://azure.microsoft.com/services/storage/databox/data) aan uw behoeften. Uw Data Box ontvangen en volgt u de Data Box [documentatie om uw gegevens te kopiëren](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box). Zorg ervoor dat de gegevens worden gekopieerd naar deze UNC-pad op de Data Box: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>` waar de `ShareName` is de naam van de staging-share. Data Box terug naar Azure verzenden. |
| ![Stap 2 in het proces](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Wacht totdat de bestanden weergegeven in de Azure-bestandsshares die u hebt opgegeven als tijdelijke staging shares. **Schakel niet synchroniseren met deze shares!** |
| ![Stap 3 in het proces](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Maak een nieuwe share die is leeg voor elke bestandsshare die Data Box voor u gemaakt. Zorg ervoor dat deze nieuwe share in hetzelfde opslagaccount als de Data Box-share. [Over het maken van een nieuwe Azure-bestandsshare](storage-how-to-create-file-share.md). |
| ![Stap 4 in het proces](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Maak een synchronisatiegroep](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) in een opslagsynchronisatieservice en naslaginformatie over de lege share als een cloudeindpunt. Herhaal deze stap voor elke bestandsshare Data Box. Controleer de [Azure File Sync implementeren](storage-sync-files-deployment-guide.md) begeleiden en volg de stappen voor het instellen van Azure File Sync. |
| ![Stap 5 in het proces](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [De map van uw live-server toevoegen als een servereindpunt](storage-sync-files-deployment-guide.md#create-a-server-endpoint). Geef in het proces dat u de bestanden al hebt verplaatst naar Azure en verwijzen naar de staging-shares. Het is uw keuze voor het in- of uitschakelen van cloudopslaglagen indien nodig. Tijdens het maken van een servereindpunt op uw live-server, moet u verwijzen naar de staging-share. Schakel 'Offline gegevensoverdracht' (onderstaande afbeelding) in de blade van de nieuwe server-eindpunt en verwijzen naar de staging-share die zich in hetzelfde opslagaccount als het cloudeindpunt bevinden moet. De lijst met beschikbare shares wordt gefilterd op storage-account en -shares die niet al worden gesynchroniseerd. |

![De Azure portal-gebruikersinterface voor het inschakelen van Offline gegevensoverdracht tijdens het maken van een nieuw servereindpunt te visualiseren.](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>De share synchroniseren
Nadat u uw eindpunt hebt gemaakt, wordt synchronisatie begint. Voor elk bestand dat op de server bestaat, wordt synchronisatie bepalen als dit bestand ook aanwezig in de staging-share waar de bestanden in Data Box worden gebracht en dus synchronisatie van het bestand worden gekopieerd van de staging-share in plaats van het uploaden van de server. Als het bestand niet in de staging-share bestaat of een nieuwere versie beschikbaar is op de lokale server, klikt u vervolgens upload synchronisatie het bestand van de lokale server.

> [!IMPORTANT]
> U kunt alleen de modus van de migratie bulksgewijs inschakelen tijdens het maken van een servereindpunt. Wanneer een servereindpunt is gemaakt, er is momenteel geen manier om te integreren van grote hoeveelheden gegevens hebt gemigreerd van een server al synchroniseren in de naamruimte.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL's en -timestamps voor bestanden en mappen
Azure File Sync zorgt ervoor dat bestands- en ACL's van de live-server worden gesynchroniseerd, zelfs als ACL's is niet in eerste instantie vervoer van het hulpprogramma voor de migratie van grote hoeveelheden die is gebruikt. Dit betekent dat de staging-share mag niet bevatten ACL's voor bestanden en mappen. Wanneer u de migratiefunctie voor offlinesynchronisatie van gegevens bij het maken van een nieuw servereindpunt inschakelt, wordt de ACL's worden gesynchroniseerd op dat moment voor alle bestanden op de server. Hetzelfde geldt voor Maak - en gewijzigd-tijdstempels.

## <a name="shape-of-the-namespace"></a>Vorm van de naamruimte
De vorm van de naamruimte wordt bepaald door het zich op de server als de synchronisatie is ingeschakeld. Als bestanden op de lokale server worden verwijderd nadat de Data Box "-momentopname" en - migratie, en vervolgens deze bestanden wordt niet worden geregistreerd voor de naamruimte live, synchroniseert. Ze worden nog steeds in de staging-share maar nooit gekopieerd. Dat is het gewenste gedrag als synchronisatie houdt de naamruimte op basis van de live-server. De Data Box 'snapshot' is slechts een gefaseerde installatie volledig is ontwikkeld voor het kopiëren van een doeltreffende bestand en niet de instantie voor de vorm van de live-naamruimte.

## <a name="finishing-bulk-migration-and-clean-up"></a>Bulksgewijs migratie en opschonen is voltooid
De onderstaande schermafbeelding ziet u de blade van de server eindpunt-eigenschappen in de Azure-portal. U kunt de status van het proces te zien in de sectie offline gegevensoverdracht. Er wordt een 'Wordt uitgevoerd' of 'Voltooid' weergegeven.

Nadat de server is voltooid voor de initiële synchronisatie van de gehele naamruimte, deze wordt klaar bent met het gebruik te maken van de staging-bestand delen met de Data Box-bulksgewijs bestanden gemigreerd. Bekijk in de eigenschappen van servereindpunt voor offline gegevensoverdracht die de status wordt gewijzigd in 'Voltooid'. Op dit moment kunt u de faserings-share om kosten te besparen opschonen:

1. Druk op 'Uitschakelen offline gegevensoverdracht' in de eigenschappen van servereindpunt, wanneer de status 'voltooid'.
2. Houd rekening met het verwijderen van de staging-share om op te slaan kosten. De staging-share is het onwaarschijnlijk dat bestands- en ACL's bevatten en als zodanig is beperkt. Voor back-up 'punt in tijd' doeleinden, maakt u in plaats daarvan een reëel getal [momentopname van de synchroniseert Azure-bestandsshare](storage-snapshots-files.md). U kunt [back-up van Azure om momentopnamen te inschakelen]( ../../backup/backup-azure-files.md) volgens een schema.

![Het visualiseren van de Azure portal-gebruikersinterface voor eigenschappen van servereindpunt waar de besturingselementen voor status- en uitschakelen voor het Offline gegevensoverdracht zich bevinden.](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

U moet deze modus alleen uitschakelen wanneer de status 'voltooid' of u echt wilt afbreken vanwege een onjuiste configuratie. Als u de modus halverwege om een legitieme implementatie wordt uitgeschakeld, worden bestanden wordt gestart voor het uploaden van de server, zelfs als uw staging bestandsshare nog steeds beschikbaar is.

> [!IMPORTANT]
> Nadat u offline gegevensoverdracht uitgeschakeld is er geen manier enigerlei, zelfs als de staging-share van de bulk-migratie nog steeds beschikbaar is.

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
