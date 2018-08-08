---
title: Een toegangsbeoordeling maken voor leden van een groep of gebruikers met toegang tot een toepassing met Azure AD | Microsoft Docs
description: Informatie over het maken van een toegangscontrole voor leden van een groep of gebruikers met toegang tot een toepassing.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 24df484a571e838eeaa9bd86ef69a3dd10843843
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620957"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Een toegangsbeoordeling van leden van beveiligingsgroep of toegang tot toepassingen maken met Azure AD

Toegangstoewijzingen worden 'verouderde' wanneer gebruikers toegang die ze niet meer nodig hebt. Beheerders kunnen Azure Active Directory (Azure AD) gebruiken om te maken van een toegangscontrole voor leden van beveiligingsgroep of gebruikers die zijn toegewezen aan een toepassing, verminderen het risico dat samenhangt met verouderde toegangstoewijzingen. Het maken van terugkerende toegangsbeoordelingen mag tijd te besparen. Als u Controleer regelmatig de gebruikers die toegang hebben tot een toepassing of lid zijn van een groep wilt, kunt u de frequentie van deze beoordelingen definiëren. Zie voor meer informatie over deze scenario's, [gebruikerstoegang beheren](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) en [beheren van toegang voor gasten](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Een toegangsbeoordeling maken

1. Als een globale beheerder of beheerder van gebruikersaccounts, gaat u naar de [pagina toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), en selecteer **programma's**.

2. Selecteer het programma waarin het toegangsbeheer controleren dat om te maken. **Standaardprogramma dat** is altijd aanwezig is, of u kunt een ander programma maken. Bijvoorbeeld, u kunt een programma voor elke initiatief naleving of zakelijke doelstelling.

3. Selecteer in het programma **besturingselementen**, en selecteer vervolgens **toevoegen** een besturingselement toevoegen.

4. De naam van de toegangsbeoordeling. (Optioneel) Geef de beoordeling van een beschrijving. De naam en beschrijving worden weergegeven aan de revisoren.

5. Stel de begindatum. Standaard een toegangsbeoordeling gebeurt eenmaal, hetzelfde moment die wordt deze gemaakt begint en eindigt in één maand. Kunt u het begin en einddatum hebben een toegang controleren start in de toekomst en laatste hoeveel dagen u dat wilt.

6. Als u de toegang tot revisie terugkerende, de frequentie van één keer te wijzigen, wekelijks, maandelijks, per kwartaal of jaar en gebruik de schuifregelaar of in het tekstvak voor het definiëren van het aantal dagen elke beoordeling van de reeks terugkerende is geopend voor invoer van de revisoren. Bijvoorbeeld, is de maximale duur voor u kunt instellen voor een maandelijkse controle 27 dagen om te voorkomen dat overlappende beoordelingen. 

7.  De reeks terugkerende toegangsbeoordelingen in 3 manieren kunt beëindigen: deze continu wordt uitgevoerd voor het starten van beoordelingen voor onbepaalde tijd, tot een bepaalde datum of na een opgegeven aantal exemplaren is voltooid. U, een andere beheerder van gebruikersaccounts of een ander algemeen beheerder kunt stoppen de reeks na het maken van door het veranderen van de datum in de instellingen, zodat deze wordt beëindigd op die datum.

8. Met toegangsbeoordelingen kunnen zich voor de leden van een groep of voor gebruikers die zijn toegewezen aan een toepassing. U kunt de toegang verder bereik bekijken om te bekijken alleen de gastgebruikers die lid zijn (of toegewezen aan de app), in plaats van het controleren van alle gebruikers die lid zijn of die toegang tot de toepassing hebben.

9. Selecteer een of meer mensen te bekijken van alle gebruikers in het bereik. Of u kunt selecteren om de leden hun eigen toegang beoordelen. Als de resource een groep is, kunt u vragen de eigenaren van groepen om te controleren. Ook kunt u vereisen dat de revisoren een reden opgeven wanneer ze toegang goedkeuren.

10. Als u de resultaten handmatig toepassen wilt als de controle is voltooid, klikt u op **Start**.  Anders de volgende sectie wordt uitgelegd hoe u de controle voor het automatisch configureren van toepassing.

### <a name="configuring-an-access-review-with-auto-apply"></a>Een toegangsbeoordeling configureren met auto-apply

1.  Vouw het menu voor na voltooiing van de instellingen en schakel automatisch resultaten toepassen op resource. 

2.  In gevallen waar gebruikers door de revisor niet zijn beoordeeld binnen de periode van de toegangsbeoordeling, hebt u de toegangsbeoordeling overnemen van het systeem de aanbeveling (indien ingeschakeld) weigeren/goedkeuren van blijvende toegang van de gebruiker, laat u de toegang die niet worden gewijzigd of verwijderen van hun de toegang. Dit is niet van invloed op gebruikers die handmatig door de revisoren zijn gecontroleerd als van de revisor van de definitieve beslissing weigeren, wordt de gebruiker toegang wordt verwijderd.

3.  Om in te schakelen de optie voor het nemen van aanbevelingen voor moeten revisoren niet reageren, vouwt u geavanceerde instellingen en aanbevelingen weergeven inschakelen.
 
4.  Klik tot slot, **Start**.

Op basis van uw selecties in na voltooiing van de instellingen, auto-toepassing zal worden uitgevoerd na de einddatum van de beoordeling of wanneer u de beoordeling handmatig stoppen. De status van de beoordeling wordt gewijzigd van voltooide via tussenliggende Staten zoals toepassen en tot slot naar status toegepast. U kunt verwachten om te zien van geweigerde gebruikers, indien van toepassing, wordt verwijderd uit de groepstoewijzing voor lidmaatschap of uw app in een paar minuten.


## <a name="manage-the-access-review"></a>Beheren van de toegangsbeoordeling

Standaard verzendt Azure AD een e-mailbericht naar de revisors kort nadat de evaluatie wordt gestart. Als u geen Azure AD-tenant het e-mailbericht verzenden, zorg er dan voor dat laten weten de revisoren een toegangsbeoordeling wordt gewacht om uit te voeren. U ze kunt weergeven, kunt u de instructies voor het [toegang beoordelen](active-directory-azure-ad-controls-perform-access-review.md). Als uw beoordeling voor gasten op hun eigen toegang beoordelen, geven ze de instructies voor het [uw eigen toegang beoordelen](active-directory-azure-ad-controls-perform-access-review.md).

Als sommige van de revisoren gasten, gasten krijgt een melding via e-mail alleen als ze al hun uitnodiging hebt geaccepteerd.

Voor het beheren van een reeks toegangsbeoordelingen, gaat u naar de toegangsbeoordeling van **besturingselementen**, en u toekomstige voorvallen niet vinden in de geplande beoordelingen, en de einddatum bewerken of toevoegen/verwijderen revisoren dienovereenkomstig. 

U kunt de voortgang volgen als de revisoren voltooid hun beoordelingen in het dashboard Azure AD in de **Toegangsbeoordelingen** sectie. Er is geen toegangsrechten worden gewijzigd in de map tot [de beoordeling is voltooid](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Volgende stappen

Wanneer een toegangsbeoordeling is gestart, Azure AD er automatisch revisoren verzendt een e-mail waarin wordt gevraagd om ze toegang beoordelen. Als een gebruiker is niet een e-mailbericht ontvangen, kunt u verzenden ze de instructies voor het [toegang beoordelen](active-directory-azure-ad-controls-perform-access-review.md). 

Als dit een eenmalige beoordeling, klikt u vervolgens of de beheerder van de toegangsbeoordeling, stopt de periode van de toegangsbeoordeling is via de stappen in [een toegangscontrole voltooien](active-directory-azure-ad-controls-complete-access-review.md) om te zien en de resultaten van toepassing.  

Als dit een reeks toegangsbeoordelingen, navigeer vervolgens naar **geschiedenis bekijken** op de pagina toegang controleren-serie, een voltooide toegangsbeoordeling selecteren.  Toekomstige revisies worden vermeld in **geplande beoordeling**, waar u kunt de duur, bewerken en toevoegen of revisoren voor afzonderlijke beoordelingen verwijderen.
