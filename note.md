# メモ

## 戦闘

### 複数戦

単体攻撃で「誰を攻撃するか」を選択する手間が増えるので UX が悪い．
たとえば 亡霊の 4 体の群れ なら `attack.potency * Math.ceil(4 * currentHP / maxHP)` などとして，enemy 自体に troop 属性を持たせればよい．
AoE は troop に対し大きなダメージを与えるようにすることもできる．

ただこれは「アンデッドに聖属性が効く」といった特攻と何ら変わらない．特攻をゲームに持ち込むべきかは悩ましい．
たとえば PoE は AoE が基本であって，完成されたビルドでは単体であってもブーストした上記のスキルで戦うことが多い．
それならばこのゲームでは最初から AoE など持ち出さないほうがよいのかもしれない（`Increased Area of Effect` などで深みを出すのが難しいため）．
あるいは，AoE も magic 属性などと同様の属性として扱い，AoE 耐性を enemy ごとに設定するのがよいだろう．

### 属性

不要かもしれない．ローグライクで `increase 30% magic damage` といったアビリティノードは切り捨てて `Call auto attack when you guard. The potency of auto attack is 30% of your primary skill` のような特殊なアビリティだけで構成したほうが面白いだろう．
この場合，特殊な属性のスキルを活かすためにオリジナルのビルドを組む楽しみはなくなるが，それは主眼が属性でなくなった，というだけで，
発生のメカニズムを調整してやれば解決するかもしれない．

たとえば primary skill に `activate secondary skill if ultimate skill in in recharge` という効果があれば，最速で ult を発動し，かつ長いクールダウンを持たせるためのビルドをしようとするだろう．発動が楽なものは cd も短い傾向にあるので，cd を伸ばすがメリットももたらすパッシブと組み合わせればよいし，あるいは強力な ult の発動を早め，長い再起動の間を上記の primary スキルで補填する使い方もできるだろう．あるいは secondary skill 自体が，発動時に ult の cd を伸ばすが比較的強力なものか，パッシブでそういった効果を持たされたのかもしれない．属性倍率の倍々ゲームではなく，発動に軸を置いたゲームにしたい．

またスマホで遊べるような画面サイズにすると考えると，スキルが属性まで持ってしまうと画面を圧迫する，という問題がある．

### スキル

attack, block といった属性で分けるのではなく，発動条件にしたがって分けたほうがいいだろう．attack かつ block の実現も容易になる．

- category: [primary, secondary, ultimate]
- attackPotency
- blockPotency
- recharge
- callback
- text
- icon

スキルが持つべき情報は以上であり，slay the spire を参考にすれば，かなりスマートに表示できるだろう．

### BattleManager は必要か

本来はバックエンドに渡すような情報なのだから，その責務を全うするため BattleManager のようなクラスを作ればよい，と思っていた．
ただ今回はエネミー 1 体とプレイヤー 1 体の戦いなのだから，Manager のような巨大なクラスに隠蔽しなくとも，敵の行動は Enemies.tsx に，味方の行動は Character.tsx に明覚に分かれており，サイクル進行も相手が発火し終えたかどうかで十分に回る．

よって BattleManager のような抽象的なクラスを作るよりは，changeTurn() のような関数を都度用意して，関数自身を Manager のようなクラスに隠さずに`battle/interface/index.ts` からまとめて export してしまうほうがよいだろう．

### MP

表示は六花にする。かっこいい。

### バフやデバフ

汎用バフやデバフよりは、アイテム固有のパッシブ効果が発動する、といった形式のほうがよさそう。
アイコンも「再生アイコン」とかより「氷樹の葉冠」みたいな固有アイコンにしたほうがテンションあがる。

ただまったく不要かというとそうでもなくて、たとえば「凍傷」「凍結」「霧隠れ」のように複数のスキルで同様のメカニクスを共有するならば欲しい。「霧隠れ中ならばダメージ＋３」とかね。