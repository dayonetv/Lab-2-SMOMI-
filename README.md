# С использованием примера [2] обучить нейронную сеть EfficientNet-B0 [3,4,5] (случайное начальное приближение) для решения задачи классификации изображений Oregon WildLife [6]
Для решения задачи классификации изображений Oregon Wildlife использовалась нейронная сеть EfficientNet-B0. Причем данная нейронная сеть будет со случайным начальным приближением, а это значит, что веса нашей неройронной сети не будут предобученными, а будут случайными. 
Синяя линия - на валидации  
Оранжевая линия - на обучении  
* График метрики точности для нейронной сети EfficientNet-B0 (случайное начальное приближение):
<img src="./epoch_categorical_accuracy_no_imagenet.svg">

* График функции потерь для нейронной сети EfficientNet-B0 (случайное начальное приближение):

<img src="./epoch_loss_no_imagenet.svg">

* Анализ полученных результатов  
# С использованием [2] и техники обучения Transfer Learning [7] обучить нейронную сеть EfficientNet-B0 (предобученную на базе изображений imagenet) для решения задачи классификации изображений Oregon WildLife
В данной части лабораторной работы по-прежнему использовалась нейронная сеть EfficientNet-B0, однако в данном случае нейронная сеть будет иметь уже предобученные веса на базе изображений ImageNet. Для того, чтобы наши веса были предобученными на базе изображений ImageNet, необходимо параметр weights выставить равным "imagenet":
```
model = EfficientNetB0(include_top=False, weights="imagenet")(inputs)  
```
Также важным параметром является include_top, так как датасет (ImageNet) включает в себя набор из нескольких миллионов картинок распределенных на 1000 классов, однако задача нашей нейронной сети заключается в классификации 12000 изображений дикой природы Oregon Wildlife распределенных на 20 классов. Соответственно нас не интересует та часть нейронной сети EfficientNet-B0, которая отвечает за классификацию изображений ImageNet. Поэтому параметр include_top будет равен False. Таким образом остается лишь та часть нейронной сети, которая отвечает за выделение характерных признаков в изображении, а сам классификатор убирается.  
Так как та часть нейронной сети EfficientNet-B0, которая отвечает за выделение каких-то характерных признаков в изображении уже обучена, то мы можем "заморозить" данную часть и не обучать её, а будет обучаться только классификатор ( нейронная сеть будет использоваться в качестве средства извлечения признаков):
```
model.trainable = False
```
Далее состовляется классификатор (выходные слои нейронной сети)
