# Laboratorium 1 Sprawozdanie - Justyna Baran
#### P1.3. W zadaniu P1.1 zbudowany został plik konfiguracyjny Dockerfile dla serwera HTTP_PHP. Proszę, wzorując się na teście przedstawionym na rysunku 1.6, określić czas budowania obrazu wykorzystując klasyczny silnik build oraz nowy silnik BuildKit. Proszę pamiętać o opcji wyłączającej wykorzystanie pomięci cache w procesach build.

#### Dockerfile: Dockerfile_scratch
#### Budowanie obrazu:

`docker build -f Dockerfile_scratch -t server-php-apache .`

![image1](https://user-images.githubusercontent.com/105118113/169270683-2b48b76b-e684-47fa-8eaa-1a02ea7dbad9.JPG)

#### Uruchomienie obrazu:

`docker run -it -d -p 8080:80 server-php-apache`

![image2](https://user-images.githubusercontent.com/105118113/169271246-27f49fcf-7ed7-4e92-acd6-cc06cc45e83d.JPG)

![obraz](https://user-images.githubusercontent.com/105118113/169293365-aa1ea039-0f74-4ee5-9e09-640f9e8fcb5c.png)

#### Porównanie czasów realizacji procesu budowania w oparciu o klasyczny i nowy silnik BuildKit w środowisku Docker.

`time DOCKER_BUILDKIT=0 docker build -q --no-cache -f Dockerfile_scratch .`

`time DOCKER_BUILDKIT=1 docker build -q --no-cache -f Dockerfile_scratch .`

![image4](https://user-images.githubusercontent.com/105118113/169272425-4976991c-2606-4290-a77a-d7226e4cc4ea.JPG)

#### P5.1. W tym zadaniu należy: 1. Utworzyć środowisko budowania obrazów wieloplatformowych na bazie wrapera buildx. Proszę przyjąć założenie, że QEMU będzie zainstalowane lokalnie. 2. Na podstawie Dockerfile z rysunku 1.21 proszę zbudować obrazy dla czterech wybranych architektur sprzętowych. 3. Na koniec, proszę zweryfikować poprawność procesu budowania obrazów. W sprawozdaniu należy umieścić wszystkie użyte polecenia wraz w wynikiem ich działania oraz krótki opis wykonanych czynności wraz z ewentualnymi komentarzami.

1. Instalacja pakietu QEMU w lokalnym systemie plików:

`sudo apt-get install -y qemu-user-static`

![image5](https://user-images.githubusercontent.com/105118113/169279860-018aa83d-4a69-43dd-a67f-637cd0efe110.JPG)

2. Utworzenie środowiska budowania obrazów wykorzystującego wraper buildx

`docker buildx create --name testbuilder`

`docker buildx use testbuilder`

`docker buildx inspect --bootstrap`

![image6](https://user-images.githubusercontent.com/105118113/169280544-4a5065be-1834-4704-acbd-955eb4064b0f.JPG)

Przykładowa informacja o środowisku buildx

`docker buildx ls`

![obraz](https://user-images.githubusercontent.com/105118113/169280726-b1c03404-c8c1-4402-a807-3d5af8905391.png)

3. Obrazy dla czterech architektur sprzętowych: linux/amd64, linux/arm64, linux/arm/v7, linux/s390x na podstawie Dockerfile za pomocą buildx oraz QEMU

`docker buildx build -t s99171/lab:bx --platform=linux/amd64,linux/arm64,linux/arm/v7,linux/s390x --push .`

![obraz](https://user-images.githubusercontent.com/105118113/169281364-6a68e801-59c5-4a54-979a-dd1834a7f02c.png)

4. Weryfikacja parametrów utworzonych obrazów dla wybranych architektur sprzętowych

`docker buildx imagetools inspect s99171/lab:bx`

![obraz](https://user-images.githubusercontent.com/105118113/169281519-9b1bb3ae-d325-4591-98bc-c91dc79e973d.png)





