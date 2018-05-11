---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 842083f0f69552fb9076423386353dbb4dae73b5
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
1. Klik op de **Connect** knop op de eigenschappenpagina van de virtuele machine. 
2. In de **verbinding maken met virtuele machine** pagina, houden selecteert de gewenste opties en klik op **downloaden RDP-bestand**.
2. Open het gedownloade RDP-bestand en klik op **Connect** wanneer u wordt gevraagd. 
2. U ontvangt een waarschuwing dat het `.rdp`-bestand van een onbekende uitgever is. Dit is normaal. Klik in het venster Extern bureaublad op **Verbinden** om door te gaan.
   
    ![Schermafbeelding met waarschuwing over een onbekende uitgever](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de referenties voor een account op de virtuele machine en klik vervolgens op **OK**.
   
     **Lokaal account** - Dit is meestal de gebruikersnaam en het wachtwoord van het lokaal account dat u hebt opgegeven toen u de virtuele machine hebt gemaakt. In dit geval is het domein de naam van de virtuele machine en het is ingevoerd als *vmname*&#92;*gebruikersnaam*.  
   
    **In het domein opgenomen VM** - Als de virtuele machine deel uitmaakt van een domein, voert u de gebruikersnaam in met de volgende indeling: *Domein*&#92;*Gebruikersnaam*. Het account moet bovendien in de groep Administrators staan of er moet een machtiging voor externe toegang zijn verleend aan de VM.
   
    **Domeincontroller** - Als de VM een domeincontroller is, gebruikt u de gebruikersnaam en het wachtwoord van een domeinbeheerdersaccount voor het domein.
4. Klik op **Ja** om de identiteit van de virtuele machine te controleren en het aanmelden te voltooien.
   
   ![Schermafbeelding met een bericht over het verifiëren van de identiteit van de virtuele machine](./media/virtual-machines-log-on-win-server/cert-warning.png)

