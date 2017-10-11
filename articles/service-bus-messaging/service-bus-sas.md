---
title: Azure Service Bus-verificatie met handtekeningen voor gedeelde toegang | Microsoft Docs
description: Overzicht van Service Bus-verificatie met behulp van handtekeningen voor gedeelde toegang-overzicht, informatie over SAS-verificatie met Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a2760072acb7c62204759f3ec0d3cb9899460f2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Service Bus-verificatie met handtekeningen voor gedeelde toegang

*Shared Access Signatures* (SAS) zijn de primaire beveiligingsmechanisme voor Service Bus-berichtenservice. Dit artikel wordt beschreven SAS, hoe deze werken en hoe ze te gebruiken op een manier platform networkdirect.

SAS-verificatie kan toepassingen te verifiëren bij Service Bus met behulp van een toegangssleutel die is geconfigureerd op de naamruimte of op de Berichtentiteit (wachtrij of onderwerp) waaraan de specifieke rechten zijn gekoppeld. U kunt deze sleutel vervolgens gebruiken voor het genereren van een SAS-token die clients op hun beurt gebruiken kunnen om Service Bus te verifiëren.

Ondersteuning voor SAS-verificatie is opgenomen in de Azure SDK versie 2.0 en hoger.

## <a name="overview-of-sas"></a>Overzicht van de SAS

Handtekeningen voor gedeelde toegang zijn een verificatiemethode op basis van beveiligde SHA-256-hashes of URI's. SAS is een zeer krachtig mechanisme dat wordt gebruikt door alle Service Bus-services. In het werkelijke gebruik SAS bestaat uit twee onderdelen: een *gedeeld toegangsbeleid* en een *Shared Access Signature* (vaak genoemd een *token*).

SAS-verificatie in Service Bus omvat de configuratie van een cryptografische sleutel met de gekoppelde rechten van een Service Bus-resource. Clients claim toegang tot de Service Bus-resources in de vorm van een SAS-token. Dit token bestaat uit de bron-URI wordt geopend en een verloopdatum die zijn ondertekend met de geconfigureerde sleutel.

U kunt de Shared Access Signature-autorisatieregels configureren op Service Bus [doorstuurt](service-bus-fundamentals-hybrid-solutions.md#relays), [wachtrijen](service-bus-fundamentals-hybrid-solutions.md#queues), en [onderwerpen](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS verificatie maakt gebruik van de volgende elementen:

* [Gedeelde toegang autorisatieregel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): A 256-bits primaire cryptografische sleutel in Base64-weergave, een optionele secundaire sleutel en een sleutelnaam en gekoppelde rechten (een verzameling van *luisteren*, *verzenden*, of *beheren* rechten).
* [Shared Access Signature](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token: met het HMAC-SHA256 van een resourcetekenreeks die bestaat uit de URI van de resource die wordt geopend en een verloopbewerking met de cryptografische sleutel gegenereerd. De handtekening en andere elementen in de volgende secties wordt beschreven, worden opgemaakt in een tekenreeks om het SAS-token.

## <a name="shared-access-policy"></a>Beleid voor gedeelde toegang

Een belangrijkste leert over SAS is dat deze met een beleid begint. Voor elk beleid dat u besluit op drie soorten informatie: **naam**, **bereik**, en **machtigingen**. De **naam** is slechts; een unieke naam binnen dit bereik. Het bereik is eenvoudig genoeg: de URI van de betreffende resource is. Voor een Service Bus-naamruimte is het bereik de volledig gekwalificeerde domeinnaam (FQDN), zoals `https://<yournamespace>.servicebus.windows.net/`.

De beschikbare machtigingen voor een beleid zijn grotendeels spreken voor zich:

* Verzenden
* Luisteren
* Beheren

Nadat u het beleid hebt gemaakt, krijgt het een *primaire sleutel* en een *secundaire sleutel*. Dit zijn cryptografisch sterke sleutels. Geen verliezen ze of ze lekken - ze moet altijd beschikbaar zijn in de [Azure-portal][Azure portal]. U kunt een van de gegenereerde sleutels gebruiken en u ze op elk gewenst moment kunt opnieuw genereren. Als u opnieuw genereren of de primaire sleutel in het beleid wijzigt, worden er handtekeningen voor gedeelde toegang gemaakt op basis van het ongeldig.

Wanneer u een Service Bus-naamruimte maakt, een beleid wordt automatisch gemaakt voor de gehele naamruimte aangeroepen **RootManageSharedAccessKey**, en alle machtigingen voor dit beleid heeft. U niet aanmelden als **hoofdmap**, zodat dit beleid niet gebruiken tenzij er een goede reden. Kunt u extra beleidsregels in de **configureren** tabblad voor de naamruimte in de portal. Het is belangrijk te weten een niveau één tree in Service Bus (naamruimte, wachtrij, enz.) kan alleen gekoppeld aan het beleid voor maximaal 12 hebben.

## <a name="configuration-for-shared-access-signature-authentication"></a>Configuratie voor Shared Access Signature-verificatie
U kunt de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -regel op de Service Bus-naamruimten, wachtrijen en onderwerpen. Configureren van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) in een Service Bus abonnement wordt momenteel niet ondersteund, maar u kunt regels geconfigureerd op een naamruimte of het onderwerp gebruiken om toegang aan abonnementen, te beveiligen. Zie voor een werkende-voorbeeldtoepassing die u ziet u deze procedure, de [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus abonnementen](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) voorbeeld.

Maximaal 12 dergelijke regels kan worden geconfigureerd op een Service Bus-naamruimte, wachtrij of onderwerp. Regels die zijn geconfigureerd op een Service Bus-naamruimte van toepassing op alle entiteiten in waarmee de naamruimte.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

In deze afbeelding is de *manageRuleNS*, *sendRuleNS*, en *listenRuleNS* autorisatieregels van toepassing op wachtrij W1 en onderwerp T1, terwijl *listenRuleQ*  en *sendRuleQ* gelden alleen voor wachtrij W1 en *sendRuleT* geldt alleen voor onderwerp T1.

De belangrijkste parameters van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) zijn als volgt:

| Parameter | Beschrijving |
| --- | --- |
| *Sleutelnaam* |Een tekenreeks die de autorisatieregel beschrijft. |
| *PrimaryKey* |Een base64-gecodeerd 256-bits primaire sleutel voor ondertekening en valideren van de SAS-token. |
| *Secundaire sleutel* |Een base64-gecodeerd 256-bits secundaire sleutel voor ondertekening en valideren van de SAS-token. |
| *AccessRights* |Een lijst met rechten verleend door de autorisatieregel. Deze rechten kunnen worden van een verzameling van rechten luisteren, verzenden en beheren. |

Wanneer een Service Bus-naamruimte is ingericht, een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), met [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) ingesteld op **RootManageSharedAccessKey**, wordt standaard gemaakt.

## <a name="generate-a-shared-access-signature-token"></a>Genereren van een Shared Access Signature (token)

Het beleid zelf is niet het toegangstoken voor Service Bus. Het object op basis waarvan het toegangstoken is gegenereerd - met de primaire of secundaire sleutel is. De SAS-token kan worden gegenereerd door een client die toegang tot de ondertekeningssleutels die is opgegeven in de autorisatieregel voor gedeelde toegang heeft. Het token wordt gegenereerd door zorgvuldig een tekenreeks in de volgende indeling:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Waar `signature-string` is de SHA-256-hash van het bereik van het token (**bereik** zoals beschreven in de vorige sectie) met een CRLF toegevoegd en een verlooptijd (in seconden sinds de epoche: `00:00:00 UTC` op 1 januari 1970). 

> [!NOTE]
> Om te voorkomen dat een toegangstoken korte tijd, wordt het aanbevolen het coderen van de tijdwaarde verstrijken als ten minste een 32-bits geheel getal zonder teken of bij voorkeur een geheel getal zijn lange (64-bits).  
> 
> 

De hash lijkt op de volgende pseudocode en retourneert 32 bytes.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

De niet-hashing-waarden zijn de **SharedAccessSignature** string, zodat de ontvanger kan de hash met dezelfde parameters, om ervoor te zorgen dat deze als resultaat geeft hetzelfde resultaat berekenen. De URI geeft u het bereik en de sleutelnaam identificeert het beleid moet worden gebruikt voor het berekenen van de hash. Dit is belangrijk uit veiligheidsoverwegingen. Als de handtekening komt niet overeen met wat de ontvanger (Service Bus) wordt berekend, is toegang geweigerd. Op dit moment kunt u ervoor dat de afzender toegang had tot de sleutel en de rechten die zijn opgegeven in het beleid moet worden verleend zijn.

Houd er rekening mee dat u de gecodeerde bron-URI voor deze bewerking gebruiken moet. De bron-URI is de volledige URI van de Service Bus-bron waartoe toegang wordt geclaimd. Bijvoorbeeld: `http://<namespace>.servicebus.windows.net/<entityPath>` of `sb://<namespace>.servicebus.windows.net/<entityPath>`; dat wil zeggen, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

De autorisatieregel voor gedeelde toegang gebruikt voor de ondertekening moet worden geconfigureerd op de entiteit opgegeven door deze URI of door een van de hiërarchische bovenliggende items. Bijvoorbeeld: `http://contoso.servicebus.windows.net/contosoTopics/T1` of `http://contoso.servicebus.windows.net` in het vorige voorbeeld.

Een SAS-token is geldig voor alle resources onder de `<resourceURI>` gebruikt in de `signature-string`.

De [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) in de SAS token verwijst naar de **keyName** van de autorisatieregel voor gedeelde toegang gebruikt voor het genereren van het token.

De *URL-codering resourceURI* moet hetzelfde zijn als de URI die wordt gebruikt in de tekenreeks voor aanmelding bij de berekening van de handtekening. Deze moet [procent gecodeerd](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Het is raadzaam dat u de sleutels die worden gebruikt regelmatig opnieuw genereert de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object. Toepassingen in het algemeen moeten gebruikmaken van de [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) om een SAS-token te genereren. Wanneer de sleutels opnieuw genereren, vervangt u de [secundaire sleutel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) met de oude primaire sleutel en een nieuwe sleutel als de nieuwe primaire sleutel genereren. Hiermee kunt u doorgaan met het gebruik van tokens voor autorisatie die zijn uitgegeven met de oude primaire sleutel en die nog niet zijn verlopen.

Als een sleutel is geknoeid en u voor het intrekken van de sleutels hebt, kunt u beide genereren de [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) en de [secundaire sleutel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)en vervang ze met nieuwe sleutels. Deze procedure ongeldig alle tokens die zijn ondertekend met de oude sleutels.

## <a name="how-to-use-shared-access-signature-authentication-with-service-bus"></a>Het gebruik van Shared Access Signature-verificatie met Service Bus

De volgende scenario's zijn de configuratie van autorisatieregels, het genereren van SAS-tokens en Clientautorisatie.

Voor een volledige werkende voorbeeld van een Service Bus-toepassing die laat zien van de configuratie en maakt gebruik van SAS-autorisatie Zie [Shared Access Signature-verificatie met Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Een gerelateerde voorbeeldtoepassing die u ziet u het gebruik van SAS-autorisatieregels geconfigureerd voor naamruimten of onderwerpen voor het beveiligen van Service Bus-abonnementen is hier beschikbaar: [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus abonnementen](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Toegang tot gedeelde toegang autorisatieregels voor een naamruimte

Bewerkingen op basis van de Service Bus-naamruimte vereist verificatie via certificaat. U moet een beheercertificaat uploaden voor uw Azure-abonnement. Volg de stappen voor het uploaden van een beheercertificaat [hier](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate), waarbij de [Azure-portal][Azure portal]. Zie voor meer informatie over Azure-beheercertificaten de [overzicht van Azure-certificaten](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Het eindpunt voor toegang tot autorisatieregels voor gedeelde toegang voor een Service Bus-naamruimte is als volgt:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

Maken van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object op een Service Bus-naamruimte, voert u een POST-bewerking op dit eindpunt met de regelgegevens geserialiseerd als JSON of XML. Bijvoorbeeld:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Op dezelfde manier gebruiken een GET-bewerking op het eindpunt te lezen van de autorisatieregels die is geconfigureerd op de naamruimte.

Als u wilt bijwerken of verwijderen van een specifieke autorisatieregel, gebruikt u het volgende eindpunt:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Toegang tot gedeelde toegang autorisatieregels voor een entiteit

U hebt toegang tot een [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object is geconfigureerd voor een Service Bus-wachtrij of onderwerp via de [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) verzameling in de bijbehorende [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) of [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

De volgende code laat zien hoe de autorisatieregels voor een wachtrij toevoegen.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Shared Access Signature-verificatie gebruiken

Toepassingen met behulp van de Azure .NET SDK met de Service Bus .NET-bibliotheken kunnen gebruiken autorisatie via SAS de [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) klasse. De volgende code ziet u het gebruik van de tokenprovider om berichten te verzenden naar een Service Bus-wachtrij.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Toepassingen kunnen ook SAS gebruiken voor verificatie met behulp van een SAS-verbindingsreeks in de methoden die verbindingsreeksen accepteren.

Houd er rekening mee dat voor het gebruik van SAS-autorisatie met Service Bus relays, kunt u SAS-sleutels die zijn geconfigureerd op de Service Bus-naamruimte. Als u expliciet een relay op de naamruimte maakt ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) met een [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription))-object, kunt u de SAS-regels alleen voor die relay instellen. Als u wilt gebruiken autorisatie SAS met Service Bus-abonnementen, kunt u SAS-sleutels die zijn geconfigureerd op een Service Bus-naamruimte of op een onderwerp.

## <a name="use-the-shared-access-signature-at-http-level"></a>Gebruik de Shared Access Signature (op http-niveau)

Nu u weet hoe handtekeningen voor gedeelde toegang maken voor alle entiteiten in Service Bus, bent u klaar voor het uitvoeren van een HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Denk eraan dat dit werkt voor alles. U kunt een SAS maken voor een wachtrij, onderwerp of abonnement. 

U geeft een afzender of de client een SAS-token, hebben niet de sleutel rechtstreeks als ze niet de hash voor het ongedaan maken. Hierdoor hebt u controle over wat ze kunnen openen en hoe lang. Een belangrijkste te onthouden is als u de primaire sleutel in het beleid wijzigt, wordt alle handtekeningen voor gedeelde toegang gemaakt op basis van het ongeldig.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Gebruik de Shared Access Signature (op AMQP niveau)

In de vorige sectie hebt u gezien hoe u de SAS-token met een HTTP POST-aanvraag voor het verzenden van gegevens naar de Service Bus. Als u weet, kunt u Service Bus met behulp van de geavanceerde Message Queuing-Protocol (AMQP) die het voorkeursprotocol voor gebruik voor betere prestaties in veel scenario's te openen. Het SAS-token gebruik met AMQP wordt beschreven in het document [AMQP Claim-Based Security versie 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) die in werkende concept sinds 2013 maar goed ondersteund door Azure vandaag.

Voordat u begint met het verzenden van gegevens naar Service Bus, is de uitgever moet het SAS-token binnen een AMQP-bericht te verzenden naar een goed gedefinieerde AMQP-knooppunt met de naam **$cbs** (u kunt zien als een 'speciale' wachtrij gebruikt door de service te verkrijgen en valideren van de SAS tokens). De uitgever moet opgeven de **ReplyTo** veld in het bericht AMQP; dit is het knooppunt waarin antwoorden op de service met de publisher met het resultaat van de validatie van tokens (een eenvoudige aanvraag/antwoordbewerking patroon tussen publisher en de service ). Dit antwoord-knooppunt wordt gemaakt 'onderweg,' over 'dynamische maken van het externe knooppunt' spreken, zoals beschreven in de AMQP 1.0-specificatie. Nadat u hebt gecontroleerd of het SAS-token geldig is, de uitgever vooruit en gegevens verzenden naar de service start.

De volgende stappen ziet u hoe u de SAS-token met het gebruik van een AMQP-protocol verzendt de [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) bibliotheek. Dit is handig als u niet de officiële Service Bus-SDK gebruiken (bijvoorbeeld op WinRT, .net Compact Framework, .net Framework Micro en Mono) ontwikkeling in C\#. Deze bibliotheek is natuurlijk nuttig voor meer informatie over hoe op claims gebaseerde beveiliging op het niveau van AMQP werkt zoals u hebt gezien hoe het werkt op het niveau van HTTP (met een HTTP POST-aanvraag en de SAS-token verzonden binnen de koptekst 'Verificatie'). Als u niet grondige kennis over AMQP hoeft, kunt u de officiële Service Bus-SDK gebruiken met .net Framework-toepassingen, wat het voor u doen.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

De `PutCbsToken()` methode ontvangt de *verbinding* (AMQP verbinding klasse-instantie worden geleverd door de [AMQP .NET Lite-bibliotheek](https://github.com/Azure/amqpnetlite)) die staat voor de TCP-verbinding naar de service en de *sasToken* parameter die de SAS-token om te verzenden. 

> [!NOTE]
> Het is belangrijk dat de verbinding is gemaakt met **SASL verificatiemechanisme instellen op externe** (en niet de standaard zonder opmaak met gebruikersnaam en wachtwoord dat wordt gebruikt wanneer u hoeft niet te verzenden van het SAS-token).
> 
> 

Vervolgens maakt de uitgever van de twee AMQP koppelingen voor de SAS-token verzenden en ontvangen van het antwoord (het resultaat van de validatie van tokens) van de service.

Het AMQP-bericht bevat een set eigenschappen en meer informatie dan een eenvoudige bericht. De SAS-token is de hoofdtekst van het bericht (met behulp van de constructor). De **'ReplyTo'** eigenschap is ingesteld op de naam van het knooppunt voor het ontvangen van het validatieresultaat op de koppeling van de ontvanger (u kunt de naam wijzigen als u wilt en het dynamisch door de service gemaakt wordt). De laatste drie toepassing/aangepaste eigenschappen worden gebruikt door de service om aan te geven wat voor soort bewerking niet uitvoeren. Zoals beschreven in de ontwerp-specificatie van CBS, moeten ze de **bewerkingsnaam** ('put-token'), de **type token** (in dit geval een ' servicebus.windows.net:sastoken'), en de **'naam' van het de doelgroep** waarop het token van toepassing is (de hele entiteit).

Na het verzenden van het SAS-token op de koppeling van de afzender, moet de uitgever lezen van het antwoord op de koppeling van de ontvanger. Het antwoord is een eenvoudige AMQP-bericht met de eigenschap van een toepassing met de naam **'statuscode'** die dezelfde waarden als een HTTP-statuscode kan bevatten.

## <a name="rights-required-for-service-bus-operations"></a>Rechten die vereist zijn voor Service Bus-bewerkingen

De volgende tabel toont de toegangsrechten nodig zijn voor verschillende bewerkingen voor Service Bus-resources.

| Bewerking | Claim vereist | Claim-bereik |
| --- | --- | --- |
| **Namespace** | | |
| Verificatieregel configureren op een naamruimte |Beheren |Een naamruimte-adres |
| **Service-register** | | |
| Beleid voor persoonlijke opsommen |Beheren |Een naamruimte-adres |
| Beginnen met luisteren op een naamruimte |Luisteren |Een naamruimte-adres |
| Berichten verzenden naar een listener op een naamruimte |Verzenden |Een naamruimte-adres |
| **Wachtrij** | | |
| Een wachtrij maken |Beheren |Een naamruimte-adres |
| Een wachtrij verwijderen |Beheren |Geldige wachtrij-adres |
| Wachtrijen opsommen |Beheren |Map resources/wachtrijen |
| De beschrijving van de wachtrij ophalen |Beheren |Geldige wachtrij-adres |
| Autorisatieregel voor een wachtrij configureren |Beheren |Geldige wachtrij-adres |
| In naar de wachtrij verzenden |Verzenden |Geldige wachtrij-adres |
| Berichten ontvangen uit een wachtrij |Luisteren |Geldige wachtrij-adres |
| Afbreken of volledige berichten na de ontvangst van het bericht in de modus peek vergrendelen |Luisteren |Geldige wachtrij-adres |
| Een bericht voor later gebruik stellen |Luisteren |Geldige wachtrij-adres |
| Wachtrij voor onbestelbare een bericht |Luisteren |Geldige wachtrij-adres |
| De status is gekoppeld aan een sessie van de wachtrij bericht ophalen |Luisteren |Geldige wachtrij-adres |
| De status is gekoppeld aan een sessie van de wachtrij bericht instellen |Luisteren |Geldige wachtrij-adres |
| **Onderwerp** | | |
| Een onderwerp maken |Beheren |Een naamruimte-adres |
| Een onderwerp verwijderen |Beheren |Elk adres ongeldig onderwerp |
| Onderwerpen over opsommen |Beheren |Resources: map/onderwerpen |
| De beschrijving van het onderwerp ophalen |Beheren |Elk adres ongeldig onderwerp |
| Autorisatieregel voor een onderwerp configureren |Beheren |Elk adres ongeldig onderwerp |
| Verzenden naar het onderwerp |Verzenden |Elk adres ongeldig onderwerp |
| **Abonnement** | | |
| Een abonnement maken |Beheren |Een naamruimte-adres |
| Abonnement verwijderen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Abonnementen opsommen |Beheren |.. / myTopic/abonnementen |
| Beschrijving van het abonnement ophalen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Afbreken of volledige berichten na de ontvangst van het bericht in de modus peek vergrendelen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| Een bericht voor later gebruik stellen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| Wachtrij voor onbestelbare een bericht |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| De status is gekoppeld aan een sessie onderwerp ophalen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| De status is gekoppeld aan een sessie voor onderwerp instellen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| **Regels** | | |
| Een regel maken |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Een regel verwijderen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Regels opsommen |Beheren of luisteren |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Service Bus-wachtrijen gebruiken](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com