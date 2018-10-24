---
title: Maken van DNS-zones en -recordsets in Azure DNS met behulp van de .NET SDK | Microsoft Docs
description: Het maken van DNS-zones en -recordsets in Azure DNS met behulp van de .NET SDK.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: victorh
ms.openlocfilehash: a814c543b9f4bfe6717e639342d82ed13dac35b0
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954603"
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Maken van DNS-zones en -recordsets met de .NET SDK

U kunt de bewerkingen maken, verwijderen of de DNS-zones, recordsets en records bijwerken met behulp van de DNS-SDK met de DNS-beheer voor .NET-bibliotheek automatiseren. Een volledige Visual Studio-project is beschikbaar [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Een service-principal-account maken

Programmatische toegang tot Azure-resources wordt normaal gesproken verleend via een toegewezen account in plaats van uw eigen gebruikersreferenties. Deze speciale accounts zijn accounts 'service-principal' genoemd. Voor het gebruik van het voorbeeldproject Azure DNS-SDK, moet u eerst een service-principal-account maken en hieraan de juiste machtigingen.

1. Ga als volgt [deze instructies](../active-directory/develop/howto-authenticate-service-principal-powershell.md) te maken van een service principal-account (het voorbeeldproject Azure DNS-SDK wordt ervan uitgegaan dat de verificatie op basis van wachtwoorden.)
2. Een resourcegroep maken ([als volgt hoe](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Azure RBAC gebruiken de service-principalaccount 'Inzender voor DNS-Zone' om machtigingen te verlenen tot de resourcegroep ([als volgt hoe](../role-based-access-control/role-assignments-portal.md).)
4. Als het DNS-SDK van Azure-voorbeeldproject, bewerkt u het bestand 'program.cs' als volgt:

   * Voeg de juiste waarden voor de `tenantId`, `clientId` (ook wel bekend als account-ID), `secret` (service-principalaccount wachtwoord) en `subscriptionId` zoals gebruikt in stap 1.
   * Voer de naam van de resourcegroep hebt gekozen in stap 2.
   * Voer een DNS-zone-naam van uw keuze.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-pakketten en naamruimtedeclaraties

Voor het gebruik van de Azure DNS .NET SDK, moet u voor het installeren van de **Azure DNS-Beheerbibliotheek** NuGet-pakket en andere Azure-pakketten vereist.

1. In **Visual Studio**, opent u een project of een nieuw project.
2. Ga naar **extra** **>** **NuGet-Pakketbeheer** **>** **NuGet-pakketten voor beheren Oplossing...** .
3. Klik op **Bladeren**, inschakelen de **Include prerelease** selectievakje, en het type **Microsoft.Azure.Management.Dns** in het zoekvak.
4. Selecteer het pakket en klikt u op **installeren** toe te voegen aan uw Visual Studio-project.
5. Herhaal het proces dat hierboven is ook het volgende om pakketten te installeren: **Microsoft.Rest.ClientRuntime.Azure.Authentication** en **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

De volgende naamruimtedeclaraties toevoegen

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a>De DNS-management-client initialiseren

De `DnsManagementClient` bevat de methoden en eigenschappen die nodig zijn voor het beheren van DNS-zones en -recordsets.  De volgende code meldt zich aan bij de service-principalaccount en maakt een `DnsManagementClient` object.

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Maken of bijwerken van een DNS-zone

Voor het maken van een DNS-zone, eerst een "Zone" object gemaakt voor de DNS-zone-parameters bevatten. Omdat de DNS-zones zijn niet gekoppeld aan een bepaalde regio, is de locatie ingesteld op 'global'. In dit voorbeeld wordt een [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) wordt ook toegevoegd aan de zone.

Als u wilt daadwerkelijk maken of bijwerken van de zone in Azure DNS, de zone-object met de parameters van de zone wordt doorgegeven aan de `DnsManagementClient.Zones.CreateOrUpdateAsyc` methode.

> [!NOTE]
> DnsManagementClient ondersteunt drie bewerkingsmodi: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).  U kunt een van deze modi, afhankelijk van de behoeften van uw toepassing.

Azure DNS ondersteunt optimistische gelijktijdigheid, met de naam [Etags](dns-getstarted-create-dnszone.md). In dit voorbeeld op te geven ' * ' voor de 'If-None-Match-header geeft aan Azure DNS een DNS-zone maken als deze niet al bestaat.  De aanroep mislukt als een zone met de opgegeven naam al in de opgegeven resourcegroep bestaat.

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

DNS-records worden beheerd als een recordset. Een recordset is een set records met dezelfde naam en type van de record in een zone.  De naam van de recordset is ten opzichte van de zonenaam, niet de volledig gekwalificeerde DNS-naam.

Als u wilt maken of bijwerken van een recordset, een 'RecordSet' parameters-object wordt gemaakt en doorgegeven aan `DnsManagementClient.RecordSets.CreateOrUpdateAsync`. Met DNS-zones, zijn er drie bewerkingsmodi: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).

Net als bij de DNS-zones, bewerkingen op recordsets zijn onder meer ondersteuning voor optimistische gelijktijdigheid.  In dit voorbeeld, omdat er 'If-None-Match' of 'If-None-Match' zijn opgegeven, is de recordset altijd gemaakt.  Deze aanroep overschrijft alle bestaande recordset met dezelfde naam en het recordtype in deze DNS-zone.

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

De `DnsManagementClient.Zones.Get` en `DnsManagementClient.RecordSets.Get` methoden ophalen afzonderlijke zones en -recordsets, respectievelijk. RecordSets worden aangeduid met hun type, de naam en de zone en -resource-groep aanwezig zijn in. Zones worden aangeduid met hun naam en de resourcegroep aanwezig zijn in.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Een bestaande recordset bijwerken

Voor het bijwerken van een bestaande DNS-record-set, eerst ophalen van de recordset en vervolgens de inhoud van de recordset bijwerken, kunt u de wijziging vervolgens verzendt.  In dit voorbeeld geven we de Etag van de opgehaalde recordset in de If-Match-parameter. De aanroep mislukt als een gelijktijdige bewerking in de tussentijd de recordset is gewijzigd.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Lijst met zones en -recordsets

Aan de lijst met zones, gebruikt u de *DnsManagementClient.Zones.List...*  methoden, die ondersteuning bieden voor weergeven van alle zones in een bepaalde resourcegroep ofwel alle zones in een Azure-abonnement (in verschillende resourcegroepen.) Lijst met recordsets, gebruikt *DnsManagementClient.RecordSets.List...*  methoden, die ondersteuning bieden voor een lijst met alle recordsets in een opgegeven zone of deze recordsets van een bepaald type.

Houd er rekening mee wanneer zones en recordsets die het resultaat kunnen gepagineerde.  Het volgende voorbeeld ziet hoe u doorloopt de pagina's van resultaten. (Een kunstmatig kleine paginagrootte van '2' wordt gebruikt om af te dwingen wisselbestand; in de praktijk deze parameter moet worden weggelaten en het standaardpaginaformaat dat wordt gebruikt.)

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

Download de [.NET-SDK van Azure DNS-voorbeeldproject](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), waaronder meer voorbeelden over het gebruik van de Azure DNS .NET SDK, inclusief voorbeelden voor andere DNS-recordtypen.
