---
title: Het configureren van gebruikers inrichten tot een galerie van Azure AD-toepassing | Microsoft Docs
description: Hoe kunt u snel uitgebreide gebruikersaccount inrichten en toepassingen die al wordt vermeld in de Azure AD-Toepassingsgalerie opheffen van inrichting configureren
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2e38fcb30ea7632339a3ba8815a536872cfcc69e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a>Het configureren van gebruikers inrichten tot een toepassing die Azure AD-galerie

*Het inrichten van het account* wordt het maken, bijwerken, en/of account gebruikersrecords in lokale opgeslagen gebruikersprofiel van de toepassing uit te schakelen. De meeste cloud en SaaS-toepassingen Sla de gebruikers, rollen en machtigingen in hun eigen lokale opgeslagen gebruikersprofiel en aanwezigheid van dergelijke een gebruikersrecord in hun lokale archief is *vereist* voor eenmalige aanmelding en toegang tot werkitems.

In de Azure portal, de **inrichten** tabblad in het navigatiedeelvenster links voor een Enterprise-App wordt weergegeven welke inrichting modi voor die app worden ondersteund. Dit kan een van twee waarden zijn:

## <a name="configuring-an-application-for-manual-provisioning"></a>Configureren van een toepassing voor handmatige inrichting

*Handmatige* inrichting betekent dat gebruikersaccounts handmatig met behulp van de methoden die door die app moeten worden gemaakt. Dit kan betekenen aan te melden bij een portal beheerdersrechten voor die app en het toevoegen van gebruikers met behulp van een webgebaseerde gebruikersinterface. Of het een spreadsheet met account-details met een mechanisme dat is opgegeven door de toepassing kan uploaden. Raadpleeg de documentatie van de app of neem contact op met de ontwikkelaar van de app om te bepalen wat mechanismen zijn beschikbaar.

Als u handmatig is alleen de modus voor een bepaalde toepassing weergegeven, betekent dit dat er geen automatische Azure AD connector inrichting is gemaakt voor de app nog. Of betekent dit dat de vereiste gebruiker management API waarop voor het bouwen van een geautomatiseerde inrichting connector biedt geen ondersteuning voor de app.

Als u ondersteuning voor automatische inrichting voor een bepaalde app aanvragen wilt, kunt u een van de aanvraag op invullen <http://aka.ms/aadapprequest>.

## <a name="configuring-an-application-for-automatic-provisioning"></a>Een toepassing configureren voor automatische inrichting

*Automatische* betekent dat een Azure AD connector inrichting is ontwikkeld voor deze toepassing. Zie voor meer informatie over de Azure AD-service en hoe het werkt inrichting [gebruikersaanvragen automatiseren en Deprovisioning voor SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Zie voor meer informatie over het inrichten van specifieke gebruikers en groepen naar een toepassing [het beheren van gebruikers account inrichten voor zakelijke apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

Werkelijke instructies voor het inschakelen en configureren van automatische inrichting variëren afhankelijk van de toepassing.

>[!NOTE]
>U moet beginnen met het vinden van de setup-zelfstudie specifiek zijn voor het instellen van de inrichting voor uw toepassing, en na deze stappen voor het configureren van de app en de Azure AD om het inrichtingsproces verbinding te maken. 
>
>

App-zelfstudies kunnen worden gevonden op [lijst zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Een belangrijkste waarmee u rekening moet houden bij het instellen van de inrichting worden om te bekijken en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD voor de toepassing definiëren. Dit omvat de 'overeenkomende eigenschap' instelt met worden gebruikt voor unieke identificatie dient en overeen met gebruikers/groepen tussen de twee systemen. Voor meer informatie over dit belangrijk proces.

## <a name="next-steps"></a>Volgende stappen
[Gebruikers inrichten kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

