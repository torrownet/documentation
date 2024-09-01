.. _widgetffsetUp:

Установка полного виджета онлайн-записи
---------------------------------------

Для работы с виджетом Вам понадобится ссылка на виджет. Для её получения произведите следующие действия:

1. Откройте контекстное меню своей услуги
2. Нажмите "Поделиться"
3. Выберите пункт "Ссылка"
4. Готово! Ссылка скопирована в буфер обмена

*За подробностями о параметрах полного виджета обратитесь к страничке:* :ref:`Параметры полного виджета<widgetffinst>`

Виджет встроен в сайт
~~~~~~~~~~~~~~~~~~~~~

:ref:`Как это выглядит?<widget-ff-exmpl-2>`

#. Используйте тег <iframe>.
#. Внутри тега введите параметры желаемого размера виджета. К примеру, используйте атрибуты height и width.
#. Добавьте атрибут src, значением которого будет ссылка на Вашу услугу.
 
  *Для более подробной работы с iframe обратитесь к руководству по html.*

Пример:
'''''''

.. code-block::

    <iframe height="500px" width="250px" src="https://web.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking">
    </iframe>

Виджет открывается по кнопке
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:ref:`Как это выглядит?<widget-ff-exmpl-3>`

*Виджет может открываться 2 видами кнопок:*

Анимированная кнопка Torrow для записи
''''''''''''''''''''''''''''''''''''''

a. Используйте блок HTML-кода
b. Введите скрипт виджета (скрипт можно найти на странице: :ref:`Параметры полного виджета<widgetffinst>`)
c. Убедитесь, что параметр **show-widget-button = true** и в **url:** вставлена ссылка правильная ссылка
d. Готово!

Пример:
'''''''

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
         modal="right"
         modal-active="false"
         show-widget-button="true"
     ></torrow-widget>
     <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>
    

Существующая кнопка на сайте
'''''''''''''''''''''''''''''

*Реализация на примере работы с Tilda:*

a. Добавьте изображение кнопки
b. Задайте кнопке CSS Class Name (к примеру, send-appeal) 
c. Добавьте блок HTML-код
d. Введите скрипт (скрипт можно найти на странице: :ref:`Параметры полного виджета<widgetffinst>`)
e. Убедитесь, что show-widget-button = false и в url: вставлена ссылка
f. Добавьте следующий скрипт:

.. code-block:: 
    
    <script>
        var buttonCollection =  document.getElementsByClassName('send-appeal')
        if(buttonCollection.length) {
            buttonCollection[0].addEventListener('click', event => 
            {document.querySelector('#torrow-widget').setAttribute('modal-active', 'true')})
        }
    </script>

g. Готово!

Пример:
'''''''

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
         modal="right"
         modal-active="false"
         show-widget-button="false"
     ></torrow-widget>
     <script>
        var buttonCollection =  document.getElementsByClassName('send-appeal')
        if(buttonCollection.length) {
            buttonCollection[0].addEventListener('click', event => 
            {document.querySelector('#torrow-widget').setAttribute('modal-active', 'true')})
        }
     </script>     
     <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

.. note:: Обратите внимание, где указано название *CSS Class Name:* **send-appeal** внутри скрипта! В это место необходимо вводить заданное Вами значение.
    
.. raw:: html
   
   <torrow-widget
      id="torrow-widget"
      url="https://web.torrow.net/app/tabs/tab-search/service;id=103edf7f8c4affcce3a659502c23a?closeButtonHidden=true&tabBarHidden=true"
      modal="right"
      modal-active="false"
      show-widget-button="true"
      button-text="Заявка эксперту"
      modal-width="550px"
      button-style = "rectangle"
      button-size = "60"
      button-y = "top"
   ></torrow-widget>
   <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

.. raw:: html

   <!-- <script src="https://code.jivo.ru/widget/m8kFjF91Tn" async></script> -->