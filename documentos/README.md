# Portal de Sistemas · RE/MAX Litorânea

Portal único que reúne os sistemas internos da imobiliária em uma só tela, com cabeçalho da RE/MAX Litorânea e um menu lateral recolhível. Cada sistema continua independente: o portal apenas os abre dentro dele.

## Sistemas incluídos

| Menu | Pasta | Para que serve |
|---|---|---|
| Gerador de Documentos | `documentos/` | Gera em Word (.docx) três tipos de documento: contrato de **aluguel** (permite importar um contrato .docx já feito para corrigir dados), contrato de **venda** e contrato de **representação imobiliária** (para venda, aluguel ou ambos). Aceita mais de um proprietário, inquilino ou contratante. |
| Sistema de Vistoria | `vistoria/` | Cria laudos de vistoria de imóveis, com a identidade visual da RE/MAX e opção de importar uma vistoria já feita. |
| Rateio de Contas | `rateio/` | Divide contas de consumo (Coelba, Embasa) entre locador e locatário, pelos dias entre as leituras e pela data de entrada do locatário. |

### Contrato de representação imobiliária (dentro do Gerador de Documentos)

Escolha **REPRESENTAÇÃO IMOBILIÁRIA** em "Tipo de contrato". O documento é gerado a partir do modelo oficial, embutido no `documentos/index.html`, e o texto se adapta ao tipo escolhido:

- **Venda:** o modelo original, com os honorários em % do valor bruto de venda, pagos no sinal.
- **Aluguel:** troca as menções a venda por locação (carta de abertura, cláusula de remuneração e hipóteses de remuneração devida) e usa o campo "Honorários do aluguel".
- **Ambos:** usa "venda ou locação" nos textos, traz as duas remunerações e mostra os dois valores (venda e aluguel) na tabela do imóvel.

Campos preenchidos: contratante(s) (com RG, órgão emissor, telefone e e-mail), corretor(a) (lista de corretores cadastrados, com CPF e CRECI preenchidos sozinhos, ou "Outro(a)" para digitar), imóvel, prazo da representação, honorários, cidade e data, e testemunhas (opcional). Com 3 ou mais contratantes, as assinaturas ficam em coluna única.

Para incluir ou tirar um corretor da lista, edite `REP_CORRETORES` no início do módulo de representação, no fim do `documentos/index.html`.

## Estrutura de pastas

```
projeto/
├── index.html                ← o portal (este sistema)
├── README.md                 ← este arquivo
├── assets/                   ← imagens do portal
│   ├── LogoLitoranea.png     (logo do cabeçalho)
│   └── LogoBalao.ico         (ícone da aba do navegador)
├── documentos/
│   └── index.html            ← Gerador de Documentos (arquivo único)
├── vistoria/
│   ├── index.html            ← Sistema de Vistoria
│   ├── assets/               ← logos usadas pela vistoria
│   └── JSON/
│       ├── estados.json
│       └── municipios.json
└── rateio/
    └── index.html            ← Rateio de Contas (arquivo único)
```

Cada sistema usa caminhos relativos à própria pasta. Por isso, não mova arquivos de dentro de uma pasta de sistema sem mover a pasta toda, e não misture os `assets` do portal com os `assets` da vistoria.

## Como usar o portal

- **Abrir um sistema:** clique no nome no menu lateral ou em um dos atalhos da tela inicial.
- **Voltar ao início:** clique na logo RE/MAX Litorânea no cabeçalho.
- **Recolher o menu:** clique no botão ☰ no cabeçalho. O menu fica só com os ícones, e o nome aparece ao passar o mouse. O portal lembra a sua escolha na próxima vez que for aberto.
- **Celular e tablet (telas estreitas):** o menu fica escondido e abre por cima da tela ao tocar em ☰. Toque fora dele, escolha um sistema ou pressione `Esc` para fechar.
- **Abrir em nova aba:** cada sistema tem esse botão na barra acima dele, útil para trabalhar com a tela inteira.
- **Não perde o que foi preenchido:** depois de aberto, o sistema continua carregado quando você troca para outro no menu. Só é recarregado se você atualizar a página.
- **Endereço de cada sistema:** o endereço do navegador muda para `#documentos`, `#vistoria` ou `#rateio`. Dá para guardar nos favoritos ou enviar o link já apontando para um sistema.

## Como executar

Os sistemas precisam ser abertos por um endereço `http://`, e não clicando duas vezes no arquivo (`file://`). O Sistema de Vistoria carrega os estados e municípios com `fetch`, e o navegador bloqueia isso em arquivos abertos direto do disco.

Para testar no seu computador, abra um terminal na pasta do projeto e rode:

```
python -m http.server 8000
```

Depois acesse `http://localhost:8000`. Em uma hospedagem (servidor da imobiliária, Netlify, GitHub Pages etc.), basta enviar a pasta inteira.

É necessária conexão com a internet, porque os sistemas carregam fontes do Google e bibliotecas (como as de PDF e ZIP da vistoria) de servidores externos.

## Como adicionar um sistema novo

1. Crie uma pasta para o sistema, por exemplo `novo-sistema/`, com o `index.html` dentro (e os arquivos que ele usa).
2. Abra o `index.html` do portal e acrescente um item na lista `SISTEMAS`, no início do script:

```js
{
  id: 'novo',                          // nome curto, sem espaços (aparece no endereço #novo)
  nome: 'Nome no Menu',
  desc: 'Frase curta mostrada no atalho da tela inicial.',
  url: 'novo-sistema/index.html',
  icone: 'documento'                   // documento, vistoria, calculadora ou generico
}
```

O item do menu, o atalho na tela inicial e a tela do sistema são criados automaticamente.

Para criar um ícone próprio, acrescente uma entrada em `ICONES` (um SVG de 24×24 com `stroke="currentColor"`) e use o nome dela em `icone`.

## Personalização

Tudo fica no `index.html` do portal.

- **Logo do cabeçalho:** a constante `LOGO_URL` aponta para `assets/LogoLitoranea.png`. Para trocar, substitua o arquivo ou mude o caminho. Se a imagem não for encontrada, o portal mostra um logo desenhado no código.
- **Ícone da aba do navegador:** `assets/LogoBalao.ico`, definido na tag `<link rel="icon">` no início do arquivo.
- **Cores:** estão no início do CSS, nas variáveis `--remax-red` (`#DC1C2E`) e `--remax-blue` (`#003DA5`), além das variações de azul e do fundo.
- **Largura do menu:** `--side-w` (aberto) e `--side-w-collapsed` (recolhido).

## Problemas comuns

| Sintoma | Causa provável | O que fazer |
|---|---|---|
| `Cannot GET /algum-caminho.html` ao abrir um sistema | O `url` do sistema na lista `SISTEMAS` não bate com o caminho do arquivo, ou o portal está em cache. | Confira a pasta e o nome do arquivo e atualize a página com `Ctrl+F5`. |
| Vistoria sem estados e municípios | O sistema foi aberto com duplo clique (`file://`). | Abra por `http://localhost` ou hospedagem. |
| A tela do sistema fica carregando | O arquivo do sistema não foi encontrado. | Use "Abrir em nova aba" para ver o erro e corrigir o caminho. |
| Logo diferente do oficial no cabeçalho | `assets/LogoLitoranea.png` não foi encontrada. | Confira se a pasta `assets` está ao lado do `index.html` do portal. |

## Observações

- Os dados digitados nos sistemas não são enviados a nenhum servidor pelo portal. A única preferência guardada é o estado do menu (recolhido ou aberto), no próprio navegador.
- Para atualizar um sistema, substitua o `index.html` da pasta dele. O portal não precisa ser alterado, a não ser que o nome ou a pasta mudem.
