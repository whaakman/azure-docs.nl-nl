---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: d7a9923d5bd9e357bcd75fae6e0a7d1bcd437a53
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161198"
---
Een juiste SSL-certificaat zorgt ervoor dat u versleutelde gegevens naar de juiste server verzendt. Naast de versleuteling kan het certificaat ook voor verificatie. U kunt uw eigen vertrouwde SSL-certificaat via de PowerShell-interface van het apparaat uploaden.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Set-HcsCertificate` cmdlet om het certificaat te uploaden. Als u wordt gevraagd, geeft u de volgende parameters:

   - `CertificateFilePath` -Pad naar de share met het certificaatbestand in *pfx* indeling.
   - `CertificatePassword` -Er is een wachtwoord gebruikt voor het beveiligen van het certificaat.
   - `Credentials` -Gebruikersnaam en wachtwoord voor toegang tot de share met het certificaat.

     Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username/Password"
     ```

