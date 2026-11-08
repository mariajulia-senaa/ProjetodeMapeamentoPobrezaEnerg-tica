# Projeto de Mapeamento da Pobreza Energética no Distrito Federal

Análise e visualização geográfica do **risco de pobreza energética** nas Regiões Administrativas (RAs) do Distrito Federal. O projeto cruza um dataset tabular com a malha cartográfica do DF e gera um mapa coroplético (mapa de calor por região) que destaca as áreas mais vulneráveis.

## Sobre o tema

Pobreza energética é a situação em que domicílios não conseguem acessar serviços de energia adequados (como eletricidade para iluminação, refrigeração e climatização) ou comprometem uma parcela excessiva da renda para pagá-los. Mapear esse risco por território ajuda a identificar onde políticas públicas e ações de apoio são mais necessárias.

## Resultado

| Arquivo | Descrição |
|---|---|
| `mapa_vulnerabilidade_df.png` | Mapa do DF colorido pelo risco de pobreza energética de cada Região Administrativa (paleta `OrRd`: quanto mais escuro, maior o risco). Regiões sem dados aparecem em cinza. |
| `graficos_pobreza_energetica.png` | Gráficos complementares da análise. |

## Estrutura do repositório

```
.
├── dataset_pobreza_energetica(2).csv   # Dataset com o risco por região administrativa
├── mapa_df/                            # Malha cartográfica (shapefile) das RAs do DF
│   └── regioes_administrativas.shp     # (e arquivos auxiliares .shx, .dbf, .prj...)
├── projetoenergia.py                   # Script principal: gera o mapa final do DF
├── mapa_energia.py                     # Versão inicial/modelo do script (caminho do mapa a ajustar)
├── mapa_vulnerabilidade_df.png         # Mapa gerado
└── graficos_pobreza_energetica.png     # Gráficos da análise
```

## Como funciona

O script `projetoenergia.py` executa quatro etapas:

1. **Carrega os dados:** lê o CSV e soma a coluna `risco_pobreza_energetica` agrupando por `regiao_administrativa`. Os nomes das regiões são convertidos para maiúsculas para casar com o shapefile.
2. **Carrega o mapa:** lê `mapa_df/regioes_administrativas.shp` com o GeoPandas.
3. **Cruza os dados:** faz um *merge* (left join) entre o shapefile (coluna `ra_nome`) e os dados agregados (coluna `regiao_administrativa`).
4. **Desenha e salva o mapa:** gera o mapa com Matplotlib e salva em `mapa_vulnerabilidade_df.png` (300 dpi).

> `mapa_energia.py` é uma versão anterior do mesmo fluxo. Ela usa um caminho de exemplo (`seu_arquivo_de_mapa_aqui.shp`) e a coluna `NM_REGIAO`, então precisa ser ajustada antes de rodar. Para gerar o mapa do DF, use `projetoenergia.py`.

## Tecnologias

- Python 3
- [pandas](https://pandas.pydata.org/): leitura e agregação dos dados
- [GeoPandas](https://geopandas.org/): leitura do shapefile e cruzamento espacial
- [Matplotlib](https://matplotlib.org/): geração do mapa

## Como executar

1. Clone o repositório:

   ```bash
   git clone https://github.com/mariajulia-senaa/ProjetodeMapeamentoPobrezaEnerg-tica.git
   cd ProjetodeMapeamentoPobrezaEnerg-tica
   ```

2. (Opcional) Crie e ative um ambiente virtual:

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Linux/macOS
   .venv\Scripts\activate         # Windows
   ```

3. Instale as dependências:

   ```bash
   pip install pandas geopandas matplotlib
   ```

4. Execute o script principal:

   ```bash
   python projetoenergia.py
   ```

O mapa será salvo como `mapa_vulnerabilidade_df.png` na pasta do projeto e exibido em uma janela.

## Dados

- **Dataset:** `dataset_pobreza_energetica(2).csv`. Colunas usadas pelo código: `regiao_administrativa` e `risco_pobreza_energetica`.
- **Malha cartográfica:** shapefile das Regiões Administrativas do DF em `mapa_df/`, com o nome da região na coluna `ra_nome`.

<!-- Complete aqui: fonte e ano dos dados, como o indicador de risco foi construído e a origem do shapefile. -->

## Observações

- Regiões presentes no mapa mas ausentes no CSV (ou escritas de forma diferente) aparecem em cinza. Se isso ocorrer, confira a grafia dos nomes nas duas fontes.
- O risco exibido é a **soma** de `risco_pobreza_energetica` por região. Se o dataset tiver mais de uma linha por região (por exemplo, vários domicílios), regiões com mais registros tendem a aparecer como mais vulneráveis. Considere usar a média ou uma taxa por habitante caso queira comparar regiões de tamanhos diferentes.

## Autora

Desenvolvido por [mariajulia-senaa](https://github.com/mariajulia-senaa).
