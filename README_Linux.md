# Compilação da API C++ para inferencia com TensorFlow Lite (Linux)

Tutorial para a compilação das bibliotecas do TFlite para serem incluídas no projeto Android. O processo foi realizado utilizando o [**Ubuntu 18.04 LTS**](https://releases.ubuntu.com/18.04.4/), sem instalação de componentes auxiliares.
## 1. Download e instalação de dependências
Para realizar o *download* e instalação das dependências, será necessário seguir os passos a seguir 

### 1.1. Compilação
[![Python badge](https://img.shields.io/badge/Python-3.6-blue.svg)](https://www.python.org/downloads/release/python-360/)
[![Bazel](https://img.shields.io/badge/Bazel-0.27.1-blue.svg)](https://docs.bazel.build/versions/0.27.0/install-ubuntu.html)
[![Tensorflow](https://img.shields.io/badge/Tensorflow-v2.1.0-blue.svg)](https://github.com/tensorflow/tensorflow/releases/tag/v2.1.0)

#### 1.1.1. Python
Normalmente, o `Python 3.6` já vem instalado nas *releases* mais novas do Ubuntu. Mas, pode ser instalado através da execução do conjunto de comandos, abaixo. Vale observar que há uma dependência pelo pacote `numpy`, dessa forma, ele também deve ser instalado.

```bash
sudo apt update

yes | sudo apt upgrade
yes | sudo apt-get install python3 python3-pip python3-dev

pip3 install --upgrade pip3
pip3 install numpy
```

#### 1.1.2. Bazel
O bazel é a ferramenta de compilação do tensorflow. Para instalar a utilizada no processo deste tutorial, é preciso seguir os seguintes passos:

```bash
yes | sudo apt install pkg-config zip g++ zlib1g-dev unzip python

wget http://github.com/bazelbuild/bazel/releases/download/0.27.1/bazel-0.27.1-installer-linux-x86_64.sh
chmod +x bazel-0.27.1-installer-linux-x84_64.sh
bazel-0.27.1-installer-linux-x84_64.sh --user
bazel --version
```

#### 1.1.3. Tensorflow
O `Tensorflow` deve ser baixado do [repositório oficial](https://github.com/tensorflow/tensorflow) através da utilização do git. Para instalar o git e fazer o download dos arquivos, é preciso executar os seguintes comandos:

```bash
yes | sudo apt install git

git clone https://github.com/tensorflow/tensorflow
cd tensorflow
git checkout v2.1.0
```

### 1.2. Android
[![Android NDK](https://img.shields.io/badge/NDK-18b-yellow.svg)](https://developer.android.com/ndk/downloads/older_releases?hl=pt-br)
[![Android SDK](https://img.shields.io/badge/SDK-28.0.3-yellow.svg)](https://developer.android.com/studio#downloads)
[![Android Build Tools](https://img.shields.io/badge/Build%20Tools-v2.1.0-yellow.svg)](https://developer.android.com/studio#downloads)


#### 1.2.1. Android NDK
É possível fazer o *download* da NDK através do site do Android ou através de linha de comandos. Para fazer pelo terminal, é preciso executar os seguintes passos:

```bash
yes | apt install zip

wget https://dl.google.com/android/repository/android-ndk-r18b-linux-x86_64.zip
unzip android-ndk-r18b-linux-x86_64.zip
rm android-ndk-r18b-linux-x86_64.zip
```

#### 1.2.2. Android SDK e Android Build tools
Para fazer o *download* do `SDK` e do `build tools` nas versões indicadas, é preciso baixar a `ferramenta de linha de comando` do Android Studio. É possível fazer o *download* através do site
É possível fazer o download da NDK através do [site](https://developer.android.com/studio#downloads) ou executando os seguintes comandos a seguir. Vale observar que no momento da confecção deste tutorial a versão estável é a `6200805`.

```bash
wget https://dl.google.com/android/repository/commandlinetools-linux-6200805_latest.zip
unzip commandlinetools-linux-6200805_latest.zip
rm commandlinetools-linux-6200805_latest.zip

export PATH=$PWD/tools/:$PWD/tools/bin:$PATH
```

Após a instalação da ferramenta, ainda com o terminal aberto, será feito o *download* do SDK e build tools nas versões **28** e **28.0.3**, respectivamente.

```bash
ANDROID_COMPILE_SDK=28
BUILD_TOOLS_VERSION=29.0.2

export ANDROID_HOME=$PWD/android-sdk-linux

yes | sdkmanager --sdk_root=${ANDROID_HOME} --licenses
sdkmanager --sdk_root=${ANDROID_HOME} "build-tools;28.0.3" "platform-tools" "platforms;android-28"
```

## 2. Compilação Tensorflow

Para realizar o processo de compilação, é preciso fazer a configuração do pacote do tensorflow. O processo é definido pela execução do `./configure`, presente dentro do diretório do tensorflow. No processo de configuração serão solicitadas informações sobre a compilação e é **muito importante** preencher corretamente. 

A configuração errada pode inviabilizar o processo de compilação.

```bash
cd tensorflow
./configure
```

No processo, será necessário responder um conjunto de pergunta acerca do processo de geração das bibliotecas. Nas perguntas relacionadas ao android, é necessário fornecer os caminhos dos pacotes baixados.

```bash
Please specify the home path of the Android NDK to use. [Default is /home/usr/Android/Sdk/ndk-bundle]: /home/usr/Documentos/compilation1/android-ndk-r18b
 
 
Please specify the (min) Android NDK API level to use. [Available levels: ['16', '17', '18', '19', '21', '22', '23', '24', '26', '27', '28']] [Default is 21]: 18
 
 
Please specify the home path of the Android SDK to use. [Default is /home/usr/Android/Sdk]: /home/usr/Documentos/compilation1/android-sdk-linux
 
 
Please specify the Android SDK API level to use. [Available levels: ['28']] [Default is 28]:
 
 
Please specify an Android build tools version to use. [Available versions: ['28.0.3', '29.0.2']] [Default is 29.0.2]: 28.0.3
```
É possível fazer o *download* de mais de um `SDK`, `NDK` ou `build tool` e por isso é importante selecionar corretamente os valores.

A seguir, é possível observar um exemplo de execução do `./configure`.

```bash
$ ./configure

WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
You have bazel 0.27.1 installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3.6
 
 
Found possible Python library paths:
  /usr/lib/python3/dist-packages
  /usr/local/lib/python3.6/dist-packages
Please input the desired Python library path to use.  Default is [/usr/lib/python3/dist-packages]
 
Do you wish to build TensorFlow with XLA JIT support? [Y/n]: N
No XLA JIT support will be enabled for TensorFlow.
 
Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]:
No OpenCL SYCL support will be enabled for TensorFlow.
 
Do you wish to build TensorFlow with ROCm support? [y/N]:
No ROCm support will be enabled for TensorFlow.
 
Do you wish to build TensorFlow with CUDA support? [y/N]:
No CUDA support will be enabled for TensorFlow.
 
Do you wish to download a fresh release of clang? (Experimental) [y/N]: Y
Clang will be downloaded and used to compile tensorflow.
 
Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native -Wno-sign-compare]:
 
 
Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: Y
Searching for NDK and SDK installations.
 
Please specify the home path of the Android NDK to use. [Default is /home/usr/Android/Sdk/ndk-bundle]: /home/usr/Documentos/compilation1/android-ndk-r18b
 
 
Please specify the (min) Android NDK API level to use. [Available levels: ['16', '17', '18', '19', '21', '22', '23', '24', '26', '27', '28']] [Default is 21]: 18
 
 
Please specify the home path of the Android SDK to use. [Default is /home/usr/Android/Sdk]: /home/usr/Documentos/compilation1/android-sdk-linux
 
 
Please specify the Android SDK API level to use. [Available levels: ['28']] [Default is 28]:
 
 
Please specify an Android build tools version to use. [Available versions: ['28.0.3', '29.0.2']] [Default is 29.0.2]: 28.0.3
 
 
Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See .bazelrc for more details.
    --config=mkl            # Build with MKL support.
    --config=monolithic     # Config for mostly static monolithic build.
    --config=ngraph         # Build with Intel nGraph support.
    --config=numa           # Build with NUMA support.
    --config=dynamic_kernels    # (Experimental) Build kernels into separate shared objects.
    --config=v2             # Build TensorFlow 2.x instead of 1.x.
Preconfigured Bazel build configs to DISABLE default on features:
    --config=noaws          # Disable AWS S3 filesystem support.
    --config=nogcp          # Disable GCP support.
    --config=nohdfs         # Disable HDFS support.
    --config=nonccl         # Disable NVIDIA NCCL support.
Configuration finished
```

Após a finalização desse processo, será necessário executar os comandos de compilação para cada um das plataformas desejadas. 

**arm64-v8a**
```bash
bazel build //tensorflow/lite:libtensorflowlite.so --crosstool_top=//external:android/crosstool --cpu=arm64-v8a --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cxxopt="-std=c++11"
```
**armeabi-v7a**
```bash
bazel build //tensorflow/lite:libtensorflowlite.so --crosstool_top=//external:android/crosstool --cpu=armeabi-v7a --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cxxopt="-std=c++11"
```

**x86_64**
```bash
bazel build //tensorflow/lite:libtensorflowlite.so --crosstool_top=//external:android/crosstool --cpu=x86_64 --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cxxopt="-std=c++11"
```
**x86** 
```bash
bazel build //tensorflow/lite:libtensorflowlite.so --crosstool_top=//external:android/crosstool --cpu=x86 --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cxxopt="-std=c++11"
```

Após a compilação, os binários ficam localizados dentro do diretório `tensorflow/bazel-bin/tensorflow/lite/` e o arquivo gerado recebe o nome de `libtensorflowlite.so`.
