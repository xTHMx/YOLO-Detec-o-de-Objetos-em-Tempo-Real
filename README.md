Pesquisa realizada por Tulio Miranda sob a orientação do Prof. Dr. Wallace Correa de Oliveira Casaca.

Fontes dos Algoritmos Usados: 
YoloV5: [https://github.com/ultralytics/yolov5](https://github.com/ultralytics/yolov5) 
YoloV7: [https://github.com/WongKinYiu/yolov7](https://github.com/WongKinYiu/yolov7)

Repositório sem pesos (do modelos), apenas com dados customizados e versões do algoritmo que foram utilizados na pesquisa.



## Instalação

O YOLO pode ser utilizado na própria maquina do usuário ou em um interpretador web de python.

### Máquina local

É necessário ter a versão mais recente do python, que pode ser adquirida por: https://www.python.org/downloads/
	ATENÇÃO: O algoritmo pode não funcionar em versões muito recentes, pelo fato de não haver versões compatíveis dos pacotes necessário para executar o YOLO.

Para usar o YOLO tudo que é preciso é baixar os arquivos de cada versão e instalar as dependências encontradas no arquivo `requirements.txt` usando o comando pip:

```
pip install -r requirements.txt
```

A partir disso, já é possível executar todos os programas utilizando a pasta como fonte.



## Como usar!

### Detecção

Supondo que já existe um modelo gerado pelo algoritmo, seja um dos que já vem com o próprio YOLO ou um personalizado feito pelo usuário, o processo de uso para detecção é bem simples e rápido.

O programa `detect.py` é utilizado para realizar as detecções e pode ser utilizado em diversas fontes, sejam vídeos, webcam, imagens ou links.

```py
python detect.py --source 0  # webcam
                          img.jpg  # image
                          vid.mp4  # video
                          screen  # screenshot
                          path/  # directory
                         'path/*.jpg'  # glob
                         'https://youtu.be/LNwODJXcvt4'  # YouTube
                         'rtsp://example.com/media.mp4'  # RTSP, RTMP, HTTP stream
```

Exemplo de execução:

```py
python detect.py --weights yolov5s.pt --img 640 --conf 0.25 --source data/images
```

Os dados resultantes aparecem em `runs/train/exp`.

### Validação

A validação serve para calcular a precisão de um modelo, utiliza-se os programas `val.py` ou `test.py`.

No Yolov5:
```
python val.py --weights yolov5s.pt --data coco.yaml --img 640
```
	weights: é o modelo.
	data: dataset, um .yaml.
	img: tamanho final das imagens quando passadas para processar.
No Yolov7:

```
python test.py --data data/coco.yaml --img 640 --batch 32 --conf 0.001 --iou 0.65 --device 0 --weights yolov7.pt
```
	weights: é o modelo.
	data: dataset, um .yaml.
	img: tamanho final das imagens quando passadas para processar.
	batch: tamanho do batch
	conf: limite minimo de confiabilidade
	iou: limite do *Intersect over Union* (intersecção sobre união)
	device: dispositivo ex) webcam (0)

Os dados resultantes aparecem em `runs/val/exp`.

### Treinamento

O treinamento serve para gerar um modelo e seus pesos á partir de um dataset customizado. Esse processo é realizado utilizando o programa `train.py`.

Esse dataset se trata de um arquivo `.yaml` que contem informações dos locais das imagens para treino e validação, juntamente com os índices e nomes da cada classe.

Para treinar um modelo, além do dataset, serão necessários imagens e rótulos. Esses rótulos são arquivos com dados que representam o tipo de classe e as dimensões e posição da área que representa o objeto. 
Ambos imagem e rótulos devem estar localizados no mesmo diretório.

Nas pastas já existem os datasets originais que podem ser utilizados, como por exemplo, o dataset do COCO128 (`coco128.yaml`)

Exemplo de comando para treinar um modelo:

```
python train.py --img 640 --batch 16 --epochs 3 --data coco128.yaml --weights yolov5s.pt --cache
```
	Treinando um modelo yolov5s em um dataset COCO128 por 3 épocas

Vale ressaltar que o YOLO possui diferentes modelos base para ser treinados. Esses modelos diferem-se em relação ao tamanho da sua rede neural.

## Inferência

Video:
```
python detect.py --weights yolov7.pt --conf 0.25 --img-size 640 --source yourvideo.mp4
```

Imagem:
```
python detect.py --weights yolov7.pt --conf 0.25 --img-size 640 --source inference/images/horses.jpg
```
