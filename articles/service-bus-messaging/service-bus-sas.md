---
title: Azure Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang | Microsoft Docs
description: Overzicht van Service Bus-toegangsbeheer met het overzicht van handtekeningen voor gedeelde toegang, informatie over SAS autorisatie met Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2018
ms.author: spelluru
ms.openlocfilehash: daefb07761217ff4bb0800dfd9f1f05b6e22c1e1
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284911"
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang

*Shared Access Signatures* (SAS) zijn een beveiligingsmechanisme voor de primaire voor Service Bus-berichten. Dit artikel worden besproken SAS, hoe deze werken en hoe u deze kunt gebruiken in een platform-agnostische manier.

SAS biedt toegang tot Service Bus op basis van regels voor bescherming. Die zijn geconfigureerd op een naamruimte of een Berichtentiteit (relay, wachtrij of onderwerp). Een autorisatieregel heeft een naam, is gekoppeld aan specifieke rechten, en een combinatie van een cryptografische sleutel heeft. U de naam van de regel en de sleutel via de Service Bus-SDK of in uw eigen code gebruiken om een SAS-token te genereren. Een client kan het token vervolgens doorgeven met Service Bus om te bewijzen dat de autorisatie voor de aangevraagde bewerking.

## <a name="overview-of-sas"></a>Overzicht van SAS

Shared Access Signatures zijn een autorisatiemechanisme voor op basis van claims met behulp van eenvoudige-tokens. Met behulp van SAS, sleutels nooit doorgegeven op de kabel. Sleutels worden cryptografisch om informatie te ondertekenen die later kan worden geverifieerd door de service gebruikt. SAS kan worden gebruikt die vergelijkbaar is met een gebruikersnaam en wachtwoord schema waar de client deel uitmaakt van onmiddellijke bezit is van een regelnaam autorisatie en een overeenkomende sleutel. SAS kan ook worden gebruikt die vergelijkbaar is met een federatieve beveiligingsmodel, waar de client een beperkte tijd en ondertekende toegangstoken ontvangt van een service voor beveiligingstokens zonder ooit in bezit is van de ondertekeningssleutel die afkomstig zijn.

SAS-verificatie in Service Bus is geconfigureerd met de naam [autorisatieregels voor gedeelde toegang](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) hebben die zijn gekoppeld aan toegangsrechten en een combinatie van primaire en secundaire cryptografische sleutels. De sleutels zijn 256-bits waarden in Base 64-indeling. U kunt regels configureren op het niveau van de naamruimte voor Service Bus [relays](../service-bus-relay/relay-what-is-it.md), [wachtrijen](service-bus-messaging-overview.md#queues), en [onderwerpen](service-bus-messaging-overview.md#topics).

De [Shared Access Signature](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token bevat de naam van de gekozen autorisatieregel, de URI van de resource die moet worden geopend, een directe, verlopen en een cryptografische handtekening voor HMAC-SHA256 berekend voor deze velden gebruiken de primaire of de secundaire cryptografische sleutel van de gekozen autorisatieregel.

## <a name="shared-access-authorization-policies"></a>Beleid voor gedeelde toegang autorisatie

Elke Service Bus-naamruimte en elke Service Bus-entiteit heeft een beleid voor gedeelde toegangsautorisatie bestaat uit regels. Het beleid op het niveau van de naamruimte is van toepassing op alle entiteiten in de naamruimte, ongeacht hun afzonderlijke beleidsconfiguratie.

Voor elke beleidsregel autorisatie u besluit op drie soorten informatie: **naam**, **bereik**, en **rechten**. De **naam** precies dat; is een unieke naam binnen dat bereik. Het bereik is simpel: dit is de URI van de betreffende resource. Voor een Service Bus-naamruimte, is het bereik de volledig gekwalificeerde domeinnaam (FQDN), zoals `https://<yournamespace>.servicebus.windows.net/`.

De rechten die door de beleidsregel kunnen bestaan uit een combinatie van:

* 'Verzenden' - biedt het recht om berichten te verzenden naar de entiteit
* 'Listen' - biedt het recht om te luisteren (relais) of ontvangen (wachtrij, abonnementen) en alle gerelateerde afhandeling van berichten
* 'Beheren' - het recht voor het beheren van de topologie van de naamruimte, inclusief het maken en verwijderen van instanties biedt

Het recht 'Beheren' bevat de rechten 'Verzenden' en 'Ontvangen'.

Een beleid voor de naamruimte of entiteit kan maximaal 12 gedeelde toegang tot autorisatieregels, ruimte voor drie sets met regels, die elk die betrekking hebben op de fundamentele rechten en de combinatie van verzenden en luisteren naar bevatten. Deze limiet onderstrepingen waarin de SAS-beleid is niet bedoeld voor een gebruiker of service-account opslaan. Als uw toepassing nodig heeft om toegang te verlenen aan de Service Bus op basis van gebruiker of service-identiteiten, moet deze een service voor beveiligingstokens die problemen met SAS-tokens na een controle-verificatie en toegangsbeheer implementeren.

Een autorisatieregel is toegewezen een *primaire sleutel* en een *secundaire sleutel*. Dit zijn cryptografisch sterke sleutels. Niet ze verloren gaan of ze lekken - ze altijd zijn beschikbaar in de [Azure-portal][Azure portal]. U kunt een van de gegenereerde sleutels gebruiken en u ze op elk gewenst moment kunt opnieuw genereren. Als u opnieuw genereren of een sleutel in het beleid wijzigt, alle eerder dat is uitgegeven tokens op basis van die sleutel onmiddellijk ongeldig geworden. Actieve verbindingen gemaakt op basis van deze tokens blijven echter werken totdat het token is verlopen.

Wanneer u een Service Bus-naamruimte hebt gemaakt, een beleidsregel met de naam **RootManageSharedAccessKey** voor de naamruimte wordt automatisch gemaakt. Dit beleid heeft machtigingen voor de gehele naamruimte beheren. Het wordt aanbevolen dat u net zo deze regel, zoals een administratieve behandelt **hoofdmap** en dit niet gebruiken in uw toepassing. Kunt u extra regels in de **configureren** tabblad voor de naamruimte in de portal via Powershell of Azure CLI.

## <a name="configuration-for-shared-access-signature-authentication"></a>Configuratie voor de Shared Access Signature-verificatie

U kunt configureren dat de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -regel op Service Bus-naamruimten, wachtrijen of onderwerpen. Configureren van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) in een Service Bus abonnement wordt momenteel niet ondersteund, maar u kunt regels die zijn geconfigureerd in een naamruimte of onderwerp gebruiken voor beveiligde toegang tot abonnementen. Zie voor een voorbeeld van werken die deze procedure laat zien, de [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus-abonnementen](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) voorbeeld.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

In deze afbeelding is de *manageRuleNS*, *sendRuleNS*, en *listenRuleNS* autorisatieregels van toepassing op zowel Q1 wachtrij en onderwerp T1, terwijl *listenRuleQ*  en *sendRuleQ* gelden alleen voor wachtrij Q1 en *sendRuleT* geldt alleen voor onderwerp T1.

## <a name="generate-a-shared-access-signature-token"></a>Een Shared Access Signature-token genereren

Een willekeurige client die toegang tot de naam van een regel voor autorisatie heeft en een van de ondertekeningssleutels kunnen een SAS-token genereren. Het token wordt gegenereerd door het samenstellen van een tekenreeks in de volgende indeling:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`** -Directe token verlopen. Geheel getal zijn dat weergeeft seconden sinds de epoche `00:00:00 UTC` op 1 januari 1970 (UNIX-epoche) wanneer het token is verlopen.
* **`skn`** : De naam van de autorisatieregel.
* **`sr`** -URI van de resource waartoe toegang wordt verkregen.
* **`sig`** -Handtekening.

De `signature-string` is de SHA-256-hash berekend voor de resource-URI (**bereik** zoals beschreven in de vorige sectie) en de tekenreeksweergave van de vervaldatum van token instant, gescheiden door CRLF.

De berekening van de hash op de volgende pseudocode lijkt en retourneert een 256-bits/32-byte-hash-waarde.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Het token bevat de waarden niet gehasht, zodat de ontvanger kan opnieuw van de hash met dezelfde parameters, gecontroleerd of de uitgever in het bezit is van een geldige ondertekeningssleutel is. 

De resource-URI is de volledige URI van de Service Bus-resource waaraan toegang is aangevraagd. Bijvoorbeeld, `http://<namespace>.servicebus.windows.net/<entityPath>` of `sb://<namespace>.servicebus.windows.net/<entityPath>`; dat wil zeggen, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. De URI moet [procent gecodeerde](https://msdn.microsoft.com/library/4fkewx0t.aspx).

De verificatieregel voor gedeelde toegang die wordt gebruikt voor het ondertekenen van moet worden geconfigureerd op de entiteit die door deze URI of door een van de hiërarchische bovenliggende items hebt opgegeven. Bijvoorbeeld, `http://contoso.servicebus.windows.net/contosoTopics/T1` of `http://contoso.servicebus.windows.net` in het vorige voorbeeld.

Een SAS-token is geldig voor alle resources die worden voorafgegaan door de `<resourceURI>` gebruikt in de `signature-string`.

## <a name="regenerating-keys"></a>Sleutels opnieuw genereren

Het is raadzaam dat u de sleutels die worden gebruikt regelmatig opnieuw genereert de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object. De primaire en secundaire sleutel sleuven bestaan zodat u kunt sleutels geleidelijk draaien. Als uw toepassing wordt doorgaans gebruikt voor de primaire sleutel, kunt u de primaire sleutel kopiëren naar de secundaire sleutel sleuf, en vervolgens alleen de primaire sleutel opnieuw genereren. De nieuwe waarde voor de primaire sleutel kan vervolgens worden geconfigureerd in de clienttoepassingen, die voortdurend toegang met behulp van de oude primaire sleutel in de secundaire site hebben. Als alle clients worden bijgewerkt, kunt u de secundaire sleutel voor het uiteindelijk buiten gebruik stellen van de oude primaire sleutel opnieuw genereren.

Als u weet of u vermoedt dat een sleutel is geïnfecteerd en wordt u om in te trekken van de sleutels hebt, kunt u opnieuw te genereren beide de [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) en de [secundaire sleutel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) van een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), deze overal vervangen door nieuwe sleutels. Deze procedure worden alle tokens die zijn ondertekend met de oude sleutels ongeldig.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Shared Access Signature-verificatie met Service Bus

De scenario's beschreven als volgt zijn de configuratie van autorisatieregels, het genereren van SAS-tokens en Clientautorisatie.

Voor een volledig werkend voorbeeld van een Service Bus-toepassing die laat zien van de configuratie en maakt gebruik van SAS-autorisatie Zie [Shared Access Signature-verificatie met Service Bus](https://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Een gerelateerde voorbeeld illustreert het gebruik van SAS-autorisatieregels geconfigureerd op de naamruimten of -onderwerpen voor het beveiligen van Service Bus-abonnementen is hier beschikbaar: [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus-abonnementen](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Machtigingen voor gedeelde toegang toegangsregels voor een entiteit

Met Service Bus .NET Framework-bibliotheken, kunt u toegang tot een [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -object is geconfigureerd op een Service Bus-wachtrij of onderwerp via de [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) verzameling in de bijbehorende [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) of [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

De volgende code laat zien hoe om toe te voegen autorisatieregels voor een wachtrij.

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

Toepassingen met behulp van de Azure .NET SDK met de Service Bus .NET-bibliotheken kunnen gebruikmaken van SAS-autorisatie via de [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) klasse. De volgende code illustreert het gebruik van de tokenprovider berichten te verzenden naar een Service Bus-wachtrij. Alternatief voor het gebruik die hier worden weergegeven, dat kunt u ook een eerder uitgegeven tokens doorgeven aan de tokenprovider factory-methode.

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

U kunt ook de tokenprovider rechtstreeks voor het uitgeven van tokens gebruiken om door te geven aan andere clients. 

Verbindingsreeksen kunnen bestaan uit een regelnaam (*SharedAccessKeyName*) en de sleutel van de regel (*SharedAccessKey*) of een eerder uitgegeven tokens (*SharedAccessSignature*). Wanneer die aanwezig in de verbindingsreeks die is doorgegeven aan een constructor of factory methode accepteert een verbindingsreeks zijn, wordt de SAS-tokenprovider automatisch gemaakt en ingevuld.

Houd er rekening mee dat voor het gebruik van SAS-autorisatie met Service Bus relays, kunt u SAS-sleutels die zijn geconfigureerd op de Service Bus-naamruimte. Als u expliciet een relay in de naamruimte maakt ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) met een [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription))-object, kunt u de SAS-regels alleen voor de desbetreffende relay instellen. Voor het gebruik van SAS-autorisatie met Service Bus-abonnementen, kunt u SAS-sleutels die zijn geconfigureerd op een Service Bus-naamruimte of op een onderwerp.

## <a name="use-the-shared-access-signature-at-http-level"></a>Gebruik de Shared Access Signature (op het niveau van de HTTP)

Nu u weet hoe handtekeningen voor gedeelde toegang maken voor alle entiteiten in Service Bus, bent u klaar voor het uitvoeren van een HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Denk eraan dat dit voor van alles werkt. U kunt een SAS maken voor een wachtrij, onderwerp of abonnement.

Als u een afzender of een client een SAS-token, ze hoeft de sleutel rechtstreeks, en ze niet de hash voor het ongedaan maken. Zo hebt u controle over wat ze toegang krijgen tot, en hoe lang. Een belangrijke dingen om te onthouden is dat als u de primaire sleutel in het beleid wijzigt, alle handtekeningen voor gedeelde toegang gemaakt op basis van deze zijn ongeldig.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>De handtekening voor gedeelde toegang (op het niveau van de AMQP) gebruiken

In de vorige sectie hebt u gezien hoe u met het SAS-token met een HTTP POST-aanvraag voor het verzenden van gegevens naar de Service Bus. Zoals u weet, kunt u Service Bus met behulp van de geavanceerde AMQP Message Queuing Protocol () die het gewenste protocol te gebruiken voor betere prestaties in veel scenario's kunt openen. Het gebruik van de SAS-token met AMQP wordt beschreven in het document [AMQP Claim-Based Security versie 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) dat wil zeggen in werken concept sinds 2013 maar goed ondersteund door Azure vandaag nog.

Voordat u begint met het verzenden van gegevens naar Service Bus, moet de uitgever van de SAS-token in een AMQP-bericht verzenden naar een goed gedefinieerde AMQP-knooppunt met de naam **$cbs** (u kunt zien dit als een 'speciale' wachtrij gebruikt door de service te verkrijgen en alle SAS te valideren tokens). De uitgever moet opgeven de **ReplyTo** veld in het bericht AMQP; dit is het knooppunt waarin de service met de publisher met het resultaat van de validatie van tokens (een eenvoudige aanvraag/antwoord patroon tussen publisher en de service antwoorden ). Dit knooppunt antwoord wordt gemaakt 'op elk gewenst moment,"spreken over 'dynamische maken van het externe knooppunt', zoals is beschreven in de AMQP 1.0-specificatie. Nadat u hebt gecontroleerd dat de SAS-token geldig is, de uitgever vooruit en beginnen met het verzenden van gegevens naar de service.

De volgende stappen laten zien hoe u voor het verzenden van de SAS-token met het gebruik van AMQP-protocol de [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) bibliotheek. Dit is handig als u niet de officiële Service Bus-SDK gebruiken (bijvoorbeeld op WinRT, .net Compact Framework, .net Framework Micro en Mono) ontwikkeling in C\#. Deze bibliotheek is natuurlijk nuttig voor meer informatie over hoe op claims gebaseerde beveiliging op het niveau van AMQP werkt zoals u hebt gezien hoe het werkt op het niveau van HTTP (met een HTTP POST-aanvraag en de SAS-token verzonden in de header "Autorisatie"). Als u geen grondige kennis over AMQP nodig hebt, kunt u de officiële SDK van Service Bus gebruiken met .net Framework-toepassingen, die het voor u doen.

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

De `PutCbsToken()` methode ontvangt de *verbinding* (AMQP-verbinding klasse-instantie worden geleverd door de [AMQP .NET Lite-bibliotheek](https://github.com/Azure/amqpnetlite)) die staat voor de TCP-verbinding naar de service en de *sasToken* parameter die de SAS-token voor het verzenden van. 

> [!NOTE]
> Het is belangrijk dat de verbinding wordt gemaakt met **SASL verificatiemechanisme ingesteld op anoniem** (en niet de standaard zonder opmaak met gebruikersnaam en wachtwoord dat wordt gebruikt wanneer u niet nodig hebt voor het verzenden van de SAS-token).
> 
> 

De uitgever maakt vervolgens twee AMQP koppelingen voor het SAS-token verzenden en ontvangen van het antwoord (het resultaat van de validatie van tokens) van de service.

Het AMQP-bericht bevat een set eigenschappen en meer gegevens dan een eenvoudige bericht. De SAS-token is de hoofdtekst van het bericht (met behulp van de constructor). De **"ReplyTo"** eigenschap is ingesteld op de naam van het knooppunt voor het ontvangen van het resultaat van clustervalidatie op de koppeling van de ontvanger (u kunt de naam wijzigen als u wilt en het wordt dynamisch worden gemaakt door de service). De laatste drie toepassing/aangepaste eigenschappen worden gebruikt door de service om aan te geven wat voor soort bewerking heeft om uit te voeren. Zoals wordt beschreven in de ontwerp-specificatie van CBS, moeten ze de **bewerkingsnaam** ('put-token'), de **type token** (in dit geval een `servicebus.windows.net:sastoken`), en de **'name' van de doelgroep** waarop het token van toepassing is (de volledige entiteit).

Nadat het SAS-token is verzonden op de koppeling van de afzender, moet de uitgever van het antwoord op de koppeling ontvanger gelezen. Het antwoord is een eenvoudige AMQP-bericht met een eigenschap van de toepassing met de naam **'statuscode'** die dezelfde waarden als een HTTP-statuscode kan bevatten.

## <a name="rights-required-for-service-bus-operations"></a>Rechten die zijn vereist voor Service Bus-bewerkingen

De volgende tabel toont de toegangsrechten nodig zijn voor verschillende bewerkingen voor Service Bus-resources.

| Bewerking | Claim vereist | Claim bereik |
| --- | --- | --- |
| **Namespace** | | |
| Verificatieregel voor een naamruimte configureren |Beheren |Een naamruimte-adres |
| **Service-register** | | |
| Opsommen van privé-beleid |Beheren |Een naamruimte-adres |
| Beginnen met het luisteren naar een naamruimte |Luisteren |Een naamruimte-adres |
| Berichten verzenden naar een listener in een naamruimte |Verzenden |Een naamruimte-adres |
| **wachtrij** | | |
| Een wachtrij maken |Beheren |Een naamruimte-adres |
| Een wachtrij verwijderen |Beheren |Een geldige wachtrij-adres |
| Het inventariseren van wachtrijen |Beheren |Map resources/wachtrijen |
| De wachtrijbeschrijving van de ophalen |Beheren |Een geldige wachtrij-adres |
| Verificatieregel voor een wachtrij configureren |Beheren |Een geldige wachtrij-adres |
| In de naar de wachtrij verzenden |Verzenden |Een geldige wachtrij-adres |
| Berichten ontvangen van een wachtrij |Luisteren |Een geldige wachtrij-adres |
| Afbreken of voltooien berichten na de ontvangst van het bericht in peek-lock-modus |Luisteren |Een geldige wachtrij-adres |
| Een bericht voor het ophalen van hoger uitstellen |Luisteren |Een geldige wachtrij-adres |
| Onbestelbare berichten van een bericht |Luisteren |Een geldige wachtrij-adres |
| De status is gekoppeld aan een sessie van de wachtrij bericht ophalen |Luisteren |Een geldige wachtrij-adres |
| De status die is gekoppeld aan een sessie van de wachtrij bericht instellen |Luisteren |Een geldige wachtrij-adres |
| Een bericht voor latere levering; plannen bijvoorbeeld, [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Luisteren | Een geldige wachtrij-adres
| **Onderwerp** | | |
| Een onderwerp maken |Beheren |Een naamruimte-adres |
| Een onderwerp verwijderen |Beheren |Elk adres geldig onderwerp |
| Onderwerpen over opsommen |Beheren |Map resources/onderwerpen |
| De beschrijving van het onderwerp ophalen |Beheren |Elk adres geldig onderwerp |
| Verificatieregel voor een onderwerp configureren |Beheren |Elk adres geldig onderwerp |
| Verzenden naar het onderwerp |Verzenden |Elk adres geldig onderwerp |
| **Abonnement** | | |
| Een abonnement maken |Beheren |Een naamruimte-adres |
| Abonnement verwijderen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Het inventariseren van abonnementen |Beheren |.. / myTopic/abonnementen |
| Beschrijving van het abonnement ophalen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Afbreken of voltooien berichten na de ontvangst van het bericht in peek-lock-modus |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| Een bericht voor het ophalen van hoger uitstellen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| Onbestelbare berichten van een bericht |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| De status is gekoppeld aan een sessie onderwerp ophalen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| De status die is gekoppeld aan een sessie voor onderwerp instellen |Luisteren |.. /myTopic/Subscriptions/mySubscription |
| **Regels** | | |
| Een regel maken |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Een regel verwijderen |Beheren |.. /myTopic/Subscriptions/mySubscription |
| Regels opsommen |Beheren of luisteren |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Service Bus-wachtrijen gebruiken](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com