---
title: Como disparar animação de botão com enter
date: 2019-06-01 12:14:12
layout: post
---


**Problema**: quando o botão está selecionado, ao apertar enter, o evento de clicado acontece, mas a animação não. O motivo é muito simples, a pseudo-classe :active só funciona com o clique do mouse. 

Fun fact: no chrome, a tecla espaço também ativa a pseudo-classe :active, já no firefox não, divertido né?

Abaixo vai o html e css (crédito para o [w3schools](https://www.w3schools.com/csS/tryit.asp?filename=trycss_buttons_animate3)):

```html
<button class="button">Click Me</button>
```

```css
button {
  display: inline-block;
  padding: 15px 25px;
  font-size: 24px;
  cursor: pointer;
  text-align: center;
  text-decoration: none;
  outline: none;
  color: #fff;
  background-color: #4CAF50;
  border: none;
  border-radius: 15px;
  box-shadow: 0 9px #999;
}

.button:hover {background-color: #3e8e41}

.button:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
```

O jeito é partir para a ignorância e usar javascript para disparar a animação. É só usar o evento keyup para adicionar a classe de css  e o evento de keydown para retirar a classe.

```css
.button--active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
```

{% highlight javascript %}
const button = document.getElementsByClassName('button')[0];

button.addEventListener('keydown', event => {
  if(event.key === 'Enter') {
    button.classList.add('button--active');
  }
})

button.addEventListener('keyup', event => {
  if(event.key === 'Enter') {
    button.classList.remove('button--active');
  }
})
{% endhighlight %}

O html continua o mesmo, o que muda é que criamos uma nova classe no css. Aqui vai o link para do [codepen](https://codepen.io/rafaellcoellho/pen/arPVRe).

