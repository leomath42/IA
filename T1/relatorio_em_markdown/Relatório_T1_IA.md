# Relatório de IA  T1 
---
###### Vídeo Timestamp:
- 0:00 - Introdução
- 00:32 - Q1
- 03:46 - Q1 - Algoritmo DFS
- 09:19 - Q1 - Execução do DFS
- 10:35 - Q1 - DFS Exponencial Problem
- 12:11 - Q2 - BFS
- 13:42 - Q2 - Algoritmo BFS 
- 16:29 - Q2 - Execução do BFS e comparação entre BFS x DFS
- 18:22 - Q3 - A*, optmistic and consistent heuristic 
- 21:01 - Q3 - Algoritmo A* 
- 23:49 - Q3 - OpenMaze e estratégias de busca 
- 25:46 - Q4 
- 31:13 - Q5 
- 33:05 - Q7 
- 35:29 - Q7 - shortest way problem

---

![[Pasted image 20211230192848.png]]

![[Pasted image 20211230193819.png]]

##### Q1 (4 pontos) - Encontrando comida usando DFS

O Algoritmo DFS é um algoritmo de busca "vertical" e *não ótimo*, ou seja, ele não garante encontrar a solução de custo mínimo, encontrando possíveis soluções subótimas varrendo a árvore de busca verticalmente da esquerda para a direita.

- 1.**A ordem de exploração foi de acordo com o esperado? **
- 2.**Essa é uma solução ótima? Senão,  
discuta o que a busca em profundidade está fazendo de errado?**
- 3.**O Pacman realmente passa por todos os estados explorados no seu caminho para o objetivo?**
- 
###### O gif abaixo demostra parte da resposta das perguntas **1 e 2 **.

![[pacman_gif2.gif]]

Podemos perceber que o *pacman* percorre o caminho mais longo até a comida(objetivo final). Isso ocorre pois o algortimo **DFS** não é um algoritmo ótimo, ou seja, ele não garante encontrar a melhor solução para o problema, onde nesse caso, **a solução encontrada é uma solução subótima**, por isso é esperado que em alguns casos ele percorra o maior caminho até o seu objetivo.


###### Resposta da **3**.
Não. O pacman pecorre somente os estados explorados pelo caminho escolhido no final do retorno do algoritmo DFS, alguns estados não serão pecorridos pelo pacman porque não fazem parte da sequência de ações da solução.

Além disso, devemos lembrar que o DFS não é um algoritmo muito adequado para os casos onde existam ciclos no grafo de estados / árvore de busca.

O trecho de código abaixo é a **implementação do DFS que explora estados já visitados(o trecho de código comentado evita isso)**, o que causa loops, tornando o DFS um algoritmo **não completo** o que significa que ele não encontraria uma solução para o problema, causando um aumento exponencial do gasto de memória(expanção dos nós visitados).

```python
def depthFirstSearch(problem):
	node = getStartNode(problem)
	frontier = util.Stack()
	frontier.push(node)

	#closed = set()

	while not frontier.isEmpty():

		node = frontier.pop()

		#if node['STATE'] in closed:
		#	continue

		#closed.add(node['STATE'])

		if problem.isGoalState(node['STATE']):
			return getActionSequence(node)

		for sucessor in problem.expand(node['STATE']):
			child_node = getChildNode(sucessor, node)
			frontier.push(child_node)
	return []
```

Se executarmos o bloco de código abaixo em um terminal do linux e depois executarmos o pacman em outro terminal, iremos conseguir visualizar que o consumo de memória irá aumentar gradualmente

```bash
pacman() {
	echo "$(ps afu | awk 'NR>1 {$5=int($5/1024)"M";}{ print;}' | grep -i pacman.py)"; 
}

export -f pacman

watch -n1 "pacman"

# python3 pacman.py -l tinyMaze -p SearchAgent
```

![[pacman_dfs_memory-2021-12-28_11.35.20.mkv]]


execuções do tinyMaze, mediumMaze e bigMaze para o DFS:
```bash
python pacman.py -l tinyMaze -p SearchAgent  
python pacman.py -l mediumMaze -p SearchAgent  
python pacman.py -l bigMaze -z .5 -p SearchAgent
```

Saídas respectivas:
```
-- tinyMaze
Path found with total cost of 10 in 0.0 seconds
Search nodes expanded: 15
Pacman emerges victorious! Score: 500
Average Score: 500.0
Scores:        500.0
Win Rate:      1/1 (1.00)
Record:        Win

-- mediumMaze
Path found with total cost of 130 in 0.0 seconds
Search nodes expanded: 146
Pacman emerges victorious! Score: 380
Average Score: 380.0
Scores:        380.0
Win Rate:      1/1 (1.00)
Record:        Win

-- bigMaze
Path found with total cost of 210 in 0.0 seconds
Search nodes expanded: 390
Pacman emerges victorious! Score: 300
Average Score: 300.0
Scores:        300.0
Win Rate:      1/1 (1.00)
Record:        Win
```


##### Q2 (4 pontos) - BFS
- 1. **A sua implementação da BFS encontra a solução ótima? Senão, verifique a sua implementação.**
- 2. **Quantas ações compõem a solução encontrada pelo BFS?**


###### O gif abaixo demostra parte da resposta das perguntas **1 e 2 **.

Diferente do DFS, o BFS é um algoritmo ótimo e completo, isso significa que ele garante encontrar uma solução e está é uma solução ótima, ou seja, o melhor caminho.

 O gif abaixo mostra um comparação ao gif anterior:
 
![[pacman_bfs_q2.gif]]

Podemos perceber que diferente do DFS, neste caso, o pacman percorre o menor caminho até a comida.

###### Resposta da **2**.
- para o tinyMaze : 8
- para o mediumMaze: 68
- para o bigMaze:

execuções do tinyMaze, mediumMaze e bigMaze para o DFS:
```bash
python pacman.py -l tinyMaze -p SearchAgent  -a fn=bfs
python pacman.py -l mediumMaze -p SearchAgent  -a fn=bfs
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=bfs -z 0.5
```

Saídas respectivas:
```
-- tinyMaze
Path found with total cost of 8 in 0.0 seconds
Search nodes expanded: 15
Pacman emerges victorious! Score: 502
Average Score: 502.0
Scores:        502.0
Win Rate:      1/1 (1.00)
Record:        Win

-- mediumMaze
Path found with total cost of 68 in 0.1 seconds
Search nodes expanded: 269
Pacman emerges victorious! Score: 442
Average Score: 442.0
Scores:        442.0
Win Rate:      1/1 (1.00)
Record:        Win

-- bigMaze
Path found with total cost of 210 in 0.0 seconds
Search nodes expanded: 620
Pacman emerges victorious! Score: 300
Average Score: 300.0
Scores:        300.0
Win Rate:      1/1 (1.00)
Record:        Win
```


##### Q3 (4 pontos) - A* search

- 1.**A busca A* deve achar a solução ótima um pouco mais rapidamente do que a busca  de custo uniforme (549 vs. 620 nós de busca expandidos na nossa implementação, mas a aplicação de desempates pode produzir valores um pouco diferentes).**
- 2.**O que acontece em openMaze para as várias estratégias de busca?**

- O algoritmo **A\*** é uma fusão de outros dois algoritmos, o Gready Search o Uniform Cost Search(UCS).

- O **Gready Search é um algoritmo heurístico**, e é o algoritmo mais rápido(em relação aos estudados), porém é um **algoritmo não completo e subótimo** o que significa que ele não garante encontrar uma solução, e a possível solução será um solução não ótima.
- Já o **UCS**, diferente do Gready Search, é um algoritmo **completo e ótimo**, quando ele é implementado respeitando algumas regras como:
	- **O teste de objetivo** é feito **a um nó filho** no momento em que ele é **selecionado para expanção;**
	- O **teste de parada é feito quando** um nó que passa no teste de objetivo é **retirado da fronteira**.

Sendo assim, o **A\***  herda os pontos positivos dos dois algoritmos, garantindo que ele **seja rápido, completo e ótimo**.

**Além disso, para que o **A\***  se comporte como Gready Search, basta usar um g(n)\=\=0(custo de caminho utilizado no UCS), já para se comportar como UCS, basta que h(n) \=\=0  (uma heurística utlizada no Gready Search) seja heurística.
**

###### Resposta da **1**.

O seguinte comando é a execução do UCS(A* com heurística nula):
```python
python3 pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=nullHeuristic 
```

O seguinte comando é a execução do A* com heurística manhattan:
```
python3 pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic 
```

Saídas respectivas:
```python
-- UCS(A* with null heuristic)
Path found with total cost of 210 in 0.1 seconds
Search nodes expanded: 620
Pacman emerges victorious! Score: 300
Average Score: 300.0
Scores:        300.0
Win Rate:      1/1 (1.00)
Record:        Win

-- A* with manhattan heuristic
Path found with total cost of 210 in 0.1 seconds
Search nodes expanded: 549
Pacman emerges victorious! Score: 300
Average Score: 300.0
Scores:        300.0
Win Rate:      1/1 (1.00)
Record:        Win
```

- **Conseguimos perceber que a afirmação da 1) está correta, pois para o UCS a quantidade de nós expandidos é 620, já para o A* é de 549**

###### Resposta da **2**.


- **DFS*:*
	- comando:```python3 pacman.py -l openMaze -p SearchAgent -a fn=dfs```
	- saída:
	```python
	[SearchAgent] using function dfs
	[SearchAgent] using problem type PositionSearchProblem
	Path found with total cost of 298 in 0.1 seconds
	Search nodes expanded: 576
	Pacman emerges victorious! Score: 212
	Average Score: 212.0
	Scores:        212.0
	Win Rate:      1/1 (1.00)
	Record:        Win
	```

- gif: ![[dfs_openmaze.gif]]

- **BFS:**
	- comando:```python3 pacman.py -l openMaze -p SearchAgent -a fn=bfs```
	- saída:
	```python
	[SearchAgent] using function bfs
	[SearchAgent] using problem type PositionSearchProblem
	Path found with total cost of 54 in 0.1 seconds
	Search nodes expanded: 682
	Pacman emerges victorious! Score: 456
	Average Score: 456.0
	Scores:        456.0
	Win Rate:      1/1 (1.00)
	Record:        Win
	```
- gif: ![[bfs_openmaze.gif]]
-
- **UCS:**
	- comando: ```python3 pacman.py -l openMaze -p SearchAgent -a fn=astar,heuristic=nullHeuristic```
	- saída:
	```python
	[SearchAgent] using function astar and heuristic nullHeuristic
	[SearchAgent] using problem type PositionSearchProblem
	Path found with total cost of 54 in 0.1 seconds
	Search nodes expanded: 682
	Pacman emerges victorious! Score: 456
	Average Score: 456.0
	Scores:        456.0
	Win Rate:      1/1 (1.00)
	Record:        Win
	```

- gif:![[ucs_openmaze.gif]]


- **A:\***
	- comando: ```python3 pacman.py -l openMaze -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic ```
	- saída:
	```python
	[SearchAgent] using function astar and heuristic manhattanHeuristic
	[SearchAgent] using problem type PositionSearchProblem
	Path found with total cost of 54 in 0.1 seconds
	Search nodes expanded: 535
	Pacman emerges victorious! Score: 456
	Average Score: 456.0
	Scores:        456.0
	Win Rate:      1/1 (1.00)
	Record:        Win
	```
	
	- gif:![[astar_openmaze.gif]]


- **Gready Search:**
	- comando: ```python3 pacman.py -l openMaze -p SearchAgent -a fn=gready,heuristic=manhattanHeuristic --frameTime 0.1 -z 0.7```
	- saída:
	```python
	[SearchAgent] using function gready and heuristic manhattanHeuristic
	[SearchAgent] using problem type PositionSearchProblem
	Path found with total cost of 68 in 0.0 seconds
	Search nodes expanded: 89
	Pacman emerges victorious! Score: 442
	Average Score: 442.0
	Scores:        442.0
	Win Rate:      1/1 (1.00)
	Record:        Win
	```
	- gif:![[gready_openmaze.gif]]
	
	

- Conseguimos perceber que, para cada estratégia de busca, o pacman toma um caminho diferente e/ou explora diferentes espaços do mapa.
- Podemos perceber que, pelo o **UCS e o BFS acabam tendo o mesmo desempenho** em relação ao melhor caminho(isso é esperado) e **com relação ao número de nós expandidos na árvore de busca.**
	- Isso provavelmente ocorra por causa do tamanho do mapa e por ele ser 	aberto, com menso muros !
	
- Já o **DFS** teve a pior performace entre todos os algoritmos, expandindo no **total 298 nós da árvore de busca** e **pecorrendo um caminho "peculiar".**
	- Esse comportamento é esperado, pois o **DFS** não garante encontrar a melhor solução para o problema
	- **O caminho percorrido pelo DFS**acabou sendo **um "zing-zag" até o objetivo final**. Muito provavelmente a causa seja "**por como as coordenadas do mapa são contadas"(o lado direito inferior possui a maior distância do mapa)** e pelo **DFS ser um algoritmo de profundidade, o mesmo "escanea" a árvore de busca da menor até a maior distância do mapa.**
	- 
- Diferente do **DFS**, o **Gready Search** teve um desempenho melhor em relação ao número de nós expandidos da árvore de busca sendo **89 nós expandidos**, porem ainda assim tomando um caminho **"peculiar"**.
	- O **Gready Search** expande em uma única direção, ou seja, **sempre para expande na direção de menor custo da heurística,** que nesse caso é a **manhattan**. Então, de certa forma, é esperado que **ele explore poucos pontos** do mapa e **siga um caminho "relativamente curto", dependendo do formato e tamanho do mapa.**

- Já o A*, teve o melhor **desempenho em relação a expanção de nós(no total 535)**,  **perdendo somente pro Gready Search**, contudo, escolhendo o **melhor caminho até o objetivo final**.



##### Q4 (3 pontos): Encontrando todos os cantos


**Métodos:**
- CornersProblem:
	- getStartState
	-  isGoalState
	-  expand
	-  getActions
	-  getActionsCost
	-  getNextState
	-  getCostOfactionSequence

Comandos:
```python
python3 pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem

python3 pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
```

```python
-- tinyCorners
[SearchAgent] using function bfs
[SearchAgent] using problem type CornersProblem
Path found with total cost of 28 in 0.0 seconds
Search nodes expanded: 269
Pacman emerges victorious! Score: 512
Average Score: 512.0
Scores:        512.0
Win Rate:      1/1 (1.00)
Record:        Win

-- mediumCorners
[SearchAgent] using function bfs
[SearchAgent] using problem type CornersProblem
Path found with total cost of 106 in 0.1 seconds
Search nodes expanded: 1988
Pacman emerges victorious! Score: 434
Average Score: 434.0
Scores:        434.0
Win Rate:      1/1 (1.00)
Record:        Win


-- tinyCorners

-- mediumCorners
```

##### Q5 (3 pontos): Heurística para o Problema dos Cantos


Comandos:

```
python3  pacman.py -l tinyCorners -p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristic

python3  pacman.py -l mediumCorners -p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristic 
```


```python

-- tinyCorners
[SearchAgent] using function aStarSearch and heuristic cornersHeuristic
[SearchAgent] using problem type CornersProblem
Path found with total cost of 28 in 0.0 seconds
Search nodes expanded: 190
Pacman emerges victorious! Score: 512
Average Score: 512.0
Scores:        512.0
Win Rate:      1/1 (1.00)
Record:        Win

-- mediumCorners
[SearchAgent] using function aStarSearch and heuristic cornersHeuristic
[SearchAgent] using problem type CornersProblem
Path found with total cost of 106 in 0.1 seconds
Search nodes expanded: 709
Pacman emerges victorious! Score: 434
Average Score: 434.0
Scores:        434.0
Win Rate:      1/1 (1.00)
Record:        Win
```

```python
# Heuristica 
def cornersHeuristic(state, problem):
	# return 0 # Default to trivial solution

	xy1 = state[0]     # pacman position

	corners = state[1] # corners positions #

	manhattans = []    # manhattans poisitions

	for xy2 in corners:
		manhattans.append(abs(xy1[0] - xy2[0]) + abs(xy1[1] - xy2[1]))

	return max(manhattans)
```

Para descobrimos que a heuristica é admissível e consistente, devemos perceber os seguintes fatos:

A heurística ideal **h\*(n)** e a heurística **h(n)** do cornersHeuristic.

- Sabemos que *h(n) <= h\*(n)* vai ser sempre verdadeiro, pois a cornersHeuristic foi desenvolvida desconsiderando os obstáculos e regras do jogo, ou seja, é uma heurística otimista. Além disso, **temos que o valor estimado pela heurística é o valor máximo das distancias entre o pacman e cada corner.**
- Sendo assim, sabemos que **h(n)** será uma **heurística admissível para todo n no espaço de estados.**
- Sabemos também que a estimativa da heurística "diminui" conforme a aproximação de um estado objetivo(goal), **sendo no estado objetivo o valor estimado pela heurística igual a 0**
- Ou seja, conforme a heurística "diminui", o valor de f(n) sempre irá aumentar, pois o valor estimado por h(n) sempre será menor que o valor de h*(n).
- ***Sendo assim, a heurística cornersHeuristic é consistente !***


##### Q7 (3 pontos): Busca subótima


Comando: 
```
python3 pacman.py -l bigSearch -p ClosestDotSearchAgent -z .5
```

Saídas:
```
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
Path found with cost 350.
Pacman emerges victorious! Score: 2360
Average Score: 2360.0
Scores:        2360.0
Win Rate:      1/1 (1.00)
Record:        Win
```

**Seu agente ClosestDotSearchAgent nem sempre encontrará o caminho mais curto  
possível pelo labirinto. Certifique-se de entender o porquê e tente criar um pequeno  
exemplo em que ir repetidamente para o ponto mais próximo não resulte em  
encontrar o caminho mais curto para comer todos os pontos.**

O q7Maze demostra esse processo:

Comando:
```
python3  pacman.py -l q7Maze  -p ClosestDotSearchAgent -z .5 --frameTime 100
```
Saída:
```
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
Path found with cost 95.
Pacman emerges victorious! Score: 1295
Average Score: 1295.0
Scores:        1295.0
Win Rate:      1/1 (1.00)
Record:        Win
```

- **O caminho mais rápido** para o pacman seria percorrer todo o **mapa apartir do centro como** se fosse **uma espiral**