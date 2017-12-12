---
title: Azure Active Directory Connect Health-bewerkingen
description: "Dit artikel wordt beschreven aanvullende bewerkingen die kunnen worden uitgevoerd nadat u Azure AD Connect Health hebt geïmplementeerd."
services: active-directory
documentationcenter: 
author: karavar
manager: mtillman
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 5e6910ea52f880e28378c9bf7fde02d080bc2e58
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-connect-health-operations"></a>Azure Active Directory Connect Health-bewerkingen
Dit onderwerp beschrijft de verschillende bewerkingen die u uitvoeren kunt met behulp van Azure Active Directory (Azure AD) Connect Health.

## <a name="enable-email-notifications"></a>E-mailmeldingen inschakelen
U kunt de Azure AD Connect Health-service voor het verzenden van e-mailmeldingen wanneer waarschuwingen wijzen op uw infrastructuur voor identiteiten is niet goed configureren. Dit gebeurt wanneer een waarschuwing wordt gegenereerd en wanneer het is opgelost.

![Schermopname van Azure AD Connect Health e-mailinstellingen voor meldingen](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> E-mailmeldingen zijn standaard ingeschakeld.
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD Connect Health e-mailmeldingen inschakelen
1. Open de **waarschuwingen** blade voor de service waarvoor u wilt ontvangen van e-mailmeldingen.
2. Klik in de actiebalk op **meldingsinstellingen**.
3. Selecteer bij de switch e-mailmelding **ON**.
4. Schakel het selectievakje in als u wilt dat alle globale beheerders voor het ontvangen van e-mailmeldingen.
5. Als u ontvangen van e-mailmeldingen op andere e-mailadressen wilt, geeft u ze op in de **extra e-mailontvangers** vak. Als u wilt een e-mailadres verwijderen uit deze lijst met de rechtermuisknop op de vermelding en selecteer **verwijderen**.
6. Om de wijzigingen hebt voltooid, klikt u op **opslaan**. Wijzigingen van kracht nadat u opslaan.

## <a name="delete-a-server-or-service-instance"></a>Verwijderen van een server of -service-exemplaar

In sommige gevallen wilt u mogelijk een server verwijderen uit het bewaakt. Dit is wat u moet weten om een server verwijderen uit de Azure AD Connect Health-service.

Wanneer u een server wilt verwijderen, rekening met het volgende:

* Deze actie stopt geen verdere gegevens verzameld van die server. Deze server wordt verwijderd uit de controleservice. Na deze actie kan u geen nieuwe waarschuwingen, bewaking of gebruik analytische gegevens voor deze server weer te geven.
* Deze actie worden de Health-Agent van uw server niet verwijderd. Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk fouten met betrekking tot de Health-Agent op de server.
* Deze actie worden de gegevens die al zijn verzameld van deze server niet worden verwijderd. Dat gegevens worden verwijderd volgens het gegevensretentiebeleid van Azure.
* Nadat u deze actie uitvoert, als u wilt starten van de controle van dezelfde server moet opnieuw, u verwijderen en opnieuw installeren van de Health-Agent op deze server.

### <a name="to-delete-a-server-from-the-azure-ad-connect-health-service"></a>Een server verwijderen uit de Azure AD Connect Health-service
Azure AD Connect Health voor Active Directory Federatieservices (AD FS) en Azure AD Connect (Sync):

1. Open de **Server** blade van de **serverlijst** blade door het selecteren van de naam van de server worden verwijderd.
2. Op de **Server** blade van de actiebalk, klikt u op **verwijderen**.
3. Bevestig de door de servernaam te typen in het bevestigingsvenster.
4. Klik op **Verwijderen**.

Azure AD Connect Health voor Azure Active Directory Domain Services:

1. Open de **domeincontrollers** dashboard.
2. Selecteer de domeincontroller worden verwijderd.
3. Klik in de actiebalk op **verwijderen ingeschakeld**.
4. Bevestig de actie voor het verwijderen van de server.
5. Klik op **Verwijderen**.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Verwijderen van een service-exemplaar van Azure AD Connect Health-service
In sommige gevallen is het raadzaam om te verwijderen van een service-exemplaar. Dit is wat u moet weten om te verwijderen van een service-exemplaar van de Azure AD Connect Health-service.

Wanneer u een service-exemplaar wilt verwijderen, rekening met het volgende:

* Deze actie wordt de huidige serviceinstantie verwijderd uit de controleservice.
* Deze actie niet verwijderen of de Health-Agent verwijderen uit een van de servers die als onderdeel van dit service-exemplaar zijn gecontroleerd. Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk fouten met betrekking tot de Health-Agent op de servers.
* Alle gegevens van deze service-exemplaar worden verwijderd volgens het gegevensretentiebeleid van Azure.
* Nadat u deze actie uitvoert als u wilt de bewaking van de service te starten, verwijderen en opnieuw installeren van de Health-Agent op alle servers. Nadat u deze actie uitvoert als u wilt bewaking dezelfde server opnieuw te starten, verwijderen, opnieuw te installeren en registreren van de Health-Agent op die server.

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a>Verwijderen van een service-exemplaar van de Azure AD Connect Health-service
1. Open de **Service** blade van de **servicelijst** blade door het selecteren van de service-id (farmnaam) die u wilt verwijderen.
2. Op de **Server** blade van de actiebalk, klikt u op **verwijderen**.
3. Bevestig door de servicenaam te typen in het bevestigingsvenster (bijvoorbeeld: sts.contoso.com).
4. Klik op **Verwijderen**.
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Toegang beheren met toegangsbeheer op basis van rollen
[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control-configure.md) voor Azure AD Connect Health toegang tot gebruikers en groepen dan globale beheerders biedt. RBAC toegewezen rollen voor de beoogde gebruikers en groepen en biedt een mechanisme om te beperken van de globale beheerders binnen uw directory.

### <a name="roles"></a>Rollen
Azure AD Connect Health ondersteunt de volgende ingebouwde rollen:

| Rol | Machtigingen |
| --- | --- |
| Eigenaar |Eigenaars kunnen *toegang beheren* (bijvoorbeeld een rol toewijzen aan een gebruiker of groep), *alle informatie bekijken* (bijvoorbeeld waarschuwingen weergeven) vanuit de portal en *instellingen wijzigen* (bijvoorbeeld e-mailmeldingen) in Azure AD Connect Health. <br>Standaard globale beheerders van Azure AD aan deze rol zijn toegewezen, en dit kan niet worden gewijzigd. |
| Inzender |Inzenders kunnen *alle informatie bekijken* (bijvoorbeeld waarschuwingen weergeven) vanuit de portal en *instellingen wijzigen* (bijvoorbeeld e-mailmeldingen) in Azure AD Connect Health. |
| Lezer |Lezers kunnen *alle informatie bekijken* (bijvoorbeeld waarschuwingen weergeven) vanuit de portal in Azure AD Connect Health. |

Alle andere functies (zoals beheerders van de gebruiker toegang of DevTest Labs gebruikers) hebben geen invloed voor toegang tot binnen de Azure AD Connect Health, zelfs als de rollen beschikbaar in de ervaring van de portal zijn.

### <a name="access-scope"></a>Toegangsbereik
Azure AD Connect Health biedt ondersteuning voor het beheer van toegang op twee niveaus:

* **Alle exemplaren van de service**: dit is de aanbevolen procedure in de meeste gevallen. Hiermee kunt u toegang voor alle exemplaren van de service (bijvoorbeeld een AD FS-farm) alle functie-typen die worden bewaakt door Azure AD Connect Health.
* **Service-exemplaar**: In sommige gevallen moet u wellicht toegang op basis van rollen typen of door een service-exemplaar te scheiden. In dit geval kunt u access op het niveau van de service-exemplaar beheren.  

Machtiging wordt toegewezen als een eindgebruiker toegang aan de directory of een service heeft level-instantie.

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a>Toestaan dat gebruikers of groepen toegang tot Azure AD Connect Health
De volgende stappen laten zien hoe om toegang te verlenen.
#### <a name="step-1-select-the-appropriate-access-scope"></a>Stap 1: Selecteer het juiste toegangsbereik
Voor een gebruikerstoegang op de *alle service-exemplaren* niveau binnen de Azure AD Connect Health, opent u de hoofdblade in Azure AD Connect Health.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>Stap 2: Gebruikers en groepen toevoegen en toewijzen van rollen
1. Van de **configureren** sectie, klikt u op **gebruikers**.<br>
   ![Schermopname van Azure AD Connect Health RBAC hoofdblade, met gebruikers die zijn gemarkeerd](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Selecteer **Toevoegen**.
3. In de **Selecteer een rol** deelvenster een rol selecteren (bijvoorbeeld **eigenaar**).<br>
   ![Schermopname van Azure AD Connect Health RBAC gebruikers venster](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Typ de naam of id van de betreffende gebruiker of groep. U kunt een of meer gebruikers of groepen selecteren op hetzelfde moment. Klik op **Selecteren**.
   ![Schermopname van Azure AD Connect Health RBAC gebruikers venster](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Selecteer **OK**.<br>
6. Nadat de roltoewijzing voltooid is, worden de gebruikers en groepen in de lijst weergegeven.<br>
   ![Schermopname van Azure AD Connect Health RBAC venster gebruikers met nieuwe gebruikers die zijn gemarkeerd](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Nu de vermelde gebruikers en groepen toegang hebben, volgens hun rollen.

> [!NOTE]
> * Globale beheerders altijd volledige toegang hebben tot alle bewerkingen, maar globale beheerdersaccounts zijn niet aanwezig in de bovenstaande lijst.
> * De functie gebruikers uitnodigen wordt niet ondersteund in Azure AD Connect Health.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Stap 3: De locatie van de blade delen met gebruikers of groepen
1. Nadat u machtigingen toewijzen, een gebruiker toegang krijgen tot Azure AD Connect Health door te gaan [hier](http://aka.ms/aadconnecthealth).
2. De gebruiker kan op de blade vastmaken de blade of verschillende onderdelen ervan naar het dashboard. Klik op de **vastmaken aan dashboard** pictogram.<br>
   ![Schermopname van Azure AD Connect Health RBAC blade vastmaken, met Punaisepictogram gemarkeerd](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Een gebruiker met de rol lezer kan geen Azure AD Connect Health-extensie niet ophalen uit Azure Marketplace. De gebruiker kan niet de vereiste 'maken' opnieuw om dit te doen uitvoeren. De gebruiker toegang tot de blade door te gaan naar de vorige koppeling. Voor de volgende syntaxis, kan de gebruiker de blade naar het dashboard vastmaken.
>
>

### <a name="remove-users-or-groups"></a>Gebruikers of groepen verwijderen
U kunt een gebruiker of groep toegevoegd aan Azure AD Connect Health RBAC verwijderen. Gewoon met de rechtermuisknop op de gebruiker of groep en selecteer **verwijderen**.<br>
![Schermopname van Azure AD Connect Health RBAC gebruikers venster verwijderen die zijn gemarkeerd](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="next-steps"></a>Volgende stappen
* [Azure AD Connect Health (Engelstalig)](active-directory-aadconnect-health.md)
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
