====================
Интеграция c Тильдой
====================


**Инструкция по подключению виджета Torrow к сайту на Tilda**

Вы можете за считанные минуты подключить виджет Torrow к своему сайту на Tilda.
Виджет позволяет напрямую с сайта получать заявки и заказы на ваши услуги.

1. Зарегистрируйтесь в Torrow

------------------------------

2. Создайте свою услугу (:ref:`service-label`)

   .. |точка| image:: media/Menu24.png
      :width: 21
      :alt: alternative text

------------------------------

3. На странице созданной услуги откройте меню, нажав на иконку |точка|. В меню выберите пункт "Поделиться"

.. figure:: media/1.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

4. Нажмите кнопку "Поделиться" и выберите пункт "Виджет онлайн-записи"

.. figure:: media/2.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

5. Будет скопирован Embed код следующего вида
   
.. figure:: media/3-1.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

6. Который необходимо разместить внутри тега <Iframe> </frame>

.. figure:: media/3-2.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

7. При необходимости можно задать размер
   
.. figure:: media/4.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

8. На сайте в нужное место добавьте блок T123 или T868

.. figure:: media/5.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

1. В добавленном блоке нажмите кнопку "Контент"
    
.. figure:: media/6.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

10. Ранее скопированный код виджета вставьте в соответствующее поле. После чего сохраните и опубликуйте страницу

.. figure:: media/7.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

Поздравляем! Форма онлайн-записи подключена и готова к работе.

.. figure:: media/8.png
   :scale: 25 %
   :alt: alternate text
   :align: center

------------------------------

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