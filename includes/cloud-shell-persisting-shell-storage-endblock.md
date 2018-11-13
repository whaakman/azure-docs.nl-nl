---
author: cynthn
ms.service: azure
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: c2ed33aea77b5478e8d17f6bd0213ef3e778b806
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572009"
---
## <a name="transfer-local-files-to-cloud-shell"></a>Lokale bestanden overbrengen naar de Cloud Shell
De `clouddrive` directory wordt gesynchroniseerd met de opslag van Azure portal-blade. Gebruik deze blade om over te dragen van lokale bestanden naar of van de bestandsshare. Bijwerken van de bestanden in Cloud Shell wordt doorgevoerd in de file storage GUI bij het vernieuwen van de blade.

### <a name="download-files"></a>Bestanden downloaden

![Lijst met lokale bestanden](../articles/cloud-shell/media/persisting-shell-storage/download.png)
1. In de Azure-portal, gaat u naar de gekoppelde bestandsshare.
2. Selecteer het doelbestand.
3. Selecteer de **downloaden** knop.

### <a name="upload-files"></a>Bestanden uploaden

![Lokale bestanden worden geüpload](../articles/cloud-shell/media/persisting-shell-storage/upload.png)
1. Ga naar de gekoppelde bestandsshare.
2. Selecteer de knop **Uploaden**.
3. Selecteer het bestand of de bestanden die u wilt uploaden.
4. Controleer of het uploaden.

U ziet nu de bestanden die toegankelijk zijn in uw `clouddrive` Active directory in Cloud Shell.