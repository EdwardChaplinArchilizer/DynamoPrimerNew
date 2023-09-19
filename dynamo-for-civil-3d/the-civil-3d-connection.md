# Совместимость с Civil 3D

<figure><img src="../.gitbook/assets/DynamoSwissKnife-WhiteBackground_edit (2).jpg" alt="" width="563"><figcaption></figcaption></figure>

Dynamo for Civil 3D предоставляет возможности _визуального программирования_ инженерам и проектировщикам, работающим над инфраструктурными проектами. Dynamo можно рассматривать как цифровой швейцарский нож для пользователей Civil 3D: какая бы задача перед вами ни стояла, вы сможете найти подходящее решение. Интуитивно понятный интерфейс позволяет создавать эффективные настраиваемые процедуры без написания кода. Для использования Dynamo не нужно быть _программистом_, но нужно уметь _думать_, как программист. В этой главе, а также в других главах этого руководства представлены сведения, которые помогут вам развить ваши логические навыки, что позволит вам применять принципы машинного проектирования для решения любых задач.

## История

Программа Dynamo была впервые представлена в составе Civil 3D 2020, и с тех пор ее возможности постоянно расширялись и улучшались. Изначально она устанавливалась отдельно как обновление программного обеспечения. Теперь же она поставляется в пакете со всеми версиями Civil 3D. В зависимости от используемой версии Civil 3D интерфейс Dynamo может немного отличаться от приведенных в этой главе примеров. Это связано с существенными изменениями в интерфейсе Civil 3D 2023.

<figure><img src="../.gitbook/assets/c3d-ui-old.png" alt=""><figcaption><p>Интерфейс Dynamo, Civil 3D 2020–2022</p></figcaption></figure>

<figure><img src="../.gitbook/assets/c3d-ui-new.png" alt=""><figcaption><p>Интерфейс Dynamo, Civil 3D 2023 – настоящее время</p></figcaption></figure>

Для получения наиболее актуальной информации о разработке Dynamo рекомендуем посетить [блог Dynamo](https://dynamobim.org/blog/). В таблице ниже приведены ключевые этапы развития Dynamo for Civil 3D. 

<table data-full-width="false"><thead><tr><th width="180">Версия Civil 3D</th><th width="161">Версия Dynamo</th><th>Примечания</th></tr></thead><tbody><tr><td>2024</td><td>2.17</td><td>Обновление интерфейса проигрывателя Dynamo</td></tr><tr><td>2023.2</td><td>2.15</td><td></td></tr><tr><td>2023</td><td>2.13</td><td>Обновление интерфейса Dynamo</td></tr><tr><td>2022.1</td><td>2.12</td><td><ul><li>Добавлены параметры хранения данных привязки объектов</li><li>Новые узлы для управления привязкой объектов</li></ul></td></tr><tr><td>2022</td><td>2.10</td><td><ul><li>Программа включена в основной пакет установки Civil 3D</li><li>Переход с IronPython на Python.NET</li></ul></td></tr><tr><td>2021</td><td>2.5</td><td></td></tr><tr><td>2020.2</td><td>2.4</td><td></td></tr><tr><td>2020 Update 2</td><td>2.4</td><td>Добавлены новые узлы</td></tr><tr><td>2020.1</td><td>2.2</td><td></td></tr><tr><td>2020</td><td>2.1</td><td>Исходная версия</td></tr></tbody></table>