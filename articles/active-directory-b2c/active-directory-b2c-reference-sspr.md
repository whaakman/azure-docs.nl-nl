---
title: Self-service voor wachtwoord opnieuw instellen in Azure Active Directory B2C | Microsoft Docs
description: Ziet u hoe u voor het instellen van self-service voor wachtwoord opnieuw instellen voor uw klanten in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7dd68f409ad4c8130ed841ddc7ef5910d3b1945c
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52719739"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Instellen van self-service voor wachtwoord opnieuw instellen voor uw klanten
Uw klanten die zich hebben aangemeld voor lokale accounts kunnen hun wachtwoorden op hun eigen opnieuw instellen met de functie voor self-service voor wachtwoord opnieuw instellen. Dit vermindert aanzienlijk de werkbelasting van uw medewerkers, met name als uw toepassing heeft miljoenen klanten die gebruikmaken van deze regelmatig. Met behulp van een geverifieerde e-mailadres is momenteel de enige ondersteunde herstelmethode.

> [!NOTE]
> In dit artikel is van toepassing op de self-service voor wachtwoord opnieuw instellen die wordt gebruikt in de context van de V1 **aanmelden** gebruikersstroom, dat gebruikmaakt van **aanmelden met lokaal Account** als de id-provider. Als u volledig aanpasbare wachtwoord opnieuw instellen-gebruikersstromen aangeroepen vanuit uw app nodig hebt, raadpleegt u [in dit artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Uw directory geen standaard Self-service voor wachtwoord opnieuw instellen is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als beheerder van het abonnement. Dit is hetzelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt voor het maken van uw directory.
2. Open **Azure Active Directory** (in de navigatiebalk aan de linkerkant).
4. Stel **Self service voor wachtwoordherstel ingeschakeld** naar **alle**. 
5. Klik bovenaan de pagina op **Opslaan**. U bent klaar.

Als u wilt testen, gebruikt u de 'Nu uitvoeren'-functie op een gebruikersstroom aanmelden met lokale accounts als een id-provider. Op het lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **geen toegang tot uw account?** om te controleren of de ervaring van de klant.

> [!NOTE]
> De self-service voor wachtwoord opnieuw instellen van pagina's kunnen worden aangepast met behulp van de [functie huisstijl van bedrijf](../active-directory/fundamentals/customize-branding.md).
> 
> 

