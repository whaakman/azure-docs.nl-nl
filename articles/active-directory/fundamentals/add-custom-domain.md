---
title: Uw aangepaste domein toevoegen aan Azure Active Directory | Microsoft Docs
description: Informatie over het toevoegen van een aangepast domein met behulp van de Azure Active Directory-portal.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: lizross
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: b33f2e809ae5758e41f7a76680347b9487f3f461
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735325"
---
# <a name="how-to-add-your-custom-domain-name-using-the-azure-active-directory-portal"></a>Hoe: de naam van uw aangepaste domein met behulp van de Azure Active Directory-portal toevoegen
Elke nieuwe Azure AD-tenant wordt geleverd met een initiële domeinnaam *domainname*. onmicrosoft.com. U niet wijzigen of verwijderen van de initiële domeinnaam, maar u kunt de namen van uw organisatie toevoegen aan de lijst. Toevoegen van aangepaste domeinnamen, helpt u bij het maken van de gebruikersnamen die bekend bij uw gebruikers, zoals zijn _alain@contoso.com_.

>[!Note]
>U moet dit hele proces, van begin tot eind, voor elk van uw aangepaste domeinnamen herhalen.

## <a name="add-a-custom-domain-name"></a>Een aangepaste domeinnaam toevoegen
U moet eerst uw aangepaste domeinnaam toevoegen aan de Azure AD-tenant.

### <a name="to-add-a-custom-domain-name"></a>Een aangepaste domeinnaam toevoegen
1. Aanmelden bij de [Azure AD-portal](https://portal.azure.com/) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, selecteer **aangepaste-domeinnamen**, en selecteer vervolgens **aangepast domein toevoegen**.

    ![Fabrikam - aangepast domein namen blade met de optie aangepast domein toevoegen gemarkeerd](media/add-custom-domain/add-custom-domain.png)

3. Typ de naam van uw nieuwe bedrijfsdomein in de **aangepaste domeinnaam** vak (bijvoorbeeld _contoso.com_), en selecteer vervolgens **domein toevoegen**.

    >[!Important]
    >U moet opnemen .com, .net of andere op het hoogste niveau extensie voor deze goed te laten werken.

    ![Fabrikam - aangepast domein namen blade met domain-knop toevoegen gemarkeerd](media/add-custom-domain/add-custom-domain-blade.png)

4. Kopieer de gegevens van de DNS-vermelding van de **Contoso** blade.

    ![Contoso-blade met de gegevens van DNS-vermelding](media/add-custom-domain/contoso-blade-with-dns-info.png)

## <a name="add-your-domain-name-with-a-domain-name-registrar"></a>Uw domeinnaam met een domeinnaamregistrar toevoegen
Vervolgens moet u de DNS-zonebestand voor uw nieuwe aangepaste domein bijwerken. U kunt [DNS-zones](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) voor uw Azure, Office 365, of externe DNS-records, of u de nieuwe DNS-vermelding met behulp van een andere DNS-registratieservice kunt toevoegen (bijvoorbeeld [InterNIC](https://go.microsoft.com/fwlink/p/?LinkId=402770)).

### <a name="to-add-your-domain-name"></a>Uw domeinnaam toevoegen 
1. Aanmelden bij de domeinnaamregistrar voor uw aangepaste domein. Als u niet de juiste machtigingen voor het bijwerken van uw inzending hebt, moet u contact opnemen met iemand met de juiste machtigingen.

2. Nadat de DNS-vermelding is bijgewerkt met de registrar, moet u de DNS-zonebestand bijwerken met de informatie die wordt geleverd door Azure AD.

    >[!Note]
    >De DNS-vermelding verandert niet de werking van uw mailroutering of webhosting.

## <a name="verify-your-custom-domain-name"></a>Controleer of de naam van uw aangepaste domein
Nadat u uw aangepaste domeinnaam hebt geregistreerd, duurt het een paar seconden aan een paar uur voordat de DNS-gegevens doorgegeven aan waar Azure AD dit als geldig zien kunt.

### <a name="to-verify-your-custom-domain-name"></a>Om te controleren of uw aangepaste domeinnaam
1. Aanmelden bij de [Azure AD-portal](https://portal.azure.com/) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **aangepaste-domeinnamen**.

3. Op de **Fabrikam - aangepaste-domeinnamen** blade, selecteert u de aangepaste domeinnaam **Contoso**.

    ![Fabrikam - aangepast domein namen blade met contoso gemarkeerd](media/add-custom-domain/custom-blade-with-contoso-highlighted.png)

4. Op de **Contoso** Selecteer **controleren** om te controleren of uw aangepaste domein juist is geregistreerd en is geldig voor Azure AD.

    ![Contoso-blade met DNS-vermeldingsgegevens en de knop controleren](media/add-custom-domain/contoso-blade-with-dns-info-verify.png)

### <a name="common-verification-issues"></a>Veelvoorkomende problemen met verificatie
Als Azure AD een aangepaste domeinnaam niet verifiëren kan, probeert u de volgende suggesties:
- **Wacht ten minste een uur en probeer het opnieuw**. DNS-records moeten worden doorgegeven voordat Azure AD kunt controleren of dat het domein en dit proces kunnen een uur of langer duren.

- **Zorg ervoor dat de DNS-record klopt.** Ga terug naar de site domein naam registrar en zorg ervoor dat de vermelding wordt weergegeven, en dat deze overeenkomt met de DNS-vermeldingsgegevens geleverd door Azure AD.

    Als u de record op de site registrar niet bijwerken, moet u de vermelding delen met iemand die de juiste machtigingen voor de vermelding toevoegen en controleer of dat deze juist is.

- **Zorg ervoor dat de domeinnaam niet al in een andere map.** De naam van een domein kan alleen worden geverifieerd in een bepaalde map, wat betekent dat als de domeinnaam van uw is momenteel geverifieerd in een andere directory, het kan ook worden geverifieerd in de nieuwe map. U lost dit probleem duplicatie, moet u de domeinnaam verwijderen uit de oude map. Zie voor meer informatie over het verwijderen van domeinnamen [aangepaste domeinnamen beheren](../users-groups-roles/domains-manage.md).    

## <a name="next-steps"></a>Volgende stappen
- Gebruikers toevoegen aan uw domein, Zie [aangepaste domeinnamen beheren](../users-groups-roles/domains-manage.md).

- Als u beschikt over on-premises versies van Windows-Server die u wilt gebruiken samen met Azure Active Directory, raadpleegt u [uw on-premises directory's integreren met Azure Active Directory](../connect/active-directory-aadconnect.md).