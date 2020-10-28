---
title: "Lista Ligada (Linked List) em C"
date: 2019-02-22 18:47:38
layout: post
---


Pretendo implementar alguns algoritmos de grafos no futuro e pensei que seria interessante implementar as estruturas de dados necessárias. Não tenho nenhum compromisso com a performance ou elegância do código, a implementação é a mais simples possível.

Caso queira ver uma **boa** implementação de linked list, [essa](http://www.bxr.su/OpenBSD/sys/sys/queue.h) é bem interessante.

## Começando

Antes de implementar de fato, vamos pensar em como uma lista ligada funciona. 

> **Definição:** É um conjunto de itens onde cada item é parte de um  **nó**. Cada nó também contém um link para o pŕóximo elemento.

*Quer que eu desenhe?*

![lista ligada](/images/2019-02-22-lista-ligada-linked-list-em-c/01.png)

Na imagem **x** é um nó e a seta indica um link para o elemento **t**.

## Estruturas

Já temos informação o suficiente para começar a implementação.

{% highlight c %}
// linked_list.h
typedef struct node* link;

typedef struct node 
{
    uint16_t item;
    link next;
} node;
{% endhighlight %}

Essas linhas de código expressam exatamente a definição dada anteriormente. Um link é um ponteiro para um nó, e um nó é formado por itens e links. Nesse caso eu resolvi usar 2 bytes sem sinal para representar o item. 

Em seguida precisamos da representação da lista ligada em sí:

{% highlight c %}
struct llist 
{
    link head;
    link tail;
    uint32_t size;
};
{% endhighlight %}

Head (cabeça) é um ponteiro para o primeiro elemento da lista ligada e tail (calda) é um ponteiro para o ultimo elemento. Size representa o tamanho da lista ligada.

## Cabeçalho

A interface é a seguinte:

{% highlight c %}
typedef struct llist llist;

llist *LinkedList_Create(void);
void LinkedList_Destroy(llist *self);
void LinkedList_Prepend(llist *self, uint16_t item);
uint16_t LinkedList_Shift(llist *self);
void LinkedList_DeleteItem(llist *self, uint16_t item);
void LinkedList_Show(llist *self);
bool LinkedList_IsEmpty(llist *self);
uint32_t LinkedList_GetSize(llist *self);
{% endhighlight %}

+ **Create**: Cria uma lista ligada vazia;
+ **Destroy**: Libera todos os nós e a lista ligada;
+ **Prepend**: Adiciona um elemento no inicio da lista;
+ **Shift**: Remove o primeiro elemento da lista e retorna seu valor;
+ **DeleteItem**: Procura o item na lista e remove *(considerando que nenhum elemento tem o mesmo valor, obviamente)*;
+ **Show**: Mostra no terminal a lista;
+ **IsEmpty**: Se a lista está vazia retorna falso, caso contrário verdadeiro;
+ **GetSize**: Consulta o tamanho da lista.

A implementação tenta usar o conceito de encapsulamento. O usuário só tem acesso ao tipo da lista (llist) e seus métodos *públicos*. A biblioteca não tem nenhum método *privado* mas tem um tipo privado, o nó. 

## Criando e destruindo

Criar é bastante simples. Aloca a memória necessária, seta tudo pra zero/nulo e retorna a lista para o usuário utilizar. É, eu sei que malloc pode retornar nulo se não tiver memória o suficiente, mas não importa muito agora.

{% highlight c %}
llist *LinkedList_Create(void) 
{   
    llist *self = malloc(sizeof(llist));

    self->head = NULL;
    self->tail = NULL;
    self->size = 0;
    return self;
}
{% endhighlight %}

Agora vem o apocalipse. A destruição também é bem direta: enquanto a lista existe, libere a memória dos nós e vá para o próximo. No final, libere a memória da lista. Eu utilizei ali um link **p** para percorrer a lista, e um link **auxiliar** para guardar a referência para o próximo nó enquanto eu libero a memória do nó atual.

{% highlight c %}
void LinkedList_Destroy(llist *self)
{
    link p = self->head;
    link aux;

    while (p != NULL) {
        aux = p->next;
        free(p);
        p = aux;
    }
    free(self);
}
{% endhighlight %}

## Inserindo coisas

Vamos considerar no meio da lista. O nó aponta primeiro pro elemento que vai ficar a sua frente:

![1/2](/images/2019-02-22-lista-ligada-linked-list-em-c/03.png)

O elemento que vai ficar atrás aponta para o novo nó:

![2/2](/images/2019-02-22-lista-ligada-linked-list-em-c/04.png)

*Tcharam*

Eu não fiz a implementação de uma inserção em qualquer posição da lista porque não vou precisar para implementar um grafo. Mas eu preciso inserir no começo.

### No começo

{% highlight c %}
void LinkedList_Prepend(llist *self, uint16_t item)
{
    link new_node = (link) malloc(sizeof(node));
    new_node->item = item;
    new_node->next = self->head;
    self->head = new_node;
    if(LinkedList_IsEmpty(self)) self->tail = new_node;
    self->size++;
}
{% endhighlight %}

## Retirando coisas

Queremos retirar o elemento do meio:

![1/2](/images/2019-02-22-lista-ligada-linked-list-em-c/05.png)

*Easy*, só o nó anterior a ele apontar para o próximo.

![2/2](/images/2019-02-22-lista-ligada-linked-list-em-c/06.png)

Quase como um passo de mágica, não?

### No começo

Retirar do começo é bem simples, o mais importante é lembrar dos *edge cases*.

1. Se a lista já tiver vazia? retorna 0; (considerando que 0 não é um valor válido para a lista, só valores > 0)
2. Resgata o item antes de apagar o nó;
3. E se só tiver 1 elemento na lista? Esvazia a lista colocando para null;
4. Se for qualquer outra situação, aponta a cabeça para o segundo elemento da lista.
5. Retorna o item para o usuário caso ele precise.

{% highlight c %}
uint16_t LinkedList_Shift(llist *self)
{
    if(LinkedList_IsEmpty(self)) return 0;
    uint16_t item = self->head->item;
    link node = self->head;
    if(LinkedList_GetSize(self) == 1) {
        self->head = NULL;
        self->tail = NULL;
    } else {
        self->head = self->head->next;
    }
    free(node);
    self->size--;
    return item;
}
{% endhighlight %}

### Um item específico

Como eu disse acima, um item tem de ser único dentro da lista. Nada impede isso na verdade, vai ser um *acordo de cavalheiro*.

Essa função ficou complicada, mas está funcionando, é isso que importa. 

{% highlight c %}
void LinkedList_DeleteItem(llist *self, uint16_t item)
{
    if(self->size == 0) return;

    // Caso seja o primeiro item, já retira e termina a função
    link p = self->head;
    if (p->item == item) {
        self->head = p->next;
        free(p);
        self->size--;
        return;
    } else if (self->size == 1) { // Caso não seja, mas só tem 1 elemento
        return;
    }
    
    // Caso esteja no segundo pra frente ou não esteja na lista
    link p_after = p->next;
    while ( (p_after->item != item) && (p_after->next != NULL) ) {
        p = p_after;
        p_after = p_after->next;
    }
    
    // Esta na lista
    if (p_after->item == item) {
        if(p_after->next == NULL) p->next = NULL; // É o ultimo? 
        else p->next = p_after->next; 
        self->size--;
        free(p_after);
    }
}
{% endhighlight %}

## Considerações finais

Os outros três métodos (Show, GetSize e IsEmpty) são bem triviais. Acho que com essa estrutura eu consigo implementar uma lista de adjacências para representar um grafo no futuro :D. Tentarei escrever um post sobre isso também. 

Nesse projeto eu utilizei [Ceedling](https://github.com/ThrowTheSwitch/Ceedling), uma ferramenta para buildar e testar projetos em C. Foi a primeira vez que usei e funcionou perfeitamente. Fica a dica para quem escreve C para sistemas embarcados: começar a escrever ["código em C que não é uma merda"](http://www.throwtheswitch.org/) haha. 

Esse código pode ser encontrado [aqui](https://github.com/rafaellcoellho/linked-list).

## Referências 
+ [Sedgewick, Robert - Algorithms in C, Parts 1-4 (1998)](https://www.amazon.com/Algorithms-Parts-1-4-Fundamentals-Structures/dp/0201314525)
