
De NC- en NV-grootten worden ook wel GPU-exemplaren genoemd. Dit zijn speciale virtuele machines die NVIDIA GPU-kaarten bevatten, geoptimaliseerd voor verschillende scenario’s en toepassingen. De NV-grootten zijn geoptimaliseerd en ontworpen voor externe visualisatie, streaming, games, codering en VDI-scenario’s waarin frameworks als OpenGL en DirectX worden gebruikt. De NC-grootten zijn meer geoptimaliseerd voor reken- en netwerkintensieve toepassingen en algoritmen, waaronder CUDA - en OpenCL-toepassingen en -simulaties. 


De NV-exemplaren worden aangestuurd door een NVIDIA Tesla M60 GPU-kaart en NVIDIA GRID voor bureaubladtoepassingen en virtuele bureaubladen waarmee klanten hun gegevens of simulatie kunnen visualiseren. Gebruikers kunnen hun grafisch intensieve werkstromen op de NV-exemplaren visualiseren voor grafische mogelijkheden en daarnaast enkelvoudige precisieworkloads uitvoeren, zoals codering en rendering. De Tesla M60 biedt 4096 CUDA-kernen in een ontwerp met dubbele GPU, met maximaal 36 streams van 1080p H.264. 

De NC-exemplaren worden aangestuurd door een NVIDIA Tesla R80-kaart. Gebruikers kunnen gegevens nu veel sneller verwerken door gebruik te maken van CUDA voor energie-exploratietoepassingen, crashsimulaties, ray traced rendering, 'deep learning' en meer. De Tesla R80 levert 4992 CUDA-kernen met een ontwerp met dubbele GPU prestaties tot 2,91 Teraflops dubbele precisie en maximaal 8,93 Teraflops enkele precisie.

## <a name="nv-instances"></a>NV-exemplaren

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 |
| Standard_NV12 |12 |112 |680 | 2 | 16 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 |

1 GPU = halve M60-kaart.

## <a name="nc-instances"></a>NC-exemplaren

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | Maximumaantal gegevensschijven |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 8 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 |

1 GPU = halve K80-kaart.

*RDMA-compatibel


