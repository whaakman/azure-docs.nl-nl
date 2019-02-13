---
title: Configureren van de Azure MFA NPS-extensie | Microsoft Docs
description: Nadat u de NPS-extensie geïnstalleerd, gebruikt u deze stappen voor geavanceerde configuratie, zoals de UPN vervangen en IP-whitelists.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b236cc799a4ff84c3833f181ebec6305f1ec6942
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171313"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Geavanceerde configuratieopties voor de NPS-extensie voor meervoudige verificatie

De extensie voor Network Policy Server (NPS) breidt uw cloud-gebaseerde Azure multi-factor Authentication-functies in uw on-premises infrastructuur. In dit artikel wordt ervan uitgegaan dat u al de extensie geïnstalleerd hebben en wilt weten hoe u de extensie voor uw behoeften aanpassen. 

## <a name="alternate-login-id"></a>Alternatieve aanmeldings-ID

Omdat de NPS-extensie maakt verbinding met uw on-premises en de cloud-adreslijsten, u mogelijk een probleem ondervindt waar in uw on-premises UPN-namen (UPN) niet overeenkomen met de namen in de cloud. U lost dit probleem, gebruik van alternatieve aanmeldings-id. 

In de NPS-extensie kunt u een Active Directory-kenmerk moet worden gebruikt in plaats van de UPN voor Azure multi-factor Authentication aanwijzen. Hiermee kunt u uw on-premises resources beveiligen met verificatie in twee stappen zonder te wijzigen van uw on-premises UPN's. 

Voor het configureren van alternatieve aanmeldings-id, gaat u naar `HKLM\SOFTWARE\Microsoft\AzureMfa` en bewerken van de volgende registerwaarden:

| Name | Type | Standaardwaarde | Description |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Leeg | De naam van Active Directory-kenmerk dat u wilt gebruiken in plaats van de UPN wordt aangegeven. Dit kenmerk wordt gebruikt als het kenmerk AlternateLoginId. Als deze registerwaarde is ingesteld op een [geldig Active Directory-kenmerk](https://msdn.microsoft.com/library/ms675090.aspx) (voor bijvoorbeeld e-mail of naam), klikt u vervolgens de waarde van het kenmerk wordt gebruikt in plaats van de UPN van de gebruiker voor verificatie. Als deze registerwaarde leeg of niet is is geconfigureerd, klikt u vervolgens AlternateLoginId is uitgeschakeld en de UPN van de gebruiker wordt gebruikt voor verificatie. |
| LDAP_FORCE_GLOBAL_CATALOG | booleaans | False | Gebruik deze eigenschap om af te dwingen van het gebruik van globale catalogus LDAP-zoekopdrachten wanneer AlternateLoginId opzoeken. Een domeincontroller configureren als een globale catalogus, het kenmerk AlternateLoginId toevoegen aan de globale catalogus en vervolgens deze optie inschakelen. <br><br> Als LDAP_LOOKUP_FORESTS is geconfigureerd (niet leeg), **deze vlag wordt afgedwongen als waar**, ongeacht de waarde van de instelling in het register. De NPS-extensie is in dit geval de globale catalogus kan worden geconfigureerd met het kenmerk AlternateLoginId voor elk forest vereist. |
| LDAP_LOOKUP_FORESTS | string | Leeg | Geef een door puntkomma's gescheiden lijst met forests om te zoeken. Bijvoorbeeld, *contoso.com;foobar.com*. Als deze registerwaarde is geconfigureerd, zoekt de NPS-extensie alle forests iteratief in de volgorde waarin ze werden weergegeven en retourneert de eerste geslaagde AlternateLoginId-waarde. Als deze registerwaarde is niet geconfigureerd, wordt de zoekopdracht AlternateLoginId beperkt tot het huidige domein.|

Gebruik de aanbevolen stappen voor het oplossen van problemen met alternatieve aanmeldings-id, [alternatieve aanmeldings-ID fouten](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP-uitzonderingen

Als u nodig hebt voor het bewaken van de beschikbaarheid van de server, zoals als netwerktaakverdelers welke servers worden uitgevoerd controleren voordat u werkbelastingen, verzendt wilt u niet dat deze controles door verificatie aanvragen worden geblokkeerd. In plaats daarvan maakt u een lijst met IP-adressen die worden gebruikt door de service-accounts en uitschakelen van multi-factor Authentication-vereisten voor die lijst. 

Als u wilt een goedgekeurde IP-adressen configureren, gaat u naar `HKLM\SOFTWARE\Microsoft\AzureMfa` en configureer de volgende registerwaarde: 

| Name | Type | Standaardwaarde | Description |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Leeg | Geef een door puntkomma's gescheiden lijst met IP-adressen. Neem het IP-adressen van machines waar aanvragen afkomstig, zoals de NAS/VPN-server zijn. IP-bereiken zijn subnetten worden niet ondersteund. <br><br> Bijvoorbeeld, *10.0.0.1;10.0.0.2;10.0.0.3*.

Wanneer een aanvraag gaat vanaf een IP-adres dat in de whitelist staan bestaat, wordt verificatie in twee stappen wordt overgeslagen. De goedgekeurde IP-adressen wordt vergeleken met het IP-adres dat is opgegeven in de *ratNASIPAddress* kenmerk van de RADIUS-aanvraag. Als een RADIUS-aanvraag zonder het kenmerk ratNASIPAddress komt, wordt de volgende waarschuwing vastgelegd: "P_WHITE_LIST_WARNING::IP Whitelist wordt genegeerd als de bron-IP ontbreekt in de RADIUS-aanvraag in NasIpAddress kenmerk."

## <a name="next-steps"></a>Volgende stappen

[Resolve error messages from the NPS extension for Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md) (Foutberichten van de NPS-extensie voor Azure Multi-Factor Authentication oplossen)
