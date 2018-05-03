## CSS arrow

```html
<p>Right arrow: <i class="arrow right"></i></p>
<p>Left arrow: <i class="arrow left"></i></p>
<p>Up arrow: <i class="arrow up"></i></p>
<p>Down arrow: <i class="arrow down"></i></p>
```

```css
i {
    border: solid black;
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 3px;
}

.right {
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
}

.left {
    transform: rotate(135deg);
    -webkit-transform: rotate(135deg);
}

.up {
    transform: rotate(-135deg);
    -webkit-transform: rotate(-135deg);
}

.down {
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
}
```
https://www.w3schools.com/howto/howto_css_arrows.asp
