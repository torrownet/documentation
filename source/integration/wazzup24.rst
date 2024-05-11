.. _wazzup24-label:

=========================================================
Интеграция с WhatsApp через Wazzup24 сервис
=========================================================

    .. |галка| image:: media/galka.png
        :width: 21
        :alt: alternative text

Наш сервис интегрирован с WhatsApp для отправки уведомлений через сервис Wazzup24. Чтобы настроить рассылку уведомлений вашим клиентам через WhatsApp используйте инструкцию по подключению описанную ниже.

.. note:: При выборе данного способа отправки уведомлений вашим клиентам учитывайте следующую информацию:
.. note:: 1. Отправка уведомлений WhatsApp платная, так же как и отправка Sms.
.. note:: 2. В отличие от Sms-уведомлений, в WhatsApp вы платите за 1 день переписки с вашим клиентом, а не за отдельные сообщения.
.. note:: 3. Не все ваши клиенты имеют WhatsApp, поэтому не все клиенты будут получать уведомления. По статистике, примерно 15% клиентов не имеют WhatsApp.

4. Перейдите в Ваш аккаунт amoCRM и вызовите **Инструменты разработчика** с помощью клавиши **F12**.

.. figure:: media/amoCRM/amo0.png
    :scale: 40 %
    :alt: alternative text
    :align: center
    
    Инструменты разработчика

----------------------------------

2. В появившемся окне выберите **Console** (консоль).

.. figure:: media/amoCRM/amo1.png
    :scale: 60 %
    :alt: alternative text
    :align: center

----------------------------------

3. В строку консоли втавьте код, указанный ниже и нажмите **Enter**.

.. code-block::

    "https://hooks.torrow.net/webhook/amocrm?subdomain="+AMOCRM.widgets.system.subdomain+"&userHash="+AMOCRM.widgets.system.amohash+"&userLogin="+AMOCRM.widgets.system.amouser

.. figure:: media/amoCRM/amo2.png
    :scale: 60 %
    :alt: alternative text
    :align: center

----------------------------------

4. Вы получите ссылку (вебхук), которая нужна для передачи информации о заказах из приложения. Скопируйте данную ссылку.

.. figure:: media/amoCRM/amo3.png
    :scale: 60 %
    :alt: alternative text
    :align: center

----------------------------------

5. Зайдите в свою услугу в приложении torrow и перейдите к **Общим настройкам**. Нажмите на поле **Интеграции**.

.. figure:: media/amoCRM/amo4.png
    :scale: 42 %
    :alt: alternative text
    :align: center

----------------------------------


6. Из списка интеграций выберите **Webhook**.

.. figure:: media/amoCRM/amo5.png
    :scale: 42 %
    :alt: alternative text
    :align: center

------------------------------------

7. Вставьте полученную ссылку в поле URL. Укажите название для интеграции и выберите действия, при которых будет отправляться webhook. После всех действий сохраните изменения, нажав на |галка|.

.. hint:: Рекомендуем по умолчанию выбрать: Создание заказа (CreateOrder), Заказ отменен (CaseCancel), Обновление заказа/события (UpdateCase), Изменено время заказа/события (UpdateCaseTime).

.. figure:: media/amoCRM/amo6.png
    :scale: 42 %
    :alt: alternative text
    :align: center

----------------------------------

8. После сохранения новая интеграция будет выглядеть, как на скриншоте ниже, а также получит состояние **Активный**.

.. figure:: media/amoCRM/amo7.png
    :scale: 42 %
    :alt: alternative text
    :align: center

----------------------------------


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
   <script src="https://cdn.jsdelivr.net/gh/torrowtechnologies/torrow-widget@1/dist/torrow-widget.min.js" defer></script>

.. raw:: html

   <script src="https://code.jivo.ru/widget/m8kFjF91Tn" async></script>