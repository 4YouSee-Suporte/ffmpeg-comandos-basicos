<h1 align="center" >
    Comandos básicos de ffmpeg <br>
    <img alt="GitHub stars" src="https://img.shields.io/github/stars/4YouSee-Suporte/ffmpeg-comandos-basicos?style=social">
    <img alt="GitHub followers" src="https://img.shields.io/github/followers/4YouSee-Suporte?label=Follow%20me%20%3A%29&style=social">
</h1>

<h1>⚈ Sobre este Projeto</h1>
Este repositório contém alguns comandos básicos para o uso de ffmpeg tais como juntar videos em tempo e espaço, tirar audio, girar video, etc.

<h1>⚈ Comandos </h1>

### ✅ Tirar o audio de um video
> ffmpeg -i example.mkv -c copy -an example-nosound.mkv

### ✅ Juntar por Dimensão Horizontalmente:

Opção 1:
> ffmpeg -i in_1.mp4 -i in_2.mp4 -filter_complex "hstack=inputs=2" out.mp4

Opção 2:
> ffmpeg -i video0.mp4 -i video1.mp4 -map "[v]" -an -filter_complex "[0:v][1:v]hstack=inputs=2[v]" video_dimensao_somada.mp4

Note que o comando acima funciona para 2 vídeos. Para 3 vídeos, seria da seguinte forma:

> ffmpeg -i video0.mp4 -i video1.mp4 -i video2.mp4 -map "[v]" -an -filter_complex "[0:v][1:v][2:v]hstack=inputs=3[v]" video_dimensao_somada.mp4

### ✅ Juntar por Dimensão Verticalmente:

Para mais informações consulte neste post:

[Fonte](https://stackoverflow.com/questions/11552565/vertically-or-horizontally-stack-mosaic-several-videos-using-ffmpeg)

> ffmpeg -i in_1.mp4 -i in_2.mp4 -filter_complex "vstack=inputs=2" out.mp4


### ✅ Juntar por tempo:

Crie um arquivo de texto e nele adicione o nome dos arquivos que você quer juntar. Exemplo:
```
file 'video0.mp4'
file 'video1.mp4'
```

Salve o arquivo com o nome `arquivos.txt`. Em seguida, execute o comando abaixo:

> ffmpeg -f concat -i arquivos.txt -c copy a\a.mp4

> ffmpeg.exe -f concat -safe 0 -i arquivos.txt -c copy a.mp4  <--- No caso do erro "Unsafe file name"


### ✅ Cortar video por tempo:

Cortar um vídeo `entrada.mp4` a partir da duração. Exemplo: os primeiros 5 segundos.

> ffmpeg -i entrada.mp4 -acodec copy -vcodec copy -t 5 salida.mp4
  
A partir do segundo 31, ele corta 30 segundos.

> ffmpeg -ss 31 -i out.mp4 -acodec copy -vcodec copy -t 30 saida.mp4

### ✅ Cortar video por espaço:

> ffmpeg -i in.mp4 -filter:v "crop=out_w:out_h: x:y" out.mp4

Onde as opções são as seguintes:

out_w é a largura do retângulo de saída
out_h é a altura do retângulo de saída
x e y especifique o canto superior esquerdo do retângulo de saída

> ffmpeg.exe -i input.mp4 -filter:v "crop=2880:540:961:540" out.mp4

O anterior exemplo corta um video de dimensões 3840(largura)x540(altura) um retângulo de acordo com as seguintes indicações:

Largura de : 2880px

Altura de : 540px

Na posição x : 961

E na posição y : 540

### ✅ Tirar fondo verde

[Fonte](https://ffmpeg.org/ffmpeg-filters.html#chromakey)

Opção 1

Onde a cor é 0x3BBD1E.

Tenha um arquivo chamado Background.png que é uma imagem com fundo transparente com as mesmas dimensões do vídeo.

> ffmpeg -i background.png -i video.mp4 -filter_complex "[1:v]colorkey=0x3BBD1E:0.3:0.2[ckout];[0:v][ckout]overlay[out]" -map "[out]" output.webm

Melhorando a qualidade e gerando um webm.

> ffmpeg -i background.png -i video.mov -b:v 10M -r 60 -filter_complex "[1:v]colorkey=0x000000:0.3:0.2[ckout];[0:v][ckout]overlay[out]" -map "[out]" output5.webm

Opção 2

O valor crf define a qualidade do vídeo e varia de 0 a 51

> ffmpeg -y -rtbufsize 100M -f gdigrab -framerate 30 -probesize 10M -draw_mouse 1 -i desktop -c:v libx264 -r 30 -preset ultrafast -tune zerolatency -crf 25 -pix_fmt yuv420p "video output.mp4"


### ✅ Transparencia

A partir de um vídeo com fundo transparente, ele é convertido em um vídeo .webm mantendo a qualidade.

> ffmpeg -i video.mov -b:v 10M output6.webm

### ✅ Girar video

[Fonte ](https://www.ostechnix.com/how-to-rotate-videos-using-ffmpeg-from-commandline/)

> ffmpeg -i input.mp4 -vf "transpose=1" output.mp4

Aqui, o parâmetro transpose = 1 instrui o FFMpeg a transpor o vídeo fornecido em 90 graus no sentido horário. Aqui está a lista de parâmetros disponíveis para o recurso de transposição.

0 - girar 90 graus no sentido anti-horário e girar verticalmente. Este é o padrão.
1 - Girar 90 graus no sentido horário.
2 - Girar 90 graus no sentido anti-horário.
3 - Gire 90 graus no sentido horário e gire verticalmente.


### ✅ Frames del Video

Esse comando vai obter todos os frames de um video e gerar imagens por cada um deles.

> ffmpeg -i video.mp4 thumb%04d.jpg -hide_banner


### ✅ Video a gif

> ffmpeg -i input.mp4 -vf scale="1000:563" 1.gif -hide_banner 


### ✅ Passar video pelo hanbrake

Esse comando é o equivalente a passar o video pelo handbrake. 

> ffmpeg -i video.mp4 -bufsize 1M -maxrate 2M -c copy -an out.mp4
