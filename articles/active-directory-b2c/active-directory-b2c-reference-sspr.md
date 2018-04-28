---
title: Selfservice voor wachtwoordherstel | Microsoft Docs
description: Demonstreert hoe u voor het instellen van de selfservice voor wachtwoordherstel voor uw klanten in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/17/2018
ms.author: davidmu
ms.openlocfilehash: 5b75455ad604b594a5f85fea8299d35a7d02c848
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Selfservice voor wachtwoordherstel voor uw klanten instellen
Uw klanten die zich hebben geregistreerd voor lokale accounts kunnen hun wachtwoorden op hun eigen opnieuw instellen met de functie zelf uw wachtwoord opnieuw instellen. Dit vermindert de werkbelasting van uw medewerkers, met name als uw toepassing heeft miljoenen klanten die gebruikmaken van deze regelmatig. Met behulp van een geverifieerde e-mailadres is momenteel de enige ondersteunde herstelmethode.

> [!NOTE]
> In dit artikel is van toepassing op zelf uw wachtwoord opnieuw instellen die worden gebruikt in de context van een beleid voor aanmelden. Als u volledig aanpasbare wachtwoord opnieuw instellen van beleidsregels aangeroepen vanuit uw app nodig hebt, raadpleegt u [in dit artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Uw directory geen standaard zelf uw wachtwoord opnieuw instellen is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als Abonnementsbeheerder. Dit is hetzelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt voor het maken van uw directory.
2. Open **Azure Active Directory** (in de navigatiebalk aan de linkerkant).
4. Stel **Self service voor wachtwoordherstel ingeschakeld** naar **alle**. 
5. Klik op **opslaan** boven aan de pagina. U bent nu klaar!

Als u wilt testen, gebruikt u de functie 'Nu uitvoeren' op elk beleid aanmelden met lokale accounts als een id-provider. Op de lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **geen toegang tot uw account?** om te controleren of de gebruikerservaring.

> [!NOTE]
> De zelf uw wachtwoord opnieuw instellen van pagina's kunnen worden aangepast via de [functie huisstijl](../active-directory/customize-branding.md).
> 
> 

