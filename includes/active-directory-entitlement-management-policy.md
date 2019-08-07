---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 07/31/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: db16a2f122da1bf6c767e0a47c93c22f1882c406
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68817210"
---
### <a name="policy-for-users-in-your-directory"></a>Beleid: Voor gebruikers in uw Directory

Volg deze stappen als u uw beleid wilt gebruiken voor gebruikers in uw directory die dit toegangs pakket kunnen aanvragen.  De **gebruikers in uw directory** verwijzen naar zowel interne gebruikers als externe gebruikers die eerder zijn uitgenodigd voor de Directory, door hen het recht te geven met een ander toegangs pakket of om te worden uitgenodigd met Azure AD B2B. Wanneer u het beleid definieert, kunt u afzonderlijke gebruikers of meer vaak groepen gebruikers opgeven. Uw organisatie kan bijvoorbeeld al een groep hebben, zoals **alle werk nemers**.  Als deze groep wordt toegevoegd aan het beleid voor gebruikers die toegang kunnen aanvragen, kan een lid van die groep vervolgens toegang vragen.

1. Selecteer in de sectie **gebruikers die toegang kunnen aanvragen** , **de optie voor gebruikers in uw directory**.

    Houd er rekening mee dat de **voor gebruikers in uw directory** -instelling zowel leden gebruikers als gast gebruikers bevatten die zijn toegevoegd aan uw Directory. Als u alleen leden wilt toevoegen en niet gast gebruikers, selecteert u **voor gebruikers in uw directory** en selecteert u vervolgens een groep gebruikers. Als dat nodig is, kunt u een dynamische groep van uw gebruikers voor leden (User. User type-EQ "lid") maken. Zie voor meer informatie [dynamische lidmaatschaps regels voor groepen in azure Active Directory](../articles/active-directory/users-groups-roles/groups-dynamic-membership.md).

1. Klik in de sectie **gebruikers en groepen selecteren** op **gebruikers en groepen toevoegen**.

1. Selecteer in het deel venster gebruikers en groepen selecteren de gebruikers en groepen die u wilt toevoegen.

    ![Toegangs pakket-beleid: gebruikers en groepen selecteren](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Klik op **selecteren** om de gebruikers en groepen toe te voegen.

1. Ga naar het [beleid: Aanvraag](#policy-request) sectie.

### <a name="policy-for-users-not-in-your-directory"></a>Beleid: Voor gebruikers die niet in uw Directory voor komt

Volg deze stappen als u uw beleid wilt gebruiken voor gebruikers die zich niet in uw map bevindt en dit toegangs pakket kunnen aanvragen. De **gebruikers die zich niet in uw directory** bevinden, verwijzen naar gebruikers in een andere Azure AD-adres lijst en zijn mogelijk nog niet in uw directory uitgenodigd.  Directory's moeten worden geconfigureerd om te worden toegestaan in de instellingen voor **samenwerkings beperkingen van organisatie relaties** .

> [!NOTE]
> Er wordt een externe gast gebruikers account voor een gebruiker gemaakt die nog niet in uw directory is waarvan de aanvraag is goedgekeurd of automatisch goedgekeurd. De gast wordt uitgenodigd, maar ontvangt geen uitnodigings-e-mail. In plaats daarvan ontvangen ze een e-mail bericht wanneer hun toegangs pakket toewijzing wordt bezorgd. Later, wanneer deze gast gebruiker geen toegangs pakket toewijzingen meer heeft, omdat de laatste toewijzing is verlopen of is geannuleerd, wordt het gast gebruikers account geblokkeerd om zich aan te melden en vervolgens te verwijderen. Als u wilt dat gast gebruikers voor onbepaalde tijd in uw Directory blijven, zelfs als ze geen toegangs pakket toewijzingen hebben, kunt u de instellingen voor uw rechten beheer configuratie wijzigen.

1. Selecteer in de sectie **gebruikers die toegang kunnen aanvragen** , de optie **voor gebruikers die zich niet in uw directory**bevindt.

1. Klik in de sectie **externe Azure AD-Directory selecteren** op **mappen toevoegen**.

1. Voer een domein naam in en zoek naar een Azure AD-Directory met die domein naam.

1. Controleer of het de juiste map is op basis van de naam van de map en het eerste domein.

    > [!NOTE]
    > Alle gebruikers uit de Directory kunnen dit toegangs pakket aanvragen. Dit omvat gebruikers uit alle subdomeinen die zijn gekoppeld aan de Directory, niet alleen het domein dat wordt gebruikt in de zoek opdracht.

    ![Toegangs pakket-beleid: mappen selecteren](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Klik op **toevoegen** om de map toe te voegen.

1. Herhaal deze stap om nog meer directory's toe te voegen.

1. Wanneer u alle directory's hebt toegevoegd die u wilt toevoegen aan het beleid, klikt u op **selecteren**.

1. Ga naar het [beleid: Aanvraag](#policy-request) sectie.

### <a name="policy-none-administrator-direct-assignments-only"></a>Beleid: Geen (alleen directe toewijzingen voor beheerders)

Volg deze stappen als u wilt dat uw beleid de toegang tot aanvragen omzeilt en beheerders toestaat specifieke gebruikers rechtstreeks toe te wijzen aan het toegangs pakket. Gebruikers hoeven het toegangs pakket niet aan te vragen. U kunt nog steeds verloop instellingen instellen, maar er zijn geen aanvraag instellingen.

1. Selecteer in de sectie **gebruikers die toegang kunnen vragen** , de optie **geen (alleen directe toewijzingen beheerder**.

    Nadat u het toegangs pakket hebt gemaakt, kunt u rechtstreeks specifieke interne en externe gebruikers aan het toegangs pakket toewijzen. Als u een externe gebruiker opgeeft, wordt er een gast gebruikers account in uw directory gemaakt.

1. Ga naar het [beleid: De](#policy-expiration) sectie verval datum.

### <a name="policy-request"></a>Beleid: Aanvraag

In het gedeelte aanvraag geeft u goedkeurings instellingen op wanneer gebruikers het toegangs pakket aanvragen.

1. Als u goed keuring wilt vereisen voor aanvragen van de geselecteerde gebruikers, stelt u de schakel optie **goed keuring vereisen** in op **Ja**. Als u wilt dat aanvragen automatisch worden goedgekeurd, stelt u de wissel knop in op **Nee**.

1. Als u goed keuring nodig hebt, klikt u in de sectie **goed keurders selecteren** op **fiatteurs toevoegen**.

1. Selecteer in het deel venster goed keurders selecteren een of meer gebruikers en/of groepen voor goed keurders.

    Slechts één van de geselecteerde goed keurders moet een aanvraag goed keuren. Goed keuring van alle goed keurders is niet vereist. De goedkeurings beslissing is gebaseerd op welke fiatteur de aanvraag eerst bekijkt.

    ![Toegangs pakket-beleid: Selecteer goed keurders](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Klik op **selecteren** om de goed keurders toe te voegen.

1. Klik op **Geavanceerde instellingen voor aanvragen weer geven** om aanvullende instellingen weer te geven.

    ![Toegangs pakket-beleid: mappen selecteren](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Als u wilt dat gebruikers een reden opgeven om het toegangs pakket aan te vragen, stelt u in **dat** moet worden ingesteld op **Ja**.

1. Stel in dat de goed keurder een motivering moet geven voor het goed keuren van een aanvraag voor het toegangs pakket, stelt u de uitvulling van **goed keurder** in op **Ja**.

1. Geef in het vak **goedkeurings aanvraag time-out (dagen)** op hoe lang de fiatteurs een aanvraag moeten beoordelen. Als geen goed keurders dit aantal dagen bekijken, verloopt de aanvraag en moet de gebruiker nog een aanvraag indienen voor het toegangs pakket.

### <a name="policy-expiration"></a>Beleid: Verlooptijd

In de sectie verloop geeft u op wanneer de toewijzing van een gebruiker aan het toegangs pakket verloopt.

1. Stel in de sectie **verloop** het **toegangs pakket** in **op datum**, **aantal dagen**of **nooit**.

    Selecteer voor **op datum**in de toekomst een verval datum.

    Geef voor **aantal dagen**een getal tussen 0 en 3660 dagen op.

    Op basis van uw selectie verloopt de toewijzing van een gebruiker aan het toegangs pakket op een bepaalde datum, een bepaald aantal dagen nadat ze zijn goedgekeurd of nooit.

1. Klik op **Geavanceerde verloop instellingen weer geven** om de extra instellingen weer te geven.

1. Als u wilt dat gebruikers hun toewijzingen kunnen uitbreiden, stelt u **gebruikers toestaan** in om de toegang tot **Ja**uit te breiden.

    Als uitbrei dingen zijn toegestaan in het beleid, ontvangt de gebruiker een e-mail bericht van 14 dagen en ook 1 dag voordat de toewijzing van het toegangs pakket wordt ingesteld op verlopen om de toewijzing uit te breiden.

    ![Toegangs pakket-instellingen voor verlopen van beleid](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>Beleid: Beleid inschakelen

1. Als u wilt dat het toegangs pakket onmiddellijk beschikbaar is voor gebruikers in het beleid, klikt u op **Ja** om het beleid in te scha kelen.

    U kunt deze altijd in de toekomst inschakelen nadat u klaar bent met het maken van het toegangs pakket.

    ![Toegangs pakket-beleids instelling beleid inschakelen](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Klik op **volgende** of **maken**.
