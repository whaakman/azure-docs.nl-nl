---
title: Beveiligingsgegevens (preview) gebruiken een SMS-berichten verzenden - Azure Active Directory instellen | Microsoft Docs
description: Over het instellen van je beveiligingsgegevens om uw identiteit met behulp van SMS-berichten verzenden en uw mobiele apparaat te verifiëren.
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
ms.openlocfilehash: a130113c7e6052befdc61927589c1151a062bdb9
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456092"
---
# <a name="set-up-security-info-preview-to-use-text-messaging"></a>Beveiligingsgegevens (preview) instellen voor het gebruik van SMS-berichten verzenden
U kunt deze stappen om toe te voegen uw tweeledige verificatie en methoden voor wachtwoord opnieuw instellen. Nadat u hebt dit tijdens de eerste keer hebt ingesteld, kunt u terugkeren naar de **beveiligingsgegevens** pagina toevoegen, bijwerken of verwijderen van uw beveiligingsgegevens.

Als u wordt gevraagd om in te stellen deze onmiddellijk nadat u zich aanmeldt bij uw account voor werk of school, ziet u de gedetailleerde stappen in de [instellen van je beveiligingsgegevens vanaf de opdrachtprompt aanmeldingspagina](security-info-setup-signin.md) artikel.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Als u een optie niet ziet, is het mogelijk dat uw organisatie niet toe dat u een telefoonnummer voor verificatie gebruiken. In dit geval moet u een andere methode kiezen of neem contact op met uw beheerder voor meer informatie.

## <a name="set-up-text-messages-from-the-security-info-page"></a>SMS-berichten instellen vanuit de pagina beveiligingsgegevens
Afhankelijk van instellingen van uw organisatie, kunt u mogelijk gebruik van SMS-berichten als een van de beveiligingsmethoden voor informatie. De optie tekst bericht is een onderdeel van de optie Telefoon, zodat u alles op dezelfde manier als u dat zou doen voor uw telefoonnummer moet instellen, maar in plaats van dat Microsoft aanroep die u hebt, kiest u het gebruik van een SMS-bericht.

>[!Note]
>Als u een telefonische oproep in plaats van een SMS-bericht ontvangen wilt, volgt u de stappen in de [beveiligingsgegevens ingesteld voor het gebruik van telefoongesprekken](security-info-setup-phone-number.md) artikel.

### <a name="to-set-up-text-messages"></a>Voor het instellen van de SMS-berichten

1. Meld u aan bij uw werk- of schoolaccount en gaat u naar uw https://myprofile.microsoft.com/ pagina.

    ![Mijn profiel-pagina, met gemarkeerde Security info koppelingen](media/security-info/securityinfo-myprofile.png)

2. Selecteer **beveiligingsgegevens** vanuit het navigatiedeelvenster links in of via de koppeling in de **beveiligingsgegevens** blokkeren, en selecteer vervolgens **-methode toevoegen** uit de **beveiligingsgegevens**  pagina.

    ![Pagina beveiligingsgegevens met optie van de methode gemarkeerde toevoegen](media/security-info/securityinfo-myprofile-addmethod.png)

3. Op de **toevoegen van een methode** weergeeft, schakelt **Phone** uit de vervolgkeuzelijst en selecteer vervolgens **toevoegen**.

    ![Vak methode toevoegen met telefoon geselecteerd](media/security-info/securityinfo-myprofile-addphonetext.png)

4. Op de **Phone** pagina, typt u het telefoonnummer voor uw mobiele apparaat, kies **Stuur me een SMS een code**, en selecteer vervolgens **volgende**.

    ![Telefoonnummer toevoegen en kiest u SMS-berichten](media/security-info/securityinfo-myprofile-phonetext-addnumber.png)

5. Typ de code naar u verzonden via SMS-bericht naar uw mobiele apparaat en selecteer vervolgens **volgende**.

    ![Telefoonnummer toevoegen en kiest u SMS-berichten](media/security-info/securityinfo-myprofile-phonetext-entercode.png)

    De paginawijzigingen om uw succes weer te geven.

    ![Aantal geslaagde melding, verbinding te maken van de telefoon, de keuze voor het ontvangen van tekst messags en uw account](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Je beveiligingsgegevens wordt bijgewerkt en kunt u SMS-berichten om uw identiteit te verifiëren bij het gebruik van verificatie in twee stappen verificatie of het wachtwoord opnieuw instellen. Als u wilt maken van uw standaardmethode voor tekstberichten, raadpleegt u de [uw standaardmethode voor de beveiliging-informatie wijzigen](#change-your-default-security-info-method) sectie van dit artikel.

## <a name="delete-text-messaging-from-your-security-info-methods"></a>SMS-berichten van uw informatie beveiligingsmethoden verwijderen
Als u niet meer gebruiken van SMS-berichten als een methode-info wilt, kunt u het verwijderen van de **beveiligingsgegevens** pagina.

>[!Important]
>Als u de SMS-berichten per ongeluk verwijdert, is er geen manier om deze ongedaan te maken. Hebt u op opnieuw, voegt u de methode volgens de stappen in de [instellen van de SMS-berichten](#set-up-text-messages-from-the-security-info-page) sectie van dit artikel.

### <a name="to-delete-text-messaging"></a>Verwijderen van de SMS-berichten verzenden

1. Op de **beveiligingsgegevens** weergeeft, schakelt de **verwijderen** koppelen naast de **Phone** optie.

    ![Koppeling naar de telefoon en de SMS-berichten van de methode van beveiligingsgegevens verwijderen](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Selecteer **Ja** uit de bevestiging van verwijderen van de **Phone** getal. Nadat uw telefoonnummer wordt verwijderd, wordt deze verwijderd uit je beveiligingsgegevens en, verdwijnt het uit de **beveiligingsgegevens** pagina. Als **Phone** is uw standaardmethode, de standaardinstelling wordt gewijzigd in een andere beschikbare methode.

## <a name="change-your-default-security-info-method"></a>De standaard security info methode wijzigen
Als u wilt dat SMS-berichten om de standaard-methode gebruikt wanneer u zich aanmeldt en uw werk of school-account met behulp van tweeledige verificatie of voor wachtwoord opnieuw aanvragen instellen, kunt u het instellen van de **beveiligingsgegevens** pagina.

### <a name="to-change-your-default-security-info-method"></a>De standaard security info methode wijzigen

1. Op de **beveiligingsgegevens** weergeeft, schakelt de **wijziging** koppelen naast de **standaard aanmeldingsmethode** informatie.

    ![Koppeling voor de aanmeldingsmethode standaard wijzigen](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Selecteer **telefoon - tekst (*_your_phone_number_*)** uit de vervolgkeuzelijst met beschikbare methoden en selecteer vervolgens **bevestigen**.

    ![Methode voor aanmelding bij de standaard kiezen](media/security-info/securityinfo-myprofile-phonetext-changeddefault.png)

    De standaardmethode die wordt gebruikt voor aanmelding bij wijzigingen in **telefoon - tekst (*_your_phone_number_*)**.

## <a name="additional-security-info-methods"></a>Aanvullende informatie voor beveiligingsmethoden
Hebt u extra opties voor hoe uw organisatie contactpersonen u om te controleren of uw identiteit, afhankelijk van wat u probeert te doen. De opties zijn:

- **Authenticator-app.** Download en gebruik van een verificator-app voor een goedkeuringsmelding of een willekeurig gegenereerde goedkeuringscode voor verificatie in twee stappen verificatie of het wachtwoord opnieuw instellen. Zie voor stapsgewijze instructies over het instellen en gebruiken van de Microsoft Authenticator-app [beveiligingsgegevens ingesteld voor het gebruik van een verificator-app](security-info-setup-auth-app.md).

- **Mobiele apparaten of werk telefoongesprekken.** Voer het nummer van uw mobiele apparaat en ontvang een telefoonoproep voor verificatie in twee stappen verificatie of het wachtwoord opnieuw instellen. Zie voor stapsgewijze instructies over hoe u uw identiteit verifiëren met een telefoonnummer [beveiligingsgegevens ingesteld voor het gebruik van telefoongesprekken](security-info-setup-phone-number.md).

- **E-mailadres.** Voer uw werk of school-e-mailadres voor het ophalen van een e-mailbericht voor wachtwoord opnieuw instellen. Deze optie is niet beschikbaar voor verificatie in twee stappen. Zie voor stapsgewijze instructies over het instellen van uw e-mailadres [beveiligingsgegevens ingesteld voor het gebruik van e-mailbericht](security-info-setup-email.md).

- **Vragen over de beveiliging.** Sommige gemaakt door uw beheerder voor uw organisatie beveiligingsvragen beantwoorden. Deze optie is alleen beschikbaar voor wachtwoord opnieuw instellen en niet voor verificatie in twee stappen. Zie voor stapsgewijze instructies over het instellen van uw vragen over de beveiliging, de [beveiligingsgegevens ingesteld voor het gebruik van beveiligingsvragen](security-info-setup-questions.md) artikel.
    
    >[!Note]
    >Als sommige van deze opties ontbreekt, is het meest waarschijnlijk omdat uw organisatie niet toegestaan voor deze methoden. Als dit het geval is, moet u een methode voor beschikbaar of neem contact op met uw beheerder voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Uw wachtwoord opnieuw instellen als u hebt verloren of bent vergeten, uit de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com/) of volg de stappen in de [uw werk- of schoolaccount wachtwoord opnieuw instellen](user-help-reset-password.md) artikel.

- Problemen oplossen met tips en Help-informatie voor problemen met aanmelden in de [niet kunt aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artikel.