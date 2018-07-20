---
title: Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Naslaginformatie over foutcodes voor aanmeldactiviteitenrapporten.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: aab03c92bee0d3b69062cdcb179eebbb5c0fc8f8
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160243"
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal

In de informatie die wordt aangeboden in het rapport over aanmeldactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

- Wie heeft zich aangemeld met Azure Active Directory?
- Bij welke apps hebben gebruikers zich aangemeld?
- Welke aanmeldingen zijn mislukt en waarom?

In dit artikel worden de foutcodes en de bijbehorende beschrijvingen genoemd. 

## <a name="how-can-i-display-failed-sign-ins"></a>Hoe kan ik mislukte aanmeldingen weergeven? 

Uw startpunt voor alle aanmeldingsactiviteitgegevens is **[Aanmeldingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** in het gedeelte **Activiteit** van **Azure Active**.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Aanmeldingsactiviteit")

In het aanmeldingenrapport kunt u alle mislukte aanmeldingen weergeven door **Mislukt** te selecteren als **Aanmeldstatus**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Aanmeldingsactiviteit")

Wanneer u op een item in de weergegeven lijst klikt, wordt de blade **Activiteitendetails: aanmeldingen** geopend. Deze weergave biedt u alle details die Azure Active Directory bijhoudt voor aanmeldingen, met inbegrip van de **Foutcode voor aanmelding** en een **Reden van fout**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Aanmeldingsactiviteit")


In plaats van Azure Portal kunt u ook de [rapportage-API](active-directory-reporting-api-getting-started-azure-portal.md) gebruiken voor toegang tot aanmeldingsgegevens.


De volgende sectie biedt een volledig overzicht van alle mogelijke fouten en de bijbehorende beschrijvingen. 

## <a name="error-codes"></a>Foutcodes


|Fout|Beschrijving|
|---|---|
|16000|Dit is een interne implementatiedetail en niet een fout optreedt. U kunt deze referentie negeren.|
|20001|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|20012|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|20033|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40008|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40009|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40014|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|50000|Er is een probleem met onze aanmeldingsservice. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) om dit probleem op te lossen.|
|50001|De Service Principal Name is niet gevonden in deze tenant. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant. Of de resource-principal is niet gevonden in de map of is ongeldig.|
|50002|Aanmelden is mislukt vanwege beperkte proxytoegang op de tenant. Als dit uw eigen tenantbeleid is, kunt u uw beperkingsinstellingen voor tenants wijzigen om dit probleem te verhelpen|
|50003|Aanmelden is mislukt vanwege een ontbrekende handtekeningsleutel of ontbrekend handtekeningcertificaat. Dit kan komen doordat er geen handtekeningsleutel is geconfigureerd in de toepassing. Bekijk de oplossingen die worden beschreven in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Als u nog steeds problemen hebt, neemt u contact op met de eigenaar van de toepassing of de beheerder van de toepassing|
|50005|Gebruiker heeft geprobeerd zich aan te melden bij een apparaat vanaf een platform dat momenteel niet wordt ondersteund wegens beleid voor voorwaardelijke toegang|
|50006| Handtekeningverificatie is mislukt vanwege ongeldige handtekening. Bekijk de oplossing die wordt beschreven in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery). Als u nog steeds problemen hebt, neemt u contact op met de eigenaar van de toepassing of de beheerder van de app|
|50007|Het partnerversleutelingscertificaat is niet gevonden voor deze toepassing. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) bij Microsoft om dit te verhelpen|
|50008|SAML-verklaring ontbreekt of is niet juist geconfigureerd in het token. Neem contact op met uw federatieprovider.|
|50010|Validatie van de doelgroep-URI voor de toepassing is mislukt, omdat er geen tokendoelgroepen zijn geconfigureerd. Neem contact op met de eigenaar van de toepassing|
|50011|Het antwoordadres ontbreekt, is onjuist geconfigureerd of komt niet overeen met de antwoordadressen die voor de toepassing zijn geconfigureerd. Probeer de oplossing die wordt vermeld in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Als u nog steeds problemen hebt, neemt u contact op met de eigenaar van de toepassing of de beheerder van de app|
|50012| Dit is een algemene foutmelding die aangeeft dat de verificatie is mislukt. Dit kan gebeuren om redenen als de ontbrekende of ongeldige referenties of claims in de aanvraag. Zorg ervoor dat de aanvraag is verzonden met de juiste referenties en claims. |
|50013|Verklaring is ongeldig vanwege verschillende oorzaken: de uitgever van het token komt niet overeen met de api-versie binnen het geldige bereik - verlopen - onjuist gevormd - Vernieuwingstoken in de verklaring is niet een primair vernieuwingstoken.|
|50017|Validatie van certificaat is mislukt om de volgende redenen:<ul><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Verwacht CrlSegment kan niet worden gevonden</li><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Delta CRL-distributiepunt is geconfigureerd zonder een bijbehorend CRL-distributiepunt</li><li>Kan geen geldige CRL-segmenten ophalen als gevolg van time-outprobleem</li><li>Kan CRL niet downloaden</li></ul>Neem contact op met de beheerder van de tenant.|
|50020|De gebruiker is niet gemachtigd voor een van de volgende oorzaken hebben.<ul><li>De gebruiker zich probeert aan te melden met een MSA-account met de v1-eindpunt</li><li>De gebruiker bestaat niet in de tenant.</li></ul> Neem contact op met de eigenaar van de toepassing.|
|50027|Ongeldig JWT-token om de volgende redenen:<ul><li>bevat geen nonceclaim, subclaim</li><li>onderwerp-id komt niet overeen</li><li>duplicaatclaim in idToken-claims</li><li>onverwachte certificaatverlener</li><li>onverwachte doelgroep</li><li>niet binnen geldig tijdsbereik </li><li>indeling van het token is niet juist</li><li>Handtekeningverificatie van extern ID-token van de verlener is mislukt.</li></ul>Neem contact op met de eigenaar van de toepassing|
|50029|Ongeldige URI: domeinnaam bevat ongeldige tekens. Neem contact op met de beheerder van de tenant.|
|50034|De gebruiker bestaat niet in de lijst. Neem contact op met de beheerder van de tenant.|
|50042|De salt vereist voor het genereren van een pairwise-id ontbreekt in de principal. Neem contact op met de beheerder van de tenant.|
|50048|Onderwerpverschillen verlenerclaim in de clientverklaring. Neem contact op met de beheerder van de tenant.|
|50050|De aanvraag is onjuist gevormd. Neem contact op met de eigenaar van de toepassing.|
|50053|Uw account is vergrendeld omdat u zich te vaak hebt aangemeld met een onjuiste gebruikers-id of een onjuist wachtwoord.|
|50055|Het opgegeven wachtwoord is ongeldig of verlopen.|
|50056|Wachtwoord ongeldig of null-wachtwoord: wachtwoord bestaat niet in het archief voor deze gebruiker|
|50057|Het gebruikersaccount is uitgeschakeld. Het account is uitgeschakeld door een beheerder.|
|50058|De toepassing probeert een stille aanmelding uit te voeren en dit is niet gelukt voor de gebruiker. De toepassing moet een interactieve stroom starten zodat gebruikers een optie krijgen om zich aan te melden. Neem contact op met de eigenaar van de app.|
|50059|De gebruiker bestaat niet in de lijst. Neem contact op met de beheerder van de tenant|
|50061|De aanvraag voor afmelden is ongeldig. Neem contact op met de eigenaar van de toepassing|
|50072|Gebruiker moet zich registreren voor verificatie met een tweede factor (interactief)|
|50074|Gebruiker heeft de MFA-controle niet doorstaan.|
|50076|De gebruiker heeft de MFA-controle niet doorstaan (niet-interactief)|
|50079|Gebruiker moet zich registreren voor verificatie met een tweede factor (niet-interactieve aanmelding)|
|50085|Vernieuwingstoken vereist een sociale IDP-aanmelding. Laat gebruikers proberen zich opnieuw aan te melden met gebruikersnaam en wachtwoord|
|50089|Stroomtoken verlopen, verificatie is mislukt. Laat gebruikers proberen zich opnieuw aan te melden met gebruikersnaam en wachtwoord|
|50097|Verificatie van apparaten nodig, DeviceId-, DeviceAltSecId-claims zijn null of er bestaat geen apparaat dat overeenkomt met de apparaat-id|
|50099|JWT-handtekening is ongeldig. Neem contact op met de eigenaar van de toepassing.|
|50105|De aangemelde gebruiker is niet toegewezen aan een rol voor de aangemelde toepassing. Wijs de gebruiker toe aan de toepassing. Voor meer informatie: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|Aangevraagde federation realm-object bestaat niet. Neem contact op met de beheerder van de tenant.|
|50120|Probleem met de JWT-header. Neem contact op met de beheerder van de tenant.|
|50124|Claimtransformatie bevat een ongeldige invoerparameter. Neem contact op met de tenantbeheerder om het beleid bij te werken.|
|50125|Het aanmelden is onderbroken vanwege het opnieuw instellen van een wachtwoord of een nieuwe wachtwoordregistratie-invoer|
|50126|Ongeldige gebruikersnaam of wachtwoord of ongeldige on-premises gebruikersnaam of het wachtwoord.|
|50127|De gebruiker moet een broker-app installeren om toegang te krijgen tot zijn inhoud.|
|50128|De domeinnaam is ongeldig: geen tenant-identificatiegegevens gevonden in de aanvraag of impliciet door opgegeven referenties|
|50129|Het apparaat is niet toegevoegd aan de werkplek: aan werkplek toevoegen is vereist om het apparaat te registreren.|
|50130|Claimwaarde kan niet worden geïnterpreteerd als bekende verificatiemethode|
|50131|Wordt gebruikt voor verschillende fouten voor voorwaardelijke toegang. Bijvoorbeeld Een slechte Windows-apparaatstatus, geblokkeerde aanvraag vanwege verdachte activiteit, beslissingen voor toegangsbeleid en beveiligingsbeleid.|
|50132|Referenties zijn om de volgende redenen ingetrokken:<ul><li>SSO-artefact is ongeldig of verlopen</li><li>Sessie is niet actueel genoeg voor toepassing</li><li>Een aanvraag voor stille aanmelding is verzonden, maar de gebruikerssessie met Azure AD is ongeldig of verlopen.</li></ul>|
|50133|Sessie is ongeldig omdat het wachtwoord is verlopen of recent is gewijzigd.|
|50135|Wijzigen van wachtwoord is vereist vanwege accountrisico|
|50136|MSA-sessie omleiden naar app, enkele MSA-sessie gedetecteerd |
|50140|Deze fout is opgetreden vanwege de interrupt 'Aangemeld blijven' toen de gebruiker bezig was met aanmelden. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-id, aanvraag-id en een foutcode voor meer informatie. |
|50143|Sessie komt niet overeen: sessie is ongeldig omdat de gebruikertenant niet overeenkomt met de domeinhint als gevolg van andere resource. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-id, aanvraag-id en een foutcode voor meer informatie.|
|50144|Het Active Directory-wachtwoord van de gebruiker is verlopen. Genereer een nieuw wachtwoord voor de gebruiker of laat de eindgebruiker dit doen met de selfservice-resettool|
|50146|Deze toepassing moet worden geconfigureerd met een ondertekeningssleutel specifiek voor de toepassing. De toepassing is niet met een dergelijke sleutel geconfigureerd, of de sleutel is verlopen of nog niet geldig. Neem contact op met de eigenaar van de toepassing|
|50148|De codeverificatie komt niet overeen met de code-uitdaging opgegeven in de autorisatieaanvraag voor PKCE. Neem contact op met de ontwikkelaar van de toepassing. |
|50155|Apparaatverificatie is mislukt voor deze gebruiker|
|50158|Er is niet aan de externe beveiligingsvraag voldaan|
|50161|Claims verzonden door de externe provider zijn niet voldoende of ontbrekende claim gevraagd aan externe provider|
|50166|Kan aanvraag niet verzenden naar claimprovider|
|50169|De realm is niet een geconfigureerde realm van de huidige servicenaamruimte.|
|50172|Externe claimprovider is niet goedgekeurd. Neem contact op met de beheerder van de tenant|
|50173|Nieuwe verificatietoken is vereist. Laat de gebruiker zich opnieuw aanmelden met nieuwe referenties|
|50177|Externe uitdaging wordt niet ondersteund voor passthrough-gebruikers|
|50178|Sessiebeheer wordt niet ondersteund voor passthrough-gebruikers|
|50180|Geïntegreerde Windows-verificatie is vereist. Schakel de tenant in voor een naadloze eenmalige aanmelding (SSO).|
|51001|Domeinhint is niet aanwezig met on-premises beveiligings-id - on-premises UPN|
|51004|Gebruikersaccount bestaat niet in de map.|
|51006|Geïntegreerde Windows-verificatie is vereist. De gebruiker is aangemeld met sessietoken waarin wia-claim ontbreekt. Vraag de gebruiker zich opnieuw aan te melden.|
|52004|Gebruiker heeft geen toestemming gegeven voor toegang tot LinkedIn-resources. |
|53000|Voor beleid voor voorwaardelijke toegang is een compatibel apparaat vereist. Het apparaat is niet compatibel. Laat de gebruiker het apparaat inschrijven bij een goedgekeurde MDM-provider zoals Intune|
|53001|Voor beleid voor voorwaardelijke toegang is een domeindeelname-apparaat vereist en het apparaat neemt niet deel. Laat de gebruiker een domeindeelname-apparaat gebruiken|
|53002|Gebruikte toepassing is niet goedgekeurd voor voorwaardelijke toegang. Gebruiker moet een van de apps uit de lijst met goedgekeurde toepassingen gebruiken om toegang te krijgen.|
|53003|Toegang is geblokkeerd vanwege beleid voor voorwaardelijke toegang.|
|53004|Gebruiker moet multi-factor authentication-registratieproces voltooien om toegang te krijgen tot deze inhoud. Gebruiker moet zich registreren voor multi-factor authentication.|
|65001|Toepassing X heeft geen toegangsmachtiging voor toepassing Y of de machtiging is ingetrokken. Of de gebruiker of beheerder heeft niet toegestaan dat de toepassing wordt gebruikt met id X. Stuur een interactieve autorisatieaanvraag voor deze gebruiker en resource. Of de gebruiker of beheerder heeft niet toegestaan dat de toepassing wordt gebruikt met id X. Stuur een autorisatieaanvraag naar uw tenant-beheerder uit naam van toepassing Y voor resource Z.|
|65004|Gebruiker heeft geweigerd toestemming te geven app-toegang. Laat de gebruiker zich opnieuw aanmelden en toestemming geven voor de app|
|65005|De voor de toepassing vereiste resourcetoegangslijst bevat geen toepassingen die kunnen worden gedetecteerd door de resource, de clienttoepassing heeft toegang aangevraagd tot een resource die niet is opgegeven in de vereiste resourcetoegangslijst, de Graph-service heeft een onjuiste aanvraag geretourneerd of de resource is niet gevonden. Als de toepassing SAML ondersteunt, hebt u mogelijk de toepassing met de verkeerde id (entiteit) geconfigureerd. Probeer de oplossing voor SAML via de koppeling hieronder: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list)|
|70000|Ongeldige toekenning om de volgende redenen:<ul><li>Aangevraagde SAML 2.0-verklaring heeft een ongeldige bevestigingsmethode voor onderwerpen</li><li>App OnBehalfOf flow wordt niet ondersteund op V2</li><li>Primaire vernieuwingstoken is niet ondertekend met sessiesleutel</li><li>Ongeldig extern vernieuwingstoken</li><li>Het verlenen van toegang is verkregen voor een andere tenant.</li></ul>|
|70001|De toepassing X is niet gevonden in de tenant Y. Dit kan gebeuren als de toepassing X niet is geïnstalleerd door de beheerder van de tenant of er geen toestemming voor is verleend door een gebruiker in de tenant. U hebt mogelijk de id-waarde voor de toepassing onjuist geconfigureerd of uw verificatieaanvraag verzonden naar de verkeerde tenant|
|70002|De toepassing heeft ongeldige clientreferenties geretourneerd. Neem contact op met de eigenaar van de app|
|70003|De toepassing heeft een niet-ondersteunde toekenningstype geretourneerd. Neem contact op met de eigenaar van de app|
|70004|De toepassing heeft een ongeldige omleidings-URI geretourneerd. Het omleidingsadres dat is gespecificeerd door de client komt niet overeen met een geconfigureerd adres of een adres op de OIDC-goedkeuringslijst. Neem contact op met de eigenaar van de app|
|70005|De toepassing heeft een niet-ondersteund reactietype geretourneerd om de volgende redenen:<ul><li>reactietype 'token' is niet ingeschakeld voor de toepassing</li><li>reactietype 'id_token' vereist 'OpenID'-bereik - bevat een niet-ondersteunde OAuth-parameterwaarde in de gecodeerde wctx</li></ul>Neem contact op met de eigenaar van de toepassing|
|70007|De toepassing heeft een niet-ondersteunde 'response_mode'-waarde geretourneerd bij het aanvragen van een token. Neem contact op met de eigenaar van de app|
|70008|De opgegeven autorisatiecode of het vernieuwingstoken is verlopen - ingetrokken. Laat de gebruiker zich opnieuw aanmelden|
|70011|Het bereik dat is aangevraagd door de toepassing is ongeldig. Neem contact op met de eigenaar van de toepassing|
|70012|Er is een serverfout opgetreden tijdens het verifiëren van een MSA-gebruiker (consument). Probeer het opnieuw. Als dit probleem blijft bestaan, [opent u een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) |
|70018|Ongeldige verificatiecode doordat gebruiker verkeerde gebruikerscode typt voor apparaatcodestroom. Autorisatie niet is goedgekeurd|
|70019|Verificatiecode verlopen. Laat de gebruiker zich opnieuw aanmelden|
|70037|Onjuist antwoord op verificatievraag. Externe verificatiesessie is geweigerd.|
|75001|Er is een fout opgetreden tijdens de binding van SAML-bericht.|
|75003|De toepassing heeft een fout met betrekking tot niet-ondersteunde binding geretourneerd (reactie van SAML-protocol kan niet worden verzonden via andere bindingen dan HTTP POST). Neem contact op met de eigenaar van de app|
|75005|Azure AD biedt geen ondersteuning voor de SAML-aanvraag die door de toepassing is verzonden voor eenmalige aanmelding. Neem contact op met de eigenaar van de app|
|75008|De aanvraag van de toepassing is geweigerd omdat de SAML-aanvraag een onverwacht doel heeft. Neem contact op met de eigenaar van de app|
|75011|Verificatiemethode waarmee de gebruiker is geverifieerd met de service komt niet overeen met aangevraagd verificatiemethode. Neem contact op met de eigenaar van de app|
|75016|SAML2-verificatieverzoek heeft ongeldige NameIdPolicy. Neem contact op met de eigenaar van de app|
|80001|Verificatieagent kan geen verbinding maken met Active Directory. Zorg ervoor dat de verificatie-agent is geïnstalleerd op een apparaat met domeindeelname met zicht op een DC die de aanmelding van de gebruiker kan regelen.|
|80002|Interne fout. Time-out bij wachtwoordvalidatie. We kunnen de verificatieaanvraag niet verzenden naar de interne hybride identiteitsservice. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout|
|80003|Ongeldig antwoord ontvangen door de verificatieagent. Er is een onbekende fout opgetreden tijdens een verificatiepoging op basis van on-premises Active Directory. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout.|
|80005|Verificatieagent: er is een onbekende fout opgetreden tijdens het verwerken van het antwoord van de verificatieagent. [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout.|
|80007|Verificatieagent kan wachtwoord van gebruiker niet verifiëren.|
|80010|Verificatieagent kan wachtwoord niet ontsleutelen. |
|80011|Verificatieagent kan versleutelingssleutel niet ophalen.|
|80012|De gebruikers hebben geprobeerd om zich aan te melden buiten de toegestane periode (dit is opgegeven in AD)|
|80013|De verificatiepoging kan niet worden voltooid vanwege tijdverschil tussen de computer met de verificatieagent en AD. Los het probleem met tijdsynchronisatie op|
|80014|Time-out van verificatieagent [Open een ondersteuningsticket](fundamentals/active-directory-troubleshooting-support-howto.md) met de foutcode, correlatie-id en Datetime voor meer informatie over deze fout|
|81001|Kerberos-ticket van de gebruiker is te groot. Dit kan gebeuren als de gebruiker te veel groepen heeft en het Kerberos-ticket dus te veel groepslidmaatschappen bevat. Reduceer het aantal groepslidmaatschappen van de gebruiker en probeer het opnieuw.|
|81005|Verificatiepakket wordt niet ondersteund|
|81007|Tenant is niet ingeschakeld voor naadloze eenmalige aanmelding|
|81012|Dit is een fout optreedt. Hiermee wordt aangegeven dat gebruiker zich aanmelden bij Azure AD verschilt van de gebruiker is aangemeld bij het apparaat. U kunt deze code in de logboeken negeren.|
|90010|De aanvraag wordt niet ondersteund om verschillende redenen. Bijvoorbeeld, de aanvraag wordt gedaan met behulp van een niet-ondersteunde aanvraag-methode (alleen POST-methode wordt ondersteund) of het token-ondertekening algoritme dat is aangevraagd, wordt niet ondersteund. Neem contact op met de ontwikkelaar van de toepassing.|
|90014| Er ontbreekt een vereist veld voor een protocolbericht, neem contact op met de eigenaar van de toepassing. Als u eigenaar van de toepassing bent, zorgt u ervoor dat u de vereiste parameters voor de aanmeldingsaanvraag hebt. |
|90072| Het account moet eerst worden toegevoegd als een externe gebruiker in de tenant. Afmelden en aanmelden met een andere Azure AD account.|
|90094| De toekenning zijn beheerdersrechten vereist. Vraag de beheerder van de tenant voor toestemming voor deze toepassing.|

## <a name="next-steps"></a>Volgende stappen

Raadpleeg [Aanmeldactiviteitenrapporten in Azure Active Directory Portal](active-directory-reporting-activity-sign-ins.md) voor meer informatie.
