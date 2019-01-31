---
title: Status van de Azure Import/Export-taken bekijken | Microsoft Docs
description: Informatie over het weergeven van de status van Import/Export-taken en de stations die worden gebruikt.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 225164fe00f70839446f8b74155cd3959f745a49
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55467744"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>De status van de Azure Import/Export-taken weergeven

In dit artikel bevat informatie over hoe u de status van het station en de taak voor de Azure Import/Export-taken weergeven. Azure Import/Export-service wordt gebruikt om veilig grote hoeveelheden gegevens overdragen naar Azure-Blobs en Azure Files. De service wordt ook gebruikt voor het exporteren van gegevens uit Azure Blob storage.  

## <a name="view-job-and-drive-status"></a>Status van de taak en het station weergeven
U kunt de status van de importbewerking volgen of taken exporteren vanuit Azure portal. Klik op de **Import/Export** tabblad. Een lijst van uw taken wordt weergegeven op de pagina.

![Status van de taak weergeven](./media/storage-import-export-service/jobstate.png)

## <a name="view-job-status"></a>Taakstatus weergeven

U ziet een van de volgende status van een taak, afhankelijk van waar de schijf zich in het proces bevindt.

| Taakstatus | Description |
|:--- |:--- |
| Maken | Nadat een taak is gemaakt, de status is ingesteld op **maken**. Terwijl de taak wordt de **maken** staat, wordt de Import/Export-service wordt ervan uitgegaan dat de schijven niet zijn verzonden naar het datacenter. Een taak kan in deze status blijven tot twee weken, waarna deze worden automatisch verwijderd door de service. |
| Verzenden | Nadat u het pakket te verzenden, moet u de controle-informatie in de Azure-portal bijwerken.  Hiermee schakelt u de taak in **verzending** staat. De taak blijft in de **verzending** staat voor maximaal twee weken. 
| Ontvangen | Nadat alle stations zijn ontvangen in het datacenter, de taakstatus is ingesteld op **ontvangen**. |
| Overbrengen | Nadat ten minste één station is begonnen verwerking, de taakstatus is ingesteld op **overdragen**. Ga voor meer informatie naar [station Staten](#view-drive-status). |
| Verpakken | Nadat alle stations verwerking is voltooid, de taak wordt geplaatst **verpakking** status totdat de schijven terug naar u worden verzonden. |
| Voltooid | Nadat alle stations worden verzonden naar u, als de taak is voltooid zonder fouten, wordt de taak ingesteld op **voltooid**. De taak wordt automatisch verwijderd na 90 dagen in de **voltooid** staat. |
| Gesloten | Nadat alle stations worden verzonden naar u, als er fouten tijdens de verwerking van de taak zijn, de taak is ingesteld op **gesloten**. De taak wordt automatisch verwijderd na 90 dagen in de **gesloten** staat. |

## <a name="view-drive-status"></a>Status van station weergeven

De volgende tabel beschrijft de levenscyclus van een afzonderlijke station als het overgangen via een taak importeren of exporteren. De huidige status van elke schijf in een taak wordt weergegeven in de Azure-portal.

De volgende tabel beschrijft elke status die elke schijf in een taak kan passeren.

| Status van station | Description |
|:--- |:--- |
| Opgegeven | Voor een importtaak wanneer de taak wordt gemaakt in Azure portal, wordt de initiële status voor een station is **opgegeven**. Voor een exporttaak bekijken, omdat er geen station is opgegeven wanneer de taak is gemaakt, wordt de status van de eerste schijf is **ontvangen**. |
| Ontvangen | Het station verandert in de **ontvangen** status wanneer de Import/Export-service de stations die zijn ontvangen van het transportbedrijf voor een importtaak heeft verwerkt. Voor een exporttaak bekijken, de status van de eerste schijf is de **ontvangen** staat. |
| NeverReceived | Het station wordt verplaatst naar de **NeverReceived** status wanneer het pakket voor een taak wordt ontvangen, maar het pakket niet het station bevat. Een station wordt ook in deze staat verplaatst als dit twee weken is, omdat de service heeft de verzendinformatie ontvangen, maar het pakket nog niet in het datacenter ontvangen is. |
| Overbrengen | Een station wordt verplaatst naar de **overdragen** status wanneer begint de service gegevens uit het station overdragen naar Azure Storage. |
| Voltooid | Een station wordt verplaatst naar de **voltooid** status wanneer de service is de gegevens zonder fouten is overgedragen.
| CompletedMoreInfo | Een station wordt verplaatst naar de **CompletedMoreInfo** status wanneer de service enkele problemen is opgetreden tijdens het kopiëren van gegevens van of naar het station. De informatie kan bestaan uit fouten, waarschuwingen of informatieve berichten over het overschrijven van blobs.
| ShippedBack | Een station wordt verplaatst naar de **ShippedBack** status wanneer deze vanuit het datacenter terug naar het retouradres is verzonden. |

Deze installatiekopie vanuit Azure portal wordt de status van het station van de taak voor een voorbeeld weergegeven:

![Status van station weergeven](./media/storage-import-export-service/drivestate.png)

De volgende tabel beschrijft de statussen van de fout station en de acties die voor elke status.

| Status van station | Gebeurtenis | Oplossing / volgende stap |
|:--- |:--- |:--- |
| NeverReceived | Een station dat is gemarkeerd als **NeverReceived** (omdat deze niet is ontvangen als onderdeel van de verzending van de taak) binnenkomt in een andere verzending. | Het operationele team verplaatst het station **ontvangen**. |
| N/A | Een schijf die geen deel uitmaakt van een taak wordt ontvangen in het datacenter als onderdeel van een andere taak. | Het station is gemarkeerd als een extra station en aan u wordt geretourneerd wanneer de taak die is gekoppeld aan het oorspronkelijke pakket is voltooid. |

## <a name="time-to-process-job"></a>Tijd voor het procestaak
De hoeveelheid tijd die nodig is voor het verwerken van een import-/ exporttaak varieert op basis van een aantal factoren zoals:

-  Verzendtijd
-  Laden in het datacenter
-  Type taak en de grootte van de gegevens worden gekopieerd
-  Het aantal schijven in een taak. 

Import/Export-service beschikt niet over een SLA, maar de service streeft ernaar voor het voltooien van de kopie in 7 tot en met 10 dagen nadat de schijven zijn ontvangen. Naast de status geplaatst in Azure Portal, kan de REST-API's worden gebruikt voor het bijhouden van de voortgang van de taak. Het percentage voltooid-parameter in de [lijst met taken](/previous-versions/azure/dn529083(v=azure.100)) API-bewerkingsaanroep bevat het percentage exemplaar wordt uitgevoerd.


## <a name="next-steps"></a>Volgende stappen

* [Instellen van het hulpprogramma WAImportExport](storage-import-export-tool-how-to.md)
* [Gegevens overdragen met AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Voorbeeld van Azure Import Export REST-API](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
