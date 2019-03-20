---
title: Lokalisatie tekenreeks id's - Azure Active Directory B2C | Microsoft Docs
description: Geef de id's voor een inhoudsdefinitie met de Id van api.signuporsignin in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5f937a86e4e063ad1e8346dc629a917ae7ce8b65
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58077204"
---
# <a name="localization-string-ids"></a>Gelokaliseerde tekenreeks id 's

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **lokalisatie** element kunt u ter ondersteuning van meerdere landinstellingen of talen in het beleid voor de gebruiker reizen. Dit artikel bevat de lijst met lokalisatie-id's die u in uw beleid gebruiken kunt. Als u bekend bent met lokalisatie van de gebruikersinterface, Zie [lokalisatie](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Meld u aan of aanmelden pagina-elementen

De volgende id's worden gebruikt voor een inhoudsdefinitie met de ID van `api.signuporsignin`.

| Id | Standaardwaarde |
| -- | ------------- |
| **local_intro_email** | Aanmelden met uw bestaande account |
| **logonIdentifier_email** | E-mailadres |
| **requiredField_email** | Voer uw e-mailadres in |
| **invalid_email** | Voer een geldig e-mailadres in |
| **email_pattern** | ^[a-zA-Z0-9.!#$%&’'*+/=?^_\`{\|}~-]+@[a-zA-Z0-9-]+(?:\\.[a-zA-Z0-9-]+)*$ |
| **local_intro_username** | Aanmelden met uw gebruikersnaam |
| **logonIdentifier_username** | Gebruikersnaam |
| **requiredField_username** | Voer uw gebruikersnaam in |
| **Wachtwoord** | Wachtwoord |
| **requiredField_password** | Voer uw wachtwoord in |
| **invalid_password** | Het ingevoerde wachtwoord heeft niet de verwachte indeling. |
| **forgotpassword_link** | Wachtwoord vergeten? |
| **createaccount_intro** | Hebt u geen account? |
| **createaccount_link** | Nu registreren |
| **divider_title** | OF |
| **cancel_message** | De gebruiker is het wachtwoord vergeten |
| **button_signin** | Aanmelden |
| **social_intro** | Aanmelden met uw sociale account |
  **remember_me** |Aangemeld blijven|
| **unknown_error** | Er zijn problemen met het aanmelden. Probeer het later opnieuw. |

Het volgende voorbeeld ziet u het gebruik van een aantal van de gebruiker de gebruikersinterface-elementen op de pagina voor registreren of aanmelden:

![Meld u aan of aanmelden pagina UX-elementen](./media/localization-string-ids/localization-susi.png)

De ID van de id-providers is geconfigureerd in de gebruikersbeleving **ClaimsExchange** element. Om te lokaliseren van de titel van de id-provider, de **ElementType** is ingesteld op `ClaimsProvider`, terwijl de **StringId** is ingesteld op de ID van de `ClaimsExchange`.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

De Facebook-id-provider op Arabisch is vertaald in het volgende voorbeeld:

```XML
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

## <a name="sign-up-or-sign-in-error-messages"></a>Meld u aan of aanmelden foutberichten

| Id | Standaardwaarde |
| -- | ------------- |
| **UserMessageIfInvalidPassword** | Uw wachtwoord is onjuist. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Uw account is niet gevonden. |
| **UserMessageIfOldPasswordUsed** | Het lijkt erop dat u een oud wachtwoord hebt gebruikt. |  
| **DefaultMessage** | Ongeldige gebruikersnaam of wachtwoord. |  
| **UserMessageIfUserAccountDisabled** | Uw account is vergrendeld. Neem contact op met uw ondersteuningsmedewerker om te heffen en probeer het opnieuw. |  
| **UserMessageIfUserAccountLocked** | Uw account is tijdelijk vergrendeld om te voorkomen dat het gebruik door onbevoegden. Probeer het later opnieuw. |  
| **AADRequestsThrottled** | Er zijn te veel aanvragen op dit moment. Wacht even en probeer het opnieuw. |  

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Meld u aan en zelfondertekend door de bevestigde pagina's gebruikersinterface-elementen

Hieronder vindt u de id's voor een inhoudsdefinitie met de ID van `api.localaccountsignup` of een inhoudsdefinitie die met begint `api.selfasserted`, zoals `api.selfasserted.profileupdate` en `api.localaccountpasswordreset`.

| Id | Standaardwaarde |
| -- | ------------- |
| **ver_sent** | De verificatiecode is verzonden naar: |
| **ver_but_default** | Standaard |
| **cancel_message** | De gebruiker heeft geannuleerd zelf-gecontroleerde gegevens invoeren |
| **preloader_alt** | Een ogenblik geduld |
| **ver_but_send** | Verificatiecode verzenden |
| **alert_yes** | Ja |
| **error_fieldIncorrect** | Een of meer velden zijn niet juist ingevuld. Controleert u de invoer en probeer het opnieuw. |
| **year** | Jaar |
| **verifying_blurb** | De gegevens worden verwerkt. Een ogenblik geduld... |
| **button_cancel** | Annuleren |
| **ver_fail_no_retry** | U hebt aangebracht te veel onjuiste aanmeldingspogingen. Probeer het later opnieuw. |
| **Maand** | Maand |
| **ver_success_msg** | E-mailadres is geverifieerd. U kunt nu doorgaan. |
| **months** | januari, februari, maart, april, mei, juni, juli, augustus, september, oktober, november, december |
| **ver_fail_server** | Er zijn problemen met het controleren van uw e-mailadres. Voer een geldig e-mailadres en probeer het opnieuw. |
| **error_requiredFieldMissing** | Er ontbreekt een vereist veld. Vul alle verplichte velden en probeer het opnieuw. |
| **initial_intro** | Geef de volgende gegevens op. |
| **ver_but_resend** | Nieuwe code verzenden |
| **button_continue** | Maken |
| **error_passwordEntryMismatch** | De vermelding wachtwoordvelden komen niet overeen. Voer hetzelfde wachtwoord in beide velden in en probeer het opnieuw. |
| **ver_incorrect_format** | Onjuiste indeling. |
| **ver_but_edit** | E-mailadres wijzigen |
| **ver_but_verify** | Code verifiëren |
| **alert_no** | Nee |
| **ver_info_msg** | Verificatiecode is verzonden naar uw postvak in. Kopieer deze naar het invoervak hieronder. |
| **dag** | Dag |
| **ver_fail_throttled** | Er zijn te veel aanvragen om te controleren of dit e-mailadres. Wacht even en probeer het opnieuw. |
| **helplink_text** | Wat is dit? |
| **ver_fail_retry** | Deze code is onjuist. Probeer het opnieuw. |
| **alert_title** | Invoeren van uw gegevens annuleren |
| **required_field** | Deze informatie is vereist. |
| **alert_message** | Weer u zeker dat u het invoeren van uw gegevens wilt annuleren? |
| **ver_intro_msg** | Verificatie is nodig. Klik op verzenden klikt. |
| **ver_input** | Verificatiecode |

## <a name="sign-up-and-self-asserted-pages-error-messages"></a>Meld u aan en zelfondertekend door de bevestigde pagina's foutberichten

| Id | Standaardwaarde |
| -- | ------------- |
| **UserMessageIfClaimsPrincipalAlreadyExists** | Een gebruiker met de opgegeven ID bestaat al. Kies een ander account. |
| **UserMessageIfClaimNotVerified** | De claim is niet geverifieerd: {0} |
| **UserMessageIfIncorrectPattern** | Onjuiste patroon voor: {0} |
| **UserMessageIfMissingRequiredElement** | Ontbrekende vereiste element: {0} |
| **UserMessageIfValidationError** | Fout bij de validatie door: {0} |
| **UserMessageIfInvalidInput** | {0} heeft ongeldige invoer. |
| **ServiceThrottled** | Er zijn te veel aanvragen op dit moment. Wacht even en probeer het opnieuw. |

Het volgende voorbeeld ziet u het gebruik van een aantal van de gebruikersinterface-elementen in de pagina voor het registreren:

![Meld u aan pagina UX-elementen](./media/localization-string-ids/localization-sign-up.png)

Het volgende voorbeeld ziet u het gebruik van een aantal van de gebruikersinterface-elementen in de pagina voor het registreren, nadat de gebruiker klikt op de knop code verificatie verzenden:

![Meld u aan pagina e-mailbericht verificatie UX-elementen](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Phone factor authentication-pagina gebruiker gebruikersinterface-elementen

Hieronder vindt u de id's voor een inhoudsdefinitie met de ID van `api.phonefactor`. 

| Id | Standaardwaarde |
| -- | ------------- |
| **button_verify** | Mij bellen |
| **country_code_label** | Landnummer |
| **cancel_message** | De gebruiker is multi-factor authentication geannuleerd |
| **text_button_send_second_code** | verzend een nieuwe code |
| **code_pattern** | \\d{6} |
| **intro_mixed** | We hebben het volgende nummer voor de record voor u. We kunnen een code via SMS of telefoon om u te verifiëren. |
| **intro_mixed_p** | We hebben de volgende getallen in de record voor u. Kies een getal dat we kunnen telefoon of een code via SMS om u te verifiëren. |
| **button_verify_code** | Code verifiëren |
| **requiredField_code** | Voer de verificatiecode in die u hebt ontvangen |
| **invalid_code** | Voer de code van zes cijfers in die u hebt ontvangen |
| **button_cancel** | Annuleren |
| **local_number_input_placeholder_text** | Telefoonnummer |
| **button_retry** | Opnieuw proberen |
| **alternative_text** | Ik heb mijn telefoon niet bij me |
| **intro_phone_p** | We hebben de volgende getallen in de record voor u. Kies een getal dat we kunt telefoon om u te verifiëren. |
| **intro_phone** | We hebben het volgende nummer voor de record voor u. We zullen telefoon om u te verifiëren. |
| **enter_code_text_intro** | Voer hieronder uw verificatiecode in of  |
| **intro_entry_phone** | Voer een nummer in waarop u kunt worden gebeld om uw account te verifiëren. |
| **intro_entry_sms** | Voer hieronder een nummer in waarnaar via sms een code kan worden verzonden om uw account te verifiëren. |
| **button_send_code** | Code verzenden |
| **invalid_number** | Voer een geldig telefoonnummer in |
| **intro_sms** | We hebben het volgende nummer voor de record voor u. We sturen een code via SMS om u te verifiëren. |
| **intro_entry_mixed** | Voer hieronder een nummer in waarop u kunt worden gebeld of waarnaar via sms een code kan worden verzonden om uw account te verifiëren. |
| **number_pattern** | ^\\+ (?: [0-9] [\\x20-]?) {6,14}[0-9] $ |
| **intro_sms_p** |We hebben de volgende getallen in de record voor u. Kies een getal dat we een code via SMS om u te verifiëren kunnen sturen. |
| **requiredField_countryCode** | Selecteer uw landcode |
| **requiredField_number** | Voer uw telefoonnummer in |
| **country_code_input_placeholder_text** |Land of regio |
| **number_label** | Telefoonnummer |
| **error_tryagain** | Het telefoonnummer dat u hebt opgegeven is bezet of niet beschikbaar. Controleer het nummer en probeer het opnieuw. |
| **error_incorrect_code** | De ingevoerde verificatiecode komt niet overeen met onze records. Probeer het opnieuw, of een nieuwe code aanvraagt. |
| **countryList** | {\"Standaard\":\"land/regio\",\"AF\":\"Afghanistan\",\"AX\":\"Åland Eilanden\",\"AL\":\"Albanië\",\"DZ\":\"Algerije\",\"AS\":\" Amerikaans-Samoa\",\"AD\":\"Andorra\",\"door de AO\":\"Angola\",\"AI\": \"Anguilla\",\"AQ\":\"Antarctica\",\"AG\":\"Antigua en Barbuda\",\"AR\":\"Argentinië\",\"AM\":\"Armenië\",\"AW\":\"Aruba \",\"AU\":\"Australia\",\"op\":\"Oostenrijk\",\" AZ\":\"Azerbeidzjan\",\"BS\":\"Bahama's\",\"BH\":\" Bahrein\",\"BD\":\"Bangladesh\",\"BB\":\"Barbados\",\" DOOR\":\"Wit-Rusland\",\"BE\":\"België\",\"BZ\":\" Belizaanse\",\"BJ\":\"Benin\",\"BM\":\"Bermuda\",\"BT\":\"Bhutan\",\"BO\":\"Bolivia\",\"BQ\":\" Bonaire\",\"BA\":\"Bosnië en Herzegovina\",\"BW\":\"Botswaanse<span class="notransla class=""></span class="notransla> Afgelegen eilanden\",\"VI\":\"VS Britse Maagdeneilanden\",\"mg\":\"Oeganda\",\"UA\":\"Oekraïne\",\"AE\":\" Verenigde Arabische Emiraten\",\"GB\":\"Verenigd Koninkrijk\",\"VS\":\"Verenigde Staten\",\"UY \":\"Uruguay\",\"UZ\":\"Oezbekistan\",\"VU\":\"Vanuatu\", \"VA\":\"Vaticaanstad\",\"VE\":\"Venezuela\",\"VN\":\"Vietnam \",\"WF\":\"Wallis en Futuna\",\"YE\":\"Jemen\",\"ZM\":\"Zambia\",\"ZW\":\"Zimbabwe\"} |
| **error_448** | Het nummer dat u hebt opgegeven, is niet beschikbaar. |
| **error_449** | De gebruiker heeft het maximum aantal pogingen overschreden. |
| **verification_code_input_placeholder_text** | Verificatiecode |

Het volgende voorbeeld ziet u het gebruik van een aantal van de gebruikersinterface-elementen op de pagina met MFA-inschrijving:

![Meld u aan pagina e-mailbericht verificatie UX-elementen](./media/localization-string-ids/localization-mfa1.png)

Het volgende voorbeeld ziet u het gebruik van een aantal van de gebruikersinterface-elementen op de pagina met MFA-validatie:

![Meld u aan pagina e-mailbericht verificatie UX-elementen](./media/localization-string-ids/localization-mfa2.png)







