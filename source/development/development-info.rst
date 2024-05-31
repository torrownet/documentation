Для интеграторов и разработчиков
==================================

Вы можете интегрировать **Torrow** в свои приложения, используя API и WebHook.

Swagger
----------------------------

Swagger вы можете найти по адресу: https://torrow.net/swagger/index.html

Пример кода с вызовом API
----------------------------

Пример кода с последовательностью вызовов на Kotlin: https://github.com/torrowtechnologies/torrow-api-kotlin-sample/blob/main/src/main/kotlin/Main.kt


WebHook
----------------------------

В настройках услуги вы можете указать WebHook на различные события, связанные с услугой (создание заказа, оплата и т.д.). 
Данная возможность позволяет интегрировать сервис Torrow с любыми внешними системами (AmoCrm, 1C и т.д.). 
Интеграция может быть еще проще, если использовать сервисы интеграции, например Albato_.

.. _Albato: https://albato.ru/.

#. Откройте услугу, для которой нужно настроить WebHook
#. Раскройте блок "Общие настройки"
#. Найдите поле "Интеграции" и нажите на него
#. Включите WebHook и задайте его параметры.

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