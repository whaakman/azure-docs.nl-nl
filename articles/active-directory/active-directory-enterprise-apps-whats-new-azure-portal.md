---
title: Wat is er nieuw in Enterprise Application management in Azure Active Directory | Microsoft Docs
description: Meer informatie over wat is er nieuw in Enterprise Application management in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
editor: ''
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: fefc508679a309262d07a582fc8f5bdf9f67cfe5
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310111"
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>Wat is er nieuw in Enterprise Application management in Azure Active Directory 

Azure Active Directory (Azure AD) is uitgebreid enterprise programma's voor toepassingsbeheer, met nieuwe functies en mogelijkheden voor het beheren van apps eenvoudiger en efficiënter maken.

Hieronder volgen enkele van de verbeteringen voor Azure AD in de [Azure-portal](https://portal.azure.com).

- Een toepassingsgalerie betere ondervindt, met een vereenvoudigde toepassingsmodel maken en ondersteuning voor alle soorten toepassingen die u hebt gebruikt voor het. 
- Een gloednieuwe Quick Start-ervaring waarmee u kunt aan de slag met een prototype van uw toepassing. 
- Selfservice-beleid configureren met een paar klikken. 
- Verbeteringen aan application proxy, eenmalige aanmelding configureren en de ervaringen van uw eigen toepassing, zodat u kunt meer doen dan voordat u brengen.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Verbeteringen in de Azure Active Directory-Toepassingsgalerie

Uw favoriete toepassingen toevoegen of ze afkomstig van zijn de [toepassingsgalerie](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery), aangepaste toepassingen die u wilt uitbreiden naar de cloud of nieuwe toepassingen die u ontwikkelt.  U kunt aan de slag met deze nieuwe ervaring door te klikken op **toevoegen** onder **bedrijfstoepassingen** of **alle toepassingen**.
 
  ![Een toepassing toevoegen](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Eenmaal in de galerie ziet u de aanbevolen toepassingen die ondersteuning bieden voor gebruikers inrichten front-en-midden weergegeven. U kunt allerlei verschillende categorieën om te zoomen op de toepassingen die u het belangrijkst bladeren of kunt u de zoekervaring snel vinden de toepassingen die u wilt integreren.

  ![De galerie](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Toevoegen van aangepaste toepassingen vanaf één plek

Naast het toevoegen van vooraf geïntegreerde toepassingen uit de galerie, zijn alle optreedt in de configuratie van de aangepaste toepassing die u voor zijn gebruikt in de klassieke-beheerportal nu mogelijk in de nieuwe portal. Of u een toepassing van on-premises via de toepassingsproxy, integreren van uw eigen wachtwoord of de federatieve SSO toepassing uitbreiden of het maken van een gloednieuwe toepassing met behulp van het toepassingsregister, u toegang hebt tot deze vanaf deze één enkele locatie.

  ![Uw eigen toepassing toe te voegen](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Om uw eigen toepassing toe te voegen aan de slag**:

1. Klik op **uw eigen koppeling toevoegen** aan de bovenkant van de toepassingsgalerie. 
1. Ziet u twee opties voor u: **implementeert u een bestaande toepassing** of **ontwikkelen van een nieuwe toepassing**. Lees verder voor informatie over het verschil tussen de twee opties en het gebruik ervan.

### <a name="deploying-existing-applications"></a>Bestaande toepassingen implementeren

1. Als u hebt een toepassing die al wordt uitgevoerd en willen integreren in Azure AD voor eenmalige aanmelding op of wanneer het inrichten, kies de **implementeert u een bestaande toepassing** optie. Kies een naam voor uw toepassing, klikt u op **toevoegen**.
1. Dat is alles. In plaats van dat u hoeft de details over uw toepassing vooraf te kennen, kunt u nu instellen van de werking van uw nieuwe toepassing door te gaan via het menu links en de toepassing naar wens configureren op elk gewenst moment.

  ![Toevoegen van een bestaande toepassing met één klik](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Nieuwe toepassingen ontwikkelen

1. Als u een nieuwe toepassing ontwikkelt, is er een eenvoudige manier voor u meteen aan het register van de toepassing uit de galerie:
1. Klik op de **toevoegen aan uw eigen** optie uit de galerie, selecteer **ontwikkelen van een bestaande toepassing**, en ziet u een snelkoppeling naar rechts in de manier waarop toepassingen toevoegen.

  ![Een nieuw ontwikkelde toepassing toe te voegen in een paar klikken](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Nadat u een toepassing met behulp van het register van de toepassing hebt toegevoegd, ziet u deze in de lijst van zakelijke toepassingen, waar u eenmalige aanmelding configureren en beheren van toegangsbeleid voor uw nieuwe toepassing moet worden weergegeven.

  ![Beheer van toegang tot uw nieuwe toepassing met zakelijke toepassingen](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quickstart-get-going-with-your-new-application-right-away"></a>Snelstartgids: Aan de slag met uw nieuwe toepassing meteen 

Nadat u een toepassing, of zijn de vooraf geïntegreerde of uw eigen app hebt toegevoegd, hebben we een op maat gemaakte ervaring snel starten-u kunt snel gegrond in de nieuwe ervaring voor toepassingen gemaakt. Als u elke optie systematisch volgt, we helpen u bij de gebruikersinterface en ziet u hoe u aan de slag met een prototype van uw nieuwe toepassing zo snel mogelijk. 
 
  ![De nieuwe toepassingen Quick Start-ervaring](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 U kunt deze nieuwe Quick Start-ervaring op elk gewenst moment en voor elke toepassing, door te klikken op **snelstartgids** in het navigatiemenu links toepassing.


## <a name="updated-application-proxy-configuration"></a>Bijgewerkte toepassing proxyconfiguratie

Nu gaan we zeggen een van de nieuwe toepassingen die u hebt toegevoegd in uw on-premises omgeving wordt uitgevoerd en u wilt integreren met Azure AD.  Een van de nieuwe dingen over de nieuwe ervaring voor de configuratie van toepassing in de nieuwe Azure AD-portal is dat door het splitsen van de toepassing aanmeldings-modus van de application proxy-configuratie, u nu eenvoudig SSO-wachtwoord of federatieve toepassingen weergeven kunt die worden uitgevoerd in uw bedrijfsnetwerk direct naar de cloud, zonder te hoeven maken van meerdere exemplaren van de toepassing.

U kunt ook een van de nieuwe toepassingen die u hebt toegevoegd voor gebruik met Azure AD-toepassingsproxy rechts van de nieuwe portal, met inbegrip van toepassingen die ondersteuning bieden voor systeemeigen Windows-authenticatie-ervaringen configureren.

  ![Configureren van een toepassing de optie van de aanmeldings-geïntegreerde Windows-verificatie te gebruiken](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 
Om te beginnen een systeemeigen toepassing van de Windows-verificatie configureren met de Application Proxy:
1. Klik op de eenmalige aanmelding, navigatie-item en kiest u **geïntegreerde Windows-verificatie** onder instellingen voor eenmalige aanmelding en configureer de instellingen naar wens.
1. Op deze nieuwe verificatiemodi ondersteunen, kunt u certificaten van aangepaste domeinen voor de ondersteuning van toepassingen die worden uitgevoerd op beveiligde eindpunten binnen uw organisatie nu ook uploaden.  
 
   ![Uploaden van een certificaat moet worden gebruikt bij de toepassingsproxy](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

1. Als u wilt een nieuw certificaat voor uw favoriete on-premises toepassing uploaden, klikt u op de **toepassingsproxy** optie in het navigatiemenu links, klikt u op de **certificaat** selector en upload een certificaat we gebruiken kunnen voor het versleutelen van aanvragen van onze cloudeindpunt voor uw toepassing het bestand.

## <a name="advanced-federated-single-sign-on-configuration"></a>Geavanceerde federatieve eenmalige aanmelding configuratie

Voor het geval u federatieve toepassingen gebruiken, moet u er veel nieuwe functies zijn in de configuratie op basis van SAML-aanmelding. Beginnen met, kunt nu u volledig aanpassen, toevoegen, verwijderen en toewijzen van de bestaande gebruikerskenmerken als claims in het SAML-tokens worden uitgegeven.
 
  ![Aanpassen van de kenmerken van de SAML-token gebruiker doorgegeven aan een federatieve toepassing](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)

Om te controleren dat van de nieuwe federatieve SSO-configuratie:
1. Open een federatieve toepassing **eenmalige aanmelding** in het navigatiemenu links en zorg ervoor dat de '*SAML gebaseerde aanmelding** modus is ingeschakeld. 
1. Zodra er, schakelt u het selectievakje onder de **gebruikerskenmerken** kop wijzigen alle kenmerken die zijn opgenomen in het SAML-token is doorgegeven aan die toepassing.

U kunt ook maken, rollover, en beheren van certificaten die worden gebruikt voor federatieve eenmalige aanmelding, evenals bewerken die wordt een melding wanneer uw certificaat is verlopen. U ziet deze nieuwe opties onder de **certificaten** kop van de dezelfde-deelvenster eenmalige aanmelding.
 
  ![Het maken van een nieuw certificaat, e-mailmelding verlopen en opties voor Certificaatondertekening aanpassen](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-parameter"></a>Status van de relay-parameter
Tot slot hebben we ook de set URL voor SAML-parameters, we ondersteunen om op te nemen uitgebreid de **Relaystatus parameter**, dit is de pagina die uw gebruikers komt terecht op binnen een federatieve toepassing als de aanmelding is voltooid. Dit is een nuttig instelling configureren als u wilt uw gebruikers verzenden met een specifieke locatie binnen de toepassing snel gaan ophalen.

  ![Als u de parameter SAML Relaystatus](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Om in te stellen van de relay-parameter state**:

1. Schakel de **geavanceerde URL-instellingen weergeven** selectievakje onder de **domein en URL's** kop van de eenmalige aanmelding in het deelvenster configuratie. 
1. Een set nieuwe URL als invoer worden vakken weergegeven waarmee u kunt deze parameter en een andere URL voor SAML-instellingen.

## <a name="bring-your-own-password-sso-applications"></a>Breng uw eigen wachtwoord SSO-toepassingen

We weten dat niet elke toepassing federation kant ondersteunt. Bijvoorbeeld, misschien heeft een van de nieuwe toepassingen die u hebt toegevoegd een aangepaste aanmeldingsscherm die uw gebruikers gebruiken een gebruikersnaam en wachtwoord aanmelden bij. U kunt nog steeds dergelijke toepassingen integreren met Azure AD via onze **Breng uw eigen toepassingen** functie, die nu beschikbaar voor u configureren in de nieuwe portal.
 
  ![Aangepaste wachtwoord vaulting toepassingen met Azure AD integreren](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**Om te controleren of de functie 'Breng uw eigen toepassingen' uit**:

1. Na het instellen van de modus voor eenmalige aanmelding voor een nieuwe aangepaste toepassing die u hebt toegevoegd aan **op basis van wachtwoorden Sign-on**, voert u de URL waar de toepassing het aanmeldingsscherm wordt weergegeven en 
1. Klik op **Opslaan**.  
1. Als u dat doet, wordt u automatisch kan verzamelen die URL voor een gebruikersnaam en wachtwoord invoervak en kunt u Azure AD gebruiken voor het veilig verzenden van wachtwoorden bij die toepassing met behulp van het deelvenster browser-extensie voor toegang.

## <a name="configure-self-service-application-access"></a>Toegang tot Self-servicetoepassingen configureren

Nadat u tal van nieuwe toepassingen hebt toegevoegd, misschien wilt u dat uw gebruikers om te bladeren en deze toepassingen toevoegen aan hun eigen toegangsvensters, zonder dat u als een beheerder zich niet druk te. U kunt met deze nieuwste release, nu, configureren en beheren van toegang tot Self-servicetoepassingen rechtstreeks vanuit de nieuwe portal.

  ![Configureren van toegang tot Self-servicetoepassingen om een wachtwoord SSO-toepassing](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Configureren en beheren van toegang tot Self-servicetoepassingen**:

1. Om te beginnen, kunt u de **selfservice** optie van de toepassing het navigatiemenu links en stel de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** op de optie '**Ja**'. 
1. Hierdoor kunt u het configureren van wie toegang tot deze toepassing goedkeuren is toegestaan en welke groep Self-servicegebruikers kunnen worden toegevoegd. Bovendien, als de toepassing is geconfigureerd voor wachtwoord eenmalige aanmelding, ziet ook u een andere optie waarmee u deze fiatteurs voor het beheren van de wachtwoorden die zijn toegewezen aan de toepassing (optioneel) toestaan.

## <a name="feedback"></a>Feedback

We hopen dat u, zoals het gebruik van de verbeterde ervaring voor Azure AD. Houd uw feedback! Plaats uw feedback en ideeën voor verbetering van de **-beheerportal** sectie van onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  We enthousiast bent over het bouwen van coole nieuwe dingen elke dag en gebruik de informatie voor de shape en definiëren wat we hierna bouwen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [toepassingen beheren met Azure Active Directory](manage-apps/what-is-application-management.md).



