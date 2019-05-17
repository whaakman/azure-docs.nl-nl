---
title: Eenmalige aanmelding configureren - Azure Active Directory | Microsoft Docs
description: In deze zelfstudie wordt de Azure-portal gebruikt om eenmalige aanmelding voor een toepassing met Azure Active Directory (Azure AD) op basis van SAML te configureren.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa18bc637ec31a1f83b5cab090e008715c5e0c2a
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825019"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Zelfstudie: Op SAML gebaseerde eenmalige aanmelding voor een toepassing met Azure Active Directory configureren

In deze zelfstudie wordt de [Azure-portal](https://portal.azure.com) gebruikt om eenmalige aanmelding voor een toepassing met Azure Active Directory (Azure AD) op basis van SAML te configureren. Gebruik deze zelfstudie wanneer een [toepassingsspecifieke zelfstudie](../saas-apps/tutorial-list.md) niet beschikbaar is. 

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

1. Als de toepassing nog niet is toegevoegd aan uw Azure Active Directory-tenant, raadpleegt u [Snelstart: Een toepassing toevoegen aan uw Azure Active Directory-tenant](add-application-portal.md).

2. Vraag de leverancier van uw toepassing voor de gegevens die zijn beschreven [basic SAML-opties configureren](#configure-basic-saml-options).

3. Gebruik een niet-productieomgeving voor het testen van de stappen in deze zelfstudie. Als u niet beschikt over een niet-productieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

4. Meld u bij [Azure Portal](https://portal.azure.com) aan als beheerder van de cloudtoepassing of als toepassingsbeheerder voor uw Azure Active Directory-tenant.

## <a name="select-a-single-sign-on-mode"></a>Een modus voor eenmalige aanmelding selecteren

Nadat u uw Azure AD-tenant van een toepassing hebt toegevoegd, u kunt het configureren van eenmalige aanmelding voor de toepassing.

Hiermee opent u de instellingen voor eenmalige aanmelding:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**. 

2. Onder **beheren** in de **Azure Active Directory** navigatie-deelvenster dat verschijnt, selecteer **bedrijfstoepassingen**. Een steekproef van de toepassingen in uw Azure AD-tenant wordt weergegeven. 

3. In de **toepassingstype** in het menu **alle toepassingen**, en selecteer vervolgens **toepassen**.

4. Voer de naam in van de toepassing waarvoor u eenmalige aanmelding wilt configureren. Bijvoorbeeld, kunt u **GitHub-test** het configureren van de toepassing die u hebt toegevoegd in de [toepassing toevoegen](add-application-portal.md) Quick Start.  

     ![Schermafbeelding van de zoekbalk van de toepassing.](media/configure-single-sign-on-portal/azure-portal-application-search.png)

5. Kies de toepassing waarvoor u wilt configureren van eenmalige aanmelding.

6. Onder de **beheren** sectie, selecteer **eenmalige aanmelding**. 

7. Selecteer **SAML** het configureren van eenmalige aanmelding. De **instellen van eenmalige aanmelding met SAML - Preview** pagina wordt weergegeven.

## <a name="configure-basic-saml-options"></a>Basic SAML-opties configureren

Om het domein en de URL's te configureren:

1. Neem contact op met de leverancier van de toepassing voor de juiste gegevens voor de volgende instellingen:

    | Configuratie-instelling | SP-geïnitieerd | idP-geïnitieerd | Description |
    |:--|:--|:--|:--|
    | Id (Entiteits-id) | Vereist voor sommige apps | Vereist voor sommige apps | Unieke identificatie van de toepassing waarvoor eenmalige aanmelding wordt geconfigureerd. Azure Active Directory stuurt de id naar de toepassing als parameter van de doelgroep van het SAML-token. Er wordt verwacht dat de toepassing deze valideert. Deze waarde wordt ook weergegeven als de entiteit-ID in SAML-metagegevens die worden geleverd door de toepassing.|
    | Antwoord-URL | Optioneel | Vereist | Hiermee geeft u op waar de toepassing verwacht het SAML-token te ontvangen. De antwoord-URL wordt ook de ACS-URL (Assertion Consumer Service) genoemd. |
    | Aanmeldings-URL | Vereist | Niet opgeven | Wanneer een gebruiker deze URL opent, leidt de serviceprovider hem om naar Azure Active Directory om de gebruiker te verifiëren en aan te melden. Azure Active Directory maakt gebruik van de URL om de toepassing te starten vanuit Office 365 of het toegangsvenster van Azure Active Directory. Als dit veld leeg is, is Azure AD afhankelijk van de id-provider voor eenmalige aanmelding wordt gestart wanneer een gebruiker de toepassing wordt gestart.|
    | Relaystatus | Optioneel | Optioneel | Hiermee geeft u aan de toepassing op waar de gebruiker naar moet worden omgeleid nadat de verificatie is voltooid. De waarde is doorgaans een geldige URL voor de toepassing. Maar sommige toepassingen maken gebruik dit veld anders. Vraag de leverancier van de toepassing voor meer informatie.
    | Afmeldings-URL | Optioneel | Optioneel | Gebruikt voor het verzenden van de SAML-afmelding-reacties terug naar de toepassing.


2. Als de basisopties voor SAML-configuratie bewerken, selecteert u de **bewerken** pictogram (een potlood) in de rechterbovenhoek van de **SAML-basisconfiguratie** sectie.

     ![Certificaten configureren](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

3. Geef de informatie die is geleverd door de leverancier van de toepassing in stap 1 in de juiste velden op de pagina.

4. Aan de bovenkant van de pagina, selecteer **opslaan**.

## <a name="configure-user-attributes-and-claims"></a>Gebruikerskenmerken en claims configureren 

U kunt bepalen welke Azure AD-gegevens worden verzonden naar de toepassing in het SAML-token wanneer een gebruiker zich aanmeldt. U beheren deze informatie door het configureren van de kenmerken van gebruiker. U kunt bijvoorbeeld configureren dat Azure AD voor het verzenden van de naam, e-mailbericht en werknemer-ID van de gebruiker naar de toepassing wanneer een gebruiker zich aanmeldt. 

Deze kenmerken kunnen verplicht of optioneel zijn om eenmalige aanmelding correct te laten werken. Zie voor meer informatie de [Toepassingsspecifieke zelfstudie](../saas-apps/tutorial-list.md), of neem contact op met de leverancier van de toepassing.

1. Als u wilt bewerken gebruikerskenmerken en claims, selecteer de **bewerken** pictogram (een potlood) in de rechterbovenhoek van de **gebruikerskenmerken en Claims** sectie.

   De **naam id-waarde** is ingesteld met de standaardwaarde van *user.principalname*. De gebruikers-id is uniek voor elke gebruiker in de toepassing. Als het e-mailadres bijvoorbeeld zowel de gebruikersnaam als de unieke id is, wordt de waarde ingesteld op *user.mail*.

2. Om te wijzigen de **naam id-waarde**, selecteer de **bewerken** pictogram (een potlood) voor de **naam id-waarde** veld. De benodigde wijzigingen aanbrengen aan de id-indeling en de bron, indien nodig. Sla de wijzigingen wanneer u klaar bent. Zie voor meer informatie over het aanpassen van claims, de [uitgegeven claims in het SAML-token voor bedrijfstoepassingen aanpassen](../develop/active-directory-saml-claims-customization.md) artikel met instructies.

3. Als u wilt een claim toevoegen, selecteert u **toevoegen nieuwe claim** aan de bovenkant van de pagina. Voer de **naam** en selecteer de juiste bron. Als u selecteert de **kenmerk** bron, u moet kiezen het **bronkenmerk** u wilt gebruiken. Als u selecteert de **vertaling** bron, u moet kiezen het **transformatie** en **Parameter 1** u wilt gebruiken.

4. Selecteer **Opslaan**. De nieuwe claim wordt weergegeven in de tabel.
 
## <a name="generate-a-saml-signing-certificate"></a>Genereren van een SAML-handtekeningcertificaat

Azure Active Directory maakt gebruik van een certificaat voor het ondertekenen van de SAML-tokens die worden verzonden naar de toepassing. 

1. Als u wilt een nieuw certificaat genereren, selecteert u de **bewerken** pictogram (een potlood) in de rechterbovenhoek van de **SAML-handtekeningcertificaat** sectie.

2. In de **SAML-handtekeningcertificaat** sectie, selecteer **nieuw certificaat**.

3. In het nieuwe certificaatrij die wordt weergegeven, stelt u de **vervaldatum**. Zie voor meer informatie over de beschikbare configuratieopties, de [geavanceerde opties voor Certificaatondertekening](certificate-signing-options.md) artikel.

4. Selecteer **opslaan** aan de bovenkant van de **SAML-handtekeningcertificaat** sectie. 

## <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan een toepassing

Er is een goed idee om te testen de eenmalige aanmelding met verschillende gebruikers of groepen voordat het uitrollen van de toepassing in uw organisatie.

> [!NOTE]
>
> Deze stappen gaat u naar de **gebruikers en groepen** configuratiesectie in de portal. Wanneer u klaar bent, moet u terug te keren naar de **eenmalige aanmelding** sectie om de zelfstudie te voltooien.

Een gebruiker of groep toewijzen aan de toepassing:

1. Open de toepassing in de portal, als deze nog niet is geopend.
2. Selecteer in het linkernavigatievenster van de toepassing, **gebruikers en groepen**.
3. Selecteer **Gebruiker toevoegen**.
4. In de **toevoegen toewijzing** sectie, selecteer **gebruikers en groepen**.
5. Als u een specifieke gebruiker zoekt, typ de naam van de gebruiker in de **lid selecteren of een externe gebruiker uitnodigen** vak. Selecteer vervolgens de profielfoto van de gebruiker of het logo, en kies vervolgens **Selecteer**. 
6. In de **toevoegen toewijzing** sectie, selecteer **toewijzen**. Wanneer u klaar bent, de geselecteerde gebruikers worden weergegeven in de **gebruikers en groepen** lijst.

## <a name="set-up-the-application-to-use-azure-ad"></a>Het gebruik van Azure AD-toepassing instellen

U bent bijna klaar.  Als laatste stap moet u de toepassing Azure AD gebruiken als een SAML-identiteitsprovider instellen. 

1. Schuif omlaag naar de **instellen <applicationName>**  sectie. Voor deze zelfstudie in deze sectie wordt aangeroepen **instellen van GitHub-test**. 
2. Kopieer de waarde van elke rij in deze sectie. Plak elke waarde in de desbetreffende rij in de **SAML-basisconfiguratie** sectie. Kopieer bijvoorbeeld de **aanmeldings-URL** waarde uit de **instellen van GitHub-test** sectie en plak deze in de **aanmelding URL** veld in de **SAML-basisconfiguratie**  sectie, enzovoort.
3. Wanneer u alle waarden in de juiste velden hebt geplakt, selecteert u **opslaan**.

## <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

U kunt uw instellingen testen.  

1. Open de instellingen voor eenmalige aanmelding voor uw toepassing. 
2. Schuif naar de **valideren eenmalige aanmelding met <applicationName>**  sectie. Voor deze zelfstudie in deze sectie wordt aangeroepen **instellen van GitHub-test**.
3. Selecteer **Test**. De test-opties worden weergegeven.
4. Selecteer **Meld u als de huidige gebruiker**. Met deze test kunt u eerst controleren of eenmalige aanmelding voor u, de beheerder, werkt.

Als er een fout optreedt, wordt een foutbericht weergegeven. Voltooi de volgende stappen:

1. Kopieer en plak de gegevens in het vak **Hoe ziet de fout eruit?**.

    ![Hulp bij het oplossen van problemen krijgen](media/configure-single-sign-on-portal/error-guidance.png)

2. Selecteer **resolutie begeleiding**. De richtlijnen voor de oorzaak en de oplossing van het probleem worden weergegeven.  In dit voorbeeld was de gebruiker niet toegewezen aan de toepassing.

3. Lees de richtlijnen resolutie en vervolgens, indien mogelijk, los het probleem.

4. Voer de test opnieuw uit totdat de bewerking is voltooid.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de instellingen voor eenmalige aanmelding voor een toepassing geconfigureerd. Nadat de configuratie is voltooid, hebt u een gebruiker toegewezen aan de toepassing, en de toepassing geconfigureerd om eenmalige aanmelding op basis van SAML te gebruiken. Hierna hebt u geverifieerd dat de SAML-aanmelding goed werkt.

U hebt het volgende gedaan:
> [!div class="checklist"]
> * SAML geselecteerd voor de modus voor eenmalige aanmelding
> * Verbinding gemaakt met de leverancier van de toepassing voor het configureren van domein en URL 's
> * Gebruikerskenmerken geconfigureerd
> * Een SAML-handtekeningcertificaat gemaakt
> * Handmatig gebruikers of groepen aan de toepassing toegewezen
> * De toepassing is geconfigureerd voor het gebruik van Azure Active Directory als een SAML-identiteitsprovider
> * De SAML-gebaseerde modus voor eenmalige aanmelding getest

Voor de implementatie van de toepassing meer gebruikers in uw organisatie, gebruikt u automatisch inrichten van gebruikers.

> [!div class="nextstepaction"]
> [Informatie over het toewijzen van gebruikers met automatische inrichting](configure-automatic-user-provisioning-portal.md)


