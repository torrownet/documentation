.. _resources-label:

======
Ресурс
======

.. note:: **Ресурсы Torrow** — элементы, которые формируют **услуги** (:ref:`service-label`). Имеют гибкую конструкцию, поэтому можно создать как товар на продажу, так и для сдачи в аренду.

Вы можете указать общее количество товара и дату инвентаризации для контроля остатка, указать скидку на товар в процентном соотношении, установить минимальный и максимальный срок аренды, количества товара и т.д.

.. hint:: Ресурсы делятся по типу использования на:
    
    * **Продажа** — использовать в случае, если товар количественно ограничен. Например: игрушки, кулинарные блюда, виды одежды и т.д.
    * **Аренда** — использовать в случае, если товар сдается в аренду на какой-то интервал времени. Например: аренда квартиры, аренда антиквариата, аренда рабочей силы и т.д.
    * **Спецификация** — использовать в случае, если товар количественно не ограничен. Например: стрижка, услуги тренера, занятие в музыкальной школе и т.д.
    * **Оплата** — использовать, если планируете подключить к услуге способ оплаты. Инструкция по настройке оплаты: :ref:`payment-label`.

.. toctree::
    :maxdepth: 1
    :caption: Создание ресурса типа

    sale
    rent-resource
    specification
    payment-resource


.. toctree::
    :caption: Настройка ресурса

    timetable-resource
    price-resource
    options-resource
    conditions-resource
    relations-resource

.. .. raw:: html
   
..    <torrow-widget
..       id="torrow-widget"
..       url="https://web.torrow.net/app/tabs/tab-search/service;id=103edf7f8c4affcce3a659502c23a?closeButtonHidden=true&tabBarHidden=true"
..       modal="right"
..       modal-active="false"
..       show-widget-button="true"
..       button-text="Заявка эксперту"
..       modal-width="550px"
..       button-style = "rectangle"
..       button-size = "60"
..       button-y = "top"
..    ></torrow-widget>
..    <script src="https://cdn.jsdelivr.net/gh/torrowtechnologies/torrow-widget@1/dist/torrow-widget.min.js" defer></script>

.. .. raw:: html

..    <script src="https://code.jivo.ru/widget/m8kFjF91Tn" async></script>