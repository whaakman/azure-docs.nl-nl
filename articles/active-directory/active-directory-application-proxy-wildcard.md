---
title: Toepassingen van jokertekens in de Azure Active Directory-toepassingsproxy | Microsoft Docs
description: Informatie over het gebruik van jokertekens toepassingen in de Azure Active Directory-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 61426d992e279cf88ae9750f0047b0cd81a797e3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="wildcard-applications-in-the-azure-active-directory-application-proxy"></a>Toepassingen van jokertekens in de Azure Active Directory-toepassingsproxy 

In Azure Active Directory (Azure AD), het configureren van een groot aantal lokale toepassingen kan al gauw onbeheerbaar en onnodige risico's voor configuratiefouten introduceert als veel van deze dezelfde instellingen vereist. Met [Azure AD-toepassingsproxy](manage-apps/application-proxy.md), u kunt dit probleem oplossen met behulp van jokertekens toepassingen publiceren publiceren en beheren van veel toepassingen tegelijkertijd. Dit is een oplossing waarmee u kunt:

-   Uw administratieve overhead vereenvoudigen
-   Verminder het aantal mogelijke fouten in de configuratie
-   Uw gebruikers veilig toegang geven tot meer bronnen

In dit artikel biedt u informatie die u nodig hebt voor het configureren van jokertekens toepassingen publiceren in uw omgeving.


## <a name="create-a-wildcard-application"></a>Een jokerteken-toepassing maken 

Als u een groep van toepassingen met dezelfde configuratie hebt, kunt u een jokerteken (*)-toepassing maken. Mogelijke kandidaten voor een toepassing jokertekens zijn toepassingen delen van de volgende instellingen:

- De groep gebruikers toegang hebben tot deze
- De methode eenmalige aanmelding
- De access protocol (http, https)

U kunt toepassingen met jokertekens publiceren als zowel de interne en externe URL's in de volgende indeling:

> HTTP (s) :/ / *. \<domein\>

Bijvoorbeeld: `http(s)://*.adventure-works.com`. De interne en externe URL's kunnen u verschillende domeinen gebruiken als een best practice, moeten ze niet hetzelfde zijn. Als de toepassing publiceert, ziet u een fout als een van de URL's niet een jokerteken.

Als u aanvullende toepassingen met andere configuratie-instellingen hebt, moet u deze uitzonderingen publiceren als afzonderlijke toepassingen om te overschrijven de standaardinstellingen voor het jokerteken. Toepassingen zonder een jokerteken altijd voorrang op toepassingen van jokertekens. Dit zijn 'net' reguliere toepassingen vanuit het perspectief configuratie.

Maken van een toepassing jokerteken is gebaseerd op dezelfde [toepassing publiceren stroom](manage-apps/application-proxy-publish-azure-portal.md) die beschikbaar is voor alle andere toepassingen. Het enige verschil is dat u een jokerteken in de URL's en mogelijk de SSO-configuratie opnemen.


## <a name="prerequisites"></a>Vereisten

### <a name="custom-domains"></a>Aangepaste domeinen

Terwijl [aangepaste domeinen](manage-apps/application-proxy-configure-custom-domain.md) zijn optioneel voor alle andere toepassingen, ze zijn vereist voor de jokerteken-toepassingen. Maken van aangepaste domeinen, moet u:

1. Maken van een geverifieerde domeinnaam in Azure 
2. Een SSL-certificaat in PFX-indeling uploaden naar de toepassingsproxy.

U moet een certificaat met jokertekens overeenkomen met de toepassing die u wilt maken. U kunt ook kunt u ook een certificaat met alleen een lijst met specifieke toepassingen gebruiken. In dit geval wordt zijn alleen de toepassingen die worden vermeld in het certificaat toegankelijk via deze toepassing jokerteken.

Dit is een vereiste vaste en jokertekens wordt niet ondersteund voor toepassingen die een aangepast domein niet voor de externe URL gebruiken uit veiligheidsoverwegingen.

### <a name="dns-updates"></a>DNS-updates

Wanneer u aangepaste domeinen, moet u een DNS-vermelding maken met een CNAME-record voor de externe URL (bijvoorbeeld `*.adventure-works.com`) die verwijst naar de externe URL van de toepassing proxy-eindpunt. De CNAME-record moet verwijzen naar de relevante externe URL's voor toepassingen van jokertekens:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

Om te bevestigen dat u uw CNAME juist hebt geconfigureerd, kunt u [nslookup](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) op een van de doeleindpunten, bijvoorbeeld `expenses.adventure-works.com`.  Uw antwoord bevatten de al genoemde alias (`<yourAADTenantId>.tenant.runtime.msappproxy.net`).


## <a name="considerations"></a>Overwegingen


### <a name="accepted-formats"></a>Geaccepteerde indelingen

Voor de jokerteken-toepassingen, de **interne URL** moet zijn geformatteerd als `http(s)://*.<domain>`. 

![AppId](./media/active-directory-application-proxy-wildcard\22.png)


Wanneer u configureert een **externe URL**, moet u de volgende indeling: `https://*.<custom domain>` 

![AppId](./media/active-directory-application-proxy-wildcard\21.png)

Andere functies van het jokerteken, meerdere jokertekens of andere regex-tekenreeksen worden niet ondersteund en fouten veroorzaken.


### <a name="excluding-applications-from-the-wildcard"></a>Het jokerteken uitsluiten van toepassingen

U kunt een toepassing uitsluiten van de toepassing jokertekens door

- De toepassing uitzondering als reguliere toepassing publiceren 
- Het jokerteken alleen voor specifieke toepassingen via de DNS-instellingen inschakelen  


Een toepassing als reguliere toepassing publiceren is de voorkeursmethode voor het uitsluiten van een toepassing van een jokerteken. Publiceer de uitgesloten toepassingen voordat de jokerteken-toepassingen om ervoor te zorgen dat uw uitzonderingen vanaf het begin worden afgedwongen. De meest specifiek toepassing hebben altijd voorrang – een toepassing die is gepubliceerd als `budgets.finance.adventure-works.com` heeft voorrang op de toepassing `*.finance.adventure-works.com`, die op zijn beurt voorrang op de toepassing `*.adventure-works.com`. 

U kunt het jokerteken alleen werkt voor specifieke toepassingen via uw DNS-beheer beperken. Als een best practice moet u een CNAME-vermelding bestaat uit een jokerteken en komt overeen met de indeling van de externe URL die u hebt geconfigureerd. U kunt specifieke URL's echter in plaats daarvan verwijzen naar de jokertekens. Bijvoorbeeld, in plaats van `*.adventure-works.com`, wijst u `hr.adventure-works.com`, `expenses.adventure-works.com` en `travel.adventure-works.com individually` naar `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`. 

Als u deze optie gebruikt, moet u ook een andere CNAME-vermelding voor de waarde `AppId.domain`, bijvoorbeeld `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`, ook aan te wijzen naar dezelfde locatie. U vindt de **AppId** op de eigenschappenpagina van de toepassing van de toepassing jokertekens:

![AppId](./media/active-directory-application-proxy-wildcard\01.png)



### <a name="setting-the-homepage-url-for-the-myapps-panel"></a>Instellen van de URL van de startpagina voor het paneel MyApps

De jokerteken-toepassing wordt weergegeven met slechts één tegel in de [MyApps Configuratiescherm](https://myapps.microsoft.com). Deze tegel is standaard verborgen. De tegel weergeven en hebben gebruikers land op een bepaalde pagina:

1. Volg de richtlijnen voor [instellen van een URL startpagina](application-proxy-office365-app-launcher.md).
2. Stel **toepassing weergeven** naar **waar** op de eigenschappenpagina van de toepassing.

### <a name="kerberos-constrained-delegation"></a>Kerberos-beperkte overdracht

Voor toepassingen met [kerberos-beperkte delegatie (KCD) als de methode eenmalige aanmelding](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md), de SPN voor de methode eenmalige aanmelding een jokerteken ook moet mogelijk vermeld. De SPN-naam kan bijvoorbeeld: `HTTP/*.adventure-works.com`. U moet nog steeds de afzonderlijke SPN's geconfigureerd op uw back-endservers hebben (bijvoorbeeld `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`).



## <a name="scenario-1-general-wildcard-application"></a>Scenario 1: Algemene jokertekens toepassing

In dit scenario hebt u drie verschillende toepassingen die u wilt publiceren:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

Alle drie de toepassingen:

- Worden gebruikt door alle gebruikers
- Gebruik *geïntegreerde Windows-verificatie* 
- Dezelfde eigenschappen


U kunt het jokerteken toepassingen publiceren met de stappen in [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md). Dit scenario wordt ervan uitgegaan dat:

- Een tenant met de volgende ID: `000aa000-11b1-2ccc-d333-4444eee4444e` 

- Een geverifieerde domeinnaam aangeroepen `adventure-works.com` is geconfigureerd.

- Een **CNAME** vermelding die wijst `*.adventure-works.com` naar `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net` is gemaakt.

Na de [beschreven stappen](manage-apps/application-proxy-publish-azure-portal.md), u een nieuwe toepassing proxy-toepassing maken in uw tenant. In dit voorbeeld is het jokerteken in de volgende velden:

- Interne URL:

    ![Interne URL](./media/active-directory-application-proxy-wildcard\42.png)


- Externe URL:

    ![Externe URL](./media/active-directory-application-proxy-wildcard\43.png)

 
- Interne toepassing SPN: 

    ![SPN-configuratie](./media/active-directory-application-proxy-wildcard\44.png)


Door de jokerteken-toepassing publiceert, u kunt nu toegang tot uw drie toepassingen door te navigeren naar de URL's die u gebruikt voor zijn (bijvoorbeeld `travel.adventure-works.com`).

De configuratie implementeert de volgende structuur:

![AppId](./media/active-directory-application-proxy-wildcard\05.png)

| Kleur | Beschrijving |
| ---   | ---         |
| Blauw  | Toepassingen expliciet gepubliceerd en zichtbaar in de Azure-Portal. |
| Grijs  | Toepassingen kunt u toegankelijk zijn via de bovenliggende toepassing. |




## <a name="scenario-2-general-wildcard-application-with-exception"></a>Scenario 2: Algemene jokertekens toepassing met de uitzondering

In dit scenario hebt u bovendien voor de drie algemene toepassingen een andere toepassing `finance.adventure-works.com`, die alleen de afdeling Financiën toegankelijk moeten zijn. Met de huidige toepassingsstructuur, zou uw toepassing Financiën toegankelijk via de jokerteken-toepassing en door alle werknemers zijn. Om dit te wijzigen, uitsluiten u uw toepassing uit uw jokertekens door Financiën configureren als een afzonderlijke toepassing met meer beperkte machtigingen.



U moet ervoor zorgen dat een CNAME-records bestaan die verwijst `finance.adventure-works.com` aan de specifieke eindpunt van de toepassing, op de pagina Application Proxy voor de toepassing opgegeven. In dit scenario `finance.adventure-works.com` verwijst naar `https://finance-awcycles.msappproxy.net/`. 

Na de [beschreven stappen](manage-apps/application-proxy-publish-azure-portal.md), in dit scenario moeten de volgende instellingen:


- In de **interne URL**, u stelt **financiën** in plaats van een jokerteken. 

    ![Interne URL](./media/active-directory-application-proxy-wildcard\52.png)

- In de **externe URL**, u stelt **financiën** in plaats van een jokerteken. 

    ![Externe URL](./media/active-directory-application-proxy-wildcard\53.png)

- Interne toepassing SPN instellen **financiën** in plaats van een jokerteken.

    ![SPN-configuratie](./media/active-directory-application-proxy-wildcard\54.png)


Deze configuratie implementeert het volgende scenario:

![Scenario](./media/active-directory-application-proxy-wildcard\09.png)

Omdat `finance.adventure-works.com` is een meer specifiek URL dan `*.adventure-works.com`, heeft deze prioriteit. Gebruikers navigeren naar `finance.adventure-works.com` de ervaring die is opgegeven in de toepassing Financiën Resources hebben. In dit geval wordt alleen medewerkers van Financiën hebben toegang tot `finance.adventure-works.com`.

Als u meerdere toepassingen die worden gepubliceerd voor Financiën hebt en u hebt `finance.adventure-works.com` als een geverifieerde domeinnaam kan u een andere jokertekens toepassing publiceren `*.finance.adventure-works.com`. Omdat dit meer dan de algemene specifieke is `*.adventure-works.com`, heeft deze prioriteit als een gebruiker naar een toepassing in het domein van Financiën.


## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over:

- **Aangepaste domeinen**, Zie [werken met aangepaste domeinen in Azure AD-toepassingsproxy](manage-apps/application-proxy-configure-custom-domain.md).

- **Toepassingen publiceren**, Zie [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md)


