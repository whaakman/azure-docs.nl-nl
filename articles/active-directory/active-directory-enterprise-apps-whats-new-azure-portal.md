---
title: Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory | Microsoft Docs
description: Ontdek wat er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory.
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 949eae51c469205489153c271a7c20fcc843ad46
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory 

Azure Active Directory (Azure AD) is uitgebreid enterprise hulpprogramma's voor toepassingsbeheer, met nieuwe functies en mogelijkheden voor het maken van het beheer van apps eenvoudiger en efficiënter.

Hieronder volgen enkele van de uitbreidingen voor Azure AD in de [Azure-portal](https://portal.azure.com).

- Een verbeterde toepassingsgalerie ervaring, met een vereenvoudigde toepassingsmodel maken en ondersteuning voor alle soorten toepassingen waarmee u bent. 
- Een ervaring gloednieuw snel starten waarmee u kunt aan de slag met een test van uw toepassing. 
- Selfservice-beleid configureren met een paar klikken. 
- Verbeteringen aan toepassingsproxy, eenmalige aanmelding configureren en breng uw eigen toepassingen mogelijk, zodat u meer gedaan dan voordat ophalen.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Verbeteringen in de Azure Active Directory-Toepassingsgalerie

Toevoegen van uw favoriete toepassingen ongeacht of deze uit de [toepassingsgalerie](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), aangepaste toepassingen die u wilt uitbreiden naar de cloud of nieuwe toepassingen die u ontwikkelt.  U kunt aan de slag met deze nieuwe ervaring door te klikken op **toevoegen** op de **bedrijfstoepassingen** overzicht of **alle toepassingen** blades.
 
  ![Een toepassing toevoegen](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Eenmaal in de galerie ziet u onze uitgelichte toepassingen die ondersteuning bieden voor gebruikers inrichten begin-en-midden weergegeven.  Kunt u allerlei verschillende categorieën om de toepassingen die u interesseren inzoomen bladeren of kunt u de zoekfunctie snel vinden de toepassingen die u wilt integreren.

  ![De galerie met toepassingen](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Toevoegen van aangepaste toepassingen vanaf één locatie

Naast het toevoegen van vooraf geïntegreerde toepassingen uit de galerie, zijn configuratie van de aangepaste toepassing ondervindt dat u werden gebruikt in de klassieke beheerportal nu mogelijk in de nieuwe portal. Of u een toepassing met on-premises met de toepassingsproxy integreren van uw eigen wachtwoord of de federatieve SSO-toepassing, uitbreiden of een gloednieuw toepassing met het toepassingsregister maakt, hebt u deze via deze één enkele locatie.

  ![Uw eigen toepassing toe te voegen](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Uw eigen toepassing toe te voegen aan de slag**:

1. Klik op de **uw eigen koppeling toevoegen** boven aan de galerie met toepassingen. 
2. Ziet u twee opties voor u: **implementeren van een bestaande toepassing** of **ontwikkelen van een nieuwe toepassing**. Lees verder voor meer informatie over het verschil tussen de twee opties en hoe ze te gebruiken.

### <a name="deploying-existing-applications"></a>Bestaande toepassingen implementeren

1. Als u hebt een toepassing die al wordt uitgevoerd en alleen wilt integreren met Azure AD voor eenmalige aanmelding op of inrichting, kies de **implementeren van een bestaande toepassing** optie. Kies een naam voor uw toepassing, klikt u op **toevoegen**.
2. Dat is alles. In plaats van dat de details over uw toepassing vooraf te kennen, kunt u nu instellen hoe uw nieuwe toepassing werkt door te navigeren in het menu links en de toepassing naar wens configureren op elk gewenst moment.

  ![Toevoegen van een bestaande toepassing met één klik](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Ontwikkelen van nieuwe toepassingen

1. Als u een nieuwe toepassing ontwikkelt, is er een eenvoudige manier voor u direct aan het register van de toepassing uit de galerie:
2. Klik op de **toevoegen aan uw eigen** optie uit de galerie met toepassingen, selecteer de **ontwikkelen van een bestaande toepassing** keuze en u ziet een snelle koppeling aan de toepassing toevoegen-ervaring.

  ![Een toepassing recentelijk ontwikkelde toe te voegen in een paar klikken](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Als u een toepassing met het register van de toepassing toevoegt, wordt deze weergegeven in de lijst met bedrijfstoepassingen waar u zult kunnen eenmalige aanmelding configureren en beheren van beleidsregels voor toegang voor uw nieuwe toepassing weergegeven.

  ![Het beheren van toegang tot uw nieuwe toepassing onder bedrijfstoepassingen](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a>Snel aan de slag: aan de slag met uw nieuwe toepassing meteen 

Nadat u een toepassing, of zijn de vooraf geïntegreerde of uw eigen app hebt toegevoegd, er een op maat gemaakte Snel starten-ervaring om u snel fundering in de nieuwe ervaring van toepassingen gemaakt. Als u elke optie systematischer volgt, we helpt u stapsgewijs door de gebruikersinterface en hoe u aan de slag met een test van uw nieuwe toepassing zo snel mogelijk. 
 
  ![De nieuwe toepassingen die snelle start ervaring](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 U kunt deze nieuwe ervaring voor snel starten op elk gewenst moment en voor elke toepassing, door te klikken op **snel starten** in het menu van de linkernavigatiebalk toepassing.


## <a name="updated-application-proxy-configuration"></a>Bijgewerkte toepassing proxyconfiguratie
Nu gaan we als een van de nieuwe toepassingen die u hebt toegevoegd in uw on-premises omgeving wordt uitgevoerd en u wilt integreren met Azure AD.  Een van de cool nieuwe dingen over de nieuwe ervaring voor de configuratie van toepassing in de nieuwe Azure AD portal is dat door het verdelen van de toepassing aanmelding-modus van de configuratie van de toepassing proxy kan nu eenvoudig worden blootgesteld wachtwoord SSO of federatieve toepassingen die worden uitgevoerd in uw bedrijfsnetwerk rechts in de cloud, zonder te hoeven maken van meerdere exemplaren van de toepassing.

Naast dit, kunt u nu ook configureren van de nieuwe toepassingen die u hebt toegevoegd voor gebruik met Azure AD-toepassingsproxy rechts van de nieuwe portal, met inbegrip van toepassingen die ondersteuning bieden voor systeemeigen Windows-verificatie-ervaring.

  ![Configureren van een toepassing de optie van de aanmelding geïntegreerde Windows-verificatie te gebruiken](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

Om te beginnen met een systeemeigen Windows-verificatie toepassing configureren met de toepassingsproxy:
1. Klik op het één aanmelding navigatie-item en kies **geïntegreerde Windows-verificatie** op de instellingenblade voor eenmalige aanmelding en de instellingen naar wens configureren.
2. Boven op deze nieuwe verificatiemodi ondersteunen, kunt u certificaten van aangepaste domeinen om toepassingen die worden uitgevoerd op de beveiligde eindpunten binnen uw organisatie te ondersteunen nu ook uploaden.  
 
   ![Uploaden van een certificaat moet worden gebruikt bij de toepassingsproxy](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. Als u wilt uploaden een nieuw certificaat voor uw favoriete on-premises toepassing, klikt u op de **toepassingsproxy** optie in het linkernavigatievenster menu, klikt u op de **certificaat** selector en upload een certificaat we gebruiken kunnen voor het versleutelen van uw toepassing aanvragen van onze cloudeindpunt-bestand.

## <a name="advanced-federated-single-sign-on-configuration"></a>Geavanceerde federatieve eenmalige aanmelding-configuratie

Voor de federatieve toepassingen vandaag gebruikt, moet u er veel nieuwe functies zijn in de aanmelding configuratie op basis van SAML-blade. Beginnen met, kunt nu u volledig aanpassen, toevoegen, verwijderen en toewijzen van de bestaande gebruikerskenmerken als claims in SAML-tokens uitgegeven.
 
  ![Aanpassen van de kenmerken van de SAML-token gebruiker doorgegeven aan een federatieve toepassing](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


Om te controleren dat uit de nieuwe federatieve SSO-configuratie:
1. Open een federatieve toepassing **eenmalige aanmelding** blade in het linkernavigatievenster menu en zorg ervoor dat de '*op basis van SAML aanmelding** modus is ingeschakeld. 
2. Eenmaal er, schakel het selectievakje onder de **gebruikerskenmerken** kop wijzigen alle kenmerken die zijn opgenomen in het SAML-token wordt doorgegeven aan die toepassing.

U kunt ook maakt, rollover, en het beheren van certificaten gebruikt voor federatieve eenmalige aanmelding, evenals bewerken die met deze eigenschap wordt een melding wanneer uw certificaat is verlopen. Ziet u deze nieuwe opties onder de **certificaten** kop op de dezelfde eenmalige aanmelding blade.
 
  ![Maken van een nieuw certificaat verlopen meldingse-mail en opties voor Certificaatondertekening aanpassen](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a>Status van de relay-parameter worden
Ten slotte wordt ook de set parameters van SAML-URL wordt ondersteund als u wilt opnemen hebt uitgebreid de **parameter Relay State**, dit is de pagina die uw gebruikers terechtkomt op binnen een federatieve toepassing zodra de aanmeldingspagina is voltooid. Dit is zeer nuttig instelling configureren als u wilt uw gebruikers verzenden met een specifieke locatie binnen de toepassing snel gaan ophalen.

  ![Als u de status van SAML-Relay-parameter](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Instellen van de relay-parameter state**:

1. Inschakelen de **weergeven geavanceerde instellingen voor URL** selectievakje onder de **domein en de URL's** kop op eenmalige aanmelding op de blade configuratie. 
2. Wanneer u dit doet, ziet u dat een set nieuwe URL invoer vakken weergegeven waarmee u kunt deze en andere, instellingen voor SAML-URL ingesteld.

## <a name="bring-your-own-password-sso-applications"></a>Breng uw eigen wachtwoord SSO-toepassingen

We weten dat niet elke toepassing federation klare ondersteunt. Zo mogelijk heeft een van de nieuwe toepassingen die u hebt toegevoegd een aangepast aanmeldingsscherm die uw gebruikers gebruiken een gebruikersnaam en wachtwoord aanmelden bij. U kunt nog steeds deze soorten toepassingen integreren met Azure AD via onze **brengt uw eigen toepassingen** functie is nu beschikbaar voor u in de nieuwe portal te configureren.
 
  ![Integratie van aangepaste wachtwoord vaulting toepassingen met Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**De functie 'Brengt uw eigen toepassingen' uitchecken**:

1. Na het instellen van de modus voor één aanmelding voor een nieuwe aangepaste toepassing die u hebt toegevoegd aan **op basis van wachtwoorden aanmelding**, voert u de URL waar de toepassing de aanmeldingsscherm renders en klikt u op **opslaan**.  
2. Zodra u dat doet, wordt je automatisch scrape die URL voor een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden voor deze toepassing met behulp van de uitbreiding voor toegang tot Configuratiescherm browser.

## <a name="configure-self-service-application-access"></a>Toegang tot selfservice-toepassingen configureren

Nadat u veel nieuwe toepassingen hebt toegevoegd, mogelijk wilt u uw gebruikers om te bladeren en deze toepassingen aan hun eigen deelvensters toegang toevoegen zonder zin hebt u als beheerder in staat. U kunt in deze nieuwe versie nu configureren en beheren van toegang tot selfservice toepassingen rechtstreeks vanuit de nieuwe portal.

  ![Toegang tot de toepassing zelf om een wachtwoord SSO-toepassing configureren](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Configureren en beheren van toegang tot de toepassing zelf**:

1. Om te beginnen, kunt u de **Self-service** optie van de toepassing navigatiemenu links en instellen van de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** optie naar '**Ja**'. 
2. Hierdoor kunt u configureren die is toegestaan voor het goedkeuren van toegang tot deze toepassing, en welke groep Self-Service gebruikers worden toegevoegd. Bovendien, als de toepassing is geconfigureerd voor wachtwoord eenmalige aanmelding, ziet ook u een andere optie waarmee u toestaan die goedkeurders voor het beheren van de wachtwoorden die zijn toegewezen aan de toepassing.

##<a name="feedback"></a>Feedback

We hopen dat u u zoals het gebruik van het verbeterde ervaring voor Azure AD. Zorg ervoor dat uw feedback! Plaats uw feedback en ideeën voor verbetering van de **beheerportal** sectie van onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  We enthousiast bent over het bouwen van cool nieuw per dag uit, en gebruik de richtlijnen voor vorm en definiëren wat we hierna bouwen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie [toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md).



