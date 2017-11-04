---
title: CHAP configureren voor StorSimple 8000 series apparaat | Microsoft Docs
description: Beschrijft hoe de Challenge Handshake Authentication Protocol (CHAP) configureren op een StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 83ad256522ca19a19b3fe46fcc48e9cb37cbe246
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>CHAP configureren voor uw StorSimple-apparaat
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. De versie van dit artikel voor de nieuwe Azure portal, Ga naar [CHAP configureren voor uw StorSimple-apparaat](storsimple-8000-configure-chap.md). Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

Deze zelfstudie wordt uitgelegd hoe CHAP configureren voor uw StorSimple-apparaat. De procedure beschreven in dit artikel is van toepassing op StorSimple 8000-serie, evenals 1200 StorSimple-apparaten.

CHAP staat voor Challenge Handshake Authentication Protocol. Het is een verificatieschema dat wordt gebruikt door servers om de identiteit van externe clients te valideren. De verificatie is gebaseerd op een gedeelde wachtwoord of geheim. CHAP mag eenzijdige (Unidirectioneel) of onderlinge (bidirectioneel). Eenzijdige CHAP is wanneer het doel een initiator verifieert. Wederzijdse of omgekeerde CHAP, moet aan de andere kant het doel de initiator verifiëren en vervolgens de initiator het doel te verifiëren. Initiator verificatie kan worden geïmplementeerd zonder verificatie doel. Doel-verificatie kan echter alleen als de verificatie van de initiator is ook geïmplementeerd worden geïmplementeerd. 

Als een best practice is het raadzaam dat u CHAP om iSCSI-beveiliging te verbeteren.

> [!NOTE]
> Houd er rekening mee dat IPSEC wordt momenteel niet ondersteund op het StorSimple-apparaten.
> 
> 

De CHAP-instellingen op het StorSimple-apparaat kunnen worden geconfigureerd op de volgende manieren:

* Unidirectioneel of eenrichtings-verificatie
* Bidirectionele of wederzijdse of reverse-verificatie

In alle gevallen moet de portal voor het apparaat en de server iSCSI-initiatorsoftware worden geconfigureerd. De gedetailleerde stappen voor deze configuratie worden beschreven in de volgende zelfstudie.

## <a name="unidirectional-or-one-way-authentication"></a>Unidirectioneel of eenrichtings-verificatie
In één richting verificatie verifieert het doel de initiator. Deze verificatie vereist dat u de CHAP-initiator-instellingen op het StorSimple-apparaat en de iSCSI-initiatorsoftware op de host configureren. De gedetailleerde procedures voor uw StorSimple-apparaat en de Windows-host worden nu beschreven.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Uw apparaat voor eenzijdige verificatie configureren
1. In de klassieke Azure-portal op de **apparaten** pagina, klikt u op de **configureren** tabblad.
   
    ![CHAP-Initiator](./media/storsimple-configure-chap/IC740943.png)
2. Schuif naar beneden op deze pagina en in de **CHAP-Initiator** sectie:
   
   1. Geef een gebruikersnaam op voor uw CHAP-initiator.
   2. Een wachtwoord opgeven voor CHAP-initiator.
      
    > [!IMPORTANT]
    > De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten. De CHAP-wachtwoord moet tussen 12 en 16 tekens. Een langer gebruikersnaam of wachtwoord leidt tot een verificatiefout opgetreden op de Windows-host.
   
   3. Bevestig het wachtwoord.
3. Klik op **Opslaan**. Een bevestigingsbericht wordt weergegeven. Klik op **OK** om de wijzigingen op te slaan.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Eenzijdige verificatie configureren op de host WindowsServer
1. Start de iSCSI-Initiator op de host WindowsServer.
2. In de **eigenschappen iSCSI-Initiator** venster de volgende stappen uitvoeren:
   
   1. Klik op de **detectie** tabblad.
      
       ![Eigenschappen iSCSI-initiator](./media/storsimple-configure-chap/IC740944.png)
   2. Klik op **Portal detecteren**.
3. In de **doelportaal detecteren** in het dialoogvenster:
   
   1. Geef het IP-adres van uw apparaat.
   2. Klik op **geavanceerde**.
      
       ![Doelportaal detecteren](./media/storsimple-configure-chap/IC740945.png)
4. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Selecteer de **inschakelen CHAP aanmelden** selectievakje.
   2. In de **naam** veld, geef de gebruikersnaam die u hebt opgegeven voor de CHAP-Initiator in de klassieke portal.
   3. In de **geheim van het doel** veld, het wachtwoord die u hebt opgegeven voor de CHAP-Initiator in de klassieke portal opgeven.
   4. Klik op **OK**.
      
       ![Geavanceerde instellingen algemeen](./media/storsimple-configure-chap/IC740946.png)
5. Op de **doelen** tabblad van de **eigenschappen iSCSI-Initiator** venster status van het apparaat moet worden weergegeven als **verbonden**. Als u een 1200 StorSimple-apparaat gebruikt, zal klikt u vervolgens elk volume worden gekoppeld als een iSCSI-doel zoals hieronder wordt weergegeven. Stap 3 en 4 moet daarom worden herhaald voor elk volume.
   
    ![Volumes die zijn gekoppeld als afzonderlijke doelen](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Als u de naam van de iSCSI-wijzigt, wordt de nieuwe naam worden gebruikt voor nieuwe iSCSI-sessies. Nieuwe instellingen worden niet gebruikt voor bestaande sessies totdat u afmelden en aanmelden opnieuw.
   > 
   > 

Voor meer informatie over het configureren van CHAP op de host WindowsServer, gaat u naar [aanvullende overwegingen](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Bidirectionele of wederzijdse verificatie
Het doel verifieert de initiator in twee richtingen verificatie, en vervolgens de initiator verifieert het doel. Hiervoor moet de gebruiker de CHAP-initiator-instellingen, evenals de omgekeerde CHAP-instellingen configureren op het apparaat- en iSCSI-initiatorsoftware op de host. De volgende procedures beschrijven de stappen voor het configureren van wederzijdse verificatie op het apparaat en op de Windows-host.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Om uw apparaat voor wederzijdse verificatie te configureren
1. In de klassieke Azure-portal op de **apparaten** pagina, klikt u op de **configureren** tabblad.
   
    ![CHAP-doel](./media/storsimple-configure-chap/IC740948.png)
2. Schuif naar beneden op deze pagina en in de **CHAP Target** sectie:
   
   1. Geef een **omgekeerde CHAP-gebruikersnaam** voor uw apparaat.
   2. Geef een **omgekeerde CHAP-wachtwoord** voor uw apparaat.
   3. Bevestig het wachtwoord.
3. In de **CHAP-Initiator** sectie:
   
   1. Geef een **gebruikersnaam** voor uw apparaat.
   2. Geef een **wachtwoord** voor uw apparaat.
   3. Bevestig het wachtwoord.
4. Klik op **Opslaan**. Een bevestigingsbericht wordt weergegeven. Klik op **OK** om de wijzigingen op te slaan.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Bidirectionele verificatie configureren op de host WindowsServer
1. Start de iSCSI-Initiator op de host WindowsServer.
2. In de **eigenschappen iSCSI-Initiator** venster, klikt u op de **configuratie** tabblad.
3. Klik op **CHAP**.
4. In de **iSCSI-Initiator wederzijdse CHAP Secret** in het dialoogvenster:
   
   1. Typ de **omgekeerde CHAP-wachtwoord** die u hebt geconfigureerd in de klassieke Azure portal.
   2. Klik op **OK**.
      
       ![iSCSI-initiator wederzijdse CHAP-geheim](./media/storsimple-configure-chap/IC740949.png)
5. Klik op de **doelen** tabblad.
6. Klik op de **Connect** knop. 
7. In de **verbinding naar doel** in het dialoogvenster, klikt u op **Geavanceerd**.
8. In de **geavanceerde eigenschappen** in het dialoogvenster:
   
   1. Selecteer de **inschakelen CHAP aanmelden** selectievakje.
   2. In de **naam** veld, geef de gebruikersnaam die u hebt opgegeven voor de CHAP-Initiator in de klassieke portal.
   3. In de **geheim van het doel** veld, het wachtwoord die u hebt opgegeven voor de CHAP-Initiator in de klassieke portal opgeven.
   4. Selecteer de **wederzijdse verificatie uitvoeren** selectievakje.
      
       ![Geavanceerde instellingen voor wederzijdse verificatie](./media/storsimple-configure-chap/IC740950.png)
   5. Klik op **OK** de CHAP-configuratie voltooien

Voor meer informatie over het configureren van CHAP op de host WindowsServer, gaat u naar [aanvullende overwegingen](#additional-considerations).

## <a name="additional-considerations"></a>Aanvullende overwegingen
De **snelle verbinding** functie biedt geen ondersteuning voor verbindingen waarvoor CHAP ingeschakeld. Wanneer CHAP is ingeschakeld, zorg ervoor dat u de **Connect** knop die beschikbaar is op de **doelen** tabblad verbinding maken met een doel.

![Verbinding maken met doel](./media/storsimple-configure-chap/IC740947.png)

In de **verbinding maken met doel** in het dialoogvenster wordt weergegeven, selecteer de **deze verbinding toevoegen aan de lijst met favoriete doelen** selectievakje. Dit zorgt ervoor dat elke keer dat de computer opnieuw is opgestart, wordt geprobeerd de verbinding te herstellen met de favoriete iSCSI-doelen.

## <a name="errors-during-configuration"></a>Fouten tijdens het configureren van
Als de CHAP-configuratie onjuist is, wordt u ziet er waarschijnlijk een **verificatiefout** foutbericht.

## <a name="verification-of-chap-configuration"></a>Verificatie van CHAP-configuratie
U kunt controleren dat CHAP wordt gebruikt door de volgende stappen uit te voeren.

#### <a name="to-verify-your-chap-configuration"></a>De CHAP-configuratie controleren
1. Klik op **favoriete doelen**.
2. Selecteer het doelobject op waarvoor verificatie is ingeschakeld.
3. Klik op **Details**.
   
    ![iSCSI-initiator eigenschappen favoriete doelen](./media/storsimple-configure-chap/IC740951.png)
4. In de **favoriete Doeldetails** dialoogvenster Noteer de vermelding in de **verificatie** veld. Als wordt de configuratie voltooid is, moet spreken **CHAP**.
   
    ![Details van de favoriet doel](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-security.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

