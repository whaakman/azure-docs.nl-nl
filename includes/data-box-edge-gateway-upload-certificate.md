---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/26/2019
ms.author: alkohli
ms.openlocfilehash: 09d9b5bbf3f9ca7a4eef37891d03c9c865e7f74b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448632"
---
Een juiste SSL-certificaat zorgt ervoor dat u versleutelde gegevens naar de juiste server verzendt. Naast de versleuteling kan het certificaat ook voor verificatie. U kunt uw eigen vertrouwde SSL-certificaat via de PowerShell-interface van het apparaat uploaden.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Set-HcsCertificate` cmdlet om het certificaat te uploaden. Als u wordt gevraagd, geeft u de volgende parameters:

   - `CertificateFilePath` -Pad naar de share met het certificaatbestand in *pfx* indeling.
   - `CertificatePassword` -Er is een wachtwoord gebruikt voor het beveiligen van het certificaat.
   - `Credentials` -Gebruikersnaam voor toegang tot de share met het certificaat. Geef het wachtwoord voor de netwerkshare wanneer hierom wordt gevraagd.

     Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username"
     ```

