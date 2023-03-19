---
title: "Chat GPTを使って開発中に出会った、良さげなプロンプト"
emoji: "📚"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [ChatGPT,Next.js]
published: true
---

Chat GPTを使って、簡単サイトを作っている時に良さげなプロンプトがありましたので紹介します。


## はじめに

簡単なサイトだと、割と正しい回答が得られると思い、
Chat GPTに聞きながらNext.jsで入力された情報を元に、markdownを生成するだけのサイトを作成しました。

![Chat GPTにブログ案を出してもらうテンプレート作成ツール](/images/gpt-website-prompts/01.png)

:::details 作成したサイトの詳細

ブログのタイトルや構成をChat GPTに聞く際に使用するプロンプトのテンプレート(markdown)を作成しました。

- ブログのテーマや、キーワードを入力
- 画面右に作成中のテンプレートを表示
- コピーボタンでクリップボードにコピー
- サンプルデータを入力することも可能

![Chat GPTにブログ案を出してもらうテンプレート作成ツール](/images/gpt-website-prompts/01.png)

[Chat GPTにブログ案を出してもらうテンプレート作成ツール](https://gpt-blog-template-generator.vercel.app/)
:::

基本的に下記のようなことを聞くと精度があがります。
- 前提を明確にする
- 出力を明確にする
- 繰り返し聞く、情報を追加する

また、下記のようなことを聞くと精度が下がります。
- 急に話題を切り替える
- やりとりを繰り返すと、前提を忘れがち

## プロジェクトの設定

```
# 指示
あなたは、開発者です。
以下の設定と入力を元に最高の結果を出してください。

前提
- ブラウザで動く
- 言語: TypeScript
- ライブラリ: Next.js
- パッケージマネージャー: yarn

上記を元にsetupしてください。
```

他のライブラリを使用した場合も、良い感じに環境構築してくれました。
ただGPT-3だとNext.js 13を考慮していないようでした。

作るシステムの詳細も追記すると良さそうです。

### tailwind

```
tailwindを導入してください。
```

設定方法を教えてくれました。
ChatGPTを使用する場合、BootstrapやTailwind使用する方が、HTMLとCSSをバラバラで質問するより良さそうです。

### lintとprettier

```
lintとprettierを導入してください。
```

設定方法やルールも設定してくれました。
あまり設定する機会がないので、毎回調べていたので助かりました。
「続けて」聞くと「Husky」と「lint-staged」の設定も提案してくれました。

### VSCodeの設定

```
VSCodeでファイルを保存時に、自動でlintとprettieを実行するVSCodeの設定ファイルの作り方
```

Chat GPTによる回答

:::message
Visual Studio Code (VSCode) で保存時に自動的に lint と Prettier を実行するように設定するには、以下の手順を実行してください。

1. VSCode の拡張機能をインストールする
  - ESLint 拡張機能 (dbaeumer.vscode-eslint) をインストールしてください。
  - Prettier 拡張機能 (esbenp.prettier-vscode) をインストールしてください。

1. プロジェクトのルートディレクトリに .vscode ディレクトリを作成し、その中に settings.json ファイルを作成します。

```bash
mkdir .vscode
touch .vscode/settings.json
```

3. 作成した settings.json ファイルに以下の設定を追加します。
```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```
この設定により、以下のことが実現されます。

- 保存時に ESLint による自動修正が実行されます。
- 保存時に Prettier によるコード整形が実行されます。
- JavaScript, TypeScript, JSX, TSX ファイルに対してデフォルトのフォーマッタとして Prettier を使用します。
- 設定ファイルを保存した後、VSCode を再起動することで、これらの設定が有効になります。これで、VSCode でファイルを保存するたびに、自動的に lint と Prettier が実行されるようになります。
:::

毎回プロジェクトを作成するときに、過去のコードを見たり、調べたりしますが、簡単な設定やプラグインの追加などは、Chat GPTに聞いてみると良さそうです。
※ もちろん、仕事などでChat GPTの回答をそのまま使う場合は気をつけてください。

## 開発中のやりとり

### コードの置き換え

````
React-Hook Formに置き換えてください。
現行のコードは下記になります。

```
~~ コードは省略 ~~
``` 
````

コードを置き換えるのには有用そうです。
ただ出力されるコードが長いと、途中で切れてしまうことがあります。


````
以下のコードにtailwindを使用して、デザインしてください。
```
const TemplateLog: React.FC<TemplateComponentProps> = ({ formData }) => {
  return (
    <div>
      <h2>作成中のテンプレート</h2>
      <p>カテゴリー: {formData.category}</p>
      <p>テーマ: {formData.theme}</p>
      <p>タグ: {formData.tags}</p>
      <p>ペルソナ: {formData.persona}</p>
    </div>
  );
};
```
````

Chat GPTによる回答
:::message
デザインをシンプルかつ見やすくするために、以下のようにTemplateLogコンポーネントを変更します。
```tsx
const TemplateLog: React.FC<TemplateComponentProps> = ({ formData }) => {
  return (
    <div className="bg-gray-100 p-6 rounded-lg shadow-sm">
      <h2 className="text-2xl font-bold mb-4">作成中のテンプレート</h2>
      <p className="text-lg mb-2">
        <span className="font-semibold">カテゴリー:</span> {formData.category}
      </p>
      <p className="text-lg mb-2">
        <span className="font-semibold">テーマ:</span> {formData.theme}
      </p>
      <p className="text-lg mb-2">
        <span className="font-semibold">タグ:</span> {formData.tags}
      </p>
      <p className="text-lg mb-2">
        <span className="font-semibold">ペルソナ:</span> {formData.persona}
      </p>
    </div>
  );
};
```

この変更により、テンプレートログは以下のようになります。

- 背景色がグレーで、周囲にパディングが追加され、角が丸くなります。
- タイトルの文字サイズが大きくなり、フォントが太くなります。
- 各項目のラベルが太字になり、各行の間隔が適切に調整されます。
:::

適当にstyleを当ててくれるのは便利です。
詳細なデザインの条件を与えると、より良い回答が得られました。

### エラーの解決

```
TypeError: createContext only works in Client Components. Add the "use client" directive at the top of the file to use it. Read more: https://nextjs.org/docs/messages/context-in-server-component
```

Chat GPTによる回答

:::message
このエラーは、Next.js のサーバーコンポーネント内で createContext や useContext が使われている場合に発生します。サーバーコンポーネントでは、これらの React のコンテキスト API を使用することはできません。

この問題を解決するために、以下の手順を実行してください。

1. エラーが発生しているファイルを開きます。
2. ファイルの先頭に // @client ディレクティブを追加して、そのコンポーネントがクライアントコンポーネントであることを指定します。
例えば、次のようになります。

```tsx
// @client
import React, { createContext, useContext } from 'react';

// 以降のコード...
```

// @client ディレクティブを追加することで、Next.js はそのファイル内でクライアント側の機能を使用できるようになります。ただし、この変更により、そのコンポーネントはクライアント側でのみ実行されるため、サーバー側で実行されるべき処理がある場合は注意が必要です。

また、エラーが発生しているコンポーネントが実際にサーバーコンポーネントである場合は、コンポーネントを分割してクライアント側とサーバー側の機能を別々のコンポーネントに移動することを検討してください。
:::

このエラー自体は、エラー文通りなのですが、
エラー文を貼り付けるだけで、日本語で解説してくれるのでググるよりは早そうです。
また、過去のやりとりを踏まえて回答してくれるので良い感じです。

解決しない場合は、「改善されませんでした」と続けると、回答が改善される場合もありますが、
その前にエラーが発生する、条件などを追記する方が良さそうです。

また、こちらから解決方法を提案する方が早い場合もあります。

```
watchを使う必要があるのでは？
```

Chat GPTによる回答
:::message
はい、その通りです。react-hook-formのwatch関数を使って、入力内容をリアルタイムで監視し、formDataに反映させることができます。以下のようにTemplateFormコンポーネントとPageコンポーネントを変更してください。

まず、TemplateFormコンポーネントにonChangeイベントハンドラを追加します。

```jsx
type TemplateFormProps = {
  onSubmit: (data: TemplateFormData) => void;
  onChange: (data: TemplateFormData) => void;
};

const TemplateForm: React.FC<TemplateFormProps> = ({ onSubmit, onChange }) => {
  const { register, handleSubmit, watch } = useForm<TemplateFormData>();
  const watchedValues = watch();

  useEffect(() => {
    onChange(watchedValues);
  }, [watchedValues, onChange]);

  // ...
};
```

次に、PageコンポーネントでonChangeイベントハンドラを定義し、TemplateFormコンポーネントに渡します。

```jsx
const Page: NextPage = () => {
  const [formData, setFormData] = useState<TemplateFormData>({
    category: '',
    theme: '',
    tags: '',
    persona: '',
  });

  const handleSubmit = (data: TemplateFormData) => {
    setFormData(data);
    alert('Submitted values: ' + JSON.stringify(data));
  };

  const handleChange = (data: TemplateFormData) => {
    setFormData(data);
  };

  return (
    <div className="flex bg-gray-100 py-6 sm:py-12">
      <div className="mx-auto w-full max-w-5xl">
        <div className="flex bg-white px-4 py-10 shadow-lg sm:rounded-3xl sm:p-20">
          <div className="flex-1">
            <TemplateForm onSubmit={handleSubmit} onChange={handleChange} />
          </div>
          <div className="flex-1">
            <TemplateLog formData={formData} />
          </div>
        </div>
      </div>
    </div>
  );
};
```

これにより、TemplateLogに入力内容がリアルタイムで表示されるようになります。
:::


### 新しく機能を実装する際に、前提を改めて伝える

```
前提

貴方はプロのフロントエンドエンジニアです。
ブログの記事の案をChat GPTに聞く際に、ポストするテンプレートを作成するツールを作ります。

システムの流れは以下の通りです。

1. ユーザーがブログのカテゴリー、テーマ、タグ、ペルソナを設定します。
2. ユーザーが過去に書いた記事のタイトルを登録します。
3. ユーザーが新しい記事の内容について、簡単な箇条書きを入力します。
4. 1~3の情報を元に、テンプレートを作成。
5. ボタンを押すと、クリップボードにコピーされる。

画面構成は以下の通りです。

1. 画面左はブログのカテゴリー、テーマ、タグ、ペルソナを設定する画面
2. 画面右は生成中のテンプレートを表示する画面
3. 画面下にボタンを配置

webアプリケーションです。
使う技術はtypescript、nextです。

このタスクで最高の結果を出すために、もっと情報が必要な場合は、質問してください。

次のタスク機能は実装です。
よろしく。
```

Chat GPTによる回答

:::message
次のタスクは、ユーザーが過去に書いた記事のタイトルを登録する機能を実装しましょう。

まず、ステートフックを使用して、登録された記事タイトルを保持するためのステートを作成します。

```tsx
import { useState } from 'react';

// ... その他のコード ...

const [titles, setTitles] = useState<string[]>([]);
```

次に、過去の記事タイトルを追加できるフォームを作成し、追加ボタンが押されたときにステートを更新する関数を定義します。

```tsx
// タイトルを追加する関数
const addTitle = (newTitle: string) => {
  setTitles([...titles, newTitle]);
};

// ... その他のコード ...

// タイトル入力フォーム
<div className="mb-4">
  <label className="block text-gray-700">過去の記事タイトル</label>
  <input
    className="focus:border-light-blue-500 focus:ring-light-blue-500 mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-1 text-gray-900 placeholder-gray-500"
    type="text"
    value={titleInput}
    onChange={(e) => setTitleInput(e.target.value)}
  />
  <button
    className="... 任意のボタンスタイル ..."
    onClick={() => {
      addTitle(titleInput);
      setTitleInput('');
    }}
  >
    追加
  </button>
</div>
```

最後に、登録された記事タイトルを表示するリストを作成します。

```tsx
// 登録された記事タイトルのリスト
<div className="mb-4">
  <label className="block text-gray-700">登録済みの記事タイトル</label>
  <ul>
    {titles.map((title, index) => (
      <li key={index}>{title}</li>
    ))}
  </ul>
</div>
```

これで、ユーザーが過去に書いた記事のタイトルを登録し、登録されたタイトルがリストとして表示される機能が実装されました。続いて、「新しい記事の内容について、簡単な箇条書きを入力する」機能を実装していきましょう。
:::

やりとりを続けているうちに前提が忘れられるので、改めて伝えると良さそうです。

### ファイル分割

````
```
~~ コードは省略 ~~
```
上記のコードをatomic designに沿って、ファイルを分割してください。
````

Chat GPTによる回答
:::message
Atomic Design に従って、TemplateLog コンポーネントを分割するには、以下のようにコンポーネントを作成します。

1. atoms
  - Label.tsx: ラベルを表示するコンポーネント
  - TemplateText.tsx: テンプレートのテキストを表示するコンポーネント
2. molecules
  -  TemplateItem.tsx: ラベルとテキストをまとめたアイテムを表示するコンポーネント
3. organisms
  - TemplateLog.tsx: 分割されたコンポーネントを組み合わせて、最終的な TemplateLog コンポーネントを作成します。

以下は、各コンポーネントのコード例です。

Label.tsx (atoms)
```tsx
~~ コードは省略 ~~
```

TemplateText.tsx (atoms)
```tsx
~~ コードは省略 ~~
```

TemplateItem.tsx (molecules)
```tsx
~~ コードは省略 ~~
```

TemplateLog.tsx (organisms)
```tsx
~~ コードは省略 ~~
```

これで、TemplateLog コンポーネントが Atomic Design に従って分割されました。
:::

個人的にはじめにざっくり作ってから、細かく分割することが多いので、助かりました。
「コンポーネントに分割してください」なども、良さそうでした。

### テストデータの作成

````
```
/**
 * TemplateFormData は、ブログ提案のための入力情報を表します。
 *
 * @property blogTheme - ブログのテーマ
 * @property targetAudience - ターゲットオーディエンス
 * @property blogStyle - ブログのスタイル
 * @property existingCategories - 既存のカテゴリー
 * @property popularArticles - 人気のある記事
 * @property recentTitles - 最近の記事のタイトル
 * @property desiredKeywords - 希望するキーワード
 * @property tone - コンテンツのトーン
 * @property additionalNotes - 追加のメモやコメント
 */
export type TemplateFormData = {
  blogTheme: string;
  targetAudience: string;
  blogStyle: string;
  existingCategories: string[];
  popularArticles: string[];
  recentTitles: string[];
  desiredKeywords: string[];
  tone: string;
  additionalNotes: string;
};
```

上記の型を元に、テストデータを作成してください。
````

Chat GPTによる回答
:::message
export const testData: TemplateFormData[] = [
  {
    blogTheme: 'レトロゲーム',
    targetAudience: 'ゲーム愛好家',
    blogStyle: 'エンターテイメント',
    existingCategories: ['アーケード', 'コンソール', '名作'],
    popularArticles: [
      '80年代のアーケードゲームベスト10',
      '懐かしのファミコン名作ゲーム',
      'セガサターンの伝説的なゲーム',
    ],
    recentTitles: ['スーパーマリオの進化', 'メタルギアシリーズの秘密'],
    desiredKeywords: ['ファミコン', 'メガドライブ', 'ゲームボーイ'],
    tone: 'フレンドリー',
    additionalNotes: '',
  },
  ~~ 省略 ~~
:::

テストデータを作成するのはかなり得意でした。
ある程度条件を与えれば、パターンの違うデータが作成できます。

### データ処理

````
1. 
```
~~ コードは省略 ~~
```

2. 
```
~~ コードは省略 ~~
```

1のコードを2のmarkdownの形式で表示してください。
他のファイルでも使用できるように、データを処理する関数を作成し使用してください。
配列はリストで表示してください。
````

Chat GPTによる回答
:::message
まず、入力データを上記の形式に変換する関数を作成します。この関数は、formDataを引数として受け取り、指定された形式のテキストを返します。

```tsx
function convertFormDataToText(formData) {
  const existingCategories = (formData.existingCategories || [])
    .filter((category) => category.trim() !== '')
    .map((category) => `  - ${category}`)
    .join('\n');
~~ 省略 ~~
}
```
:::

簡単な処理なら問題なさそうです。

### その他

他にも、下記を使用しました。

- 全てのinputにプレースホルダーを設定してください → テストデータを元に、プレースホルダーを設定してくれた
- 追加したデータをformにしてください → 複数のファイルに跨って、更新してくれた
- テストデータを元に上記の関数を実行してください
- headにSEOも考慮してタグを追加してください
- プライバシーポリシーを作成してください。GAを使用しているので、GAのことも考慮してください
- MITライセンスを作成してください


### おしゃれにはならない

````
あなたは新進気鋭 web デザイナーです。

以下を元に、web サイトのデザインを変更してください

- おしゃれなデザインにしてください
- フラットデザインを採用してください
- 薄い色合いを採用してください
- 背景は黒にしてください
- tailwind を使用してください

以下コードをデザインしてください。
```
~~ コードは省略 ~~
```
````

tailwindは使用してくれますが、オシャレにはなりませんでした。
また、背景色などを考慮しないので、ボタンやテキストの色が背景と同化することもありました。
下記は何度も試して、マシになりましたが、具体的に伝えるのが良さそうです。

参考画像 ※実際に作成したサイトは人力でstyleを調整しています。

![](/images/gpt-website-prompts/02.png)

## プログラミングを勉強し始めた人に良さげ

ちょうど独学でプログラミングを勉強し始めた人に、仕事を願いしつつメンターのようなことをしています。
「何度もつまづいている間に、やめたくなるかも」と考えていましたが、気軽に聞けるので、Github Copilotと合わせて使用してもらっています。

Github Copilotとは違い、プロンプトを作成する必要があるので、手間がかかりますが、
部分的にお願いする場合は、かなり良さそうです。
ケースに合わせて、テンプレートを作成しておくと便利そうです。
※ GPT-3だと過去のプロンプトを参考にしてくれるケースが明らかに減るので注意です。

ChatGPTはたまに、応援してくれるのでこちらもお礼を言うと性能が上がります。

### 使用したプロンプト

#### コードのに機能を追加
※ *どれか1つを使う

````jsx
# 指示書

## code
```
* ここにコード
```

## 設定
*「code」に以下の対応を行ってください。
*「code」の以下の不具合を修正してください。
- xxx
- xxx
````

#### Aのコードを元にBのコードを修正

※ *どれか1つを使う
※「code A = HTML・code B = JavaScript」と置き換えても良い

````jsx
# 指示書

## code A
```
* ここにコード
```

## code B
```
* ここにコード
```

## 設定
*「code A」を参考に「code B」を修正してください。
*「code A」を参考に「code B」にxxxを追加してください。
````

#### シンプルな指示
※ *どれか1つを使う

````jsx
# 指示書

## code
```
* ここにコード
```

## 設定
*「code」を仮のデータで実行してください。
*「code」にコメントを追記してください。
*「code」をリファクタリングできる箇所を教えてください。
*「code」をリファクタリングしてください。
*「code」をファイル分割してください。
````
