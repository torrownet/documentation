.. _amocrm-label:

===================
Интеграция с AmoCRM
===================

    .. |галка| image:: media/galka.png
        :width: 21
        :alt: alternative text

.. note:: Раздел находится в разработке.

Наш сервис интегрирован с CRM системой amoCRM. Чтобы передавать информацию о заказах из приложения или виджета, используйте инструкцию по подключению описанную ниже.

1. Перейдите в Ваш аккаунт amoCRM и вызовите **Инструменты разработчика** с помощью клавиши **F12**.

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
   <script src="https://cdn-public.torrow.net/widget/torrow-widget.min.js" defer></script>

.. raw:: html

   <script src="https://code.jivo.ru/widget/m8kFjF91Tn" async></script>