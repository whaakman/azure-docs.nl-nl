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
ms.openlocfilehash: 1ae74f7c43e763962224683954b28e5941136c08
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295815"
---
# <a name="how-to-configure-password-writeback"></a>Instructies: Het terugschrijven van wachtwoorden configureren

Het is raadzaam dat u de functie voor automatisch bijwerken van [Azure AD Connect](../hybrid/how-to-connect-install-express.md) wanneer u het terugschrijven van wachtwoorden.

De volgende stappen wordt ervan uitgegaan dat u Azure AD Connect al hebt geconfigureerd in uw omgeving met behulp van de [Express](../hybrid/how-to-connect-install-express.md) of [aangepaste](../hybrid/how-to-connect-install-custom.md) instellingen.

1. Als u het terugschrijven van wachtwoorden wilt configureren en inschakelen, meld u zich aan bij de Azure AD Connect-server en start u de configuratiewizard **Azure AD Connect**.
2. Selecteer **Configureren** op de **welkomstpagina**.
3. Op de pagina **Extra taken** selecteert u **Synchronisatieopties aanpassen** en vervolgens **Volgende**.
4. Op de pagina **Verbinding maken met Azure AD** voert u de referenties van een globale beheerder in en selecteert u **Volgende**.
5. Op de pagina's **Verbinding maken met directory´s** en **domein/OE filteren** selecteert u **Volgende**.
6. Op de pagina **Optionele functies** schakelt u het selectievakje in naast **Wachtwoord terugschrijven** en selecteert u **Volgende**.
   ![Wachtwoord terugschrijven inschakelen in Azure AD Connect][Writeback]
7. Op de pagina **Gereed om te configureren** selecteert u **Configureren** wacht u tot het proces is voltooid.
8. Als u ziet dat de configuratie is voltooid, selecteert u **Afsluiten**.

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
