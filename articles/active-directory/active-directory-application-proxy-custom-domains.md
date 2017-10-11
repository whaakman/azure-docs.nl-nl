---
title: Aangepaste domeinen in Azure AD-toepassingsproxy | Microsoft Docs
description: Aangepaste domeinen in Azure AD-toepassingsproxy beheren zodat de URL voor de app hetzelfde is, ongeacht waar uw gebruikers toegang toe.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1dde300780c8d1f7ea9eee4c92de06bcf70a1f12
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Werken met aangepaste domeinen in Azure AD-toepassingsproxy

Wanneer u een toepassing via Azure Active Directory-toepassingsproxy publiceert, maakt u een externe URL voor uw gebruikers gaan wanneer ze extern werken. Deze URL wordt het standaarddomein *yourtenant.msappproxy.net*. Bijvoorbeeld, als u een app met de naam uitgaven en uw tenant gepubliceerd is met de naam Contoso, de externe URL https://expenses-contoso.msappproxy.net zou zijn. Als u wilt om uw eigen domeinnaam te gebruiken, moet u een aangepast domein voor uw toepassing configureren. 

Het is raadzaam dat u instelt aangepaste domeinen voor uw toepassingen indien mogelijk. Enkele van de voordelen van aangepaste domeinen:

- Uw gebruikers toegang krijgen tot de toepassing met dezelfde URL, of ze binnen of buiten uw netwerk werken.
- Als al uw toepassingen dezelfde interne en externe URL's hebt, klikt u vervolgens blijven koppelingen in één toepassing die naar een andere verwijzen werken zelfs buiten het bedrijfsnetwerk. 
- U uw huisstijl beheren en de URL's die u wilt maken. 


## <a name="configure-a-custom-domain"></a>Een aangepast domein configureren

### <a name="prerequisites"></a>Vereisten

Voordat u een aangepast domein configureren, zorg ervoor dat u de volgende vereisten voorbereid hebt: 
- Een [gecontroleerde domein is toegevoegd aan Azure Active Directory](active-directory-domains-add-azure-portal.md).
- Een aangepaste certificaat voor het domein, in de vorm van een PFX-bestand. 
- Een lokale app [gepubliceerd via toepassingsproxy](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Uw aangepaste domein configureren

Wanneer u deze drie vereisten gereed hebt, volg deze stappen voor het instellen van uw aangepaste domein:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** en kies de app die u wilt beheren.
3. Selecteer **toepassingsproxy**. 
4. Gebruik de vervolgkeuzelijst om uw aangepaste domein in het veld externe URL. Als u uw domein in de lijst niet ziet, nog niet klikt u vervolgens deze geverifieerd nog. 
5. Selecteer **opslaan**
5. De **certificaat** veld dat is uitgeschakeld, wordt ingeschakeld. Selecteer dit veld. 

   ![Klik hier om een certificaat te uploaden](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Als u al een certificaat voor dit domein hebt geüpload, geeft de certificaatinformatie in het certificaatveld weer. 

6. Het PFX-certificaat uploaden en voer het wachtwoord voor het certificaat. 
7. Selecteer **opslaan** uw wijzigingen op te slaan. 
8. Voeg een [DNS-record](../dns/dns-operations-recordsets-portal.md) die de nieuwe externe URL leidt om naar het domein msappproxy.net. 

>[!TIP] 
>U hoeft alleen te uploaden één certificaat per aangepast domein. Wanneer u een certificaat uploaden, kunt u het aangepaste domein wanneer u een nieuwe app publiceert en geen aanvullende instellingen, met uitzondering van de DNS-record. 

## <a name="manage-certificates"></a>Certificaten beheren

### <a name="certificate-format"></a>De indeling van certificaat
Er is geen beperking voor de methoden van de handtekening certificaat. Alle worden Elliptic Curve Cryptography (ECC), SAN Subject Alternative Name () en andere algemene typen certificaten ondersteund. 

U kunt een jokertekencertificaat, zolang het jokerteken overeenkomt met de gewenste externe URL. 

U kunt zelf-ondertekende certificaten, evenals gebruiken. Als u een persoonlijke certificeringsinstantie, moet het CRL-Distributiepunt (certificaat intrekken point-distributiepunt) voor het certificaat openbaar zijn.

### <a name="changing-the-domain"></a>Het wijzigen van het domein
Alle geverifieerde domeinen worden weergegeven in de vervolgkeuzelijst van de externe URL voor uw toepassing. Om te wijzigen van het domein, moet u zojuist hebt dat veld voor de toepassing bijwerken. Als het domein dat u wilt dat niet in de lijst [toevoegen als een geverifieerde domeinnaam](active-directory-domains-add-azure-portal.md). Als u een domein dat niet heeft een bijbehorende certificaat nog stap 5-7 het certificaat toevoegen Volg selecteert. Controleer vervolgens of dat u de DNS-record om te leiden van de nieuwe externe URL bijwerken. 

### <a name="certificate-management"></a>Certificaatbeheer
U kunt hetzelfde certificaat gebruiken voor meerdere toepassingen, tenzij de toepassingen een externe host delen. 

U krijgt een waarschuwing wanneer een certificaat is verlopen, dat u een ander certificaat via de portal uploaden. Als het certificaat is ingetrokken, kunnen uw gebruikers een beveiligingswaarschuwing zien bij het openen van de toepassing. We uitvoeren geen intrekkingscontroles voor certificaten.  Navigeer naar de toepassing en volg de stappen 5 tot en met 7 voor het configureren van aangepaste domeinen op gepubliceerde toepassingen voor het uploaden van een nieuw certificaat voor het bijwerken van het certificaat voor een bepaalde toepassing. Als het oude certificaat niet door andere toepassingen gebruikt wordt, wordt deze automatisch verwijderd. 

Alle Certificaatbeheer is momenteel door afzonderlijke toepassing pagina's er daarom voor het beheren van certificaten in de context van de relevante toepassingen. 

## <a name="next-steps"></a>Volgende stappen
* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md) naar uw gepubliceerde apps met Azure AD-verificatie.
* [Inschakelen van voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md) naar uw gepubliceerde apps.
* [Uw aangepaste domeinnaam toevoegen aan Azure AD](active-directory-domains-add-azure-portal.md)


