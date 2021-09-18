# Vue-Paragraph
Vue 实现在文本溢出后浮现Tooltip、及文本展开收起效果

示例：

1. expandable模式(默认) - 文本展开收起效果：
```vue
<Paragraph
text="使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例"
:max-lines="1"
:width="400"
/>
```

2. tooltip模式 - 文本使用Tooltip效果： 

- 文本超出后，显示Tooltip
```vue
<Paragraph
text="使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例"
:max-lines="1"
:width="400"
type="tooltip"
/>
```

- 文本超出后，显示Tooltip 文本未超出，不显示Tooltip
```vue
<Paragraph
text="使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例"
:max-lines="3"
:width="400"
type="tooltip"
/>
```

3. 文本未超出，不显示Tooltip

- tooltipExpandable模式 - 文本同时使用Tooltip和展开收起效果：
```vue
<Paragraph
text="使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例使用示例"
:max-lines="1"
:width="400"
type="tooltipExpandable"
/>
```
