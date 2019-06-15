---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 05/16/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 6711506c1e489dcbd50aedd36241affc3bbed80b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66113393"
---
### <a name="policy-for-users-in-your-directory"></a>Beleid: Voor gebruikers in uw directory

Volg deze stappen als u wilt dat uw beleid voor gebruikers en groepen in uw directory dat dit pakket toegang kunnen vragen.

1. In de **gebruikers die toegang kunnen vragen** sectie, selecteer **voor gebruikers in uw directory**.

1. In de **gebruikers en groepen selecteren** sectie, klikt u op **gebruikers en groepen toevoegen**.

1. In de geselecteerde gebruikers en groepen deelvenster, selecteert u de gebruikers en groepen die u wilt toevoegen.

    ![Toegang tot package - beleid selecteren gebruikers en groepen](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Klik op **Selecteer** om toe te voegen de gebruikers en groepen.

1. Ga omlaag naar de [beleid: Aanvraag](#policy-request) sectie.

### <a name="policy-for-users-not-in-your-directory"></a>Beleid: Voor gebruikers die niet in uw directory

Volg deze stappen als u wilt dat uw beleid voor gebruikers die niet in uw directory dat dit pakket toegang kunnen vragen. Mappen moeten worden geconfigureerd om te worden toegestaan in de **organisatie relaties samenwerkingsbeperkingen** instellingen.

> [!NOTE]
> Een gastgebruikersaccount zullen worden gemaakt voor een gebruiker in uw directory waarvan aanvraag is goedgekeurd of automatisch goedgekeurd nog niet. De Gast wordt uitgenodigd, maar ontvangt een e-mailbericht voor de uitnodiging niet. In plaats daarvan ontvangen ze een e-mailbericht wanneer hun pakkettoekenning toegang wordt geleverd. Als dat niet langer gastgebruiker heeft standaard alle toegangstoewijzingen pakket, omdat de laatste toewijzing is verlopen of is geannuleerd, dat gastgebruikersaccount worden geblokkeerd voor aanmelden en verwijderd. Als u wilt dat gastgebruikers ook kunnen voor onbepaalde tijd in uw directory, zelfs als ze geen pakket toegangstoewijzingen hebben, kunt u de instellingen voor de rechten van een management-configuratie wijzigen.

1. In de **gebruikers die toegang kunnen vragen** sectie, selecteer **voor gebruikers die niet in uw directory**.

1. In de **Selecteer externe Azure AD-directory** sectie, klikt u op **mappen toevoegen**.

1. Voer een domeinnaam en zoek naar een externe Azure AD-directory.

1. Controleer of dat de juiste directory door de naam van de opgegeven map en het eerste domein is.

    > [!NOTE]
    > Alle gebruikers in de map is mogelijk om aan te vragen van dit pakket toegang. Dit omvat gebruikers uit alle subdomeinen die zijn gekoppeld aan de directory, niet alleen het domein dat wordt gebruikt in het zoekvak.

    ![Toegang tot package - mappen voor beleid selecteren](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Klik op **toevoegen** naar de map toe te voegen.

1. Herhaal deze stap als u wilt alle meer mappen toevoegen.

1. Nadat u alle mappen hebt toegevoegd u wilt opnemen in het beleid, klikt u op **Selecteer**.

1. Ga omlaag naar de [beleid: Aanvraag](#policy-request) sectie.

### <a name="policy-none-administrator-direct-assignments-only"></a>Beleid: Geen (alleen directe toewijzingen beheerder)

Volg deze stappen als u wilt dat uw beleid wilt omzeilen toegangsaanvragen en kunnen beheerders specifieke gebruikers rechtstreeks toewijzen aan het pakket toegang. Gebruikers hebben geen om aan te vragen van de access-pakket. U kunt nog steeds instellingen voor verlooptijd instellen, maar er zijn geen instellingen van de aanvraag.

1. In de **gebruikers die toegang kunnen vragen** sectie, selecteer **geen (alleen in directe toewijzingen beheerder**.

    Nadat u het pakket toegang hebt gemaakt, kunt u specifieke interne en externe gebruikers rechtstreeks toewijzen aan het pakket toegang. Als u een externe gebruiker opgeeft, wordt een gast-gebruikersaccount worden gemaakt in uw directory.

1. Ga omlaag naar de [beleid: Vervaldatum](#policy-expiration) sectie.

### <a name="policy-request"></a>Beleid: Aanvraag

In de sectie aanvraag geeft u instellingen voor goedkeuring wanneer gebruikers het pakket toegang aanvraagt.

1. Om te vereisen goedkeuring voor aanvragen van de geselecteerde gebruikers, stel de **goedkeuring vereisen** overzet naar **Ja**. Als u wilt aanvragen automatisch goedgekeurd, zet u de wisselknop op **Nee**.

1. Als u nodig hebt na goedkeuring de **fiatteurs selecteren** sectie, klikt u op **fiatteurs toevoegen**.

1. Selecteer een of meer gebruikers en/of groepen worden goedkeurders in het deelvenster Selecteer goedkeurders.

    Slechts één van de geselecteerde goedkeurders moet een aanvraag goedkeuren. Goedkeuring van alle goedkeurders is niet vereist. De goedkeuringsbeslissing is gebaseerd op de fiatteur de aanvraag eerst beoordeelt.

    ![Toegang tot package - beleid selecteren goedkeurders](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Klik op **Selecteer** om toe te voegen van de goedkeurders.

1. Klik op **geavanceerde instellingen weergeven** om aanvullende instellingen weer te geven.

    ![Toegang tot package - mappen voor beleid selecteren](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Instellen om te vereisen dat gebruikers een reden voor het aanvragen van het pakket met toegang op te geven, **reden vereisen** naar **Ja**.

1. Instellen om te vereisen dat de fiatteur een reden voor het goedkeuren van een aanvraag voor het pakket met toegang op te geven, **goedkeurder reden vereisen** naar **Ja**.

1. In de **time-out van aanvraag voor goedkeuring (dagen)** , geeft u de hoeveelheid tijd die de goedkeurders een aanvraag bekijken. Als u geen fiatteurs bekijken in dit aantal dagen, de aanvraag is verlopen en de gebruiker heeft een andere aanvraag voor het pakket toegang indienen.

### <a name="policy-expiration"></a>Beleid: vervaldatum

In de sectie verlopen, kunt u opgeven wanneer de toewijzing van een gebruiker aan het pakket toegang is verlopen.

1. In de **verlopen** sectie, stelt u de **toegang pakket verloopt** naar **op datum**, **aantal dagen**, of **nooit**.

    Voor **op datum**, selecteert u in de toekomst een vervaldatum.

    Voor **aantal dagen**, Geef een getal tussen 0 en 3660 dagen.

    Op basis van uw selectie, de toewijzing van een gebruiker aan het pakket toegang is op een bepaalde datum, een bepaald aantal dagen nadat ze zijn goedgekeurd, is verlopen of nooit.

1. Klik op **geavanceerde instellingen voor verlooptijd weergeven** om aanvullende instellingen weer te geven.

1. Instellen zodat de gebruiker om uit te breiden hun toewijzingen **toestaan dat gebruikers om uit te breiden toegang** naar **Ja**.

    Als de extensies zijn toegestaan in het beleid, ontvangt de gebruiker een e-mailbericht 14 dagen en ook 1 dag voordat hun toegang pakkettoekenning is ingesteld op verlopen wordt gevraagd om uit te breiden de toewijzing.

    ![Toegang tot package - instellingen voor beleid - verlooptijd](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>Beleid: Beleid inschakelen

1. Als u wilt dat het pakket toegang onmiddellijk beschikbaar worden gesteld aan gebruikers in het beleid, klikt u op **Ja** het beleid inschakelen.

    U kunt altijd deze inschakelen in de toekomst wanneer u klaar bent met het maken van het pakket toegang.

    ![Toegang tot package - beleid maken voor de beleidsinstelling](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Klik op **volgende** of **maken**.
