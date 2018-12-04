---
title: Working with the editing form in the visual editor
sidebar: flexberry-designer_sidebar
keywords: Flexberry Designer, структура приложения, editform
summary: Принципы создания и редактирования editform
toc: true
permalink: en/fd_ember-editform-constructor.html
lang: en
---

Создание формы редактирования доступно через редактирование [Структуры приложения](fd_structure_all_forms.html) по кнопке «Добавить форму редактирования». 
 
На вкладках справа можно редактировать свойства формы и элементов управления, которые на ней размещаются.

## Свойства формы

Данная вкладка позволяет настроить общие параметры формы редактирования.
 
## Элементы управления

На данной вкаладке расположены кнопки, с помощью которых можно добавлять элементы управления на форму, а также перемещать и удалять их. Помимо элементов управления, которые будут связаны с атрибутами класса данных, на форму редактирования можно добавлять группы и вкладки.

Добавить новый элемент можно одним из двух способов:

* _добавить новое свойство_ – в этом случае в классе данных и представлении, связанными с данной списковой формой, автоматически добавляется новый атрибут;
* _выбрать из существующих свойств_ – в этом случае новый атрибут в классе данных не создается, добавляется лишь один из существующих атрибутов класса в представление, связанное со списковой формой.
 
Если при добавлении нового элемента управления будет выделена какая-либо группа или вкладка, новый элемент автоматически добавится внутрь выделенного элемента (вкладки или группы). Если будет выделен элемент управления, новый элемент управления добавится рядом с ним (в одну строку).
 
Один элемент можно поместить в другой (или, в случае полей – поменять место в строке или между строками) с помощью кнопки «Переместить». Для перемещения следует выделить перемещаемый элемент, затем кликнуть по кнопке «Переместить» и щелкнуть по нужному месту на форме. Также можно менять порядок расположения элементов перетаскивая их мышью. Порядок расположения находящихся рядом элементов управления можно менять кнопками-указателями вправо-влево.
 
Если выделить вкладку или группу и нажать кнопку «Удалить», то будут удалены как вкладка/группа, так и все элементы управления, находящиеся в них. Вкладки и группы могут также добавляться друг в друга произвольным образом.