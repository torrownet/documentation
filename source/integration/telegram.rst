.. _telegram-label:

=========================================================
Интеграция с Telegram
=========================================================

    .. |галка| image:: media/galka.png
        :width: 21
        :alt: alternative text

Можно настроить Telegram бота для отправки уведомлений Менеджерам услуги или Исполнителям о записях клиентов.

.. note:: 
    Обращаем внимание, что нельзя настроить Telegram бота для уведомлений клиентам, так как в отличие от WhatsApp сервис Telegram не позволяет отправлять уведомления на номер телефона.
    

Настройка Telegram для группы
----------------------------

1. Создайте Telegram группу

2. Добавьте в Telegram группу бота @TorrowBot

3. Запустите бота командой /start

4. Вы увидите сообщение: "Ваш идентификатор для настройки уведомлений Torrow: -123456789" (*отрицательное число*)

5. Скопируйте идентификатор (*отрицательное число*) в поле Telegram для Контакта, который добавляется как исполнитель заказов.

Если у вас будут вопросы по настройке, то напишите в `техническую поддержку Torrow`_.

.. _`техническую поддержку Torrow`: https://t.me/TorrowSupport


Настройка Telegram для личных уведомлений
-------------------------------------------

1. Найдите в поиске Telegram бота @TorrowBot и откройте переписку с ним

2. Запустите бота командой /start

3. Вы увидите сообщение: "Ваш идентификатор для настройки уведомлений Torrow: 123456789" (*положительное число*)

4. Скопируйте идентификатор (*положительное число*) в поле Telegram для вашей Визитной карточки, которая добавляется как исполнитель заказов.

Если у вас будут вопросы по настройке, то напишите в `техническую поддержку Torrow`_.

.. _`техническую поддержку Torrow`: https://t.me/TorrowSupport

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