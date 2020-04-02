# Compilação da API C++ do TensorFlow Lite para inferencia
Este arquivo descreve os passos para fazer a compilação da API C++ do Tensorflow Lite para  inferência com NDK para Android.

### Objetivo
Existem alguns poucos tutoriais que descrevem os passos da compilação da API C++ do TF Lite. No entanto, seguir apenas um único desses tutoriais pode não corresponder as suas necessidades e versões, você pode passar muito tempo tentando fazer isso. Pensando nessas possibilidades, criamos este passo-a-passo que nos guiará na criação dos arquivos necessário para inferência com a API C++ do TFLite. Com isso teremos mais um tutorial que poderá ajudar você na busca por informações desse seguimento.

#### Nota
Nesse tutorial assumimos que você já possui um projeto Android e também um modelo do arquivo do Tensorflow Lite ```.tflite```.
Se você precisar de um projeto Android de demonstração veja este site [Exemplos para Android com Tensorflow](https://github.com/tensorflow/examples/blob/master/lite/examples/image_classification/android/README.md). Se você ainda não converteu seu modelo do Keras ou Tensorflow para o Tensorflow Lite veja este site para a versão 2.x [Convertsor TF Lite 2.x](https://www.tensorflow.org/lite/convert) ou este para versões do TF 1.x [Conversor TF Lite 1.x](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/g3doc/r1/convert/python_api.md).

### Requisitos
Para seguir com a compilação corretamente você precisará dos arquivos e versões a seguir:
#### windows
* Python 3.6
* Git bash ou MSYS64
* Bazel 2.0.0 - Siga os passos no site [aqui](https://docs.bazel.build/versions/master/install-windows.html).
* Visual C++ Redistributable for Visual Studio 2015 [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=48145)
* Tensorflow 1.14.0

### Instalação
Clone o repositório oficial do Tensorflow v1.14.0 [obtido aqui](https://github.com/tensorflow/tensorflow/releases/tag/v1.14.0). Se você clonar sem especificar a versão, será feito o clone da vesão mais recente na ```brach master``` do tensorflow. Logo, especifique a brach da versão 1.14.0 ``` git checkout branch_name # r1.9, r1.10, etc.``` ou siga direto por [aqui](https://github.com/tensorflow/tensorflow/releases/tag/v1.14.0).

