---
title: Azure Active Directory voorwaardelijke toegang wat gebeurt er als hulpprogramma - preview | Microsoft Docs
description: Meer informatie over hoe u de configuratie van uw beleid voor voorwaardelijke toegang van Azure Active Directory kunt testen.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/21/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: de6b3dcd77132154e583d7333983d6745c4aa3bd
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-active-directory-conditional-access-what-if-tool---preview"></a>Azure Active Directory voorwaardelijke toegang wat gebeurt er als hulpprogramma - preview

[Voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) is een functie van Azure Active Directory (Azure AD) dat u bepalen hoe kunt gemachtigde gebruikers toegang hebben uw cloud-apps. Hoe u weet wat ze kunnen verwachten formulier de beleidsregels voor voorwaardelijke toegang in uw omgeving? Als u wilt deze vraag te beantwoorden, kunt u de **voorwaardelijke toegang wat gebeurt er als hulpprogramma**.

Dit artikel wordt uitgelegd hoe u dit hulpprogramma kunt gebruiken voor het testen van uw beleid voor voorwaardelijke toegang.

## <a name="what-it-is"></a>Wat is het?

De **voorwaardelijke toegang tot wat als hulpprogramma beleid** kunt u het effect van uw beleid voor voorwaardelijke toegang van uw omgeving. In plaats van de test uw beleid aangedreven door meerdere aanmeldingen handmatig uit te voeren, kunt dit hulpprogramma u een gesimuleerde aanmelden van een gebruiker evalueren. De simulatie maakt een schatting van de impact dit aanmelden heeft op uw beleid en een simulatierapport genereert. Het rapport lijst bevat niet alleen de toegepaste voorwaardelijke toegangsbeleid maar ook [klassieke beleid](active-directory-conditional-access-migration.md#classic-policies) indien ze bestaan.    

De wat bepalen als extra biedt ook een manier om snel de beleidsregels die van toepassing op een specifieke gebruiker. Kunt u de informatie, bijvoorbeeld, als u wilt een probleem op te lossen.  

## <a name="how-it-works"></a>Hoe werkt het?

In de **voorwaardelijke toegang wat gebeurt er als hulpprogramma**, moet u eerst de instellingen configureren van u wilt simuleren scenario. Deze instellingen omvatten:

- De gebruiker die u wilt testen 

- De gebruiker probeert te krijgen tot de cloud-apps

- De voorwaarden waaraan de toegang tot de cloud configureert apps wordt uitgevoerd
     
Een volgende stap kunt u een simulatie uitvoeren die wordt geëvalueerd als uw instellingen te starten. Alleen de beleidsregels die zijn ingeschakeld uitmaken deel van een evaluatie uitgevoerd.


Wanneer de evaluatie is voltooid, genereert het hulpprogramma een rapport van het betreffende beleid.


## <a name="running-the-tool"></a>Het programma wordt uitgevoerd

U vindt de **wat gebeurt er als** hulpprogramma op de  **[voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)**  pagina in de Azure-portal.

Voor het starten van het hulpprogramma op de werkbalk boven op de lijst met beleidsregels, klikt u op **wat gebeurt er als**.

![Wat als](./media/active-directory-conditional-access-whatif/01.png)

Voordat u een evaluatie uitvoeren kunt, moet u de instellingen configureren.

## <a name="settings"></a>Instellingen

Deze sectie vindt u informatie over de instellingen van de simulatie uitvoeren.

![Wat als](./media/active-directory-conditional-access-whatif/02.png)


### <a name="user"></a>Gebruiker

U kunt slechts één gebruiker selecteren. Dit is de enige vereiste veld.

### <a name="cloud-apps"></a>Cloud-apps

De standaardwaarde voor deze instelling is **alle cloud-apps**. De standaardinstelling voert een evaluatie van alle beschikbare beleidsregels in uw omgeving. U kunt u het bereik aan beleidsregels die invloed hebben op specifieke cloud-apps beperken.


### <a name="ip-address"></a>IP-adres

Het IP-adres is één IPv4-adres om na te bootsen de [locatie voorwaarde](active-directory-conditional-access-azure-portal.md#locations). Het adres vertegenwoordigt Internetgericht adres van het apparaat dat wordt gebruikt door de gebruiker aan te melden. U kunt controleren of het IP-adres van een apparaat, bijvoorbeeld, navigeren naar [wat Mijn IP-adres is](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Apparaatplatformen

Deze instelling lijkt de [apparaat platforms voorwaarde](active-directory-conditional-access-azure-portal.md#device-platforms) en geeft u het equivalent van **alle platforms (inclusief niet-ondersteunde)**. 
### <a name="client-apps"></a>Client-apps

Deze instelling lijkt de [client apps voorwaarde](active-directory-conditional-access-azure-portal.md#client-apps).
Standaard deze instelling zorgt ervoor dat een evaluatie van alle beleidsregels met **Browser** of **mobiele apps en bureaublad-clients** ofwel afzonderlijk of beide geselecteerd. Detecteert ook beleidsregels die afdwingen **Exchange ActiveSync (EAS)**. U kunt deze instelling afbakenen door te selecteren:

- **Browser** om alle beleidsregels met ten minste te evalueren **Browser** geselecteerde. 

- **Mobiele apps en bureaublad-clients** om alle beleidsregels met ten minste te evalueren **mobiele apps en bureaublad-clients** geselecteerde. 


### <a name="sign-in-risk"></a>Aanmeldingsrisico

Deze instelling lijkt de [aanmelden risico voorwaarde](active-directory-conditional-access-azure-portal.md#sign-in-risk).   


## <a name="evaluation"></a>Evaluatie 

U start een evaluatie door te klikken op **wat gebeurt er als**. Het resultaat van evaluatie van biedt u een rapport dat bestaat uit: 

![Wat als](./media/active-directory-conditional-access-whatif/03.png)

- Een indicator of klassieke beleid bestaat in uw omgeving
- Beleidsregels die voor uw gebruiker gelden
- Beleidsregels die niet van toepassing op uw gebruikers


Als [klassieke beleid](active-directory-conditional-access-migration.md#classic-policies) bestaat voor de geselecteerde cloud-apps, een indicator is aan u gepresenteerd. Door te klikken op de indicator, wordt u omgeleid naar de pagina klassieke beleidsregels. U kunt op de pagina beleid voor klassieke migreren van een klassieke beleid of alleen uitschakelen. U sluit deze pagina kunt u terugkeren naar het resultaat van evaluatie.

Op de lijst met beleidsregels die betrekking hebben op de geselecteerde gebruiker ook vindt u een lijst met [verlenen besturingselementen](active-directory-conditional-access-controls.md#grant-controls) en [sessie](active-directory-conditional-access-controls.md#session-controls) besturingselementen moet voldoen aan uw gebruikers.

In de lijst van beleidsregels die niet van toepassing op uw gebruikers, u kunt en ook vinden de redenen waarom dit beleid niet van toepassing. Voor elke vermelde beleid vertegenwoordigt de reden voor de eerste voorwaarde waaraan niet is voldaan. Een mogelijke reden voor een beleid dat niet is toegepast is een uitgeschakeld beleid, omdat ze nog niet zijn geëvalueerd.   



## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 

- Als u migreren van klassieke beleid wilt, raadpleegt u [migreren klassieke beleid in de Azure portal](active-directory-conditional-access-migration.md)  
