---
title: Azure Active Directory Power BI Content Pack gebruiken | Microsoft Docs
description: In dit artikel leest u hoe u Azure Active Directory Power BI Content Pack gebruikt.
services: active-directory
author: cawrites
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d56802e96028b6b01b6be749405c56df2648161
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988241"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Het Azure Active Directory Power BI-inhouds pakket gebruiken

|  |
|--|
|Op dit moment maakt het inhoudspakket voor Microsoft Azure AD Power BI gebruik van de Azure AD Graph-API's om gegevens op te halen uit uw Azure AD-tenant. Hierdoor ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met de [Microsoft Graph-API's voor rapportage](concept-reporting-api.md). |
|  |

Het Power BI inhouds pakket voor Azure Active Directory (Azure AD) bevat vooraf ontwikkelde rapporten waarmee u kunt begrijpen hoe uw gebruikers Azure AD-functies aannemen en gebruiken. Op deze manier kunt u inzicht krijgen in alle activiteiten binnen uw directory, met behulp van de geavanceerde visualisatie ervaring in Power BI. U kunt ook uw eigen dash board maken en dit delen met iedereen in uw organisatie. 

## <a name="prerequisites"></a>Vereisten

U hebt een Azure AD Premium-licentie (P1/P2) nodig voor het gebruik van het inhouds pakket. Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden.

## <a name="install-the-content-pack"></a>Het inhouds pakket installeren

Bekijk de [Quick](quickstart-install-power-bi-content-pack.md) start om het Azure AD Power bi-inhouds pakket te installeren.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Rapporten die zijn opgenomen in deze versie van het inhouds pakket van Azure AD-logboeken

De volgende rapporten zijn opgenomen in het Azure AD Power BI-inhouds pakket. De rapporten bevatten gegevens van de **afgelopen 30 dagen**.

**Rapport over app-gebruik en trends**:  Dit rapport geeft u inzicht in de toepassingen die in uw organisatie worden gebruikt. U kunt een lijst met veelgebruikte toepassingen ophalen of begrijpen hoe een toepassing die u onlangs hebt geïmplementeerd in uw organisatie wordt gebruikt. Zo kunt u het gebruik in de loop van de tijd bijhouden en verbeteren.

**Aanmeldingen per locatie en gebruikers**: Dit rapport bevat gegevens over alle aanmeldingen die zijn uitgevoerd met behulp van Azure-identiteit. Met dit rapport kunt u inzoomen op afzonderlijke aanmeldingen en vragen beantwoorden zoals:

- Waar is deze gebruiker aangemeld?
- Welke gebruiker heeft de meeste aanmeldingen en vanaf welke locatie? 
- Is het aanmelden gelukt?  
 
U kunt de resultaten ook filteren door een specifieke datum of locatie te selecteren.

**Unieke gebruikers per app**:  Dit rapport geeft een overzicht van alle unieke gebruikers die gebruikmaken van een bepaalde app. Het bevat alleen gebruikers die zijn aangemeldbij een toepassing.

Aanmeldingen voor **apparaten**: Dit rapport helpt u inzicht te krijgen in de verschillende apparaatprofielen die binnen uw organisatie worden gebruikt en om het apparaatbeleid op basis van gebruik te bepalen. Het biedt gegevens over het type besturings systeem en de browsers die worden gebruikt om u aan te melden bij toepassingen, samen met gedetailleerde informatie over de gebruikers, waaronder:

- Naam van gebruiker
- IP-adres
- Location 
- Aanmeldstatus 

**SSPR trechter**: Dit rapport helpt u inzicht te krijgen in de manier waarop het SSPR-hulp programma binnen uw organisatie wordt gebruikt. U kunt zien hoeveel wacht woord opnieuw is ingesteld via het hulp programma SSPR en hoeveel van deze zijn geslaagd. U kunt ook dieper uitvallen op het opnieuw instellen van wacht woorden en begrijpen waarom bepaalde storingen zich voordoen. 

## <a name="customize-azure-ad-activity-content-pack"></a>Het inhouds pakket voor Azure AD-activiteiten aanpassen

**Visualisatie wijzigen**:  U kunt een visualisatie van een rapport wijzigen door te klikken op **rapport bewerken** en de gewenste visualisatie te selecteren.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/10.png) 

**Aanvullende velden toevoegen**:  U kunt een veld toevoegen aan het rapport of het verwijderen door het visuele element te selecteren dat u wilt toevoegen aan of verwijderen van het veld. U kunt bijvoorbeeld het veld ' aanmeldings status ' toevoegen aan de tabel weergave, zoals hieronder wordt weer gegeven. 
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/11.png) 

**Visualisaties vastmaken aan het dash board**:  U kunt uw dash board aanpassen door uw eigen visualisaties in het rapport op te nemen en deze aan het dash board vast te maken. 

![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/13.png) 
 
**Het dash board delen**: U kunt het dash board ook delen met de gebruikers in uw organisatie. Zodra u het rapport hebt gedeeld, kunnen gebruikers de velden zien die u hebt geselecteerd in het rapport.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Een dagelijks vernieuwen van uw Power BI rapport plannen

Als u een dagelijkse vernieuwing van uw Power bi-rapport wilt plannen, gaat u naar **gegevens sets** > **instellingen** > **plannen vernieuwen** en stelt u deze in zoals hieronder wordt weer gegeven.
 
![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Bijwerken naar nieuwere versie van inhouds pakket

Als u uw inhouds pakket wilt bijwerken naar een nieuwere versie:

- Down load het nieuwe inhouds pakket en stel dit in met behulp van de instructies in dit artikel.

- Wanneer u het hebt ingesteld, gaat u naar **gegevens** > bron**instellingen** > **gegevens bron referenties** en voert u uw referenties opnieuw in.

    ![Azure Active Directory Power BI Content Pack](./media/howto-power-bi-content-pack/16.png) 

Wanneer u hebt gecontroleerd of de nieuwe versie van het inhouds pakket werkt zoals verwacht, kunt u indien nodig de oude versie verwijderen door de onderliggende rapporten en gegevens sets te verwijderen die aan dat inhouds pakket zijn gekoppeld.

## <a name="troubleshoot-content-pack-errors"></a>Problemen met inhouds pakketten oplossen

Wanneer u met het inhouds pakket werkt, kunt u de volgende fouten uitvoeren: 

- [Vernieuwen is mislukt](#refresh-failed) 
- [Kan de referenties van de gegevens bron niet bijwerken](#failed-to-update-data-source-credentials) 
- [Het importeren van gegevens duurt te lang](#data-import-is-too-slow) 

Voor algemene hulp met Power BI raadpleegt u deze [Help-artikelen](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Vernieuwen is mislukt 
 
**Hoe deze fout wordt opgehaald**: E-mail van de Power BI-of mislukte status in de geschiedenis vernieuwen. 


| Oorzaak | Oplossen |
| ---   | ---        |
| Fout bij het vernieuwen van fouten kan worden veroorzaakt wanneer de referenties van de gebruikers die verbinding maken met het inhouds pakket, opnieuw zijn ingesteld, maar niet zijn bijgewerkt in de verbindings instellingen van het inhouds pakket. | Ga in Power BI naar de gegevensset die overeenkomt met het dash board van Azure AD-activiteiten Logboeken (**Azure Active Directory activiteiten logboeken**), kies planning vernieuwen en voer uw Azure AD-referenties in. |
| Het vernieuwen kan mislukken vanwege gegevens problemen in het onderliggende inhouds pakket. | [Een ondersteunings ticket indienen](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>Kan de referenties van de gegevens bron niet bijwerken 
 
**Hoe deze fout wordt opgehaald**: Wanneer u in Power BI verbinding maakt met het inhouds pakket van Azure AD-activiteiten Logboeken. 

| Oorzaak | Oplossen |
| ---   | ---        |
| De gebruiker die verbinding maakt, is geen globale beheerder of beveiligings lezer of beveiligings beheerder. | Gebruik een account dat een globale beheerder of beveiligings lezer is, of een beveiligings beheerder voor toegang tot de inhouds pakketten. |
| Uw Tenant is geen Premium-Tenant of heeft niet ten minste één gebruiker met een Premium-licentie bestand. | [Een ondersteunings ticket indienen](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>Het importeren van gegevens is te langzaam 
 
**Hoe deze fout wordt opgehaald**: In Power BI, nadat u uw inhouds pakket hebt verbonden, wordt het gegevens import proces gestart om uw dash board voor te bereiden op Azure AD-activiteiten Logboeken. U ziet het bericht: **Gegevens importeren...** zonder verdere voortgang.  

| Oorzaak | Oplossen |
| ---   | ---        |
| Afhankelijk van de grootte van uw Tenant kan deze stap enkele minuten tot 30 minuten duren. | Als het bericht niet wordt gewijzigd zodat het dash board binnen een uur wordt weer gegeven, moet u [een ondersteunings ticket indienen](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Volgende stappen

* [Installeer Power bi inhouds pakket](quickstart-install-power-bi-content-pack.md).
* [Wat zijn Azure AD-rapporten?](overview-reports.md).
