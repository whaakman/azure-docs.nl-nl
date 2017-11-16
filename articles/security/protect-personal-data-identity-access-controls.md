---
title: Persoonlijke gegevens beschermen met Azure identiteits- en toegangsbeheer besturingselementen | Microsoft Docs
description: Met behulp van Azure identiteits- en toegangsbeheer bepaalt voor hulp bij het beveiligen van uw persoonlijke gegevens
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: e6de9526a1a72cfc81caca51207e000f8b3673cc
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Azure Active Directory en de multi-factor Authentication: persoonlijke gegevens beschermen met besturingselementen voor identiteits- en toegangsbeheer

Dit artikel bevat informatie en procedures die u gebruiken kunt voor het beveiligen van persoonlijke gegevens met Azure Active Directory en de multi-factor authentication-beveiligingsfuncties en -services.

## <a name="scenario"></a>Scenario

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse, Adriatische, Baltische veiligheid ook Florida. Ter ondersteuning van deze inspanningen is die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk 

Het bedrijf gebruikmaakt van Microsoft Azure voor het opslaan van bedrijfsgegevens in de cloud. Dit omvat persoonlijk gegevens zoals namen, adressen, telefoonnummers en creditcardgegevens van globale klantendatabase. Dit omvat ook traditionele Human Resources informatie zoals adressen, telefoonnummers, BTW-id's en andere informatie over de werknemers van het bedrijf op alle locaties. De regel cruise onderhoudt ook een grote database van derden en loyaliteit programma leden met persoonlijke gegevens voor de relaties met de huidige en eerdere klanten bijhouden.

Zakelijke werknemers toegang tot het netwerk van het bedrijf externe kantoren en reizen agents over de hele wereld hebben toegang tot een aantal bedrijfsresources.

## <a name="problem-statement"></a>Probleemformulering

Het bedrijf moet de privacy van klanten en werknemers persoonlijke gegevens beveiligen tegen kwaadwillende personen willen waarmee is geknoeid id's gebruiken om toegang te krijgen. Ze ook moeten ervoor zorgen dat de toegang tot persoonlijke gegevens door legitieme gebruikers is beperkt tot alleen degenen die u nodig hebt om hun werk te doen.

## <a name="company-goal"></a>Bedrijf-doel

Doel van het bedrijf is om ervoor te zorgen dat toegang tot persoonlijke gegevens strikt wordt beheerd. Het is essentieel dat de identiteit van gebruikers met toegang tot persoonlijke gegevens door sterke verificatie worden beveiligd. Een beleid van [minimale bevoegdheden] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) moeten worden afgedwongen, zodat legitieme gebruikers hoeven alleen het niveau van toegang hebben en niet meer.

## <a name="solutions"></a>Oplossingen

Microsoft Azure biedt identiteits- en toegangsbeheer beheerhulpprogramma's, zodat bedrijven bepalen wie toegang heeft tot bronnen die persoonlijke gegevens bevatten.

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) beheert identiteiten en controleert de toegang tot Azure evenals andere on-premises en andere cloudbronnen, gegevens en toepassingen. [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) bedoeld om beheerders van Azure om het aantal mensen die toegang tot bepaalde gegevens zoals persoonlijke gegevens hebben te beperken. Dit kan ze detecteren, te beperken en te controleren van bevoegde identiteiten en de toegang tot bronnen en toe te wijzen tijdelijke, beheerdersrechten hebben voor gebruikers in aanmerking komende JIT (Just in time). Het bevat ook inzicht in de AAD-beheerdersbevoegdheden hebben.

De activiteiten die betrokken zijn bij het gebruik van AAD PIM omvatten:

- Privileged Identity Management voor uw directory inschakelen

- Met behulp van Privileged Identity Management admin dashboard belangrijke informatie in één oogopslag zien

- Het beheren van de bevoegde identiteiten (beheerders) toe te voegen of te verwijderen of de in aanmerking komende beheerders aan elke rol

- De instellingen van de activering configureren

- Rollen activeren

- Rol activiteit controleren

#### <a name="how-do-i-enable-aad-pim"></a>Hoe kan ik AAD PIM inschakelen?

Als u wilt gebruiken PIM voor uw directory, het volgende doen:

1. Meld u aan bij de Azure portal als globale beheerder van uw directory.

2. Als uw organisatie meerdere directory's heeft, selecteert u uw gebruikersnaam rechtsboven in de Azure Portal. Selecteer de map waar u Azure AD Privileged Identity Management gebruikt.

3. Selecteer **meer services** en gebruik de **Filter** textbox om te zoeken naar Azure AD Privileged Identity Management.

4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

Nadat Azure AD Privileged Identity Management is ingesteld, ziet u de navigatieblade telkens wanneer u de toepassing opent.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

Zie voor meer informatie en instructies voor het aan de slag met AAD PIM [Start met behulp van Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)

### <a name="azure-role-based-access-control"></a>Op rollen gebaseerde toegangsbeheer van Azure

[Azure op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) helpt Azure beheerders toegang tot Azure-resources beheren doordat het verlenen van toegang op basis van de gebruiker toegewezen rol. U kunt taken in een team scheiden en de hoeveelheid toegang verlenen aan gebruikers, groepen en toepassingen die ze nodig hebben voor het uitvoeren van hun taken.

Op rollen gebaseerde toegang kan worden verleend aan gebruikers via de Azure Portal, Azure-opdrachtregelprogramma's of Azure Management-API's.

Zie voor meer informatie over de basisprincipes van Azure RBAC [aan de slag met toegangsbeheer op basis van rollen in Azure Portal.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>Hoe beheer ik Azure RBAC met PowerShell

U kunt de PowerShell-cmdlets gebruiken voor het beheren van Azure RBAC, met inbegrip van de volgende beheertaken:

- Lijst met rollen

- Zien wie toegang heeft

- Toegang verlenen

- Toegang verwijderen

- Een aangepaste beveiligingsrol maken

- Acties voor een Resourceprovider ophalen

- Een aangepaste rol wijzigen

- Een aangepaste rol verwijderen

- Lijst met aangepaste rollen

Zie voor instructies over het beheren van Azure RBAC met PowerShell [toegang op basis van rollen beheren met Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) is een in twee stappen verificatie-oplossing waarmee u toegang tot gegevens en toepassingen, beveiligt en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Het biedt krachtige verificatie via een reeks verificatiemethoden waaronder verificatie per telefoon, sms of mobiele app.

Voor het implementeren van MFA in de Azure-cloud, moet u deze eerst inschakelen en schakelt u verificatie in twee stappen voor gebruikers.

#### <a name="how-do-i-enable-azure-to-use-mfa"></a>Hoe schakel ik Azure MFA gebruiken?

Als uw gebruikers licenties met Azure multi-factor Authentication hebt, moet u gewoon Azure MFA configureren op een per gebruiker of groep. 

![Gebruikers MFA ingeschakeld](media/protect-personal-data-identity-access-controls/enable-mfa.png)

Als u momenteel geen licenties die u wilt gaan door het proces voor het bepalen van het meest geschikte implementatietype voor uw scenario. U kunt starten door te kijken naar het artikel [kiezen van de Azure multi-factor Autehntication-oplossing voor u](../multi-factor-authentication/multi-factor-authentication-get-started.md). Als u besluit dat u moet een multi-Factor Authentication-server maken. U kunt starten met de volgende stappen:

1. Selecteer **Active Directory** in de Azure portal (aangemeld als beheerder).

2. Selecteer **MFA-Server**

3. Geef een time-outwaarde. 

    ![](media/protect-personal-data-identity-access-controls/mfa-server-settings.png)

4. Klik op **opslaan**

In dit venster hebt u ook de optie voor het downloaden van de MFA-Server. U kunt extra informatie over hoe u de grootte en het plannen van uw implementatie aan de hand van het artikel krijgen [aan de slag met de Azure multi-factor Authentication-server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)

Zie voor meer instructies voor het beheren van uw multi-factor Authentication-Provider [aan de slag met een Azure multi-factor Authentication-Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Hoe schakel ik verificatie in twee stappen voor gebruikers?

U kunt afdwingen dat de verificatie in twee stappen voor alle aanmeldingen of kunt u beleidsregels voor voorwaardelijke toegang om te vereisen verificatie in twee stappen alleen wanneer bepaalde voorwaarden van toepassing.

Azure MFA inschakelen door het wijzigen van de status van gebruikers is de traditionele aanpak voor het vereisen van verificatie in twee stappen. Verificatie in twee stappen uitvoeren telkens wanneer ze zich dezelfde vereisten hebben alle gebruikers die u inschakelt. Alle beleidsregels voor voorwaardelijke toegang die mogelijk gevolgen hebben voor die gebruiker inschakelen van een gebruiker worden onderdrukt.

Azure MFA inschakelen met een beleid voor voorwaardelijke toegang is een flexibelere benadering voor het vereisen van verificatie in twee stappen. U kunt beleid voor voorwaardelijke toegang die voor zowel afzonderlijke gebruikers als groepen gelden maken. Met een hoog risico groepen meer beperkingen dan laag risico groepen kunnen worden opgegeven of verificatie in twee stappen kan worden alleen vereist voor met een hoog risico cloud-apps en voor de laag risico die zijn overgeslagen. Voorwaardelijke toegang is echter een betaald functie van Azure Active Directory.

Schakel MFA door het wijzigen van de status van gebruiker door het volgende doen:

1. Meld u aan bij de Azure portal als beheerder.
2. Ga naar **Azure Active Directory \> gebruikers en groepen \> alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Zoek de gebruiker die u wilt inschakelen voor Azure MFA. Mogelijk moet u de weergave bovenaan wijzigen.
5. Schakel het selectievakje naast de naam van de gebruiker.
6. Kies aan de rechterkant, onder snelle stappen **inschakelen**.

   ![](media/protect-personal-data-identity-access-controls/mfa-bulk.png)

7. Bevestig uw selectie in het pop-upvenster dat wordt geopend.  Gebruikers voor wie MFA is ingeschakeld wordt gevraagd om te registreren van de volgende keer dat ze zich aanmelden.

Schakel Azure MFA met beleid voor voorwaardelijke toegang door het volgende doen:

1. Meld u aan bij de Azure portal als beheerder.

2. Ga naar **Azure Active Directory \> voorwaardelijke toegang**.

3. Selecteer **nieuw beleid**.

4. Onder **toewijzingen**, selecteer **gebruikers en groepen**. Gebruik de **opnemen** en **uitsluiten** tabbladen om op te geven welke gebruikers en groepen worden beheerd door het beleid.

5. Onder **toewijzingen**, selecteer **Cloud-apps.** Kies voor **omvatten alle cloud-apps**.
6.  Onder **toegangscontroles**, selecteer **Grant**. Kies **meervoudige authenticatie**.
7.  Schakel **beleid inschakelen** naar **op** en selecteer vervolgens **opslaan**.

Voor informatie over het configureren van Azure MFA-instellingen voor Fraudewaarschuwingen instellen, eenmalig overslaan te maken, aangepaste spraakberichten gebruiken, caching configureren, goedgekeurde IP-adressen opgeven, app-wachtwoorden maken, schakelt u MFA onthouden voor apparaten die gebruikers vertrouwt, en selecteer verificatiemethoden, Zie [Azure multi-factor Authentication-instellingen configureren.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)

## <a name="next-steps"></a>Volgende stappen

- [Bevoegde toegang beveiligen in Azure AD](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Veelgestelde vragen over Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Probleemoplossing voor toegangsbeheer op basis van rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
