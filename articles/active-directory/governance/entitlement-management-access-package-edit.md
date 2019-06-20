---
title: Bewerken en beheren van een bestaand pakket toegang in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Informatie over het bewerken en beheren van een bestaand pakket met toegang tot beheer van de rechten van een Azure Active Directory (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73c1717249abac30847f96073ee6daf7d98112d7
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190337"
---
# <a name="edit-and-manage-an-existing-access-package-in-azure-ad-entitlement-management-preview"></a>Bewerken en beheren van een bestaand pakket toegang in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Een pakket toegang kunt u een eenmalige installatie van resources en beleidsregels die automatisch worden beheerd door de toegang voor de levensduur van het pakket toegang. Als een access-pakketmanager, kunt u de resources in een pakket met toegang op elk gewenst moment wijzigen zonder u zorgen te maken over het inrichten van de gebruiker toegang tot de nieuwe resources of de toegang verwijderen uit de vorige resources. Beleid kunnen ook op elk gewenst moment worden bijgewerkt, maar de wijzigingen in het beleid alleen van invloed op toegang tot een nieuwe.

In dit artikel wordt beschreven hoe u bewerken en beheren van toegang van bestaande pakketten.

## <a name="add-resource-roles"></a>Resource-rollen toevoegen

Een resourcerol is een verzameling van de machtigingen die zijn gekoppeld aan een resource. De manier waarop die u resources beschikbaar voor gebruikers om aan te vragen maken is door de resource-rollen toe te voegen aan uw pakket toegang. U kunt de resource-rollen voor groepen, toepassingen en SharePoint-sites toevoegen.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik in het menu links op **resourcerollen**.

1. Klik op **resource-rollen toevoegen** openen de functies van de resource toevoegen voor toegang tot de pagina.

    ![Toegang tot de pakket - resource-rollen toevoegen](./media/entitlement-management-access-package-edit/resource-roles-add.png)

1. Afhankelijk van of u toevoegen van een groep, de toepassing of de SharePoint-site wilt, moet u de stappen uitvoeren in een van de volgende secties van de resource-rol.

### <a name="add-a-group-resource-role"></a>De rol van een groep resource toevoegen

U hebt recht management gebruikers automatisch toevoegen aan een groep wanneer ze een pakket toegang worden toegewezen. 

- Wanneer een groep deel uitmaakt van een access-pakket en een gebruiker is toegewezen aan dit pakket toegang tot de gebruiker wordt toegevoegd aan die groep, als dit nog niet geïnstalleerd.
- Wanneer een gebruiker toegang pakkettoekenning is verlopen, worden ze verwijderd uit de groep, tenzij ze momenteel beschikken over een toewijzing aan een ander pakket van toegang dat die dezelfde groep bevat.

U kunt een Office 365-groep of Azure AD-beveiligingsgroep selecteren.  Beheerders kunnen een groep toevoegen aan een catalogus. eigenaren van de catalogus kunnen een groep toevoegen aan de catalogus als ze de eigenaar van de groep zijn. Houd rekening met de volgende Azure AD-beperkingen bij het selecteren van een groep:

- Wanneer een gebruiker, met inbegrip van een Gast wordt toegevoegd als een lid aan een groep, zien ze alle andere leden van die groep.
- Azure AD wijzigen niet van het lidmaatschap van een groep die is gesynchroniseerd vanuit Windows Server Active Directory met Azure AD Connect.  
- Het lidmaatschap van dynamische groepen kan niet worden bijgewerkt door het toevoegen of verwijderen van een lid, zodat het lidmaatschap van dynamische groepen zijn niet geschikt voor gebruik met beheer van rechten.

1. Op de **resource-rollen voor toegang tot pakket toevoegen** pagina, klikt u op **groepen** om het deelvenster van de groepen te openen.

1. Selecteer de groepen die u wilt opnemen in het pakket toegang.

    ![Toegang tot de pakket - resource-rollen - groepen toevoegen](./media/entitlement-management-access-package-edit/group-select.png)

1. Klik op **Selecteren**.

1. In de **rol** in de lijst met **eigenaar** of **lid**.

    Doorgaans selecteert u de rol van het lid. Als u de rol van eigenaar, die gebruikers selecteert kunnen andere leden of eigenaren toevoegen of verwijderen.

    ![Toegang tot de pakket - bron-rol voor een groep toevoegen](./media/entitlement-management-access-package-edit/group-role.png)

1. Klik op **Toevoegen**.

    Alle gebruikers met bestaande toewijzingen aan het pakket toegang wordt automatisch de leden van deze groep wanneer deze wordt toegevoegd.

### <a name="add-an-application-resource-role"></a>Een toepassingsrol resource toevoegen

U kunt Azure AD automatisch toewijzen gebruikers toegang tot een Azure AD-bedrijfstoepassing, met inbegrip van zowel SaaS-toepassingen en toepassingen van van uw organisatie is gefedereerd naar Azure AD, wanneer een gebruiker een access-pakket wordt toegewezen hebben. Voor toepassingen die kunnen worden geïntegreerd met Azure AD via federatieve eenmalige aanmelding, Azure AD worden tokens worden uitgegeven federation voor gebruikers die zijn toegewezen aan de toepassing.

Toepassingen kunnen meerdere rollen hebben. Bij het toevoegen van een toepassing naar een access-pakket, als deze toepassing meer dan één rol heeft, moet u de juiste rol voor die gebruikers opgeven.  Als u toepassingen ontwikkelt, vindt u meer over hoe deze rollen worden geleverd aan uw toepassingen in het artikel over het [configureren van de rol claim uitgegeven in het SAML-token](../develop/active-directory-enterprise-app-role-management.md).

Zodra een toepassingsrol deel uit van een pakket met toegang maakt:

- Wanneer een gebruiker, wordt dat pakket toegang toegewezen, de gebruiker is toegevoegd aan de toepassingsrol van deze, als dit nog niet geïnstalleerd.
- Wanneer een gebruiker toegang pakkettoekenning is verlopen, wordt hun toegang verwijderd uit de toepassing, tenzij ze beschikken over een toewijzing aan een ander pakket van toegang dat die toepassingsrol bevat.

Hier volgen enkele overwegingen bij het selecteren van een toepassing:

- Toepassingen kunnen ook toegewezen aan hun rollen en groepen hebben.  U kunt een groep in plaats van een toepassingsrol echter in een pakket toegang toevoegen en vervolgens de toepassing niet zichtbaar voor de gebruiker kan worden als onderdeel van het pakket toegang in de portal mijn toegang.

1. Op de **resource-rollen voor toegang tot pakket toevoegen** pagina, klikt u op **toepassingen** om het deelvenster Selecteer toepassingen te openen.

1. Selecteer de toepassingen die u wilt opnemen in het pakket toegang.

    ![Toegang tot de pakket - resource-rollen - Selecteer toepassingen toevoegen](./media/entitlement-management-access-package-edit/application-select.png)

1. Klik op **Selecteren**.

1. In de **rol** , selecteert u een toepassingsrol.

    ![Toegang tot de pakket - bron-rol voor een toepassing toevoegen](./media/entitlement-management-access-package-edit/application-role.png)

1. Klik op **Toevoegen**.

    Alle gebruikers met bestaande toewijzingen aan het pakket toegang wordt automatisch toegang krijgen tot deze toepassing wanneer deze wordt toegevoegd.

### <a name="add-a-sharepoint-site-resource-role"></a>Een SharePoint-site resource-rol toevoegen

Azure AD kan automatisch toewijzen gebruikers toegang aan een SharePoint Online-site of SharePoint Online-site-verzameling wanneer ze een pakket toegang worden toegewezen.

1. Op de **resource-rollen voor toegang tot pakket toevoegen** pagina, klikt u op **SharePoint-sites** om de SharePoint Online selecteren sites deelvenster te openen.

1. Selecteer de SharePoint Online sites die u wilt opnemen in het pakket toegang.

    ![Toegang tot de pakket - resource-rollen - Selecteer SharePoint Online-sites toevoegen](./media/entitlement-management-access-package-edit/sharepoint-site-select.png)

1. Klik op **Selecteren**.

1. In de **rol** , selecteert u een SharePoint Online-site-rol.

    ![Toegang tot de pakket - bron-rol voor een SharePoint Online-site toevoegen](./media/entitlement-management-access-package-edit/sharepoint-site-role.png)

1. Klik op **Toevoegen**.

    Alle gebruikers met bestaande toewijzingen aan het pakket toegang wordt automatisch toegang krijgen tot deze SharePoint Online-site wanneer deze wordt toegevoegd.

## <a name="remove-resource-roles"></a>Resource-rollen verwijderen

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik in het menu links op **resourcerollen**.

1. Zoek in de lijst van de resource-rollen, de functie van de resource die u wilt verwijderen.

1. Klik op het weglatingsteken ( **...** ) en klik vervolgens op **verwijderen resourcerol**.

    Alle gebruikers met bestaande toewijzingen aan het pakket toegang heeft automatisch de toegang ingetrokken voor de resourcerol van deze wordt verwijderd.

## <a name="add-a-new-policy"></a>Een nieuw beleid toevoegen

De manier die u opgeeft die een pakket toegang kunnen vragen is om een beleid te maken. U kunt meerdere beleidsregels voor een eenmalige toegang-pakket maken als u wilt toestaan dat verschillende sets van gebruikers om te worden toegekend toewijzingen met verschillende goedkeuring en instellingen voor verlooptijd. Een enkele beleidsregel kan niet worden gebruikt om interne en externe gebruikers toewijzen aan hetzelfde toegang-pakket. U kunt echter twee beleidsregels maken in hetzelfde pakket toegang--één voor interne gebruikers en één voor externe gebruikers. Als er meerdere beleidsregels die betrekking hebben op een gebruiker, wordt ze gevraagd op het moment van de aanvraag om te selecteren van het beleid dat ze graag willen worden toegewezen aan.

Het volgende diagram toont het proces op hoog niveau om een beleid voor een bestaand pakket met toegang te maken.

![Het beleidsvormingsproces van een maken](./media/entitlement-management-access-package-edit/policy-process.png)

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **beleid** en vervolgens **beleid toevoegen**.

1. Typ een naam en een beschrijving voor het beleid.

    ![Beleid maken met de naam en beschrijving](./media/entitlement-management-access-package-edit/policy-name-description.png)

1. Op basis van uw selectie voor **gebruikers die toegang kunnen vragen**, volg de stappen in een van de volgende secties van beleid.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="edit-an-existing-policy"></a>Een bestaande beleidsregel bewerken

U kunt een beleid op elk gewenst moment bewerken. Als u de vervaldatum voor een beleid wijzigt, wordt de vervaldatum voor aanvragen die zich al in een goedkeuring in behandeling of de status goedgekeurd niet wijzigen.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **beleid** en klik vervolgens op het beleid dat u wilt bewerken.

    De **beleidsdetails** deelvenster wordt geopend aan de onderkant van de pagina.

    ![Toegang tot package - beleid detailvenster](./media/entitlement-management-access-package-edit/policy-details.png)

1. Klik op **bewerken** om het beleid te bewerken.

    ![Toegang tot package - beleid bewerken](./media/entitlement-management-access-package-edit/policy-edit.png)

1. Wanneer u klaar bent, klikt u op **Update**.

## <a name="directly-assign-a-user"></a>Een gebruiker rechtstreeks toewijzen

In sommige gevallen kunt u specifieke gebruikers rechtstreeks toewijzen aan een pakket toegang zodat gebruikers niet hoeven te doorlopen van het proces voor het aanvragen van het pakket toegang. Voor het rechtstreeks gebruikers toewijzen, moet het pakket met toegang tot een beleid waarmee de beheerder directe toewijzingen hebben.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik in het menu links op **toewijzingen**.

1. Klik op **nieuwe toewijzing** gebruiker toevoegen aan pakket toegang openen.

    ![Toevoegen van toewijzingen - gebruiker toegang pakket](./media/entitlement-management-access-package-edit/assignments-add-user.png)

1. Klik op **gebruikers toevoegen** en selecteer de gebruikers die u wilt het pakket dat toegang moet toewijzen.

1. In de **Selecteer beleid** , selecteert u een beleid dat is de [geen (alleen directe toewijzingen beheerder)](#policy-none-administrator-direct-assignments-only) instelling.

    Als dit pakket toegang heeft geen dit type beleid, klikt u op **nieuw beleid maken** een toe te voegen.

1. Stel de datum en tijd die u wilt dat de geselecteerde gebruikers toewijzing moet beginnen en eindigen. Als een einddatum niet opgegeven is, worden de instellingen voor het vervallen van het beleid worden gebruikt.

1. Geef eventueel een reden voor de rechtstreekse toewijzing toe voor de record te houden.

1. Klik op **toevoegen** rechtstreeks de geselecteerde gebruikers toewijzen aan het pakket toegang.

    Na enkele ogenblikken, klikt u op **vernieuwen** om te zien van de gebruikers in de lijst met toewijzingen.

## <a name="view-who-has-an-assignment"></a>Weergave die een toewijzing heeft

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **toewijzingen** voor een overzicht van actieve toewijzingen.

1. Klik op een specifieke toewijzing voor aanvullende informatie.

1. Een lijst met toewijzingen die niet alle resourcerollen juist ingericht wilt bekijken, klikt u op de filterstatus en selecteer **leveren**.

    Kunt u extra details weergeven over levering fouten door te zoeken op de bijbehorende verzoek van de gebruiker op de **aanvragen** pagina.

1. Als u wilt zien verlopen toewijzingen, klikt u op de filterstatus en selecteer **verlopen**.

1. Voor het downloaden van een CSV-bestand van de gefilterde lijst, klikt u op **downloaden**.

## <a name="view-requests"></a>Aanvragen weergeven

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **aanvragen**.

1. Klik op een specifieke aanvraag om meer details te bekijken.

## <a name="view-a-requests-delivery-errors"></a>Een aanvraag voor de levering van fouten weergeven

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **aanvragen**.

1. Selecteer de aanvraag die u wilt weergeven.

    Als de aanvraag fouten levering heeft, de status van de aanvraag worden **Undelivered** en de substatus **gedeeltelijk geleverd**.

    Als er fouten levering in het detailpaneel van de aanvraag, wordt er een telling van de levering van fouten.

1. Klik op het aantal om alle foutberichten over de bezorging van de aanvraag te zien.

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren

U kunt alleen een aanvraag in behandeling die nog niet is geleverd annuleren.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op **aanvragen**.

1. Klik op de aanvraag die u wilt annuleren

1. Klik in het detailvenster van de aanvraag op **verzoek tot annuleren**.

## <a name="copy-my-access-portal-link"></a>Mijn toegang portal koppeling kopiëren

De meeste gebruikers in uw directory kunnen aanmelden bij de portal My Access en automatisch een overzicht van de toegang tot pakketten die kan worden aangevraagd. Echter, voor gebruikers van partnerorganisaties externe bedrijven die nog niet in uw directory, moet u stuurt u hun een koppeling die ze gebruiken kunnen om aan te vragen van een access-pakket. Als het pakket toegang is ingeschakeld voor externe gebruikers en u een beleid voor de externe gebruiker directory hebt, kunt de externe gebruiker de koppeling naar de mijn toegang gebruiken om aan te vragen van de access-pakket.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Kopieer op de pagina overzicht de **koppeling naar de mijn toegang**.

    ![Overzicht van Access-pakket - koppeling naar de mijn toegang](./media/entitlement-management-shared/my-access-portal-link.png)

1. E- of de koppeling naar uw externe zakelijke partners sturen. Ze kunnen de koppeling delen met gebruikers om aan te vragen van de access-pakket.

## <a name="change-the-hidden-setting"></a>Wijzig de instelling verborgen

Toegang tot pakketten zijn standaard kunnen worden gedetecteerd. Dit betekent dat als een beleid kan een gebruiker om aan te vragen van de access-pakket, automatisch ze het toegang pakket die worden vermeld in de portal mijn toegang zien.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik op de pagina overzicht **bewerken**.

1. Stel de **verborgen** instelling.

    Indien ingesteld op **Nee**, het pakket toegang wordt weergegeven in de gebruikersportal mijn toegang.

    Indien ingesteld op **Ja**, het pakket toegang wordt niet weergegeven in de gebruikersportal mijn toegang. De enige manier om een gebruiker kan het pakket toegang bekijken is als ze de directe **koppeling naar de mijn toegang** aan het pakket toegang.

## <a name="delete"></a>Verwijderen

Een pakket toegang kan alleen worden verwijderd als er geen actieve gebruiker-toewijzingen.

**Vereiste rol:** Gebruikersbeheerder, de eigenaar van catalogus of Pakketbeheer voor toegang

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten** en open vervolgens het pakket toegang.

1. Klik in het menu links op **toewijzingen** en toegang voor alle gebruikers verwijderen.

1. Klik in het menu links op **overzicht** en klik vervolgens op **verwijderen**.

1. In het verwijderbericht die wordt weergegeven, klikt u op **Ja**.

## <a name="when-are-changes-applied"></a>Wanneer worden de wijzigingen toegepast

Beheer van rechten van een worden Azure AD verwerkt door bulksgewijs wijzigingen voor toewijzing en resources in uw toegang tot pakketten meerdere keren per dag. Dus als u een toewijzing maken of wijzigen van de resource-rollen van uw pakket toegang, het tot 24 uur voor de wijziging duurt te worden gemaakt in Azure AD, plus de hoeveelheid tijd er nodig is voor deze wijzigingen doorvoeren in andere Microsoft Online Services of verbonden SaaS-toepassing s. Als de wijziging is van invloed op een paar objecten, duurt de wijziging waarschijnlijk slechts enkele minuten in Azure AD, waarna die andere Azure AD-onderdelen vervolgens detecteert wijzigen en bijwerken van de SaaS-toepassingen toe te passen. Als de wijziging is van invloed op duizenden objecten, wordt de wijziging langer duren. Bijvoorbeeld, als u een pakket toegang met 2 toepassingen en -toewijzingen van 100 gebruikers hebt en u wilt de rol van een SharePoint-site toevoegen aan het pakket toegang, kunnen er een vertraging totdat alle gebruikers deel uit van deze SharePoint-site-rol maken. U kunt de voortgang via het auditlogboek van Azure AD, het logboek van Azure AD-inrichting en de auditlogboeken van de SharePoint-site.

## <a name="next-steps"></a>Volgende stappen

- [De eigenaar van een catalogus of een pakket access manager toevoegen](entitlement-management-delegate.md#add-a-catalog-owner-or-an-access-package-manager)
- [Aanvraag verwerken en e-mailmeldingen](entitlement-management-process.md)
