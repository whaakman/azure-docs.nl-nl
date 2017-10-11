---
title: Klassieke portal toepassingsproxy van Azure AD-connectors | Microsoft Docs
description: Bevat informatie over het maken en beheren van groepen van connectors in Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publiceren van toepassingen op afzonderlijke netwerken en locaties met groepen van de connector
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klassieke Azure Portal](active-directory-application-proxy-connectors.md)
>
>

Connector-groepen zijn handig voor verschillende scenario's, waaronder:

* -Sites met meerdere onderling verbonden datacenters. In dit geval u zoveel verkeer binnen het datacenter mogelijk behouden omdat cross-datacenter koppelingen dure en traag zijn. U kunt implementeren connectors in elk datacenter te bedienen alleen de toepassingen die zich in het datacenter bevinden. Deze aanpak cross-datacenter koppelingen wordt geminimaliseerd en een volledig transparant ervaring voor uw gebruikers.
* Het beheren van toepassingen die zijn geïnstalleerd op de geïsoleerde netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk. Connector-groepen kunt u specifieke connectors installeren op geïsoleerde netwerken ook om toepassingen te isoleren met het netwerk.
* Toepassingen die zijn geïnstalleerd op IaaS voor toegang tot de cloud, bieden connector groepen een algemene service de toegang tot alle apps beveiligen. Groepen van de connector geen extra afhankelijkheid in uw bedrijfsnetwerk maken of fragment van de appervaring. Connectors kunnen worden geïnstalleerd op elke clouddatacenter en dienen alleen toepassingen die zich in dit netwerk bevinden. U kunt verschillende connectors om te zorgen voor hoge beschikbaarheid installeren.
* Ondersteuning voor meerdere forests omgevingen waarin specifieke connectors kunnen worden geïmplementeerd per forest en instellen voor specifieke toepassingen.
* Connector-groepen kunnen worden gebruikt in herstel na noodgevallen sites voor het detecteren van een failover of als back-up voor de primaire site.
* Groepen van de connector kunnen ook worden gebruikt voor meerdere bedrijven van een enkele tenant.

## <a name="prerequisite-create-your-connectors"></a>Voorwaarde: Uw connectors maken
Voor het groeperen van uw connectors [meerdere connectors installeren](active-directory-application-proxy-enable.md), geef de naam en ze te groeperen. Tot slot moet u deze toewijzen aan specifieke apps.

## <a name="step-1-create-connector-groups"></a>Stap 1: Connector groepen maken
U kunt zoveel connector groepen als u wilt maken. Het maken van connector wordt uitgevoerd in de klassieke Azure portal.

1. Selecteer uw directory en klik op **configureren**.  
    ![Toepassingsproxy, schermafbeelding configureren: klik op de connector-groepen beheren](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. Klik onder Application Proxy op **Connector groepen beheren** en een groep connector maken door de groep een naam geven.  
    ![Connector voor toepassingsproxy schermafbeelding - groep voor de nieuwe groepen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Stap 2: Connectors toewijzen aan uw groepen
Als de connector-groepen zijn gemaakt, verplaatst u de connectors aan de juiste groep.

1. Onder **toepassingsproxy**, klikt u op **Connectors beheren**.
2. Onder **groep**, selecteer de groep die u voor elke connector. Het duurt maximaal 10 minuten actief is in de nieuwe groep de connectors.  
    ![Schermafbeelding van Application proxy connectors - selecte groep uit de vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Stap 3: Toepassingen aan de connector-groepen toewijzen
De laatste stap is het instellen van de connector-groepen die aan deze elke toepassing.

1. Selecteer de toepassing die u wilt toewijzen aan de groep en klikt u op in de klassieke Azure-portal in uw directory **configureren**.
2. Onder **Connector groep**, selecteer de groep die u wilt dat de toepassing te gebruiken. Deze wijziging wordt onmiddellijk toegepast.  
    ![Application proxy connector groep schermafbeelding - selecte groep uit de vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Zie ook
* [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
* [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
* [Oplossen van problemen met toepassingsproxy](active-directory-application-proxy-troubleshoot.md)

Ga naar het [blog over toepassingsproxy](http://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.
