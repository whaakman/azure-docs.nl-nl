---
title: Maken van een access-bekijken voor leden van een groep of gebruikers met toegang tot een toepassing met Azure AD | Microsoft Docs
description: Informatie over het maken van een access-bekijken voor leden van een groep of gebruikers met toegang tot een toepassing.
services: active-directory
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b2f8985f12e17ac69543cfb3a33725f796eedde8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Maken van een beoordeling toegang van de leden van beveiligingsgroep of toegang tot toepassingen met Azure AD

Toewijzingen Access verouderd ' ' wanneer gebruikers toegang die ze niet meer nodig hebt. Beheerders kunnen als u het risico dat samenhangt met toewijzingen van verouderde access, Azure Active Directory (Azure AD) voor het maken van een controle van toegang voor leden van de beveiligingsgroep of gebruikers die zijn toegewezen aan een toepassing gebruiken. Zie voor meer informatie over deze scenario's [gebruikerstoegang beheren](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) en [gasttoegang beheren](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Maken van een toegang controleren

1. Als een globale beheerder bent, gaat u naar de [toegang controleert pagina](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), en selecteer **programma's**.

2. Selecteer het programma waarin het toegangsbeheer controleren dat u wilt maken. **Standaardprogramma** is altijd aanwezig is, of u kunt een ander programma maken. Bijvoorbeeld, u kunt een programma voor elke initiative naleving of zakelijke doel.

3. Selecteer in het programma **besturingselementen**, en selecteer vervolgens **toevoegen** een besturingselement toevoegen.

4. Naam van elke toegang controleren. Geef elke Bekijk desgewenst een beschrijving. De naam van de worden aan de revisoren weergegeven.

5. De begin- en einddatum instellen. Een toegang controleren standaard dezelfde dag wordt gemaakt, en eindigt in een maand wordt gestart. Kunt u de begindatum en einddatum een toegang hebben controleren start in de toekomst en laatste echter het aantal dagen dat u wilt.

6. Toegang beoordelingen kunnen zijn voor de leden van een groep of gebruikers die zijn toegewezen aan een toepassing. U kunt de toegang verder bereik bekijken om te bekijken alleen de gastgebruikers die lid zijn (of toegewezen aan de app), in plaats van de hand van de gebruikers die lid zijn of die toegang tot de toepassing hebben.

7. Selecteer een of meer personen om te controleren van alle gebruikers in het bereik. Of u kunt selecteren om de leden hun eigen toegang controleren. Als de bron een groep is, kunt u vragen de eigenaars Groepsbeleid om te controleren. Ook kunt u vereisen dat de revisoren een reden opgeven wanneer ze toegang goedkeurt.

8. Tot slot selecteert **Start**.


## <a name="manage-the-access-review"></a>De controle van toegang beheren

Standaard stuurt Azure AD een e-mailbericht naar revisoren kort nadat de controle wordt gestart. Als u geen Azure AD het e-mailbericht verzenden kiest, moet u informeren over de revisoren die een onderzoek toegang wacht ze zijn voltooid. U ze kunt weergeven, kunt u de instructies voor het [toegang controleren](active-directory-azure-ad-controls-perform-access-review.md). Als uw beoordeling voor gasten om te controleren van hun eigen toegang, tonen deze de instructies voor het [Controleer uw eigen toegang](active-directory-azure-ad-controls-perform-access-review.md).

Als sommige van de revisoren gasten, worden de gasten melding via e-mail alleen als ze al hun uitnodiging hebt geaccepteerd.


U kunt de voortgang volgen als de revisoren voltooid hun beoordelingen in het Azure AD-dashboard in de **toegang controleert** sectie. Er is geen toegangsrechten zijn gewijzigd in de map tot [de revisie is voltooid](active-directory-azure-ad-controls-complete-access-review.md).

## <a name="next-steps"></a>Volgende stappen

Wanneer een onderzoek toegang is gestart, verzendt Azure AD automatisch revisoren een e-mailbericht waarin wordt gevraagd om ze toegang te bekijken. Als een gebruiker een e-mailbericht niet hebt, kunt u ze verzenden de instructies voor het [toegang controleren](active-directory-azure-ad-controls-perform-access-review.md). 

Nadat de periode van toegang controleren of de beheerder Hiermee stopt u de controle van toegang, volg de stappen in [een beoordeling toegang voltooien](active-directory-azure-ad-controls-complete-access-review.md) om te bekijken en toepassen van de resultaten.


