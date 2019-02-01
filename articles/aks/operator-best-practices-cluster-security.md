---
title: Operator aanbevolen procedures - beveiliging-clusters in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor informatie over het beheren van beveiliging van clusters en -upgrades in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: d9ce2661fbdca0a28f917e27e27a3e3f954a9999
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488374"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor beveiliging van clusters en upgrades in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, is de beveiliging van uw workloads en gegevens een van de belangrijkste factoren. Met name wanneer u meerdere tenants clusters met behulp van de logische isolatie uitvoert, moet u voor het beveiligen van toegang tot resources en workloads. Om te beperken het risico van aanvallen, moet u ook om te controleren of dat u de meest recente Kubernetes en knooppunt OS beveiligingsupdates toepassen.

In dit artikel richt zich op het beveiligen van uw AKS-cluster. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Azure Active Directory en op rollen gebaseerd toegangsbeheer gebruiken voor het beveiligen van toegang tot de API-server
> * Beveiligde container-toegang tot resources van knooppunt
> * Een AKS-cluster upgraden naar de nieuwste versie van Kubernetes
> * Knooppunten bijwerken tot datum blijven en beveiligingspatches automatisch worden toegepast

U kunt ook de aanbevolen procedures voor lezen [installatiekopie containerbeheer] [ best-practices-container-image-management] en voor [pod security][best-practices-pod-security].

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>Beveiligde toegang tot de API-server en cluster-knooppunten

**Aanbevolen procedurerichtlijn** -voor het beveiligen van toegang naar de Kubernetes API-Server is een van de meest belangrijke dingen die u doen kunt om uw cluster beveiligen. Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC) integreren met Azure Active Directory voor het beheren van toegang tot de API-server. Deze besturingselementen kunnen u AKS dezelfde manier dat u veilige toegang tot uw Azure-abonnementen beveiligen.

De Kubernetes API-server biedt een één-verbinding voor aanvragen voor het uitvoeren van acties binnen een cluster. Als u wilt beveiligen en controle van toegang tot de API-server, de toegang beperken en bieden de laagst mogelijke machtigingen vereist. Deze benadering is niet uniek is voor Kubernetes, maar is vooral belangrijk wanneer het AKS-cluster logisch geïsoleerd voor meerdere tenants gebruikt is.

Azure Active Directory (AD) biedt een bedrijfsklare identity management-oplossing die kan worden geïntegreerd met AKS-clusters. Als Kubernetes geen een oplossing voor identiteitsbeheer biedt, kan het anders zijn moeilijk te bieden van een zorgvuldige manier toegang te beperken tot de API-server. Met Azure AD geïntegreerde clusters in AKS kunt u uw bestaande gebruikers- en groepsaccounts verifiëren voor toegang tot de API-server.

![Azure Active Directory-integratie voor clusters met AKS](media/operator-best-practices-cluster-security/aad-integration.png)

Gebruik RBAC Kubernetes en Azure AD-integratie voor het beveiligen van de API-server en bieden de minste aantal vereiste machtigingen voor een bereik instellen van resources, zoals een enkele naamruimte. Verschillende gebruikers of groepen in Azure AD kunnen verschillende RBAC-rollen worden verleend. Deze gedetailleerde machtigingen kunnen u toegang tot de API-server beperken en bieden een duidelijke audittrail van acties die worden uitgevoerd.

De aanbevolen best practice is om groepen gebruiken om te voorzien van toegang tot bestanden en mappen versus afzonderlijke identiteiten, gebruikt u Azure AD *groep* lidmaatschap van gebruikers verbinden met RBAC-rollen in plaats van afzonderlijke *gebruikers*. Als de wijzigingen van een gebruiker groepslidmaatschap, zou de toegangsmachtigingen voor het AKS-cluster dienovereenkomstig wijzigen. Als u de gebruiker rechtstreeks aan een rol bindt, kan de functie kan worden gewijzigd. Het lidmaatschap van de Azure AD-groepen wilt bijwerken, maar dat zou niet overeen met machtigingen voor het AKS-cluster. In dit scenario van de gebruiker eindigt meer machtigingen dan vereist dat een gebruiker wordt verleend.

Zie voor meer informatie over Azure AD-integratie en RBAC [aanbevolen procedures voor verificatie en autorisatie in AKS][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Beveiligde container toegang tot resources

**Aanbevolen procedurerichtlijn** -beperkt de toegang tot de acties die containers kunnen uitvoeren. Bieden van de minste machtigingen, het aantal en te voorkomen dat het gebruik van root / escalatie in beschermde modus.

Op dezelfde manier dat gebruikers of groepen de minste Verleen aantal bevoegdheden vereist, containers moeten ook worden beperkt tot alleen de acties en processen die ze nodig hebben. Niet configureren om te beperken het risico van aanvallen, toepassingen en containers die geëscaleerde bevoegdheden is vereist of de hoofd-toegang. Bijvoorbeeld, stel `allowPrivilegeEscalation: false` in het manifest schil. Deze *pod beveiligingscontext* zijn ingebouwd in Kubernetes en kunt u extra machtigingen, zoals de gebruiker of groep uit te voeren als, opgeven of de Linux-mogelijkheden om beschikbaar te stellen. Zie voor meer best practices, [pod beveiligde toegang tot resources][pod-security-contexts].

Voor gedetailleerdere controle van de containeracties u kunt ook gebruiken ingebouwde Linux-beveiligingsfuncties, zoals *AppArmor* en *seccomp*. Deze functies zijn gedefinieerd op het knooppuntniveau van het en vervolgens is geïmplementeerd via een pod-manifest.

> [!NOTE]
> Kubernetes-omgevingen in AKS of ergens anders, zijn niet volledig veilig voor onveilig multitenant gebruik. Aanvullende beveiligingsfuncties zoals *AppArmor*, *seccomp*, *Pod beveiligingsbeleid*, of meer fijnmazig op rollen gebaseerd toegangsbeheer (RBAC) voor knooppunten aanvallen maken moeilijker. Voor de waarde true beveiliging bij het uitvoeren van workloads voor onveilig multitenant, is een hypervisor echter de enige niveau van beveiliging die u moet vertrouwen. Het beveiligingsdomein voor Kubernetes wordt het hele cluster, niet een afzonderlijke knooppunten. Voor deze typen werkbelastingen voor onveilig multitenant, moet u fysiek geïsoleerd clusters.

### <a name="app-armor"></a>App-chassis

Als u wilt beperken de acties die containers kunnen uitvoeren, kunt u de [AppArmor] [ k8s-apparmor] beveiligingsmodule voor Linux-kernel. AppArmor is beschikbaar als onderdeel van het onderliggende knooppunt met AKS-besturingssysteem hebt, en is standaard ingeschakeld. U maakt AppArmor profielen die beperken van acties zoals lezen, schrijven of uitvoeren of systeemfuncties zoals het koppelen van bestandssystemen. AppArmor standaardprofielen toegang beperken tot verschillende `/proc` en `/sys` locaties, en een methode voor het isoleren van logisch containers van het onderliggende knooppunt. AppArmor werkt voor elke toepassing die wordt uitgevoerd op Linux, niet alleen Kubernetes-schillen.

![AppArmor-profielen gebruikt in een AKS-cluster om te beperken containeracties](media/operator-best-practices-container-security/apparmor.png)

Als u wilt zien AppArmor in actie, wordt het volgende voorbeeld een profiel waarmee wordt voorkomen het schrijven naar bestanden dat. [SSH] [ aks-ssh] een AKS-knooppunt, maakt u een bestand met de naam *weigeren write.profile* en plak de volgende inhoud:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

AppArmor profielen zijn toegevoegd met behulp van de `apparmor_parser` opdracht. Voeg het profiel toe aan AppArmor en geef de naam van het profiel in de vorige stap hebt gemaakt:

```console
sudo apparmor_parser deny-write.profile
```

Er is geen uitvoer die wordt geretourneerd als het profiel is juist geparseerd en op AppArmor toegepast. U bent weer terug naar de opdrachtprompt.

Vanaf uw lokale computer, Maak nu een pod-manifest met de naam *aks-apparmor.yaml* en plak de volgende inhoud. Dit manifest definieert een aantekening voor `container.apparmor.security.beta.kubernetes` verwijzingen toevoegen de *weigeren schrijven* profiel hebt gemaakt in de vorige stappen:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

Implementatie van het voorbeeld pod met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl apply -f aks-apparmor.yaml
```

Met de schil is geïmplementeerd, gebruiken de [kubectl exec] [ kubectl-exec] opdracht uit om te schrijven naar een bestand. De opdracht kan niet worden uitgevoerd, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

Zie voor meer informatie over AppArmor [AppArmor-profielen in Kubernetes][k8s-apparmor].

### <a name="secure-computing"></a>Beveiligde computers

Terwijl AppArmor voor alle Linux-toepassing werkt [seccomp (*sec*ureren *comp*uting)] [ seccomp] werkt op procesniveau. Seccomp is ook een Linux-kernel security module en wordt systeemeigen worden ondersteund door de Docker-runtime die wordt gebruikt door AKS-knooppunten. Met seccomp geldt de proces-aanroepen die containers kunnen uitvoeren. U definieert u welke acties wilt toestaan of weigeren filters maken en vervolgens aantekeningen in een schil YAML-manifest gebruiken om te koppelen aan het filter seccomp. Dit Framework sluit aan de aanbevolen procedures van alleen de minimaal vereiste machtigingen die nodig zijn om uit te voeren voor de container toekennen en mag niet meer.

Als wilt seccomp in actie zien, maakt u een filter waarmee wordt voorkomen dat het wijzigen van machtigingen voor een bestand. [SSH] [ aks-ssh] een AKS-knooppunt, maakt u vervolgens een seccomp filter met de naam */var/lib/kubelet/seccomp/prevent-chmod* en plak de volgende inhoud:

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "syscalls": [
    {
      "name": "chmod",
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

Vanaf uw lokale computer, Maak nu een pod-manifest met de naam *aks-seccomp.yaml* en plak de volgende inhoud. Dit manifest definieert een aantekening voor `seccomp.security.alpha.kubernetes.io` en verwijst naar de *te voorkomen dat chmod* filter in de vorige stap hebt gemaakt:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: chmod-prevented
  annotations:
    seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
spec:
  containers:
  - name: chmod
    image: busybox
    command:
      - "chmod"
    args:
     - "777"
     - /etc/hostname
  restartPolicy: Never
```

Implementatie van het voorbeeld pod met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl apply -f ./aks-seccomp.yaml
```

Bekijk de status van de pods die met behulp van de [kubectl ophalen schillen] [ kubectl-get] opdracht. De schil meldt een fout. De `chmod` opdracht wordt uitgevoerd door het filter seccomp voorkomen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Zie voor meer informatie over beschikbare filters [Seccomp beveiligingsprofielen voor Docker][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Regelmatig bij te werken naar de nieuwste versie van Kubernetes

**Aanbevolen procedurerichtlijn** - Blijf op de nieuwe functies en bugfixes, regelmatig een upgrade uitvoeren naar de Kubernetes-versie in uw AKS-cluster.

Kubernetes-releases nieuwe functies in een tempo sneller dan de meer traditionele infrastructuurplatformen. Kubernetes-updates bevatten nieuwe functies en verbeteringen bug of beveiliging. Nieuwe functies worden overgezet via een *alpha* en vervolgens *Bèta* status voordat ze worden *stabiel* en zijn algemeen beschikbaar en worden aanbevolen voor gebruik in productieomgevingen. De releasecyclus van deze kunt u Kubernetes bijwerken zonder dat regelmatig er belangrijke wijzigingen of het aanpassen van uw implementaties en sjablonen.

AKS ondersteunt vier secundaire versies van Kubernetes. Dit betekent dat wanneer een nieuwe versie van de secundaire-patch is geïntroduceerd, de oudste secundaire versie en patch-versies ondersteund zijn buiten gebruik gesteld. Kleine updates naar Kubernetes gebeuren op periodieke basis. Zorg ervoor dat u hebt een governance-proces om te controleren en een upgrade uit als nodig is, zodat u ondersteuning niet nakomen. Zie voor meer informatie [ondersteund Kubernetes-versies AKS][aks-supported-versions]

Om te controleren of de versies die beschikbaar voor uw cluster zijn, gebruikt u de [az aks get-upgrades] [ az-aks-get-upgrades] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

U kunt vervolgens een upgrade uw AKS-cluster met de [az aks upgrade] [ az-aks-upgrade] opdracht. Het upgradeproces veilig cordons en verkeer naar één knooppunt tegelijk, schillen in de resterende knooppunten plant en implementeert vervolgens een nieuw knooppunt met de meest recente OS en Kubernetes-versies.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.3
```

Zie voor meer informatie over upgrades in AKS [ondersteund Kubernetes-versies in AKS] [ aks-supported-versions] en [een AKS-cluster upgraden][aks-upgrade].

## <a name="process-node-updates-and-reboots-using-kured"></a>Proces knooppunt updates en opnieuw is opgestart met behulp van kured

**Aanbevolen procedurerichtlijn** - AKS automatisch gedownload en geïnstalleerd beveiliging worden opgelost op elk van de worker-knooppunten, maar wordt niet automatisch opnieuw opgestart als dat nodig. Gebruik `kured` wilt bekijken voor in afwachting van opnieuw wordt opgestart, en vervolgens veilig cordon en leegmaken van het knooppunt om toe te staan van het knooppunt opnieuw op te starten, de updates toepassen en net zo veilig mogelijk met betrekking tot het besturingssysteem.

Elke avond ophalen de AKS-knooppunten beveiligingspatches beschikbaar via de distributie-kanaal bijwerken. Dit gedrag wordt automatisch geconfigureerd wanneer de knooppunten in een AKS-cluster worden geïmplementeerd. Om te beperken wordt onderbroken en mogelijke impact op actieve werkbelastingen, knooppunten worden niet automatisch opnieuw opgestart als een beveiligingspatch of kernel-update vereist is.

De open-source [kured (KUbernetes opnieuw opstarten Daemon)] [ kured] project door deze te Weaveworks controleert op in behandeling zijnde knooppunt opnieuw wordt opgestart. Wanneer een knooppunt van de toepassing is voor updates waarvoor opnieuw worden opgestart, wordt het knooppunt veilig afgebakend en geleegd om te verplaatsen en plannen van de pods op andere knooppunten in het cluster. Wanneer het knooppunt opnieuw wordt opgestart, wordt deze toegevoegd in het cluster en de Kubernetes-hervat schillen op deze planning. Om te beperken wordt onderbroken, mag slechts één knooppunt tegelijk opnieuw worden opgestart door `kured`.

![Het proces AKS knooppunt opnieuw opstarten met behulp van kured](media/operator-best-practices-cluster-security/node-reboot-process.png)

Als u wilt dat fijner gedetailleerde controle over wanneer opnieuw opstarten is gebeurd, `kured` kan worden geïntegreerd met Prometheus om te voorkomen dat opnieuw wordt opgestart als er andere onderhoudsgebeurtenissen of problemen met de cluster wordt uitgevoerd. Deze integratie minimaliseert de aanvullende problemen door knooppunten opnieuw wordt opgestart terwijl u actief andere problemen wilt oplossen.

Zie voor meer informatie over het afhandelen van knooppunt opnieuw wordt opgestart, [beveiligings- en -kernel-updates toepassen op knooppunten in AKS][aks-kured].

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op over het beveiligen van uw AKS-cluster. Voor het implementeren van sommige van deze gebieden, Zie de volgende artikelen:

* [Azure Active Directory integreren met AKS][aks-aad]
* [Een AKS-cluster upgraden naar de nieuwste versie van Kubernetes][aks-upgrade]
* [Proces-beveiligingsupdates en knooppunt opnieuw wordt opgestart met kured][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://docs.docker.com/engine/security/seccomp/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: aad-integration.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md
