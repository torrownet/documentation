.. _widgetdifiform2:

Полный виджет онлайн-записи
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: **Полная версия виджета онлайн-записи** - обладает всеми функциями записи и оплаты. 

.. toctree::
     :maxdepth: 1

     widget-FF-inst
     widget-FF-setUp

Примеры установок
`````````````````````
.. _widget-ff-exmpl-1:

Виджет открывается по ссылке на отдельной странице
''''''''''''''''''''''''''''''''''''''''''''''''''

Пользователь получает ссылку на полный виджет и переходит по ней. 

.. figure:: media/gif/newgif.gif
    :scale: 60 %
    :alt: alternative text
    :align: center

---------------------------------------------

.. _widget-ff-exmpl-2:

Виджет встроен на страницу сайта
''''''''''''''''''''''''''''''''

Пользователь прямо на сайте пользуется виджетом! 

.. raw:: html

    <h1 align="center">
    <iframe src="https://web.torrow.net/app/tabs/tab-announcement/service;id=1b5903ed42bf48b428fb31ee7af27866" height="600px" width="70%" align="center"></iframe> 
    </h1>

-------------------------------------------

.. _widget-ff-exmpl-3:

Виджет открывается при нажатии по анимированной кнопке
''''''''''''''''''''''''''''''''''''''''''''''''''''''
   
Пользователь прямо на сайте может вызвать окно с полным виджетом! 

.. figure:: media/gif/WidgetFullButton2.gif
      :scale: 54 %
      :align: center
      :alt: Альтернативный текст

      Круглый вариант отображения кнопки

.. figure:: media/gif/new-button.gif
      :scale: 80 %
      :align: center
      :alt: Альтернативный текст

      Прямоугольный вариант отображения кнопки

------------------------------------

.. _widget-ff-exmpl-4:

Виджет открывается при нажатии по существующей кнопке на сайте
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. figure:: media/gif/widget-ff-customButton.gif
      :scale: 35 %
      :align: center
      :alt: Альтернативный текст

.. raw:: html
   
   <torrow-widget
      id="torrow-widget"
      url="https://web.torrow.net/app/tabs/tab-search/service;id=103edf7f8c4affcce3a659502c23a?closeButtonHidden=true&tabBarHidden=true"
      modal="right"
      modal-active="false"
      show-widget-button="true"
      button-text="Заявка эксперту"
      modal-width="550px"
   ></torrow-widget>
   <script src="https://cdn.jsdelivr.net/gh/torrowtechnologies/torrow-widget@1/dist/torrow-widget.min.js" defer></script>