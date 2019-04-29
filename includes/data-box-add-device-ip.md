---
author: WenJason
ms.service: databox
ms.topic: include
origin.date: 12/07/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60728276"
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
