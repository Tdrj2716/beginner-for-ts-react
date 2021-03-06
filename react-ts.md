# VSCode + TypeScript で React を書くための備忘録

## 始めに

React による UI 開発を初めて行う人に向けて

1. JavaScript/TypeScript を書く上で必要になる環境の構築
2. TypeScript で React のプロジェクトを作成する方法
3. TypeScript で React を書くときの基本（まだ）
4. ESLint + Prettier によってコードスタイルを保つための環境の構築（2021/2/19 時点）

についてメモしたものです．特に 4 は複雑で，手順を飲み込んだ上で実際に手元に環境を構築するのにかなり苦労しました．訂正箇所があればコメントお願いします．

## 1. Node.js と yarn を使えるようにする

React などを使って JavaScript/TypeScript で UI の構築を行うためには，まず JavaScript 実行環境である Node.js をインストールする必要があります．使用 PC ごとに下の記事を参考にして Node.js の環境を構築しましょう．

Mac の場合：[Mac に Node.js をインストール](https://qiita.com/kyosuke5_20/items/c5f68fc9d89b84c0df09)

Windows の場合：[windows10 に Node.js をインストールする](https://qiita.com/Masayuki-M/items/840a997a824e18f576d8)

Node.js をインストールした際にパッケージマネージャーである npm も PC にインストールできているはずです（ターミナルに` npm --version` と入力すればバージョンと共に入っていることが確認できます）．最近では npm と互換性があり高速に動作する yarn というパッケージマネージャーも出てきているため，今回はこちらも合わせて `npm --global install yarn` でインストールします．

参考：[npm と yarn のコマンド早見表](https://qiita.com/rubytomato@github/items/1696530bb9fd59aa28d8)

## 2. TypeScript によるプロジェクト用のフォルダを作る

大規模なプログラムを書く際，生の JavaScript ではなく TypeScript を使うことによって型チェックや（ VSCode による）入力補完といった恩恵を得られます．結果として，コードの可読性や開発の効率を上げることが可能です．そのため，ここからは TypeScript で React の開発を行うための方法について紹介します．

例として，カレントディレクトリに ts-react-practice という名前で React のプロジェクトフォルダを作成することを考えます．下のように `--template typescript` を加えることで TypeScript による React のプロジェクトが作成できます（加えなかった場合は JavaScript によるプロジェクトが作成されます）．

```zsh
npx create-react-app ts-react-practice --template typescript
```

成功すればターミナルで次のような表示が出るはずです．

```zsh
We suggest that you begin by typing:

  cd ts-react-practice
  yarn start

Happy hacking!
```

画面通りに入力するとブラウザでこのように実行されます．
![fig1](./react-screen.png)

## 3. TypeScript で React のプログラムを書く際の基本

_この章は書きかけです_

## 4. ESLint + Prettier によるコードの作成環境を構築する

上までの環境構築だけでも TypeScript による React のプロジェクト作成は可能です．しかし，複数人でコードを書く際はコードの書式を統一することで作業効率の向上を図る必要が出てきます．
ここでは JavaScript/TypeScript の linter（コードの静的解析を行い問題部分をチェックするツール）である ESLint と，フォーマッタ（コードの整形を設定に沿って自動で行うツール）である Prettier を導入して環境構築を行います．

日本語で書かれた各種 Web サイトだけでなく公式のドキュメントも参照する方が良い（サイトによって情報が曖昧だったり，そもそも情報の移り変わりが激しいため，かつて取られた方法が非推奨になっているケースがあるから）です．調べた中で参考になると感じた記事を以下に示します．

- [ESLint と Prettier の共存設定とその根拠について](https://blog.ojisan.io/eslint-prettier)：

  設定について悩んでいたとき非常に参考になりました．

- [VS Code に Prettier・ESLint・Stylelint を導入してファイル保存時にコードを自動整形させる方法](https://wemo.tech/3307)：

  今回 VSCode での環境構築を行う際に参考にしました．

- 公式のドキュメント
  - [ESLint: Getting Started with ESLint](https://eslint.org/docs/user-guide/getting-started)
  - [Prettier: Install](https://prettier.io/docs/en/install.html)

### ESLint の導入

1. ESLint 本体について

   通常は `yarn add eslint --dev` によってインストールしますが，今回は `create-react-app` でプロジェクトを作成した際に ESLint が導入されています． `eslint -v` or `eslint --version` でバージョンが確認できますが，

   ```zsh
   npm ls eslint
   ```

   とするとプロジェクトフォルダ内の `node_modules` に ESLint があることが確認できます．

1. VSCode との連携

   - 拡張機能の追加

     VSCode の拡張機能である「 [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 」を追加します．このプラグインによって， VSCode の「設定」から ESLint に関する設定が使えるようになります．

   - VSCode の「設定」について

     `ESLint: enable` の項目がチェックされていることを確認します．また ESLint の自動修正を保存時に自動で適用するために， `settings.json` の編集から `editor.codeActionsOnSave` を下のように設定します．[^formatonsave]

     ```json
     "editor.codeActionsOnSave": {
         "source.fixAll.eslint": true
     },
     ```

     [^formatonsave]:
         似た設定で `editor.formatOnSave` というものもありますが，ファイル保存時の ESLint の実行は`editor.codeActionsOnSave` で設定することが推奨されています（引用は VSCode ESLint Extention の `eslint.format.enable` より）．

         > Although you can also use the formatter on save using the setting `editor.formatOnSave` it is recommended to use the `editor.codeActionsOnSave` feature since it allows for better configurability.

1. ESLint の設定

   VSCode のコマンドパレットを開き `ESLint: Create ESLint Configuration` を実行します（ `node_modules/.bin/eslint --init` を実行するのと同じ）．次のように入力していくと `.eslintrc.js` と `package.json` が作成されます．

   ![fig1](./lintrc-screen.jpeg)

   特に気をつけなければならないのは

   - Which framework does your project use?:
     今回は **`React`** を選択（ `Vue` もある）．
   - Does your project use TypeScript?: **Yes**
   - What format do you want your config file to be in?:
     `.json` の形式で設定することもできますが，今回は `.js` の場合を説明します．
   - Would you like to install them now with npm?: **No**

   入力を終えるとプロジェクトフォルダの中に `.eslintrc.js` が作成されるので，以下のようになっているか確認します．

   ```javascript
   module.exports = {
   	env: {
   		browser: true,
   		es2021: true,
   	},
   	extends: [
   		"eslint:recommended",
   		"plugin:react/recommended",
   		"plugin:@typescript-eslint/recommended",
   	],
   	parser: "@typescript-eslint/parser",
   	parserOptions: {
   		ecmaFeatures: {
   			jsx: true,
   		},
   		ecmaVersion: 12,
   		sourceType: "module",
   	},
   	plugins: ["react", "@typescript-eslint"],
   	rules: {},
   };
   ```

   試しに `"rules"` の部分を

   ```javascript
   {
       "rules": {
        quotes: ['error', 'double'],
      }
   }
   ```

   として，ダブルクオーテーションでないとエラーが出るように設定してみましょう．写真のようになれば， ESLint が動いていることが確認できるはずです．

   ![fig](lint-check.jpeg)

   また ESLint の設定は `.eslintrc.js` でなされているので，`package.json` の `eslintConfig` の設定は削除します（残しておいた場合も `.eslintrc.js` の方の設定が優先されます）．

   ```javascript
   {
     ...,
     "eslintConfig": {  //削除
       "extends": "react-app"
     },
     ...,
   }
   ```

### Prettier の導入

1. Prettier をインストールする
   ```zsh
   yarn add --dev --exact prettier
   ```
1. VSCode の拡張機能「 [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 」を追加する
1. Prettier の設定ファイルである `.prettirrc` を作成・記述する

   例を示します．

   ```javascript
   {
     "printWidth": 100,
     "trailingComma": "es5"
   }
   ```

### ESLint と Prettier の連携

1. VSCode の設定
   `settings.json` にて以下のように設定します．

```json
{
	"[typescript]": {
		"editor.defaultFormatter": "esbenp.prettier-vscode" // formatter: prettier
	},
	"editor.formatOnSave": true, // format by prettier when saving a file
	"editor.codeActionsOnSave": {
		"source.fixAll.eslint": true // execute ESLint when saving a file
	}
}
```

2. `eslint-config-prettier` のインストール

   ```zsh
   yarn add --dev eslint-config-prettier
   ```

3. `eslint-plugin-prettier` の適用
   `.eslintrc.js` の `"extends"` を以下のように書き加えて保存します．

   ```javascript
   {
     "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "prettier",
        "plugin:@typescript-eslint/eslint-recommended"
    ],
   }
   ```

4. 動作確認
   `app.tsx` に適当にスペースを加えたり，セミコロンを抜いたりしてフォーマットを乱してみましょう．保存した際に自動整形されれば ESLint と Prettier がう置いていることが確認できます．
