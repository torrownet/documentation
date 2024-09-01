.. _metrika-label:

===================================================
Как подключить Яндекс.Метрику или Google Analytics?
===================================================

.. note:: Инструменты аналитики позволят Вам получать статистику посещений, использований услуг и других элементов приложения клиентами.

--------------------------
Подключение Яндекс.Метрики
--------------------------

1. Зайдите в свой аккаунт Яндекс.Метрики и добавьте новый счетчик.

.. figure:: media/metrika/yandex-m1.png
    :scale: 60 %
    :alt: alternate text
    :align: center

----------------------------

2. Задайте **имя счетчика**, укажите **ссылку** на элемент torrow и почту, куда будут приходить уведомления от сервиса.

.. figure:: media/metrika/yandex-m2.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

3. Нажмите на кнопку **Создать счетчик**.

.. figure:: media/metrika/yandex-m3.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

4. Выберите способ **CMS и конструкторы сайтов**. Для сбора аналитики Вам понадобится номер счетчика, скопируйте его.

.. figure:: media/metrika/yandex-m4.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

5. Нажмите на кнопку **Начать пользоваться**.

.. figure:: media/metrika/yandex-m5.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

6. Счетчик создан. Теперь перейдите в элемент torrow, где планируется сбор статистики.

.. figure:: media/metrika/yandex-m6.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

7. Перейдите в **Общие настройки**, выберите **Инструменты аналитики**.

.. figure:: media/metrika/yandex-m7.png
    :scale: 53 %
    :alt: alternate text
    :align: center

----------------------------

8. Укажите номер счетчика в поле Код Яндекс.Метрики. Сохраните измнения, нажав на |галка|.

    .. |галка| image:: media/galka.png
        :scale: 42 %

.. figure:: media/metrika/yandex-m8.png
    :scale: 53 %
    :alt: alternate text
    :align: center

----------------------------

При необходимости можно добавиль Цель - Создание заказа

1. Для этого откройте вкладку **Цели** в Яндекс.Метрике.
   
.. figure:: media/metrika/goal1.png
    :scale: 60 %
    :alt: alternate text
    :align: center

----------------------------

2. Нажмите на кнопку **Добавить цель**.

.. figure:: media/metrika/goal2.png
    :scale: 60 %
    :alt: alternate text
    :align: center

----------------------------

3. Впишите название Цели, выберите **Тип условия** - **JavaScript-событие** и впишите **Индентификатор цели** - **orderCreated**.

.. figure:: media/metrika/goal3.png
    :scale: 60 %
    :alt: alternate text
    :align: center

----------------------------

Нажмите на **Добавить цель** - Цель начнет работать **автоматически**.


----------------------------
Подключение Google Analytics
----------------------------

1. Зайдите в свой аккаунт Google Аналитики или создайте новый, как изображено ниже.

.. figure:: media/metrika/google-m1.png
    :scale: 70 %
    :alt: alternate text
    :align: center

----------------------------

2. Задайте **Название аккаунта**, нажмите **След.**

.. figure:: media/metrika/google-m2.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

3. Укажите название для ресурса, оно никак не связано с ресурсом torrow. Выберите нужный часовой пояс и валюту. Нажмите **Далее**.

.. figure:: media/metrika/google-m3.png
    :scale: 45 %
    :alt: alternate text
    :align: center

----------------------------

4. Внесите информацию о своей компании и нажмите **Создать**.

.. figure:: media/metrika/google-m4.png
    :scale: 50 %
    :alt: alternate text
    :align: center

----------------------------

5. Укажите настройки электронной рассылки по своему усмотрению. **Сохраните** изменения.

.. figure:: media/metrika/google-m5.png
    :scale: 45 %
    :alt: alternate text
    :align: center

----------------------------

6. Выберите платформу **Веб**.

.. figure:: media/metrika/google-m6.png
    :scale: 40 %
    :alt: alternate text
    :align: center

----------------------------

7. Выберите URL веб-сайта **https://** и укажите ссылку на элемент (например, услугу) в поле отмеченное стрелкой. Впишите название для потока. Сохрание изменения по кнопке **Создать поток**.

.. hint:: При вводе ссылки **https://** указывать не надо, так как он прописан в поле слева.

.. figure:: media/metrika/google-m7.png
    :scale: 40 %
    :alt: alternate text
    :align: center

----------------------------

8. Ваш поток готов. Скопируйте **идентификатор потока данных**.

.. figure:: media/metrika/google-m8.png
    :scale: 40 %
    :alt: alternate text
    :align: center

----------------------------

9.  Перейдите в **Общие настройки**, выберите **Инструменты аналитики**.

.. figure:: media/metrika/yandex-m7.png
    :scale: 53 %
    :alt: alternate text
    :align: center

----------------------------

10. Укажите номер счетчика в поле **Идентификатор Google аналитики**. Сохраните измнения, нажав на |галка|.

    .. |галка| image:: media/galka.png
        :scale: 42 %

.. figure:: media/metrika/google-m9.png
    :scale: 53 %
    :alt: alternate text
    :align: center


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