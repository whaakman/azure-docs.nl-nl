---
title: Azure-Stack Public Key Infrastructure certificaten voorbereiden voor Azure-Stack geïntegreerd systemen implementatie | Microsoft Docs
description: Beschrijft hoe de Azure-Stack PKI-certificaten voorbereiden voor Azure-Stack geïntegreerd systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 934585082e2832c41885874c82ab43d64a1fa361
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure-Stack PKI-certificaten voor de implementatie voorbereiden
De certificaatbestanden [verkregen van uw Certificeringsinstantie van keuze](azure-stack-get-pki-certs.md) moet worden geïmporteerd en geëxporteerd met eigenschappen die overeenkomt met de Azure-Stack-certificaatvereisten.


## <a name="prepare-certificates-for-deployment"></a>Certificaten voor de implementatie voorbereiden
Volg deze stappen voor het voorbereiden en valideren van de Azure-Stack PKI-certificaten: 

### <a name="import-the-certificate"></a>Het certificaat importeren

1.  Kopieer het oorspronkelijke certificaat versies [verkregen van uw Certificeringsinstantie van keuze](azure-stack-get-pki-certs.md) naar een map op de host voor de implementatie. 
  > [!WARNING]
  > Kopieer niet bestanden die al zijn geïmporteerd, geëxporteerd of gewijzigd op een manier van de bestanden die rechtstreeks door de CA worden geleverd.

2.  Met de rechtermuisknop op het certificaat en selecteer **certificaat installeren** of **PFX installeren** afhankelijk van hoe het certificaat van uw Certificeringsinstantie is geleverd.

3. In de **Wizard Certificaat importeren**, selecteer **lokale Machine** als locatie voor de import. Selecteer **Volgende**. In het volgende scherm, klikt u op volgende opnieuw.

    ![Locatie van de lokale machine importeren](.\media\prepare-pki-certs\1.png)

4.  Kies **plaats alle in het onderstaande archief certificaten** en selecteer vervolgens **ondernemingsvertrouwen** als de locatie. Klik op **OK** om het dialoogvenster Certificaat store selectie te sluiten en vervolgens **volgende**.

    ![Het certificaatarchief configureren](.\media\prepare-pki-certs\3.png)

    a. Als u een PFX importeert u krijgt een extra dialoogvenster. Op de **beveiliging voor persoonlijke sleutels** pagina, voer het wachtwoord voor uw certificaatbestanden en schakel vervolgens de **deze sleutel als exporteerbaar markeren. Hiermee kunt u back-up maken of de sleutels op een later tijdstip verplaatsen** optie. Selecteer **Volgende**.

    ![Sleutel als exporteerbaar markeren](.\media\prepare-pki-certs\2.png)

5. Klik op Voltooien om de importeren te voltooien.

### <a name="export-the-certificate"></a>Het certificaat exporteren

Open MMC Certificate Manager-console en maak verbinding met het certificaatarchief van de lokale computer.

1. Open de Microsoft Management Console, in Windows 10-Klik met de rechtermuisknop op het Menu Start en klik op uitvoeren. Type **mmc** Klik op ok.

2. Klik op bestand, module toevoegen/verwijderen en selecteer certificaten klikt u op toevoegen.

    ![Module Certificaten toevoegen](.\media\prepare-pki-certs\mmc-2.png)
 
3. Computeraccount selecteren, klikt u op volgende lokale computer selecteren vervolgens voltooien. Klik op ok om de pagina module toevoegen/verwijderen te sluiten.

    ![Module Certificaten toevoegen](.\media\prepare-pki-certs\mmc-3.png)

4. Blader naar certificaten > ondernemingsvertrouwen > locatie van het certificaat. Controleer of u uw certificaat zien aan de rechterkant.

5. Selecteer in de taak balk van Certificate Manager-console **acties** > **alle taken** > **exporteren**. Selecteer **Volgende**.

  > [!NOTE]
  > Afhankelijk van hoeveel Azure Stack moet certificaten die u hebt u mogelijk meer dan één keer dit proces te voltooien.

4. Selecteer **Ja, de persoonlijke sleutel exporteren**, en klik vervolgens op **volgende**.

5. Selecteer in de sectie bestandsindeling voor Export **alle uitgebreide eigenschappen exporteren** en klik vervolgens op **volgende**.

6. Selecteer **wachtwoord** en een wachtwoord opgeven voor de certificaten. Onthoud dit wachtwoord omdat deze wordt gebruikt als een implementatieparameter. Selecteer **Volgende**.

7. Kies een bestandsnaam en locatie voor het pfx-bestand exporteren. Selecteer **Volgende**.

8. Selecteer **Voltooien**.

## <a name="next-steps"></a>Volgende stappen
[Valideren van PKI-certificaten](azure-stack-validate-pki-certs.md)