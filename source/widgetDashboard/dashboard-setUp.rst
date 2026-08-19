.. _dashboard-widget-setup:

Установка виджета расписания
============================

Параметры ссылки и готовые шаблоны — на странице :ref:`Виджет расписания<dashboard-widget-label>`. Как получить **id** ресурса или услуги — там же.

Виджет встроен на страницу (iframe)
-----------------------------------

Подходит для постоянного экрана на сайте или TV.

#. Используйте тег ``<iframe>``.
#. Задайте ``height`` и ``width`` под ваш макет.
#. В ``src`` укажите ссылку на виджет расписания с ``sideMenuHidden=true`` и ``tabBarHidden=true``.

Пример:

.. code-block:: html

   <iframe height="700px" width="100%" src="https://torrow.net/app/tabs/tab-search/dashboard;id={resourceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true"></iframe>

Виджет открывается по кнопке Torrow
-----------------------------------

На сайте можно показать кнопку; по нажатию откроется модальное окно с расписанием.

#. Добавьте на страницу HTML-блок со скриптом ниже.
#. В ``url`` подставьте свою ссылку на виджет расписания (не ссылку на услугу для записи).
#. Убедитесь, что ``show-widget-button="true"``.

Пример:

.. code-block:: html

   <torrow-widget
      id="torrow-widget"
      url="https://torrow.net/app/tabs/tab-search/dashboard;id={resourceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true"
      modal="right"
      modal-active="false"
      show-widget-button="true"
      button-text="Расписание"
      modal-width="550px"
      button-style = "rectangle"
      button-size = "60"
      button-y = "top"
   ></torrow-widget>
   <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

Виджет открывается по своей кнопке на сайте
-------------------------------------------

#. Добавьте на сайт кнопку или элемент с CSS-классом, например ``send-appeal``.
#. Вставьте виджет с ``show-widget-button="false"`` и той же ссылкой на расписание в ``url``.
#. Подключите скрипт, который по клику на вашу кнопку открывает модальное окно.

Пример:

.. code-block:: html

   <torrow-widget
      id="torrow-widget"
      url="https://torrow.net/app/tabs/tab-search/dashboard;id={resourceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true"
      modal="right"
      modal-active="false"
      show-widget-button="false"
      modal-width="550px"
   ></torrow-widget>
   <script>
       var buttonCollection =  document.getElementsByClassName('send-appeal')
       if(buttonCollection.length) {
           buttonCollection[0].addEventListener('click', event =>
           {document.querySelector('#torrow-widget').setAttribute('modal-active', 'true')})
       }
   </script>
   <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

.. note:: В скрипте замените ``send-appeal`` на CSS-класс вашей кнопки.

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
