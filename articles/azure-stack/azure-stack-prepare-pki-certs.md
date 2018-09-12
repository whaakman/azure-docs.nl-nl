---
title: Azure Stack Public Key Infrastructure certificaten voorbereiden voor de implementatie van de geïntegreerde Azure Stack-systemen | Microsoft Docs
description: Beschrijft hoe u de Azure Stack PKI-certificaten voorbereiden voor Azure Stack-geïntegreerde systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: ef9fe0e05343f9c99656634a075b1bd464a13c7e
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379584"
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure Stack PKI-certificaten voorbereiden voor implementatie
De certificaatbestanden [verkregen van uw Certificeringsinstantie keuze](azure-stack-get-pki-certs.md) moet worden geïmporteerd en geëxporteerd met eigenschappen die overeenkomen met de Azure Stack-certificaatvereisten.


## <a name="prepare-certificates-for-deployment"></a>Certificaten voorbereiden voor implementatie
Volg deze stappen voor het voorbereiden en valideren van de Azure Stack PKI-certificaten: 

### <a name="import-the-certificate"></a>Het certificaat importeren

1.  Het oorspronkelijke certificaat versies kopiëren [verkregen van uw Certificeringsinstantie keuze](azure-stack-get-pki-certs.md) in een map op de host van de implementatie. 
  > [!WARNING]
  > Kopieer bestanden die al zijn geïmporteerd, geëxporteerd of gewijzigd op geen enkele manier uit de bestanden rechtstreeks geleverd door de CA niet.

1.  Met de rechtermuisknop op het certificaat en selecteer **certificaat installeren** of **PFX installeren** , afhankelijk van hoe het certificaat van uw Certificeringsinstantie is geleverd.

1. In de **Wizard Certificaat importeren**, selecteer **lokale Machine** als importlocatie voor de. Selecteer **Volgende**. Klik op het volgende scherm op vervolgens opnieuw.

    ![Locatie van de lokale machine importeren](.\media\prepare-pki-certs\1.png)

1.  Kies **alle certificaten in het onderstaande archief opslaan** en selecteer vervolgens **ondernemingsvertrouwen** als de locatie. Klik op **OK** te sluiten van het dialoogvenster Certificaat store selecteren en vervolgens **volgende**.

    ![Het certificaatarchief configureren](.\media\prepare-pki-certs\3.png)

    a. Als u een PFX importeert, u krijgt een dialoogvenster met een extra. Op de **beveiliging met persoonlijke sleutel** pagina, typ het wachtwoord voor de certificaatbestanden van uw en schakel vervolgens de **deze sleutel als exporteerbaar markeren. Hiermee kunt u een back-up maken of ze uw sleutels op een later tijdstip** optie. Selecteer **Volgende**.

    ![Sleutel als exporteerbaar markeren](.\media\prepare-pki-certs\2.png)

1. Klik op Voltooien om het importeren te voltooien.

### <a name="export-the-certificate"></a>Het certificaat exporteren

Open MMC Certificate Manager-console en maak verbinding met het certificaatarchief van lokale computer.

1. Open de Microsoft Management Console, in Windows 10 Klik met de rechtermuisknop op het Menu Start en klik op uitvoeren. Type **mmc** Klik op ok.

1. Klik op bestand, klik op toevoegen module toevoegen/verwijderen en selecteer certificaten.

    ![Module Certificaten toevoegen](.\media\prepare-pki-certs\mmc-2.png)
 
1. Computeraccount selecteren, klikt u op volgende en vervolgens selecteert u lokale computer en voltooien. Klik op ok om de pagina module toevoegen/verwijderen te sluiten.

    ![Module Certificaten toevoegen](.\media\prepare-pki-certs\mmc-3.png)

1. Blader naar certificaten > ondernemingsvertrouwen > locatie van het certificaat. Controleer of dat u het certificaat aan de rechterkant ziet.

1. Selecteer in de taak balk van Certificate Manager-console **acties** > **alle taken** > **exporteren**. Selecteer **Volgende**.

  > [!NOTE]
  > Certificaten die u hebt u mogelijk meer dan één keer dit proces te voltooien, afhankelijk van hoeveel Azure Stack.

1. Selecteer **Ja, de persoonlijke sleutel exporteren**, en klik vervolgens op **volgende**.

1. Selecteer in de sectie bestandsindeling voor Export **alle uitgebreide eigenschappen exporteren** en klik vervolgens op **volgende**.

1. Selecteer **wachtwoord** en een wachtwoord opgeven voor de certificaten. Onthoud dit wachtwoord omdat deze wordt gebruikt als een implementatieparameter. Selecteer **Volgende**.

1. Kies een bestandsnaam en locatie voor het pfx-bestand te exporteren. Selecteer **Volgende**.

1. Selecteer **Voltooien**.

## <a name="next-steps"></a>Volgende stappen
[Valideren van PKI-certificaten](azure-stack-validate-pki-certs.md)