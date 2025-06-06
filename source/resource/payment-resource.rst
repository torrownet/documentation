.. _paymentresource-label:

======
Оплата
======

Чтобы создать ресурс с типом **Оплата**:

1. Перейдите в элемент или раздел, где хотите создать ресурс. Нажмите на |плюс|.

    .. |плюс| image:: media/plus.png
        :scale: 42 %


.. figure:: media/payment_resource/payment1.png
    :scale: 42 %
    :alt: alternate text
    :align: center

2. Нажмите на кнопку |массив|.

    .. |массив| image:: media/reserved.png
        :scale: 42 %

.. figure:: media/payment_resource/payment2.png
    :scale: 42 %
    :alt: alternate text
    :align: center

3. Выбираем **Создать ресурс**.

.. figure:: media/payment_resource/payment3.png
    :scale: 42 %
    :alt: alternate text
    :align: center

4. Нажмите на поле **Тип использования** и выберите из списка **Оплата**.

.. figure:: media/payment_resource/payment4.png
    :scale: 42 %
    :alt: alternate text
    :align: center

5. Нажмите на поле **Тип оплаты**.

.. figure:: media/payment_resource/payment5.png
    :scale: 42 %
    :alt: alternate text
    :align: center

6. Выберите необходимый тип оплаты из списка. При необходимости введите **Комментарий к оплате**.

.. figure:: media/payment_resource/payment6.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

--------
Наличные
--------

.. figure:: media/payment_resource/payment7.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

----------------
Платежная ссылка
----------------

.. hint:: Платежная ссылка — уникальная ссылка, содержащая счет на оплату, по которой покупатель переходит на платежную систему. Ссылка генерируется в вашем интернет-эквайринге один раз с заранее заданной суммой.

.. figure:: media/payment_resource/payment8.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

----------
|Лого Robokassa| |Название Robokassa|
----------

.. |Лого Robokassa| image:: media/payment_resource/robokassa_logo.png
    :width: 40
    :alt: Настройка Torrow Service Bot для Telegram

.. |Название Robokassa| image:: media/payment_resource/robokassa_name.png
    :width: 100
    :alt: Настройка Torrow Service Bot для Telegram

Чтобы получить дополнительную техническую поддержку подключения платежей, зарегистрируйтесь в Robokassa по ссылке https://partner.robokassa.ru/Reg/Register?PromoCode=torrow&culture=ru

.. hint:: ID магазина и пароли настраиваются в личном кабинете `robokassa.com`_.
    
    .. _`robokassa.com`: https://partner.robokassa.ru/Reg/Register?PromoCode=torrow&culture=ru

.. figure:: media/payment_resource/Robokassa.png
    :scale: 42 %
    :alt: Robokassa resource screenshot
    :align: center

------------------------------------

----------
PayKeeper
----------

.. hint:: Ссылка, имя пользователя и пароль настраиваются в личном кабинете `paykeeper.ru`_.
    
    .. _`paykeeper.ru`: https://paykeeper.ru/

.. figure:: media/payment_resource/Paykeeper.png
    :scale: 42 %
    :alt: Paykeeper resource screenshot
    :align: center

------------------------------------

----------
Prodamus
----------

.. hint:: Ссылка на оплату и секретный ключ получают из личного кабинета `prodamus.ru`_. Сервис позволяет принимать платежи из-за границы.
    
    .. _`prodamus.ru`: https://prodamus.ru/

.. figure:: media/payment_resource/Paykeeper.png
    :scale: 42 %
    :alt: Paykeeper resource screenshot
    :align: center

------------------------------------

------------------
Т-Банк (Тинькофф)
------------------

.. hint:: ИД терминала и пароль настраиваются в личном кабинете `t-bank.ru`_.
    
    .. _`t-bank.ru`: https://t-bank.ru/

.. figure:: media/payment_resource/Tinkoff.png
    :scale: 42 %
    :alt: T-Bank resource screenshot
    :align: center

------------------------------------

------------------
АльфаБанк
------------------

.. hint:: Имя пользователя и пароль настраиваются в личном кабинете `alfabank.ru`_.
    
    .. _`alfabank.ru`: https://alfabank.ru/

.. figure:: media/payment_resource/Alfabank.png
    :scale: 42 %
    :alt: Alfabank resource screenshot
    :align: center

------------------------------------

------------------
Уралсиб
------------------

.. hint:: Имя пользователя и пароль настраиваются в личном кабинете `uralsib.ru`_.
    
    .. _`uralsib.ru`: https://uralsib.ru/

.. figure:: media/payment_resource/Uralsib.png
    :scale: 42 %
    :alt: Uralsib resource screenshot
    :align: center

------------------------------------

--------
Сбербанк
--------

.. hint:: Имя пользователя и пароль предоставляется при подключении эквайринга Сбербанка.

.. figure:: media/payment_resource/payment10.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

------
ЮMoney
------

.. hint:: Номер счета ЮMoney, состоящий из 16 цифр, можно посмотреть в личном кабинете на `yoomoney.ru`_.
    
    .. _`yoomoney.ru`: https://yoomoney.ru/

.. figure:: media/payment_resource/payment11.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

------
ЮKassa
------

.. hint:: Идентификатор магазина и секретный ключ предоставляется при подключении в `yookassa.ru`_.

    .. _`yookassa.ru`: https://yookassa.ru/

.. figure:: media/payment_resource/payment12.png
    :scale: 42 %
    :alt: alternate text
    :align: center

------------------------------------

----
Qiwi
----

.. hint:: Для работы API потребуются публичный и секретный ключи. Ключи создаются в личном кабинете на `p2p.qiwi.com`_.

    .. _`p2p.qiwi.com`: https://p2p.qiwi.com/

.. figure:: media/payment_resource/payment9.png
    :scale: 42 %
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