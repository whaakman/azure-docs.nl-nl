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
ms.openlocfilehash: dadb443f8b7739e3a18c0d3beb558d8c42e9d19c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure-Stack PKI-certificaten voor de implementatie voorbereiden
De certificaatbestanden [verkregen van uw Certificeringsinstantie van keuze](azure-stack-get-pki-certs.md) moet worden geïmporteerd en geëxporteerd met eigenschappen die overeenkomt met de Azure-Stack-certificaatvereisten.


## <a name="prepare-certificates-for-deployment"></a>Certificaten voor de implementatie voorbereiden
Volg deze stappen voor het voorbereiden en valideren van de Azure-Stack PKI-certificaten: 

1.  Kopieer het oorspronkelijke certificaat versies [verkregen van uw Certificeringsinstantie van keuze](azure-stack-get-pki-certs.md) naar een map op de host voor de implementatie. 
  > [!WARNING]
  > Kopieer niet bestanden die al zijn geïmporteerd, geëxporteerd of gewijzigd op een manier van de bestanden die rechtstreeks door de CA worden geleverd.

2.  Importeer de certificaten naar het certificaatarchief van de lokale computer:

    a.  Met de rechtermuisknop op het certificaat en selecteer **PFX installeren**.

    b.  In de **Wizard Certificaat importeren**, selecteer **lokale Machine** als locatie voor de import. Selecteer **Volgende**.

    ![Locatie van de lokale machine importeren](.\media\prepare-pki-certs\1.png)

    c.  Selecteer **volgende** op de **Kies te importeren bestand** pagina.

    d.  Op de **beveiliging voor persoonlijke sleutels** pagina, voer het wachtwoord voor uw certificaatbestanden en schakel vervolgens de **deze sleutel als exporteerbaar markeren. Hiermee kunt u back-up maken of de sleutels op een later tijdstip verplaatsen** optie. Selecteer **Volgende**.

    ![Sleutel als exporteerbaar markeren](.\media\prepare-pki-certs\2.png)

    e.  Kies **plaats alle in het onderstaande archief certificaten** en selecteer vervolgens **ondernemingsvertrouwen** als de locatie. Klik op **OK** om het dialoogvenster Certificaat store selectie te sluiten en vervolgens **volgende**.

    ![Het certificaatarchief configureren](.\media\prepare-pki-certs\3.png)

  f.    Klik op **voltooien** voltooid de Wizard Certificaat importeren.

  g.    Het proces herhalen voor alle certificaten die u voor uw implementatie opgeeft.

3. Exporteer het certificaat naar de PFX-indeling met Azure-Stack-vereisten:

  a.    Open MMC Certificate Manager-console en maak verbinding met het certificaatarchief van de lokale computer.

  b.    Ga naar de **ondernemingsvertrouwen** directory.

  c.    Selecteer een van de certificaten die u hebt geïmporteerd in stap 2 hierboven.

  d.    Selecteer in de taak balk van Certificate Manager-console **acties** > **alle taken** > **exporteren**.

  e.    Selecteer **Volgende**.

  f.    Selecteer **Ja, de persoonlijke sleutel exporteren**, en klik vervolgens op **volgende**.

  g.    Selecteer in de sectie bestandsindeling voor Export **alle uitgebreide eigenschappen exporteren** en klik vervolgens op **volgende**.

  h.    Selecteer **wachtwoord** en een wachtwoord opgeven voor de certificaten. Onthoud dit wachtwoord omdat deze wordt gebruikt als een implementatieparameter. Selecteer **Volgende**.

  i.    Kies een bestandsnaam en locatie voor het pfx-bestand exporteren. Selecteer **Volgende**.

  j.    Selecteer **Voltooien**.

  k.    Herhaal dit proces voor alle certificaten die u voor uw implementatie in stap 2 hierboven hebt geïmporteerd.

## <a name="next-steps"></a>Volgende stappen
[Valideren van PKI-certificaten](validate-pki-certs.md)