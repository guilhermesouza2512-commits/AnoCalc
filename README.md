AnoCalc

A Lógica por trás da Anodização · um laboratório interativo da CPA Tech

O AnoCalc simula uma carga de anodização sulfúrica de alumínio. Você ajusta o retificador, o banho e a peça, e o app mostra na hora a espessura da camada, a dureza, a cor da peça tingida e uma seção transversal da camada no estilo de microscópio eletrônico. Ele cobre tanto a anodização convencional (Tipo II) quanto a dura (Tipo III).

O objetivo é didático: mesmo que o operador faça algo muito fora dos padrões, ele vê o resultado na camada e entende por quê.

Mostrar Imagem

O que dá para ajustar
Grupo	Parâmetro	Faixa
Processo	Tipo de anodização	Convencional (Tipo II) · Dura (Tipo III)
	Aplicação alvo	Decorativo 8–12 µm · Cor escura 15–20 µm · Arquitetura 20–25 µm · Dura 40–60 µm
Retificador	Modo de controle	Tensão constante · Corrente constante
	Tensão	5–100 V
	Densidade de corrente	0,2–8 A/dm²
	Tempo	2–180 min
Banho	H₂SO₄ livre	50–400 g/L
	Alumínio dissolvido	0–40 g/L
	Temperatura	0–35 °C
	Distância ânodo–cátodo	3–60 cm
	Aditivo redutor de dissolução	0 % · 1 % · 2 % v/v
Peça	Liga	159 ligas trabalhadas (wrought) com condutividade elétrica conhecida
	Área total	Em mm², cm², dm², m², pol² ou pé², convertida para dm²
Cor	Corante	Preto · Dourado · Azul · Vermelho · Natural
O que o app mostra

Resultado da carga, sempre visível

Amostra da peça tingida, com o tom que a camada consegue absorver.
Seção transversal simulada da camada, com a espessura marcada.
Seis indicadores: espessura, densidade de corrente, tensão no fim, microdureza (HV), eficiência de Faraday e energia consumida.
Classificação da camada: Tipo II, Tipo III, camada incipiente ou fora do padrão.
Diagnósticos em linguagem de chão de fábrica, como queima, pulverulência, cor irregular, banho saturado de alumínio ou corrente baixa demais.
Simulação animada da carga, minuto a minuto, da rampa de partida ao fim.

Para aprofundar (seções recolhíveis)

Tanque em 3D com bolhas, cátodos e peças reagindo à corrente.
Vista superior dos poros, geometria da célula, curva corrente × tensão e para onde vai cada volt.
Comparação de ligas representativas no banho atual.
Tabela de causa e efeito de cada variável.
Equações do modelo e seus limites.
Como usar
Windows
Baixe AnoCalc-Windows.zip na página de Releases.
Descompacte e abra AnoCalc.exe.
Se o Windows SmartScreen avisar, clique em Mais informações → Executar assim mesmo. O aviso aparece porque o executável não tem assinatura digital paga.

Requer o Microsoft Edge WebView2, que já vem instalado no Windows 10 e 11 atualizados.

macOS
Baixe AnoCalc-Mac.zip e descompacte.
Arraste AnoCalc.app para a pasta Aplicativos.
Na primeira vez, clique com o botão direito no app e escolha Abrir. O macOS pede essa confirmação para apps fora da App Store.

Funciona em Macs Intel e Apple Silicon.

Qualquer sistema, sem instalar nada

Abra AnoCalc-offline.html em qualquer navegador moderno. Fontes, biblioteca 3D e imagens estão embutidas no arquivo, então ele funciona sem internet.

Como o modelo funciona

O AnoCalc combina leis físicas com coeficientes calibrados pela literatura e por dados de campo, e integra a carga em 90 passos de tempo.

Ohm na célula. A tensão do retificador precisa vencer um limiar (a camada barreira) e depois as resistências do banho e da camada porosa:

V = V₀ + I · (R_banho + R_camada)

O limiar V₀ sobe com o alumínio dissolvido e com o frio. A resistência do banho cresce com a distância entre ânodo e cátodo e com o alumínio dissolvido, e cai com a concentração de ácido e a temperatura. A resistência da camada aumenta à medida que ela engrossa, e por isso, em tensão constante, a corrente cai ao longo da carga. No modo corrente constante, a densidade de corrente para cada tensão é resolvida por bisseção.

Faraday. A espessura cresce com a carga elétrica, descontada a fração que não vira óxido:

de/dt = 0,352 · J · η − k_d        (µm/min, J em A/dm²)
η (eficiência): cai com temperatura alta, ácido concentrado, corrente muito baixa e ligas com cobre ou silício. O aditivo a recupera em parte.
k_d (dissolução): cresce exponencialmente com a temperatura e com o ácido. Quando ela empata com o crescimento, a camada para de engrossar e vira pó.

Joule. A potência P = V · I vira calor na interface metal–óxido. Acima de um limite, que depende da liga, da temperatura e do aditivo, o app indica queima.

Geometria do poro. O diâmetro da célula é de cerca de 2,5 nm/V e a camada barreira de cerca de 1,0 nm/V. A tensão desenha o poro, a corrente decide quanto ele cresce.

Química do banho. Cada g/L de alumínio dissolvido imobiliza cerca de 5,4 g/L de ácido sulfúrico:

Al₂O₃ + 3 H₂SO₄ → Al₂(SO₄)₃ + 3 H₂O

Dureza. A microdureza sobe com banho frio e ácido menos concentrado, e cai quando a dissolução é intensa.

Calibração

Sem aditivo, na condição de referência de campo, o modelo prevê 7,2 µm contra 7,4 µm medidos. A condição padrão do app (6063-T5, 16 V, 40 min, 190 g/L de H₂SO₄, 8 g/L de Al, 20 °C) resulta em 16,6 µm, 1,39 A/dm² e cerca de 300 HV, dentro do esperado para anodização convencional para cor escura.

Limites
A camada barreira conduz por condução iônica de alto campo, não como um resistor ôhmico. O simulador a trata como um limiar de tensão seguido de um trecho quase linear.
Os coeficientes de dissolução, dureza, queima, resposta das ligas e efeito do aditivo representam tendências. Eles não são constantes de projeto.
O AnoCalc é uma ferramenta de ensino e de raciocínio de processo. Ele não substitui ensaios de espessura, dureza e selagem na linha.
Tecnologia
HTML, CSS e JavaScript puro, sem framework e sem etapa de build no app.
Three.js r128 para o tanque 3D em WebGL.
Canvas 2D para as micrografias simuladas e a amostra tingida.
SVG para a curva corrente × tensão.
Fontes Inter e IBM Plex Mono, embutidas na versão offline.
Neutralinojs 6.10.0 para empacotar o app como programa desktop leve (cerca de 4 MB no Windows e 9 MB no Mac), usando o WebView do próprio sistema.
Estrutura sugerida do repositório
anocalc/
├── src/
│   ├── body.html          # marcação da interface
│   ├── style.css          # tema e componentes
│   ├── layout.css         # grade da bancada e responsividade
│   └── script.js          # modelo físico, desenho e interação
├── dist/
│   └── AnoCalc-offline.html   # arquivo único, tudo embutido
├── desktop/
│   ├── neutralino.config.json
│   └── resources/
│       ├── index.html     # cópia de dist/AnoCalc-offline.html
│       └── icon.png
└── docs/
    └── anocalc.png
Gerar os executáveis
bash
npm install -g @neutralinojs/neu
cd desktop
neu update
neu build --release --embed-resources

Os binários saem em desktop/dist/.

Fontes
Runge, J. M. The Metallurgy of Anodizing Aluminum: Connecting Science to Practice. Springer, 2018.
Collaboration for NDT Education. Conductivity and Resistivity Values for Aluminum & Alloys, 2002, com dados da ASM International.
Fichas técnicas de aditivos para banho sulfúrico e literatura aberta sobre redutores de dissolução.
Dados de campo de linhas de anodização atendidas pela CPA.
Licença

© CPA Corantes e Produtos para Anodização. Todos os direitos reservados.

Contato

CPA Tech · Soluções para a cadeia do alumínio comercial@cpacorantes.com.br
