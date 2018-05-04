---
title: Azure Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang | Microsoft Docs
description: Overzicht van Service Bus-toegangsbeheer met het overzicht van handtekeningen voor gedeelde toegang, informatie over SAS-verificatie met Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: sethm
ms.openlocfilehash: 420f4573fbe8b5139a4e1e5fa4dea3404c4e099d
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang

*Shared Access Signatures* (SAS) zijn de primaire beveiligingsmechanisme voor Service Bus-berichtenservice. Dit artikel wordt beschreven SAS, hoe deze werken en hoe ze te gebruiken op een manier platform networkdirect.

SAS beschermt toegang tot de Service Bus op basis van autorisatieregels. Die zijn geconfigureerd op een naamruimte, of op een Berichtentiteit (relay, wachtrij of onderwerp). Een autorisatieregel heeft een naam, is gekoppeld aan specifieke rechten en voert een paar cryptografische sleutels. Met de naam van de regel en de sleutel via de Service Bus-SDK of in uw eigen code kunt u een SAS-token te genereren. Een client kan het token vervolgens doorgegeven aan Service Bus om te bewijzen autorisatie voor de aangevraagde bewerking.

## <a name="overview-of-sas"></a>Overzicht van de SAS

Handtekeningen voor gedeelde toegang zijn een op claims gebaseerde autorisatiemechanismen eenvoudige tokens gebruiken. Met SAS kunt worden sleutels nooit doorgegeven de kabel. Sleutels worden gebruikt voor het ondertekenen van cryptografisch informatie die later kan worden gecontroleerd door de service. SAS kan worden gebruikt vergelijkbaar met een gebruikersnaam en wachtwoord schema waar de client zich bevindt in de onmiddellijke bezit is van een naam van de regel voor autorisatie en een overeenkomende sleutel. SAS kan ook worden gebruikt vergelijkbaar met een federatieve beveiligingsmodel waarin de client een tijdelijke en ondertekende toegangstoken ontvangt van een service voor beveiligingstokens zonder ooit in bezit is van de ondertekeningssleutel binnenkort.

SAS-verificatie in Service Bus is geconfigureerd met de naam [autorisatieregels voor gedeelde toegang](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) hebben die zijn gekoppeld aan een combinatie van primaire en secundaire cryptografische sleutels en rechten. De sleutels zijn 256-bits waarden in Base64-weergave. Kunt u regels configureren op het niveau van de naamruimte in Service Bus [doorstuurt](service-bus-fundamentals-hybrid-solutions.md#relays), [wachtrijen](service-bus-fundamentals-hybrid-solutions.md#queues), en [onderwerpen](service-bus-fundamentals-hybrid-solutions.md#topics).

De [Shared Access Signature](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token bevat de naam van de gekozen autorisatieregel de URI van de resource die moet worden geopend, een directe, verlopen en een cryptografische handtekening voor HMAC SHA256 berekend over deze velden met de primaire of secundaire cryptografiesleutel van de van de gekozen autorisatieregel.

## <a name="shared-access-authorization-policies"></a>Autorisatiebeleid voor gedeelde toegang

Elke Service Bus-naamruimte en elke Service Bus-entiteit heeft een Shared Access autorisatiebeleid bestaat uit regels. Het beleid op het niveau van de naamruimte is van toepassing op alle entiteiten in de naamruimte, ongeacht hun afzonderlijke beleidsconfiguratie.

Voor elke beleidsregel autorisatie u besluit op drie soorten informatie: **naam**, **bereik**, en **rechten**. De **naam** is slechts; een unieke naam binnen dit bereik. Het bereik is eenvoudig genoeg: de URI van de betreffende resource is. Voor een Service Bus-naamruimte is het bereik de volledig gekwalificeerde domeinnaam (FQDN), zoals `https://<yournamespace>.servicebus.windows.net/`.

De rechten die door de beleidsregel kunnen bestaan uit een combinatie van:

* 'Verzenden' - verleent het recht om berichten te verzenden naar de entiteit
* 'Luisteren' - verleent het recht om te luisteren (relais) of ontvangen (wachtrij, abonnementen) en alle bijbehorende afhandeling van berichten
* 'Beheren' - verleent het recht voor het beheren van de topologie van de naamruimte, inclusief het maken en verwijderen van entiteiten

Het recht 'Beheren' bevat de rechten 'Verzenden' en 'Ontvangen'.

Een beleid naamruimte of entiteit kan maximaal 12 gedeelde toegang autorisatieregels ruimte voor drie sets met regels, die elk die betrekking hebben op de fundamentele rechten en de combinatie van verzenden en luisteren bevatten. Deze limiet onderstrepingen die de SAS-beleidsarchief is niet bedoeld voor een gebruiker of service-account store. Als uw toepassing moet om toegang te verlenen aan Service Bus op basis van gebruiker of service-identiteiten, moet deze een service voor beveiligingstokens die SAS-tokens na een controle voor verificatie en toegang uitgeeft implementeert.

Een autorisatieregel is toegewezen een *primaire sleutel* en een *secundaire sleutel*. Dit zijn cryptografisch sterke sleutels. Geen verliezen ze of ze lekken - ze moet altijd beschikbaar zijn in de [Azure-portal][Azure portal]. U kunt een van de gegenereerde sleutels gebruiken en u ze op elk gewenst moment kunt opnieuw genereren. Als u opnieuw genereren of wijzigen van een sleutel in het beleid, uitgegeven alle eerder tokens op basis van die sleutel onmiddellijk ongeldig geworden. Actieve verbindingen gemaakt op basis van deze tokens blijven echter werken totdat het token is verlopen.

Wanneer u een Service Bus-naamruimte maakt, er een beleidsregel met de naam **RootManageSharedAccessKey** wordt automatisch gemaakt voor de naamruimte. Dit beleid heeft machtigingen beheren voor de gehele naamruimte. Het raadzaam dat u deze regel als een administratieve behandelen **hoofdmap** account en niet gebruiken in uw toepassing. U kunt extra beleidsregels maken in de **configureren** tabblad voor de naamruimte in de portal via Powershell of Azure CLI.

## <a name="configuration-for-shared-access-signature-authentication"></a>Configuratie voor Shared Access Signature-verificatie

U kunt de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -regel op de Service Bus-naamruimten, wachtrijen en onderwerpen. Configureren van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) in een Service Bus abonnement wordt momenteel niet ondersteund, maar u kunt regels geconfigureerd op een naamruimte of het onderwerp gebruiken om toegang aan abonnementen, te beveiligen. Zie voor een werkende-voorbeeldtoepassing die u ziet u deze procedure, de [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus abonnementen](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) voorbeeld.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

In deze afbeelding is de *manageRuleNS*, *sendRuleNS*, en *listenRuleNS* autorisatieregels van toepassing op wachtrij W1 en onderwerp T1, terwijl *listenRuleQ*  en *sendRuleQ* gelden alleen voor wachtrij W1 en *sendRuleT* geldt alleen voor onderwerp T1.

## <a name="generate-a-shared-access-signature-token"></a>Een Shared Access Signature-token genereren

Een SAS-token kunnen worden gegenereerd door een client die toegang tot de naam van een regel voor autorisatie heeft en een van de handtekeningsleutels. Het token wordt gegenereerd door een tekenreeks in de volgende indeling:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`** -Toegangstoken chatberichten. Geheel getal als gevolg van seconden sinds de epoche `00:00:00 UTC` op 1 januari 1970 (UNIX-epoche) wanneer het token is verlopen.
* **`skn`** -De naam van de autorisatieregel.
* **`sr`** -URI van de resource wordt geopend.
* **`sig`** -Handtekening.

De `signature-string` de SHA-256-hash wordt berekend op de bron-URI (**bereik** zoals beschreven in de vorige sectie) en de tekenreeksweergave van het toegangstoken chatberichten, gescheiden door CRLF.

De berekening hash lijkt op de volgende pseudocode en retourneert een 256-bits/32-byte-hash-waarde.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Het token bevat de waarden niet gehasht, zodat de ontvanger van de hash met dezelfde parameters herberekenen kan, controleren of de uitgever in bezit is van een geldige handtekeningsleutel. 

De bron-URI is de volledige URI van de Service Bus-bron waartoe toegang wordt geclaimd. Bijvoorbeeld: `http://<namespace>.servicebus.windows.net/<entityPath>` of `sb://<namespace>.servicebus.windows.net/<entityPath>`; dat wil zeggen, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. De URI moet [procent gecodeerd](https://msdn.microsoft.com/library/4fkewx0t.aspx).

De autorisatieregel voor gedeelde toegang gebruikt voor de ondertekening moet worden geconfigureerd op de entiteit opgegeven door deze URI of door een van de hiërarchische bovenliggende items. Bijvoorbeeld: `http://contoso.servicebus.windows.net/contosoTopics/T1` of `http://contoso.servicebus.windows.net` in het vorige voorbeeld.

Een SAS-token is geldig voor alle resources die worden voorafgegaan door de `<resourceURI>` gebruikt in de `signature-string`.

## <a name="regenerating-keys"></a>Opnieuw genereren van sleutels

Het is raadzaam dat u de sleutels die worden gebruikt regelmatig opnieuw genereert de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object. De primaire en secundaire sleutel sleuven bestaan zodat u kunt sleutels geleidelijk draaien. Als uw toepassing wordt doorgaans gebruikt voor de primaire sleutel, kunt u Kopieer de primaire sleutel in de secundaire sleutel sleuf en vervolgens alleen de primaire sleutel opnieuw genereren. De nieuwe primaire sleutelwaarde kan vervolgens worden geconfigureerd in de clienttoepassingen die voortdurend met de oude primaire sleutel in de secundaire site toegang hebben. Wanneer alle clients zijn bijgewerkt, kunt u de secundaire sleutel om in te trekken ten slotte de oude primaire sleutel opnieuw genereren.

Als u weet of veronderstelt dat een sleutel is geknoeid en u voor het intrekken van de sleutels hebt, kunt u beide genereren de [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) en de [secundaire sleutel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), vervangen door nieuwe sleutels. Deze procedure ongeldig alle tokens die zijn ondertekend met de oude sleutels.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Shared Access Signature-verificatie met Service Bus

De scenario's beschreven als volgt zijn configuratie van autorisatieregels, het genereren van SAS-tokens en Clientautorisatie.

Voor een volledige werkende voorbeeld van een Service Bus-toepassing die laat zien van de configuratie en maakt gebruik van SAS-autorisatie Zie [Shared Access Signature-verificatie met Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Een gerelateerde voorbeeldtoepassing die u ziet u het gebruik van SAS-autorisatieregels geconfigureerd voor naamruimten of onderwerpen voor het beveiligen van Service Bus-abonnementen is hier beschikbaar: [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus abonnementen](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Toegang tot gedeelde toegang autorisatieregels voor een entiteit

Met Service Bus .NET Framework-bibliotheken u toegang hebt tot een [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object is geconfigureerd voor een Service Bus-wachtrij of onderwerp via de [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) verzameling in het bijbehorende [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) of [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

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

Toepassingen met behulp van de Azure .NET SDK met de Service Bus .NET-bibliotheken kunnen gebruiken autorisatie via SAS de [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) klasse. De volgende code ziet u het gebruik van de tokenprovider om berichten te verzenden naar een Service Bus-wachtrij. Alternatief voor het gebruik die hier worden weergegeven, dat kunt u ook een eerder uitgegeven token doorgeven aan de tokenprovider fabrieksmethode.

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

U kunt ook de tokenprovider gebruiken rechtstreeks voor het uitgeven van tokens moeten worden doorgegeven aan andere clients. 

Verbindingsreeksen kunnen bestaan uit de regelnaam van een (*SharedAccessKeyName*) en de sleutel van de regel (*SharedAccessKey*) of een eerder uitgegeven token (*SharedAccessSignature*). Wanneer die aanwezig in de verbindingsreeks die is doorgegeven aan een constructor of fabrieksmethode een verbindingsreeks worden geaccepteerd zijn, wordt de SAS-tokenprovider automatisch gemaakt en ingevuld.

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

U geeft een afzender of de client een SAS-token, hebben niet de sleutel rechtstreeks als ze niet de hash voor het ongedaan maken. Hierdoor hebt u controle over wat ze kunnen openen en hoe lang. Een belangrijkste te onthouden is dat als u de primaire sleutel in het beleid wijzigt, alle handtekeningen voor gedeelde toegang gemaakt op basis hiervan zijn ongeldig.

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
> Het is belangrijk dat de verbinding is gemaakt met **SASL verificatiemechanisme ingesteld op anoniem** (en niet de standaard zonder opmaak met gebruikersnaam en wachtwoord dat wordt gebruikt wanneer u hoeft niet te verzenden van het SAS-token).
> 
> 

Vervolgens maakt de uitgever van de twee AMQP koppelingen voor de SAS-token verzenden en ontvangen van het antwoord (het resultaat van de validatie van tokens) van de service.

Het AMQP-bericht bevat een set eigenschappen en meer informatie dan een eenvoudige bericht. De SAS-token is de hoofdtekst van het bericht (met behulp van de constructor). De **'ReplyTo'** eigenschap is ingesteld op de naam van het knooppunt voor het ontvangen van het validatieresultaat op de koppeling van de ontvanger (u kunt de naam wijzigen als u wilt en het dynamisch door de service gemaakt wordt). De laatste drie toepassing/aangepaste eigenschappen worden gebruikt door de service om aan te geven wat voor soort bewerking niet uitvoeren. Zoals beschreven in de ontwerp-specificatie van CBS, moeten ze de **bewerkingsnaam** ('put-token'), de **type token** (in dit geval een `servicebus.windows.net:sastoken`), en de **'naam' van de doelgroep** waarop het token van toepassing is (de hele entiteit).

Na het verzenden van het SAS-token op de koppeling van de afzender, moet de uitgever lezen van het antwoord op de koppeling van de ontvanger. Het antwoord is een eenvoudige AMQP-bericht met de eigenschap van een toepassing met de naam **'statuscode'** die dezelfde waarden als een HTTP-statuscode kan bevatten.

## <a name="rights-required-for-service-bus-operations"></a>Rechten die vereist zijn voor Service Bus-bewerkingen

De volgende tabel toont de toegangsrechten nodig zijn voor verschillende bewerkingen voor Service Bus-resources.

| Bewerking | Claim vereist | Claim-bereik |
| --- | --- | --- |
| **naamruimte** | | |
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
| Plannen van een bericht voor latere levering; bijvoorbeeld: [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Luisteren | Geldige wachtrij-adres
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