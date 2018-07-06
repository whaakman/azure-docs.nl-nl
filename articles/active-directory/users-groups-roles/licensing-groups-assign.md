---
title: Licenties toewijzen aan een groep in Azure Active Directory | Microsoft Docs
description: Over het toewijzen van licenties aan gebruikers met behulp van Azure Active Directory-groep licentieverlening
services: active-directory
keywords: Azure AD-licenties
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a235851d7172d32d62c64b163e0b7635a1a47fd
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861870"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Licenties toewijzen aan gebruikers door groepslidmaatschappen in Azure Active Directory

Dit artikel helpt u bij het toewijzen van productlicenties aan een groep gebruikers in Azure Active Directory (Azure AD) en vervolgens verifiëren dat ze correct zijn gelicentieerd.

In dit voorbeeld bevat de tenant een beveiligingsgroep met de naam **HR-afdeling**. Deze groep bevat alle leden van de afdeling human resources (ongeveer 1000 gebruikers). U Office 365 Enterprise E3-licenties toewijzen aan de gehele afdeling. De Enterprise Yammer-service die opgenomen in het product moet tijdelijk worden uitgeschakeld totdat de afdeling gereed is om deze te gebruiken. U ook wilt implementeren van Enterprise Mobility + Security-licenties in dezelfde groep gebruikers.

> [!NOTE]
> Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat een licentie kan worden toegewezen aan een gebruiker, wordt de beheerder heeft om op te geven van de locatie-eigenschap van het gebruik van de gebruiker.

> Alle gebruikers geen gebruikslocatie opgegeven overnemen voor licentietoewijzing groep, de locatie van de map. Als u gebruikers op meerdere locaties hebt, raden wij aan dat u altijd gebruikslocatie als onderdeel van de gebruikersstroom maken in Azure AD (bijvoorbeeld via AAD Connect-configuratie) - die ervoor zorgt het resultaat van licentietoewijzing dat altijd juist is en gebruikers geen ontvangen Services op locaties die niet zijn toegestaan.

## <a name="step-1-assign-the-required-licenses"></a>Stap 1: De vereiste licenties toewijzen

1. Aanmelden bij de [ **Azure-portal** ](https://portal.azure.com) met een Administrator-account. Het account moet een globale beheerder rol of de gebruiker de accountbeheerder voor het beheren van licenties.

2. Selecteer **alle services** in het navigatiedeelvenster links in en selecteer vervolgens **Azure Active Directory**. U kunt dit deelvenster toevoegen aan Favorieten of vastmaken aan het portaldashboard.

3. Op de **Azure Active Directory** venster **licenties** om een deelvenster waar u kunt zien en beheren van alle licentieproducten in de tenant te openen.

4. Onder **alle producten**, Office 365 Enterprise E3 en Enterprise Mobility + Security selecteren door de productnamen te selecteren. Selecteer eerst de toewijzing **toewijzen** aan de bovenkant van het deelvenster.

   ![Alle producten toewijzen-licentie](./media/licensing-groups-assign/all-products-assign.png)

5. Op de **licentie toewijzen** deelvenster, klikt u op **gebruikers en groepen** openen de **gebruikers en groepen** deelvenster. Zoek de naam van de *HR-afdeling*, selecteert u de groep en moet u controleren of om te controleren door te klikken op **Selecteer** aan de onderkant van het deelvenster.

   ![Een groep selecteren](./media/licensing-groups-assign/select-a-group.png)

6. Op de **licentie toewijzen** deelvenster, klikt u op **toewijzingsopties (optioneel)**, weergeven met alle service-abonnementen opgenomen in de twee producten die we eerder hebt geselecteerd. Zoek **Yammer Enterprise** en schakel het **uit** om uit te schakelen die service van de productlicentie. Controleer of door te klikken op **OK** aan de onderkant van **toewijzingsopties**.

   ![Toewijzingsopties](./media/licensing-groups-assign/assignment-options.png)

7. Voltooid de toewijzing voor de **licentie toewijzen** deelvenster, klikt u op **toewijzen** aan de onderkant van het deelvenster.

8. Een melding wordt weergegeven in de rechterbovenhoek waarin de status en het resultaat van het proces. Als de toewijzing aan de groep kan niet worden voltooid (bijvoorbeeld vanwege bestaande licenties in de groep), klikt u op de melding om details van de fout te bekijken.

We hebben hebt een licentiesjabloon voor de groep HR-afdeling nu opgegeven. Een achtergrondproces in Azure AD is gestart voor het verwerken van alle bestaande leden van die groep. Deze eerste bewerking kan enige tijd, afhankelijk van de huidige grootte van de groep duren. De volgende stap wordt beschreven hoe om te verifiëren dat het proces is voltooid en bepalen als meer aandacht is vereist voor het oplossen van problemen.

> [!NOTE]
> U kunt dezelfde toewijzing starten vanaf een alternatieve locatie: **gebruikers en groepen** in Azure AD. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**. Zoek vervolgens de groep, selecteert u deze en gaat u naar de **licenties** tabblad. De **toewijzen** knop boven op het deelvenster opent u het deelvenster van de toewijzing van licentie.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Stap 2: Controleren of de eerste toewijzing is voltooid

1. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**. Gaat u naar de **HR-afdeling** groep voor die licenties zijn toegewezen aan.

2. Op de **HR-afdeling** groep venster **licenties**. Hiermee kunt u snel bevestigen als licenties volledig zijn toegewezen aan gebruikers en als er fouten optreden die u nodig hebt om uit te zoeken. De volgende informatie is beschikbaar:

   - Lijst van productlicenties die momenteel zijn toegewezen aan de groep. Selecteer een item om weer te geven van de specifieke services die zijn ingeschakeld en wijzigingen aanbrengen.

   - De status van de meest recente licentiewijzigingen die zijn aangebracht aan de groep (als de wijzigingen worden verwerkt of als de verwerking is voltooid voor alle gebruikersleden van de).

   - Informatie over gebruikers die zich in een foutstatus omdat licenties kunnen niet worden toegewezen aan.

   ![Toewijzingsopties](./media/licensing-groups-assign/assignment-errors.png)

3. Meer gedetailleerde informatie over het verwerken van onder licentie **Azure Active Directory** > **gebruikers en groepen** > *groepsnaam*  >  **Auditlogboeken**. Houd rekening met de volgende activiteiten:

   - Activiteit: **Start toepassen op basis van groepslicentie op gebruikers**. Dit wordt geregistreerd wanneer het systeem het wijzigen van het toewijzen van licenties op de groep wordt opgehaald en wordt gestart toe te passen op alle gebruikersleden van de. Bevat informatie over de wijziging die is gemaakt.

   - Activiteit: **toepassen op basis van groepslicentie op gebruikers voltooien**. Dit wordt geregistreerd wanneer het systeem is verwerkt alle gebruikers in de groep. Het bevat een overzicht van hoeveel gebruikers zijn verwerkt en hoeveel gebruikers Groepslicenties kunnen niet worden toegewezen.

   [Lees deze sectie](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) voor meer informatie over hoe de logboeken voor controle kunnen worden gebruikt voor het analyseren van wijzigingen van Groepslicenties.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Stap 3: Controleren op problemen en op te lossen

1. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**, en zoek de **HR-afdeling** groep voor die licenties zijn toegewezen aan.
2. Op de **HR-afdeling** groep venster **licenties**. De melding boven op het deelvenster ziet u dat er 10 gebruikers die licenties kunnen niet worden toegewezen aan zijn. Erop te klikken, opent u een lijst van alle gebruikers in een status-licentieverlening-fout voor deze groep.
3. De **mislukte toewijzingen** kolom kan worden achterhaald dat beide productlicenties kunnen niet worden toegewezen aan de gebruikers. De **belangrijkste reden voor mislukken** kolom bevat de oorzaak van het probleem. In dit geval heeft **conflicterende serviceabonnementen**.

   ![Mislukte toewijzingen](./media/licensing-groups-assign/failed-assignments.png)

4. Selecteer een gebruiker te openen de **licenties** deelvenster. Dit deelvenster toont alle licenties die momenteel zijn toegewezen aan de gebruiker. In dit voorbeeld wordt de gebruiker heeft de Office 365 Enterprise E1-licentie die is overgenomen van de **Kiosk gebruikers** groep. Dit veroorzaakt een conflict met de E3-licentie die het systeem heeft geprobeerd om toe te passen van de **HR-afdeling** groep. Als gevolg hiervan is geen van de licenties van die groep toegewezen aan de gebruiker.

   ![Licenties voor een gebruiker weergeven](./media/licensing-groups-assign/user-license-view.png)

5. Als u wilt dit conflict is opgelost, verwijdert u de gebruiker van de **Kiosk gebruikers** groep. Nadat Azure AD de wijziging, verwerkt de **HR-afdeling** licenties correct worden toegewezen.

   ![Licentie correct toegewezen](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functieset voor Licentiebeheer via groepen, de volgende artikelen:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Identificeren en oplossen van problemen voor een groep in Azure Active Directory](licensing-groups-resolve-problems.md)
* [Hoe gebruikers met een afzonderlijke licentie migreren naar het Groepslicenties in Azure Active Directory](licensing-groups-migrate-users.md)
* [Azure Active Directory-groep op basis van aanvullende scenario's-licentieverlening](../active-directory-licensing-group-advanced.md)
