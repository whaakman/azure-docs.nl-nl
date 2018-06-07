---
title: Status van de Azure Import/Export-taken weergeven | Microsoft Docs
description: Informatie over het weergeven van de status van taken voor importeren/exporteren en de stations die worden gebruikt.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 176cbf190b6442682a222eb4f24b6583fb87a46b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660808"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>De status van de Azure Import/Export-taken weergeven

In dit artikel bevat informatie over het weergeven van het station en de status voor Azure Import/Export-taken. Azure Import/Export-service wordt gebruikt voor het veilig grote hoeveelheden gegevens overdragen naar Azure Blobs en Azure-bestanden. De service wordt ook gebruikt om gegevens te exporteren uit Azure Blob-opslag.  

## <a name="view-job-and-drive-status"></a>Status van taak en het station weergeven
U kunt de status van de importbewerking volgen of taken exporteren vanuit de Azure-portal. Klik op de **voor importeren/exporteren** tabblad. Een lijst met uw taken wordt weergegeven op de pagina.

![Status van de taak weergeven](./media/storage-import-export-service/jobstate.png)


## <a name="view-job-status"></a>Taakstatus weergeven

U ziet een van de volgende statussen van de taak afhankelijk van waar de schijf in het proces is.

| Taakstatus | Beschrijving |
|:--- |:--- |
| Maken | Nadat een taak is gemaakt, de status is ingesteld op **maken**. Terwijl de taak wordt de **maken** staat, de Import/Export-service wordt ervan uitgegaan dat de schijven nog niet zijn verzonden naar het datacenter. Een taak kan in deze status blijven tot twee weken, waarna deze worden automatisch verwijderd door de service. |
| Verzenden | Nadat u uw pakket verzendt, moet u de controle-informatie in de Azure portal bijwerken.  Hiermee schakelt u de taak in **back-upfunctie** status. De taak blijft in de **back-upfunctie** staat zijn voor maximaal twee weken. 
| Ontvangen | Nadat alle stations zijn ontvangen in het datacenter, de taakstatus is ingesteld op **ontvangen**. |
| Overbrengen | Nadat ten minste één station is begonnen verwerking, de taakstatus is ingesteld op **overdragen**. Ga voor meer informatie naar [station statussen](#view-drive-status). |
| Verpakken | Nadat alle stations verwerking is voltooid, de taak wordt geplaatst **verpakking** status totdat de stations worden verzonden naar u terug. |
| Voltooid | Nadat alle stations worden verzonden voor u, als de taak is voltooid zonder fouten, wordt de taak ingesteld op **voltooid**. De taak wordt automatisch verwijderd na 90 dagen in de **voltooid** status. |
| Gesloten | Nadat alle stations worden verzonden voor u, als er fouten tijdens de verwerking van de taak zijn, de taak is ingesteld op **gesloten**. De taak wordt automatisch verwijderd na 90 dagen in de **gesloten** status. |

## <a name="view-drive-status"></a>Status van station weergeven

De volgende tabel bevat de levenscyclus van een afzonderlijke schijf als deze door middel van een taak worden geïmporteerd of geëxporteerd overgezet. De huidige status van elke schijf in een taak wordt weergegeven in de Azure portal.

De volgende tabel beschrijft elke status die elk station in een taak kan doorgeven.

| Status van station | Beschrijving |
|:--- |:--- |
| Opgegeven | Voor een import-taak wanneer de taak is gemaakt vanuit de Azure-portal wordt de initiële status voor een station is **opgegeven**. Voor een exporttaak omdat er geen station wordt opgegeven wanneer de taak is gemaakt, wordt de status van de initiële station is **ontvangen**. |
| Ontvangen | Het station wordt overgezet naar de **ontvangen** status wanneer de Import/Export-service de stations die zijn ontvangen van het bedrijf back-upfunctie voor een import-taak is verwerkt. Voor een exporttaak de status van de initiële station is het **ontvangen** status. |
| NeverReceived | Het station wordt verplaatst naar de **NeverReceived** status wanneer het pakket voor een taak binnenkomt, maar het pakket niet het station bevat. Een station wordt ook deze status als deze twee weken is sinds de service heeft ontvangen van de back-ups van gegevens, maar het pakket nog niet in het datacenter ontvangen is. |
| Overbrengen | Een station wordt verplaatst naar de **overdragen** status wanneer de service start de gegevens uit het station overdraagt naar Azure Storage. |
| Voltooid | Een station wordt verplaatst naar de **voltooid** status wanneer de service heeft de gegevens zonder fouten is overgedragen.
| CompletedMoreInfo | Een station wordt verplaatst naar de **CompletedMoreInfo** status wanneer de service enkele problemen is opgetreden tijdens het kopiëren van gegevens van of naar het station. De informatie kan bestaan fouten, waarschuwingen of informatieve berichten over het overschrijven van blobs.
| ShippedBack | Een station wordt verplaatst naar de **ShippedBack** status wanneer deze van het datacenter terug naar het retouradres is verzonden. |

Deze installatiekopie van de Azure-portal wordt de status van de schijf van de taak voor een voorbeeld weergegeven:

![Status van station weergeven](./media/storage-import-export-service/drivestate.png)

De volgende tabel beschrijft de statussen van de fout station en de acties die voor elke status.

| Status van station | Gebeurtenis | Resolutie / de volgende stap |
|:--- |:--- |:--- |
| NeverReceived | Een station dat is gemarkeerd als **NeverReceived** (omdat deze niet is ontvangen als onderdeel van de verzending van de taak) in een andere verzending binnenkomt. | Het operationele team verplaatst het station om **ontvangen**. |
| N/A | Een station dat geen deel uitmaakt van elke taak komt in het datacenter als onderdeel van een andere taak. | Het station is gemarkeerd als een extra schijf en u wordt geretourneerd wanneer de taak die is gekoppeld aan het oorspronkelijke pakket is voltooid. |

## <a name="time-to-process-job"></a>Tijd tot verwerkingstaak
De hoeveelheid tijd die nodig is voor het verwerken van een taak van import/export varieert op basis van een aantal factoren, zoals:

-  Verzendtijd
-  Laden in het datacenter
-  Taaktype en grootte van de gegevens worden gekopieerd
-  Het aantal schijven in een taak. 

Import/Export-service beschikt niet over een SLA, maar de service wil de kopie in 7 tot 10 dagen na van de schijven ontvangst te voltooien. Naast de status die wordt gepost op Azure-Portal, kan de REST-API's worden gebruikt om de voortgang van de taak te volgen. Het percentage voltooid parameter in de [lijst taken]() bewerking API-aanroep geeft de voortgang van de kopie percentage.


## <a name="next-steps"></a>Volgende stappen

* [Het hulpprogramma WAImportExport instellen](storage-import-export-tool-how-to.md)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Azure Import exporteren REST-API-voorbeeld](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

