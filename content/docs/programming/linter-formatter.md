# Linterとは
- コードの間違いを指摘するもの.

# Formatterとは
- コードのスタイルを統一/調整するもの.

## Prettier
- JavaScript(including ES2017), JSX, Angular, Vue, Flow, TypeScrip,t CSS, Less, SCSS, HTML, JSON, GraphQL, Markdown,  GFM, MDX, YAMLをフォーマットしてくれる.
- **GitHubリポジトリ:** [prettier/prettier](https://github.com/prettier/prettier)

### vimでの使い方
1. [こちら](https://github.com/solareenlo/vim-config/blob/master/.vimrc)の.vimrcの様に[vim-prettier](https://github.com/prettier/vim-prettier)を設定する.
2. 当該のディレクトリで,

    ```bash
    yarn add prettier --dev --exact
    ```
3. コードを開いて, コードの一番上に`// @format`を記入して,

    ```bash
    :w
    ```
で, 自動でコードをフォーマットしてくれる.
4. **Reference:** [prettier/vim-prettier](https://github.com/prettier/vim-prettier)
