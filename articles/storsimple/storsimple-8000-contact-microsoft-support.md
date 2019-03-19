---
title: Maak ondersteuningsticket of een aanvraag voor StorSimple 8000-serie | Microsoft Docs
description: Leer hoe u een ondersteuningsaanvraag registreren en een ondersteuningssessie te starten op uw StorSimple 8000-apparaat.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli;
ms.openlocfilehash: fb8cfd4767f6bb9afe1b5731d3b4db1c68a73056
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58107124"
---
# <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft-ondersteuning

StorSimple Device Manager biedt de mogelijkheid voor **melden van een nieuwe ondersteuningsaanvraag** binnen de overzichtsblade van de service. Als u problemen ondervindt met uw StorSimple-oplossing, kunt u een serviceaanvraag voor technische ondersteuning. In een online-sessie met de ondersteuningstechnicus moet u mogelijk ook een ondersteuningssessie te starten op uw StorSimple-apparaat. In dit artikel begeleidt u bij:

* Het maken van een ondersteuningsaanvraag.
* Over het beheren van een aanvraag ondersteuningslevenscyclus uit in de portal.
* Hoe u een ondersteuningssessie te starten in de Windows PowerShell-interface van uw StorSimple-apparaat.

Controleer de [StorSimple 8000-serie ondersteuning Sla's en informatie](https://msdn.microsoft.com/library/mt433077.aspx) voordat u een ondersteuningsaanvraag maken.

## <a name="create-a-support-request"></a>Maak een ondersteuningsaanvraag

Afhankelijk van uw [ondersteuningsplan](https://azure.microsoft.com/support/plans/), u kunt ondersteuningstickets voor een probleem maken op uw StorSimple-apparaat rechtstreeks vanuit de overzichtsblade van de StorSimple Device Manager-service. De volgende stappen uitvoeren om een ondersteuningsaanvraag maken:

#### <a name="to-create-a-support-request"></a>Een ondersteuningsaanvraag maken

1. Ga naar uw StorSimple-apparaatbeheerservice. In de overzichtsblade van service-instellingen, gaat u naar **ondersteuning + probleemoplossing** sectie en klik vervolgens op **nieuwe ondersteuningsaanvraag**.
     
    ![Neem contact op met MS-ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. In de **nieuwe ondersteuningsaanvraag** Selecteer **basisbeginselen**. In de **basisbeginselen** blade de volgende stappen uit:
   1. Uit de **type probleem** vervolgkeuzelijst, selecteer **technische**.
   2. De huidige **abonnement**, **Service** type en de **Resource** (StorSimple Device Manager-service) worden automatisch gekozen. 
   3. Selecteer een **ondersteuningsplan** uit de vervolgkeuzelijst als u meerdere abonnementen die zijn gekoppeld aan uw abonnement hebt. U moet een betaald abonnement om in te schakelen technische ondersteuning.
   4. Klik op **volgende**.

       ![Neem contact op met MS-ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. In de **nieuwe ondersteuningsaanvraag** Selecteer **stap 2 probleem**. In de **probleem** blade de volgende stappen uit:
    
    1. Kies de **ernst**.
    2. Geef als het probleem is gerelateerd aan het apparaat of de service StorSimple Device Manager.
    3. Kies een **categorie** voor dit probleem en bieden meer **Details** over het probleem.
    4. Geef de begindatum en -tijd voor het probleem.
    5. In de **uploaden van het bestand**, klik op het mappictogram om te bladeren naar uw ondersteuningspakket.
    6. Controleer **diagnostische gegevens delen**.
    7. Klik op **volgende**.

       ![Neem contact op met MS-ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. In de **nieuwe ondersteuningsaanvraag** blade, klikt u op **contactgegevens van stap 3**. In de **contactgegevens** blade de volgende stappen uit:

   1. In de **opties voor contactpersonen**, geef de gewenste manier (telefoon of e-mail) en de taal. De reactietijd wordt automatisch geselecteerd op basis van uw abonnement wilt.
   2. Geef uw naam, e-mailbericht, contact op met de optionele, land/regio in de contactgegevens. Selecteer de **contact op met wijzigingen opslaan voor toekomstige ondersteuningsaanvragen** selectievakje.
   3. Klik op **Create**.
   
       ![Neem contact op met MS-ondersteuning via de nieuwe portal](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

      Deze informatie wordt door Microsoft Support contact op met u voor meer informatie, diagnose en het probleem zou moeten worden gebruikt.
      Nadat u uw aanvraag hebt ingediend, een ondersteuningsmedewerker neemt contact met u zo snel mogelijk om door te gaan met uw aanvraag.

## <a name="manage-a-support-request"></a>Een ondersteuningsaanvraag beheren

Nadat u een ondersteuningsticket hebt gemaakt, kunt u de voortgang van het ticket vanuit de portal beheren.

#### <a name="to-manage-your-support-requests"></a>Voor het beheren van uw ondersteuningsaanvragen

1. Als u naar de pagina help en ondersteuning, gaat u naar **Bladeren > Help + ondersteuning**.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. Een lijst in tabelvorm met alle aanvragen voor de ondersteuning wordt weergegeven in de **Help en ondersteuning** blade.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. Selecteer en klik op een aanvraag voor ondersteuning. U kunt de status en de details voor deze aanvraag bekijken. Klik op **+ nieuw bericht** als u wilt deze aanvraag opgevolgd.

    ![Ondersteuningsaanvragen beheren](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Een ondersteuningssessie starten in Windows PowerShell voor StorSimple

Voor het oplossen van problemen die u met het StorSimple-apparaat tegenkomen kunt, moet u contact opnemen met het team van Microsoft Support. Microsoft Support moet mogelijk een ondersteuningssessie gebruiken voor aanmelding bij uw apparaat.

De volgende stappen uitvoeren om een ondersteuningssessie te starten:

#### <a name="to-start-a-support-session"></a>Een ondersteuningssessie te starten

1. Toegang tot het apparaat rechtstreeks via de seriële console of via een Telnet-sessie vanaf een externe computer. U doet dit door u de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. In de sessie die wordt geopend, drukt u op de **Enter** sleutel aan een opdrachtprompt.
3. Selecteer in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**.
4. Bij de opdrachtprompt, typt u het volgende wachtwoord:
   
    `Password1`
5. Typ achter de prompt de volgende opdracht:
   
    `Enable-HcsSupportAccess`
6. Een gecodeerde tekenreeks wordt aan u worden gepresenteerd. Kopieer deze tekenreeks in een teksteditor zoals Kladblok.
7. Deze tekenreeks opslaat en in een e-mailbericht verzenden naar Microsoft Support.

> [!IMPORTANT]
> U kunt toegang tot ondersteuning uitschakelen door het uitvoeren van `Disable-HcsSupportAccess`. Het StorSimple-apparaat wordt ook proberen om uit te schakelen toegang tot ondersteuning 8 uur na de sessie is gestart. Het is aanbevolen om te wijzigen van de referenties van uw StorSimple-apparaat na het initiëren van een ondersteuningssessie voor.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [vaststellen en oplossen van problemen met betrekking tot de StorSimple 8000-apparaat](storsimple-8000-troubleshoot-deployment.md)
