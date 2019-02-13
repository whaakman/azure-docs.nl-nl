---
title: Aan de slag met Key Vault-certificaten
description: De volgende scenario's worden enkele van de primaire gebruik van Key Vault de certificate management-service met inbegrip van de extra stappen vereist voor het maken van uw eerste certificaat in uw key vault.
services: key-vault
documentationcenter: ''
author: bryanla
manager: barbkess
tags: azure-resource-manager
ms.assetid: a788b958-3acb-4bb6-9c94-4776852aeea1
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: aeba7473437e6cb7bfe8060b742ed1d6863047e9
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109211"
---
# <a name="get-started-with-key-vault-certificates"></a>Aan de slag met Key Vault-certificaten
De volgende scenario's worden enkele van de primaire gebruik van Key Vault de certificate management-service met inbegrip van de extra stappen vereist voor het maken van uw eerste certificaat in uw key vault.

Hieronder worden beschreven:
- Het maken van uw eerste Key Vault-certificaat
- Het maken van een certificaat met een certificeringsinstantie die wordt het in samenwerking met Key Vault
- Het maken van een certificaat met een certificeringsinstantie die is niet een partnerschap aangegaan met Key Vault
- Een certificaat importeren

## <a name="certificates-are-complex-objects"></a>Certificaten zijn complexe objecten
Certificaten worden samengesteld uit drie onderling verbonden resources samen als een Key Vault-certificaat gekoppeld het certificaat van de metagegevens van een sleutel en een geheim.


![Certificaten zijn complexe](media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>Het maken van uw eerste Key Vault-certificaat  
 Voordat een certificaat kan worden gemaakt in een Key Vault (KV), de vereiste stappen 1 en 2 moeten worden uitgevoerd en een key vault moet bestaan voor deze gebruiker / organisatie.  

**Stap 1** -Providers (certificeringsinstantie) van het certificaat  
-   Toevoegen als de IT-beheerder, beheerder van de PKI of iedereen beheren van accounts met CA's voor een opgegeven bedrijf (ex.) Contoso) is een vereiste voor het gebruik van Key Vault-certificaten.  
    De volgende CA's zijn de huidige hebben providers met Key Vault:  
    -   DigiCert - Key Vault biedt OV SSL-certificaten met DigiCert.  
    -   GlobalSign - Key Vault biedt OV SSL-certificaten met GlobalSign  
    -   WoSign - Key Vault biedt OV SSL of VW SSL-certificaten met WoSign op basis van de instelling die is geconfigureerd door de klant in hun WoSign-account in de portal WoSign.  

**Stap 2** -een accountbeheerder voor een CA-provider maakt referenties op die moeten worden gebruikt door Key Vault om te schrijven, vernieuwen en SSL-certificaten via Key Vault gebruiken.

**Stap 3** -een Contoso-beheerder, samen met een Contoso-werknemer (Key Vault-gebruiker) die eigenaar is van certificaten, afhankelijk van de CA kunt verkrijgen van een certificaat van de beheerder of rechtstreeks vanuit het account met de CA.  

-   Beginnen met een bewerking van de referentie toevoegen tot een key vault door [instellen van een certificaatverlener](/rest/api/keyvault/setcertificateissuer/setcertificateissuer) resource. Een certificaatverlener is een entiteit in Azure Key Vault (KV) als een resource CertificateIssuer weergegeven. Deze wordt gebruikt voor informatie over de oorzaak van een certificaat KV; naam van verlener, provider, referenties en andere administratieve informatie.
    -   Bijvoorbeeld MyDigiCertIssuer  
        -   Provider  
        -   Referenties: de referenties van de CA-account. Elke CA heeft een eigen specifieke gegevens.  

     Zie voor meer informatie over het maken van accounts met CA-Providers, het bijbehorende bericht op de [Key Vault-blog](https://aka.ms/kvcertsblog).  

**Stap 3.1** -instellen [contactpersonen van het certificaat](/rest/api/keyvault/setcertificatecontacts/setcertificatecontacts) voor meldingen. Dit is de contactpersoon voor de Key Vault-gebruiker. Deze stap worden niet afgedwongen door Key Vault.  

Opmerking: dit proces, via stap 3.1, is een eenmalige bewerking.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Het maken van een certificaat met een CA een partnerschap aangegaan met Key Vault

![Maak een certificaat met een Key Vault is een partnerschap aangegaan certificeringsinstantie](media/certificate-authority-2.png)

**Stap 4** -beschrijvingen van de volgende komen overeen met de groene genummerde stappen in het voorgaande diagram.  
  (1): in het bovenstaande diagram uw toepassing een certificaat dat intern wordt gestart met het maken van een sleutel in uw key vault maakt.  
  (2) - Key Vault stuurt een SSL-certificaat aanvragen naar de CA.  
  (3) - uw toepassing, in een lus en wait-proces, pollt voor uw Key Vault voor het certificaat is voltooid. Het maken van het certificaat is voltooid wanneer Key Vault van de CA-antwoord met x509 ontvangt certificaat.  
  (4) - de CA van de Sleutelkluis SSL-certificaat aanvragen met een X509 reageert SSL-certificaat.  
  (5) - het maken van het nieuwe certificaat is voltooid met de integratie van de X509 certificaat voor de CA.  

  Gebruikers van de Key Vault, wordt een certificaat gemaakt door een beleid op te geven

  -   Herhaal deze stappen zo nodig  
  -   Beleidsbeperkingen  
      -   X509 eigenschappen  
      -   Sleuteleigenschappen  
      -   Naslaginformatie voor de provider - > ex. MyDigiCertIssure  
      -   Informatie over verlenging - > ex. 90 dagen vóór de verloopdatum  

  - Proces voor het maken van een certificaat is meestal een asynchroon proces en omvat het opvragen van configuratiegegevens bij uw key vault voor de status van de certificaatbewerking maken.  
[Get-certificaatbewerking](/rest/api/keyvault/getcertificateoperation/getcertificateoperation)  
      -   Status: voltooid, is mislukt met de informatie over de fout of, geannuleerd  
      -   Vanwege de vertraging te maken, kan een annuleringsbewerking worden gestart. Het annuleren kan of kan niet worden van kracht.  

## <a name="import-a-certificate"></a>Een certificaat importeren  
 U kunt ook: een certificaat kan worden geïmporteerd in Key Vault – PFX of PEM.  

 Zie voor meer informatie over PEM-indeling, de sectie certificaten van [over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).  

 Certificaat importeren: vereist een PEM- of PFX worden op schijf en een persoonlijke sleutel hebben. 
-   U moet opgeven: naam en de naam van het certificaat-kluis (beleid is optioneel)

-   PEM / PFX-bestanden bevat kenmerken die KV kunt parseren en gebruiken voor het vullen van het certificaatbeleid. Als een certificaatbeleid al is opgegeven, KV wordt geprobeerd om gegevens van PFX te vergelijken / PEM-bestand.  

-   Nadat het importeren voltooid is, verdere bewerkingen gebruiken het nieuwe beleid (nieuwe versies).  

-   Als er verder geen bewerkingen, is het eerste wat dat de Key Vault biedt een melding over verlopen verzenden. 

-   De gebruiker kan ook het beleid, dat werkt op het moment van importeren, maar bevat standaardinstellingen waarbij er geen informatie is opgegeven tijdens het importeren bewerken. Bijvoorbeeld Er zijn geen gegevens van de verlener  

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Het maken van een certificaat met een CA niet een partnerschap aangegaan met Key Vault  
 Deze methode kunt werken met andere CA's dan van de Sleutelkluis hebben providers, wat betekent dat uw organisatie met een CA van de keuze kan werken.  

![Een certificaat maken met uw eigen certificeringsinstantie](media/certificate-authority-1.png)  

 De beschrijvingen van de volgende stap komen overeen met de groene letters stappen in het voorgaande diagram.  

  (1): in het bovenstaande diagram uw toepassing een certificaat dat intern wordt gestart met het maken van een sleutel in uw key vault maakt.  

  (2) - Key Vault wordt aan uw toepassing een Certificate Signing Request (CSR) worden geretourneerd.  

  (3) - de toepassing de CSR doorgegeven aan de door u gekozen CA.  

  (4) - de door u gekozen CA reageert met een X509 certificaat.  

  (5) - de toepassing klaar is voor het nieuwe certificaat maken met een fusie van de X509 certificaat van uw Certificeringsinstantie.

## <a name="see-also"></a>Zie ook

- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
