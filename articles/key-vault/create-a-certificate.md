---
title: Methoden voor het maken van certificaat
description: Manieren om te maken van een certificaat in de Sleutelkluis.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e17b4c9b-4ff3-472f-8c9d-d130eb443968
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 7b71c6a8daa97300ecf3b37ec6ab47207fece98e
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="certificate-creation-methods"></a>Methoden voor het maken van certificaat

 Een sleutel kluis (KV)-certificaat kan worden gemaakt of geïmporteerd in een sleutelkluis. Tijdens het maken van een certificaat KV is de persoonlijke sleutel gemaakt binnen de sleutelkluis en nooit worden blootgesteld aan de eigenaar van het certificaat. De volgende zijn manieren om te maken van een certificaat in de Sleutelkluis:  

-   **Een zelfondertekend certificaat maken:** Hiermee wordt een openbaar / persoonlijk sleutelpaar en deze koppelen aan een certificaat. Het certificaat wordt ondertekend door een eigen sleutel.  

-    **Maak handmatig een nieuw certificaat:** Hiermee wordt een openbaar / persoonlijk sleutelpaar en een X.509-aanvraag voor Certificaatondertekening genereren. De ondertekening aanvraag kan worden ondertekend door uw registratie-instantie of de certificeringsinstantie (CA). De ondertekende x509 certificaat kan worden samengevoegd met de in behandeling sleutel koppelen het certificaat KV in de Sleutelkluis. Hoewel deze methode zijn meer stappen vereist, biedt dit u betere beveiliging omdat de persoonlijke sleutel is gemaakt en tot de Sleutelkluis beperkt. Dit wordt uitgelegd in het onderstaande diagram.  

![Een certificaat maken met uw eigen certificeringsinstantie](media/certificate-authority-1.png)  

De volgende beschrijvingen komen overeen met de groene letters stappen in het voorgaande diagram.

1. In het bovenstaande diagram maakt in uw toepassing een certificaat dat intern begint met het maken van een sleutel in de sleutelkluis.
2. Sleutelkluis retourneert aan uw toepassing een Certificate Signing Request (CSR)
3. Uw toepassing geeft de CSR aan de door u gekozen CA.
4. De door u gekozen CA reageert met een een X509 certificaat.
5. Uw toepassing is voltooid voor het nieuwe certificaat maken met een fusie van de X509 certificaat van uw Certificeringsinstantie.

-   **Een certificaat maken met een bekende verlener provider:** deze methode moet u een eenmalige taak voor het maken van een verlener-object. Zodra een verlener-object is gemaakt in uw sleutel kluis, de naam ervan in het beleid van het certificaat KV kan worden verwezen. Een aanvraag voor het maken van zo'n KV certificaat maakt een sleutelpaar in de kluis en communiceren met de verlener provider-service met behulp van de informatie in het object waarnaar wordt verwezen verlener ophalen van een x509 certificaat. De x509 certificaat wordt opgehaald uit de service verlener en wordt samengevoegd met het sleutelpaar voltooid de KV-certificaat maken.  

![Een certificaat maken met een certificeringsinstantie Sleutelkluis samenwerking](media/certificate-authority-2.png)  

De volgende beschrijvingen komen overeen met de groene letters stappen in het voorgaande diagram.

1. In het bovenstaande diagram maakt in uw toepassing een certificaat dat intern begint met het maken van een sleutel in de sleutelkluis.
2. Sleutelkluis verzendt en SSL-certificaat aanvragen bij de Certificeringsinstantie.
3. Uw toepassing worden opgevraagd, in een proces lus en wacht voor uw Sleutelkluis voor voltooiing van het certificaat. Het certificaat is gemaakt wanneer Sleutelkluis van de CA-antwoord met x509 ontvangt certificaat.
4. De CA van de Sleutelkluis SSL-certificaat aanvragen met een X509 reageert SSL-certificaat.
5. Het maken van uw nieuwe certificaat is voltooid met de fusie van de X509 certificaat voor de CA.

## <a name="asynchronous-process"></a>Asynchroon proces
Maken van het certificaat KV is een asynchrone proces. Deze bewerking maakt een certificaataanvraag KV en retourneren van een HTTP-statuscode 202 (geaccepteerde). De status van de aanvraag kan worden gevolgd door de in behandeling object wordt gemaakt door deze bewerking. De locatie-header wordt geretourneerd als de volledige URI van het object dat in behandeling.  

Wanneer een aanvraag voor het maken van een certificaat KV is voltooid, wordt de status van het object dat in behandeling wordt gewijzigd in 'voltooid' uit 'inprogress' en een nieuwe versie van het certificaat KV wordt gemaakt. Hiermee worden de huidige versie.  

## <a name="first-creation"></a>Eerste maken
 Wanneer een certificaat KV voor het eerst wordt gemaakt, is ook een adresseerbare sleutel en geheime gemaakt met dezelfde naam als die van het certificaat. Als de naam al in gebruik is, mislukt de bewerking met een HTTP-statuscode van 409 (conflict).
De adresseerbare sleutel en geheime meer ontvangen hun kenmerken van de kenmerken van KV certificaat. De adresseerbare sleutel en geheime gemaakt op deze manier zijn gemarkeerd als beheerde sleutels en geheimen, waarvan de levensduur wordt beheerd door de Sleutelkluis. Beheerde sleutels en geheimen zijn alleen-lezen. Opmerking: Als een KV-certificaat is verlopen of is uitgeschakeld, de bijbehorende sleutel en geheime wordt niet meer worden gebruikt.  

 Als dit de eerste bewerking voor het maken van een certificaat KV zijn een beleid is vereist.  Een beleid kan ook worden opgegeven met opeenvolgende bewerkingen ter vervanging van de beleid-resource maakt. Als een beleid niet opgegeven is, wordt de beleidsbron op de service gebruikt voor een volgende versie van KV certificaat maken. Denk eraan dat wanneer een aanvraag voor het maken van een volgende versie wordt uitgevoerd, de huidige KV certificaat en de bijbehorende adresseerbare sleutel en geheim, blijven ongewijzigd.  

## <a name="self-issued-certificate"></a>Zelf-verleende certificaat
 Als u een zelf-verleende certificaat, stelt de naam van de certificaatverlener als 'Self' in het certificaatbeleid zoals weergegeven in het volgende fragment van certificaatbeleid.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 Als de naam van de certificaatverlener niet is opgegeven, wordt de naam van de certificaatverlener ingesteld op 'Onbekend'. Als 'Onbekend' certificaatverlener is de eigenaar van het certificaat moet een x509 handmatig ophalen certificaat van de verlener van stelt zijn/haar keuze en vervolgens de openbare x509 samenvoegen certificaat met het certificaat van de sleutelkluis in behandeling zijnde-object op voor het maken van het certificaat niet voltooien.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>Waarmee een relatie bestaat CA-Providers
Maken van het certificaat kan worden voltooid handmatig of met een 'zelf' verlener. Sleutelkluis partners ook met bepaalde providers verlener voor het vereenvoudigen van het maken van certificaten. De volgende typen certificaten kunnen worden besteld voor sleutelkluis met deze partner verlener providers.  

|Provider|Certificaattype|  
|--------------|----------------------|  
|DigiCert|Sleutelkluis biedt OV of EV SSL-certificaten met DigiCert|
|GlobalCert|Sleutelkluis biedt OV of EV SSL-certificaten met GlobalSign|

 Zie voor meer informatie, waaronder geografische beschikbaarheid van deze providers verlener [certificaatverleners](/rest/api/keyvault/certificate-issuers.md).

Houd er rekening mee dat wanneer een order wordt geplaatst met de provider van de uitgever, het kan inwilligen of certificaatuitbreidingen overschrijven de x509 en de geldigheidsduur van certificaat op basis van het type certificaat.  

 Autorisatie: Vereist de machtiging Certificaten/maken.

 ## <a name="see-also"></a>Zie ook
 - [Over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
 - [Bewaken en beheren van certificaat maken](create-certificate-scenarios.md)
