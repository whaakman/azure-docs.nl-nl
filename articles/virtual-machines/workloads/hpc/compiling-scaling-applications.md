---
title: HPC-toepassingen - Azure Virtual Machines schalen | Microsoft Docs
description: Informatie over het schalen van HPC-toepassingen op Azure Virtual machines.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 00d5b86c8cae01d342d55b7ad20ec59c3f7530bd
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707843"
---
# <a name="scaling-hpc-applications"></a>HPC-toepassingen schalen

Optimale prestaties voor de scale-up en scale-out van HPC-toepassingen op Azure vereist het afstemmen van prestaties en optimalisatie van experimenten voor de specifieke werkbelasting. In deze sectie en de VM-serie-specifieke pagina's bieden algemene richtlijnen voor het schalen van uw toepassingen.

## <a name="compiling-applications"></a>Compileren van toepassingen

Hoewel niet nodig, biedt compileren van toepassingen met optimalisatie van de juiste vlaggen de beste prestaties van de scale-up op HB en HC-serie VM's.

### <a name="amd-optimizing-cc-compiler"></a>AMD optimaliseren C /C++ Compiler

De AMD optimaliseren C /C++ Compiler (AOCC) compiler systeem biedt een hoge mate van geavanceerde optimalisaties, multithreading, en processorondersteuning met globale optimalisatie, vectorisatie in, tussen procedurele analyses, transformaties van de lus, en genereren van code. AOCC compiler binaire bestanden zijn geschikt voor Linux-systemen met GNU C-bibliotheek (glibc) versie 2.17 en hoger. De compiler suite bestaat uit een C /C++ compiler (clang), een compiler Fortran (FLANG) en een Fortran front end-to-Clang (Dragon ei).

### <a name="clang"></a>Clang

CLang is een C C++, en Objective-C-compiler voorverwerking, parseren, optimalisatie, genereren van code, assembly verwerken en te koppelen. CLang ondersteunt de `-march=znver1` vlag om in te schakelen van best-code genereren en afstemmen voor AMD van Zen op basis van x86 architectuur.

### <a name="flang"></a>FLANG

De compiler FLANG is een recente toevoeging aan het AOCC-pakket (April 2018 toegevoegd) en is momenteel in de voorlopige versie voor ontwikkelaars om te downloaden en te testen. Op basis van Fortran 2008, AMD breidt de GitHub-versie van FLANG (https://github.com/flangcompiler/flang). De compiler FLANG biedt ondersteuning voor alle Clang compileropties en een aantal extra FLANG-specifieke opties.

### <a name="dragonegg"></a>DragonEgg

DragonEgg is een gcc-invoegtoepassing die wordt vervangen door de GCC-optimalisatieprogramma's en code generatoren met die van het project LLVM. DragonEgg die wordt geleverd met AOCC werkt met gcc-4.8.x voor x86-32/x86-64 doelen is getest en met succes is gebruikt in verschillende Linux-platforms.

GFortran is de frontend van de werkelijke voor Fortran programma's die verantwoordelijk is voor voorverwerking, parseren en semantische analyse genereren van de GCC GIMPLE tussenliggende weergave (IR). DragonEgg is een invoegtoepassing GNU aansluiten bij GFortran compilatie flow. De invoegtoepassing GNU API geïmplementeerd. Met de invoegtoepassing-architectuur wordt DragonEgg het stuurprogramma compiler besturen van de verschillende fasen van de compilatie.  Na het downloaden en installatie-instructies te volgen, worden Dragon ei aangeroepen met: 

```bash
$ gfortran [gFortran flags] 
   -fplugin=/path/AOCC-1.2-Compiler/AOCC-1.2-     
   FortranPlugin/dragonegg.so [plugin optimization flags]     
   -c xyz.f90 $ clang -O3 -lgfortran -o xyz xyz.o $./xyz
```
   
### <a name="pgi-compiler"></a>BGa Compiler
BGa Community Edition versie 17 is om te werken met AMD EPYC bevestigd. Een bGa gecompileerde versie van STREAM levert volledige geheugenbandbreedte van het platform. De nieuwere Community Edition 18.10 (november 2018) moeten ook goed werken. Hieronder volgt compiler optimaal met de Intel-Compiler monster CLI:

```bash
pgcc $(OPTIMIZATIONS_PGI) $(STACK) -DSTREAM_ARRAY_SIZE=800000000 stream.c -o stream.pgi
```

### <a name="intel-compiler"></a>Intel Compiler
Intel-Compiler versie 18 is om te werken met AMD EPYC bevestigd. Hieronder vindt u voorbeeld CLI voor compileerprogramma optimaal met de Intel-Compiler.

```bash
icc -o stream.intel stream.c -DSTATIC -DSTREAM_ARRAY_SIZE=800000000 -mcmodel=large -shared-intel -Ofast –qopenmp
```

### <a name="gcc-compiler"></a>GCC-Compiler 
Voor HPC raadt AMD GCC-compiler 7.3 of hoger. Oudere versies, zoals 4.8.5 opgenomen met RHEL/CentOS 7.4, worden niet aanbevolen. GCC 7.3 en hoger, verzorgt aanzienlijk betere prestaties op HPL HPCG en DGEMM tests.

```bash
gcc $(OPTIMIZATIONS) $(OMP) $(STACK) $(STREAM_PARAMETERS) stream.c -o stream.gcc
```

## <a name="scaling-applications"></a>Toepassingen vergroten/verkleinen 

De volgende suggesties voor optimale toepassing schalen efficiency, prestaties en consistentie van toepassing:

* Pincode worden verwerkt voor het gebruik van cores-0-59 een sequentiële vastmaken benadering (in plaats van een aanpak die automatisch worden verdeeld). 
* Binding met Numa/Core/HwThread is beter dan standaard-binding.
* Voor hybride parallelle toepassingen (OpenMP + MPI), 4 threads en 1 MPI rang per CCX te gebruiken.
* Experimenteer met 1-4 die MPI ranks per CCX voor optimale prestaties voor pure MPI-toepassingen.
* Sommige toepassingen met extreme gevoeligheid geheugenbandbreedte mogelijk voordeel hebben van een kleiner aantal kernen per CCX. Voor deze toepassingen met behulp van 3 of 2 kernen per CCX kan geheugen bandbreedte conflicten verminderen en resulteert in betere prestaties voor real-world of consistente schaalbaarheid. In het bijzonder kunnen MPI Allreduce profiteren van deze.
* Voor veel grotere schaal worden uitgevoerd, is het aanbevolen UD of hybride RC + UD transporten gebruiken. Veel bibliotheken voor MPI-bibliotheken/runtime hiervoor intern (zoals UCX of MVAPICH2). Controleer de configuraties van uw transport voor grootschalige worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) op Azure.
