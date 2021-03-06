# コンポーネントのカスタマイズ

<p class="description">Material-UIコンポーネントの外観を簡単にカスタマイズできます。</p>

As components can be used in different contexts, there are several approaches to customizing them. 最小のユースケースから最大のユースケースまで、次をご覧ください

1. [One-off customization](#1-one-off-customization)
1. [Reusable style overrides](#2-reusable-style-overrides)
1. [Dynamic variation](#3-dynamic-variation)
1. [グローバルテーマバリエーション](#4-global-theme-variation)

## 1. One-off customization

次のソリューションを使用できる特定の実装のコンポーネントのスタイルを変更する必要がある場合があります。

### Use the `sx` prop

The easiest way to add style overrides for a one-off situation is to use the `sx` prop available on all Material-UI components. Here is an example:

{{"demo": "pages/customization/how-to-customize/SxProp.js"}}

Next you'll see how you can you can use global class selectors for accessing slots inside the component. You'll also learn how to easily identify the classes which are available to you for each of the states and slots in the component.

### Overriding nested component styles

You can use the browser dev tools to identify the slot for the component you want to override. It can save you a lot of time. The styles injected into the DOM by Material-UI rely on class names that [follow a simple pattern](/styles/advanced/#class-names): `[hash]-Mui[Component name]-[name of the slot]`.

⚠️ These class names can't be used as CSS selectors because they are unstable, however, Material-UI applies global class names using a consistent convention: `Mui[Component name]-[name of the slot]`.

上記のデモに戻りましょう。 How can you override the slider's thumb?

<img src="/static/images/customization/dev-tools.png" alt="dev-tools" width="406" />

In this example, the styles are applied with `.css-ae2u5c-MuiSlider-thumb` so the name of the component is `Slider` and the name of the slot is `thumb`.

You now know that you need to target the `.MuiSlider-thumb` class name for overriding the look of the thumb:

{{"demo": "pages/customization/how-to-customize/DevTools.js"}}

### クラス名classNameでスタイルをオーバーライドする

If you would like to override the styles of the components using classes, you can use the `className` prop available on each component. For overriding the styles of the different parts inside the component, you can use the global classes available for each slot, as described in the previous section.

You can find examples of this using different styles libraries in the [Styles library interoperability](/guides/interoperability/) guide.

### 擬似クラス

*hover*、*focus*、*disabled*、*selected*などのコンポーネントの特殊状態は、より高いCSS 特異性(specificity) が設定されています。 [特異性の重み](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)は、特定のCSS宣言に適用されます。

コンポーネントの特別な状態をオーバーライドするには、**特異性を高める必要があります** 。 *disable*状態と、 **pseudo-class**(`:disabled`)を使用したボタンコンポーネントの例を示します。

```css
.Button {
  color: black;
}

/* Increase the specificity */
.Button:disabled {
  color: white;
}
```

```jsx
<Button disabled className="Button">
```

Sometimes, you can't use a **pseudo-class**, as the state doesn't exist in the web specification. メニュー項目の構成要素と*選ばれた*例として述べる。 You can use the `.Mui-selected` global class name to customize the special state of the `MenuItem` component:

```css
.MenuItem {
  color: black;
}

/* Increase the specificity */
.MenuItem.Mui-selected {
  color: blue;
}
```

```jsx
<MenuItem selected className="MenuItem">
```

#### 1つのコンポーネント状態をオーバーライドするために、特異性を高める必要があるのはなぜですか。

設計上、CSS仕様では疑似クラスを使用することで、特定性を高めています。 For consistency with native elements, Material-UI increases the specificity of its custom pseudo-classes. これには1つの重要な利点があり、カスタマイズしたい状態を簡単に選択できます。

#### What custom pseudo-classes are available in Material-UI?

You can rely on the following [global class names](/styles/advanced/#with-material-ui-core) generated by Material-UI:

| State         | グローバルクラス名(global class name) |
|:------------- |:---------------------------- |
| checked       | `.Mui-checked`               |
| disabled      | `.Mui-disabled`              |
| error         | `.Mui-error`                 |
| focused       | `.Mui-focused`               |
| focus visible | `.Mui-focusVisible`          |
| required      | `.Mui-required`              |
| expanded      | `.Mui-expanded`              |
| selected      | `.Mui-selected`              |

> ⚠️ Never style these pseudo-class class names directly:

```css
/* ❌ NOT OK, impact all the components with unclear side-effects */
.Mui-error {
  color: red;
}

/* ✅ OK */
.MuiOutinedInput-root.Mui-error {
  color: red;
}
```

## 2. Reusable style overrides

If you find that you need the same overrides in multiple places across your application, you can use the `experimentalStyled()` utility for creating a reusable component:

{{"demo": "pages/customization/how-to-customize/StyledCustomization.js", "defaultCodeOpen": true}}

With it, you have access to all of a component's props to dynamically style the component.

## 3. Dynamic variation

In the previous section, we learned how to override the style of a Material-UI component. では、これらのオーバーライドを動的にする方法を見てみましょう。 Here are four alternatives; each has its pros and cons.

### 動的CSS

{{"demo": "pages/customization/how-to-customize/DynamicCSS.js", "defaultCodeOpen": false}}

### クラス名のブランチ

{{"demo": "pages/customization/how-to-customize/DynamicClassName.js"}}

### CSS変数

{{"demo": "pages/customization/how-to-customize/DynamicCSSVariables.js"}}

### テーマのネスティング

{{"demo": "pages/customization/how-to-customize/DynamicThemeNesting.js"}}

## 4. グローバルテーマバリエーション

`theme`の`overrides`キーを利用すると、Material-UIによってDOMに注入されるすべてのスタイルを潜在的に変更できます。 詳細については、ドキュメントの[テーマセクションをご覧ください](/customization/globals/#css)。

Please take a look at the theme's [global overrides page](/customization/theme-components/) for more details.
