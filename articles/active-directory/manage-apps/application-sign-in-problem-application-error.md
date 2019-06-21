---
title: Foutbericht wordt weergegeven op de pagina app nadat u zich hebt aangemeld | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen met Azure AD-aanmelding in wanneer de app een foutbericht weergegeven wordt.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: d41ec1f510b028a2ffe2554bfcbd77bc439c4e79
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272949"
---
# <a name="an-app-page-shows-an-error-message-after-the-user-signs-in"></a>Een app-pagina ziet u een foutbericht weergegeven nadat de gebruiker zich aanmeldt

In dit scenario wordt in Azure Active Directory (Azure AD) de gebruiker zich aanmeldt. Maar de toepassing wordt een foutbericht weergegeven en kunt niet de gebruiker die de stroom aanmelden te voltooien. Het probleem is dat de app niet akkoord gaat met de reactie die Azure AD heeft uitgegeven.

Er zijn verschillende mogelijke redenen waarom de app de reactie van Azure AD niet accepteren. Als het foutbericht niet duidelijk wordt aangegeven wat ontbreekt in het antwoord, probeert u het volgende:

-   Als de app de Azure AD-galerie, moet u controleren of u de stappen in gevolgd [fouten opsporen in SAML gebaseerde eenmalige aanmelding tot toepassingen in Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Gebruik een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) om vast te leggen van de SAML-aanvraag, antwoord en -token.

-   Het SAML-antwoord aan de leverancier van de app te verzenden en vraag deze wat ontbreekt.

## <a name="attributes-are-missing-from-the-saml-response"></a>Kenmerken ontbreken in het SAML-antwoord

Als u wilt toevoegen een kenmerk in de Azure AD-configuratie die in de Azure AD-reactie wordt verzonden, als volgt te werk:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer aan de bovenkant van het navigatiedeelvenster aan de linkerkant, **alle services** openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE]
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster.

8. In de **gebruikerskenmerken** sectie, selecteer **weergeven en bewerken van alle andere gebruikerskenmerken**. Hier kunt u wijzigen welke kenmerken moeten worden verzonden naar de app in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Selecteer **kenmerk toevoegen**. Voer de **naam**, en selecteer de **waarde** uit de vervolgkeuzelijst.

   1.  Selecteer **Opslaan**. Hier ziet u het nieuwe kenmerk in de tabel.

9. Sla de configuratie op.

   De volgende keer dat de gebruiker zich aanmeldt bij de app, wordt Azure AD het nieuwe kenmerk in het SAML-antwoord verzonden.

## <a name="the-app-doesnt-identify-the-user"></a>De app identificeren de gebruiker niet

Aanmelden bij de app is mislukt omdat het SAML-antwoord een kenmerk, zoals een rol ontbreekt. Of deze mislukt, omdat de app wordt verwacht dat een andere indeling of een waarde voor de **NameID** kenmerk (gebruikers-id).

Als u [Azure AD geautomatiseerde gebruikersinrichting](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) wilt maken, onderhouden, en gebruikers verwijderen die in de app, controleert u of dat de gebruiker is ingericht voor de SaaS-app. Zie voor meer informatie, [geen gebruikers worden ingericht tot een toepassing met Azure AD-galerie](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-no-users-provisioned).

## <a name="add-an-attribute-to-the-azure-ad-app-configuration"></a>Een kenmerk toevoegen aan de configuratie van de Azure AD-app

De gebruikers-id-waarde wilt wijzigen, de volgende stappen uit:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer **alle services** aan de bovenkant van het navigatiedeelvenster aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE]
   > Als u de app die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster.

8. Onder **gebruikerskenmerken**, selecteert u de unieke id voor de gebruiker de **gebruikers-id** vervolgkeuzelijst.

## <a name="change-the-nameid-format"></a>De NameID-indeling wijzigen

Als de toepassing wordt verwacht dat een andere indeling voor de **NameID** kenmerk (gebruikers-id), Zie [bewerken nameID](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization.md#editing-nameid) te wijzigen van de NameID-indeling.

Azure AD worden geselecteerd voor de indeling voor de **NameID** kenmerk (gebruikers-id) op basis van de waarde die geselecteerd of de indeling die door de app in de SAML-AuthRequest wordt aangevraagd. Zie voor meer informatie de sectie "NameIDPolicy" van [eenmalige aanmelding in de SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol#nameidpolicy).

## <a name="the-app-expects-a-different-signature-method-for-the-saml-response"></a>De app wordt verwacht dat een andere handtekening-methode voor het SAML-antwoord

Als u wilt wijzigen welke onderdelen van het SAML-token digitaal zijn ondertekend door Azure AD, de volgende stappen uit:

1. Open de [Azure-portal](https://portal.azure.com/) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer **alle services** aan de bovenkant van het navigatiedeelvenster aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw apps weer te geven.

   > [!NOTE]
   > Als u de toepassing die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster.

8. Onder **SAML-handtekeningcertificaat**, selecteer **geavanceerde instellingen voor het ondertekenen van certificaat weergeven**.

9. Selecteer de **ondertekening optie** die de app wordt verwacht dat een van deze opties:

   * **SAML-antwoord ondertekenen**
   * **SAML-antwoord en -bewering ondertekenen**
   * **SAML-bewering ondertekenen**

   De volgende keer dat de gebruiker zich aanmeldt bij de app, ondertekent Azure AD het deel van het SAML-antwoord dat u hebt geselecteerd.

## <a name="the-app-expects-the-sha-1-signing-algorithm"></a>De app wordt verwacht dat het algoritme voor ondertekening van SHA-1

Standaard wordt met Azure AD het SAML-token ondertekent met behulp van de meest veilige algoritme. We raden u niet te wijzigen naar het algoritme voor ondertekening *SHA-1* , tenzij de app is SHA-1 vereist.

Als u wilt wijzigen van het algoritme voor ondertekening, de volgende stappen uit:

1. Open de [Azure-portal](https://portal.azure.com/) en meld u aan als een globale beheerder of co-beheerder.

2. Selecteer **alle services** aan de bovenkant van het navigatiedeelvenster aan de linkerkant om te openen van de Azure AD-extensie.

3. Type **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **bedrijfstoepassingen** in het navigatievenster van de Azure AD.

5. Selecteer **alle toepassingen** om een lijst van uw toepassingen weer te geven.

   > [!NOTE]
   > Als u de toepassing die u wilt niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**. Stel de **weergeven** optie "Alle toepassingen."

6. Selecteer de app die u wilt configureren voor eenmalige aanmelding.

7. Nadat de app wordt geladen, selecteert u **eenmalige aanmelding** in het navigatiedeelvenster aan de linkerkant van de app.

8. Onder **SAML-handtekeningcertificaat**, selecteer **geavanceerde instellingen voor het ondertekenen van certificaat weergeven**.

9. Selecteer **SHA-1** als de **handtekeningalgoritme**.

   De volgende keer dat de gebruiker zich aanmeldt bij de app, wordt Azure AD het SAML-token ondertekenen met behulp van het algoritme SHA-1.

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in SAML gebaseerde eenmalige aanmelding tot toepassingen in Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).
