---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 216380cf7069468d13c4e533fc90b2596aa211c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125128"
---
Een juiste SSL-certificaat zorgt ervoor dat u versleutelde gegevens naar de juiste server verzendt. Naast de versleuteling kan het certificaat ook voor verificatie. U kunt uw eigen vertrouwde SSL-certificaat via de PowerShell-interface van het apparaat uploaden.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Set-HcsCertificate` cmdlet om het certificaat te uploaden. Als u wordt gevraagd, geeft u de volgende parameters:

   - `CertificateFilePath` -Pad naar de share met het certificaatbestand in *pfx* indeling.
   - `CertificatePassword` -Er is een wachtwoord gebruikt voor het beveiligen van het certificaat.
   - `Credentials` -Gebruikersnaam en wachtwoord voor toegang tot de share met het certificaat.

     Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credentials "Username/Password"
     ```

