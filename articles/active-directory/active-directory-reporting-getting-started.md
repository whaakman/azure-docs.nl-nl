---
title: 'Azure Active Directory-rapportage: aan de slag | Microsoft Docs'
description: Bevat een lijst met de diverse beschikbare rapporten in Azure Active Directory-rapportage
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: eec9b73cbaccfa50eec6f237e4d1d810c6efa1d9
ms.openlocfilehash: e5b8ac91914203156bd395d7f462385e9f6dbcb4
ms.contentlocale: nl-nl
ms.lasthandoff: 02/24/2017


---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Aan de slag met Azure Active Directory-rapportage
## <a name="what-it-is"></a>Wat is het?
Azure Active Directory (Azure AD) bevat beveiligings-, activiteits- en controlerapporten voor uw directory. Hierna volgt een lijst met de beschikbare rapporten:

### <a name="security-reports"></a>Beveiligingsrapporten
* Aanmeldingen van onbekende bronnen
* Aanmeldingen na meerdere mislukte pogingen
* Aanmeldingen vanuit meerdere locaties
* Aanmeldingen van IP-adressen met verdachte activiteit
* Onregelmatige aanmeldingsactiviteiten
* Aanmeldingen vanaf mogelijk geïnfecteerde apparaten
* Gebruikers met afwijkende aanmeldingsactiviteiten

### <a name="activity-reports"></a>Activiteitsrapporten
* Toepassingsgebruik: samenvatting
* Toepassingsgebruik: gedetailleerd
* Toepassingsdashboard
* Fouten bij het inrichten van een account
* Afzonderlijke gebruikersapparaten
* Afzonderlijke gebruikersactiviteiten
* Rapport met groepsactiviteiten
* Rapport voor de registratie van opnieuw ingestelde wachtwoorden
* Activiteit voor wachtwoord opnieuw instellen

### <a name="audit-reports"></a>Controlerapporten
* Directorycontrolerapport

> [!TIP]
> Bekijk [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) voor meer documentatie over Azure AD-rapportage.
> 
> 

## <a name="how-it-works"></a>Hoe werkt het?
### <a name="reporting-pipeline"></a>Rapportagepijplijn
De rapportagepijplijn bestaat uit drie hoofdstappen. Elke keer wanneer een gebruiker zich aanmeldt of een verificatie wordt uitgevoerd, gebeurt het volgende:

* Eerst wordt de gebruiker geverifieerd (geslaagd of mislukt) en wordt het resultaat opgeslagen in de Azure Active Directory-servicedatabases.
* Op vaste intervallen worden alle recente aanmeldingen verwerkt. Op dit punt worden met onze algoritmen voor beveiliging en afwijkende activiteiten alle recente aanmeldingen doorzocht op verdachte activiteiten.
* Na de verwerking worden de rapporten geschreven, in de cache opgeslagen en weergegeven in de klassieke Azure-portal.

### <a name="report-generation-times"></a>Tijden waarop rapporten worden gegenereerd
Vanwege het grote aantal verificaties en aanmeldingen dat wordt verwerkt door het Azure AD-platform, zijn de meest recente verwerkte aanmeldingen gemiddeld een uur oud. Heel soms kan het acht uur duren voordat de meest recente aanmeldingen worden verwerkt.

U vindt de meest recente verwerkte aanmelding in de Help-tekst boven aan elk rapport.

![Help-tekst boven aan elk rapport](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Bekijk [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) voor meer documentatie over Azure AD-rapportage.
> 
> 

## <a name="getting-started"></a>Aan de slag
### <a name="sign-into-the-azure-classic-portal"></a>Aanmelden bij de klassieke Azure-portal
U moet u eerst aanmelden bij de [klassieke Azure-portal](https://manage.windowsazure.com) als globale beheerder of compliancebeheerder. U moet ook een servicebeheerder of medebeheerder zijn van het Azure-abonnement of gebruikmaken van het Azure-abonnement Toegang tot Azure AD.

### <a name="navigate-to-reports"></a>Navigeren naar rapporten
Als u rapporten wilt bekijken, gaat u naar het tabblad Rapporten boven aan uw directory.

Als dit de eerste keer is dat u de rapporten bekijkt, moet u akkoord gaan met de tekst in een dialoogvenster voordat u de rapporten kunt weergeven. Hiermee wordt gecontroleerd of beheerders in uw organisatie deze gegevens mogen bekijken omdat deze in sommige landen worden beschouwd als persoonlijke gegevens.

![Dialoogvenster](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Elk rapport verkennen
Navigeer in elk rapport om de verzamelde gegevens en verwerkte aanmeldingen te bekijken. U vindt [hier een lijst met alle rapporten](active-directory-reporting-guide.md).

![Alle rapporten](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>De rapporten downloaden als CSV
Elk rapport kan worden gedownload als een CSV-bestand (door komma's gescheiden waarden). U kunt deze bestanden gebruiken in Excel, Power BI of analyseprogramma's van derden om uw gegevens verder te analyseren.

Als u een rapport wilt downloaden als CSV-bestand, gaat u naar het rapport en klikt u onderaan op Downloaden.

![Knop Downloaden](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Bekijk [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) voor meer documentatie over Azure AD-rapportage.
> 
> 

## <a name="next-steps"></a>Volgende stappen
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Waarschuwingen voor afwijkende aanmeldingsactiviteiten aanpassen
Ga naar het tabblad Configureren van uw directory.

Ga naar de sectie Meldingen.

Schakel de sectie E-mailmeldingen van afwijkende aanmeldingen in of uit.

![De sectie Meldingen](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integreren met de rapportage-API van Azure AD
Zie [Aan de slag met de rapportage-API](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Multi-Factor Authentication inschakelen voor gebruikers
Selecteer een gebruiker in een rapport.

Klik op de knop MFA inschakelen onder in het scherm.

![De knop Multi-Factor Authentication onder in het scherm](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Bekijk [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) voor meer documentatie over Azure AD-rapportage.
> 
> 

## <a name="learn-more"></a>Meer informatie
### <a name="audit-events"></a>Controlegebeurtenissen
Lees meer over de gebeurtenissen die worden gecontroleerd in de directory in [Controlegebeurtenissen van Azure Active Directory-rapportage](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API-integratie
Zie [Aan de slag met de rapportage-API](active-directory-reporting-api-getting-started.md) en de [API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Contact opnemen
Stuur een e-mail naar [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) voor feedback, hulp of als u nog andere vragen hebt.

> [!TIP]
> Bekijk [Uw toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) voor meer documentatie over Azure AD-rapportage.
> 
> 


