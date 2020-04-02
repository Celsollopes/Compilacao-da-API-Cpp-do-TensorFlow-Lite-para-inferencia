# Compilação da API C++ do TensorFlow Lite para inferencia
Este arquivo descreve os passos para fazer a compilação da API C++ do Tensorflow Lite para  inferência com NDK para Android.

### Objetivo
Existem alguns poucos tutoriais que descrevem os passos da compilação da API C++ do TF Lite. No entanto, seguir apenas um único desses tutoriais pode não corresponder as suas necessidades e versões, você pode passar muito tempo tentando fazer isso. Pensando nessas possibilidades, criamos este passo-a-passo que nos guiará na criação dos arquivos necessário para inferência com a API C++ do TFLite. Com isso teremos mais um tutorial que poderá ajudar você na busca por informações desse seguimento.

Nesse tutorial assumimos que você já possui um projeto Android e também um modelo do aruivo do Tensorflow Lite .tflite.
Se você precisar de um projeto Android de demonstração veja este site [Exemplos para Android com Tensorflow](https://github.com/tensorflow/examples/blob/master/lite/examples/image_classification/android/README.md). Se você ainda não converteu seu modelo do Keras ou Tensorflow para o Tensorflow Lite veja este site para a versão 2.x [Convertsor TF Lite 2.x](https://www.tensorflow.org/lite/convert) ou este para versões do TF 1.x [Conversor TF Lite 1.x](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/g3doc/r1/convert/python_api.md).

### Requisitos
Estamos assumindo que voce já possui o seu projeto Android criado e seguimos para criação do TF lite. 
Para seguir com a compilação corretamente você precisará dos arquivos e versões a seguir:

* 
