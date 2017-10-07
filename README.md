#  Minimum Viable Block Chain

### これは何か

- [Minimum Viable Block Chain](https://www.igvita.com/2014/05/05/minimum-viable-block-chain/)の日本語訳です
- Ilya Grigorikさんの許可を頂いて日本語翻訳します
    - https://twitter.com/_achiku/status/914645913754324992
- 当方翻訳ははじめてなので不備等ありましたらprください


### はじめに

暗号通貨、特にビットコインはほぼ全ての側面で注目されてきています。法制度、ガバナンス、税制、テクノロジー、プロダクトイノベーション、注目しているセクターのリストは途切れる事はありません。分散化された(peer-to-peer)電子的通貨システムの核心にあるコンセプトは、私達の多くが以前から持っているお金や金融の前提を覆すものです。

とはいえ、電子的な通貨という側面を抜きにしても、議論の余地はありますが、更に興味深く様々な分野への応用可能性を秘めたイノベーションはブロックチェーン技術です。あなたがビットコインや[派生したアルトコイン](https://en.bitcoin.it/wiki/Comparison_of_cryptocurrencies)を通貨としてまたは価値の保存方式として、どのような考えて持っていようともその裏ではみな同じ[サトシ・ナカモトが示したブロックチェーンの原理](https://bitcoin.org/bitcoin.pdf)で動作しています。

>  We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power... The network itself requires minimal structure.

ブロックチェーンはただの"通貨"に留まりません。実際に、その他様々なユースケースに応用可能であり、応用されていくものでしょう。よって、最小構成のブロックチェーンの裏にある「なぜ」や「どのように」を理解する事には価値があります。

- この先続く文章は、ビットコインに対する分析ではありません。実際、私は意図的にブロックチェーンの通貨的側面、今日のビットコインブロックチェーンが本番環境で利用している多くの追加機能に関しての記述を省いています。
- この先続く文章は、ブロックチェーンを構成する各仕組み(電子証明、proof-of-work、トランザクションブロック)が必要なのか、そしてそれらがどのように組み合わせれ最小構成のブロックチェーンを動作させているのかを、ボトムアップで、構成要素が持つ注目すべき特性と共に説明しようという試みです。

書くことは理解の深化を手助けしてくれます。なのでこの文章を書きました。自分の為に書いたものなのですが、誰かの理解の手助けにもなると良いなと思っています。フィードバック大歓迎。コメント待ってます！


### 三式簿記による取引履歴の保護

アリスとボブはスタンプ収集家です。彼らはスタンプ収集ガチ勢なわけではなく、趣味を同じくする人々と会い、お互いスタンプの話をしながらたまにスタンプ交換をするといった社会的側面に魅了された側の人たちです。スタンプ交換に関して言うと、お互いのコレクションの中でお互いが気に入ったものを見つければその場で交渉して交換するという、シンプルな物々交換システムになっています。

ある日、アリスはボブのコレクションの中になんとしても手に入れたいスタンプを見つけてしまいました。ただ困ったことに、ボブはアリスの持っているスタンプの中に特に興味のあるものを見いだせません。アリスはその事実に打ちひしがれながらも交渉を続けます。努力のかいあり、最終的にアリスとボブは一つの解決策に合意します。その解決策とは今回はボブが見返りなしでスタンプをアリスに渡し、アリスは将来ボブの気に入るスタンプを手に入れたタイミングでボブに渡す、という約束です。

ボブとアリスは前から知り合いでしたが、この約束をお互い守る為(アリスは特に)、チャックにこの取引を証明してもらうことにしました。

![](https://www.igvita.com/posts/14/xtransaction-signatures.png.pagespeed.ic.YvZjH0kZiQ.webp)

彼ら三人は上のような「ボブがアリスに紅いスタンプを渡した」という取引の証明書を複製して三人で持ち合います。ボブとアリスはお互いにこの証明書があれば彼らの取引の履歴を確認できますし、チャックはこの証明書のコピーを取引の証明として保存しておきます。単純な仕組みですが以下のようにいくつか素晴らしい特性があります。

1. チャックは知らないうちに悪い奴等が偽の取引履歴を差し込まないようにアリスとボブを承認する
2. チャックが持つ証明書のコピーの存在は取引の存在の証明になる。アリスが取引なんてなかったという主張をしてもボブはチャックのところに行き証明書を確認することでアリスの主張を退ける事ができる。
3. チャックがもし証明書のコピーを持ってない場合、取引は発生しなかった事の証明になる。これはつまりアリスもボブも取引をごまかせないという事です。彼らは自分が持つ証明書のコピーを改竄して取引相手は嘘をついていると主張する事はできるが、その場合はチャックのところに行き確認できる。

上記は三式簿記の仕組みです。ルールは単純且つ取引する二人はどちらもある程度保護されます。ただし、もう皆さんこの方式の弱点を見つけましたよね？この仕組の上では私たちは多くの信頼を仲介者(チャック)に置かなければなりません。もしチャックが他の悪いグループと組むことにした場合、このシステムは吹っ飛んでしまいます。

この話の教訓？仲介者選択にはとっても気をつけないといけません！


### 公開鍵暗号による取引履歴の保護
