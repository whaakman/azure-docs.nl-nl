---
title: Azure AD Privileged Identity Management configureren | Microsoft Docs
description: Dit onderwerp wordt beschreven wat Azure AD Privileged Identity Management is en hoe u PIM gebruikt om de beveiliging van uw cloud te verbeteren.
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: c35b0c4171d411bd0c0483866f88780757cb3ee8
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Wat is Azure AD Privileged Identity Management?

Met Azure Active Directory (AD) Privileged Identity Management kunt u toegang binnen de organisatie beheren, controleren en bewaken. Dit geldt ook toegang tot bronnen in Azure AD, Azure-Resources (Preview) en andere Microsoft Online Services, zoals Office 365 of Microsoft Intune.

> [!NOTE]
> Privileged Identity Management is beschikbaar voor uw hele organisatie wanneer u uw beheerders met de Premium P2-editie van Azure Active Directory van licentie. Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.

Beperk het aantal gebruikers die toegang hebben tot beveiligde gegevens of bronnen, omdat die vermindert de kans op een kwaadwillende gebruiker die toegang verkrijgen of een geautoriseerde gebruiker per ongeluk die invloed hebben op een gevoelige resource dat organisaties willen.  Gebruikers moeten echter nog steeds bevoorrechte bewerkingen uitvoeren in Azure AD, Azure, Office 365 of SaaS-apps. Organisaties kunnen gebruikers bevoorrechte toegang geven tot Azure-resources zoals abonnementen en Azure AD. Er is een toezicht voor wat gebruikers met hun beheerdersbevoegdheden doen nodig. Azure AD Privileged Identity Management vermindert het risico van overmatige, overbodige of foutieve toegangsrechten.

Azure AD Privileged Identity Management helpt uw organisatie:

- Zien welke gebruikers bevoorrechte rollen voor het beheren van Azure-resources (Preview), en welke gebruikers beheerdersrechten rollen zijn toegewezen in Azure AD zijn toegewezen
- On-demand 'just in time' beheerderstoegang Microsoft Online Services zoals Office 365 en Intune en Azure-resources (Preview) van de abonnementen en resourcegroepen afzonderlijke resources, zoals virtuele Machines inschakelen 
-   Een historisch overzicht van de activering van de beheerder, inclusief wat beheerders wijzigingen aangebracht in Azure-resources (Preview)
- Ontvang waarschuwingen over wijzigingen in beheerderstoewijzingen
- Goedkeuring vereist voor het activeren van Azure AD bevoegde beheerdersrollen (Preview) 
- Controleer het lidmaatschap van beheerderrollen en vereisen dat gebruikers een reden voor voortdurende lidmaatschap op te geven

In Azure AD, Azure AD Privileged Identity Management de gebruikers die zijn toegewezen aan de ingebouwde kunt beheren organisatie Azure AD-functies, zoals globale beheerder. In Azure, worden de Azure AD Privileged Identity Management kunt beheren de gebruikers en groepen die zijn toegewezen via Azure RBAC rollen, met inbegrip van de eigenaar of Bijdrager.

## <a name="just-in-time-administrator-access"></a>Alleen bij beheerderstoegang tijd

In het verleden hebben toewijzen u een gebruiker aan een rol admin via de Azure-portal, andere Microsoft Online Services-portals of de Azure AD-cmdlets in Windows PowerShell. Als gevolg hiervan die gebruiker wordt een **permanente beheerder**, altijd actief is in de toegewezen rol. Azure AD Privileged Identity Management introduceert het concept van een **in aanmerking komende admin**. In aanmerking komende beheerders moeten gebruikers die de juiste rechten moeten toegang tot nu en dan maar niet een hele dag, elke dag. De rol is niet actief totdat de gebruiker toegang, moet vervolgens zij een activering te voltooien en een actieve beheerder voor een vooraf bepaalde hoeveelheid tijd zijn geworden. Steeds meer organisaties zijn met deze benadering voor verlagen of elimineren 'permanente beheerderstoegang' voor bevoorrechte rollen te kiezen.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privileged Identity Management voor uw directory inschakelen

U kunt starten met Azure AD Privileged Identity Management in de [Azure-portal](https://portal.azure.com/).

> [!NOTE]
> U moet een globale beheerder met een organisatieaccount (bijvoorbeeld @yourdomain.com), niet in een Microsoft-account (bijvoorbeeld @outlook.com), zodat de Azure AD Privileged Identity Management voor een map.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van uw directory.
2. Als uw organisatie meerdere directory's heeft, selecteert u uw gebruikersnaam rechtsboven in de Azure Portal. Selecteer de map waar u Azure AD Privileged Identity Management gebruikt.
3. Selecteer **Meer services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

Als u de eerste persoon die het gebruik van Azure AD Privileged Identity Management in uw directory bent en u naar Azure AD-directory-functies navigeren, en u naar Azure AD-directory-functies navigeren, een [beveiligingswizard](active-directory-privileged-identity-management-security-wizard.md) wordt u begeleid bij de eerste ervaring van de toewijzing. Hierna als u automatisch de eerste **beveiligingsbeheerder** en **beheerder met bevoorrechte rol** van de map.

Voor Azure AD-functies kan alleen een gebruiker die zich in de rol beheerder met bevoorrechte rol toewijzingen voor andere beheerders in Azure AD PIM beheren. U kunt [andere gebruikers de mogelijkheid voor het beheren van rollen in PIM directory geven](active-directory-privileged-identity-management-how-to-give-access-to-pim.md). Globale beheerders, Beveiligingsbeheerders en beveiliging lezers kunt toewijzingen aan Azure AD-rollen weergeven in Azure AD PIM.
Azure RBAC-rollen, kunt alleen de abonnementsbeheerder van een, een resource-eigenaar of beheerder voor een resource gebruikerstoegang beheren voor toewijzingen voor andere beheerders in Azure AD PIM.  Gebruikers die bevoorrechte rol beheerders, Beveiligingsbeheerders of beveiliging lezers zijn hebben niet standaard toegang tot het weergeven van toewijzingen aan Azure RBAC-rollen in Azure AD PIM.

## <a name="privileged-identity-management-overview-entry-point"></a>Overzicht van Privileged Identity Management (toegangspunt)

Azure AD Privileged Identity Management ondersteunt beheer van Azure AD-directory-rollen en functies voor Azure-Resources (Preview). De functie van de rollen voor Azure-resources verschillen van beheerdersrollen in Azure AD. Azure-resourcerollen bieden gedetailleerde machtigingen in voor de resource waarop ze worden toegewezen, en alle onderliggende resources in de resource-hiërarchie (bekend als overname). [Meer informatie over RBAC, resource-hiërarchie en overname](role-based-access-control-configure.md). PIM voor zowel Azure AD-directory rollen en Azure-Resources (Preview) kan worden beheerd door het openen van de koppeling onder het gedeelte van het beheren van de PIM-overzicht vermelding punt linkernavigatievenster menu.

PIM biedt snel toegang tot rollen activeren, bekijken in behandeling zijnde activeringen/aanvragen in behandeling zijnde goedkeuringen (voor Azure AD directory-functies), en in afwachting van uw antwoord in het gedeelte van de taken van het navigatiemenu links beoordeelt.

Bij het openen van een van de taken menu-items van het toegangspunt overzicht, bevat het resulterende weergave resultaten voor Azure AD-directory-functies en functies van Azure Resource (Preview).

![Snel starten](./media/active-directory-privileged-identity-management-configure/quick-start.png)

Mijn rollen bevatten een lijst met actieve en in aanmerking komende functietoewijzingen voor Azure AD-directory-functies en Azure Resource-rollen (Preview). [Meer informatie over het activeren van in aanmerking komende roltoewijzingen](active-directory-privileged-identity-management-how-to-activate-role.md).

Rollen activeren voor Azure-Resources (Preview) introduceert een nieuwe ervaring waarmee in aanmerking komende leden van een rol activeren voor een toekomstige datum/tijd plannen en selecteert u een specifieke activeringsduur binnen de toegestane door beheerders.

![](./media/active-directory-privileged-identity-management-configure/activations.png)

In het geval van een geplande activering is niet langer vereist, kunnen gebruikers hun aanvraag in behandeling annuleren door te navigeren naar aanvragen in behandeling in het menu linkernavigatiegedeelte en te klikken op de annuleren knop inline verzoek.

![Aanvragen in behandeling](./media/active-directory-privileged-identity-management-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management admin-dashboard

Azure AD Privileged Identity Manager biedt een beheerder dashboard waarmee u belangrijke informatie zoals:

* Waarschuwingen die wijzen op de mogelijkheden voor verbeterde beveiliging
* Het aantal gebruikers die zijn toegewezen aan elke bevoorrechte rol  
* Het aantal in aanmerking komende en permanente beheerders
* Een grafiek van activeringen voor bevoorrechte rollen in uw directory
*   Het aantal Just-In-Time, tijdsgebonden en permanente toewijzingen voor Azure Resource rollen (Preview)
*   Gebruikers en groepen met nieuwe roltoewijzingen in de afgelopen 30 dagen (Azure Resource rollen)


![PIM-dashboard - schermafbeelding][2]

## <a name="privileged-role-management"></a>Bevoorrechte rollen beheren

U kunt de beheerders toe te voegen of te verwijderen of de in aanmerking komende beheerders aan elke rol voor Azure AD-directory-functies beheren met Azure AD Privileged Identity Management. Met PIM voor Azure-Resources (Preview) eigenaars en beheerders van de gebruiker toegang globale beheerders waarmee beheer van abonnementen in de tenant kunnen toewijzen aan gebruikers of groepen aan rollen als in aanmerking komende Azure-resource (Just-In-Time access), of tijdsgebonden ( de activering is niet vereist) toegang met een begin- en einddatum/tijd- of permanente (indien ingeschakeld in de functie-instellingen).

![PIM toevoegen of verwijderen administrators - schermafbeelding][3]

## <a name="configure-the-role-activation-settings"></a>Configureer de instellingen van de activering

Met behulp van de [rolinstellingen](active-directory-privileged-identity-management-how-to-change-default-settings.md) kunt u de eigenschappen van de activering in aanmerking komende rol voor Azure AD-directory functies met inbegrip van:

* De duur van de rol activeringsperiode
* De melding van rolactivering
* De informatie die een gebruiker moet worden geleverd tijdens de activering van de rol
* Nummer van het ticket of incident service
* [Vereisten voor goedkeuring workflow - Preview](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM-instellingen - activering van de administrator - schermafbeelding][4]

Houd er rekening mee dat in de installatiekopie van de knoppen voor **multi-Factor Authentication** zijn uitgeschakeld. Voor bepaalde, maximaal beschermde rollen, MFA is vereist voor betere beveiliging.

Functie-instellingen voor Azure Resource rollen (Preview) kunnen beheerders Just-In-Time en rechtstreekse toewijzing toe instellingen, inclusief configureren:

- De mogelijkheid gebruikers of groepen toewijzen aan rollen zonder een einddatum/tijd (permanente toewijzing)
- De standaardduur van een toewijzing (indien niet permanent)
- De maximale activeringsduur (wanneer u een in aanmerking komende rollid activeert)
- De informatie die een gebruiker moet opgeven tijdens de rolactivering van de (Just In Time toewijzingen) of het toewijzingsproces (direct toewijzingen)

![](./media/active-directory-privileged-identity-management-configure/role-settings-details.png)

## <a name="role-activation"></a>Rolactivering

Naar [een rol activeren](active-directory-privileged-identity-management-how-to-activate-role.md), een in aanmerking komende beheerder vraagt een tijdsgebonden 'activation' voor de rol. De activering kan worden aangevraagd met behulp van de **mijn rol activeren** optie in Azure AD Privileged Identity Management.

Een beheerder die een rol activeren wil moet het initialiseren van Azure AD Privileged Identity Management in Azure portal.

Rolactivering kan worden aangepast. In de PIM-instellingen, kunt u bepalen de lengte van de activering en wat de beheerder moet worden geleverd voor het activeren van de rol informatie.

![PIM-beheerder aanvraag rolactivering - schermafbeelding][5]

## <a name="review-role-activity"></a>Rol controleactiviteit

Er zijn twee manieren om bij te houden hoe uw werknemers en beheerders bevoorrechte rollen gebruiken. Het gebruik van de eerste optie [Directory rollen controlegeschiedenis](active-directory-privileged-identity-management-how-to-use-audit-log.md). De controlegeschiedenis registreert wijzigingen bijhouden in de toewijzingen van bevoorrechte rol, rol activering geschiedenis en en wijzigingen in de instellingen voor Azure Resource rollen (Preview). 

![PIM-activering geschiedenis - schermafbeelding][6]

De tweede optie is het instellen van reguliere [toegang tot beoordelingen](active-directory-privileged-identity-management-how-to-start-security-review.md). Deze beoordelingen toegang kunnen worden uitgevoerd door en toegewezen revisor (zoals een teammanager) of de werknemers zelf kunnen bekijken. Dit is de beste manier om te controleren die nog steeds toegang is vereist en die niet langer wordt.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM op het abonnement is verlopen

Een tenant moet een Azure AD Premium P2 (of EMS E5) proefaccount of betaald abonnement hebben in hun tenant voordat u Azure AD PIM.  Bovendien moeten licenties worden toegewezen aan de groep administrators van de tenant.  In het bijzonder licenties moeten worden toegewezen aan beheerders in Azure AD-rollen wordt beheerd via Azure AD PIM, beheerders in Azure RBAC-rollen die worden beheerd via de Azure AD PIM en niet-beheerders gebruikers die toegang beoordelingen uitvoeren.
Als uw organisatie Azure AD Premium-P2 niet verlengt, of de proefversie verloopt, de Azure AD PIM-functies niet langer beschikbaar in uw tenant, in aanmerking komende roltoewijzingen wordt verwijderd en gebruikers zich niet langer rollen activeren. U kunt meer informatie in de [vereisten voor Azure AD PIM-abonnement](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
