.. _widgeteasyform2:

Упрощенный виджет онлайн-записи
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: **Упрощенная версия виджета онлайн-записи** - обладает ограниченными функциями (только выбор времени и контактная информация). Используется только для услуг с типом: заявка, заказ на время, запись на время. Если У Вас другой тип услуги или присутствует выбор ресурса, воспользуйтесь :ref:`полным виджетом <widgetdifiform2>`.

.. toctree:: 
      :maxdepth: 1

      widget-EF-inst
      widget-EF-setUp

Примеры установок
`````````````````

.. _widget-ef-exmpl-1:

Виджет открывается по ссылке на отдельной странице
''''''''''''''''''''''''''''''''''''''''''''''''''

Пользователь получает ссылку и переходит по ней.

.. figure:: media/gif/widgetEasyLink.gif
      :scale: 60 %
      :align: center
      :alt: Альтернативный текст

_____________________________________________________________________

.. _widget-ef-exmpl-2:

Виджет встроен на страницу сайта
''''''''''''''''''''''''''''''''

Пользователь использует Ваш сайт и там находит виджет.

Инструкция по настройке: :ref:`перейти. <widgetEFSetUp-1-1>`

.. raw:: html

    <iframe src="https://embed.torrow.net/service/103edf7f8c4affcce3a659502c23a/booking" height="490px" width="150%"></iframe> 


_____________________________________________________________________

.. _widget-ef-exmpl-3:

Виджет открывается по кнопке 
''''''''''''''''''''''''''''

Пользователь прямо на сайте может вызвать окно с полным виджетом! 

Инструкция по настройке: :ref:`перейти. <widgetEFSetUp-2-1>`

.. figure:: media/gif/widgetEasyButton2.gif
      :scale: 50 %
      :align: center
      :alt: Альтернативный текст

.. figure:: media/gif/easy-new-button.gif
      :scale: 90 %
      :align: center
      :alt: Альтернативный текст

--------------------------------------------------

.. _widget-ff-exmpl-4:

Виджет открывается при нажатии по существующей кнопке на сайте
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

Инструкция по настройке: :ref:`перейти. <widgetEFSetUp-2-2>`

.. figure:: media/gif/widget_customButton.gif
      :width: 969
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
      button-style = "rectangle"
      button-size = "60"
      button-y = "top"
   ></torrow-widget>
   <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

.. raw:: html

   <!-- <script src="https://code.jivo.ru/widget/m8kFjF91Tn" async></script> -->