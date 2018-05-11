---
title: Aan de slag met Sleutelkluis-certificaten
description: De volgende scenario's worden enkele van de primaire vormen van gebruik van een van de Sleutelkluis certificate management-service met inbegrip van de extra stappen vereist voor het maken van uw eerste certificaat in de sleutelkluis.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a788b958-3acb-4bb6-9c94-4776852aeea1
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 5f3b8a7b9c7bf582ebc2fac2be8ff55134fbc6f2
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-key-vault-certificates"></a>Aan de slag met Sleutelkluis-certificaten
De volgende scenario's worden enkele van de primaire vormen van gebruik van een van de Sleutelkluis certificate management-service met inbegrip van de extra stappen vereist voor het maken van uw eerste certificaat in de sleutelkluis.

Hieronder worden beschreven:
- Het maken van uw eerste Sleutelkluis-certificaat
- Maken van een certificaat bij een certificeringsinstantie die samenwerking met Sleutelkluis
- Maken van een certificaat bij een certificeringsinstantie die niet samenwerking met Sleutelkluis
- Een certificaat importeren

## <a name="certificates-are-complex-objects"></a>Certificaten zijn complexe objecten
Certificaten bestaan uit drie verwante resources die zijn gekoppeld als een certificaat voor Sleutelkluis; de certificaatmetagegevens van het, een sleutel en een geheim.


![Certificaten zijn complexe](media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>Het maken van uw eerste Sleutelkluis-certificaat  
 Voordat een certificaat kan worden gemaakt in een sleutel kluis (KV), de vereiste stappen 1 en 2 is moeten worden uitgevoerd en een sleutelkluis moet bestaan voor deze gebruiker / organisatie.  

**Stap 1** -Providers (certificeringsinstantie) van het certificaat  
-   Op twee locaties als de IT-beheerder, PKI-beheer of iemand anders het beheren van accounts met CA's, voor een opgegeven bedrijf (bijvoorbeeld Contoso) is vereist voor het gebruik van certificaten voor Sleutelkluis.  
    De volgende CA's zijn de huidige waarmee een relatie bestaat providers met Sleutelkluis:  
    -   DigiCert - Sleutelkluis biedt OV SSL-certificaten met DigiCert.  
    -   GlobalSign - Sleutelkluis biedt OV SSL-certificaten met GlobalSign  
    -   WoSign - Sleutelkluis aanbiedingen OV SSL of EV SSL-certificaten met WoSign op basis van de instelling geconfigureerd door de klant in hun account WoSign op de portal WoSign.  

**Stap 2** -beheerder van het account voor een CA-provider maakt referenties moeten worden gebruikt door een Sleutelkluis te registreren, vernieuwen en SSL-certificaten via Sleutelkluis gebruikt.

**Stap 3** -A Contoso admin, samen met een Contoso-werknemer (Sleutelkluis gebruiker) die eigenaar is van certificaten, afhankelijk van de CA kunt een certificaat verkrijgen van de beheerder of rechtstreeks van het account met de CA.  

-   Een bewerking van de referentie toevoegen om een sleutelkluis te beginnen met het maken van een [certificaatverlener](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers) resource. 
    -   Bijvoorbeeld MyDigiCertIssuer  
        -   Provider  
        -   Referenties – CA-accountreferenties. Elke CA heeft een eigen specifieke gegevens.  

     Zie voor meer informatie over het maken van accounts met CA-Providers, de verwante bericht op de [Sleutelkluis blog](http://aka.ms/kvcertsblog).  

**Stap 3.1** : Stel [contactpersonen van het certificaat](https://docs.microsoft.com/rest/api/keyvault/certificate-contacts) voor meldingen. Dit is de contactpersoon voor de Sleutelkluis-gebruiker. Sleutelkluis worden niet afgedwongen door deze stap.  

Opmerking: dit proces via stap 3.1, is een eenmalige bewerking.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Het maken van een certificaat met een CA in samenwerking met Sleutelkluis

![Een certificaat maken met een certificeringsinstantie Sleutelkluis samenwerking](media/certificate-authority-2.png)

**Stap 4** -de volgende beschrijvingen komen overeen met de groene genummerde stappen in het voorgaande diagram.  
  (1) - in het bovenstaande diagram uw toepassing een certificaat dat intern begint met het maken van een sleutel in uw sleutelkluis wordt gemaakt.  
  (2) - Sleutelkluis verzendt een SSL-certificaat aanvragen naar de CA.  
  (3) - de toepassing worden opgevraagd, in een proces lus en wacht voor uw Sleutelkluis voor voltooiing van het certificaat. Het certificaat is gemaakt wanneer Sleutelkluis van de CA-antwoord met x509 ontvangt certificaat.  
  (4) - de CA van de Sleutelkluis SSL-certificaat aanvragen met een X509 reageert SSL-certificaat.  
  (5) - het maken van uw nieuwe certificaat is voltooid met de fusie van de X509 certificaat voor de CA.  

  Sleutelkluis gebruiker – een certificaat gemaakt door te geven van een beleid

  -   Herhaal zo nodig  
  -   Beleidsbeperkingen  
      -   X509 eigenschappen  
      -   Sleuteleigenschappen  
      -   Referentie provider - > ex. MyDigiCertIssure  
      -   Vernieuwingsinformatie - > ex. 90 dagen vóór de verloopdatum  

  - Een proces voor het maken van een certificaat is meestal een asynchroon proces en omvat polling van de sleutelkluis voor de status van de bewerking van het certificaat maken.  
[De certificaatbewerking ophalen](https://docs.microsoft.com/en-us/rest/api/keyvault/getcertificateoperation) -Status: voltooid is, is mislukt met de informatie over de fout of, geannuleerd  
            -Vanwege van de vertraging voor het maken, kan een annuleringsbewerking worden gestart. Het annuleren al dan niet is toegestaan.  

## <a name="import-a-certificate"></a>Een certificaat importeren  
 U kunt ook: een certificaat kan worden geïmporteerd in Sleutelkluis – PFX of PEM.  

 Voor meer informatie over PEM-indeling, Zie de sectie certificaten van [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).  

 Certificaat importeren: vereist een PEM of PFX worden op schijf en een persoonlijke sleutel hebben. 
-   U moet opgeven: vault naam en de naam van het certificaat (het beleid is optioneel)

-   PEM / kenmerken die KV kan parseren en gebruik voor het vullen van het certificaatbeleid voor PFX-bestanden bevat. Als een certificaatbeleid al is opgegeven, KV zal proberen te matchen met gegevens van PFX / PEM-bestand.  

-   Nadat het importeren final is, verdere bewerkingen gebruik het nieuwe beleid (nieuwe versies).  

-   Als er geen verdere bewerkingen, is het eerste wat dat de Sleutelkluis heeft een bericht met een vervaldatum verzenden. 

-   Bovendien kan de gebruiker het beleid, die werkt op het moment van importeren, maar bevat standaardinstellingen waarbij er geen informatie is opgegeven tijdens het importeren bewerken. Bijvoorbeeld Er zijn geen gegevens van de certificaatverlener  

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Het maken van een certificaat met een Certificeringsinstantie die niet in samenwerking met Sleutelkluis  
 Deze methode kunt werken met andere CA's dan waarmee een relatie bestaat providers van de Sleutelkluis, wat betekent dat uw organisatie met een CA van de keuze kan werken.  

![Een certificaat maken met uw eigen certificeringsinstantie](media/certificate-authority-1.png)  

 De volgende stap beschrijvingen overeenkomen met de groene letters stappen in het voorgaande diagram.  

  (1) - in het bovenstaande diagram uw toepassing een certificaat dat intern begint met het maken van een sleutel in uw sleutelkluis wordt gemaakt.  

  (2) - Sleutelkluis retourneert aan uw toepassing een Certificate Signing Request (CSR).  

  (3) - de toepassing de CSR doorgegeven aan de door u gekozen CA.  

  (4) - de door u gekozen CA reageert met een X509 certificaat.  

  (5) - de toepassing is voltooid voor het nieuwe certificaat maken met een fusie van de X509 certificaat van uw Certificeringsinstantie.

## <a name="see-also"></a>Zie ook
- [Certificaat-bewerkingen](/rest/api/keyvault/certificate-operations.md)
- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
