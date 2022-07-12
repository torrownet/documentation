.. _widgetEFsetUp:

Установка упрощенного виджета онлайн-записи
-------------------------------------------

Для работы с упрощенным виджетом онлайн-записи Вам понадобится ссылка на виджет. Для её получения произведите следующие действия:

     1. Откройте контекстное меню своей услуги
     2. Нажмите "Поделиться"
     3. Выберите пункт "Виджет онлайн-записи"
     4. Готово! Ссылка скопирована в буфер обмена

.. _widgetEFSetUp-1-1:

Виджет встроен в сайт
~~~~~~~~~~~~~~~~~~~~~

Как это выглядит можно узнать :ref:`тут.<widget-ef-exmpl-2>`

     #. Используйте тег <iframe>
     #. Внутри тега введите параметры желаемого размера виджета. К примеру, используйте атрибуты height и width (стандартно height = 500px)
     #. Добавьте атрибут src, значением которого будет ссылка на Вашу услугу
 
     *Для более подробной работы с iframe обратитесь к руководству по html.*


.. _widget-hint:
.. hint:: Также Вы можете добавить дополнительные параметры в ссылке виджета, чтобы отображать кличество доступных слотов времени или всегда использовать определенный язык отображения виджета. Для этого воспользуйтесь инструкцией ниже.

Если Вы хотите, чтобы виджет всегда отображался на русском языке, необходимо добавить параметр **culture=ru-RU**. Тогда ссылка будет выглядеть следующим образом:
https://embed.torrow.net/service/103ede4333d697c6a1c059057ca52/booking?culture=ru-RU

Чтобы отобразить виджет на английском языке используйте параметр **culture=en-EN**.

------------------------------

Если Вы хотите, чтобы рядом со слотом выбора времени находилось доступное количество мест для записи, то необходимо добавить в ссылку параметр **showAvailableSlots=true**.

Пример ссылки:

https://embed.torrow.net/service/103ede4333d697c6a1c059057ca52/booking?culture=ru-RU&showAvailableSlots=true

-----------------------------------------------

Пример кода встроенного виджета:
'''''''''''''''''''''''''''''''

.. code-block::

    <iframe> 
    height="500px" src="https://embed.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking?culture=ru-RU&showAvailableSlots=true"
    </iframe>

--------------------------------

Виджет открывается по кнопке
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Виджет может открываться 2 видами кнопок:*

Как это выглядит можно узнать :ref:`тут.<widget-ef-exmpl-3>`

.. _widgetEFSetUp-2-1:

Анимированная кнопка Torrow для записи
'''''''''''''''''''''''''''''''''''''''

a. Используйте блок HTML-кода
b. Введите скрипт виджета
c. Убедитесь, что параметр **show-widget-button = true** и в **url:** вставлена ссылка правильная ссылка
d. Готово!

.. hint:: Также Вы можете добавить дополнительные параметры в ссылке виджета, чтобы отображать кличество доступных слотов времени или всегда использовать определенный язык отображения виджета. Для этого воспользуйтесь :ref:`инструкцией <widget-hint>`.

Пример:
'''''''

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://embed.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
         modal="right"
         modal-active="false"
         show-widget-button="true"
     ></torrow-widget>
     <script src="https://cdn.jsdelivr.net/gh/torrowtechnologies/torrow-widget@1/dist/torrow-widget.min.js" defer></script>
    

Для более подробной настройки страницы :ref:`нажмите сюда.<widgetEFinst>`

-----------------------------------------------

.. _widgetEFSetUp-2-2:

Существующая кнопка на сайте
'''''''''''''''''''''''''''''

*Реализация на примере работы с Tilda:*

a. Добавьте изображение кнопки
b. Задайте кнопке CSS Class Name (к примеру, send-appeal) 
c. Добавьте блок HTML-код
d. Введите скрипт (Как из :ref:`примера установки стандартной кнопки <widgetEFSetUp-2-1>`)
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

.. hint:: Также Вы можете добавить дополнительные параметры в ссылке виджета, чтобы отображать кличество доступных слотов времени или всегда использовать определенный язык отображения виджета. Для этого воспользуйтесь :ref:`инструкцией <widget-hint>`.

Пример:
'''''''

.. code-block:: 

     <torrow-widget
         id="torrow-widget"
         url="https://embed.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking"
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
     <script src="https://cdn.jsdelivr.net/gh/torrowtechnologies/torrow-widget@1/dist/torrow-widget.min.js" defer></script>

Для более подробной настройки :ref:`обратитесь к параметрам виджета.<widgetEFinst>`

.. note:: Обратите внимание, где указано название *CSS Class Name:* **send-appeal** внутри скрипта! В это место необходимо вводить заданное Вами значение.
