---
title: CHAP configureren voor de StorSimple 8000-apparaat | Microsoft Docs
description: Beschrijft hoe u voor het configureren van de CHAP Challenge Handshake Authentication Protocol () op een StorSimple-apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: efc116c278bfe72419800603a3b365f461fe0a28
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58095157"
---
# <a name="configure-chap-for-your-storsimple-device"></a>CHAP configureren voor uw StorSimple-apparaat

In deze zelfstudie wordt uitgelegd hoe u CHAP configureren voor uw StorSimple-apparaat. De procedure beschreven in dit artikel is van toepassing op apparaten uit de StorSimple 8000-serie.

CHAP staat voor Challenge Handshake Authentication Protocol. Het is een verificatiemethode wordt gebruikt door servers om de identiteit van externe clients te valideren. De verificatie is gebaseerd op een gedeelde wachtwoord of geheim. CHAP is één manier (Unidirectioneel) of wederzijdse (in twee richtingen). Eén manier CHAP is wanneer het doel wordt een initiator geverifieerd. Het doel wordt de initiator geverifieerd in wederzijdse of omgekeerde CHAP, en vervolgens de initiator het doel verifieert. De initiator-verificatie kan worden geïmplementeerd zonder de doel-verificatie. Doel-verificatie kan echter worden geïmplementeerd, alleen als de verificatie van de initiator ook is geïmplementeerd.

Als een best practice, raden wij aan dat u CHAP om iSCSI-beveiliging te verbeteren.

> [!NOTE]
> Houd er rekening mee dat IPSEC wordt momenteel niet ondersteund op StorSimple-apparaten.

De CHAP-instellingen op het StorSimple-apparaat kunnen worden geconfigureerd in de volgende manieren:

* Eén richting of eenrichtings-verificatie
* In twee richtingen of wederzijdse verificatie of omgekeerde verificatie

In elk van deze gevallen is moet de portal voor het apparaat en de serversoftware van de iSCSI-initiator worden geconfigureerd. De gedetailleerde stappen voor deze configuratie worden beschreven in de volgende zelfstudie.

## <a name="unidirectional-or-one-way-authentication"></a>Eén richting of eenrichtings-verificatie

In één richting verificatie, wordt de initiator geverifieerd met het doel. Deze verificatie is vereist dat u de CHAP-initiator-instellingen op het StorSimple-apparaat en de iSCSI-Initiator-software op de host configureren. De gedetailleerde procedures voor uw StorSimple-apparaat en de Windows-host worden hierna beschreven.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Uw apparaat voor eenrichtingsverkeer verificatie configureren

1. In de Azure-portal, gaat u naar uw StorSimple Device Manager-service. Klik op **apparaten** en selecteer en klik op een apparaat dat u wilt configureren van CHAP voor. Ga naar **apparaatinstellingen > beveiliging**. In de **beveiligingsinstellingen** blade, klikt u op **CHAP**.
   
    ![CHAP-Initiator](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. In de **CHAP** blade, en klik in de **CHAP Initiator** sectie:
   
   1. Geef een gebruikersnaam voor het CHAP-initiator.
   2. Een wachtwoord voor uw CHAP-initiator op te geven.
      
      > [!IMPORTANT]
      > De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten. De CHAP-wachtwoord moet 12 tot 16 tekens. Een langer gebruikersnaam of wachtwoord resulteert in een verificatiefout opgetreden op de Windows-host.
   
   3. Bevestig het wachtwoord.

       ![CHAP-Initiator](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. Klik op **Opslaan**. Er wordt een bevestigingsbericht weergegeven. Klik op **OK** om de wijzigingen op te slaan.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Eenzijdige verificatie configureren op de host Windows server
1. Start de iSCSI-Initiator op de host-server van Windows.
2. In de **eigenschappen iSCSI-Initiator** venster de volgende stappen uitvoeren:
   
   1. Klik op de **detectie** tabblad.
      
       ![Eigenschappen iSCSI-initiator](./media/storsimple-configure-chap/IC740944.png)
   2. Klik op **Portal detecteren**.
3. In de **doelportaal detecteren** in het dialoogvenster:
   
   1. Geef het IP-adres van uw apparaat.
   2. Klik op **Advanced**.
      
       ![Doelportaal detecteren](./media/storsimple-configure-chap/IC740945.png)
4. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Selecteer de **inschakelen CHAP aanmelden** selectievakje.
   2. In de **naam** veld, geef de naam van de gebruiker die u hebt opgegeven voor de CHAP-Initiator in Azure portal.
   3. In de **Doelgeheim** veld, voert u het wachtwoord die u hebt opgegeven voor de CHAP-Initiator in Azure portal.
   4. Klik op **OK**.
      
       ![Geavanceerde instellingen algemeen](./media/storsimple-configure-chap/IC740946.png)
5. Op de **doelen** tabblad van de **eigenschappen iSCSI-Initiator** venster status van het apparaat moet worden weergegeven als **verbonden**. Als u een 1200 StorSimple-apparaat gebruikt, is elk volume dat is gekoppeld als een iSCSI-doel. Stap 3 en 4 moet daarom worden herhaald voor elk volume.
   
    ![Volumes die zijn gekoppeld als afzonderlijke doelen](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Als u de naam van de iSCSI-wijzigt, wordt de nieuwe naam wordt gebruikt voor nieuwe iSCSI-sessies. Nieuwe instellingen worden niet gebruikt voor bestaande sessies totdat u zich afmelden en aanmelden opnieuw.

Voor meer informatie over het configureren van CHAP op de host Windows server, gaat u naar [aanvullende overwegingen](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Verificatie in twee richtingen of wederzijdse

In de verificatie in twee richtingen, het doel wordt de initiator geverifieerd en vervolgens de initiator het doel verifieert. Deze procedure moet de gebruiker configureren de instellingen voor CHAP initiator en omgekeerde CHAP-instellingen op het apparaat en iSCSI-Initiator-software op de host. De volgende procedures beschrijven de stappen voor het configureren van wederzijdse verificatie op het apparaat en op de Windows-host.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Uw apparaat voor wederzijdse verificatie te configureren

1. In de Azure-portal, gaat u naar uw StorSimple Device Manager-service. Klik op **apparaten** en selecteer en klik op een apparaat dat u wilt configureren van CHAP voor. Ga naar **apparaatinstellingen > beveiliging**. In de **beveiligingsinstellingen** blade, klikt u op **CHAP**.
   
    ![CHAP-doel](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Schuif naar beneden op deze pagina en in de **CHAP-doel** sectie:
   
   1. Geef een **omgekeerde CHAP-gebruikersnaam** voor uw apparaat.
   2. Geef een **omgekeerd CHAP-wachtwoord** voor uw apparaat.
   3. Bevestig het wachtwoord.
3. In de **CHAP Initiator** sectie:
   
   1. Geef een **gebruikersnaam** voor uw apparaat.
   2. Geef een **wachtwoord** voor uw apparaat.
   3. Bevestig het wachtwoord.

       ![CHAP-Initiator](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. Klik op **Opslaan**. Er wordt een bevestigingsbericht weergegeven. Klik op **OK** om de wijzigingen op te slaan.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Verificatie in twee richtingen op de Windows-host-server configureren

1. Start de iSCSI-Initiator op de host-server van Windows.
2. In de **eigenschappen iSCSI-Initiator** venster, klikt u op de **configuratie** tabblad.
3. Klik op **CHAP**.
4. In de **iSCSI-Initiator wederzijdse CHAP-geheim** in het dialoogvenster:
   
   1. Type de **omgekeerde CHAP-wachtwoord** die u hebt geconfigureerd in Azure portal.
   2. Klik op **OK**.
      
       ![iSCSI-initiator wederzijdse CHAP-geheim](./media/storsimple-configure-chap/IC740949.png)
5. Klik op de **doelen** tabblad.
6. Klik op de **Connect** knop. 
7. In de **verbinding naar doel** in het dialoogvenster, klikt u op **Geavanceerd**.
8. In de **geavanceerde eigenschappen** in het dialoogvenster:
   
   1. Selecteer de **inschakelen CHAP aanmelden** selectievakje.
   2. In de **naam** veld, geef de naam van de gebruiker die u hebt opgegeven voor de CHAP-Initiator in Azure portal.
   3. In de **Doelgeheim** veld, voert u het wachtwoord die u hebt opgegeven voor de CHAP-Initiator in Azure portal.
   4. Selecteer de **wederzijdse verificatie uitvoeren** selectievakje.
      
       ![Geavanceerde instellingen voor wederzijdse verificatie](./media/storsimple-configure-chap/IC740950.png)
   5. Klik op **OK** de CHAP-configuratie voltooien

Voor meer informatie over het configureren van CHAP op de host Windows server, gaat u naar [aanvullende overwegingen](#additional-considerations).

## <a name="additional-considerations"></a>Aanvullende overwegingen

De **snelle verbinding** functie biedt geen ondersteuning voor verbindingen waarvoor CHAP ingeschakeld. Wanneer CHAP is ingeschakeld, zorg ervoor dat u de **Connect** knop die beschikbaar is op de **doelen** tabblad verbinding maken met een doel.

![Verbinding maken met doelserver](./media/storsimple-configure-chap/IC740947.png)

In de **verbinding maken met doel** in het dialoogvenster dat wordt weergegeven, selecteer de **deze verbinding toevoegen aan de lijst met favoriete doelen** selectievakje. Deze selectie zorgt ervoor dat telkens wanneer de computer opnieuw is opgestart, wordt geprobeerd om terug te zetten van de verbinding met de favoriete iSCSI-doelen.

## <a name="errors-during-configuration"></a>Fouten tijdens de configuratie

Als de CHAP-configuratie onjuist is, dan kunt u waarschijnlijk om te zien een **verificatiefout** foutbericht weergegeven.

## <a name="verification-of-chap-configuration"></a>Verificatie van CHAP-configuratie

U kunt controleren dat CHAP wordt gebruikt door de volgende stappen te voltooien.

#### <a name="to-verify-your-chap-configuration"></a>Uw CHAP-configuratie controleren
1. Klik op **favoriete doelen**.
2. Selecteer het doel waarvoor u verificatie hebt ingeschakeld.
3. Klik op **Details**.
   
    ![iSCSI-initiator eigenschappen favoriete doelen](./media/storsimple-configure-chap/IC740951.png)
4. In de **favoriete Doeldetails** dialoogvenster vak, houd er rekening mee de vermelding in de **verificatie** veld. Als is de configuratie voltooid is, moet zijn **CHAP**.
   
    ![Favoriete Doeldetails](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [StorSimple security](storsimple-8000-security.md).
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

