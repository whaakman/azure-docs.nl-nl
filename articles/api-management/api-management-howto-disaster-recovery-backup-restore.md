---
title: Implementeer disaster recovery gebruiken back-up en herstel in Azure API Management | Microsoft Docs
description: Informatie over het gebruik van back-up en herstellen als u wilt uitvoeren van herstel na noodgevallen in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 51ec88ae2a4f10b68c7d74324c3a3a25d2893891
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Het herstel na noodgevallen met back-up van de service implementeren en te herstellen in Azure API Management
Als u kiest voor het publiceren en beheren van uw API's via Azure API Management u profiteert van veel fouttolerantie en mogelijkheden van de infrastructuur die u anders zelf zou moeten ontwerpen, implementeren en beheren. De Azure-platform vermindert een groot gedeelte van de mogelijke fouten in een fractie van de kosten.

Als u wilt herstellen op beschikbaarheid moet vraagstukken op het gebied van de regio waar uw API Management-service wordt gehost gereed voor het opnieuw samenstellen van uw service in een andere regio op elk gewenst moment. Afhankelijk van uw doelstellingen van de beschikbaarheid en de beoogde hersteltijd is het raadzaam een back-service in een of meer regio's reserveren en proberen te synchroon met de actieve service onderhouden van de configuratie en de inhoud. De service back-up en herstelfunctie biedt de benodigde bouwsteen voor het implementeren van uw strategie voor noodherstel.

Deze handleiding wordt getoond hoe om Azure Resource Manager-aanvragen te verifiëren en het back-up en herstellen van uw API Management-service-exemplaren.

> [!NOTE]
> Het proces voor een back-up en herstellen van een API Management-service-exemplaar voor herstel na noodgevallen kan ook worden gebruikt voor het repliceren van exemplaren van API Management-service voor scenario's zoals fasering.
>
> Houd er rekening mee dat elke back-up na 30 dagen verloopt. Als u een back-up herstellen probeert nadat de verloopperiode 30 dagen is verlopen, mislukt de herstelbewerking met een `Cannot restore: backup expired` bericht.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Verificatie van Azure Resource Manager-aanvragen
> [!IMPORTANT]
> De REST-API voor back-up en herstel maakt gebruik van Azure Resource Manager en heeft een ander verificatiemechanisme dan de REST-API's voor het beheren van uw API Management-entiteiten. De stappen in deze sectie beschrijven hoe om Azure Resource Manager-aanvragen te verifiëren. Zie voor meer informatie [verificatie van Azure Resource Manager-aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Alle taken die u op resources met behulp van de Azure Resource Manager uitvoeren moet worden geverifieerd met Azure Active Directory met de volgende stappen uit.

* Een toepassing toevoegen aan de Azure Active Directory-tenant.
* Stel de machtigingen voor de toepassing die u hebt toegevoegd.
* Ophalen van het token voor het verifiëren van aanvragen in Azure Resource Manager.

De eerste stap is het maken van een Azure Active Directory-toepassing. Meld u aan bij de [klassieke Azure-Portal](http://manage.windowsazure.com/) met behulp van het abonnement met uw API Management-service-instantie en navigeer naar de **toepassingen** tabblad voor uw standaard Azure Active Directory.

> [!NOTE]
> Als de standaarddirectory voor Azure Active Directory niet zichtbaar voor uw account is, moet u contact op met de beheerder van het Azure-abonnement om de vereiste machtigingen toewijzen aan uw account.

![Azure Active Directory-toepassing maken][api-management-add-aad-application]

Klik op **toevoegen**, **mijn organisatie ontwikkelt toepassing toevoegen**, en kies **Native client-toepassing**. Voer een beschrijvende naam en klik op de pijl Volgende. Geef een tijdelijke aanduiding-URL, zoals `http://resources` voor de **omleidings-URI**, zoals het is een verplicht veld, maar niet de waarde wordt gebruikt op een later. Schakel het selectievakje in om op te slaan van de toepassing.

Nadat de toepassing is opgeslagen, klikt u op **configureren**, schuif omlaag naar de **machtigingen voor andere toepassingen** sectie en op **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-aad-permissions-add]

Selecteer **Windows** **Azure Service Management API** en klik op het selectievakje in als de toepassing wilt toevoegen.

![Machtigingen toevoegen][api-management-aad-permissions]

Klik op **gedelegeerde machtigingen** naast de zojuist toegevoegde **Windows** **Azure Service Management API** toepassing, schakel het selectievakje voor **Access Azure Service Management (preview)**, en klik op **opslaan**.

![Machtigingen toevoegen][api-management-aad-delegated-permissions]

Voorafgaand aan de API's die bij het genereren van de back-up en herstel hem wordt aangeroepen, is het nodig om een token verkrijgen. Het volgende voorbeeld wordt de [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget-pakket voor het ophalen van het token.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Vervang `{tentand id}`, `{application id}`, en `{redirect uri}` met behulp van de volgende instructies.

Vervang `{tenant id}` met tenant-id van de Azure Active Directory-toepassing die u zojuist hebt gemaakt. U kunt toegang krijgen tot de id door te klikken op **eindpunten weergeven**.

![Eindpunten][api-management-aad-default-directory]

![Eindpunten][api-management-endpoint]

Vervang `{application id}` en `{redirect uri}` met behulp van de **Client-Id** en de URL van de **omleidings-URI's** gedeelte van uw Azure Active Directory-toepassing **configureren** tabblad.

![Resources][api-management-aad-resources]

Wanneer de waarden zijn opgegeven, weer het codevoorbeeld een token vergelijkbaar met het volgende voorbeeld.

![Token][api-management-arm-token]

Voordat het aanroepen van de back-up en herstel bewerkingen die worden beschreven in de volgende secties stelt u de aanvraagheader autorisatie voor de REST-aanroep.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Back-up van een API Management-service
Reservekopie maken van een probleem van API Management-service de volgende HTTP-aanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Waarbij:

* `subscriptionId`-id van het abonnement met de API Management-service die u probeert aan back-up
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service regio}, waarbij `service-region` identificeert de Azure-regio waar de API Management-service dat u wilt back-up wordt gehost, bijvoorbeeld`North-Central-US`
* `serviceName`-de naam van de API Management-service maken van een back-up van de opgegeven op het moment van het maken ervan
* `api-version`-vervangen`2014-02-14`

Geef de doel-Azure-opslag-accountnaam, toegangssleutel, de naam van de blob-container en back-upnaam in de hoofdtekst van de aanvraag:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Stel de waarde van de `Content-Type` aanvraagheader naar `application/json`.

Back-up is een langdurige bewerking die meerdere minuten kan duren.  Als de aanvraag voltooid is en de back-upproces is gestart. u ontvangt een `202 Accepted` statuscode van antwoord met een `Location` header.  Controleer 'GET-aanvragen naar de URL in de `Location` header om erachter te komen met de status van de bewerking. Terwijl de back-up uitgevoerd wordt, moet u blijft ontvangen statuscode '202 geaccepteerde'. Reactiecode `200 OK` voltooiing van de back-upbewerking wordt aangegeven.

Houd rekening met de volgende beperkingen bij het maken van een back-aanvraag.

* **Container** opgegeven in de aanvraagtekst **moet bestaan**.
* Terwijl back-up uitgevoerd u wordt **moet niet proberen deze service management bewerkingen** zoals SKU upgrade of een downgrade, wijziging van de domeinnaam, enzovoort.
* Herstellen van een **back-up kan worden gegarandeerd alleen voor 30 dagen** sinds het moment dat het maken ervan.
* **Gebruiksgegevens** gebruikt voor het analytics-Rapporten maken **wordt niet meegeleverd** in de back-up. Gebruik [REST-API van Azure API Management] [ Azure API Management REST API] periodiek ophalen analytics-Rapporten voor veilig te bewaren.
* De frequentie waarmee u de service back-ups uitvoeren heeft invloed op uw beoogd herstelpunt. We adviseren om te beperken van het implementeren van regelmatige back-ups als back-ups op aanvraag uitvoeren na het aanbrengen van belangrijke wijzigingen in uw API Management-service.
* **Wijzigingen** aangebracht in de serviceconfiguratie (bijvoorbeeld API's, beleid, developer portal vormgeving) tijdens het back-up-bewerking bezig is **mogelijk niet opgenomen in de back-up en daarom gaan verloren**.

## <a name="step2"></a>Herstellen van een API Management-service
Voor het herstellen van een API Management moet een eerder gemaakte back-up-service de volgende HTTP-aanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Waarbij:

* `subscriptionId`-id van het abonnement met de API Management-service die u herstelt een back-up in
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service regio}, waarbij `service-region` identificeert de Azure-regio waar u herstelt een back-up in de API Management-service wordt gehost, bijvoorbeeld`North-Central-US`
* `serviceName`-de naam van de API-service wordt hersteld in die zijn opgegeven op het moment van het maken ervan
* `api-version`-vervangen`2014-02-14`

Geef de locatie van back-upbestand, dat wil zeggen accountnaam van de Azure-opslag, toegangssleutel, de naam van de blob-container en back-upnaam in de hoofdtekst van de aanvraag:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Stel de waarde van de `Content-Type` aanvraagheader naar `application/json`.

Terugzetten is een langdurige bewerking die op meer dan 30 minuten duren kan om te voltooien.  Als de aanvraag voltooid is en het herstelproces is gestart. u ontvangt een `202 Accepted` statuscode van antwoord met een `Location` header.  Controleer 'GET-aanvragen naar de URL in de `Location` header om erachter te komen met de status van de bewerking. Terwijl het herstel uitgevoerd wordt u ontvangt nog altijd '202 geaccepteerde' statuscode. Reactiecode `200 OK` geeft de herstelbewerking is voltooid.

> [!IMPORTANT]
> **De SKU** van de service wordt hersteld in **moet overeenkomen met** de SKU van de back-up-service wordt hersteld.
>
> **Wijzigingen** aangebracht aan de serviceconfiguratie (bijvoorbeeld API's, beleid, developer portal vormgeving) tijdens het herstel wordt uitgevoerd **kunnen worden overschreven**.
>
>

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende blogs van Microsoft voor twee verschillende scenario's van het proces van back-up/herstel.

* [Azure API Management Accounts repliceren](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Bedankt dat u naar Gisela haar bijdragen aan dit artikel.
* [Azure API Management: Een back-Up en herstellen van de configuratie](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * De gedetailleerde door Stuart aanpak komt niet overeen met de officiële richtlijnen, maar het is heel interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
