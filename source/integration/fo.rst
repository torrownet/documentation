.. _fo-label:

========================================================
Виджет онлайн-записи/-аренды в конструкторе сайтов fo.ru
========================================================

    .. |галка| image:: media/galka.png
        :width: 21
        :alt: alternative text

.. note:: Раздел находится в разработке.

Виджет Torrow интегрирован в `конструктор сайтов fo.ru <https://fo.ru/>`_.

.. figure:: media/fo/fo0.png
    :scale: 35 %
    :alt: alternative text
    :align: center
    
    Пример виджета на конструкторе fo

Чтобы установить виджет на сайт **FO**:

1. Перейдите в раздел **Мои сайты**.

.. figure:: media/fo/fo1.png
    :scale: 45 %
    :alt: alternative text
    :align: center

2. Выберите уже начатый проект в режиме **редактирования** или же создайте **новый сайт**.

.. figure:: media/fo/fo2.png
    :scale: 45 %
    :alt: alternative text
    :align: center

3. В режиме редактирования нажмите на блок **Элементы**.

.. figure:: media/fo/fo3.png
    :scale: 60 %
    :alt: alternative text
    :align: center

4. В появившемся меню выберите **Torrow Запись**. Если Вы не нашли данный элемент, воспользуйтесь **Поиском**.

.. figure:: media/fo/fo4.png
    :scale: 50 %
    :alt: alternative text
    :align: center

5. Введите свой номер телефона или почту, к которому(ой) привязан существующий аккаунт torrow или пройдите процедуру регистрации.

.. hint:: Регистрация осуществляется тем же способом, что и авторизация. Вам необходимо просто ввести свой номер телефона или e-mail.

.. figure:: media/fo/fo5.png
    :scale: 52 %
    :alt: alternative text
    :align: center

6. Выберите услугу, которую хотите подключить к виджету из списка. Если Вы еще не создали её, нажмите **Создать услугу по шаблону**.

.. figure:: media/fo/fo6.png
    :scale: 52 %
    :alt: alternative text
    :align: center

7. Выберите подходящий шаблон из списка.

.. figure:: media/fo/fo7.png
    :scale: 52 %
    :alt: alternative text
    :align: center

8. Ознакомьтесь с информацией о шаблоне, в случае необходимости посмотрите **пример**, расположенный ниже. Если он соответствует Вашему процессу или приближен к нему, нажмите **Создать услугу по шаблону**.

.. figure:: media/fo/fo8.png
    :scale: 52 %
    :alt: alternative text
    :align: center

9. После загрузки шаблона Вы получите следующее сообщение. Нажмите на кнопку **Открыть**.

.. figure:: media/fo/fo9.png
    :scale: 52 %
    :alt: alternative text
    :align: center

10. Вы попадете к персоначальному списку. Теперь выберите необходимую услугу.

.. figure:: media/fo/fo10.png
    :scale: 52 %
    :alt: alternative text
    :align: center

11. Нажмите на кнопку **Выбрать**.

.. figure:: media/fo/fo11.png
    :scale: 52 %
    :alt: alternative text
    :align: center

12. Вы получите следующее собщение.

.. figure:: media/fo/fo12.png
    :scale: 52 %
    :alt: alternative text
    :align: center

13. Готово! Виджет установлен на Ваш сайт. Осталось опубликовать его.

.. figure:: media/fo/fo13.png
    :scale: 40 %
    :alt: alternative text
    :align: center

.. hint:: После установки виджета на сайт рекомендуем настроить Вашу услугу. Если Вы использовали готовый шаблон, то можете воспользоваться :ref:`инструкцией<template-label>`.

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