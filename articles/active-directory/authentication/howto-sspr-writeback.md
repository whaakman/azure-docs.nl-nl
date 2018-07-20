---
title: Procedures voor het terugschrijven van wachtwoorden configureren voor Azure AD SSPR
description: Gebruik Azure AD en Azure AD Connect terugschrijven van wachtwoorden met een on-premises directory
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e613ff742096077fe1765d4b855b6c7d409cc228
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158944"
---
# <a name="how-to-configure-password-writeback"></a>Instructies: Het terugschrijven van wachtwoorden configureren

Het is raadzaam dat u de functie voor automatisch bijwerken van [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) wanneer u het terugschrijven van wachtwoorden.

De volgende stappen wordt ervan uitgegaan dat u Azure AD Connect al hebt geconfigureerd in uw omgeving met behulp van de [Express](./../connect/active-directory-aadconnect-get-started-express.md) of [aangepaste](./../connect/active-directory-aadconnect-get-started-custom.md) instellingen.

1. Als u wilt configureren en schakel wachtwoord terugschrijven, meld u aan met uw Azure AD Connect-server en start de **Azure AD Connect** configuratiewizard.
2. Op de **Welkom** weergeeft, schakelt **configureren**.
3. Op de **extra taken** weergeeft, schakelt **aanpassen Synchronisatieopties**, en selecteer vervolgens **volgende**.
4. Op de **verbinding maken met Azure AD** pagina, voer de referenties van een globale beheerder en selecteer vervolgens **volgende**.
5. Op de **verbinding maken met mappen** en **domein/OE** pagina's filteren, selecteert u **volgende**.
6. Op de **optionele functies** pagina, schakel het selectievakje in naast **wachtwoord terugschrijven** en selecteer **volgende**.
   ![Wachtwoord terugschrijven inschakelen in Azure AD Connect][Writeback]
7. Op de **klaar om te configureren** weergeeft, schakelt **configureren** en wacht totdat het proces is voltooid.
8. Wanneer u de configuratie voltooien ziet, selecteert u **afsluiten**.

Voor het oplossen van problemen met taken met betrekking tot het terugschrijven van wachtwoorden, Zie de sectie [problemen met wachtwoord terugschrijven oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) in ons artikel over probleemoplossing.

## <a name="active-directory-permissions"></a>Machtigingen voor Active Directory

Het account dat is opgegeven in het hulpprogramma Azure AD Connect moeten hebben de volgende items als u wilt worden binnen het bereik van self-service voor Wachtwoordherstel:

* **Wachtwoord opnieuw instellen** 
* **Wachtwoord wijzigen** 
* **Schrijfmachtigingen** op `lockoutTime`
* **Schrijfmachtigingen** op `pwdLastSet`
* **Uitgebreide rechten** in elk:
   * Het hoofdobject van *elk domein* in dat forest
   * De gebruiker organisatie-eenheden (OU's) die u wilt in de scope voor SSPR

Als u niet zeker welk account de beschreven account verwijst, open de Azure Active Directory Connect configuratie-UI en selecteer de **de huidige configuratie weergeven** optie. Het account dat u toevoegen van machtigingen wilt voor wordt vermeld onder **mappen gesynchroniseerd**.

Als u deze machtigingen instelt, kan het MA-serviceaccount voor elk forest wachtwoorden beheren namens de gebruikersaccounts in dat forest. 

> [!IMPORTANT]
> Als u niet de volgende machtigingen toe te wijzen, vervolgens terugschrijven weergegeven om te worden geconfigureerd, maar gebruikers worden er fouten optreden bij de poging voor het beheren van hun on-premises wachtwoorden vanuit de cloud.
>

> [!NOTE]
> Het kan duren tot een uur of meer machtigingen zijn gerepliceerd naar alle objecten in uw directory.
>

Om in te stellen op de juiste machtigingen voor het terugschrijven van wachtwoorden worden uitgevoerd, voert u de volgende stappen uit:

1. Open Active Directory: gebruikers en Computers met een account met de juiste machtigingen voor domeinbeheer.
2. Uit de **weergave** menu, zorg ervoor dat **geavanceerde functies** is ingeschakeld.
3. In het linkerdeelvenster met de rechtermuisknop op het object dat staat voor de hoofdmap van het domein en selecteer **eigenschappen** > **Security** > **Geavanceerd**.
4. Uit de **machtigingen** tabblad **toevoegen**.
5. Kies het account dat machtigingen worden toegepast op (van de Azure AD Connect-installatie).
6. In de **is van toepassing op** vervolgkeuzelijst, selecteer **onderliggende gebruiker** objecten.
7. Onder **machtigingen**, selecteert u het volgende:
    * **Wachtwoord opnieuw instellen**
    * **Wachtwoord wijzigen**
    * **LockoutTime schrijven**
    * **PwdLastSet schrijven**
8. Selecteer **toepassen/OK** de wijzigingen toepassen en afsluiten van alle geopende dialoogvensters.

## <a name="next-steps"></a>Volgende stappen

[Wat is wachtwoord terugschrijven?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Wachtwoord terugschrijven inschakelen in Azure AD Connect"
