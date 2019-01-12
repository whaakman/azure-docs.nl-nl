---
title: Procedures voor het terugschrijven van wachtwoorden configureren voor Azure AD SSPR
description: Gebruik Azure AD en Azure AD Connect terugschrijven van wachtwoorden met een on-premises directory
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: f64331c2b75df84f44486f192e17a744b43fcf9b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244899"
---
# <a name="how-to-configure-password-writeback"></a>Uitleg: Configureer wachtwoord terugschrijven

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

> [!WARNING]
> Wachtwoord terugschrijven wordt niet meer voor klanten die werken met Azure AD Connect-versies 1.0.8641.0 en oudere wanneer de [Azure Access Control service (ACS) op 7 November 2018 buiten gebruik is gesteld](../develop/active-directory-acs-migration.md). Azure AD Connect-versies 1.0.8641.0 en ouder wordt geen bestanden meer worden het terugschrijven van wachtwoorden op dat moment omdat ze afhankelijk van ACS voor deze functionaliteit zijn.
>
> Om te voorkomen dat een onderbreking in de service, een upgrade uitvoeren van een eerdere versie van Azure AD Connect naar een nieuwere versie, Zie het artikel [Azure AD Connect: Upgrade van een vorige versie naar de nieuwste versie](../hybrid/how-to-upgrade-previous-version.md)
>

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor wachtwoord terugschrijven

**Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen met on-premises Write-back is een premium-functie van Azure AD**. Zie voor meer informatie over de licentieverlening voor de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

Voor het gebruik van het terugschrijven van wachtwoorden, moet u een van de volgende licenties zijn toegewezen op uw tenant hebben:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 of A3
* Enterprise Mobility + Security E5 of A5
* Microsoft 365 E3 of A3
* Microsoft 365 E5 of A5
* Microsoft 365 F1
* Microsoft 365 Business

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening *bieden geen ondersteuning voor 'Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen met on-premises Write-back van'* en vereisen dat u met een van de voorgaande plannen voor deze functionaliteit te werken.
>

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
6. In de **is van toepassing op** vervolgkeuzelijst, selecteer **Descendant gebruikersobjecten**.
7. Onder **machtigingen**, schakelt u de selectievakjes voor de volgende opties:
    * **Wachtwoord wijzigen**
    * **Wachtwoord opnieuw instellen**
8. Onder **eigenschappen**, schakelt u de selectievakjes voor de volgende opties:
    * **LockoutTime schrijven**
    * **PwdLastSet schrijven**
9. Selecteer **toepassen/OK** de wijzigingen toepassen en afsluiten van alle geopende dialoogvensters.

## <a name="next-steps"></a>Volgende stappen

[Wat is wachtwoord terugschrijven?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Wachtwoord terugschrijven inschakelen in Azure AD Connect"
