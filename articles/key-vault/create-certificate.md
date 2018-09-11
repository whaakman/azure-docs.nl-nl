---
title: Methoden voor het maken van certificaten
description: Manieren om te maken van een certificaat in Key Vault.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e17b4c9b-4ff3-472f-8c9d-d130eb443968
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: bryanla
ms.openlocfilehash: d2f9327841e0c6193a89df6459b4d8fffb14c05e
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302840"
---
# <a name="certificate-creation-methods"></a>Methoden voor het maken van certificaten

 Een Key Vault (KV)-certificaat kan worden gemaakt of geïmporteerd in een key vault. Als u een certificaat KV maakt is de persoonlijke sleutel gemaakt binnen de key vault en nooit worden blootgesteld aan de eigenaar van het certificaat. De volgende zijn manieren om te maken van een certificaat in Key Vault:  

-   **Een zelfondertekend certificaat maken:** Hiermee wordt een openbaar / persoonlijk sleutelpaar te maken en deze koppelen aan een certificaat. Het certificaat zal worden ondertekend door een eigen sleutel.  

-    **Maak een nieuw certificaat handmatig:** dit wordt gemaakt van een openbaar / persoonlijk sleutelpaar en een X.509-aanvraag voor Certificaatondertekening genereren. De aanvraag voor Certificaatondertekening kan worden ondertekend door uw Statusregistratie-instanties of de certificeringsinstantie (CA). De ondertekende x509 certificaat kan worden samengevoegd met de in behandeling sleutel worden gekoppeld aan het voltooien van het certificaat KV in Key Vault. Hoewel deze methode zijn meer stappen vereist, biedt u met betere beveiliging omdat de persoonlijke sleutel is gemaakt in en tot Key Vault beperkt. Dit wordt uitgelegd in het onderstaande diagram.  

![Een certificaat maken met uw eigen certificeringsinstantie](media/certificate-authority-1.png)  

De volgende beschrijvingen komen overeen met de groene letters stappen in het voorgaande diagram.

1. In het bovenstaande diagram is uw toepassing een certificaat dat intern wordt gestart met het maken van een sleutel in uw key vault maken.
2. Key Vault retourneert aan uw toepassing een Certificate Signing Request (CSR)
3. Uw toepassing geeft de CSR aan de door u gekozen CA.
4. De door u gekozen CA reageert met een een X509 certificaat.
5. Uw toepassing klaar is voor het nieuwe certificaat maken met een fusie van de X509 certificaat van uw Certificeringsinstantie.

-   **Maak een certificaat met de provider van een bekende verlener:** voor deze methode moet u een eenmalige taak voor het maken van een verlener-object. Zodra een verlener-object is gemaakt in uw key vault, de naam ervan in het beleid van het certificaat KV kan worden verwezen. Een aanvraag voor het maken van een certificaat KV maakt een sleutelpaar in de kluis en communiceren met de uitgever provider-service met behulp van de gegevens in het object waarnaar wordt verwezen, uitgever om een x509 certificaat. De x509 certificaat wordt opgehaald uit de service verlener en wordt samengevoegd met het sleutelpaar om uit te voeren van de KV certificaat maken.  

![Maak een certificaat met een Key Vault is een partnerschap aangegaan certificeringsinstantie](media/certificate-authority-2.png)  

De volgende beschrijvingen komen overeen met de groene letters stappen in het voorgaande diagram.

1. In het bovenstaande diagram is uw toepassing een certificaat dat intern wordt gestart met het maken van een sleutel in uw key vault maken.
2. Key Vault verzendt en SSL-certificaat aanvragen bij de Certificeringsinstantie.
3. Uw toepassing peilt, in een lus en wait-proces, uw Key Vault voor het certificaat is voltooid. Het maken van het certificaat is voltooid wanneer Key Vault van de CA-antwoord met x509 ontvangt certificaat.
4. De CA van de Sleutelkluis SSL-certificaat aanvragen met een X509 reageert SSL-certificaat.
5. Het maken van het nieuwe certificaat is voltooid met de integratie van de X509 certificaat voor de CA.

## <a name="asynchronous-process"></a>Asynchroon proces
Er is een asynchroon proces voor KV certificaat te maken. Deze bewerking wordt een KV certificaataanvraag maken en een http-statuscode 202 (geaccepteerd) retourneren. De status van de aanvraag kan worden gevolgd door het opvragen van configuratiegegevens bij het in behandeling-object gemaakt voor deze bewerking. De volledige URI van de in behandeling object wordt geretourneerd in de LOCATION-header.  

Wanneer een aanvraag voor het maken van een certificaat KV is voltooid, de status van de in behandeling object wordt gewijzigd in 'voltooid' uit 'inprogress' en een nieuwe versie van het certificaat KV wordt gemaakt. Hiermee worden de huidige versie.  

## <a name="first-creation"></a>Eerste maken
 Wanneer een KV-certificaat voor de eerste keer wordt gemaakt, wordt ook een adresseerbare sleutel en -geheim gemaakt met dezelfde naam als die van het certificaat. Als de naam al gebruikt wordt, klikt u vervolgens mislukken de bewerking met een HTTP-statuscode 409 (conflict).
De adresseerbare sleutel en -geheim ophalen hun kenmerken van de kenmerken van KV certificaat. De adresseerbare sleutel en -geheim dat is gemaakt op deze manier zijn gemarkeerd als beheerde sleutels en geheimen, waarvan de levensduur wordt beheerd door Key Vault. Beheerde sleutels en geheimen zijn alleen-lezen. Opmerking: Als een KV-certificaat is verlopen of is uitgeschakeld, de bijbehorende sleutel en -geheim wordt niet meer worden gebruikt.  

 Als dit de eerste bewerking voor het maken van een certificaat KV zijn een beleid is vereist.  Een beleid kan ook worden opgegeven met opeenvolgende maakbewerkingen ter vervanging van de resource van het beleid. Als een beleid niet opgegeven is, wordt de resource van het beleid op de service gebruikt een volgende versie van KV certificaat wilt maken. Houd er rekening mee dat wanneer een aanvraag voor het maken van een volgende versie wordt uitgevoerd, de huidige KV certificaat en de bijbehorende adresseerbare sleutel en geheim, blijven ongewijzigd.  

## <a name="self-issued-certificate"></a>Zelf-uitgegeven certificaat
 Voor het maken van een zelf-uitgegeven certificaat, stelt u de naam van de certificaatverlener als 'Self' in het certificaatbeleid zoals wordt weergegeven in het volgende fragment van certificaatbeleid.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 Als naam van de certificaatverlener niet opgegeven is, is klikt u vervolgens de naam van de uitgever ingesteld op 'Onbekend'. Als de uitgever is 'Onbekend', de eigenaar van het certificaat moet handmatig een x509 ophalen en samenvoegen van de openbare x509 certificaat met de sleutelkluis-certificaat is in behandeling object om uit te voeren van het maken van het certificaat vervolgens certificaat van de verlener van zijn/haar keuze.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>Hebben CA-Providers
Maken van het certificaat kan worden voltooid handmatig of via een 'zelf' verlener. Key Vault partners ook met bepaalde providers verlener om het maken van certificaten te vereenvoudigen. De volgende typen certificaten kunnen worden besteld voor key vault met deze partner verlener providers.  

|Provider|Certificaattype|  
|--------------|----------------------|  
|DigiCert|Key Vault biedt OV of VW SSL-certificaten met DigiCert|
|GlobalCert|Key Vault biedt OV of VW SSL-certificaten met GlobalSign|

 Een certificaatverlener is een entiteit in Azure Key Vault (KV) als een resource CertificateIssuer weergegeven. Deze wordt gebruikt voor informatie over de oorzaak van een certificaat KV; naam van verlener, provider, referenties en andere administratieve informatie.

Houd er rekening mee dat wanneer een bestelling wordt geplaatst met de provider van de uitgever, het in acht neemt kan of overschrijven de x509 certificaatuitbreidingen en de geldigheidsduur van certificaat op basis van het type certificaat.  

 Autorisatie: Vereist de machtiging Certificaten/maken.

 ## <a name="see-also"></a>Zie ook
 - [Over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
 - [Het maken van certificaten controleren en beheren](create-certificate-scenarios.md)
