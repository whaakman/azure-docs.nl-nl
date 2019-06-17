---
title: Azure Monitor weergeven voor containers-Logboeken in realtime | Microsoft Docs
description: Dit artikel beschrijft de realtime weergave van Logboeken voor containers (stdout/stderr) en gebeurtenissen zonder kubectl gebruiken met Azure Monitor voor containers.
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
ms.date: 06/12/2019
ms.author: magoedte
ms.openlocfilehash: 71c6f1936f8cbc700a24d0ffb497947c8c8d3a50
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075304"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Om weer te geven van Logboeken en gebeurtenissen in realtime (preview)
Azure Monitor voor containers bevat een functie, die zich momenteel in preview, waarmee een liveweergave van uw Azure Kubernetes Service (AKS) containerlogboeken (stdout/stderr) en -gebeurtenissen zonder kubectl-opdrachten uit te voeren. Wanneer u een van de opties selecteert, een nieuw deelvenster wordt weergegeven onder de tabel met prestaties op de **knooppunten**, **Controllers**, en **Containers** weergeven. Live logboekregistratie en gebeurtenissen die worden gegenereerd door de engine container om verder te helpen bij het oplossen van problemen in realtime worden weergegeven.

>[!NOTE]
>**Inzender** toegang tot de cluster-bron is vereist voor deze functie te gebruiken.
>

Live-logboeken bieden ondersteuning voor drie verschillende methoden voor het beheren van toegang tot de logboeken:

1. AKS zonder Kubernetes RBAC-autorisatie ingeschakeld
2. AKS ingeschakeld met Kubernetes RBAC-autorisatie
3. AKS met Azure Active Directory (AD) SAML gebaseerde eenmalige aanmelding ingeschakeld

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
         resources: ["pods/log", "events"] 
         verbs: ["get", "list"]  
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

2. Als u deze voor het eerst configureert, u de binding van de regel cluster maken met de volgende opdracht: `kubectl create -f LogReaderRBAC.yaml`. Als u ondersteuning eerder ingeschakeld voor live logboeken bekijken voordat we geïntroduceerd live gebeurtenislogboeken, voor het bijwerken van uw configuratie, voer de volgende opdracht: `kubectl apply -f LogReaderRBAC.yml`.

## <a name="configure-aks-with-azure-active-directory"></a>AKS met Azure Active Directory configureren

AKS kan worden geconfigureerd voor het gebruik van Azure Active Directory (AD) voor verificatie van de gebruiker. Als u deze voor het eerst configureert, Zie [Integreer Azure Active Directory met Azure Kubernetes Service](../../aks/azure-ad-integration.md). Tijdens de stappen voor het maken van de [clienttoepassing](../../aks/azure-ad-integration.md#create-the-client-application), geef het volgende:

- **Omleidings-URI (optioneel)** : Dit is een **Web** toepassingstype en de basiswaarde in de URL moet `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
- Na de registratie van de toepassing uit de **overzicht** pagina **verificatie** in het linkerdeelvenster. Op de **verificatie** pagina onder **geavanceerde instellingen** impliciet verlenen **toegangstokens** en **ID-tokens** en sla vervolgens uw wijzigingen.

>[!NOTE]
>Configureren van verificatie met Azure Active Directory voor eenmalige aanmelding kan alleen worden uitgevoerd tijdens de initiële implementatie van een nieuw AKS-cluster. U kunt eenmalige aanmelding niet configureren op voor een AKS-cluster die al zijn geïmplementeerd.

## <a name="view-live-logs-and-events"></a>Live-logboeken en gebeurtenissen

U kunt realtime logboekgebeurtenissen weergeven nadat ze zijn gegenereerd door de container-engine van de **knooppunten**, **Controllers**, en **Containers** weergeven. In het eigenschappendeelvenster die u selecteert **live-gegevens (preview) weergeven** optie en een deelvenster wordt weergegeven onder de prestaties gegevenstabel waar u logboek- en gebeurtenissen in een onafgebroken stroom kunt bekijken.

![Optie voor live-logboeken van knooppunt eigenschappen deelvenster weergeven](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Logboek- en gebeurtenisgegevens berichten zijn beperkt op basis van het resourcetype is geselecteerd in de weergave.

| Weergave | Resourcetype | Logboek of de gebeurtenis | Gegevens die worden gepresenteerd |
|------|---------------|--------------|----------------|
| Knooppunten | Knooppunt | Gebeurtenis | Wanneer een knooppunt is geselecteerd wordt gebeurtenissen niet worden gefilterd en brede, door het cluster Kubernetes-gebeurtenissen weergeven. De titel van het deelvenster ziet u de naam van het cluster. |
| Knooppunten | Pod | Gebeurtenis | Als u een schil hebt geselecteerd worden gebeurtenissen gefilterd op de naamruimte. De titel van het deelvenster ziet u de naamruimte van de schil. | 
| Controllers | Pod | Gebeurtenis | Als u een schil hebt geselecteerd worden gebeurtenissen gefilterd op de naamruimte. De titel van het deelvenster ziet u de naamruimte van de schil. |
| Controllers | Controller | Gebeurtenis | Als u een domeincontroller hebt geselecteerd worden gebeurtenissen gefilterd op de naamruimte. De titel van het deelvenster ziet u de naamruimte van de controller. |
| Domeincontrollers-knooppunten/Containers | Container | Logboeken | De titel van het deelvenster ziet u dat de naam van de schil de container is gegroepeerd met. |

Als het AKS-cluster is geconfigureerd met eenmalige aanmelding met AAD, wordt u gevraagd om te verifiëren bij het eerste gebruik tijdens de browsersessie. Selecteer uw account en de volledige verificatie met Azure.  

Verificatie is gelukt, wordt het live logboekvenster weergegeven in het onderste gedeelte van het middelste deelvenster. Als de statusindicator ophalen een groen vinkje, die aan de rechterkant van het deelvenster is bevat, betekent dit dat kunnen gegevens worden opgehaald.
    
  ![Live logs-deelvenster gegevens opgehaald](./media/container-insights-live-logs/live-logs-pane-01.png)  

In de zoekbalk typt, kunt u filteren op trefwoord om te markeren die tekst in het logboek of de gebeurtenis en in de zoekbalk aan de rechterkant, het laat zien hoeveel resultaten overeenkomen met uit het filter.

  ![Live logboeken deelvenster filteren voorbeeld](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Tijdens het bekijken van gebeurtenissen, kunt u ook de resultaten te beperken de **Filter** pill gevonden aan de rechterkant van de zoekbalk. Afhankelijk van welke resource die u hebt geselecteerd, bevat de pill een pod, de naamruimte of het cluster hebt geselecteerd in.  

Als u wilt onderbreken automatisch schuiven en bepalen het gedrag van het deelvenster en kunt u handmatig Blader door de nieuwe gegevens gelezen, klikt u op de **schuiven** optie. Als u wilt opnieuw automatisch schuiven inschakelen, klikt u op de **schuiven** optie opnieuw. U kunt ook het ophalen van gegevens van logboek of de gebeurtenis onderbreken door te klikken op de **onderbreken** optie en wanneer u klaar om te hervatten bent, gewoon op **afspelen**.  

![Live logboeken deelvenster onderbreken live bekijken](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

Gaat u naar Azure Monitor logboeken naar het historische containerlogboeken bekijken door te selecteren **containerlogboeken bekijken** uit de vervolgkeuzelijst **weergeven in analytics**.

## <a name="next-steps"></a>Volgende stappen
- Om door te gaan met het leren over het gebruik van Azure Monitor en controleren van andere aspecten van uw AKS-cluster [weergave Azure Kubernetes Service health](container-insights-analyze.md).
- Weergave [Meld u voorbeelden van](container-insights-log-search.md#search-logs-to-analyze-data) om te zien van vooraf gedefinieerde query's en voorbeelden om te evalueren of aanpassen voor waarschuwingen, visualiseren en analyseren van uw clusters.
