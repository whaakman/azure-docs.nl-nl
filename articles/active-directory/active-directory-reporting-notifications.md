---
title: Azure Active Directory-rapportage meldingen
description: Het gebruik van de Azure Active Directory-rapportage meldingen voor verdacht modules.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: f4632bd2af802b10c8c64972e8c605d7ad7c0eaf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory-rapportage meldingen
## <a name="what-reports-generate-email-notifications"></a>Welke rapporten genereren e-mailmeldingen
Op dit moment de onregelmatige aanmelden activiteit rapport triggers e-mailmeldingen.

## <a name="what-is-an-irregular-sign-in"></a>Wat is een 'onregelmatige aanmelden'?
Onregelmatige aanmeldingen zijn bestanden die zijn geïdentificeerd door onze machine learning-algoritmen, op basis van een 'onmogelijke reis' voorwaarde gecombineerd met een afwijkende locatie aanmelden en het apparaat. Dit kan erop wijzen dat een hacker heeft is probeert aan te melden met dit account.

## <a name="who-receives-the-email-notifications"></a>Wie de e-mailmeldingen ontvangen?
Het e-mailbericht wordt verzonden naar alle globale beheerders die zijn toegewezen in een Active Directory Premium-licentie. Deze is geleverd, zodat verzenden we naar de beheerders alternatief e-mailadres ook. Beheerders moeten bevatten aad-alerts-noreply@mail.windowsazure.com in hun lijst met veilige afzenders zodat ze niet de e-mail niet.

## <a name="how-often-are-these-emails-sent"></a>Hoe vaak worden deze e-mailberichten verzonden?
Het e-mailbericht wordt verzonden als de 10 nieuwe onregelmatige aanmelden activiteiten plaatsvinden in de afgelopen 30 dagen of sinds het laatste e-mailbericht is verzonden, afhankelijk van wat kleiner is.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Hoe krijg ik toegang tot het rapport dat wordt vermeld in het e-mailbericht?
Wanneer u op de koppeling klikt, wordt u omgeleid naar de rapportpagina in de klassieke Azure portal. Als u het rapport opent, moet u beide:

* Een beheerder of co-beheerder van uw Azure-abonnement
* Een globale beheerder in de map en een Active Directory Premium-licentie toegewezen. Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.

## <a name="can-i-turn-off-these-emails"></a>Kan ik deze e-mailberichten uitschakelen?
Ja, u kunt uitschakelen meldingen betrekking hebben op afwijkende aanmeldingen in de klassieke Azure portal, op **configureren**, en selecteer vervolgens **uitgeschakelde** onder de **meldingen** sectie.

## <a name="whats-next"></a>Volgend onderwerp
* Meer wilt weten over welke beveiliging, controle en rapporten van activiteiten zijn beschikbaar? Bekijk [Azure AD-beveiliging, controle en rapporten van activiteiten](active-directory-view-access-usage-reports.md)
* [Aan de slag met Azure Active Directory Premium](active-directory-get-started-premium.md)
* [De huisstijl van uw bedrijf toevoegen aan de aanmeldingspagina en de toegangsvensterpagina’s](active-directory-add-company-branding.md)

