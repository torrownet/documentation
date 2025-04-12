.. _sample-whatsapp-webhook-label:

=======================================================
Примеры WebHook для WhatsApp (раздел в разработке)
=======================================================

В приложении Torrow как правило настраиваются следующие WebHook для отправки сообщений в WhatsApp:

#. Создание заказа:

    - https://hooks.torrow.net/webhook/green-api?apiUrl=1103.api.green-api.com&idInstance=<Идентификатор>&apiTokenInstance=<Токен>&repeat=true&errorEmail=<email>&hide_service_name=true&hide_contact_info=true&sign_text=Если%20у%20вас%20остались%20вопросы,%20вы%20можете%20задать%20их%20в%20этом%20чате%20или%20позвонить%20по%20телефону%20☎%20+7%20911%20999-99-99.&message=Ваша%20бронь%20подтверждена.%20Выполните%20оплату.

#. Изменение заказа:

    - https://hooks.torrow.net/webhook/green-api?apiUrl=1103.api.green-api.com&idInstance=<Идентификатор>&apiTokenInstance=<Токен>&repeat=true&errorEmail=<email>&hide_service_name=true&hide_contact_info=true&sign_text=Если%20у%20вас%20остались%20вопросы,%20вы%20можете%20задать%20их%20в%20этом%20чате%20или%20позвонить%20по%20телефону%20☎%20+7%20911%20999-99-99.&message=Ваша%20бронь%20изменена. 

#. Напоминание о заказе:

    - https://hooks.torrow.net/webhook/green-api?apiUrl=1103.api.green-api.com&idInstance=<Идентификатор>&apiTokenInstance=<Токен>&repeat=true&errorEmail=<email>&hide_service_name=true&hide_contact_info=true&hide_url=true&sign_text=%0A%20🗝%20Для%20получения%20ключа%20от%20кабинета,%20необходимо%20показать%20это%20сообщение%20на%20посту%20охраны%20и%20расписаться%20(документы%20не%20требуются).%20Пожалуйста,%20не%20забудьте%20на%20выходе%20вернуть%20ключ%20и%20обязательно%20расписаться%20в%20журнале.%0A%20%0A%20Если%20у%20вас%20остались%20вопросы,%20вы%20можете%20задать%20их%20в%20этом%20чате%20или%20позвонить%20по%20телефону%20☎%20+7%20911%20999-99-99.&message=Напоминаем%20о%20бронировании.

#. Отмена заказа:

    - https://hooks.torrow.net/webhook/green-api?apiUrl=1103.api.green-api.com&idInstance=<Идентификатор>&apiTokenInstance=<Токен>&repeat=true&errorEmail=<email>&hide_service_name=true&hide_contact_info=true&hide_url=true&sign_text=Если%20у%20вас%20остались%20вопросы,%20вы%20можете%20задать%20их%20в%20этом%20чате%20или%20позвонить%20по%20телефону%20☎%20+7%20911%20999-99-99.&message=Ваша%20бронь%20отменена.%0A%20📍%20Если%20вы%20не%20отменяли%20запись,%20то%20наиболее%20вероятные%20причины%20отмены:%0A%201.%20Сбой%20оплаты%20в%20процессе%20бронирования;%20%0A%202.%20Процесс%20бронирования%20не%20был%20завершен%20оплатой.%20%0A%20🔁%20В%20этих%20случаях%20повторите%20заказ%20заново.%20%0A%20Обратите%20внимание,%20что%20подтверждение%20бронирования%20приходит%20незамедлительно,%20но%20бронирование%20отменяется%20в%20случае%20не%20поступления%20денег%20на%20счет%20в%20течение%2015%20минут.%0A%20

#. Получена оплата:

    - https://hooks.torrow.net/webhook/green-api?apiUrl=1103.api.green-api.com&idInstance=<Идентификатор>&apiTokenInstance=<Токен>&repeat=true&errorEmail=<email>&hide_service_name=true&hide_contact_info=true&hide_url=true&sign_text=Если%20у%20вас%20остались%20вопросы,%20вы%20можете%20задать%20их%20в%20этом%20чате%20или%20позвонить%20по%20телефону%20☎%20+7%20911%20999-99-99.&message=Ваша%20оплата%20получена.%20Ждем%20вас.


Для настройки WebHook необходимо зайти в раздел **Настройки** и выбрать **WebHook**.

.. figure:: media/whatsapp_webhook/webhook1.png
    :scale: 60 %
    :alt: alternate text
    :align: center


.. hint:: Описание настроек WebHook можно посмотреть в разделах `Интеграция с WhatsApp через Green-Api сервис`_ и `Интеграция с WhatsApp через Wazzup24 сервис`_

.. _`Интеграция с WhatsApp через Green-Api сервис`: ../integration/green-api.rst

.. _`Интеграция с WhatsApp через Wazzup24 сервис`: ../integration/wazzup24.rst

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
