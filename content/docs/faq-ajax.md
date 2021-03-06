---
id: faq-ajax
title: AJAX e APIs
permalink: docs/faq-ajax.html
layout: docs
category: FAQ
---

### Como fazer uma requisição AJAX? {#como-fazer-uma-requisicao-ajax}

Você pode usar qualquer biblioteca AJAX que desejar com React. Algumas populares são [Axios](https://github.com/axios/axios), [jQuery AJAX](https://api.jquery.com/jQuery.ajax/), e o nativo do navegador [window.fetch](https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API).

### Onde eu devo fazer uma requisição AJAX no ciclo de vida do componente? {#onde-eu-devo-fazer-uma-requisicao-ajax-no-ciclo-de-vida-do-componente}

Você deve preencher dados com requisições AJAX no método [`componentDidMount`](/docs/react-component.html#mounting) do ciclo de vida. Isto é para que você consiga usar `setState` para atualizar seu componente quando os dados forem recebidos.

### Exemplo: Usando resultados AJAX para definir o estado local {#exemplo-usando-ajax-para-definir-o-estado-local}

O componente abaixo demonstra como deve fazer uma requisição AJAX no `componentDidMount` para preencher o estado (state) local. 

A API de exemplo retorna um objeto JSON como este:

```
{
  "items": [
    { "id": 1, "name": "Apples",  "price": "$2" },
    { "id": 2, "name": "Peaches", "price": "$5" }
  ] 
}
```

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: null,
      isLoaded: false,
      items: []
    };
  }

  componentDidMount() {
    fetch("https://api.example.com/items")
      .then(res => res.json())
      .then(
        (result) => {
          this.setState({
            isLoaded: true,
            items: result.items
          });
        },
        // Nota: É importante lidar com os erros aqui
        // em vez de um bloco catch() para não recebermos
        // exceções de erros dos componentes.
        (error) => {
          this.setState({
            isLoaded: true,
            error
          });
        }
      )
  }

  render() {
    const { error, isLoaded, items } = this.state;
    if (error) {
      return <div>Error: {error.message}</div>;
    } else if (!isLoaded) {
      return <div>Loading...</div>;
    } else {
      return (
        <ul>
          {items.map(item => (
            <li key={item.name}>
              {item.name} {item.price}
            </li>
          ))}
        </ul>
      );
    }
  }
}
```
