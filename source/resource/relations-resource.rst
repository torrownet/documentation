.. _relationsresource-label:

===================================
Настройка связи с другими ресурсами
===================================

Чтобы настроить **Связь с другими ресурсами**:

1. Нажмите на поле **Тип связи** (по умолчанию **Нет**).

.. figure:: media/relations/relations1.png
    :scale: 42 %
    :alt: alternate text
    :align: center

2. Выберите необходимый параметр.

.. hint::

    * **Дополняет** — в таком случае, при выборе клиентом текущего настраиваемого ресурса на следующем шаге записи ему будет предложен ресурс (ресурсы), который будет выбран в **Список ресурсов**. Например, клиент находится в услуге **Заказ еды**, на шаге выбора блюда выбирает блины, следующим шагом ему предложат топинги - варенье, сметана (так как они дополняют ресурс - "блины"). При выборе блюда - мясо данных топингов уже не будет.
    * **Исключается** — в таком случае, при выборе клиентом текущего настраиваемого ресурса на следующем шаге записи ему будут предложены ресурсы, кроме тех, что выбраны в **Список ресурсов**. Например, клиент выбирает машины в среднем ценовом сегменте, тогда на шаге выбора самой машины для него будут исключены модели премиум и эконом сегмента.

.. figure:: media/relations/relations2.png
    :scale: 42 %
    :alt: alternate text
    :align: center

1. Нажмите на поле **Список ресурсов**. 

.. figure:: media/relations/relations3.png
    :scale: 42 %
    :alt: alternate text
    :align: center

4. Выберите необходимые ресурсы, которые хотите **исключить** или **дополнить**. Можно выбрать один и более ресурсов.

.. figure:: media/relations/relations4.png
    :scale: 42 %
    :alt: alternate text
    :align: center

5. Сохраните изменения, нажав на |галка|.

    .. |галка| image:: media/galka.png
        :scale: 42 %

.. figure:: media/relations/relations5.png
    :scale: 42 %
    :alt: alternate text
    :align: center

-----------------------------

.. note::

    * :ref:`timetableresource-label`
    * :ref:`priceresource-label`
    * :ref:`optionsresource-label`
    * :ref:`conditionsresource-label`

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