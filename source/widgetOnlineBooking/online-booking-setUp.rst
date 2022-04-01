.. _ob-setUp::

Установка виджета онлайн-бронирования
-------------------------------------

Для работы с виджетом Вам понадобится ссылка на виджет. Для её получения произведите следующие действия:

1. Откройте контекстное меню своей услуги
2. Нажмите "Поделиться"
3. Выберите пункт "Ссылка"
4. Готово! Ссылка скопирована в буфер обмена

*За подробностями о параметрах виджета обратитесь к страничке* :ref:`Параметры виджета.<ob-inst>`

Виджет встроен в сайт
~~~~~~~~~~~~~~~~~~~~~

:ref:`Как это выглядит?<widget-ob-exmpl-2>`

#. Используйте тег <iframe>.
#. Внутри тега введите параметры желаемого размера виджета. К примеру, используйте атрибуты height и width.
#. Добавьте атрибут src, значением которого будет ссылка на Вашу услугу.
 
  *Для более подробной работы с iframe обратитесь к руководству по html.*

*Пример:*

.. code-block::

    <iframe> 
    height="500px" width="250px" src="https://web.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
    </iframe>

Виджет открывается по кнопке
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:ref:`Как это выглядит?<widget-ob-exmpl-3>`

*Виджет может открываться 2 видами кнопок:*
   
1. **Стандартная кнопка Torrow**

a. Используйте тег <iframe>
b. Внутри тега введите скрипт виджета (скрипт можно найти на странице :ref:`Параметры виджета<ob-inst>`)
c. Убедитесь, что параметр **show-widget-button = true** и в **url:** вставлена ссылка правильная ссылка
d. Готово!

.. _widgetFFSetUp-2-1::

*Пример:*

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
         modal="right"
         modal-active="true"
         show-widget-button="true"
     ></torrow-widget>
     <script src="https://cdn-public.torrow.net/widget/torrow-widget3.js" defer></script>
    
.. 
    Для более  поднробной настройки страницы нажмите сюда

2. **Выбранная Вами кнопка на сайте**

*Реализация на примере работы с Tilda:*

a. Добавьте изображение кнопки
b. Задайте кнопке CSS Class Name (к примеру, send-appeal) 
c. Добавьте блок HTML-код
d. Введите скрипт (скрипт можно найти на странице :ref:`Параметры виджета<ob-inst>`)
e. Убедитесь, что **show-widget-button = false** и в **url:** вставлена ссылка правильная ссылка
f. Добавьте следующий скрипт:

.. code-block:: 
    
    <script>
        var buttonCollection =  document.getElementsByClassName('send-appeal')
        if(buttonCollection.length) {
            buttonCollection[0].addEventListener('click', event => 
            {document.querySelector('#torrow-widget-second').setAttribute('modal-active', 'true')})
        }
    </script>

g. Готово!

*Пример:* 

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
         modal="right"
         modal-active="true"
         show-widget-button="true"
     ></torrow-widget>
     <script>
        var buttonCollection =  document.getElementsByClassName('send-appeal')
        if(buttonCollection.length) {
            buttonCollection[0].addEventListener('click', event => 
            {document.querySelector('#torrow-widget-second').setAttribute('modal-active', 'true')})
        }
     </script>     
     <script src="https://cdn-public.torrow.net/widget/torrow-widget3.js" defer></script>

.. note:: Обратите внимание, где указано название *CSS Class Name:* **send-appeal** внутри скрипта! В это место необходимо вводить заданное Вами значение.