---
title: De Azure MFA NPS-uitbreiding configureren | Microsoft Docs
description: Nadat u de extensie NPS, volg deze stappen voor geavanceerde configuratie zoals IP-whitelisting en UPN vervanging.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: ef922668f080b8f02f07c2f9724f5a98171fb754
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Geavanceerde configuratieopties voor de NPS-extensie voor multi-factor Authentication

De extensie Network Policy Server (NPS) breidt uw cloud-gebaseerde Azure multi-factor Authentication-functies in uw on-premises infrastructuur. In dit artikel wordt ervan uitgegaan dat u al de extensie die is geïnstalleerd hebben en wilt weten hoe de extensie aanpassen voor uw behoeften. 

## <a name="alternate-login-id"></a>Alternatieve aanmeldings-ID

Aangezien de NPS-extensie maakt verbinding met uw on-premises en de cloud-adreslijsten, kunnen er een probleem waarbij uw lokale UPN-namen (UPN's) komen niet overeen met de namen in de cloud. U lost dit probleem, alternatieve aanmeldings-id's te gebruiken. 

Met de extensie NPS, kunt u een Active Directory-kenmerk moet worden gebruikt in plaats van de UPN voor Azure multi-factor Authentication aanwijzen. Hiermee kunt u bij de bescherming van uw lokale bronnen met verificatie in twee stappen zonder het wijzigen van uw lokale UPN's. 

Alternatieve aanmeldings-id's configureren, gaat u naar `HKLM\SOFTWARE\Microsoft\AzureMfa` en bewerkt u de volgende registerwaarden:

| Naam | Type | Standaardwaarde | Beschrijving |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | Tekenreeks | leeg | De naam van Active Directory-kenmerk dat u wilt gebruiken in plaats van de UPN aangegeven. Dit kenmerk wordt gebruikt als het kenmerk AlternateLoginId. Als deze registerwaarde is ingesteld op een [geldig Active Directory-kenmerk](https://msdn.microsoft.com/library/ms675090.aspx) (voor bijvoorbeeld e-mail of weergavenaam), klikt u vervolgens de waarde van het kenmerk wordt gebruikt in plaats van de UPN van de gebruiker voor verificatie. Als deze registerwaarde leeg of niet is is geconfigureerd, klikt u vervolgens AlternateLoginId is uitgeschakeld en de UPN van de gebruiker wordt gebruikt voor verificatie. |
| LDAP_FORCE_GLOBAL_CATALOG | Booleaanse waarde | False | Deze optie om af te dwingen het gebruik van de globale catalogus voor LDAP-zoekopdrachten bij het opzoeken van AlternateLoginId gebruikt. Een domeincontroller configureren als een globale catalogus, het kenmerk AlternateLoginId toevoegen aan de globale catalogus en vervolgens deze optie inschakelen. <br><br> Als LDAP_LOOKUP_FORESTS is geconfigureerd (geen lege) **deze vlag wordt afgedwongen als waar**, ongeacht de waarde van de registerinstelling. De NPS-extensie is in dit geval wordt de globale catalogus worden geconfigureerd met het kenmerk AlternateLoginId voor elk forest vereist. |
| LDAP_LOOKUP_FORESTS | Tekenreeks | leeg | Geef een puntkomma's gescheiden lijst van forests om te zoeken. Bijvoorbeeld: *contoso.com;foobar.com*. Als deze registerwaarde is geconfigureerd, zoekt de NPS-extensie alle forests iteratief in de volgorde waarin ze zijn vermeld, en retourneert de eerste geslaagde AlternateLoginId-waarde. Als deze registerwaarde niet is geconfigureerd, wordt de zoekopdracht AlternateLoginId beperkt tot het huidige domein.|

Voor het oplossen van problemen met alternatieve aanmeldings-id's, kunt u de aanbevolen stappen voor [alternatieve aanmeldings-ID fouten](multi-factor-authentication-nps-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP-uitzonderingen

Als u bewaken van de beschikbaarheid van de server wilt, zoals als netwerktaakverdelers welke servers worden uitgevoerd controleren voordat u werkbelastingen, verzendt wilt u niet dat deze controles wordt geblokkeerd door de verificatie-aanvragen. In plaats daarvan een lijst met IP-adressen die u kent worden gebruikt door de service-accounts maken en vereisten van de multi-factor Authentication voor die lijst uitschakelen. 

Voor het configureren van een lijst met geaccepteerde IP-adressen, gaat u naar `HKLM\SOFTWARE\Microsoft\AzureMfa` en configureer de volgende registerwaarde: 

| Naam | Type | Standaardwaarde | Beschrijving |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | Tekenreeks | leeg | Geef een puntkomma's gescheiden lijst van IP-adressen. De IP-adressen van computers waar serviceaanvragen afkomstig, zoals de NAS/VPN-server zijn bevatten. IP-adresbereiken zijn subnetten worden niet ondersteund. <br><br> Bijvoorbeeld: *10.0.0.1;10.0.0.2;10.0.0.3*.

Wanneer een aanvraag afkomstig is van een IP-adres die in de lijst met geaccepteerde voorkomt, wordt verificatie in twee stappen overgeslagen. De goedgekeurde IP-adressen wordt vergeleken met het IP-adres dat is opgegeven in de *ratNASIPAddress* kenmerk van de RADIUS-aanvraag. Als een RADIUS-aanvraag wordt geleverd zonder het kenmerk ratNASIPAddress, de volgende waarschuwing wordt vastgelegd: 'P_WHITE_LIST_WARNING::IP geaccepteerde wordt genegeerd als de bron-IP ontbreekt in de RADIUS-aanvraag in NasIpAddress-kenmerk.'

## <a name="next-steps"></a>Volgende stappen

[Resolve error messages from the NPS extension for Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md) (Foutberichten van de NPS-extensie voor Azure Multi-Factor Authentication oplossen)