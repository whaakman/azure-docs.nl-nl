---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 6911e06dc023027ab32b99387b9f7d3f5e708f86
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164637"
---
## <a name="configure-your-application-to-access-azure-storage"></a>Uw toepassing configureren voor toegang tot Azure Storage
Er zijn twee manieren om uw toepassing voor toegang tot opslagservices te verifiëren:

* Gedeelde sleutel: Gedeelde sleutel gebruiken alleen voor testdoeleinden
* Shared Access Signature (SAS): Gebruik SAS voor productie-Apps

### <a name="shared-key"></a>Gedeelde sleutel
Gedeelde sleutelverificatie betekent dat uw toepassing uw accountnaam en accountsleutel wordt gebruikt voor toegang tot opslagservices. Voor de toepassing van het snel weergeven van het gebruik van deze bibliotheek, gebruiken we gedeelde sleutelverificatie in deze aan de slag.

> [!WARNING] 
> **Gedeelde sleutelverificatie alleen gebruiken voor testdoeleinden.** Uw accountnaam en accountsleutel, die volledig lezen/schrijven toegang tot het Opslagaccount dat is gekoppeld verleent, worden gedistribueerd naar elke persoon die uw app kan worden gedownload. Dit is **niet** een goede oefening als u het risico dat de sleutel in gevaar komt door niet-vertrouwde clients.
> 
> 

Bij het gebruik van gedeelde sleutelverificatie, maakt u een [verbindingsreeks](../articles/storage/common/storage-configure-connection-string.md). De verbindingsreeks bestaat uit:  

* De **DefaultEndpointsProtocol** -kunt u HTTP of HTTPS. Met behulp van HTTPS wordt echter ten zeerste aanbevolen.
* De **accountnaam** -de naam van uw storage-account
* De **Accountsleutel** : Voer op de [Azure Portal](https://portal.azure.com), navigeer naar uw storage-account en klikt u op de **sleutels** pictogram om deze informatie te vinden.
* (Optioneel) **EndpointSuffix** -deze wordt gebruikt voor storage-services in de regio's met een ander eindpunt achtervoegsels, zoals Azure China of Azure Governance.

Hier volgt een voorbeeld van de connection string met behulp van gedeelde sleutelverificatie:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Shared Access Signatures (SAS)
Voor een mobiele toepassing is de aanbevolen methode voor het verifiëren van een aanvraag door een client op basis van de service Azure Storage met behulp van een Shared Access Signature (SAS). SAS kunt u een clienttoegang verlenen tot een bron voor een opgegeven periode, met een opgegeven set machtigingen.
Als eigenaar van het opslagaccount moet u een SAS genereren voor uw mobiele clients om te gebruiken. Voor het genereren van de SAS, wilt u waarschijnlijk een afzonderlijke service die wordt gegenereerd van de SA's worden gedistribueerd naar uw clients te schrijven. Voor testdoeleinden kunt u de [Microsoft Azure Storage Explorer](http://storageexplorer.com) of de [Azure Portal](https://portal.azure.com) voor het genereren van een SAS. Wanneer u de SAS hebt gemaakt, geeft u het tijdsinterval op waarover de SAS geldig is en de machtigingen die de SAS verleent aan de client.

Het volgende voorbeeld ziet hoe u kunt de Microsoft Azure Storage Explorer gebruiken voor het genereren van een SAS.

1. Als u dat nog niet gedaan hebt, [installeren van de Microsoft Azure Storage Explorer](http://storageexplorer.com)
2. Maak verbinding met uw abonnement.
3. Klik op uw Storage-account en klik op het tabblad 'Acties' in de linkerbenedenhoek. Klik op 'Shared Access Signature ophalen' voor het genereren van een "connection string" voor de SAS.
4. Hier volgt een voorbeeld van een SAS-verbindingsreeks dat verleent machtigingen lezen en op de service, container en objectniveau voor de blob-service van het opslagaccount schrijven.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Zoals u zien kunt, wanneer u een SAS, bent u niet uw accountsleutel weergeeft in uw toepassing. U kunt meer informatie over SAS en aanbevolen procedures voor het gebruik van SAS door uit te checken [Shared Access Signatures: inzicht in het SAS-model](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

