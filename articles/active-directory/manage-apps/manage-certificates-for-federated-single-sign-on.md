---
title: Federatiecertificaten beheren in Azure AD | Microsoft Docs
description: Informatie over het aanpassen van de vervaldatum voor de federatiecertificaten en het vernieuwen van certificaten die binnenkort vervallen.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: mimart
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 416e72f7e9e8622f044f7bb6430fbb36010b164e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67701974"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren

In dit artikel behandelen we veelvoorkomende vragen en informatie met betrekking tot certificaten dat Azure Active Directory (Azure AD) wordt gemaakt voor het maken van federatieve eenmalige aanmelding (SSO) met de software als een service (SaaS)-toepassingen. Toepassingen toevoegen vanuit de galerie met Azure AD-app of met behulp van de sjabloon van een toepassing buiten de galerie. De toepassing configureren met behulp van de optie voor federatieve eenmalige aanmelding.

In dit artikel is alleen relevant voor apps die zijn geconfigureerd voor het gebruik van Azure AD-eenmalige aanmelding via [Security Assertion Markup Language](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) federation (SAML).

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatisch gegenereerde certificaat voor de galerie en niet toepassingen

Wanneer u een nieuwe toepassing uit de galerie toevoegen en configureren van een aanmelding op basis van SAML (hiervoor **eenmalige aanmelding** > **SAML** overzichtspagina voor de toepassing), genereert Azure AD een certificaat voor de toepassing die is geldig gedurende drie jaar. Voor het downloaden van het actieve certificaat als een beveiligingscertificaat ( **.cer**)-bestand, Ga terug naar die pagina (**SAML gebaseerde aanmelding**) en selecteer een downloadkoppeling in de **SAML-handtekeningcertificaat** kop. U kunt kiezen tussen het certificaat met onbewerkte gegevens (binaire) of het certificaat van Base64 (base 64 gecodeerde tekenreeks). Voor galerietoepassingen, weergeven in deze sectie mogelijk ook een koppeling naar het certificaat downloaden als federatiemetagegevens XML (een **.xml** bestand), afhankelijk van de vereiste van de toepassing.

![SAML active ondertekenen certificaat Downloadinstellingen](./media/manage-certificates-for-federated-single-sign-on/active-certificate-download-options.png)

U kunt ook een actieve of inactieve-certificaat downloaden door te selecteren de **SAML-handtekeningcertificaat** van de kop **bewerken** pictogram (een potlood), waarin de **SAML-handtekeningcertificaat** pagina. Selecteer het weglatingsteken ( **...** ) naast het certificaat dat u wilt downloaden, en kies vervolgens welke certificaatindeling dat u wilt. U hebt de extra optie voor het downloaden van het certificaat in de indeling van mailcertificaten met verbeterde beveiliging van e-mail (PEM). Deze indeling is identiek aan de met Base64, maar met een **.pem** bestandsnaamextensie wordt niet herkend in Windows als een certificaatindeling.

![SAML-handtekeningcertificaten certificaat Downloadinstellingen (actieve en inactieve)](./media/manage-certificates-for-federated-single-sign-on/all-certificate-download-options.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>De vervaldatum voor het certificaat van uw federatieve aanpassen en meenemen naar een nieuw certificaat

Standaard configureert Azure een certificaat om te verlopen na drie jaar wanneer het automatisch tijdens de eenmalige SAML-aanmelding configuratie gemaakt wordt. Omdat u kunt de datum van een certificaat niet wijzigen nadat u deze opslaat, wordt u moet hebben:

1. Maak een nieuw certificaat met de gewenste datum.
1. Sla het nieuwe certificaat.
1. Download het nieuwe certificaat in de juiste indeling.
1. Upload het nieuwe certificaat naar de toepassing.
1. Het nieuwe certificaat actief zijn in de Azure Active Directory-portal maken.

De volgende twee secties kunnen u deze stappen uitvoert.

### <a name="create-a-new-certificate"></a>Een nieuw certificaat maken

Eerst maken en opslaan van nieuw certificaat met een andere vervaldatum:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/). De **Azure Active Directory-beheercentrum** pagina wordt weergegeven.
1. Selecteer in het linkerdeelvenster **Enterprise-toepassingen**. Er wordt een lijst weergegeven van de enterprise-toepassingen in uw account.
1. Selecteer de betreffende toepassing. Een overzichtspagina voor de toepassing wordt weergegeven.
1. Selecteer in het linkerdeelvenster van de overzichtspagina van de toepassing, **eenmalige aanmelding**.
1. Als de **selecteert u een methode voor eenmalige aanmelding** pagina wordt weergegeven, selecteert u **SAML**.
1. In de **instellen van eenmalige aanmelding met SAML - Preview** pagina, zoek de **SAML-handtekeningcertificaat** kop en selecteer de **bewerken** pictogram (een potlood). De **SAML-handtekeningcertificaat** pagina wordt weergegeven, wordt de status (**Active** of **inactief**), de vervaldatum en vingerafdruk (een hash-tekenreeks) van elk certificaat.
1. Selecteer **nieuw certificaat**. Een nieuwe rij wordt weergegeven onder de lijst met certificaten, waarbij de vervaldatum standaard ingesteld op exact drie jaar na de huidige datum. (Uw wijzigingen nog niet hebt opgeslagen, zodat u de vervaldatum nog steeds kunt wijzigen.)
1. In het nieuwe certificaatrij, Beweeg de muisaanwijzer over de datumkolom verlopen en selecteer de **Selecteer datum** pictogram (een kalender). Een kalenderbesturingselement weergegeven met de dagen van een maand van de huidige vervaldatum van de nieuwe rij.
1. Gebruik het besturingselement agenda een nieuwe datum in te stellen. U kunt een waarde zijn tussen de huidige datum en drie jaar na de huidige datum instellen.
1. Selecteer **Opslaan**. Het nieuwe certificaat wordt nu weergegeven met de status van **inactief**, de vervaldatum datum die u hebt gekozen, en een vingerafdruk.
1. Selecteer de **X** om terug te keren naar de **instellen van eenmalige aanmelding met SAML - Preview** pagina.

### <a name="upload-and-activate-a-certificate"></a>Uploaden en activeren van een certificaat

Vervolgens het nieuwe certificaat in de juiste indeling downloaden, uploaden naar de toepassing en in Azure Active Directory te activeren:

1. Weergeven van de toepassing extra de SAML-aanmelding configuratie-instructies door:

   - selecteren van de **configuratiehandleiding** koppeling om weer te geven in een nieuw browservenster of tabblad, of
   - gaan de **instellen** heen gaat en selecteren **Stapsgewijze instructies bekijken over** om weer te geven in een zijbalk.

1. Houd er rekening mee de coderingsindeling die is vereist voor het uploaden van het certificaat in de instructies.
1. Volg de instructies in de [automatisch gegenereerde certificaat voor de galerie en buiten de galerie toepassingen](#auto-generated-certificate-for-gallery-and-non-gallery-applications) eerdere sectie. Deze stap downloadt het certificaat in de coderingsindeling die is vereist voor het uploaden van voor de toepassing.
1. Als u meenemen naar het nieuwe certificaat wilt, gaat u terug naar de **SAML-handtekeningcertificaat** pagina en in de certificaatrij van de nieuw opgeslagen, selecteer het weglatingsteken ( **...** ) en selecteer **certificaat activeren**. Verandert de status van het nieuwe certificaat in **Active**, en het eerder actieve certificaat verandert in een status van **inactief**.
1. Volgende van de toepassing SAML-aanmelding configuratie-instructies die u eerder weergegeven zodat u het ondertekenen van SAML uploaden kunt-in de juiste coderingsindeling certificaat blijven.

## <a name="add-email-notification-addresses-for-certificate-expiration"></a>Scheid e-mailadressen melding voor het certificaat verloopt

Azure AD stuurt een e-mailbericht melding 60, 30 en 7 dagen voordat het SAML-certificaat is verlopen. U kunt meer dan één e-mailadres voor het ontvangen van meldingen toevoegen. Geef het e-mailadressen die u wilt dat de meldingen worden verzonden naar:

1. In de **SAML-handtekeningcertificaat** pagina, Ga naar de **melding e-mailadressen** kop. Deze kop gebruikt standaard alleen de e-mailadres van de beheerder die de toepassing toegevoegd.
1. Typ onder het laatste e-mailadres het e-mailadres dat moet ontvangen bericht met een vervaldatum van het certificaat, en druk op Enter.
1. Herhaal de vorige stap voor elke e-mailadres dat u wilt toevoegen.
1. Voor elke e-mailadres dat u wilt verwijderen, selecteert u de **verwijderen** pictogram (een garbagecollection kan) naast het e-mailadres.
1. Selecteer **Opslaan**.

U ontvangt de e-mailmelding van aadnotification@microsoft.com. Om te voorkomen dat het e-mailbericht naar uw locatie spam, voegt u dit e-mailbericht toe aan uw contactpersonen.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Een certificaat verlengen dat binnenkort vervallen

Als een certificaat bijna verlopen is, kunt u met behulp van een procedure die in geen significante downtime voor uw gebruikers resulteert kunt vernieuwen. Een verlopen certificaat vernieuwen:

1. Volg de instructies in de [Maak een nieuw certificaat](#create-a-new-certificate) sectie eerder, met behulp van een datum die overlapt met het bestaande certificaat. Deze datum beperkt de hoeveelheid uitvaltijd door het verlopen certificaat.
1. Als de toepassing kan automatisch rollover van een certificaat, stelt u het nieuwe certificaat actief door de volgende stappen:
   1. Ga terug naar de **SAML-handtekeningcertificaat** pagina.
   1. Selecteer in de certificaatrij van de nieuw opgeslagen, het weglatingsteken ( **...** ) en selecteer vervolgens **certificaat activeren**.
   1. De volgende twee stappen overslaan.

1. Als de app slechts één certificaat tegelijk verwerken kan, kiest u een interval van downtime om uit te voeren van de volgende stap. (Anders als de toepassing niet automatisch het nieuwe certificaat ophalen, maar meer dan één handtekeningcertificaat kan verwerken, kunt u uitvoeren de volgende stap op elk gewenst moment.)
1. Voordat u het oude certificaat is verlopen, volg de instructies in de [uploaden en activeren van een certificaat](#upload-and-activate-a-certificate) eerdere sectie.
1. Meld u aan de toepassing om ervoor te zorgen dat het certificaat correct werkt.

## <a name="related-articles"></a>Verwante artikelen:

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](../saas-apps/tutorial-list.md)
- [Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](what-is-single-sign-on.md)
- [Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md)
