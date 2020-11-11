---
layout: post
title:  "Desafio em entrevista [PT]"
date:   2020-11-11 14:00:00 -0300
categories: [python, entrevista-tecnica]
---

### Problema:
Considere o seguinte jogo hipotético muito semelhante a Banco Imobiliário, onde várias de suas mecânicas foram simplificadas. Numa partida desse jogo, os jogadores se alteram em rodadas, numa ordem definida aleatoriamente no começo da partida. Os jogadores sempre começam uma partida com saldo de 300 para cada um.
Nesse jogo, o tabuleiro é composto por 20 propriedades em sequência. Cada propriedade tem um custo de venda, um valor de aluguel, um proprietário caso já estejam compradas, e seguem uma determinada ordem no tabuleiro. Não é possível construir hotéis e nenhuma outra melhoria sobre as propriedades do tabuleiro, por simplicidade do problema. No começo da sua vez, o jogador joga um dado equiprovável de 6 faces que determina quantas espaços no tabuleiro o jogador vai andar.
Ao cair em uma propriedade sem proprietário, o jogador pode escolher entre comprar ou não a propriedade. Esse é a única forma pela qual uma propriedade pode ser comprada.
Ao cair em uma propriedade que tem proprietário, ele deve pagar ao proprietário o valor do aluguel da propriedade.
Ao completar uma volta no tabuleiro, o jogador ganha 100 de saldo.
Jogadores só podem comprar propriedades caso ela não tenha dono e o jogador tenha o dinheiro da venda.
Ao comprar uma propriedade, o jogador perde o dinheiro e ganha a posse da propriedade. Cada um dos jogadores tem uma implementação de comportamento diferente, que dita as ações que eles vão tomar ao longo do jogo. Mais detalhes sobre o comportamento serão explicados mais à frente.
Um jogador que fica com saldo negativo perde o jogo, e não joga mais. Perde suas propriedades e portanto podem ser compradas por qualquer outro jogador. Termina quando restar somente um jogador com saldo positivo, a qualquer momento da partida. Esse jogador é declarado o vencedor.
Desejamos rodar uma simulação para decidir qual a melhor estratégia. Para isso, idealizamos uma partida com 4 diferentes tipos de possíveis jogadores. 
#### Os comportamentos definidos são:
- O jogador um é impulsivo.
- O jogador dois é exigente.
- O jogador três é cauteloso.
- O jogador quatro é aleatório.
- O jogador impulsivo compra qualquer propriedade sobre a qual ele parar.
- O jogador exigente compra qualquer propriedade, desde que o valor do aluguel dela seja maior do que 50.
- O jogador cauteloso compra qualquer propriedade desde que ele tenha uma reserva de 80 saldo sobrando depois de realizada a compra.
- O jogador aleatório compra a propriedade que ele parar em cima com probabilidade de 50%.
- Caso o jogo demore muito, como é de costume em jogos dessa natureza, o jogo termina na milésima rodada com a vitória do jogador com mais saldo. 
- O critério de desempate é a ordem de turno dos jogadores nesta partida.

#### Saída:
Uma execução do programa proposto deve rodar 300 simulações, imprimindo no console os dados referentes às execuções.

#### Esperamos encontrar nos dados as seguintes informações:
- Quantas partidas terminam por time out (1000 rodadas).
- Quantos turnos em média demora uma partida.
- Qual a porcentagem de vitórias por comportamento dos jogadores.
- Qual o comportamento que mais vence.

----------------

### Solução proposta:

```python
"""
>>> jogador_1 = Jogador()
>>> jogador_1.saldo
300
>>> jogador_2 = Jogador()
>>> jogador_2.saldo
300
>>> propriedade = Propriedade(custo_de_venda=30, valor_de_aluguel=5)
>>> propriedade.custo_de_venda
30
>>> propriedade.valor_de_aluguel
5
>>> print(propriedade.proprietario)
None
>>> jogador_1.pagar_aluguel(jogador=jogador_2, propriedade=propriedade)
>>> jogador_1.saldo
295
>>> jogador_2.saldo
305
>>> jogador_impulsivo = JogadorImpulsivo()
>>> jogador_impulsivo.saldo
300
>>> print(jogador_impulsivo)
Impulsivo
>>> propriedade.proprietario = jogador_impulsivo
>>> print(propriedade.proprietario)
Impulsivo
>>> propriedade.proprietario = None
>>> print(propriedade.proprietario)
None
>>> jogador_impulsivo.comprar_propriedade(propriedade=propriedade)
>>> jogador_impulsivo.saldo
270
>>> print(propriedade.proprietario)
Impulsivo
>>> len(jogador_impulsivo.propriedades)
1
>>> jogador_impulsivo.comprar_propriedade(propriedade=propriedade)
>>> jogador_impulsivo.saldo
270
>>> print(propriedade.proprietario)
Impulsivo
>>> len(jogador_impulsivo.propriedades)
1
>>> jogador_exigente = JogadorExigente()
>>> jogador_exigente.saldo
300
>>> jogador_exigente.comprar_propriedade(propriedade=propriedade)
>>> jogador_exigente.saldo
295
>>> jogador_impulsivo.saldo
275
>>> propriedade_2 = Propriedade(custo_de_venda=100, valor_de_aluguel=51)
>>> jogador_exigente.comprar_propriedade(propriedade=propriedade_2)
>>> jogador_exigente.saldo
195
>>> print(propriedade_2.proprietario)
Exigente
>>> len(jogador_exigente.propriedades)
1
>>> jogador_impulsivo.comprar_propriedade(propriedade=propriedade_2)
>>> jogador_impulsivo.saldo
224
>>> jogador_exigente.saldo
246
>>> len(jogador_impulsivo.propriedades)
1
>>> jogador_cauteloso = JogadorCauteloso()
>>> jogador_cauteloso.saldo
300
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade)
>>> jogador_cauteloso.saldo
295
>>> jogador_impulsivo.saldo
229
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade_2)
>>> jogador_cauteloso.saldo
244
>>> jogador_exigente.saldo
297
>>> propriedade_3 = Propriedade(custo_de_venda=165, valor_de_aluguel=10)
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade_3)
>>> jogador_cauteloso.saldo
244
>>> propriedade_4 = Propriedade(custo_de_venda=164, valor_de_aluguel=10)
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade_4)
>>> jogador_cauteloso.saldo
80
>>> len(jogador_cauteloso.propriedades)
1
>>> jogador_cauteloso.ativo
True
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade_2)
>>> jogador_cauteloso.saldo
29
>>> print(propriedade_4.proprietario)
Cauteloso
>>> jogador_cauteloso.comprar_propriedade(propriedade=propriedade_2)
>>> jogador_cauteloso.saldo
-22
>>> len(jogador_cauteloso.propriedades)
0
>>> print(propriedade_4.proprietario)
None
>>> jogador_cauteloso.ativo
False
>>> tabuleiro = Tabuleiro(tamanho_tabuleiro=20)
>>> len(tabuleiro.tablero)
20
>>> jogador_exigente.saldo
399
>>> jogador_exigente.mover(5)
>>> jogador_exigente.posicao_tabuleiro
5
>>> jogador_exigente.mover(5)
>>> jogador_exigente.mover(5)
>>> jogador_exigente.posicao_tabuleiro
15
>>> jogador_exigente.mover(5)
>>> jogador_exigente.posicao_tabuleiro
0
>>> jogador_exigente.saldo
499
>>> jogador_impulsivo.saldo
229
>>> jogador_exigente > jogador_impulsivo
True
>>> jogador_cauteloso = JogadorCauteloso()
>>> jogador_cauteloso.saldo
300
>>> jogadores = []
>>> jogadores.append(jogador_exigente)
>>> jogadores.append(jogador_impulsivo)
>>> jogadores.append(jogador_cauteloso)
>>> print(max(jogadores))
Exigente
>>> jogador_cauteloso.saldo = 499
>>> print(max(jogadores))
Exigente
>>> jogador_cauteloso.saldo = 500
>>> print(max(jogadores))
Cauteloso
>>> jogador_impulsivo.saldo = 500
>>> print(max(jogadores))
Impulsivo
"""
import random
from functools import total_ordering
from collections import Counter


@total_ordering
class Jogador:

    def __init__(self, saldo=300):
        self.saldo = saldo
        self.posicao_tabuleiro = 0
        self.ativo = True
        self.propriedades = list()

    def __eq__(self, other):
        return self.saldo == other.saldo

    def __lt__(self, other):
        return self.saldo < other.saldo

    def add_propriedade(self, propriedade):
        self.propriedades.append(propriedade)

    def pagar_aluguel(self, jogador, propriedade):
        self.saldo -= propriedade.valor_de_aluguel
        jogador.saldo += propriedade.valor_de_aluguel

    def comprar(self, propriedade, estrategia):
        if estrategia:
            self.saldo -= propriedade.custo_de_venda
            propriedade.proprietario = self
            self.add_propriedade(propriedade=propriedade)
        elif propriedade.proprietario is not None:
            self.pagar_aluguel(jogador=propriedade.proprietario, propriedade=propriedade)

        if self.saldo < 0:
            self.ativo = False
            for propriedade in self.propriedades:
                propriedade.proprietario = None
            self.propriedades = list()

    def mover(self, num):
        if num + self.posicao_tabuleiro > 19:
            self.posicao_tabuleiro = self.posicao_tabuleiro + num - 20
            self.saldo += 100
        else:
            self.posicao_tabuleiro += num


class JogadorImpulsivo(Jogador):

    def comprar_propriedade(self, propriedade):
        estrategia = propriedade.proprietario is None and self.saldo >= propriedade.custo_de_venda
        self.comprar(propriedade=propriedade, estrategia=estrategia)

    def __str__(self):
        return f'Impulsivo'


class JogadorExigente(Jogador):

    def comprar_propriedade(self, propriedade):
        estrategia = (propriedade.proprietario is None
                      and self.saldo >= propriedade.custo_de_venda
                      and propriedade.valor_de_aluguel > 50)
        self.comprar(propriedade=propriedade, estrategia=estrategia)

    def __str__(self):
        return f'Exigente'


class JogadorCauteloso(Jogador):

    def comprar_propriedade(self, propriedade):
        estrategia = (propriedade.proprietario is None
                      and self.saldo - 80 >= propriedade.custo_de_venda)
        self.comprar(propriedade=propriedade, estrategia=estrategia)

    def __str__(self):
        return f'Cauteloso'


class JogadorAleatorio(Jogador):

    def comprar_propriedade(self, propriedade):
        estrategia = (propriedade.proprietario is None
                      and self.saldo >= propriedade.custo_de_venda
                      and bool(random.randrange(2)))
        self.comprar(propriedade=propriedade, estrategia=estrategia)

    def __str__(self):
        return f'Aleatório'


class Propriedade:

    def __init__(self, custo_de_venda, valor_de_aluguel):
        self.custo_de_venda = custo_de_venda
        self.valor_de_aluguel = valor_de_aluguel
        self.proprietario = None


class Tabuleiro:

    def __init__(self, tamanho_tabuleiro):
        self.tablero = [Propriedade(custo_de_venda=(random.randint(70, 280)),
                                    valor_de_aluguel=(random.randint(10, 100)))
                        for _ in range(tamanho_tabuleiro)]

    def get_propriedade(self, posicao_tabuleiro):
        return self.tablero[posicao_tabuleiro]


def partida():
    jogadores = [
        JogadorImpulsivo(),
        JogadorExigente(),
        JogadorCauteloso(),
        JogadorAleatorio()
    ]
    random.shuffle(jogadores, random.random)

    tabuleiro = Tabuleiro(tamanho_tabuleiro=20)

    rodadas = 0
    while rodadas < 1000 and len(jogadores) > 1:
        for jogador in jogadores:
            jogador.mover(num=random.randint(1, 6))
            posicao_tabuleiro = jogador.posicao_tabuleiro
            propriedade = tabuleiro.get_propriedade(posicao_tabuleiro=posicao_tabuleiro)
            jogador.comprar_propriedade(propriedade=propriedade)

            if not jogador.ativo:
                jogadores.remove(jogador)

        rodadas += 1

    if len(jogadores) == 1:
        ganhador = jogadores[0]
    else:
        ganhador = max(jogadores)

    return rodadas, str(ganhador)


if __name__ == '__main__':
    rodadas = list()
    jogadores = list()
    for i in range(300):
        cant_rodadas, ganhador = partida()
        rodadas.append(cant_rodadas)
        jogadores.append(ganhador)

    print(f'Terminam por time-out {rodadas.count(1000)} partidas.')
    print(f'Uma partida demora em média {(sum(rodadas) / len(rodadas)):.2f} turnos.')
    word_counts = Counter(jogadores)
    exigente = word_counts['Exigente']
    cauteloso = word_counts['Cauteloso']
    aleatorio = word_counts['Aleatório']
    impulsivo = word_counts['Impulsivo']
    print(f'Porcentagem de vitórias do Exigente: {(exigente / len(jogadores) * 100):.2f} %.')
    print(f'Porcentagem de vitórias do Cauteloso: {(cauteloso / len(jogadores) * 100):.2f} %.')
    print(f'Porcentagem de vitórias do Aleatório: {(aleatorio / len(jogadores) * 100):.2f} %.')
    print(f'Porcentagem de vitórias do Impulsivo: {(impulsivo / len(jogadores) * 100):.2f} %.')
    vencedor, cantidad = word_counts.most_common(1)[0]
    print(f'O comportamento que mais vence é {vencedor} com {cantidad} vitórias.')
```

----------------

### Saída do console:

```python
Terminam por time-out 5 partidas.
Uma partida demora em média 67.08 turnos.
Porcentagem de vitórias do Exigente: 31.33 %.
Porcentagem de vitórias do Cauteloso: 24.67 %.
Porcentagem de vitórias do Aleatório: 21.67 %.
Porcentagem de vitórias do Impulsivo: 22.33 %.
O comportamento que mais vence é Exigente com 94 vitórias.

Process finished with exit code 0
```
