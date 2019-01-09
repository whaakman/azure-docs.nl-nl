---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550283"
---
1. Meld u aan op het Data Box-apparaat. Zorg ervoor dat het apparaat is ontgrendeld.

    ![Data Box-dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Ga naar **Netwerkinterfaces instellen**. Noteer het IP-adres van het apparaat voor de netwerkinterface die wordt gebruikt voor verbinding met de client.

    ![Data Box-dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Ga naar **Verbinding maken en kopiëren** en klik op **Rest (Preview)**.

    ![Data Box-dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Kopieer in het dialoogvenster **Opslagaccount openen en gegevens uploaden** het **eindpunt van de blob-service**.

    ![Data Box-dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Start **Kladblok** als beheerder en open vervolgens het bestand met **hosts** dat zich bevindt in `C:\Windows\System32\Drivers\etc`.
6. Voeg de volgende vermelding toe aan uw bestand met **hosts**: `<device IP address> <Blob service endpoint>`
7. Gebruik de volgende afbeelding ter referentie. Sla het bestand met **hosts** op.

    ![Data Box-dashboard](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
