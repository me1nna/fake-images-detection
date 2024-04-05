# <a href="https://www.kaggle.com/competitions/generated-or-not/overview">Детекция сгенерированных изображений (fake image detection)</a>

Задача fake image detection заключается в том, чтобы отличить сгенерированные нейросетью изображения от реальных. В свете последних событий с выходом нейросетей для генерации изображений, задача становится всё более актуальной.  

## Подходы к решению задачи:

### 1.Самый и очевидный и простой: обучение сверточной сети или трансформер на бинарную классификацию

  1.1) Написать простую сверточную сеть и обучить на трейне, например, как делают <a href=https://gaurijagatap.github.io/assets/CGI.pdf>тут</a> (в файле ResNet.ipynb обучение)

  1.2) Зафайнтьюнить ResNet (изменить последний слой):

  ![image](https://github.com/me1nna/fake-image-detection/assets/78222093/7ec6d5d4-619c-4802-9360-75ccdbe2d1a3)

  
  лучший скор на Kaggle (на 02.04) (в файле ResNet.ipynb обучение):
  ![image](https://github.com/me1nna/fake-image-detection/assets/78222093/2313e179-74ce-431c-9460-5333dedc0e87)


  1.3) Зафайнтьюнить ViT

  Проблема: сеть, очевидно, переобучится под генератор, с помощью которого созданы картинки из трейна и теста, имеющие метку 1 (сгенерировано ИИ)


### 2. <a href="https://arxiv.org/pdf/2302.10174.pdf"> Вытащить фичи из изображений и далее обучить простой классификатор</a>

Классификатор в таком случае будет менее склонен к переобучению: обучаемая компонента намного меньше, чем в представленном выше способе.
Как решать? Взять, например, image-encoder из CLIP. С его помощью получаем эмбеддинги всех изображений из тренировчного датасета. Далее обучаем на этих эмбеддингах классификатор. Классификаторы можно обучать разные. Иллюстрация такого метода будет примерно такая:

![image](https://github.com/me1nna/fake-image-detection/assets/78222093/6900549b-1a0e-456e-8e4a-00308a9996f0)

  2.1) Обучить логистическую регрессию на эмеддингах из изображений, вытащенных с помощью CLIP.
  
  Лучший результат на Kaggle (на 05.04) (обучение в KNN_logreg.ipynb):
  ![image](https://github.com/me1nna/fake-image-detection/assets/78222093/c0977442-1512-4fe6-b099-c3e733199da9)


  2.2) Обучить KNN на эмеддингах из изображений, вытащенных с помощью CLIP (обучение в KNN_logreg.ipynb).

  2.3) Случайный лес

  2.4) Градиентный бустинг
