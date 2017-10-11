---
title: Identiteit voor Azure-app maken met Azure CLI | Microsoft Docs
description: "Beschrijft hoe Azure CLI gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal en toegang tot resources via toegangsbeheer op basis van rollen. Er wordt weergegeven hoe toepassing met een wachtwoord of certificaat te verifiëren."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 3c5826d58887ff1af4df8e66999d9c1a1643bcc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure CLI gebruiken om een service-principal te maken voor toegang tot resources

Wanneer u een app of een script dat nodig heeft voor toegang tot resources hebt, kunt u een identiteit voor de app instellen en de app met een eigen referenties verifiëren. Deze identiteit staat bekend als een service-principal. Deze aanpak kunt u:

* Machtigingen toekennen aan de identiteit van de app die anders zijn dan uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* Een certificaat voor verificatie gebruiken bij het uitvoeren van een onbewaakt script.

Dit artikel laat zien hoe u [Azure CLI 1.0](../cli-install-nodejs.md) voor het instellen van een toepassing wilt uitvoeren onder een eigen referenties en identiteit. Installeer de nieuwste versie van [Azure CLI 1.0](../cli-install-nodejs.md) om ervoor te zorgen dat uw omgeving overeenkomt met de voorbeelden in dit artikel.

## <a name="required-permissions"></a>Vereiste machtigingen
Als u dit onderwerp, moet u voldoende machtigingen in uw Azure Active Directory en uw Azure-abonnement hebben. In het bijzonder moet u een app maken in de Azure Active Directory en de service-principal toewijzen aan een rol. 

De eenvoudigste manier om te controleren of uw account over de juiste machtigingen beschikt, verloopt via de portal. Zie [Check required permission in portal](resource-group-create-service-principal-portal.md#required-permissions) (Vereiste machtigingen controleren in de portal).

Nu gaat u verder met een sectie voor [wachtwoord](#create-service-principal-with-password) of [certificaat](#create-service-principal-with-certificate) verificatie.

## <a name="create-service-principal-with-password"></a>Service-principal maken met wachtwoord
In deze sectie maakt u de stappen voor het maken van de AD-toepassing met een wachtwoord en de rol Lezer toegewezen aan de service-principal.

1. Aanmelden bij uw account.
   
   ```azurecli
   azure login
   ```
2. Voor het maken van een app-identiteit bieden u de naam van de app en een wachtwoord, zoals wordt weergegeven in de volgende opdracht:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   De nieuwe service-principal wordt geretourneerd. De Object-Id is vereist bij het verlenen van machtigingen. De guid worden weergegeven met de Service Principal Names is nodig wanneer u zich aanmeldt. Deze guid is dezelfde waarde als de app-id. In de voorbeeldtoepassingen deze waarde wordt aangeduid als de `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Aan de service principal machtigingen verlenen voor uw abonnement. In dit voorbeeld kunt u de service-principal toevoegen aan de rol lezer die machtiging toekent voor het lezen van alle resources in het abonnement. Zie voor andere rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md). Geef de Object-Id die u hebt gebruikt bij het maken van de toepassing voor de object-id-parameter. Voordat u deze opdracht uitvoert, moet u even totdat de nieuwe service-principal worden doorgegeven in Azure Active Directory toestaan. Wanneer u deze opdrachten handmatig uitvoert, is meestal voldoende tijd verstreken tussen taken. In een script, moet u een stap in de slaapstand tussen de opdrachten toevoegen (zoals `sleep 15`). Als u een foutmelding weergegeven dat de principal bestaat niet in de map ziet, voert u de opdracht opnieuw uit.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
Dat is alles. Uw AD-toepassing en service-principal worden ingesteld. De volgende sectie leest u hoe aanmelden met de referentie via Azure CLI. Als u de referentie gebruiken in uw toepassing code wilt, hoeft u niet om door te gaan met dit onderwerp. U kunt naar gaan de [voorbeeldtoepassingen](#sample-applications) voor voorbeelden van aangemeld met uw toepassings-id en wachtwoord. 

### <a name="provide-credentials-through-azure-cli"></a>Geef referenties op via Azure CLI
Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich aanmeldt als een service-principal, moet u voor de tenant-id van de map voor uw AD-app. Een tenant is een exemplaar van Azure Active Directory. Voor het ophalen van de tenant-id voor uw abonnement op dat moment geverifieerde gebruiken:
   
   ```azurecli
   azure account show
   ```
   
   Die wordt geretourneerd:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Als u de tenant-id van een ander abonnement ophalen moet, gebruikt u de volgende opdracht:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Als u ophalen van de client-id moet worden gebruikt wilt voor aanmelden, gebruiken:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     De waarde moet worden gebruikt voor het aanmelden is de guid die worden vermeld in de SPN-namen.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Aanmelden als service-principal.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    U wordt gevraagd om het wachtwoord. Geef het wachtwoord die u hebt opgegeven bij het maken van de AD-toepassing.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

U bent nu geautoriseerd als de service-principal voor de service-principal die u hebt gemaakt.

U kunt ook de REST-bewerkingen aan te melden vanaf de opdrachtregel aanroepen. U kunt het toegangstoken voor gebruik met andere bewerkingen ophalen uit het verificatieantwoord. Zie voor een voorbeeld van het ophalen van het toegangstoken door aan te roepen REST-bewerkingen [genereren van een Token toegang](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Service-principal maken met certificaat
In deze sectie kunt u de stappen voor het uitvoeren:

* Een zelfondertekend certificaat maken
* de AD-toepassing maken met het certificaat en de service-principal
* de rol Lezer toegewezen aan de service-principal

Deze stappen uit te voeren, moet u hebben [OpenSSL](http://www.openssl.org/) geïnstalleerd.

1. Een zelfondertekend certificaat maken.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. De voorgaande stap twee bestanden - privkey.pem en cert.pem hebt gemaakt. De openbare en persoonlijke sleutels worden gecombineerd tot één bestand.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Open de **examplecert.pem** -bestand en zoek naar de lang reeks tekens tussen **---BEGIN CERTIFICATE---** en **---EINDCERTIFICAAT---**. Kopieer de gegevens van het certificaat. U kunt deze gegevens als een parameter doorgeven bij het maken van de service principal.

4. Aanmelden bij uw account.

   ```azurecli
   azure login
   ```
5. Voor het maken van de service-principal bieden u de naam van de app en de gegevens van het certificaat, zoals wordt weergegeven in de volgende opdracht:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   De nieuwe service-principal wordt geretourneerd. De Object-Id is vereist bij het verlenen van machtigingen. De guid worden weergegeven met de Service Principal Names is nodig wanneer u zich aanmeldt. Deze guid is dezelfde waarde als de app-id. In de voorbeeldtoepassingen wordt deze waarde aangeduid als de Client-ID. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Aan de service principal machtigingen verlenen voor uw abonnement. In dit voorbeeld kunt u de service-principal toevoegen aan de rol lezer die machtiging toekent voor het lezen van alle resources in het abonnement. Zie voor andere rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md). Geef de Object-Id die u hebt gebruikt bij het maken van de toepassing voor de object-id-parameter. Voordat u deze opdracht uitvoert, moet u even totdat de nieuwe service-principal worden doorgegeven in Azure Active Directory toestaan. Wanneer u deze opdrachten handmatig uitvoert, is meestal voldoende tijd verstreken tussen taken. In een script, moet u een stap in de slaapstand tussen de opdrachten toevoegen (zoals `sleep 15`). Als u een foutmelding weergegeven dat de principal bestaat niet in de map ziet, voert u de opdracht opnieuw uit.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Geef het certificaat via automatische Azure CLI-script
Nu moet u zich aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich aanmeldt als een service-principal, moet u voor de tenant-id van de map voor uw AD-app. Een tenant is een exemplaar van Azure Active Directory. Voor het ophalen van de tenant-id voor uw abonnement op dat moment geverifieerde gebruiken:
   
   ```azurecli
   azure account show
   ```
   
   Die wordt geretourneerd:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Als u de tenant-id van een ander abonnement ophalen moet, gebruikt u de volgende opdracht:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Vingerafdruk van het certificaat ophalen en verwijder overbodige tekens, gebruiken:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Die een vingerafdrukwaarde retourneert vergelijkbaar met:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Als u ophalen van de client-id moet worden gebruikt wilt voor aanmelden, gebruiken:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   De waarde moet worden gebruikt voor het aanmelden is de guid die worden vermeld in de SPN-namen.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Aanmelden als service-principal.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

U bent nu geautoriseerd als de service-principal voor de Azure Active Directory-toepassing die u hebt gemaakt.

## <a name="change-credentials"></a>Referenties wijzigen

U kunt de referenties voor een AD-app, hetzij vanwege een beveiligingsinbreuk of een vervaldatum referentie wijzigen met `azure ad app set`.

Een wachtwoord wilt wijzigen, gebruikt u:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

De waarde van een certificaat wilt wijzigen, gebruikt u:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Fouten opsporen

De volgende fouten kunnen optreden bij het maken van een service-principal:

* **'Authentication_Unauthorized'** of **'geen abonnement gevonden in de context.'** -U hebt deze fout te zien wanneer uw account beschikt niet over de [vereist machtigingen](#required-permissions) op de Azure Active Directory om een app te registreren. Normaal gesproken u deze fout te zien wanneer alleen admin gebruikers in uw Azure Active Directory apps registreren kunnen en uw account niet een beheerder is. Vraag uw beheerder u ofwel toewijzen aan een beheerdersrol of waarmee gebruikers om apps te registreren.

* Uw account **"heeft geen autorisatie om uit te voeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik/subscriptions / {guid}."**  -Deze fout te zien wanneer uw account beschikt niet over voldoende machtigingen aan een rol toewijzen aan een identiteit. Vraag de abonnementsbeheerder u toe te voegen aan de rol beheerder voor gebruikerstoegang.

## <a name="sample-applications"></a>Voorbeeldtoepassingen
Zie voor informatie over logboekregistratie in als de toepassing via verschillende platforms:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Volgende stappen
* Zie voor gedetailleerde stappen voor het integreren van een toepassing in Azure voor het beheren van resources [Ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](resource-manager-api-authentication.md).
* Zie voor meer informatie over het gebruik van certificaten en Azure CLI, [certificaat gebaseerde verificatie met Azure Service-Principals vanaf de opdrachtregel Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Zie voor een lijst met beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
