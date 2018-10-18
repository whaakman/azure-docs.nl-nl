---
title: Concepten - opslag in Azure Kubernetes-Services (AKS)
description: Meer informatie over opslag in Azure Kubernetes Service (AKS), met inbegrip van volumes, permanente volumes Opslagklassen en claims
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: c3a737bdd9978e6cb02e3e8b7a34407eb1dd8fb6
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381025"
---
# <a name="storage-options-for-applications-in-azure-kubernetes-service-aks"></a>Opties voor opslag voor toepassingen in Azure Kubernetes Service (AKS)

Toepassingen die worden uitgevoerd in Azure Kubernetes Service (AKS) mogelijk voor het opslaan en ophalen van gegevens. Voor sommige werkbelastingen van toepassingen, de opslag van deze gegevens lokale opslag met een snelle gebruiken op het knooppunt dat niet meer nodig hebt is wanneer de schillen zijn verwijderd. Andere werkbelastingen van toepassingen vereisen mogelijk opslagruimte die zich blijft voordoen in meer regelmatig gegevens op volumes in het Azure-platform. Meerdere schillen zijn mogelijk de dezelfde gegevensvolumes te delen of gegevensvolumes opnieuw koppelen als de schil is opnieuw gepland op een ander knooppunt. Ten slotte moet u mogelijk gevoelige gegevens of configuratie-informatie in schillen toepassing invoeren.

![Opties voor opslag voor toepassingen in een cluster Azure Kubernetes-Services (AKS)](media/concepts-storage/aks-storage-options.png)

In dit artikel worden de belangrijkste concepten die over de opslag die aan uw toepassingen in AKS geïntroduceerd:

- [Volumes](#volumes)
- [Permanente volumes](#persistent-volumes)
- [Opslagklassen](#storage-classes)
- [Claims van permanent volume](#persistent-volume-claims)

## <a name="volumes"></a>Volumes

Toepassingen moeten vaak kunnen opslaan en ophalen van gegevens. Afzonderlijke schillen Kubernetes doorgaans behandelt als tijdelijke, beschikbare resources, zijn beschikbaar voor gebruik van toepassingen en behoud van gegevens naar behoefte verschillende benaderingen. Een *volume* vertegenwoordigt een manier opslaan, ophalen en behoud van gegevens tussen schillen en gedurende de levensduur van toepassingen.

Traditionele volumes aan gegevens opslaan en ophalen worden als ondersteund door Azure Storage Kubernetes-resources gemaakt. U kunt handmatig maken van deze gegevensvolumes moet worden toegewezen aan schillen rechtstreeks of Kubernetes ze automatisch te maken hebben. Deze gegevensvolumes kunnen Azure-schijven of Azure Files gebruiken:

- *Azure-schijven* kan worden gebruikt voor het maken van een Kubernetes *DataDisk* resource. Azure Premium-opslag, ondersteund door hoogwaardige SSD's, of Azure Standard-opslag, ondersteund door HDD's van normale schijven gebruiken. Voor de meeste productie en werkvolumes voor ontwikkeling, Premium storage te gebruiken. Azure-schijven zijn gekoppeld als *ReadWriteOnce*, dus zijn alleen beschikbaar voor een enkel knooppunt. Voor de opslagvolumes die kunnen worden geopend door meerdere knooppunten gelijktijdig, Azure Files te gebruiken.
- *Azure Files* kan worden gebruikt voor het koppelen van een ondersteund door een Azure Storage-account met schillen SMB 3.0-bestandsshare. Bestanden kunnen u gegevens in meerdere knooppunten en schillen delen. Bestanden kunnen op dit moment alleen gebruiken voor Azure Standard-opslag ondersteund door HDD's van reguliere.

In Kubernetes, kunnen volumes bestaan uit meer dan alleen maar een traditionele schijf waar informatie kan worden opgeslagen en opgehaald. Kubernetes-volumes kunnen ook worden gebruikt als een manier om gegevens injecteren in een schil voor gebruik door de containers. Algemene typen van extra volumes in Kubernetes zijn onder andere:

- *emptyDir* -dit volume wordt vaak gebruikt als tijdelijke ruimte voor een schil. Alle containers in een schil hebben toegang tot de gegevens op het volume. Gegevens geschreven naar dit volumetype zich blijft voordoen alleen voor de levensduur van de schil - wanneer de schil wordt verwijderd, het volume wordt verwijderd. Dit volume wordt doorgaans gebruikt de onderliggende opslag van het lokale knooppunt schijf, maar kunnen ook bestaan alleen in het geheugen van het knooppunt.
- *geheim* -dit volume wordt gebruikt voor het invoeren van gevoelige gegevens in schillen, zoals wachtwoorden. U maakt eerst een geheim met behulp van de Kubernetes-API. Wanneer u uw schil of de implementatie definieert, kan een specifiek geheim worden aangevraagd. Geheimen zijn alleen bedoeld als knooppunten met een geplande schil dat vereist is en het geheim is opgeslagen in *tmpfs*niet geschreven naar schijf. Wanneer de laatste schil op een knooppunt dat is vereist een geheim is verwijderd, wordt het geheim uit van het knooppunt tmpfs verwijderd. Geheimen worden opgeslagen in een bepaalde naamruimte en kunnen alleen worden geopend door schillen in de dezelfde naamruimte.
- *configMap* -deze volume wordt gebruikt om het invoeren van sleutel / waarde-paar eigenschappen in schillen, zoals informatie over de configuratie van toepassingen. In plaats van de configuratiegegevens van de toepassing binnen de installatiekopie van een container definieert, kunt u deze definiëren als een Kubernetes-resource die eenvoudig kan worden bijgewerkt en toegepast op nieuwe instanties van schillen wanneer ze worden geïmplementeerd. Als u een geheim, moet u eerst een ConfigMap met behulp van de Kubernetes-API maken. Deze ConfigMap kan vervolgens worden aangevraagd bij het definiëren van een pod of de implementatie. ConfigMaps worden opgeslagen in een bepaalde naamruimte en kan alleen worden geopend door schillen in de dezelfde naamruimte.

## <a name="persistent-volumes"></a>Permanente volumes

Volumes zijn gedefinieerd en gemaakt als onderdeel van de levenscyclus van de schil bestaan alleen totdat de schil wordt verwijderd. Schillen verwachten vaak dat de opslag om te blijven als een schil op een andere host is opnieuw gepland tijdens onderhoud, met name in StatefulSets. Een *permanent volume* (HW) is een opslagresource gemaakt en beheerd door de Kubernetes-API die buiten de levensduur van een afzonderlijke pod kan bestaan.

Azure-schijven of bestanden worden gebruikt voor de PersistentVolume. Als in de vorige sectie op Volumes die u hebt genoteerd, wordt de keuze van de schijven of bestanden vaak bepaald door de noodzaak voor gelijktijdige toegang tot de gegevens of de prestatielaag.

![Permanente volumes in een cluster Azure Kubernetes-Services (AKS)](media/concepts-storage/persistent-volumes.png)

Een PersistentVolume mag *statisch* die zijn gemaakt door de Clusterbeheerder van een, of *dynamisch* die zijn gemaakt door de Kubernetes API-server. Als een schil is gepland en opslag die is momenteel niet beschikbaar-aanvragen, kunt Kubernetes de onderliggende opslag van Azure-cd of -bestanden maken en deze koppelen aan de schil. Dynamische provisioning maakt gebruik van een *StorageClass* om te bepalen welk type Azure-opslag moet worden gemaakt.

## <a name="storage-classes"></a>Opslagklassen

Voor het definiëren van verschillende lagen van opslag, zoals Premium en Standard, kunt u een *StorageClass*. De StorageClass bepaalt ook de *reclaimPolicy*. Deze reclaimPolicy bepaalt het gedrag van de onderliggende Azure storage-resource als de schil wordt verwijderd en het permanent volume is mogelijk niet meer vereist. De onderliggende resource voor opslag kan worden verwijderd of worden bewaard voor gebruik met een toekomstige schil.

In AKS, worden twee initiële StorageClasses gemaakt:

- *standaard* -maakt gebruik van Azure Standard-opslag om een beheerde schijf te maken. Het beleid reclaim geeft aan dat de onderliggende Azure-schijf wordt verwijderd wanneer de schil waarmee deze wordt verwijderd.
- *beheerde premium* -maakt gebruik van Azure Premium storage om beheerde schijf te maken. Het beleid reclaim opnieuw geeft aan dat de onderliggende Azure-schijf wordt verwijderd wanneer de schil waarmee deze wordt verwijderd.

Als er geen StorageClass voor een permanent volume is opgegeven, wordt de standaardwaarde StorageClass gebruikt. Wees voorzichtig bij het aanvragen van permanente volumes zodat ze de juiste opslag die u moet gebruiken. U kunt een StorageClass voor extra wensen op het gebied maken `kubectl`. Het volgende voorbeeld maakt gebruik van Premium Managed Disks en geeft aan dat de onderliggende Azure-schijf moet worden *bewaard* wanneer de schil wordt verwijderd:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-premium-retain
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Retain
parameters:
  storageaccounttype: Premium_LRS
  kind: Managed
```

## <a name="persistent-volume-claims"></a>Claims van permanent volume

Een PersistentVolumeClaim vraagt een schijf of bestand opslag van een bepaalde StorageClass, toegangsmodus en grootte. De Kubernetes API-server kunt richten op dynamische wijze de onderliggende opslagresource in Azure als er geen bestaande resource om te voldoen aan de claim op basis van de gedefinieerde StorageClass. De definitie van de schil bevat de volumekoppelpunt wanneer het volume is verbonden met de schil.

![Permanent volume claims in een cluster Azure Kubernetes-Services (AKS)](media/concepts-storage/persistent-volume-claims.png)

Een PersistentVolume is *gebonden* naar een PersistentVolumeClaim zodra de resource van een beschikbare opslag is toegewezen aan de schil aanvragen. Er is een 1:1 toewijzing van permanente volumes op claims.

Het volgende voorbeeld YAML-manifest ziet u een permanent volume-claim die gebruikmaakt van de *beheerde premium* StorageClass en vraagt een schijf *5Gi* in grootte:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

Wanneer u een pod-definitie maakt, wordt de claim permanent volume is opgegeven om aan te vragen van de gewenste opslag. Ook vervolgens geeft u de *volumeMount* voor uw toepassingen te lezen en schrijven van gegevens. Het volgende voorbeeld YAML-manifest laat zien hoe de vorige permanent volume claim kan worden gebruikt om een volume koppelen aan */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken van dynamische en statische volumes die gebruikmaken van Azure-schijven of Azure-bestanden, de volgende praktische artikelen:

- [Maak een statische volume met behulp van Azure-schijven][aks-static-disks]
- [Maak een statische volume met behulp van Azure Files][aks-static-files]
- [Maken van een dynamisch volume met behulp van Azure-schijven][aks-dynamic-disks]
- [Maken van een dynamisch volume met behulp van Azure Files][aks-dynamic-files]

Zie de volgende artikelen voor meer informatie over core Kubernetes en concepten voor AKS:

- [Kubernetes / AKS-clusters en workloads][aks-concepts-clusters-workloads]
- [Kubernetes / AKS-identiteit][aks-concepts-identity]
- [Kubernetes / AKS-beveiliging][aks-concepts-security]
- [Kubernetes / AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes / AKS schalen][aks-concepts-scale]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[aks-static-disks]: azure-disk-volume.md
[aks-static-files]: azure-files-volume.md
[aks-dynamic-disks]: azure-disks-dynamic-pv.md
[aks-dynamic-files]: azure-files-dynamic-pv.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-network]: concepts-network.md
