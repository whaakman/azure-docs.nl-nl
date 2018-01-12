---
title: Maak ondersteuningsticket of de aanvraag voor StorSimple 8000 serie | Microsoft Docs
description: Informatie over het aan te melden ondersteuning aan te vragen en ondersteuningssessie starten op uw StorSimple 8000 serie-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli;
ms.openlocfilehash: e0df86e40d0dc1092ad7ff04f01bbc3e5e3d1c4e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft-ondersteuning

Apparaatbeheer StorSimple biedt de mogelijkheid om te **Meld u een nieuw ondersteuningsverzoek** binnen de samenvatting blade van de service. Als u problemen ondervindt met uw StorSimple-oplossing, kunt u een serviceaanvraag voor technische ondersteuning. In een online-sessie met de ondersteuningstechnicus moet u mogelijk ook een support-sessie starten op uw StorSimple-apparaat. Dit artikel begeleidt u bij:

* Het maken van een aanvraag voor ondersteuning.
* Het beheren van een aanvraag ondersteuningslevenscyclus uit binnen de portal.
* Het starten van een ondersteuningssessie in de Windows PowerShell-interface van uw StorSimple-apparaat.

Controleer de [StorSimple 8000 Series ondersteuning Sla's en informatie](https://msdn.microsoft.com/library/mt433077.aspx) voordat u een ondersteuningsaanvraag maken.

## <a name="create-a-support-request"></a>Maak een ondersteuningsaanvraag

Afhankelijk van uw [ondersteuningsplan](https://azure.microsoft.com/support/plans/), kunt u ondersteuningstickets voor een probleem voor uw StorSimple-apparaat rechtstreeks vanuit de samenvatting blade voor Apparaatbeheer van StorSimple-service maken. Voer de volgende stappen uit om een ondersteuningsaanvraag te maken:

#### <a name="to-create-a-support-request"></a>Om een ondersteuningsaanvraag te maken

1. Ga naar uw StorSimple-apparaatbeheerservice. Ga in de instellingen van de service-samenvatting blade naar **ondersteuning + probleemoplossing** sectie en klik vervolgens op **nieuw ondersteuningsverzoek**.
     
    ![Neem contact op met MS ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. In de **nieuw ondersteuningsverzoek** blade Selecteer **basisbeginselen**. In de **basisbeginselen** blade, moet u de volgende stappen uitvoeren:
   1. Van de **probleemtype** vervolgkeuzelijst, selecteer **technische**.
   2. De huidige **abonnement**, **Service** type, en de **Resource** (Apparaatbeheer StorSimple-service) worden automatisch gekozen. 
   3. Selecteer een **ondersteuningsplan** uit de vervolgkeuzelijst als u meerdere abonnementen die zijn gekoppeld aan uw abonnement hebt. U moet een betaald ondersteuningsplan om in te schakelen met technische ondersteuning.
   4. Klik op **Volgende**.

       ![Neem contact op met MS ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. In de **nieuw ondersteuningsverzoek** blade Selecteer **stap 2 probleem**. In de **probleem** blade, moet u de volgende stappen uitvoeren:
    
    1. Kies de **ernst**.
    2. Geef als het probleem is gerelateerd aan het toestel of de service Manager voor StorSimple-apparaat.
    3. Kies een **categorie** voor dit probleem en bieden meer **Details** over het probleem.
    4. Geef de begindatum en -tijd voor het probleem.
    5. In de **uploaden bestand**, klikt u op het pictogram van de map om te bladeren naar het ondersteuningspakket.
    6. Controleer **diagnostische gegevens delen**.
    7. Klik op **Volgende**.

       ![Neem contact op met MS ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. In de **nieuw ondersteuningsverzoek** blade, klikt u op **contactgegevens van stap 3**. In de **contactgegevens** blade, moet u de volgende stappen uitvoeren:

    1. In de **Neem contact op met de opties**, voorkeurscontactmethode (telefoon of e-mail) en de taal opgeven. De reactietijd wordt automatisch geselecteerd op basis van het plan van uw abonnement.
    2. Geef in de informatie contact op met uw naam, e, optionele contactpersoon, land. Selecteer de **contact op met wijzigingen opslaan voor toekomstige ondersteuningsaanvragen** selectievakje.
    3. Klik op **Create**.
   
        ![Neem contact op met MS ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    Microsoft Support gebruikt deze informatie om te bereiken u voor meer informatie, diagnose en resolutie.
Nadat u uw aanvraag hebt verzonden, een ondersteuningstechnicus neemt contact met u zo snel mogelijk om door te gaan met uw aanvraag.

## <a name="manage-a-support-request"></a>Een verzoek om ondersteuning te beheren

Nadat u een ondersteuningsticket hebt gemaakt, kunt u de voortgang van het ticket vanuit de portal beheren.

#### <a name="to-manage-your-support-requests"></a>Voor het beheren van uw ondersteuningsaanvragen

1. Als u naar de pagina help en ondersteuning, gaat u naar **Bladeren > Help + ondersteuning**.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. Een lijst in tabelvorm van alle ondersteuningsaanvragen voor de wordt weergegeven in de **Help + ondersteuning** blade.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. Selecteer en klikt u op een aanvraag voor ondersteuning. U kunt de status en de details voor deze aanvraag weergeven. Klik op **+ nieuw bericht** als u wilt volgen op deze aanvraag.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Een support-sessie te starten in Windows PowerShell voor StorSimple

Voor het oplossen van problemen die u met de StorSimple-apparaat ondervindt mogelijk, moet u het Microsoft Support team benaderen. Microsoft Support moet mogelijk gebruik van een ondersteuningssessie aan te melden op uw apparaat.

Voer de volgende stappen uit om een ondersteuningssessie te starten:

#### <a name="to-start-a-support-session"></a>Ondersteuningssessie starten

1. Toegang tot het apparaat via de seriële console of via een Telnet-sessie vanaf een externe computer. Volg hiervoor de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
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


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [opsporen en oplossen van problemen met betrekking tot uw StorSimple 8000 series apparaat](storsimple-8000-troubleshoot-deployment.md)
