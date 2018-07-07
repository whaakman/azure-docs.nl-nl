---
title: Azure Active Directory Connect Health-bewerkingen
description: Dit artikel wordt beschreven aanvullende bewerkingen die kunnen worden uitgevoerd nadat u Azure AD Connect Health hebt geïmplementeerd.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: mtillman
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 68be732b2a375d1667c743ac277ce653c78105f1
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37886866"
---
# <a name="azure-active-directory-connect-health-operations"></a>Azure Active Directory Connect Health-bewerkingen
Dit onderwerp beschrijft de verschillende bewerkingen die u uitvoeren kunt met behulp van Azure Active Directory (Azure AD) Connect Health.

## <a name="enable-email-notifications"></a>E-mailmeldingen inschakelen
U kunt de Azure AD Connect Health-service voor het verzenden van e-mailmeldingen wanneer waarschuwingen wijzen op uw infrastructuur voor identiteiten is niet in orde. Dit gebeurt wanneer een waarschuwing wordt gegenereerd, en wanneer het is opgelost.

![Schermafbeelding van de Azure AD Connect Health e-mailinstellingen voor melding](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> E-mailmeldingen zijn standaard ingeschakeld.
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD Connect Health e-mailmeldingen inschakelen
1. Open de **waarschuwingen** blade voor de service waarvoor u wenst te e-mailbericht ontvangen.
2. Klik in de actiebalk op **meldingsinstellingen**.
3. Selecteer bij de switch e-mailmelding **ON**.
4. Schakel het selectievakje in als u wilt dat alle globale beheerders voor het ontvangen van e-mailmeldingen.
5. Als u ontvangen van e-mailmeldingen op andere e-mailadressen wilt, geeft u deze in de **aanvullende e-mailontvangers** vak. Als u wilt een e-mailadres verwijderen uit deze lijst, met de rechtermuisknop op de vermelding en selecteer **verwijderen**.
6. Voor het voltooien van de wijzigingen, klikt u op **opslaan**. Wijzigingen van kracht nadat u opslaan.

## <a name="delete-a-server-or-service-instance"></a>Een server of -service-exemplaar verwijderen

>[!NOTE] 
> Azure AD premium-licentie is vereist voor de stappen verwijderen.

In sommige gevallen kunt u een server verwijderen uit die worden bewaakt. Dit is wat u moet weten om te verwijderen van een server uit de Azure AD Connect Health-service.

Wanneer u een server wilt verwijderen, houd rekening met het volgende:

* Deze actie stopt het verzamelen van gegevens van die server. Deze server is verwijderd uit de controleservice. Na deze actie kan u geen nieuwe waarschuwingen, bewaking of gebruik analytics-gegevens voor deze server weer te geven.
* Deze actie wordt de Health-Agent niet verwijderd van uw server. Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk fouten met betrekking tot de Health-Agent op de server.
* Deze actie worden de gegevens die al zijn verzameld van deze server niet verwijderd. Dat gegevens worden verwijderd volgens het gegevensretentiebeleid van Azure.
* Deze actie hebt uitgevoerd, als u wilt beginnen met controleren van dezelfde server moet, u verwijderen en opnieuw installeren van de Health-Agent op deze server.

### <a name="delete-a-server-from-the-azure-ad-connect-health-service"></a>Een server verwijderen uit de Azure AD Connect Health-service

>[!NOTE] 
> Azure AD premium-licentie is vereist voor de stappen verwijderen.

Azure AD Connect Health voor Active Directory Federatieservices (AD FS) en Azure AD Connect (Sync):

1. Open de **Server** blade van de **serverlijst** blade door de naam van de server moet worden verwijderd.
2. Op de **Server** blade in de actiebalk, klikt u op **verwijderen**.
3. Controleer of door de servernaam te typen in het bevestigingsvenster.
4. Klik op **Verwijderen**.

Azure AD Connect Health voor Azure Active Directory Domain Services:

1. Open de **domeincontrollers** dashboard.
2. Selecteer de domeincontroller moet worden verwijderd.
3. Klik in de actiebalk op **verwijderen ingeschakeld**.
4. Controleer of de actie voor het verwijderen van de server.
5. Klik op **Verwijderen**.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Verwijderen van een service-exemplaar van Azure AD Connect Health-service
In sommige gevallen is het raadzaam om te verwijderen van een service-exemplaar. Dit is wat u moet weten om te verwijderen van een service-exemplaar van de Azure AD Connect Health-service.

Wanneer u een service-exemplaar wilt verwijderen, houd rekening met het volgende:

* Hiermee verwijdert u het huidige service-exemplaar uit de controleservice.
* Deze actie niet verwijderen of de Health-Agent verwijderen van een van de servers die als onderdeel van dit service-exemplaar zijn gecontroleerd. Als u de Health-Agent niet hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk fouten met betrekking tot de Health-Agent op de servers.
* Alle gegevens van dit service-exemplaar wordt verwijderd in overeenstemming met het gegevensretentiebeleid van Azure.
* Deze actie hebt uitgevoerd als u beginnen met controleren van de service wilt, verwijderen en opnieuw installeren van de Health-Agent op alle servers. Deze actie hebt uitgevoerd als u beginnen met dezelfde server opnieuw te controleren wilt, verwijderen, opnieuw installeren en registreren van de Health-Agent op die server.

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a>Verwijderen van een service-exemplaar van de Azure AD Connect Health-service
1. Open de **Service** blade van de **servicelijst** blade door het selecteren van de service-id (farmnaam) die u wilt verwijderen.
2. Op de **Server** blade in de actiebalk, klikt u op **verwijderen**.
3. Controleer of door de servicenaam te typen in het bevestigingsvenster (bijvoorbeeld: sts.contoso.com).
4. Klik op **Verwijderen**.
   <br><br>

[//]: # (Begin van RBAC-sectie)
## <a name="manage-access-with-role-based-access-control"></a>Toegang beheren met toegangsbeheer op basis van rollen
[Rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/role-assignments-portal.md) voor Azure AD Connect Health toegang tot gebruikers en groepen dan globale beheerders biedt. RBAC wijst rollen toe aan de beoogde gebruikers en groepen, en biedt een mechanisme om te beperken van de globale beheerders in uw directory.

### <a name="roles"></a>Rollen
Azure AD Connect Health biedt ondersteuning voor de volgende ingebouwde rollen:

| Rol | Machtigingen |
| --- | --- |
| Eigenaar |Eigenaren kunnen *toegang beheren* (bijvoorbeeld een rol toewijzen aan een gebruiker of groep), *weergeven van alle gegevens* (bijvoorbeeld waarschuwingen weergeven) vanuit de portal en *instellingen wijzigen* (voor bijvoorbeeld, e-mailmeldingen) in Azure AD Connect Health. <br>Standaard globale beheerders van Azure AD aan deze rol zijn toegewezen en deze kan niet worden gewijzigd. |
| Inzender |Inzenders kunnen *weergeven van alle gegevens* (bijvoorbeeld waarschuwingen weergeven) vanuit de portal en *instellingen wijzigen* (bijvoorbeeld e-mailmeldingen) in Azure AD Connect Health. |
| Lezer |Lezers kunnen *weergeven van alle gegevens* (bijvoorbeeld waarschuwingen weergeven) uit de portal in Azure AD Connect Health. |

Alle andere functies (zoals gebruiker toegang beheerders of gebruikers van DevTest Labs) hebben geen gevolgen heeft voor toegang in Azure AD Connect Health, zelfs als de rollen beschikbaar in de portal-ervaring zijn.

### <a name="access-scope"></a>Toegangsbereik
Azure AD Connect Health biedt ondersteuning voor beheren van toegang op twee niveaus:

* **Alle exemplaren van de service**: dit is de aanbevolen procedure in de meeste gevallen. Hiermee kunt u toegang voor alle exemplaren van de service (bijvoorbeeld een AD FS-farm) voor alle rollen-typen die door Azure AD Connect Health worden bewaakt.
* **Service-exemplaar**: In sommige gevallen moet u wellicht toegang op basis van roltypen of door een service-exemplaar te scheiden. In dit geval kunt u de toegang op het niveau van de service-exemplaar beheren.  

Machtiging is verleend als een eindgebruiker toegang op de map of de service heeft level-exemplaar.

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a>Toestaan dat gebruikers of groepen toegang tot Azure AD Connect Health
De volgende stappen laten zien hoe om toegang te verlenen.
#### <a name="step-1-select-the-appropriate-access-scope"></a>Stap 1: Selecteer het juiste toegangsbereik
Waarmee een gebruikerstoegang op de *alle service-exemplaren* niveau in Azure AD Connect Health, opent u de hoofdblade in Azure AD Connect Health.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>Stap 2: Gebruikers en groepen toevoegen en toewijzen van rollen
1. Uit de **configureren** sectie, klikt u op **gebruikers**.<br>
   ![Schermafbeelding van de Azure AD Connect Health resource zijbalk](./media/active-directory-aadconnect-health/startRBAC.png)
2. Selecteer **Toevoegen**.
3. In de **Selecteer een rol** deelvenster, selecteert u een rol (bijvoorbeeld **eigenaar**).<br>
   ![Schermafbeelding van de Azure AD Connect Health RBAC gebruikers venster](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Typ de naam of id van de betreffende gebruiker of groep. U kunt een of meer gebruikers of groepen selecteren op hetzelfde moment. Klik op **Selecteren**.
   ![Schermafbeelding van de Azure AD Connect Health RBAC gebruikers venster](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Selecteer **OK**.<br>
6. Nadat de roltoewijzing voltooid is, worden de gebruikers en groepen weergegeven in de lijst.<br>
   ![Schermafbeelding van de Azure AD Health RBAC-gebruikers verbinding maken met venster met nieuwe gebruikers die zijn gemarkeerd](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Nu de vermelde gebruikers en groepen toegang hebben, op basis van hun rollen.

> [!NOTE]
> * Globale beheerders hebben altijd volledige toegang tot alle bewerkingen, maar global administrator-accounts zijn niet aanwezig in de voorgaande lijst.
> * De functie gebruikers uitnodigen wordt niet ondersteund in Azure AD Connect Health.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Stap 3: De locatie van de blade delen met gebruikers of groepen
1. Nadat u machtigingen hebt toegewezen, een gebruiker toegang krijgen tot Azure AD Connect Health door te gaan [hier](https://aka.ms/aadconnecthealth).
2. Op de blade kan de gebruiker de blade of verschillende onderdelen van deze, naar het dashboard vastmaken. Klik op de **vastmaken aan dashboard** pictogram.<br>
   ![Schermafbeelding van de Azure AD Connect Health RBAC blade vastmaken, met het pictogram vastmaken is gemarkeerd](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Een gebruiker met de rol lezer kan niet aan Azure AD Connect Health-extensie van de Azure Marketplace. De gebruiker uitvoeren niet de benodigde 'maken' de bewerking om dit te doen. De gebruiker nog steeds toegang hebt tot de blade door te gaan naar de vorige koppeling. Voor de volgende syntaxis, kan de gebruiker de blade aan het dashboard vastmaken.
>
>

### <a name="remove-users-or-groups"></a>Gebruikers of groepen verwijderen
U kunt een gebruiker of groep toegevoegd aan Azure AD Connect Health RBAC verwijderen. Gewoon met de rechtermuisknop op de gebruiker of groep, en selecteer **verwijderen**.<br>
![Schermafbeelding van de Azure AD Connect Health RBAC gebruikers venster met verwijderen gemarkeerd](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (Einde van RBAC-sectie)

## <a name="next-steps"></a>Volgende stappen
* [Azure AD Connect Health (Engelstalig)](active-directory-aadconnect-health.md)
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
