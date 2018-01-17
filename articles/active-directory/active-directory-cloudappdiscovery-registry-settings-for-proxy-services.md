---
title: Cloud App Discovery-registerinstellingen voor proxyservices | Microsoft Docs
description: Het doel van dit onderwerp is om te voorzien van de stappen die u moet uitvoeren om in te stellen de vereiste poort op de computers waarop de Cloud App Discovery-agent wordt uitgevoerd.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0e227d6e15789b29b40197a9ff71b2116312da78
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery-registerinstellingen voor proxyservices
Het doel van dit onderwerp wordt uitgelegd hoe u kunt uitvoeren om in te stellen de vereiste poort op de computers waarop de Cloud App Discovery-agent wordt uitgevoerd. De Cloud App Discovery-agent is standaard geconfigureerd om alleen de poorten 80 of 443 te gebruiken. Als u van plan bent over het installeren van Cloud App Discovery in een omgeving met een proxyserver die van een aangepaste poort (geen 80 of 443 gebruikmaakt), moet u de agents voor het gebruik van deze poort configureren. De configuratie is gebaseerd op een registersleutel.

> [!TIP] 
> Bekijk de verbeteringen in de nu zonder agent Cloud App Discovery in Azure Active Directory (Azure AD), die worden verhoogd door [integratie met Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

## <a name="modify-the-port-used-by-the-computer-running-the-cloud-app-discovery-agent"></a>De poort die wordt gebruikt door de computer met de Cloud App Discovery-agent wijzigen

1. Start de Registereditor.
  ![Run](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Ga naar of maak de volgende registersleutel: **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**
3. Maak een nieuwe **met meerdere tekenreeksen** waarde met de naam **poorten**. 
  ![Nieuw](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Openen van de **met meerdere tekenreeksen bewerken** dialoogvenster, dubbelklikt u op de **poorten** waarde.
5. In de **Waardegegevens**, voer de volgende waarden en alle aangepaste poorten die worden gebruikt door uw organisatie toevoegen: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Klik op **OK** sluiten de **met meerdere tekenreeksen bewerken** dialoogvenster.

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik cloudapps die worden gebruikt in mijn organisatie detecteren](active-directory-cloudappdiscovery-whatis.md) 

