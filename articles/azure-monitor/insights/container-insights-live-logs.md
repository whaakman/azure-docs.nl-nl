---
title: Azure Monitor weergeven voor containers-Logboeken in realtime | Microsoft Docs
description: Dit artikel beschrijft de realtime-weergave van Logboeken voor containers (stdout/stderr) zonder kubectl gebruiken met Azure Monitor voor containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: magoedte
ms.openlocfilehash: 6fe8cccf60e60ada34e3b7847964958cf6e03c4a
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904743"
---
# <a name="how-to-view-container-logs-real-time-with-azure-monitor-for-containers-preview"></a>Om weer te geven van de container realtime met Azure Monitor-logboeken voor containers (preview)
Deze functie momenteel in preview is, biedt realtime inzicht in uw logboeken voor containers in de Azure Kubernetes Service (AKS) (stdout/stderr) zonder de kubectl-opdrachten uit te voeren. Wanneer u deze optie selecteert, nieuwe deelvenster wordt weergegeven onder de tabel containers prestaties gegevens op de **Containers** weergeven.  Hier ziet u live logboekregistratie die zijn gegenereerd door de engine container om verder te helpen bij het oplossen van problemen in realtime. **Inzender** toegang tot de cluster-bron is vereist voor deze functie te gebruiken.

Live logboeken ondersteunt drie verschillende methoden voor het beheren van toegang tot de logboeken:

1. AKS zonder Kubernetes RBAC-autorisatie ingeschakeld 
2. AKS ingeschakeld met Kubernetes RBAC-autorisatie
3. Eenmalige aanmelding op basis van door AKS ingeschakeld met Azure Active Directory (AD) SAML 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes-cluster zonder RBAC ingeschakeld
 
Als u een Kubernetes-cluster dat niet is geconfigureerd met Kubernetes RBAC-autorisatie of geïntegreerd met Azure AD eenmalige aanmelding hebt, hoeft u niet aan de volgende stappen. Omdat de kube-api maakt gebruik van Kubernetes-autorisatie, zijn alleen-lezen-machtigingen vereist.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC-autorisatie
Als u Kubernetes RBAC-autorisatie hebt ingeschakeld, moet u om toe te passen cluster rol binding. Het volgende voorbeeld wordt laten zien hoe het configureren van cluster rol binding met deze sjabloon yaml-configuratie.   

1. Kopieer en plak het yaml-bestand en sla deze op als LogReaderRBAC.yaml.  

   ```
   apiVersion: rbac.authorization.k8s.io/v1 
   kind: ClusterRole 
   metadata: 
      name: containerHealth-log-reader 
   rules: 
      - apiGroups: [""] 
        resources: ["pods/log"] 
        verbs: ["get"] 
   --- 
   apiVersion: rbac.authorization.k8s.io/v1 
   kind: ClusterRoleBinding 
   metadata: 
      name: containerHealth-read-logs-global 
   roleRef: 
       kind: ClusterRole 
       name: containerHealth-log-reader 
       apiGroup: rbac.authorization.k8s.io 
   subjects: 
      - kind: User 
        name: clusterUser 
        apiGroup: rbac.authorization.k8s.io 
   ```

2. De binding van de regel cluster maken met de volgende opdracht: `kubectl create -f LogReaderRBAC.yaml`. 

## <a name="configure-aks-with-azure-active-directory"></a>AKS met Azure Active Directory configureren
AKS kan worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. Als u dit voor het eerst configureert, Zie [Integreer Azure Active Directory met Azure Kubernetes Service](../../aks/aad-integration.md). Tijdens de stappen voor het maken van de [clienttoepassing](../../aks/aad-integration.md#create-client-application) en geef de **omleidings-URI**, moet u een andere URI toevoegen aan de lijst met `https://ininprodeusuxbase.microsoft.com/*`.  

>[!NOTE]
>Configureren van verificatie met Azure Active Directory voor eenmalige aanmelding kan alleen worden uitgevoerd tijdens de eerste implementatie van een nieuw AKS-cluster. U kunt eenmalige aanmelding niet configureren op voor een AKS-cluster die al zijn geïmplementeerd.  
> 

## <a name="view-live-logs"></a>Live logboeken weergeven
Wanneer u bekijkt **Containers**, kunt u **Zobrazit Kontejner registreert** of **live containerlogboeken bekijken**.  Wanneer u selecteert **weergave live containerlogboeken**, een nieuw deelvenster wordt weergegeven onder de containers prestaties gegevenstabel en weergeven van de live logboekregistratie die worden gegenereerd door de engine container om verder te helpen bij het oplossen van problemen in realtime.  
1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Uit de **Microsoft Azure** in het menu **Monitor** en selecteer vervolgens **Containers**.  
3. Selecteer een container in de lijst onder de **bewaakt containers** weergeven.  
4. Selecteer de **Containers** weergeven en in het venster Eigenschappen voor een geselecteerde container, de koppeling **live containerlogboeken bekijken** wordt vermeld.  
5. Als het AKS-cluster is geconfigureerd met eenmalige aanmelding met AAD, wordt u gevraagd om te verifiëren bij het eerste gebruik tijdens de browsersessie. Selecteer uw account en de volledige verificatie met Azure.  

Verificatie is gelukt, wordt het live logboekvenster weergegeven in het onderste gedeelte van het middelste deelvenster. Als de statusindicator ophalen een groen vinkje, die aan de rechterkant van het deelvenster is bevat, betekent dit dat kunnen gegevens worden opgehaald.
    
  ![Live logs-deelvenster gegevens opgehaald](./media/container-insights-live-logs/live-logs-pane-01.png)  

In de zoekbalk typt, kunt u filteren op trefwoord om te markeren die tekst in het logboek.   

  ![Live logboeken deelvenster filteren voorbeeld](./media/container-insights-live-logs/live-logs-pane-filter-01.png)

Als u wilt onderbreken automatisch schuiven en bepalen het gedrag van het deelvenster en kunt u handmatig Blader door de nieuwe logboekgegevens gelezen, klikt u op de **schuiven** optie.  Als u wilt opnieuw automatisch schuiven inschakelen, klikt u op de **schuiven** optie opnieuw.  U kunt ook het ophalen van gegevens onderbreken door te klikken op de **onderbreken** optie en wanneer u klaar om te hervatten bent, gewoon op **afspelen**.  

![Live logboeken deelvenster onderbreken live bekijken](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

## <a name="next-steps"></a>Volgende stappen
Om door te gaan met het leren over het gebruik van Azure Monitor en controleren van andere aspecten van uw AKS-cluster [weergave Azure Kubernetes Service health](container-insights-analyze.md).
