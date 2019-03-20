---
title: Beveiligingsgegevens (preview) ingesteld voor het gebruik van een verificator-app - Azure Active Directory | Microsoft Docs
description: Over het instellen van je beveiligingsgegevens om uw identiteit met behulp van de Microsoft Authenticator-app te controleren.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4757be00a3633f56aed52dd7af22923e49b0b62
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58102380"
---
# <a name="set-up-security-info-preview-to-use-an-authenticator-app"></a>Beveiligingsgegevens (preview) ingesteld voor het gebruik van een verificator-app
U kunt deze stappen om toe te voegen uw tweeledige verificatie en methoden voor wachtwoord opnieuw instellen. Nadat u hebt dit tijdens de eerste keer hebt ingesteld, kunt u terugkeren naar de **beveiligingsgegevens** pagina toevoegen, bijwerken of verwijderen van uw beveiligingsgegevens.

Als u wordt gevraagd om in te stellen deze onmiddellijk nadat u zich aanmeldt bij uw account voor werk of school, ziet u de gedetailleerde stappen in de [instellen van je beveiligingsgegevens vanaf de opdrachtprompt aanmeldingspagina](security-info-setup-signin.md) artikel.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Als u een verificator-app-optie niet ziet, is het mogelijk dat uw organisatie niet kunt u een verificatie-app voor verificatie te gebruiken. In dit geval moet u een andere methode kiezen of neem contact op met uw beheerder voor meer informatie.

## <a name="set-up-the-microsoft-authenticator-app-from-the-security-info-page"></a>De Microsoft Authenticator-app via de pagina beveiligingsgegevens instellen
Afhankelijk van instellingen van uw organisatie, kunt u mogelijk een verificatie-app gebruiken als een van de beveiligingsmethoden voor informatie. U niet verplicht de Microsoft Authenticator-app te gebruiken en kunt u een andere app tijdens de procedure. In dit artikel gebruikt echter de Microsoft Authenticator-app. 

### <a name="to-set-up-the-microsoft-authenticator-app"></a>Voor het instellen van de Microsoft Authenticator-app

1. Meld u aan bij uw werk- of schoolaccount en gaat u naar uw https://myprofile.microsoft.com/ pagina.

    ![Mijn profiel-pagina, met gemarkeerde Security info koppelingen](media/security-info/securityinfo-myprofile-page.png)

2. Selecteer **beveiligingsgegevens** vanuit het navigatiedeelvenster links in of via de koppeling in de **beveiligingsgegevens** blokkeren, en selecteer vervolgens **-methode toevoegen** uit de **beveiligingsgegevens**  pagina.

    ![Pagina beveiligingsgegevens met optie van de methode gemarkeerde toevoegen](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Op de **toevoegen van een methode** weergeeft, schakelt **Authenticator-app** uit de vervolgkeuzelijst en selecteer vervolgens **toevoegen**.

    ![Vak methode toevoegen met verificator-app geselecteerd](media/security-info/securityinfo-myprofile-addauthapp.png)

4. Op de **beginnen met het ophalen van de app** weergeeft, schakelt **nu downloaden** te downloaden en installeren van de Microsoft Authenticator-app op uw mobiele apparaat en selecteer vervolgens **volgende**.

    Zie voor meer informatie over het downloaden en installeren van de app, [De Microsoft Authenticator-app downloaden en installeren](user-help-auth-app-download-install.md).

    ![Beginnen met het ophalen van de app-pagina](media/security-info/securityinfo-myprofile-getauthapp.png)

   > [!Note]
   > Als u een andere verificator-app dan de Microsoft Authenticator-app gebruiken wilt, selecteert u de koppeling **Ik wil een andere verificator-app gebruiken**.
   > 
   > Als u van uw organisatie u een andere methode dan de authenticator-app kiezen mag, kunt u de **Ik wil andere methode instellen-koppeling** selecteren.

5. Blijven op de **Uw account instellen**-pagina tijdens het instellen van de Microsoft Authenticator-app op uw mobiele apparaat.

    ![De authenticator-app-pagina instellen](media/security-info/securityinfo-myprofile-setupauthapp.png)

6. Open de Microsoft Authenticator-app, selecteer dat u meldingen toestaat (als u hierom wordt gevraagd), selecteer **Account toevoegen** uit het pictogram **Aanpassen en controle** in de rechterbovenhoek en selecteer vervolgens **Werk- of schoolaccount**.

7. Ga terug naar de **instellen van uw account** pagina op uw, en selecteer vervolgens **volgende**.

    De pagina **QR-code scannen** wordt weergegeven.

    ![De QR-code scannen met behulp van de Authenticator-app](media/security-info/securityinfo-myprofile-qrcodeauthapp.png)

6. De opgegeven code scannen met de Microsoft Authenticator app QR-code lezer, die op uw mobiele apparaat wordt weergegeven nadat u uw werk of school-account hebt gemaakt in stap 6.

    De verificator-app moet nu uw werk of schoolaccount toevoegen zonder aanvullende informatie van u. Als de QR-codelezer de code echter niet lezen kan, kunt u de **Koppeling kan QR-code niet scannen** selecteren en handmatig de code en URL in de Microsoft Authenticator-app invoeren. Zie voor meer informatie over het handmatig toevoegen van een code [Handmatig een account toevoegen aan de app](user-help-auth-app-add-account-manual.md).

7. Selecteer **volgende** op de **QR-code scannen** pagina op uw.

    Een melding wordt verzonden naar de Microsoft Authenticator-app op uw mobiele apparaat voor het testen van uw account.

    ![Uw account met de verificator-app testen](media/security-info/securityinfo-myprofile-tryitauthapp.png)

8. Keur de melding in de Microsoft Authenticator-app goed en selecteer vervolgens **Volgende**.

     ![Melding van geslaagd, met verbinding van de app en uw account](media/security-info/securityinfo-myprofile-successauthapp.png)

     Uw beveiligingsgegevens zijn bijgewerkt voor het standaard gebruik van de Microsoft Authenticator-app om uw identiteit te verifiëren bij het gebruik van verificatie in twee stappen of opnieuw instellen van het wachtwoord.

## <a name="delete-your-authenticator-app-from-your-security-info-methods"></a>De authenticator-app verwijderen uit uw gegevens beveiligingsmethoden
Als u niet meer gebruiken de authenticator-app als een methode-info wilt, kunt u het verwijderen van de **beveiligingsgegevens** pagina. Dit werkt voor alle verificator-apps, niet alleen de Microsoft Authenticator-app. Nadat u de app verwijdert, hebt u de authenticator-app op uw mobiele apparaat en het account verwijderen.

>[!Important]
>Als u de authenticator-app per ongeluk verwijdert, is er geen manier om deze ongedaan te maken. Hebt u opnieuw toe te voegen de authenticator-app, de stappen in de [instellen van de authenticator-app](#set-up-the-microsoft-authenticator-app-from-the-security-info-page) sectie van dit artikel.

### <a name="to-delete-the-authenticator-app"></a>De authenticator-app verwijderen

1. Op de **beveiligingsgegevens** weergeeft, schakelt de **verwijderen** koppelen naast de Authenticator-app.

    ![Koppeling naar de authenticator-app uit de beveiligingsgegevens verwijderen](media/security-info/securityinfo-myprofile-deleteauthapp.png)

2. Selecteer **Ja** uit de bevestiging van verwijderen van de authenticator-app. Nadat de authenticator-app wordt verwijderd, wordt deze verwijderd uit je beveiligingsgegevens en, verdwijnt het uit de **beveiligingsgegevens** pagina. Als de authenticator-app uw standaardmethode is, wordt de standaardwaarde op een andere beschikbare methode wijzigen.

3. Open de authenticator-app op uw mobiele apparaat, selecteer **accounts bewerken**, en vervolgens uw werk of school-account verwijderen uit de authenticator-app.

    Uw account wordt volledig verwijderd uit de authenticator-app voor tweeledige verificatie en aanvragen voor wachtwoord opnieuw instellen.

## <a name="change-your-default-security-info-method"></a>De standaard security info methode wijzigen
Als u wilt dat de authenticator-app moet de standaard-methode gebruikt wanneer u zich aanmeldt en uw werk of school-account met behulp van tweeledige verificatie of voor wachtwoord opnieuw aanvragen instellen, kunt u het instellen van de beveiliging **info** pagina.

### <a name="to-change-your-default-security-info-method"></a>De standaard security info methode wijzigen

1. Op de **beveiligingsgegevens** weergeeft, schakelt de **wijziging** koppelen naast de **standaard aanmeldingsmethode** informatie.

    ![Koppeling voor de aanmeldingsmethode standaard wijzigen](media/security-info/securityinfo-myprofile-changedefaultauthapp.png)

2. Kies **Microsoft Authenticator - melding** in de vervolgkeuzelijst lijst van beschikbare methoden. Als u de Microsoft Authenticator-app niet gebruikt, selecteert u de **Authenticator-app of hardware token** optie.

    ![Methode voor aanmelding bij de standaard kiezen](media/security-info/securityinfo-myprofile-defaultauthapp.png)

3. Selecteer **bevestigen**.

    De standaardmethode die wordt gebruikt voor aanmelding bij wijzigingen in de Microsoft Authenticator-app.

## <a name="additional-security-info-methods"></a>Aanvullende informatie voor beveiligingsmethoden
Hebt u extra opties voor hoe uw organisatie contactpersonen u om te controleren of uw identiteit, afhankelijk van wat u probeert te doen. De opties zijn:

- **Tekst van mobiele apparaten.** Voer het nummer van uw mobiele apparaat en ontvang een SMS-bericht een code die u voor verificatie in twee stappen of het wachtwoord gebruikt opnieuw instellen. Zie voor stapsgewijze instructies over hoe u uw identiteit bevestigen met een tekstbericht (SMS) [beveiligingsgegevens instellen om te gebruiken functie voor tekstberichten (SMS)](security-info-setup-text-msg.md).

- **Mobiele apparaten of werk telefoongesprekken.** Voer het nummer van uw mobiele apparaat en ontvang een telefoonoproep voor verificatie in twee stappen verificatie of het wachtwoord opnieuw instellen. Zie voor stapsgewijze instructies over hoe u uw identiteit verifiëren met een telefoonnummer [beveiligingsgegevens ingesteld voor het gebruik van telefoongesprekken](security-info-setup-phone-number.md).

- **E-mailadres.** Voer uw werk of school-e-mailadres voor het ophalen van een e-mailbericht voor wachtwoord opnieuw instellen. Deze optie is niet beschikbaar voor verificatie in twee stappen. Zie voor stapsgewijze instructies over het instellen van uw e-mailadres [beveiligingsgegevens ingesteld voor het gebruik van e-mailbericht](security-info-setup-email.md).

- **Vragen over de beveiliging.** Sommige gemaakt door uw beheerder voor uw organisatie beveiligingsvragen beantwoorden. Deze optie is alleen beschikbaar voor wachtwoord opnieuw instellen en niet voor verificatie in twee stappen. Zie voor stapsgewijze instructies over het instellen van uw vragen over de beveiliging, de [beveiligingsgegevens ingesteld voor het gebruik van beveiligingsvragen](security-info-setup-questions.md) artikel.
    
    >[!Note]
    >Als sommige van deze opties ontbreekt, is het meest waarschijnlijk omdat uw organisatie niet toegestaan voor deze methoden. Als dit het geval is, moet u een methode voor beschikbaar of neem contact op met uw beheerder voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Stel uw wachtwoord opnieuw in als u dit hebt verloren of bent vergeten, uit het [Portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com/) of volg de stappen in het artikel [Uw werk- of schoolaccount wachtwoord opnieuw instellen](user-help-reset-password.md).

- Krijg tips voor het oplossen van problemen en hulp met problemen met aanmelden in het artikel [U kunt niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).