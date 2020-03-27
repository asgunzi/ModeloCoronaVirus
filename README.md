# ModeloCoronaVirus
Implementação de modelo epidemiológico SIR para propagação do corona vírus em Excel


O modelo epidemiológico SIR (e derivados) é um dos mais utilizados na atualidade. Ele é simples de entender e modelar, e muito poderoso nas implicações. Entretanto, tem várias hipóteses fracas. No final das contas, há uma incerteza muito grande no que pode ocorrer. O texto a seguir discute essas implicações e fornece uma versão em Excel do modelo.

Antes, um aquecimento: modelo exponencial simples.

A tentativa mais simples de criar um modelo epidemiológico é pegar a curva de ocorrências e fitar uma curva exponencial, como na figura a seguir, com os casos confirmados de COVID-19 no Brasil.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi01.png)


Porém, esse modelo tem um defeito grave: ele cresce infinitamente. Projetando a série, no dia 90 já há 250 milhões de casos (mais do que a população do BR). Deixando mais tempo, a série vai a infinito, o que evidentemente está errado.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi02.png)

Vide planilha “ModeloExp.xlsx” para download.

    Modelo SIR – Saudáveis – Infectados – Recuperados

O modelo SIR considera a interação entre Saudáveis, Infectados e Recuperados.

O início considera toda a população saudável e alguns poucos infectados.

Um infectado pode transmitir para vários saudáveis – e essa taxa é a primeira equação abaixo. A taxa de decrescimento de saudáveis é proporcional a um fator vezes o número de saudáveis vezes a proporção de infectados na população total.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi03.jpg)

Ex. Para o corona vírus, alguns estimaram essa taxa de infecção em 4 (um infectado transmite para 4 saudáveis), outros estudos chegaram até a 10. Um dos grandes problemas desse modelo é estimar esse fator.

A segunda equação é a taxa do número de infectados: proporcional a quantos saudáveis se infectaram menos quantos infectados se recuperam.

A terceira equação é a taxa de quantos infectados se recuperam: para este exercício, é considerado um valor de 10 dias para a pessoa se recuperar.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi04.png)


O gráfico mostra o comportamento dessas curvas, para o caso do BR.

Note que o comportamento exponencial continua existindo, só que diminui à medida que o número de saudáveis diminui e mais gente se recupera.

Só esse modelo simples já explica muita coisa. Por exemplo, tirando 100 milhões de pessoas saudáveis (digamos, com quarentena forçada) e diminuindo um pouco a velocidade de transmissão, desloco e diminuo a curva de infectados.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi05.png)


Se existir uma vacina, é a mesma coisa.

Se tiver um remédio que cura rapidamente, a curva de recuperados aumenta mais rapidamente.

Utilizando o modelo SIR no Excel

O modelo no Excel descreve a dinâmica Saudáveis – Infectados – Recuperados.

Pelo método, os dias devem ser divididos em pedaços menores – no caso, 0,05 dia. Isso porque estamos integrando as equações diferenciais descritas, e nisso estamos discretizando uma curva contínua.

As fórmulas são:

Saudáveis (hoje) = saudáveis (ontem) – taxa dS/dt

Infectados (hoje) = Infectados(ontem) + taxa dI/dt

Recuperados(hoje) = recuperados (ontem) + taxa dR /dt

Ou seja, tudo depende de calcular as taxas de crescimento.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi06.jpg)

As taxas são descritas pelas equações, que dependem dos parâmetros de transmissão e recuperação (vide Excel para detalhar o cálculo).

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi07.png)

O grande X da questão é estimar os parâmetros a serem utilizados.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi08.png)


Para o de recuperação (Kr acima) foi utilizado um valor de 10 dias, que é o tempo médio de uma pessoa se recuperar. O parâmetro é o inverso do valor, portanto, 0,1 – é como se a pessoa se recuperasse 10% por dia.

O Ki tem que ser estimado a partir da distribuição real de casos no BR.

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi09.jpg)

O Ki tem que ser obtido de modo a minimizar o R2 entre o histórico e o modelo. Isso pode ser feito ou substituindo valores no braço, ou utilizando o solver (vide fórmulas na planilha).

![](https://ideiasesquecidas.files.wordpress.com/2020/03/epi10.jpg)

Nota. Este conteúdo é baseado em  https://m.youtube.com/watch?feature=youtu.be&v=UsIRJFdT_wc.

Há uma explicação detalhada dos pontos citados.

 
 Conclusão

O modelo SIR é bastante simples (utiliza apenas alguns poucos parâmetros) e é largamente utilizado para fazer forecast epidemiológico.

Há diversas variantes mais complexas deste, incorporando outras variáveis.

Algumas hipóteses contestáveis:

– Um infectado tem igual chance de infectar qualquer um dos 200 milhões de saudáveis do BR, o que não é verdade (teria que fazer um modelo com refinação geográfica e movimentação de pessoas para pegar essa dinâmica).

– Não se sabe se alguém recuperado pode ficar infectado novamente e transmitir de novo o vírus a outrem.

– Este modelo não incorpora diretamente fatores como aumento de prevenção, isolamento.

– Uma pequena diferença no parâmetro causa enorme variação nos resultados, principalmente em períodos longos de tempo, por causa do comportamento exponencial. Portanto, é como um modelo meteorológico, que vale por poucos dias, ou modelos de campeonato de futebol: muda a cada rodada e deve ser constantemente alimentado.

No final das contas, sempre vai existir uma incerteza enorme no que pode acontecer, por melhor que seja o modelo.

Coloquei este trabalho no Github: https://github.com/asgunzi/ModeloCoronaVirus

Outra fonte: o Kaggle tem um grande conjunto de datasets, e vários pesquisadores postam modelos de forecast a fim de avançar no tema.

https://www.kaggle.com/c/covid19-global-forecasting-week-2

Veja também:

https://ideiasesquecidas.com/2020/03/12/o-que-e-um-virus/

https://ideiasesquecidas.com/2017/08/09/a-teoria-dos-cisnes-negros/
