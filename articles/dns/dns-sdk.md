---
title: Maken van DNS-zones en -recordsets in Azure DNS met de .NET SDK | Microsoft Docs
description: Het maken van DNS-zones en -recordsets in Azure DNS met behulp van de .NET SDK.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: jonatul
ms.openlocfilehash: c0fb0be8da1c0ca48a4d43ea027d30a0bc17fe30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Maken van DNS-zones en -recordsets met de .NET SDK

U kunt de bewerkingen maken, verwijderen of bijwerken van DNS-zones, recordsets en records met behulp van DNS-SDK met DNS-beheer van .NET-bibliotheek automatiseren. Er is een volledig Visual Studio-project beschikbaar [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Het account van een service-principal maken

Normaal gesproken wordt programmatische toegang tot Azure-resources verleend via een speciaal account in plaats van uw eigen gebruikersreferenties. Deze specifieke accounts zijn accounts 'service-principal' genoemd. Voor het gebruik van het voorbeeldproject Azure DNS-SDK, moet u eerst een account van de service-principal maken en hieraan de juiste machtigingen.

1. Ga als volgt [deze instructies](../azure-resource-manager/resource-group-authenticate-service-principal.md) voor het maken van een service-principalaccount (het voorbeeldproject Azure DNS-SDK wordt ervan uitgegaan dat de verificatie op basis van wachtwoorden.)
2. Een resourcegroep maken ([Hier ziet u hoe](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Gebruik Azure RBAC de principal-serviceaccount, DNS-Zone Inzender' om machtigingen te verlenen aan de resourcegroep ([Hier ziet u hoe](../active-directory/role-based-access-control-configure.md).)
4. Als het voorbeeldproject Azure DNS-SDK wordt gebruikt, bewerkt u het bestand 'program.cs' als volgt:

   * Voeg de juiste waarden voor de tenant-id, clientId (ook wel bekend als account-ID), geheim (service-principalaccount wachtwoord) en abonnements-id gebruikt in stap 1.
   * Voer de naam van de resourcegroep hebt gekozen in stap 2.
   * Voer een DNS-zone-naam van uw keuze.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-pakketten en naamruimtedeclaraties

Azure DNS-.NET SDK, hebt u nodig voor het installeren van de **Azure DNS-bibliotheek** NuGet-pakket en andere Azure-pakketten vereist.

1. In **Visual Studio**, opent u een project of een nieuw project.
2. Ga naar **extra**  **>**  **NuGet-Pakketbeheer**  **>**  **NuGet-pakketten voor beheren Oplossing...** .
3. Klik op **Bladeren**, inschakelen de **Include prerelease** selectievakje, en het type **Microsoft.Azure.Management.Dns** in het zoekvak.
4. Selecteer het pakket en klikt u op **installeren** toe te voegen aan uw Visual Studio-project.
5. Herhaal dit proces bovenstaande ook het volgende om pakketten te installeren: **Microsoft.Rest.ClientRuntime.Azure.Authentication** en **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

De volgende naamruimtedeclaraties toevoegen

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a>De DNS-management-client initialiseren

De *DnsManagementClient* bevat de methoden en de vereiste eigenschappen voor het beheren van DNS-zones en recordsets.  De volgende code wordt geregistreerd in het serviceaccount van de principal en maakt een DnsManagementClient-object.

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Maken of bijwerken van een DNS-zone

Voor het maken van een DNS-zone is eerst een 'Zone'-object gemaakt voor de DNS-zone-parameters bevatten. Omdat DNS-zones zijn niet gekoppeld aan een specifiek gebied, is de locatie ingesteld op 'global'. In dit voorbeeld wordt een [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) wordt ook toegevoegd aan de zone.

Daadwerkelijk maken of bijwerken van de zone in Azure DNS, de zone-object met de parameters van de zone wordt doorgegeven aan de *DnsManagementClient.Zones.CreateOrUpdateAsyc* methode.

> [!NOTE]
> DnsManagementClient ondersteunt drie bewerkingsmodi: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchrone met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).  U kunt een van deze modi, afhankelijk van de behoeften van uw toepassing.

Azure DNS ondersteunt optimistische gelijktijdigheid, aangeroepen [Etags](dns-getstarted-create-dnszone.md). In dit voorbeeld geven ' * ' header geeft voor de 'If-None-Match' Azure DNS te maken van een DNS-zone als deze niet al bestaat.  De aanroep mislukt als een zone met de gegeven naam al in de opgegeven resourcegroep bestaat.

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a>DNS-recordsets en records maken

DNS-records worden beheerd als een recordset. Een recordset is een set van records met dezelfde naam en een recordtype in een zone.  De naam van de recordset is ten opzichte van de naam van de zone, niet de volledig gekwalificeerde DNS-naam.

Als u wilt maken of bijwerken van een recordset, een object van de parameters 'RecordSet' wordt gemaakt en doorgegeven aan *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Omdat met DNS-zones, er drie bewerkingsmodi zijn: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchrone met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).

Net als bij de DNS-zones, zijn bewerkingen op recordsets ondersteuning voor de optimistische gelijktijdigheid.  In dit voorbeeld, omdat er geen 'If-Match' en 'If-None-Match' zijn opgegeven, wordt de recordset altijd gemaakt.  Deze aanroep worden alle bestaande recordset met dezelfde naam en recordtype in deze DNS-zone overschreven.

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create the actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a>Get-zones en -recordsets

De *DnsManagementClient.Zones.Get* en *DnsManagementClient.RecordSets.Get* methoden afzonderlijke zones en -recordsets, respectievelijk ophalen. RecordSets worden aangeduid met hun type, naam en de zone en resource groep aanwezig zijn in. Zones worden aangeduid met hun naam en de resourcegroep die aanwezig zijn in.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Een bestaande recordset bijwerken

Voor het bijwerken van een bestaande DNS-record-set, eerst ophalen van de recordset en vervolgens de inhoud van de recordset bijwerken, moet u vervolgens de wijziging verzenden.  In dit voorbeeld wordt de Etag van de opgehaalde recordset in de If-Match-parameter opgeven. De aanroep mislukt als de recordset in de tussentijd is gewijzigd door een gelijktijdige bewerking.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Lijst met zones en -recordsets

Zones weergeven door gebruik van de *DnsManagementClient.Zones.List...*  methoden die ondersteuning bieden voor aanbieding ofwel alle zones in een opgegeven resourcegroep of alle zones in een bepaald Azure-abonnement (in verschillende resourcegroepen.) Recordsets weergeven door gebruiken *DnsManagementClient.RecordSets.List...*  methoden die ondersteuning bieden voor een lijst met alle recordsets in een bepaald gebied of deze recordsets van een bepaald type.

Houd er rekening mee wanneer zones en recordsets die het resultaat kunnen worden gepagineerd.  Het volgende voorbeeld laat zien hoe u doorloopt de pagina's van resultaten. (Een kunstmatig kleine paginagrootte van '2' wordt gebruikt om af te dwingen paginering; in de praktijk deze parameter moet worden weggelaten en grootte van de pagina die wordt gebruikt.)

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
// In practice, to improve performance you would use a large page size or just use the system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a>Volgende stappen

Download de [Azure DNS-.NET SDK-voorbeeldproject](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), waaronder meer voorbeelden van het gebruik van de Azure DNS-.NET SDK, inclusief voorbeelden voor andere typen DNS-records.
