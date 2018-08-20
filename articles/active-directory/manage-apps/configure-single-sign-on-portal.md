---
title: Eenmalige aanmelding configureren - Azure Active Directory | Microsoft Docs
description: In deze zelfstudie wordt de Azure-portal gebruikt om eenmalige aanmelding voor een toepassing met Azure Active Directory (Azure AD) op basis van SAML te configureren.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 08/09/2018
ms.author: barbkess
ms.reviewer: arvinh,luleon
ms.openlocfilehash: b0180f162996c5fc4647071feaf02d42320b7c9a
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "40036316"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Zelfstudie: Op SAML gebaseerde eenmalige aanmelding voor een toepassing met Azure Active Directory configureren

In deze zelfstudie wordt de [Azure-portal](https://portal.azure.com) gebruikt om eenmalige aanmelding voor een toepassing met Azure Active Directory (Azure AD) op basis van SAML te configureren. Gebruik deze zelfstudie voor het configureren van toepassingen die nog geen [toepassingsspecifieke zelfstudie](../saas-apps/tutorial-list.md) hebben. 


In deze zelfstudie wordt de Azure-portal gebruikt om het volgende te doen:

> [!div class="checklist"]
> * De SAML-gebaseerde modus voor eenmalige aanmelding selecteren
> * Toepassingsspecifiek domein en URL's configureren
> * Gebruikerskenmerken configureren
> * Een SAML-handtekeningcertificaat maken
> * Gebruikers toewijzen aan een toepassing
> * De toepassing voor eenmalige aanmelding op basis van SAML configureren
> * De SAML-instellingen testen

## <a name="before-you-begin"></a>Voordat u begint

1. Als de toepassing niet is toegevoegd aan uw Azure Active Directory-tenant, bekijkt u [Quickstart: een toepassing toevoegen aan uw Azure Active Directory-tenant](add-application-portal.md).

2. Vraag de leverancier van uw toepassing om de gegevens die zijn beschreven in [Domein en URL's configureren](#configure-domain-and-urls).

3. Als u de stappen in deze zelfstudie wilt testen, raden we u aan om een niet-productieomgeving te gebruiken. Als u niet beschikt over een niet-productieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

4. Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

## <a name="select-a-single-sign-on-mode"></a>Een modus voor eenmalige aanmelding selecteren

Nadat een toepassing is toegevoegd aan uw Azure Active Directory-tenant, bent u klaar om eenmalige aanmelding voor de toepassing te configureren.

Hiermee opent u de instellingen voor eenmalige aanmelding:

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Azure Active Directory**. 

2. Klik op de blade **Azure Active Directory** op **Bedrijfstoepassingen**. De blade **Alle toepassingen** wordt geopend met een willekeurige selectie uit de toepassingen in de Azure AD-tenant. 

3. Selecteer in het menu **Toepassingstype** de optie **Alle toepassingen** en klik op **Toepassen**.

4. Voer de naam in van de toepassing waarvoor u eenmalige aanmelding wilt configureren. Kies uw eigen toepassing of gebruik de GitHub-testtoepassing die is toegevoegd in de snelstart [Toepassing toevoegen](add-application-portal.md).

5. Klik op **Eenmalige aanmelding**. Onder **Modus voor eenmalige aanmelding** wordt **SAML-gebaseerde aanmelding** weergegeven als de standaardoptie. 

    ![Configuratie-opties](media/configure-single-sign-on-portal/config-options.png)

6. Klik op **Opslaan** boven aan de blade. 

## <a name="configure-domain-and-urls"></a>Domein en URL's configureren

Om het domein en de URL's te configureren:

1. Neem contact op met de leverancier van de toepassing voor de juiste gegevens voor de volgende instellingen:

    | Configuratie-instelling | SP-geïnitieerd | idP-geïnitieerd | Beschrijving |
    |:--|:--|:--|:--|
    | Aanmeldings-URL | Vereist | Niet specificeren | Wanneer een gebruiker deze URL opent, leidt de serviceprovider hem om naar Azure Active Directory om de gebruiker te verifiëren en aan te melden. Azure Active Directory maakt gebruik van de URL om de toepassing te starten vanuit Office 365 en het toegangsvenster van Azure Active Directory. Als dit veld leeg is, voert Azure Active Directory een idP-geïnitieerde eenmalige aanmelding uit wanneer een gebruiker de toepassing vanuit Office 365, het toegangsvenster van Azure Active Directory of vanuit de URL voor eenmalige aanmelding van Azure Active Directory start.|
    | Id (Entiteits-id) | Vereist voor sommige apps | Vereist voor sommige apps | Unieke identificatie van de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. Azure Active Directory stuurt de id terug naar de toepassing als parameter van de doelgroep van het SAML-token en er wordt verwacht dat de toepassing deze valideert. Deze waarde wordt ook weergegeven als de entiteit-ID in SAML-metagegevens die worden geleverd door de toepassing.|
    | Antwoord-URL | Optioneel | Vereist | Hiermee geeft u op waar de toepassing verwacht het SAML-token te ontvangen. De antwoord-URL wordt ook de ACS-URL (Assertion Consumer Service) genoemd. |
    | Relaystatus | Optioneel | Optioneel | Hiermee geeft u aan de toepassing op waar de gebruiker naar moet worden omgeleid nadat de verificatie is voltooid. De waarde is doorgaans een geldige URL voor de toepassing, maar sommige toepassingen gebruiken dit veld op een andere manier. Vraag de leverancier van de toepassing voor meer informatie.

2. Voer de gegevens in. Om alle instellingen te zien, klikt u op **Geavanceerde URL-instellingen weergeven**.

    ![Configuratie-opties](media/configure-single-sign-on-portal/config-urls.png)

3. Klik op **Opslaan** boven aan de blade.

4. In deze sectie ziet u de knop **SAML-testinstellingen**. Voer deze test verderop in de zelfstudie uit, in de sectie [Eenmalige aanmelding testen](#test-single-sign-on).

## <a name="configure-user-attributes"></a>Gebruikerskenmerken configureren

Met gebruikerskenmerken kunt u bepalen welke Azure Active Directory-gegevens worden verzonden naar de toepassing. Azure Active Directory kan bijvoorbeeld naam, e-mail en werknemer-id van de gebruiker verzenden naar de toepassing. Azure Active Directory stuurt telkens wanneer een gebruiker zich aanmeldt kenmerken van de gebruiker naar de toepassing in het SAML-token. 

Deze kenmerken kunnen verplicht of optioneel zijn om eenmalige aanmelding correct te laten werken. Zie voor meer informatie de [Toepassingsspecifieke zelfstudie](../saas-apps/tutorial-list.md), of neem contact op met de leverancier van de toepassing.

1. Als u alle opties wilt weergeven, klikt u op **Alle gebruikerskenmerken weergeven en bewerken**.

    ![Gebruikerskenmerken configureren](media/configure-single-sign-on-portal/config-user-attributes.png)

2. Voer de **Gebruikers-id** in.

    De gebruikers-id is uniek voor elke gebruiker in de toepassing. Als het e-mailadres bijvoorbeeld zowel de gebruikersnaam als de unieke id is, wordt de waarde ingesteld op *user.mail*.

3. Klik voor meer SAML-tokenkenmerken op **Alle gebruikerskenmerken weergeven en bewerken**.

4. Om een kenmerk toe te voegen aan de **SAML-tokenkenmerken**, klikt u op **Kenmerk toevoegen**. Voer de **Naam** in en selecteer de **Waarde** in het menu.

5. Klik op **Opslaan**. U kunt het nieuwe kenmerk in de tabel zien.
 
## <a name="create-a-saml-signing-certificate"></a>Een SAML-handtekeningcertificaat maken

Azure Active Directory maakt gebruik van een certificaat voor het ondertekenen van de SAML-tokens die worden verzonden naar de toepassing. 

1. Voor weergave van alle opties klikt u op **Geavanceerde opties voor certificaatondertekening weergeven**.

    ![Certificaten configureren](media/configure-single-sign-on-portal/config-certificate.png)

2. Als u een certificaat wilt configureren, klikt u op **Nieuw certificaat maken**.

3. In de blade **Nieuw certificaat maken** stelt u de vervaldatum in en klikt u op **Opslaan**.

4. Klik op **Nieuw certificaat activeren**.

5. Zie voor meer informatie [Geavanceerde opties voor certificaatondertekening](certificate-signing-options.md).

6. Om de wijzigingen die u tot nu toe hebt aangebracht te behouden, moet u klikken op **Opslaan** aan de bovenkant van de blade **Eenmalige aanmelding**. 

## <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan een toepassing

Microsoft raadt aan de eenmalige aanmelding met verschillende gebruikers of groepen te testen vóór het uitrollen van de toepassing in uw organisatie.

Een gebruiker of groep toewijzen aan de toepassing:

1. Open de toepassing in de portal, als deze nog niet is geopend.
2. Klik in de toepassingsblade links op **Gebruikers en groepen**.
3. Klik op **Gebruiker toevoegen**.
4. In de blade **Toewijzing toevoegen** klikt u op **Gebruikers en groepen**.
5. Als u een specifieke gebruiker zoekt, typt u de naam van de gebruiker in het vak **Selecteren** en klikt u op het selectievakje naast de profielfoto van de gebruiker of het logo en klikt u op **Selecteren**. 
6. Zoek uw huidige gebruikersnaam en selecteer deze. U kunt desgewenst meer gebruikers selecteren.
7. In de blade **Toewijzing toevoegen** klikt u op **Toewijzen**. Wanneer dit is voltooid, worden de geselecteerde gebruikers weergegeven in de lijst **Gebruikers en groepen**.

## <a name="configure-the-application-to-use-azure-ad"></a>De toepassing configureren om Azure Active Directory te gebruiken

U bent bijna klaar.  Als laatste stap moet u de toepassing configureren voor het gebruik van Azure Active Directory als een SAML-identiteitsprovider. 

1. Blader naar het einde van de blade **Eenmalige aanmelding** voor uw toepassing. 

    ![Toepassing configureren](media/configure-single-sign-on-portal/configure-app.png)

2. Klik op **Toepassing configureren** in de portal en volg de instructies.
3. Maak handmatig gebruikersaccounts in de toepassing voor het testen van eenmalige aanmelding. Maken de gebruikersaccounts die u hebt toegewezen aan de toepassing in de [vorige sectie](#assign-users-to-the-application).   Wanneer u klaar bent voor implementatie van de toepassing in de organisatie, wordt u aangeraden automatisch gebruikers in te richten voor het automatisch maken van gebruikersaccounts in de toepassing.

## <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

U bent klaar om uw instellingen te testen.  

1. Open de instellingen voor eenmalige aanmelding voor uw toepassing. 
2. Scroll naar de sectie **Domein en URL's configureren**.
2. Klik op **SAML-instellingen testen**. De test-opties worden weergegeven.

    ![Opties voor eenmalige aanmelding testen](media/configure-single-sign-on-portal/test-single-sign-on.png) 

3. Klik op **Aanmelden als huidige gebruiker**. Hiermee kunt u eerst controleren of eenmalige aanmelding voor u, de beheerder, werkt.
4. Als er een fout optreedt, wordt een foutbericht weergegeven. Kopieer en plak de gegevens in het vak **Hoe ziet de fout eruit?**.

    ![Hulp krijgen bij het oplossen van problemen](media/configure-single-sign-on-portal/error-guidance.png)

5. Klik op **Hulp krijgen bij het oplossen van problemen**. Richtlijnen voor de oorzaak en de oplossing van het probleem wordt weergegeven.  In dit voorbeeld was de gebruiker niet toegewezen aan de toepassing.

    ![Fout herstellen](media/configure-single-sign-on-portal/fix-error.png)

6. Lees de oplossingsrichtlijnen en klik vervolgens, indien van toepassing, op **Het probleem oplossen**.

7. Voer de test opnieuw uit totdat de bewerking is voltooid.



## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikte u de Azure-portal om een toepassing te configureren voor eenmalige aanmelding met Azure Active Directory. U hebt de pagina Configuratie voor eenmalige aanmelding gevonden en de instellingen voor eenmalige aanmelding geconfigureerd. Nadat de configuratie is voltooid, hebt u een gebruiker toegewezen aan de toepassing, en de toepassing geconfigureerd om eenmalige aanmelding op basis van SAML te gebruiken. Hierna hebt u geverifieerd dat de SAML-aanmelding goed werkt.

U hebt het volgende gedaan:
> [!div class="checklist"]
> * SAML geselecteerd voor de modus voor eenmalige aanmelding
> * Verbinding gemaakt met de leverancier van de toepassing voor het configureren van domein en URL 's
> * Gebruikerskenmerken geconfigureerd
> * Een SAML-handtekeningcertificaat gemaakt
> * Handmatig gebruikers of groepen aan de toepassing toegewezen
> * De toepassing geconfigureerd voor eenmalige aanmelding
> * De SAML-gebaseerde modus voor eenmalige aanmelding getest

Als u de toepassing voor meer gebruikers in uw organisatie wilt implementeren, wordt u aangeraden automatische inrichting te gebruiken.

> [!div class="nextstepaction"]
>[Informatie over het toewijzen van gebruikers met automatische inrichting](configure-automatic-user-provisioning-portal.md)


