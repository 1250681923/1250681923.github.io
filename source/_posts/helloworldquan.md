---
title: Markdown容器使用手册
date: 2021-12-07 17:38:01
categories:
    - Web development
tags:
    - Markdown
    - Container
    - 容器
cover: /img/bordeaux.jpg
---
<p style="text-align:center">
<img src="https://user-images.githubusercontent.com/59725125/145532785-07737376-50bd-4754-8fe2-2aa7ed01fa4d.jpg"  height="330" width="495">
</p>
此图摄于法国国庆节当日晚，波尔多水镜广场烟花晚会时。

It's my first time to build a personal blog site.
The purpose of this blog is for me to refer to how this system is used.
## Markdown
### 自定义容器
``` bash
:::[type] [title]
文本内容
:::
```

### Tip 容器
``` bash
:::tip
Normal Tips Container
:::
``` 

:::tip
Normal Tips Container
:::

``` bash
:::tip Custom header

Custom header

- tips content
- tips new line

:::
```
:::tip Custom header

Custom header

- tips content
- tips new line

:::


### Warning 容器
``` bash
:::warning
Warning!!!
:::
```

:::warning
Warning!!!
:::

### Danger 容器
``` bash
:::danger
Danger!!!
:::
```
:::danger
Danger!!!
:::

### Details 容器
``` bash
:::details Click to see more

Fusce rutrum venenatis eros in hendrerit. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Nullam eget risus egestas, aliquet ipsum sed, volutpat tortor. Proin finibus tortor ac mauris finibus rutrum. Nullam tincidunt arcu eu urna ullamcorper, eu ultricies turpis ornare. Morbi id sollicitudin orci. Proin lobortis vehicula nibh a ornare. Cras sodales eu ligula quis fermentum. Proin eu ultrices leo, quis iaculis justo. Sed dictum, nulla sit amet imperdiet commodo, libero sapien semper justo, ut lobortis elit nunc vitae ante. Nullam lobortis odio quam, ac condimentum elit posuere vitae. Sed ornare, odio et rutrum varius, lorem eros gravida urna, in pharetra sapien justo non magna.

- details content
- details new line
```
```javascript
console.log('hello world')
```
``` bash
:::
```
:::details Click to see more

Fusce rutrum venenatis eros in hendrerit. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Nullam eget risus egestas, aliquet ipsum sed, volutpat tortor. Proin finibus tortor ac mauris finibus rutrum. Nullam tincidunt arcu eu urna ullamcorper, eu ultricies turpis ornare. Morbi id sollicitudin orci. Proin lobortis vehicula nibh a ornare. Cras sodales eu ligula quis fermentum. Proin eu ultrices leo, quis iaculis justo. Sed dictum, nulla sit amet imperdiet commodo, libero sapien semper justo, ut lobortis elit nunc vitae ante. Nullam lobortis odio quam, ac condimentum elit posuere vitae. Sed ornare, odio et rutrum varius, lorem eros gravida urna, in pharetra sapien justo non magna.

- details content
- details new line

```javascript
console.log('hello world')
```

:::

