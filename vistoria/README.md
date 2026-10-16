# 🏠 Sistema de Laudo de Vistoria Imobiliária

Sistema web para elaboração de laudos de vistoria de imóveis, desenvolvido para corretores e imobiliárias. Permite documentar fotograficamente o estado do imóvel, gerar relatórios PDF profissionais e reimportar laudos já feitos para complementá-los.

---

## 📋 O que este sistema faz

### 1. Cadastro da Vistoria
- Registra informações do corretor (nome, CRECI, CPF)
- Identifica o tipo de vistoria (Inicial, Final ou Complementar)
- Cadastra dados do imóvel (endereço, tipo, destinação)
- Registra partes envolvidas (locador e locatário)

### 2. Avaliação do Estado do Imóvel
- Classifica o imóvel em: Novo, Excelente, Bom, Regular ou Ruim
- Cada classificação possui identificação visual por cores

### 3. Registro Fotográfico
O sistema permite adicionar fotos de duas formas:

**📷 Itens da Vistoria**
- Fotos de todos os ambientes e itens do imóvel
- Cada foto pode ter uma descrição detalhada

**🔧 Danos Encontrados**
- Registro fotográfico específico de problemas ou defeitos
- Documentação visual de estragos pré-existentes

### 4. Captura de Imagens
- **Câmera**: Acesse a câmera do dispositivo diretamente pelo navegador
- **Galeria**: Selecione uma ou **várias fotos de uma vez** (upload múltiplo)
- Visualização em cards organizados e de tamanho padronizado

### 5. Informações Adicionais
- Campo de texto livre para observações que devem constar no laudo
- O conteúdo é inserido no PDF final, logo após as tabelas e antes da data/assinaturas

### 6. Geração de PDF Profissional
- Laudo formatado com logo da empresa
- Texto introdutório automático com todos os dados cadastrados
- Tabelas organizadas com fotos e descrições
- Bloco de "Informações Adicionais" (quando preenchido)
- Assinaturas digitais para locador, locatário e corretor
- Numeração de páginas personalizada
- **Controle de compressão das fotos**: é possível escolher entre "Alta qualidade", "Recomendado" ou "Máxima compressão" antes de gerar o PDF, para reduzir bastante o tamanho final do arquivo sem precisar editar as fotos manualmente

### 7. Importar Vistoria (a partir de um PDF já gerado)
- Permite selecionar um PDF gerado anteriormente por este sistema
- O sistema lê o arquivo e preenche automaticamente:
  - Dados do corretor, do imóvel, das partes e as datas
  - As tabelas de "Itens da Vistoria" e "Danos Encontrados", já com as fotos e descrições
- Os itens importados são **adicionados** aos que já estiverem na tela (nada é apagado), permitindo complementar um laudo antigo com novas fotos/observações
- ⚠️ **Limitações conhecidas** (ver seção [Limitações Conhecidas](#-limitações-conhecidas) abaixo)

### 8. Exportação de Fotos
- Download de todas as fotos em arquivo ZIP organizado
- Pastas separadas: "Itens" e "Danos"

---

## 🚀 Como usar

### Acesso ao Sistema
1. Abra o arquivo `index.html` em um navegador moderno
2. O sistema funciona em computadores, tablets e smartphones
3. É necessário estar conectado à internet (o sistema carrega bibliotecas externas para gerar/ler PDFs, ZIPs e a lista de estados/municípios)

### Passo a Passo

#### 1. Preencha os Dados
- Selecione o tipo de vistoria
- Informe estado e município (Bahia e Camaçari já vêm pré-selecionados)
- Preencha datas, dados do corretor e do imóvel
- Selecione a condição do imóvel

#### 2. Adicione Fotos
- Clique em **Câmera** para tirar foto na hora
- Clique em **Galeria** para selecionar uma ou várias fotos do dispositivo de uma vez
- Descreva cada foto no campo de texto do card
- Use o ícone 🗑️ para remover fotos indesejadas

#### 3. Preencha Informações Adicionais (opcional)
- Digite qualquer observação que deva constar no laudo antes da data/assinaturas

#### 4. Gere o PDF
- Escolha o nível de compressão das fotos ("Recomendado" atende à maioria dos casos)
- Clique em **"Gerar PDF"**
- O arquivo será baixado automaticamente com o nome:
  `LAUDO_DE_VISTORIA_{Locador}_{Locatario}.pdf`

#### 5. Salve as Fotos (opcional)
- Clique em **"Salvar Fotos"** para baixar todas as imagens (na qualidade original) em ZIP

#### 6. Importe um Laudo Anterior (opcional)
- Clique em **"Importar Vistoria"**, no topo da página
- Selecione o PDF de um laudo gerado anteriormente por este sistema
- Revise os dados preenchidos automaticamente antes de gerar um novo PDF

---

## 📁 Estrutura de Arquivos

```
sistema-vistoria/
│
├── index.html            # Arquivo principal do sistema
├── assets/
│   ├── Logo.png          # Logo da empresa no cabeçalho e no PDF
│   └── LogoBalao.ico     # Ícone exibido na aba do navegador (favicon)
├── JSON/
│   ├── estados.json      # Lista de estados brasileiros
│   └── municipios.json   # Lista de municípios por estado
└── README.md             # Este arquivo
```

---

## ⚠️ Requisitos Importantes

### Navegador
- Use navegadores atualizados (Chrome, Firefox, Edge, Safari)
- É necessário permitir acesso à câmera para captura de fotos
- É necessária conexão com a internet (bibliotecas de geração/leitura de PDF, ZIP e ícones são carregadas via CDN)

### Arquivos JSON
Os arquivos `estados.json` e `municipios.json` devem estar na pasta `JSON/` com a seguinte estrutura:

**estados.json:**
```json
[
  {"id": 29, "name": "Bahia", "abbr": "BA"},
  ...
]
```

**municipios.json:**
```json
[
  {"id": 1, "state_id": 29, "name": "Camaçari"},
  ...
]
```

---

## 🐞 Limitações Conhecidas

A função **"Importar Vistoria"** funciona apenas com PDFs gerados por este próprio sistema (o arquivo é validado antes de importar). Mesmo assim, existem pontos que ainda estão sendo ajustados:

1. Em alguns casos, um trecho do texto do **rodapé do PDF** acaba sendo inserido junto da descrição de algum card importado.
2. O texto de **"Informações Adicionais"** do PDF ainda não é recuperado de volta para o campo correspondente na página.
3. O **Estado do Imóvel** (Novo/Excelente/Bom/Regular/Ruim) ainda não é marcado automaticamente nos botões ao importar.

Por serem inerentes à forma como o texto é extraído do PDF, recomenda-se sempre **revisar os dados preenchidos** após uma importação, antes de gerar um novo PDF.
