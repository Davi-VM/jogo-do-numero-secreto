# Jogo do número secreto
## Introdução :page_facing_up:
Esse projeto foi desenvolvido na formação 'A partir do zero: iniciante em programação' da [Alura](https://www.alura.com.br/) e tem como 
intuito aplicar a lógica de programação com a linguagem de programação JavaScript, através da criação de um 
jogo de adivinhação onde o usuário deve tentar acertar o número criado pelo computador.

## Descrição :notebook:
Complementando o que foi dito anteriormente, a ideia desse projeto é que o computador crie um número pseudoaleatório
e que o usuário tente acertá-lo com o menor número de tentativas possível, tendo um sistema de feedback que o orientará se o seu chute foi alto ou baixo.

Observa-se que o programa possui uma caixa de entrada de dados responsável por receber o valor do chute e dois botões, um para realizar a verificação do 
valor do chute e outro para reiniciar o programa.

No projeto, foram estabelecidas duas restrições voltadas ao valor recebido pela caixa de entrada de dados. A primeira restrição está relacionada aos valores
nulos e menores que zero, ambos não são aceitos pelo software, retornando uma mensagem caso sejam digitados e parando o programa para que mais nada seja efetuado. 
Já a segunda restrição está ligada aos valores digitados que ultrapassam o intervalo estabelecido no código. Esses também não são aceitos no programa, mas têm um 
diferencial em relação à última restrição: possuem uma mensagem de aviso e um feedback no texto da página HTML, não sendo efetuado mais nenhum comando depois disso.

Sobre o intervalo mencionado no parágrafo anterior, ele é utilizado para delimitar a margem na qual os números podem aparecer, com base em um número mínimo e um número máximo, 
no caso desse projeto, de 1 a 100. O número máximo pode ser alterado sem que ocorram problemas ao longo do código, mas caso o número mínimo seja alterado, 
terá que ser realizado alterações ao longo do código.

## Código :computer:
Ao abrir o arquivo app.js, você encontrará o código JavaScript utilizado no projeto, ele foi separado em cinco partes, por meio de comentários.
Cada parte terá as suas informações mais importantes comentadas aqui:

### Declaração de variáveis
Nessa primeira seção, estão presentes as variáveis globais, como o número limite de valores sorteados(```numeroLimite```), número de tentativas(```tentativas```) e número secreto(```numeroSecreto```).
Também está contido o vetor que receberá os números já sorteados(```listaDeNumerosSorteados```). Esse bloco de código pode ser melhor visualizado abaixo:

```
//Declaraçao de variáveis
let listaDeNumerosSorteados = [];
let numeroLimite = 100;
let numeroSecreto = gerarNumeroAleatorio();
let tentativas = 1;
```

### Funções relacionadas aos textos 
A função  ```exibirTextoNaTela()``` é essencial , por pegar uma tag específica passada como parâmetro e um texto para imprimi-los na página. Porém,
não se recebe nenhuma tag ou texto em si, esses são passados por meio da função ```exibirMensagemInicial()``` e por outras partes do código que irão utilizá-la.
A função ```exibirMensagemInicial()``` defini os textos iniciais da página. Esse bloco de código pode ser melhor visualizado abaixo:

```
//Funções relacionadas aos textos
function exibirTextoNaTela(tag, texto){
    let campo = document.querySelector(tag);
    campo.innerHTML = texto;
}

function exibirMensagemInicial(){
    exibirTextoNaTela('h1', 'Jogo do número secreto');
    exibirTextoNaTela('p', `Ecolha um número entre 1 e ${numeroLimite}`);
} 

exibirMensagemInicial()
```

### Função do botão “chutar”
aqui é recebido o valor do chute da página HTML por meio de uma variável utilizando o ```document.querySelector('input').value```. Com a variável chute, é possível
fazer a verificação se o número é menor ou igual a zero usando uma condicional, retornando uma mensagem de erro caso seja verdadeiro e não realizando nada caso seja falso.

Outra comparação também é efetuada nesse início da função com o intuito de saber se o valor digitado é maior que o limite estabelecido pela variável numeroLimite. Para isso, é utilizada uma condicional que
envia uma mensagem na tela do usuário e realiza o feedback, alterando a página HTML, não realizando nada caso a condicional seja falsa.

Nessa penúltima parte, é feita uma última comparação com o intuito de verificar se o usuário acertou ou não o número secreto. Se o usuário acertar, será enviada uma mensagem por meio da alteração do texto da página e, caso erre, será enviado
um feedback também por meio da alteração da página HTML.

Para finalizar, é feita a incrementação de um a variável tentativas, sendo chamada logo depois a função ```limparCampo()```. Esse bloco de código pode ser melhor visualizado abaixo:

```
//Função do botão "chutar"
function verificarChute(){
    let chute = document.querySelector('input').value;

    if(chute <= 0){
        alert(`Números menores ou iguais a zero não fazem parte do desafio. Tente digitar um número que pertença ao intervalo de 1 a ${numeroLimite}`);
        return;
    }

    if(chute > numeroLimite){
        alert(`O número digitado é maior que o intervalo de 1 a ${numeroLimite}. Tente digitar um número que esteja no intervalo de 1 e ${numeroLimite}`);
        if(chute > numeroSecreto){
            exibirTextoNaTela('p', 'O número secreto é menor');
        }
        else{
            exibirTextoNaTela('p', 'O número secreto é maior');
        }
        return;
    }
    
    if(chute == numeroSecreto){
        exibirTextoNaTela('h1', 'Acertou!');
        let palavraTentativa = tentativas > 1 ? 'tentativas' : 'tentativa';
        let mensagemTentativas = `Você descobriu o número secreto com ${tentativas} ${palavraTentativa}`;
        exibirTextoNaTela('p', mensagemTentativas);
        document.getElementById('reiniciar').removeAttribute('disabled');
    }
    else{
        if(chute > numeroSecreto){
            exibirTextoNaTela('p', 'O número secreto é menor');
        }
        else{
            exibirTextoNaTela('p', 'O número secreto é maior');
        }

        tentativas++;
        limparCampo();
    }
}
```

### Função do botão “reiniciar jogo”
essa função é responsável por criar um novo número secreto por meio da função ```gerarNumeroAleatorio()```, 
atribuir o valor da variável tentativas para um, limpar o campo de texto por meio da função ```limparCampo()``` e colocar os 
textos iniciais através da função  ```exibirMensagemInicial()```. Ela também muda o status do botão reiniciar para disabled.
Esse bloco de código pode ser melhor visualizado abaixo:
```
//Função do botão "reiniciar jogo"
function reiniciarJogo(){
    numeroSecreto = gerarNumeroAleatorio();
    limparCampo();
    exibirMensagemInicial();
    tentativas = 1;
    document.getElementById('reiniciar').setAttribute('disabled', true)
}
```

### Funções complementares
Aqui estão contidas as funções responsáveis por limpar o campo de entrada de dados e gerar os números secretos. A função ```limparCampo()``` é bem simples, pois só retira o que foi digitado e coloca um espaço. Já a função ```gerarNumeroAleatorio()``` cria um número 
pseudoaleatório, utilizando a função math.random() vezes o número limite mais um. 

Além disso, são efetuadas duas comparações: a primeira verifica
se todos os números do intervalo foram sorteados e a segunda verifica se o número secreto sorteado já tinha sido sorteado. Caso isso seja verdadeiro, 
a função será executada novamente e, caso seja falsa, a função retornará esse valor. Esse bloco de código pode ser melhor visualizado abaixo:
```
//Funções complementares
function limparCampo(){
    chute = document.querySelector('input');
    chute.value = '';
}

function gerarNumeroAleatorio() {
    let numeroEscolhido = parseInt(Math.random() * numeroLimite + 1);
    let quantidadeDeElementosNaLista = listaDeNumerosSorteados.length;
    if(quantidadeDeElementosNaLista == numeroLimite){
        listaDeNumerosSorteados = [];
    }
    if(listaDeNumerosSorteados.includes(numeroEscolhido)){
        return gerarNumeroAleatorio();
    }
    else{
        listaDeNumerosSorteados.push(numeroEscolhido);
        return numeroEscolhido;
    }
}
```

## Site :mag_right:
Esse projeto pode ser melhor visualizado pelo seguinte link: https://jogo-do-numero-secreto-ten-orcin.vercel.app.
