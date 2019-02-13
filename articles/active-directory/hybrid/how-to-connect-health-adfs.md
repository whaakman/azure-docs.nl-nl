---
title: Azure AD Connect Health gebruiken met AD FS | Microsoft Docs
description: Op deze Azure AD Connect Health-pagina vindt u informatie over het bewaken van uw on-premises AD FS-infrastructuur.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4714a4ee5f90194a3be47b1e5878dfa8e230f42b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173484"
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>AD FS bewaken met Azure AD Connect Health
De volgende documentatie is specifiek voor het bewaken van uw Azure AD FS-infrastructuur met Azure AD Connect Health. Zie [Using Azure AD Connect Health for Sync](how-to-connect-health-sync.md) (Engelstalig) voor informatie over het bewaken van Azure AD Connect (synchronisatie) met Azure AD Connect Health. Zie ook [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md) voor informatie over het bewaken van Active Directory Domain Services met Azure AD Connect Health.

## <a name="alerts-for-ad-fs"></a>Waarschuwingen voor AD FS
In de sectie Waarschuwingen van Azure AD Connect Health vindt u een lijst met actieve waarschuwingen. Elke waarschuwing omvat relevante informatie, stappen voor het oplossen van het probleem en koppelingen naar gerelateerde documentatie.

U kunt dubbelklikken op een actieve of opgeloste waarschuwing om een nieuwe blade te openen met aanvullende informatie, stappen die u kunt nemen om de waarschuwing op te lossen en koppelingen naar relevante documentatie. U kunt ook u historische gegevens bekijken voor waarschuwingen die in het verleden zijn opgelost.

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Gebruiksanalyses voor AD FS
Gebruiksanalyse Azure AD Connect Health analyseert het verificatieverkeer van uw federatieve servers. U kunt dubbelklikken op het vakje voor gebruiksanalyses om een blade met gebruiksanalyses te openen waarin u bepaalde gegevens en groeperingen kunt zien.

> [!NOTE]
> Om Gebruiksanalyses met AD FS te gebruiken, moet u AD FS-controle hebben ingeschakeld. Ga voor meer informatie naar [Controles voor AD FS inschakelen](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report1.png)

Om aanvullende gegevens te selecteren geeft u een tijdsperiode op, of klikt u met de rechtermuisknop op de grafiek met gebruiksanalyses en selecteert u Grafiek bewerken om de groepering te wijzigen. Vervolgens kunt u de tijdsperiode opgeven, andere gegevens selecteren en de groepering wijzigen. U kunt de distributie van het verificatieverkeer op basis van verschillende metrische gegevens weergeven en alle gegevens groeperen met behulp van de relevante 'groeperen op'-parameters die in het volgende gedeelte worden beschreven:

**Metriek: totaal aantal verzoeken**: het totale aantal verzoeken dat door AD FS-servers is verwerkt.

|Groeperen op | Wat houdt de groepering in en waarom heeft dit zin? |
| --- | --- |
| Alle | Geeft het berekende totale aantal verzoeken weer dat door alle AD FS-servers is verwerkt.|
| Toepassing | Hiermee worden alle verzoeken gegroepeerd op basis van de beoogde Relying Party. Dankzij deze groep krijgt u meer inzicht in de percentages van al het verkeer die worden verwerkt per toepassing. |
|  Server |Hiermee worden alle verzoeken gegroepeerd op basis van de server die het verzoek heeft verwerkt. Met deze groepering kunt u zien hoe de belasting van al het verkeer is verdeeld.
| Werkplek koppelen |Hiermee worden alle verzoeken gegroepeerd op verzoeken die worden verzonden door apparaten die aan de werkplek gekoppeld (bekend) zijn of niet. Deze groepering is nuttig wanneer uw resources worden geopend met behulp van apparaten die niet bekend zijn in uw infrastructuur voor identiteiten. |
|  Verificatiemethode | Hiermee worden alle verzoeken gegroepeerd op basis van de gebruikte verificatiemethode. Deze groepering biedt meer inzicht in de veelgebruikte verificatiemethoden. Hier volgen enkele mogelijke verificatiemethoden <ol> <li>Geïntegreerde Windows-verificatie (Windows)</li> <li>Op formulieren gebaseerde verificatie (formulieren)</li> <li>Optie voor eenmalige aanmelding (SSO)</li> <li>Op X509-certificaat gebaseerde verificatie (certificaat)</li> <br>Als een federatieve service het verzoek ontvangt met een SSO-cookie, wordt dit verzoek beschouwd als een eenmalige aanmelding (SSO). In zo’n geval, wanneer het cookie geldig is, wordt de gebruiker niet gevraagd om inloggegevens en krijgt deze naadloos toegang tot de toepassing. Dit gebeurt vaak wanneer u meerdere Relying Party’s hebt die worden beschermd door de federatieve servers. |
| Netwerklocatie | Hiermee worden alle verzoeken gegroepeerd op basis van de netwerklocatie van de gebruiker. Dit kan intranet of extranet zijn. Met deze groepering kunt u zien welk percentage van het verkeer van het intranet komt, en welk percentage van het extranet. |


**Metriek: totaal aantal mislukte verzoeken**: het totale aantal mislukte verzoeken dat door de federatieve service is verwerkt. (Deze gegevens zijn alleen beschikbaar op AD FS voor Windows Server 2012 R2)

|Groeperen op | Wat houdt de groepering in en waarom heeft dit zin? |
| --- | --- |
| Fouttype | Hiermee wordt het aantal fouten op basis van vooraf gedefinieerde fouttypen weergegeven. Deze groepering is nuttig om meer inzicht te verkrijgen in de veelvoorkomende fouttypen. <ul><li>Gebruikersnaam of wachtwoord onjuist: fouten omdat de gebruikersnaam of het wachtwoord onjuist is.</li> <li>'Vergrendeling van het extranet': fouten omdat het verzoek werd ontvangen van een gebruiker die is uitgesloten van extranet </li><li> 'Wachtwoord verlopen': mislukt omdat de gebruiker zich heeft aangemeld met een verlopen wachtwoord.</li><li>'Account uitgeschakeld': mislukt omdat de gebruiker zich heeft aangemeld met een uitgeschakeld account.</li><li>'Apparaatverificatie': mislukt omdat de gebruiker niet kan worden geverifieerd met behulp van Apparaatverificatie.</li><li>'Verificatie met gebruikerscertificaat': mislukt omdat de gebruiker niet kan worden geverifieerd vanwege een ongeldig certificaat.</li><li>'MFA': mislukt omdat de gebruiker niet kan worden geverifieerd met behulp van Multi-Factor Authentication.</li><li>'Andere referentie': 'Uitgifte-autorisatie': mislukt vanwege autorisatiefouten.</li><li>'Overgedragen uitgifte': mislukt wegens fouten bij het delegeren van de uitgifte.</li><li>'Acceptatie token': mislukt omdat ADFS het token van een externe id-provider weigert.</li><li>'Protocol': mislukt wegens protocolfouten.</li><li>'Onbekend': Algemene melding. Andere fouten die niet in de gedefinieerde categorieën vallen.</li> |
| Server | Hiermee worden fouten gegroepeerd op basis van de server. Dit is handig om te begrijpen hoe de fouten zijn verdeeld over de servers. Een ongelijke verdeling kan wijzen op een server die in slechte staat is. |
| Netwerklocatie | Hiermee worden fouten gegroepeerd op basis van de netwerklocatie van de verzoeken (intranet vs. extranet). Zo kunt u beter zien wat voor type verzoeken het vaakst mislukt. |
|  Toepassing | Hiermee worden fouten gegroepeerd op basis van de beoogde applicatie (Relying Party). Dit is handig om te begrijpen bij welke doeltoepassing de meeste fouten voorkomen. |

**Metriek: gebruikersaantal**: het gemiddelde aantal unieke gebruikers die actief verifiëren met AD FS

|Groeperen op | Wat houdt de groepering in en waarom heeft dit zin? |
| --- | --- |
|Alle |Hiermee ziet u het gemiddelde aantal gebruikers die gebruikmaken van de federatieve service in het geselecteerde tijdvak. De gebruikers zijn niet gegroepeerd. <br>Het gemiddelde is afhankelijk van de geselecteerde tijdsperiode. |
| Toepassing |Hiermee wordt het gemiddelde gebruikersaantal gegroepeerd op basis van de beoogde applicatie (Relying Party). Zo kunt u zien hoeveel gebruikers gebruikmaken van elke toepassing. |

## <a name="performance-monitoring-for-ad-fs"></a>Prestatiebewaking voor AD FS
Prestatiebewaking voor Azure AD Connect Health biedt bewakingsinformatie over de gegevens. Door het vakje Bewaking te selecteren, wordt een blade geopend met gedetailleerde informatie over de gegevens.

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/perf1.png)

Door de filteroptie boven aan de blade te selecteren, kunt u filteren op server om de gegevens van een individuele server te bekijken. Als u de gegevens wilt bewerken, klikt u op de bewakingsgrafiek onder de bewakingsblade en selecteert u vervolgens Grafiek bewerken (of klikt u op de knop Grafiek bewerken). In de nieuwe blade die wordt geopend, kunt u nu aanvullende gegevens selecteren uit het vervolgkeuzemenu en een tijdsperiode opgeven om de prestatiegegevens te bekijken.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>Top 50 van gebruikers met mislukte aanmeldingen vanwege een verkeerde gebruikersnaam of verkeerd wachtwoord
Een veelvoorkomende reden voor een mislukt verificatieverzoek op een AD FS-server is een verzoek met ongeldige referenties, dat wil zeggen, een onjuiste gebruikersnaam of wachtwoord. Dit gebeurt meestal vanwege complexe wachtwoorden, vergeten wachtwoorden of typfouten.

Er zijn echter andere redenen die kunnen leiden tot een onverwacht aantal verzoeken die moeten worden verwerkt door uw AD FS-servers: Een toepassing die de inloggegevens van een gebruiker opslaat in de cache waarbij de gegevens vervolgens verlopen, of een kwaadwillende gebruiker die probeert in te loggen op een account door middel van een reeks bekende wachtwoorden. De volgende twee voorbeelden zijn geldige oorzaken die tot een piek in aanvragen kunnen leiden.

Azure AD Connect Health voor AD FS biedt een rapport over top 50 van gebruikers met mislukte inlogpogingen vanwege een ongeldige gebruikersnaam of wachtwoord. Dit rapport komt tot stand door het verwerken van de controlegebeurtenissen die worden gegenereerd door de AD FS-servers in de farms.

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report1a.png)

Met dit rapport hebt u een eenvoudig overzicht van de volgende informatie:

* Totaal aantal mislukte verzoeken met een onjuiste gebruikersnaam en wachtwoord in de afgelopen 30 dagen
* Gemiddeld aantal gebruikers per dag die zich proberen aan te melden met een onjuiste gebruikersnaam en wachtwoord.

Door hier te klikken gaat u naar de blade met het hoofdrapport waar u meer informatie kunt vinden. Deze blade bevat een grafiek met trendinformatie waarmee u een basis kunt vastleggen voor aanvragen met een verkeerde gebruikersnaam of een verkeerd wachtwoord. Daarnaast biedt deze blade een lijst met vijftig gebruikers die de afgelopen week het meest last hadden van mislukte pogingen. Deze vijftig gebruikers van de afgelopen week kunnen helpen bij het identificeren van pieken in onjuiste wachtwoorden.  

Deze grafiek bevat de volgende informatie:

* Het totale aantal mislukte aanmeldingen vanwege een ongeldige gebruikersnaam en wachtwoord per dag.
* Het totale aantal unieke gebruikers met mislukte aanmeldingen per dag.
* Client IP-adres van de laatste aanvraag

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report3a.png)

Dit rapport bevat de volgende informatie:

| Rapportitem | Description |
| --- | --- |
| Gebruikers-ID |De gebruikers-ID die is gebruikt. Deze waarde is wat de gebruiker heeft getypt, wat in sommige gevallen de verkeerder gebruikers-id is. |
| Mislukte pogingen |Geeft het totale aantal mislukte pogingen voor die specifieke gebruikers-ID weer. De tabel is gesorteerd in aflopende volgorde, met het hoogste aantal mislukte pogingen bovenaan. |
| Laatste fout |Dit is het tijdstip waarop de laatste fout zich heeft voorgedaan. |
| IP laatste fout |Toont het client-IP-adres uit de laatste ongeldige aanvraag. Als er meer dan één IP-adressen in deze waarde zijn, bevat deze mogelijk ook een IP om de client door te verbinden, samen met de laatste poging van de gebruiker om een IP aan te vragen.  |

> [!NOTE]
> Dit rapport wordt elke 12 uur automatisch bijgewerkt met de nieuwe informatie die in die tijd is verzameld. Als gevolg hiervan kunnen inlogpogingen van de laatste 12 uur niet worden opgenomen in het rapport.
>
>

## <a name="risky-ip-report-public-preview"></a>Rapport van riskante IP (openbare preview)
AD FS-klanten kunnen eindpunten voor wachtwoordverificatie op internet beschikbaar maken om verificatieservices te bieden aan eindgebruikers zodat ze toegang hebben tot SaaS-toepassingen zoals Office 365. In dat geval kunnen criminelen aanmeldpogingen uitvoeren op uw AD FS-systeem om het wachtwoord van een eindgebruiker te achterhalen en toegang te krijgen tot toepassingsresources. AD FS biedt de extranetfunctionaliteit voor accountvergrendeling om dergelijke aanvallen te voorkomen vanaf AD FS in Windows Server 2012 R2. Als u een lagere versie gebruikt, raden we u ten zeerste aan uw AD FS-systeem naar Windows Server 2016 te upgraden. <br />
Daarnaast is het mogelijk dat vanaf één IP-adres meerdere aanmeldpogingen voor meerdere gebruikers worden uitgevoerd. In dit soort gevallen is het aantal pogingen per gebruiker mogelijk lager dan de drempel voor beveiliging met accountvergrendeling in AD FS. Azure AD Connect Health biedt nu het rapport Riskant IP-adres dat deze gebeurtenis detecteert en beheerders hierover waarschuwt. De belangrijkste voordelen van dit rapport zijn: 
- Detectie van IP-adressen die een drempel voor mislukte aanmeldingen op basis van wachtwoord overschrijden
- Ondersteuning voor mislukte aanmeldingen vanwege een onjuist wachtwoord of een vergrendelde status in extranet
- E-mailmeldingen voor beheerders zodra dit gebeurt, met aanpasbare e-mailinstellingen
- Aanpasbare drempelwaarde voor afstemming op het beveiligingsbeleid van een organisatie
- Downloadbare rapporten voor offline analyse en integratie met andere systemen via automatisering

> [!NOTE]
> Om dit rapport te gebruiken, moet u controles voor AD FS hebben ingeschakeld. Ga voor meer informatie naar [Controles voor AD FS inschakelen](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Voor toegang tot de preview-versie is een machtiging als Globale beheerder of [Beveiligingslezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) vereist.  
> 

### <a name="what-is-in-the-report"></a>Inhoud van het rapport
Elk item in het rapport Riskant IP-adres toont verzamelde informatie over mislukte AD FS-aanmeldactiviteiten die de aangewezen drempelwaarde overschrijden. Het rapport bevat de volgende informatie: ![Portal voor Azure Active Directory Connect Health](./media/how-to-connect-health-adfs/report4a.png)

| Rapportitem | Description |
| ------- | ----------- |
| Tijdstempel | Geeft het tijdstempel weer op basis van de lokale tijd in Azure Portal wanneer de detectieperiode start.<br /> Alle gebeurtenissen per dag worden gegenereerd om 24:00 uur UTC-tijd. <br />Bij gebeurtenissen per uur is het tijdstempel afgerond naar het begin van het uur. U vindt de begintijd van de eerste activiteit vanaf firstAuditTimestamp in het geëxporteerde bestand. |
| Triggertype | Geeft het type tijdvenster voor detectie weer. De triggertypen voor aggregatie zijn per uur of per dag. Dit is handig voor het detecteren van een beveiligingsaanval met hoge frequentie en een trage beveiligingsaanval waarbij het aantal pogingen over de dag is verdeeld. |
| IP-adres | Het specifieke riskante IP-adres voor activiteiten met betrekking tot onjuiste wachtwoorden of extranetvergrendeling. Dit kan een IPv4- of een IPv6-adres zijn. |
| Aantal mislukte wachtwoordpogingen | Het aantal mislukte wachtwoordpogingen vanaf het IP-adres tijdens de detectieperiode. Mislukte wachtwoordpogingen kunnen meerdere keren plaatsvinden bij bepaalde gebruikers. Dit omvat geen mislukte pogingen vanwege verlopen wachtwoorden. |
| Aantal mislukte extranetvergrendelingen | Het aantal mislukte extranetvergrendelingen vanaf het IP-adres tijdens de detectieperiode. Mislukte extranetvergrendelingen kunnen meerdere keren plaatsvinden bij bepaalde gebruikers. Dit is alleen zichtbaar als Vergrendeling van het extranet is geconfigureerd in AD FS (versies 2012R2 of hoger). <b>Opmerking</b>: we raden u aan deze functie in te schakelen als u extranetaanmeldingen met wachtwoorden toestaat. |
| Pogingen unieke gebruikersnaam | Het aantal mislukte pogingen met unieke gebruikersnaam vanaf het IP-adres tijdens de detectieperiode. Dit biedt een mechanisme om een aanvalspatroon met een enkele gebruiker van een aanvalspatroon met meerdere gebruikers te onderscheiden.  |

Het onderstaande rapportitem geeft bijvoorbeeld aan dat in het tijdvenster van 18:00 tot 19:00 uur op 28-02-2018 het IP-adres <i>104.2XX.2XX.9</i> geen mislukte wachtwoordpogingen en 284 mislukte extranetvergrendelingen had. 14 unieke gebruikers zijn volgens de criteria beïnvloed. De activiteitsgebeurtenis overschrijdt de aangewezen drempelwaarde per uur van het rapport. 

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report4b.png)

> [!NOTE]
> - Alleen activiteiten die de aangewezen drempelwaarde overschrijden, worden in de rapportlijst weergegeven. 
> - Dit rapport kan tot 30 dagen terug worden bekeken.
> - Dit waarschuwingsrapport geeft geen Exchange-IP-adressen of privé-IP-adressen weer. Ze worden wel opgenomen in de exportlijst. 
>

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report4c.png)

### <a name="load-balancer-ip-addresses-in-the-list"></a>IP-adressen van load balancer in de lijst
Load balancer verzamelt mislukte aanmeldactiviteiten en bereikt de drempelwaarde voor waarschuwingen. Als u load balancer IP-adressen ziet, is het zeer waarschijnlijk dat uw externe load balancer het client-IP-adres niet verzendt wanneer deze de aanvraag naar de proxyserver van de webtoepassing doorstuurt. Configureer de load balancer op een juiste manier om het client-IP-adres door te schakelen. 

### <a name="download-risky-ip-report"></a>Rapport Riskant IP-adres downloaden 
Met behulp van de functie **Downloaden** kan de volledige lijst met riskante IP-adressen van de afgelopen 30 dagen worden geëxporteerd vanuit de Connect Health-portal. De export bevat alle mislukte AD FS-aanmeldactiviteiten in elke detectieperiode, zodat u de filters na het exporteren kunt aanpassen. De export geeft naast de gemarkeerde aggregaties in de portal ook meer informatie over mislukte aanmeldactiviteiten per IP-adres weer:

|  Rapportitem  |  Description  | 
| ------- | ----------- | 
| firstAuditTimestamp | Geeft de eerste tijdstempel weer voor de start van de mislukte activiteiten tijdens de detectieperiode.  | 
| lastAuditTimestamp | Geeft de laatste tijdstempel weer voor het einde van de mislukte activiteiten tijdens de detectieperiode.  | 
| attemptCountThresholdIsExceeded | De markering voor wanneer de huidige activiteiten de drempelwaarde voor waarschuwingen overschrijden.  | 
| isWhitelistedIpAddress | De markering voor wanneer het IP-adres is uitgesloten voor waarschuwingen en rapporten. Privé-IP-adressen (<i>10.x.x.x, 172.x.x.x en 192.168.x.x</i>) en Exchange-IP-adressen worden eruit gefilterd en als waar gemarkeerd. Als u privé-IP-adresbereiken ziet, is het zeer waarschijnlijk dat de externe load balancer het client-IP-adres niet verzendt wanneer deze de aanvraag naar de proxyserver van de webtoepassing doorstuurt.  | 

### <a name="configure-notification-settings"></a>Meldingsinstellingen configureren
Beheerders van het rapport kunnen met de **Meldingsinstellingen** op de hoogte worden gehouden. De e-mailmelding voor riskante IP-adressen is standaard uitgeschakeld. U kunt de melding inschakelen door de knop onder 'E-mailmeldingen ontvangen voor een rapport van IP-adressen waarvoor de drempelwaarden voor mislukte aanmeldingen zijn overschreden' in te schakelen. Net als in de algemene meldingsinstellingen in Connect Health kunt u hier de ontvangerslijst voor Riskant IP-adres-rapporten aanpassen. U kunt ook alle globale beheerders informeren bij het maken van de wijziging. 

### <a name="configure-threshold-settings"></a>Drempelwaarde-instellingen configureren
De drempelwaarde voor waarschuwingen kan via de drempelwaarde-instellingen worden bijgewerkt. De drempelwaarde is in het begin standaard ingesteld in het systeem. Er zijn vier categorieën in de drempelwaarde-instellingen voor Riskant IP-adres-rapporten:

![Portal voor Azure AD Connect Health](./media/how-to-connect-health-adfs/report4d.png)

| Drempelwaarde-item | Description |
| --- | --- |
| (Slechte U/P + vergrendeling van het extranet) / dag  | Ingestelde drempelwaarde per **dag** voor het rapporteren van de activiteit en het activeren van een melding wanneer het aantal onjuiste wachtwoordpogingen plus het aantal extranetvergrendelingen is overschreden. |
| (Slechte U/P + vergrendeling van het extranet) / uur | Ingestelde drempelwaarde per **uur** voor het rapporteren van de activiteit en het activeren van een melding wanneer het aantal onjuiste wachtwoordpogingen plus het aantal extranetvergrendelingen is overschreden. |
| Vergrendeling van het extranet / dag | Ingestelde drempelwaarde per **dag** voor het rapporteren van de activiteit en het activeren van een melding wanneer het aantal extranetvergrendelingen is overschreden. |
| Vergrendeling van het extranet / uur| Ingestelde drempelwaarde per **uur** voor het rapporteren van de activiteit en het activeren van een melding wanneer het aantal extranetvergrendelingen is overschreden. |

> [!NOTE]
> - Een wijziging van de drempelwaarde van het rapport wordt na een uur toegepast. 
> - Bestaande gemelde items worden niet beïnvloed door het wijzigen van de drempelwaarde. 
> - We raden u aan om het aantal gebeurtenissen binnen uw omgeving te analyseren en de drempelwaarde aan de hand hiervan bij te stellen. 
>
>

### <a name="faq"></a>Veelgestelde vragen
1. Waarom zie ik privé-IP-adresbereiken in het rapport?  <br />
Privé-IP-adressen (<i>10.x.x.x, 172.x.x.x en 192.168.x.x</i>) en de Exchange-IP-adressen worden eruit gefilterd en als waar gemarkeerd in de lijst met toegestane IP-adressen. Als u privé-IP-adresbereiken ziet, is het zeer waarschijnlijk dat de externe load balancer het client-IP-adres niet verzendt wanneer deze de aanvraag naar de proxyserver van de webtoepassing doorstuurt.

2. Waarom krijg ik load balancer IP-adressen in het rapport te zien?  <br />
Als u load balancer IP-adressen ziet, is het zeer waarschijnlijk dat uw externe load balancer het client-IP-adres niet verzendt wanneer deze de aanvraag naar de proxyserver van de webtoepassing doorstuurt. Configureer de load balancer op een juiste manier om het client-IP-adres door te schakelen. 

3. Hoe kan ik een IP-adres blokkeren?  <br />
Voeg geïdentificeerde schadelijke IP-adressen toe aan de firewall of blokkeer deze in Exchange.   <br />

4. Waarom zie ik geen items in dit rapport? <br />
   - Het aantal mislukte aanmeldactiviteiten is lager dan de ingestelde drempelwaarde. 
   - Zorg ervoor dat er geen melding als 'Health-service is niet up-to-date' actief is in de AD FS-serverlijst.  Lees meer informatie over [het oplossen van deze waarschuwing](how-to-connect-health-data-freshness.md).
   - Controles zijn niet ingeschakeld in AD FS-farms.
 
5. Waarom krijg ik geen toegang tot het rapport?  <br />
Voor toegang is een machtiging als Globale beheerder of [Beveiligingslezer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) vereist. Neem contact op met uw globale beheerder om toegang te krijgen.


## <a name="related-links"></a>Verwante koppelingen
* [Azure AD Connect Health (Engelstalig)](whatis-hybrid-identity-health.md)
* [De Azure AD Connect Health-agent installeren](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health Operations](how-to-connect-health-operations.md) (Azure AD Connect Health-bewerkingen)
* [Azure AD Connect Health for Sync gebruiken](how-to-connect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](reference-connect-health-version-history.md)
