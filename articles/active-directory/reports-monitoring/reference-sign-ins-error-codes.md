---
title: Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Naslaginformatie over foutcodes voor aanmeldactiviteitenrapporten.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f1ce786b748fedd1ec4c722b28bc11c28672c2f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443399"
---
# <a name="sign-in-activity-report-error-codes"></a>Foutcodes voor aanmeldingsactiviteiten rapport 

Met de informatie die door de [gebruikersrapport van de aanmeldingen](concept-sign-ins.md), vindt u antwoorden op vragen zoals:

- Die zich aangemeld bij mijn toepassing?
- Welke toepassingen zijn aangemeld bij?
- Welke aanmeldingen kan waarom niet?

Wanneer een aanmelding is mislukt, ziet u een foutcode die overeenkomt met de fout. In dit artikel bevat de foutcodes en de bijbehorende beschrijvingen, samen met een voorgestelde actie, indien van toepassing. 

## <a name="how-can-i-display-failed-sign-ins"></a>Hoe kan ik mislukte aanmeldingen weergeven? 

Navigeer naar de [aanmeldingenrapport](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns) in de [Azure-portal](https://portal.azure.com).

![Aanmeldingsactiviteit](./media/reference-sign-ins-error-codes/61.png "Aanmeldingsactiviteit")

Alle mislukte aanmeldingen weergeven door te selecteren van het rapport filteren **fout** uit de **aanmeldingsstatus** vervolgkeuzelijst.

![Aanmeldingsactiviteit](./media/reference-sign-ins-error-codes/06.png "Aanmeldingsactiviteit")

Hiermee opent u een item selecteren in de gefilterde lijst de **activiteitendetails: Aanmeldingen** blade. Deze weergave biedt u meer informatie over de mislukte aanmelding gebeurtenis, met inbegrip van de **aanmelden foutcode** en **reden van fout**.

![Aanmeldingsactiviteit](./media/reference-sign-ins-error-codes/05.png "Aanmeldingsactiviteit")

U kunt ook programmatisch toegang verkrijgen tot de aanmelding via de [rapportage-API](concept-reporting-api.md).

## <a name="error-codes"></a>Foutcodes


|Fout|Description|
|---|---|
|16000|Dit is een interne implementatiedetail en niet een fout optreedt. U kunt deze referentie negeren.|
|20001|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|20012|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|20033|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40008|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40009|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|40014|Er is een probleem met uw federatieve id-provider. Neem contact op met uw IDP om dit probleem op te lossen.|
|50000|Er is een probleem met onze aanmeldingsservice. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) om dit probleem op te lossen.|
|50001|De Service Principal Name is niet gevonden in deze tenant. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant, of als de resource-principal is niet gevonden in de map of is ongeldig.|
|50002|Aanmelden is mislukt vanwege beperkte proxytoegang op de tenant. Als het tenantbeleid voor uw eigen, kunt u uw beperkte tenantinstellingen om dit probleem te verhelpen.|
|50003|Aanmelden is mislukt vanwege een ontbrekende handtekeningsleutel of ontbrekend handtekeningcertificaat. Dit kan komen doordat er geen handtekeningsleutel is geconfigureerd in de toepassing. Bekijk de oplossingen die worden beschreven in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Als het probleem zich blijft voordoen, neem contact op met de eigenaar van de toepassing of de beheerder van de toepassing.|
|50005|Gebruiker heeft geprobeerd aan te melden met een apparaat van een platform dat wordt momenteel niet ondersteund via beleid voor voorwaardelijke toegang.|
|50006| Handtekeningverificatie is mislukt vanwege ongeldige handtekening. Bekijk de oplossing die wordt beschreven in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery). Als het probleem zich blijft voordoen, neem contact op met de toepassingseigenaar of de beheerder van de toepassing.|
|50007|Het partnerversleutelingscertificaat is niet gevonden voor deze toepassing. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met Microsoft om dit vast.|
|50008|SAML-verklaring ontbreekt of is niet juist geconfigureerd in het token. Neem contact op met uw federatieprovider.|
|50010|Validatie van de doelgroep-URI voor de toepassing is mislukt, omdat er geen tokendoelgroepen zijn geconfigureerd. Neem contact op met de eigenaar van de toepassing voor het omzetten van.|
|50011|Het antwoordadres ontbreekt, is onjuist geconfigureerd of komt niet overeen met de antwoordadressen die voor de toepassing zijn geconfigureerd. Probeer de oplossing die wordt vermeld op [ https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application ](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Als het probleem zich blijft voordoen, neem contact op met de toepassingseigenaar of de beheerder van de toepassing.|
|50012| Dit is een algemene foutmelding die aangeeft dat de verificatie is mislukt. Dit kan gebeuren om redenen als de ontbrekende of ongeldige referenties of claims in de aanvraag. Zorg ervoor dat de aanvraag is verzonden met de juiste referenties en claims. |
|50013|Bevestiging is ongeldig vanwege verschillende redenen. Bijvoorbeeld, de uitgever van het token komt niet overeen met de api-versie binnen het bereik van geldige tijd, het token is verlopen of onjuist gevormd of het vernieuwingstoken dat in de verklaring is niet een primaire vernieuwingstoken.|
|50017|Validatie van certificaat is mislukt om de volgende redenen:<ul><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Verwacht CrlSegment kan niet worden gevonden</li><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Delta CRL-distributiepunt is geconfigureerd zonder een bijbehorend CRL-distributiepunt</li><li>Kan geen geldige CRL-segmenten ophalen als gevolg van time-outprobleem</li><li>Kan CRL niet downloaden</li></ul>Neem contact op met de tenantbeheerder.|
|50020|De gebruiker is niet gemachtigd voor een van de volgende oorzaken hebben.<ul><li>De gebruiker zich probeert aan te melden met een MSA-account met de v1-eindpunt</li><li>De gebruiker bestaat niet in de tenant.</li></ul> Neem contact op met de eigenaar van de toepassing.|
|50027|Ongeldig JWT-token om de volgende redenen:<ul><li>bevat geen nonceclaim, subclaim</li><li>onderwerp-id komt niet overeen</li><li>duplicaatclaim in idToken-claims</li><li>onverwachte certificaatverlener</li><li>onverwachte doelgroep</li><li>niet binnen geldig tijdsbereik </li><li>indeling van het token is niet juist</li><li>Handtekeningverificatie van extern ID-token van de verlener is mislukt.</li></ul>Neem contact op met de eigenaar van de toepassing|
|50029|Ongeldige URI: domeinnaam bevat ongeldige tekens. Neem contact op met de tenantbeheerder.|
|50034|De gebruiker bestaat niet in de lijst. Neem contact op met uw tenantbeheerder.|
|50042|De salt vereist voor het genereren van een pairwise id ontbreekt in principe. Neem contact op met de tenantbeheerder.|
|50048|Onderwerpverschillen verlenerclaim in de clientverklaring. Neem contact op met de tenantbeheerder.|
|50050|De aanvraag is onjuist gevormd. Neem contact op met de eigenaar van de toepassing.|
|50053|Account is vergrendeld omdat de gebruiker heeft geprobeerd aan te melden te vaak hebt aangemeld met een onjuiste gebruikersnaam of wachtwoord.|
|50055|Het opgegeven wachtwoord is ongeldig of verlopen.|
|50056|Ongeldig of null-wachtwoord - wachtwoord bestaat niet in het archief voor deze gebruiker.|
|50057|Het gebruikersaccount is uitgeschakeld. Het account is uitgeschakeld door een beheerder.|
|50058|De toepassing probeert een stille aanmelding uit te voeren en dit is niet gelukt voor de gebruiker. U moet de toepassing start een interactieve stroom zodat gebruikers een optie om aan te melden. Neem contact op met de eigenaar van toepassing.|
|50059|De gebruiker bestaat niet in de lijst. Neem contact op met uw tenantbeheerder.|
|50061|De aanvraag voor afmelden is ongeldig. Neem contact op met de eigenaar van de toepassing.|
|50072|Gebruiker moet zich registreren voor verificatie met twee factoren (interactief).|
|50074|Gebruiker heeft de MFA-controle niet doorstaan.|
|50076|Gebruiker is niet geslaagd de MFA-controle (niet-interactief).|
|50079|Gebruiker moet zich registreren voor verificatie met twee factoren (niet-interactieve aanmeldingen).|
|50085|Vernieuwingstoken vereist een sociale IDP-aanmelding. Hebben gebruikers probeer aanmelden met hun gebruikersnaam en wachtwoord.|
|50089|Stroom token verlopen - verificatie is mislukt. Gebruiker probeert aanmelden met hun gebruikersnaam en wachtwoord|
|50097|Apparaatverificatie is vereist. Dit kan zich voordoen als de apparaat-id of DeviceAltSecId claims zijn **null**, of als er geen apparaat overeenkomt met de apparaat-id bestaat.|
|50099|JWT-handtekening is ongeldig. Neem contact op met de eigenaar van de toepassing.|
|50105|De aangemelde gebruiker is niet toegewezen aan een rol voor de aangemelde toepassing. De gebruiker toewijzen aan de toepassing. Voor meer informatie: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|Aangevraagde federation realm-object bestaat niet. Neem contact op met de tenantbeheerder.|
|50120|Probleem met de JWT-header. Neem contact op met de tenantbeheerder.|
|50124|Claimtransformatie bevat een ongeldige invoerparameter. Neem contact op met de tenantbeheerder voor het bijwerken van het beleid.|
|50125|Aanmelden is onderbroken vanwege een wachtwoord opnieuw instellen of de registratie wachtwoordinvoer.|
|50126|Ongeldige gebruikersnaam of wachtwoord, of ongeldige on-premises gebruikersnaam of wachtwoord.|
|50127|Er moet een gebruiker om een broker-toepassing toegang te krijgen tot deze inhoud te installeren.|
|50128|De domeinnaam is ongeldig: Er is geen tenant-identificatiegegevens gevonden in een van de aanvraag of impliciet door een opgegeven referenties.|
|50129|Apparaat is niet aan de werkplek gekoppeld - **Workplace join** is vereist voor de registratie van het apparaat.|
|50130|Waarde van de claim kan niet worden geïnterpreteerd als bekende auth-methode.|
|50131|Wordt gebruikt voor verschillende fouten voor voorwaardelijke toegang. Bijvoorbeeld Een slechte Windows-apparaatstatus, geblokkeerde aanvraag vanwege verdachte activiteit, beslissingen voor toegangsbeleid en beveiligingsbeleid.|
|50132|Referenties zijn om de volgende redenen ingetrokken:<ul><li>SSO-artefact is ongeldig of verlopen</li><li>Sessie is niet actueel genoeg voor toepassing</li><li>Een aanvraag voor stille aanmelding is verzonden, maar de gebruikerssessie met Azure AD is ongeldig of verlopen.</li></ul>|
|50133|Sessie is ongeldig omdat het wachtwoord is verlopen of recent is gewijzigd.|
|50135|Wachtwoordwijziging is vereist om account gevaar.|
|50136|Omleiden MSA-sessie naar de toepassing - één MSA sessie gedetecteerd. |
|50140|Deze fout is opgetreden vanwege de interrupt 'Aangemeld blijven' toen de gebruiker bezig was met aanmelden. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-id, aanvraag-id en een foutcode voor meer informatie. |
|50143|Sessie komt niet overeen - sessie is ongeldig omdat de tenant van de gebruiker komt niet overeen met de domeinhint vanwege een andere resource.  [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-ID, aanvraag-ID, en foutcode voor meer informatie.|
|50144|Het Active Directory-wachtwoord van de gebruiker is verlopen. Genereren van een nieuw wachtwoord voor de gebruiker of de gebruiker opnieuw instellen van self-servicegebruikers het hulpprogramma hebt.|
|50146|Deze toepassing moet worden geconfigureerd met een ondertekeningssleutel specifiek voor de toepassing. De toepassing is niet met een dergelijke sleutel geconfigureerd, of de sleutel is verlopen of nog niet geldig. Neem contact op met de eigenaar van de toepassing.|
|50148|De codeverificatie komt niet overeen met de code-uitdaging opgegeven in de autorisatieaanvraag voor PKCE. Neem contact op met de ontwikkelaar van de toepassing. |
|50155|Apparaatverificatie is mislukt voor deze gebruiker.|
|50158|Externe beveiligingsvraag is niet voldaan aan.|
|50161|Claims die zijn verzonden door de externe provider is niet voldoende of ontbrekende claim aangevraagd naar externe provider.|
|50166|Aanvraag verzenden naar claimprovider is mislukt.|
|50169|De realm is niet een geconfigureerde realm van de huidige servicenaamruimte.|
|50172|Externe claimprovider is niet goedgekeurd. Neem contact op met de tenantbeheerder|
|50173|Nieuwe verificatietoken is vereist. Het aanmelden van gebruikers opnieuw met behulp van nieuwe referenties hebben.|
|50177|Externe uitdaging wordt niet ondersteund voor passthrough-gebruikers.|
|50178|Sessiebeheer wordt niet ondersteund voor passthrough-gebruikers.|
|50180|Geïntegreerde Windows-verificatie is vereist. Schakel de tenant in voor een naadloze eenmalige aanmelding (SSO).|
|51001|Domeinhint is niet aanwezig zijn met On-Premises beveiligings-id - On-Premises UPN.|
|51004|Gebruikersaccount bestaat niet in de map.|
|51006|Geïntegreerde Windows-verificatie is vereist. De gebruiker is aangemeld met behulp van de sessietoken dat via claim ontbreekt. Vraag de gebruiker zich opnieuw aan te melden.|
|52004|Gebruiker heeft geen toestemming gegeven voor toegang tot LinkedIn-resources. |
|53000|Voor beleid voor voorwaardelijke toegang is een compatibel apparaat vereist. Het apparaat is niet compatibel. Laat de gebruiker die hun apparaat inschrijven bij een goedgekeurde MDM-provider, zoals Intune.|
|53001|Voor beleid voor voorwaardelijke toegang is een domeindeelname-apparaat vereist en het apparaat neemt niet deel. Apparaat lid zijn van de gebruiker gebruikt een domein.|
|53002|Gebruikte toepassing is niet goedgekeurd voor voorwaardelijke toegang. Gebruiker moet een van de apps uit de lijst met goedgekeurde toepassingen gebruiken om toegang te krijgen.|
|53003|Toegang is geblokkeerd vanwege beleid voor voorwaardelijke toegang.|
|53004|Gebruiker moet multi-factor authentication-registratieproces voltooien om toegang te krijgen tot deze inhoud. Gebruiker moet zich registreren voor multi-factor authentication.|
|65001|Toepassing X heeft geen toegangsmachtiging voor toepassing Y of de machtiging is ingetrokken. Of de gebruiker of beheerder heeft niet toegestaan dat de toepassing wordt gebruikt met id X. Stuur een interactieve autorisatieaanvraag voor deze gebruiker en resource. Of de gebruiker of beheerder heeft niet toegestaan voor het gebruik van de toepassing met ID X. Send een autorisatieaanvraag naar uw tenant-beheerder om te handelen namens de App: Y voor Resource: Z.|
|65004|Gebruiker heeft geweigerd toestemming te geven app-toegang. Laat de gebruiker zich opnieuw aanmelden en toestemming geven voor de app|
|65005|De voor de toepassing vereiste resourcetoegangslijst bevat geen toepassingen die kunnen worden gedetecteerd door de resource, de clienttoepassing heeft toegang aangevraagd tot een resource die niet is opgegeven in de vereiste resourcetoegangslijst, de Graph-service heeft een onjuiste aanvraag geretourneerd of de resource is niet gevonden. Als de toepassing SAML ondersteunt, hebt u mogelijk de toepassing met de verkeerde id (entiteit) geconfigureerd. Probeer de oplossing voor SAML via de koppeling hieronder: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)|
|70000|Ongeldige toekenning om de volgende redenen:<ul><li>Aangevraagde SAML 2.0-verklaring heeft een ongeldige bevestigingsmethode voor onderwerpen</li><li>App OnBehalfOf flow wordt niet ondersteund op V2</li><li>Primaire vernieuwingstoken is niet ondertekend met sessiesleutel</li><li>Ongeldig extern vernieuwingstoken</li><li>Het verlenen van toegang is verkregen voor een andere tenant.</li></ul>|
|70001|De toepassing X is niet gevonden in de tenant Y. Dit kan gebeuren als de toepassing X niet is geïnstalleerd door de beheerder van de tenant of er geen toestemming voor is verleend door een gebruiker in de tenant. U mogelijk de id-waarde voor de toepassing onjuist geconfigureerd of de verificatieaanvraag naar de verkeerde tenant verzonden.|
|70002|De toepassing heeft ongeldige clientreferenties geretourneerd. Neem contact op met de eigenaar van de toepassing.|
|70003|De toepassing heeft een niet-ondersteunde toekenningstype geretourneerd. Neem contact op met de eigenaar van de toepassing.|
|70004|De toepassing heeft een ongeldige omleidings-URI geretourneerd. Het omleidingsadres dat is gespecificeerd door de client komt niet overeen met een geconfigureerd adres of een adres op de OIDC-goedkeuringslijst. Neem contact op met de eigenaar van de toepassing.|
|70005|De toepassing heeft een niet-ondersteund reactietype geretourneerd om de volgende redenen:<ul><li>reactietype 'token' is niet ingeschakeld voor de toepassing</li><li>reactietype 'id_token' vereist 'OpenID'-bereik - bevat een niet-ondersteunde OAuth-parameterwaarde in de gecodeerde wctx</li></ul>Neem contact op met de eigenaar van de toepassing.|
|70007|De toepassing heeft een niet-ondersteunde 'response_mode'-waarde geretourneerd bij het aanvragen van een token. Neem contact op met de eigenaar van de toepassing.|
|70008|De opgegeven autorisatiecode of het vernieuwen van token is verlopen of ingetrokken. Hebben de gebruiker opnieuw aanmelden.|
|70011|Het bereik dat is aangevraagd door de toepassing is ongeldig. Neem contact op met de eigenaar van de toepassing.|
|70012|Er is een serverfout opgetreden tijdens het verifiëren van een MSA-gebruiker (consument). Probeer de aanmelding, en als het probleem zich blijft voordoen, [een ondersteuningsticket openen](../fundamentals/active-directory-troubleshooting-support-howto.md) |
|70018|Ongeldige verificatiecode doordat gebruiker verkeerde gebruikerscode typt voor apparaatcodestroom. Autorisatie is niet goedgekeurd.|
|70019|Verificatiecode verlopen. Laat de gebruiker de aanmelding opnieuw.|
|70037|Onjuist antwoord op verificatievraag. Externe verificatiesessie is geweigerd.|
|75001|Er is een fout opgetreden tijdens de binding van SAML-bericht.|
|75003|De toepassing heeft een fout met betrekking tot niet-ondersteunde binding geretourneerd (reactie van SAML-protocol kan niet worden verzonden via andere bindingen dan HTTP POST). Neem contact op met de eigenaar van de toepassing.|
|75005|Azure AD biedt geen ondersteuning voor de SAML-aanvraag die door de toepassing is verzonden voor eenmalige aanmelding. Neem contact op met de eigenaar van de toepassing.|
|75008|De aanvraag van de toepassing is geweigerd omdat de SAML-aanvraag een onverwacht doel heeft. Neem contact op met de eigenaar van de toepassing.|
|75011|Verificatiemethode waarmee de gebruiker is geverifieerd met de service komt niet overeen met aangevraagd verificatiemethode. Neem contact op met de eigenaar van de toepassing.|
|75016|SAML2-verificatieverzoek heeft ongeldige NameIdPolicy. Neem contact op met de eigenaar van de toepassing.|
|80001|Verificatieagent kan geen verbinding maken met Active Directory. Zorg ervoor dat de verificatie-agent is geïnstalleerd op een apparaat met domeindeelname met zicht op een DC die de aanmelding van de gebruiker kan regelen.|
|80002|Interne fout. Time-out bij wachtwoordvalidatie. We kunnen de verificatieaanvraag niet verzenden naar de interne hybride identiteitsservice. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout.|
|80003|Ongeldig antwoord ontvangen door de verificatieagent. Er is een onbekende fout opgetreden tijdens een verificatiepoging op basis van on-premises Active Directory. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout.|
|80005|Verificatie-Agent: Er is een onbekende fout opgetreden tijdens het verwerken van het antwoord van de verificatie-Agent. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) om meer informatie te krijgen over de fout.|
|80007|Verificatieagent kan wachtwoord van gebruiker niet verifiëren.|
|80010|Verificatieagent kan wachtwoord niet ontsleutelen. |
|80011|Verificatieagent kan versleutelingssleutel niet ophalen.|
|80012|De gebruikers hebben geprobeerd om aan te melden buiten het toegestane aantal uren (dit is opgegeven in AD).|
|80013|De verificatiepoging kan niet worden voltooid vanwege tijdverschil tussen de computer met de verificatieagent en AD. Los het probleem met tijdsynchronisatie op|
|80014|Time-out van verificatieagent [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met de foutcode, correlatie-ID en datum/tijd voor meer informatie over deze fout.|
|81001|Kerberos-ticket van de gebruiker is te groot. Dit kan gebeuren als de gebruiker te veel groepen heeft en het Kerberos-ticket dus te veel groepslidmaatschappen bevat. Reduceer het aantal groepslidmaatschappen van de gebruiker en probeer het opnieuw.|
|81005|Verificatiepakket niet ondersteund.|
|81007|Tenant is niet ingeschakeld voor naadloze eenmalige aanmelding.|
|81012|Dit is een fout optreedt. Hiermee wordt aangegeven dat gebruiker zich aanmelden bij Azure AD verschilt van de gebruiker is aangemeld bij het apparaat. U kunt deze code in de logboeken negeren.|
|90010|De aanvraag wordt niet ondersteund om verschillende redenen. Bijvoorbeeld, de aanvraag wordt gedaan met behulp van een niet-ondersteunde aanvraag-methode (alleen POST-methode wordt ondersteund) of het token-ondertekening algoritme dat is aangevraagd, wordt niet ondersteund. Neem contact op met de ontwikkelaar van de toepassing.|
|90014| Er ontbreekt een vereist veld voor een protocolbericht, neem contact op met de eigenaar van de toepassing. Als u eigenaar van de toepassing bent, zorgt u ervoor dat u de vereiste parameters voor de aanmeldingsaanvraag hebt. |
|90072| Het account moet eerst worden toegevoegd als een externe gebruiker in de tenant. Afmelden en aanmelden met een andere Azure AD account.|
|90094| De toekenning zijn beheerdersrechten vereist. Vraag de beheerder van de tenant voor toestemming voor deze toepassing.|
|500133| De verklaring valt niet binnen het bereik van geldige tijd. Zorg ervoor dat het toegangstoken niet voordat u deze gebruikt voor een verklaring van de gebruiker is verlopen of vraag een nieuw token.|

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van de rapporten van aanmeldingen](concept-sign-ins.md)
* [Programmatische toegang tot Azure AD-rapporten](concept-reporting-api.md)
