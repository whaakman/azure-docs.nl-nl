---
title: "Genereren van certificaten voor de implementatie van Azure-Stack geïntegreerd systemen Azure Stack Public Key Infrastructure | Microsoft Docs"
description: "Hierin wordt beschreven in de Azure-Stack PKI-certificaat implementatie processfor Azure Stack geïntegreerd-systemen."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: e7edbc1c4aa6e3cb1026d493886ef7ca704b9131
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="generate-pki-certificates-for-azure-stack-deployment"></a>PKI-certificaten voor de implementatie van Azure-Stack genereren
Als u weet [PKI-certificaatvereisten](azure-stack-pki-certs.md) voor Azure-Stack-implementaties, moet u deze certificaten verkrijgen van de certificeringsinstantie (CA) van uw keuze. 

## <a name="request-certificates-using-an-inf-file"></a>Certificaten aanvragen met een INF-bestand
Een manier om aanvragen van certificaten vanuit een openbare CA of een interne Certificeringsinstantie is via een INF-bestand. Het hulpprogramma Windows ingebouwde certreq.exe kunt u een INF-bestand opgeven van details over deze certificaat gebruiken voor het genereren van een voor aanvraagbestand zoals beschreven in deze sectie. 

### <a name="sample-inf-file"></a>INF-bestand 
Het voorbeeld certificaat aanvraag INF-bestand kan worden gebruikt voor het maken van een bestand offline certificaataanvraag te verzenden naar een CA (intern of openbaar). Het INF-bestand bevat alle van de vereiste eindpunten (inclusief de optionele PaaS-services) in een enkel jokertekencertificaat. 

Het INF-bestand van het voorbeeld wordt ervan uitgegaan dat regio is gelijk aan **sea** en de externe FQDN-waarde is **sea &#46; contoso &#46; com**. Wijzig deze waarden voor uw omgeving vóór het genereren van een. INF-bestand voor uw implementatie. 

    
    [Version] 
    Signature="$Windows NT$"

    [NewRequest] 
    Subject = "C=US, O=Microsoft, L=Redmond, ST=Washington, CN=portal.sea.contoso.com"

    Exportable = TRUE                   ; Private key is not exportable 
    KeyLength = 2048                    ; Common key sizes: 512, 1024, 2048, 4096, 8192, 16384 
    KeySpec = 1                         ; AT_KEYEXCHANGE 
    KeyUsage = 0xA0                     ; Digital Signature, Key Encipherment 
    MachineKeySet = True                ; The key belongs to the local computer account 
    ProviderName = "Microsoft RSA SChannel Cryptographic Provider" 
    ProviderType = 12 
    SMIME = FALSE 
    RequestType = PKCS10
    HashAlgorithm = SHA256

    ; At least certreq.exe shipping with Windows Vista/Server 2008 is required to interpret the [Strings] and [Extensions] sections below

    [Strings] 
    szOID_SUBJECT_ALT_NAME2 = "2.5.29.17" 
    szOID_ENHANCED_KEY_USAGE = "2.5.29.37" 
    szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1" 
    szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

    [Extensions] 
    %szOID_SUBJECT_ALT_NAME2% = "{text}dns=*.sea.contoso.com&dns=*.blob.sea.contoso.com&dns=*.queue.sea.contoso.com&dns=*.table.sea.contoso.com&dns=*.vault.sea.contoso.com&dns=*.adminvault.sea.contoso.com&dns=*.dbadapter.sea.contoso.com&dns=*.appservice.sea.contoso.com&dns=*.scm.appservice.sea.contoso.com&dns=api.appservice.sea.contoso.com&dns=ftp.appservice.sea.contoso.com&dns=sso.appservice.sea.contoso.com&dns=adminportal.sea.contoso.com&dns=management.sea.contoso.com&dns=adminmanagement.sea.contoso.com" 
    %szOID_ENHANCED_KEY_USAGE% = "{text}%szOID_PKIX_KP_SERVER_AUTH%,%szOID_PKIX_KP_CLIENT_AUTH%"

    [RequestAttributes]
    

## <a name="generate-and-submit-request-to-the-ca"></a>Genereren en indienen verzoek aan de Certificeringsinstantie
De volgende werkstroom wordt beschreven hoe u kunt aanpassen en het INF-bestand eerder gegenereerd om aan te vragen van een certificaat van een CA te gebruiken:

1. **Bewerken en opslaan van INF-bestand**. Kopieer het voorbeeld opgeven en deze opslaan op een nieuw tekstbestand. De onderwerpnaam en externe FQDN vervangen door de waarden die overeenkomen met uw implementatie en het bestand opslaan als een. INF-bestand.
2. **Genereren van een aanvraag met certreq**. Met behulp van een Windows-computer, start een opdrachtprompt als beheerder en voer de volgende opdracht om een (.req)-aanvraagbestand te genereren: `certreq -new <yourinffile>.inf <yourreqfilename>.req`.
3. **Bij de Certificeringsinstantie indienen**. Verzenden van de. Vereist bestand is gegenereerd aan uw CA (kan worden intern of openbaar).
4. **Importeren. CER**. De CA-retourneert een. CER-bestand. Importeren met behulp van dezelfde Windows-computer van waaruit u het aanvraagbestand gegenereerd, de. CER-bestand geretourneerd aan de computer/personal store. 
5. **Exporteren en kopiëren. PFX naar implementatiemappen**. Het certificaat (met inbegrip van de persoonlijke sleutel) exporteren als een. PFX-bestand en kopieer de. PFX-bestand voor de implementatiemappen die worden beschreven [Azure Stack PKI implementatievereisten](azure-stack-pki-certs.md).

