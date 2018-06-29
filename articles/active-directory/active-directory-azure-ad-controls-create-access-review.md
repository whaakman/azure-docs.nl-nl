---
title: Maken van een access-bekijken voor leden van een groep of gebruikers met toegang tot een toepassing met Azure AD | Microsoft Docs
description: Informatie over het maken van een access-bekijken voor leden van een groep of gebruikers met toegang tot een toepassing.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: compliance-reports
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 20f238a7d8a3882f2a126c900e04ecf7be613be5
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084974"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Maken van een beoordeling toegang van de leden van beveiligingsgroep of toegang tot toepassingen met Azure AD

Toewijzingen Access verouderd ' ' wanneer gebruikers toegang die ze niet meer nodig hebt. Beheerders kunnen als u het risico dat samenhangt met toewijzingen van verouderde access, Azure Active Directory (Azure AD) voor het maken van een controle van toegang voor leden van de beveiligingsgroep of gebruikers die zijn toegewezen aan een toepassing gebruiken. Maken van terugkerende toegang beoordelingen mag tijd te besparen. Als u Controleer regelmatig de gebruikers die toegang hebben tot een toepassing of lid zijn van een groep wilt, kunt u de frequentie van deze beoordelingen definiëren. Zie voor meer informatie over deze scenario's [gebruikerstoegang beheren](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) en [gasttoegang beheren](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Een toegangsbeoordeling maken

1. Als een globale beheerder of gebruiker accountbeheerder, gaat u naar de [toegang controleert pagina](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), en selecteer **programma's**.

2. Selecteer het programma waarin het toegangsbeheer controleren dat u wilt maken. **Standaardprogramma** is altijd aanwezig is, of u kunt een ander programma maken. Bijvoorbeeld, u kunt een programma voor elke initiative naleving of zakelijke doel.

3. Selecteer in het programma **besturingselementen**, en selecteer vervolgens **toevoegen** een besturingselement toevoegen.

4. De naam van de controle van toegang. Geeft de revisie eventueel een beschrijving. De naam en beschrijving worden weergegeven op de revisoren.

5. Stel de begindatum. Standaard een controle van toegang tot deze gebeurtenis treedt op slechts één keer wordt gestart tegelijkertijd die wordt gemaakt en deze eindigt op één maand. Kunt u de begindatum en einddatum een toegang hebben controleren start in de toekomst en laatste echter het aantal dagen dat u wilt.

6. De frequentie van één keer te wijzigen, wekelijks, maandelijks, per kwartaal of jaarlijks zodat toegang controleren terugkerende en gebruik de schuifregelaar of in het tekstvak voor het definiëren van hoeveel dagen elke beoordeling van de reeks terugkerende worden geopend voor invoer van revisoren. De maximumduur voor u kunt instellen voor is een maandelijkse beoordeling bijvoorbeeld 27 dagen om te voorkomen dat er overlappende beoordelingen. 

7.  De reeks terugkerende toegang controleren op 3 manieren kunt beëindigen: wordt continu uitgevoerd voor het starten van de beoordelingen voor onbepaalde tijd, tot een bepaalde datum of na een opgegeven aantal exemplaren is voltooid. U, de beheerder van een andere gebruiker of een andere globale beheerder kunt stoppen de reeks na het maken door de datum in de instellingen wijzigen zodat deze wordt beëindigd op die datum.

8. Toegang beoordelingen kunnen zijn voor de leden van een groep of gebruikers die zijn toegewezen aan een toepassing. U kunt de toegang verder bereik bekijken om te bekijken alleen de gastgebruikers die lid zijn (of toegewezen aan de app), in plaats van de hand van de gebruikers die lid zijn of die toegang tot de toepassing hebben.

9. Selecteer een of meer personen om te controleren van alle gebruikers in het bereik. Of u kunt selecteren om de leden hun eigen toegang controleren. Als de bron een groep is, kunt u vragen de eigenaars Groepsbeleid om te controleren. Ook kunt u vereisen dat de revisoren een reden opgeven wanneer ze toegang goedkeurt.

10. Als u de resultaten handmatig toepassen wilt wanneer de controle is voltooid, klikt u op **Start**.  Anders wordt de volgende sectie wordt uitgelegd hoe u de revisie naar automatisch configureren van toepassing.

### <a name="configuring-an-access-review-with-auto-apply"></a>Een onderzoek toegang configureren met auto-apply

1.  Het menu uitvouwen na voltooiing instellingen en Schakel automatische toepassing resultaten op resource. 

2.  In gevallen waarin gebruikers zijn niet gecontroleerd door de revisor binnen de controleperiode hebt u de controle toegang ondernemen aanbeveling van het systeem (indien ingeschakeld) van de gebruiker continue toegang weigeren/goedkeuren, laat u de toegang niet worden gewijzigd of verwijderen van hun toegang. Dit heeft geen invloed op gebruikers die zijn onderzocht door de revisoren handmatig – als de uiteindelijke revisor besluit weigeren, wordt de gebruiker toegang wordt verwijderd.

3.  Om de optie voor het uitvoeren van aanbevelingen moeten revisoren niet reageren, vouwt u geavanceerde instellingen en weergeven, aanbevelingen inschakelen.
 
4.  Tot slot op **Start**.

Op basis van de gewenste opties op bij voltooiing instellingen, automatisch-toepassen zal worden uitgevoerd na de einddatum van de controle of wanneer u de controle handmatig stoppen. De status van de controle wordt gewijzigd van voltooid via tussenliggende status zoals toepassen en tot slot naar status toegepast. U mag verwachten geweigerde gebruikers indien aanwezig, wordt verwijderd uit de groep lidmaatschap of app-toewijzing over een paar minuten.


## <a name="manage-the-access-review"></a>De controle van toegang beheren

Standaard stuurt Azure AD een e-mailbericht naar revisoren kort nadat de controle wordt gestart. Als u geen Azure AD het e-mailbericht verzenden kiest, moet u informeren over de revisoren die een onderzoek toegang wacht ze zijn voltooid. U ze kunt weergeven, kunt u de instructies voor het [toegang controleren](active-directory-azure-ad-controls-perform-access-review.md). Als uw beoordeling voor gasten om te controleren van hun eigen toegang, tonen deze de instructies voor het [Controleer uw eigen toegang](active-directory-azure-ad-controls-perform-access-review.md).

Als sommige van de revisoren gasten, worden de gasten melding via e-mail alleen als ze al hun uitnodiging hebt geaccepteerd.

Voor het beheren van een reeks toegang beoordelingen, gaat u naar de revisie toegang van **besturingselementen**, en u toekomstige zoeken bij geplande revisies en de einddatum bewerken of toevoegen/verwijderen revisoren dienovereenkomstig. 

U kunt de voortgang volgen als de revisoren voltooid hun beoordelingen in het Azure AD-dashboard in de **toegang controleert** sectie. Er is geen toegangsrechten zijn gewijzigd in de map tot [de revisie is voltooid](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Volgende stappen

Wanneer een onderzoek toegang is gestart, verzendt Azure AD automatisch revisoren een e-mailbericht waarin wordt gevraagd om ze toegang te bekijken. Als een gebruiker een e-mailbericht niet hebt, kunt u ze verzenden de instructies voor het [toegang controleren](active-directory-azure-ad-controls-perform-access-review.md). 

Als dit een eenmalige revisie, nadat de controleperiode toegang via is of de beheerder de toegang-overzicht, stopt de stappen in [een beoordeling toegang voltooien](active-directory-azure-ad-controls-complete-access-review.md) om te bekijken en toepassen van de resultaten.  

Als dit een evaluatie-serie, navigeer naar **historie bekijken** op de pagina toegang controleren reeks, een overzicht van de voltooide toegang selecteren.  Toekomstige beoordelingen worden vermeld onder **gepland revisie**, kunt u de duur, bewerken en toevoegen of revisoren voor afzonderlijke revisies verwijderen.
