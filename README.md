# epub2docx by Jair Lima

Converte um e-book EPUB em um documento Word (DOCX) no formato A5, com margens espelhadas, página de rosto, sumário dinâmico e paginação, pronto para impressão de livro físico.

## O que faz

- Lê título, autor e idioma do EPUB e respeita a ordem de leitura (spine).
- Preserva negrito, itálico, cores de fonte (inclusive o vermelho de destaque), listas, tabelas e imagens.
- Gera capa interna, sumário dinâmico e cabeçalhos e rodapés com numeração de páginas.
- Opcional: no Windows, com o Microsoft Word instalado, atualiza o sumário automaticamente.

## Instalação

```bash
pip install -r requirements.txt
```

## Uso

```bash
python epub2docx.py livro.epub livro.docx
python epub2docx.py livro.epub livro.docx --font "Garamond" --size 11 --margin-inner 2.4
```

Opções: `--font` (padrão Georgia), `--size` (10,5), `--margin-inner` (2,2 cm), `--margin-outer` (1,8 cm), `--margin-top` e `--margin-bottom` (1,8 cm). Veja `python epub2docx.py --help` para a lista completa.

## Especificação

Detalhes de arquitetura e decisões em [SPEC.md](SPEC.md).

## Licença

MIT, veja [LICENSE](LICENSE).
