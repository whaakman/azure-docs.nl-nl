---
title: Bewaken en beheren van certificaat maken
description: Scenario's voor het demonstreren van tal van opties voor het maken, verwerken bewaken en interactie met het maken van het certificaat met Sleutelkluis.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 0d0995aa-b60d-4811-be12-ba0a45390197
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: e1ea77304fa59b67e0e28a4c7e0b13633eeeff6f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="monitor-and-manage-certificate-creation"></a>Bewaken en beheren van certificaat maken
Van toepassing op: Azure  

De volgende 

De scenario's / bewerkingen die worden beschreven in dit artikel zijn:

- Een certificaat KV met de uitgever van een ondersteunde aanvragen
- Ophalen van aanvraag in behandeling - aanvraagstatus is 'inProgress'
- Ophalen van aanvraag in behandeling - aanvraagstatus is 'complete'
- Aanvraag - aanvraag in behandeling de status 'geannuleerd' of 'is mislukt' in behandeling ophalen
- Aanvraag - aanvraag in behandeling de status 'verwijderd' of 'overschreven' in behandeling ophalen
- Maak (of importeren) wanneer in behandeling zijnde aanvraag bestaat - status is 'inProgress'
- Samenvoegen wanneer aanvraag in behandeling is gemaakt met een verlener (bijvoorbeeld DigiCert)
- Aanvragen van een annulering terwijl de status van aanvraag in behandeling 'inProgress'
- Een aanvraag in behandeling-object verwijderen
- Een certificaat KV handmatig maken
- Wanneer een aanvraag in behandeling is gemaakt - maken van het certificaat handmatig samenvoegen

## <a name="request-a-kv-certificate-with-a-supported-issuer"></a>Een certificaat KV met de uitgever van een ondersteunde aanvragen 

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

De volgende voorbeelden moet een object met de naam 'mydigicert' al worden weergegeven in de sleutelkluis met de verlener provider als DigiCert. Zie voor meer informatie over het werken met uitgevers van certificaten [uitgevers van het certificaat](/rest/api/keyvault/certificate-issuers.md).  

### <a name="request"></a>Aanvraag  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    },  
  "issuer": {  
       "name": "mydigicert",  
    "cty": "OV-SSL",  
    }  
  }  
}  

```  

### <a name="response"></a>Antwoord  

```  
StatusCode: 202, ReasonPhrase: 'Accepted'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "mydigicert"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "InProgress",  
  "status_details": "Pending certificate created. Certificate request is in progress. This may take some time based on the issuer provider. Please check again later",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="get-pending-request---request-status-is-inprogress"></a>Ophalen van aanvraag in behandeling - aanvraagstatus is 'inProgress'

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  
 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

> [!NOTE]
>  Als *request_id* is opgegeven in de query, deze fungeert als een filter. Als de *request_id* in de query en het object in behandeling zijn verschillend zijn, een http-statuscode 404 wordt geretourneerd.  

### <a name="response"></a>Antwoord  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "inProgress",  
  "status_details": "…",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="get-pending-request---request-status-is-complete"></a>Ophalen van aanvraag in behandeling - aanvraagstatus is 'complete'

### <a name="request"></a>Aanvraag  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Antwoord  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "completed",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
   "target": “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"  
}  

```  

## <a name="get-pending-request---pending-request-status-is-canceled-or-failed"></a>Aanvraag - aanvraag in behandeling de status 'geannuleerd' of 'is mislukt' in behandeling ophalen  

### <a name="request"></a>Aanvraag  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 TOEVOEGEN  `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Antwoord  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "failed",  
  "status_details": "",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
   "error":  
    {  
        "code": "<errorcode>",    
        "message": "<message>"  
    }  
}  

```  

> [!NOTE]
> De waarde van de *errorcode* kan "certificaatverlener error" of 'Verzoek geweigerd' gebaseerd op verlener of gebruiker fout respectievelijk.  

## <a name="get-pending-request---pending-request-status-is-deleted-or-overwritten"></a>Aanvraag - aanvraag in behandeling de status 'verwijderd' of 'overschreven' in behandeling ophalen  
 Een object in behandeling kan worden verwijderd of overschreven door een bewerking voor maken/importeren wanneer de status ervan niet "inProgress."

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  
 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 TOEVOEGEN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Antwoord  

```  
StatusCode: 404, ReasonPhrase: 'Not Found'  
{  
  "error":  
    {  
         "code": "PendingCertificateNotFound",  
        "message": "…"  
    }  
}  

```  

## <a name="create-or-import-when-pending-request-exists---status-is-inprogress"></a>Maak (of importeren) wanneer in behandeling zijnde aanvraag bestaat - status is 'inProgress'
 Een object in behandeling heeft vier mogelijke statussen; 'inprogress', 'geannuleerd', 'is mislukt' of 'voltooid'.

 Als de status van een aanvraag in behandeling is 'inprogress', maken (en importeren) bewerkingen mislukken met een HTTP-statuscode van 409 (conflict).  

 Een conflict oplossen:  

 - Als het certificaat handmatig wordt gemaakt, kunt u het certificaat KV voltooien als volgt een samenvoegen of verwijderen op het object in behandeling.  

 - Als het certificaat wordt gemaakt met een verlener, kunt u wachten tot het certificaat is voltooid, mislukt of geannuleerd. U kunt ook de in behandeling object verwijderen.

> [!NOTE]
> Verwijderen van een object in behandeling kan of kunnen de x509 niet annuleren certificaataanvraag met de provider.  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    },  
  "issuer": {  
       "name": "mydigicert"  
    }  
  }  
}  

```  

### <a name="response"></a>Antwoord  

```  
StatusCode: 409, ReasonPhrase: 'Conflict'  
{  
  "error":  
    {  
        "code": "Forbidden",  
        "message": "A new key vault certificate can not be created or imported while a pending key vault certificate's status is inProgress."  
    }  
}  

```  

## <a name="merge-when-pending-request-is-created-with-an-issuer"></a>Wanneer de aanvraag in behandeling is gemaakt met een verlener samenvoegen
 Merge is niet toegestaan wanneer een object in behandeling is gemaakt met een verlener, maar is toegestaan wanneer de status "inProgress." 

 Als de aanvraag voor het maken van de x509 certificaat is mislukt of geannuleerd voor een bepaalde reden en als een certificaat kan worden opgehaald door middel van de out-of-band x509, een samenvoeging voor het voltooien van het certificaat KV kan worden uitgevoerd.  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

### <a name="response"></a>Antwoord  

```json
StatusCode: 403, ReasonPhrase: 'Forbidden'  
{  
  "error":  
    {  
       "code": "Forbidden",  
       "message": "Merge is forbidden on pending object created with issuer : <issuer-name> while it is in progess."  
    }  
}  

```  

## <a name="request-a-cancellation-while-the-pending-request-status-is-inprogress"></a>Aanvragen van een annulering terwijl de status van aanvraag in behandeling 'inProgress'  
 Een annulering kan alleen worden aangevraagd.  Een aanvraag kan of kan niet worden geannuleerd. Als een aanvraag niet gelijk is aan 'inProgress', wordt er een HTTP-status van 400 (ongeldige aanvraag) geretourneerd.  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|PATCH|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  
 PATCH `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 PATCH `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

```json
{  
  "cancellation_requested": true  
}  

```  

### <a name="response"></a>Antwoord  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": true,  
  "status": "inProgress",  
  "status_details": "…",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="delete-a-pending-request-object"></a>Een aanvraag in behandeling-object verwijderen  

> [!NOTE]
> Verwijderen van het object in behandeling kan of kunnen de x509 niet annuleren certificaataanvraag met de provider.  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|DELETE|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  
 VERWIJDEREN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OF  

 VERWIJDEREN `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Antwoord  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "inProgress",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
}  
```  

## <a name="create-a-kv-certificate-manually"></a>Een certificaat KV handmatig maken  
 U kunt een certificaat zijn uitgegeven met een Certificeringsinstantie van uw keuze via een proces voor het handmatig maken. De naam van de verlener ingesteld op 'Onbekend' of geef geen veld voor de uitgever.  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    }  
  "issuer": {  
       "name": "Unknown"  
    }  
  }  
}  

```  

### <a name="response"></a>Antwoord  

```  
StatusCode: 202, ReasonPhrase: 'Accepted'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "Unknown"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "status": "inProgress",  
  "status_details": "Pending certificate created. Please Perform Merge to complete the request.",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="merge-when-a-pending-request-is-created---manual-certificate-creation"></a>Wanneer een aanvraag in behandeling is gemaakt - maken van het certificaat handmatig samenvoegen  

|Methode|Aanvraag-URI|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>Aanvraag  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

|Elementnaam|Vereist|Type|Versie|Beschrijving|  
|------------------|--------------|----------|-------------|-----------------|  
|x5c|Ja|matrix|\<Inleiding tot versie >|X509-keten certificaat als base 64 tekenreeksmatrix.|  

### <a name="response"></a>Antwoord  

```  
StatusCode: 201, ReasonPhrase: 'Created'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"  
{  
"id": "https mykeyvault.vault.azure.net/certificates/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "kid": "https:// mykeyvault.vault.azure.net/keys/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "sid": " mykeyvault.vault.azure.net/secrets/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "cer": "……de34534……",  
    "x5t": "n14q2wbvyXr71Pcb58NivuiwJKk",  
    "attributes": {  
        "enabled": true,  
        "exp": 1530394215,  
        "nbf": 1435699215,  
        "created": 1435699919,  
        "updated": 1435699919  
    },  
    "pending": {  
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/pending"  
    },  
    "policy": {  
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/policy",  
        "key_props": {  
            "exportable": false,  
            "kty": "RSA",  
            "key_size": 2048,  
            "reuse_key": false  
        },  
        "secret_props": {  
            "contentType": "application/x-pkcs12"  
        },  
        "x509_props": {  
            "subject": "CN=Mycert1",  
            "ekus": ["1.3.6.1.5.5.7.3.1", "1.3.6.1.5.5.7.3.2"],  
                       "validity_months":12  
        },  
        "lifetime_actions": [{  
            "trigger": {  
                "lifetime_percentage": 80  
            },  
            "action": {  
                "action_type": "EmailContacts"  
            }  
        }],  
        "issuer": {  
            "name": "Unknown"  
        },  
        "attributes": {  
            "enabled": true,  
            "created": 1435699811,  
            "updated": 1435699811  
        }  
    }  
}  

```

## <a name="see-also"></a>Zie ook
- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
