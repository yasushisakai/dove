# Dove
## Consideration
- The objective of the project-based funding system instead of the specific target country
  - 受取相手のWalletのPubkeyがわからないため、自己申告制にしてWalletのPubkeyを登録してもらう
  - アイデア、ユースケース、平和に興味の中心があるため、政治信条的には中立的立場で運用したい
  - 特定の国対象のシステムを作ってもカウンターとなる敵対国の同様のシステムがすぐに作られるであろうと思われるため
  - 出資を募る人はシステムをプロモーションするモチベーションがあるため、宣伝してくれるはず
- 利用を「平和維持」に限定する意図
  - 抽象化すれば、何かが実現された（例えば「特定の国が攻撃を受ける」ということが実現された）と出資者がみなせばファンドが実施される、後払いのクラウドファンディングであるとみなせる
  - アイデア自体は他のユースケースでも利用できるため、そのようにシステムを作ることも可能
  - 利用方法を限定した方が説明がシンプルで明確になり、利用者のとっつきがよいと思われる
  - 単なるクラファンであればBlockchainでやる必要性は薄まる
- プールされているSolanaの使い道
  - 例えばプールされている金額の半分を自動的にステーキングして利益を稼ぐ
  - 稼いだ利益から一部をマージンとして受け取り、開発費・運営費に回す
  - 残りは出資者に投資比率で案分して配布
- 下記で何度か触れている「自身がその本人だと証明するためのリンク」について
  - 出資を募る人間、出資する人間、いずれもWalletのPubkeyは示せても、当人のidentityを他社が容易に確認できない
  - 例えば自身がある政府の関係者、あるいは援助物資を届けるNGO/NPOだと名乗っているとしても、それを他社が容易に確認できない
  - ひとつの方法として、ソーシャルメディアで当該のプロジェクトや出資についてポストしてもらい、それへのリンクを貼ってもらうというのは参考になり得る
  - 当該のソーシャルメディアアカウントが当人、当団体であると信用できそうであれば、ある種の裏付けになると思われる

## Architecture
- Frontend
  - Node.js
  - React
- Backend
  - Rust
  - Anchor
- Blockchain
  - Solana
  - Phantom Wallet
- Deployment
  - Replit
  - GitHub

## Account Specification
- Project
  - プロジェクトのPubkey
  - 対象国の名前
  - プロジェクト登録日
  - プロジェクト更新日
  - プロジェクト有効フラグ
  - プロジェクト削除フラグ
  - プロジェクトの説明
  - 資金受取後の利用用途
  - 動画もしくは画像へのリンク
  - 自身のソーシャルメディアアカウント・Webページなどへのリンク（10個まで）
  - 資金受け取り組織・個人（自身）の名前
  - 資金受け取りWalletのPubkey
  - 自身がその組織だと証明するためのリンク
  - これまでの合計プール金額最大値
  - これまでの合計送金済金額
- Fund
  - ファンド対象プロジェクトのPubkey
  - 接続されている（投資元）WalletのPubkey
  - 出資金額
  - 被攻撃判断閾値
  - 現在攻撃を受けていると判断するか
  - 自身のアカウント情報をプロジェクトページに表示するか否か、defaultはNo
  - 自身の投資金額をプロジェクトページに表示するか否か、defaultはNo
  - ウォッチリストへの追加、defaultはNo
- User
  - 対象WalletのPubkey
  - ユーザ名（誰かと重複していても良い）
  - メールアドレス（登録されていればプッシュで情報を配信できるようになる、画面には表示しない）
  - 自身のSNSアカウント・Webページなどへのリンク（10個まで）
  - 自身がその本人だと証明するためのリンク
  - これまで出資したプロジェクトのPubkey一覧
  - 自身の情報を公開したいか否か、defaultはNo
  - 現在の合計プール金額（自動算出）
  - これまでの合計プール金額最大値
  - これまでの合計送金済金額

## Screen Specification
- ランディングページ: タイトル、ページの説明、この企画そのものへのSNSへのリンクなどと併せて登録されているプロジェクトの一覧を表示するページ
  - プロジェクトページへの遷移ボタン、表示されている各プロジェクトのファンドページへのボタンが表示される
  - プロジェクトが増えてきた場合は、プロジェクト一覧の検索・フィルタリング機能を追加する
- プロジェクトページ: プロジェクトを登録するページ。次のパラメータを設定できる
  - 対象国の名前
  - プロジェクト登録日
  - プロジェクト更新日
  - プロジェクト有効フラグ
    - 無効のプロジェクトは画面上に表示されるものの「無効」である旨が表示され、投資や情報の変更ができなくなる
  - プロジェクト削除フラグ
    - 削除されたプロジェクトは画面上に表示されないし情報が変更できなくなる
  - プロジェクトの説明
  - 資金受取後の利用用途
  - 動画もしくは画像へのリンク
    - 動画はYoutubeを想定
  - 自身のソーシャルメディアアカウント・Webページなどへのリンク（10個まで）
  - 資金受け取り組織・個人（自身）の名前
  - 資金受け取りWalletのPubkey
  - 自身がその組織だと証明するためのリンク
    - 自身のSNSアカウントでこのプロジェクトを引用して皆に出資を呼び掛けた上で、その投稿へのダイレクトリンクの登録を推奨、プロジェクト登録後に設定可能
  - 現在の合計プール金額（自動算出）
  - これまでの合計プール金額最大値
  - これまでの合計送金済金額
  - 被攻撃判断閾値（自動算出）
    - 0～100%の数値でこの数値以上の割合の出資者が「攻撃を受けている」と判断すればプールされている資金が資金受取Walletへ送金される
    - 各出資者が妥当であると思われる閾値を入力し、出資金額による加重平均を算出して決定、パーセンテージで下2桁四捨五入（e.g. 56.32%）
  - 攻撃を受けていると判断している出資者の割合（自動算出）
    - 0～100%の数値で現在の出資者の意見に出資金額で加重平均
    - 攻撃を受けていると判断されている間は出資金額がそのまま資金受取Walletにダイレクトに送金され続ける
- ファンドページ: プロジェクトにファンドしたり、取りやめたり、出資金額、意見を変更するページ。次のパラメータを設定できる
  - 対象プロジェクトの各種情報で必要そうなものがページに表示される
  - 出資金額
  - 被攻撃判断閾値
    - プロジェクトの被攻撃判断閾値を算出するための数値、defaultは50%
  - 現在攻撃を受けていると判断するか
    - 0 or 1 でなく、パーセンテージで設定可能、誰かに判断を委任することは現状ではできない
  - 自身のアカウント情報をプロジェクトページに表示するか否か、defaultはNo
  - 自身の投資金額をプロジェクトページに表示するか否か、defaultはNo
  - ウォッチリストへの追加、defaultはNo
    - メールアドレスが登録されている場合にプッシュで情報を受け取りたいかどうか
- ユーザページ: 接続しているWalletに関する情報（便宜的にユーザ情報と呼ぶ）を閲覧・変更するページ。次のパラメータを設定できる
  - ユーザ名（誰かと重複していても良い）
  - メールアドレス（登録されていればプッシュで情報を配信できるようになる、画面には表示しない）
  - 自身のSNSアカウント・Webページなどへのリンク（10個まで）
  - 自身がその本人だと証明するためのリンク
    - 自身のSNSアカウントでこのアカウントを引用して皆に出資を呼び掛けた上で、その投稿へのダイレクトリンクの登録を推奨
  - これまで出資したプロジェクトのPubkey一覧
  - 自身の情報を公開したいか否か、defaultはNo
  - 現在の合計プール金額（自動算出）
  - これまでの合計プール金額最大値
  - これまでの合計送金済金額
  
  ## Credit
- Arjun: https://github.com/LearnWithArjun
