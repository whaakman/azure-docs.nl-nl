---
title: Met extern bureaublad met Azure rollen | Microsoft Docs
description: Extern bureaublad gebruiken met Azure-functies
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: eab135d10c0d6df8ca72ac47d6804017a998a3d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Extern bureaublad gebruiken met Azure-functies
Met behulp van de Azure SDK en extern bureaublad-Services, kunt u toegang tot Azure-functies en virtuele machines die worden gehost door Azure. U kunt Extern bureaublad-Services configureren vanuit een Azure-cloud service-project in Visual Studio. Om extern bureaublad-Services, moet u een project werkt met een of meer rollen maken en deze publiceren naar Azure.

> [!IMPORTANT]
> U moet toegang tot een Azure-functie voor het oplossen van problemen of alleen-ontwikkeling. Het doel van elke virtuele machine wordt uitgevoerd van een specifieke functie in uw Azure-toepassing, niet voor andere clienttoepassingen. Als u gebruiken van Azure wilt voor het hosten van een virtuele machine die u voor enig doel gebruiken kunt, raadpleegt u de toegang tot Azure virtuele Machines in Server Explorer.
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Inschakelen en extern bureaublad gebruiken voor een Azure-rol
1. Open het snelmenu voor uw cloud service-project in Solution Explorer en kies vervolgens **publiceren**.
   
    De **Publish Azure Application** wizard wordt weergegeven.
   
    ![Opdracht voor een Cloud Service-project publiceren](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. Aan de onderkant van **Microsoft Azure Publish Settings** pagina van de wizard selecteert u de **extern bureaublad inschakelen** voor alle functies selectievakje. 
   
    De **configuratie voor extern bureaublad** dialoogvenster wordt weergegeven.
3. Aan de onderkant van de **configuratie voor extern bureaublad** dialoogvenster Kies de **meer opties** knop. 
   
    U ziet nu een vervolgkeuzelijst waarmee u Maak of Selecteer een certificaat dat u referenties gegevens versleutelen kunt om verbinding te maken via Extern bureaublad.
4. Kies in de vervolgkeuzelijst  **&lt;maken >**, of kies een bestaande uit de lijst. 
   
    Als u een bestaand certificaat kiest, moet u de volgende stappen overslaan.
   
   > [!NOTE]
   > De certificaten die u nodig hebt voor een verbinding met extern bureaublad wijken af van de certificaten die u voor andere Azure-bewerkingen gebruikt. De RAS-certificaat moet een persoonlijke sleutel hebben.
   > 
   > 
   
    De **certificaat maken** dialoogvenster wordt weergegeven.
   
   1. Geef een beschrijvende naam voor het nieuwe certificaat en kies vervolgens de **OK** knop. Het nieuwe certificaat wordt weergegeven in de vervolgkeuzelijst.
   2. In de **configuratie voor extern bureaublad** dialoogvenster Geef een gebruikersnaam en wachtwoord.
      
       U kunt een bestaand account niet gebruiken. Geen beheerder opgeven als de gebruikersnaam op voor het nieuwe account.
      
      > [!NOTE]
      > Als het wachtwoord niet voldoet aan de complexiteitsvereisten, verschijnt er een rode pictogram naast het wachtwoordvak. Het wachtwoord moet bevatten hoofdletters, kleine letters en cijfers of symbolen.
      > 
      > 
   3. Kies een datum op waarop het account verloopt en na welke Extern bureaublad-verbindingen worden geblokkeerd.
   4. Nadat u de vereiste gegevens hebt opgegeven, kiest u de **OK** knop.
      
       Diverse instellingen waarmee u Remote Access Services zijn toegevoegd aan de .cscfg en .csdef-bestanden.
5. In de **Microsoft Azure Publish Settings** wizard kiest u de **OK** knop wanneer u klaar bent voor het publiceren van uw cloudservice.
   
    Als u niet klaar voor het publiceren van bent, kiest u de **annuleren** knop. De configuratie-instellingen worden opgeslagen en u kunt uw cloudservice later publiceren.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Verbinding maken met een Azure-rol met behulp van extern bureaublad
Nadat u uw cloudservice op Azure hebt gepubliceerd, kunt u Server Explorer aan te melden bij de virtuele machines die Azure als host fungeert. 

1. Vouw in Server Explorer de **Azure** knooppunt, en vouw vervolgens het knooppunt voor een cloudservice en een van de rollen voor het weergeven van exemplaren.
2. Open het snelmenu voor een exemplaar-knooppunt en kies vervolgens **verbinding maken met behulp van extern bureaublad**.
   
    ![Verbinding maken via Extern bureaublad](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Geef de gebruikersnaam en wachtwoord dat u eerder hebt gemaakt. U bent nu aangemeld bij de externe sessie.

