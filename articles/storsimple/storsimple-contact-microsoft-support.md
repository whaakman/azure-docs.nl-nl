---
title: Meld u ondersteuningsticket voor StorSimple 8000 serie | Microsoft Docs
description: Informatie over het maken van een verzoek om ondersteuning en ondersteuningssessie starten op uw StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cecc2566b432e897b5eff0c12e66598f0518ed80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>Neem contact op met Microsoft ondersteuning voor uw StorSimple
Als u problemen ondervindt met uw Microsoft Azure StorSimple-oplossing, kunt u een serviceaanvraag voor technische ondersteuning. In een online-sessie met de ondersteuningstechnicus moet u mogelijk ook een support-sessie starten op uw StorSimple-apparaat. Dit artikel begeleidt u bij:

* Het maken van een aanvraag voor ondersteuning.
* Het starten van een ondersteuningssessie in de Windows PowerShell-interface van uw StorSimple-apparaat.

Controleer de [StorSimple 8000 Series ondersteuning Sla's en informatie](https://msdn.microsoft.com/library/mt433077.aspx) voordat u een ondersteuningsaanvraag maken.

## <a name="create-a-support-request"></a>Een ondersteuningsaanvraag maken
Voer de volgende stappen uit om een ondersteuningsaanvraag te maken:

#### <a name="to-create-a-support-request"></a>Om een ondersteuningsaanvraag te maken
1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klik op de accountnaam van uw in de rechterbovenhoek en klik vervolgens op **contact opnemen met Microsoft ondersteuning**.
   
    ![Neem contact op met MS ondersteuning via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. U wordt omgeleid naar de nieuwe Azure portal (portal.azure.com). Klik op de **nieuw ondersteuningsverzoek** tegel.
   
    ![Neem contact op met MS ondersteuning via de nieuwe portal](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Aan de rechterkant van het scherm de **nieuw ondersteuningsverzoek** deelvenster wordt weergegeven. 
   
    ![Nieuw deelvenster voor ondersteuning aanvragen](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. In de **basisbeginselen** dialoogvenster Vervolledig de volgende:                                
   
   1. Van de **probleemtype** vervolgkeuzelijst, selecteer **technische**.
   2. Selecteer een **abonnement** uit de vervolgkeuzelijst.
   3. Van de **Service** vervolgkeuzelijst, selecteer **StorSimple**. 
   4. Selecteer een **ondersteuningsplan** uit de vervolgkeuzelijst. U moet een betaald ondersteuningsplan om in te schakelen met technische ondersteuning.
4. Klik op **Volgende**. De **probleem** dialoogvenster wordt weergegeven.
   
    ![Nieuw deelvenster voor ondersteuning aanvragen](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. In de **probleem** dialoogvenster Vervolledig de volgende:
   
   1. Selecteer een **ernst** niveau uit de vervolgkeuzelijst.
   2. Selecteer een **probleemtype** uit de vervolgkeuzelijst.
   3. Selecteer een **categorie** uit de vervolgkeuzelijst. 
   4. In de **Details** vak een korte beschrijving van het probleem.
   5. In de **tijdsbestek** vak, geven de datum, tijd en tijdzone die overeenkomt met het meest recente exemplaar van het probleem.
   6. Onder **uploaden bestand**, klikt u op het pictogram van de map om te bladeren naar het ondersteuningspakket.
   7. Selecteer de **diagnostische gegevens delen** selectievakje.
6. Klik op **Volgende**. De **contactgegevens** dialoogvenster wordt weergegeven.
   
    ![Nieuw deelvenster voor ondersteuning aanvragen](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Voer uw contactgegevens en selecteer een contactmethode (telefoon of e-mail). 
8. Selecteer de **contact op met wijzigingen opslaan voor toekomstige ondersteuningsaanvragen** selectievakje.
9. Klik op **Create**.

Nadat u uw aanvraag hebt verzonden, een ondersteuningstechnicus neemt contact met u zo snel mogelijk om door te gaan met uw aanvraag.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Een support-sessie te starten in Windows PowerShell voor StorSimple
Voor het oplossen van problemen die u met de StorSimple-apparaat ondervindt mogelijk, moet u het Microsoft Support team benaderen. Microsoft Support moet mogelijk gebruik van een ondersteuningssessie aan te melden op uw apparaat. 

Voer de volgende stappen uit om een ondersteuningssessie te starten:

#### <a name="to-start-a-support-session"></a>Ondersteuningssessie starten
1. Toegang tot het apparaat via de seriële console of via een Telnet-sessie vanaf een externe computer. Volg hiervoor de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. In de sessie die wordt geopend, drukt u op de **Enter** sleutel om op te halen vanaf de opdrachtprompt.
3. Selecteer in het menu van de seriële console optie 1, **aanmelden met volledige toegang**.
4. Typ het volgende wachtwoord bij de opdrachtprompt: 
   
    `Password1`
5. Typ de volgende opdracht bij de opdrachtprompt:
   
    `Enable-HcsSupportAccess`
6. U krijgt een gecodeerde tekenreeks. Kopieer deze tekenreeks in een teksteditor zoals Kladblok.
7. Bewaar deze tekenreeks en deze in een e-mailbericht verzenden naar Microsoft Support. 

> [!IMPORTANT]
> U kunt ondersteuning toegang uitschakelen door het uitvoeren van `Disable-HcsSupportAccess`. Het StorSimple-apparaat wordt ook geprobeerd om uit te schakelen ondersteuning toegang tot 8 uur nadat de sessie is gestart. Het is een best practice om te wijzigen van de referenties van uw StorSimple-apparaat na het initiëren van een ondersteuningssessie voor.
> 
> 

