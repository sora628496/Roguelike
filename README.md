# 簡易ローグライク

【制作者】神尾 巧(法政大学)  
【使用言語】Python(Tkinter)  
【概要】  
ローグライクゲームの様々な要素を単純にして気軽にプレイできるようにした  

【操作方法】  
Escapeキー：ゲーム終了  

・タイトル画面  
Escape以外のキー：ゲーム開始  

・ダンジョン画面  
矢印キー：移動  
Zキー：攻撃  
Xキー：インベントリを開く  
スペースキー：足踏み  
WASDキー：向き変更(ターンを消費しない)  

・インベントリ画面  
左右矢印キー：アイテム選択カーソル移動  
Z/エンターキー：決定(アイテム使用)  
Xキー：インベントリを閉じる  

・階段を降りるか否かの選択画面  
左右矢印キー：選択  
Z/エンターキー：決定  

・ゲームオーバー画面  
Z/エンターキー：やり直し  

【仕様】  
新しい階層に入るたびにダンジョンは自動生成され、その中にプレイヤー、敵、アイテム、階段がランダムに配置される  
ダンジョンはセルベースのターン制  
プレイヤーが移動、攻撃、足踏みなどの行動をするとターンが進む  
プレイヤーが階段のマスに到達すると次の階に進むことができる  
プレイヤーは足踏みをすると何もせずにターンを進められる  
プレイヤーと敵は目の前のマスに相手がいた場合攻撃して相手にダメージを与えられる  
ターンを消費しないで向きを変えることもできるが、斜め向きはできない  

プレイヤーはHP、空腹度、攻撃力、経験値、レベルのパラメータを持っている  
プレイヤーのHPが0になるとゲームオーバー  
ゲームオーバーになるとアイテムやレベル、階層などすべてが初期化されて最初から  
空腹度は毎ターン少しずつ減っていく  
空腹度が残っていれば毎ターンHPが少しずつ回復していくが、空腹度が0になると毎ターン少しずつダメージを受け続ける  
経験値は敵を倒すともらえて、一定の値になるとレベルがあがる  
レベルが上がると最大HPと攻撃力も上がる  

敵はHPと攻撃力のパラメータを持っている  
敵のHP、攻撃力は階層が深くなる毎に上がっていく  
敵のHPが0になると敵は倒れてその階層のどこかにリスポーンする  
なお、倒れた時にアイテムを持っていればその場に落とす  

プレイヤーは10個、敵は1個までアイテムを拾うことができる  
敵はアイテムを使うことができない  
アイテムは回復薬、食料、矢、杖の4種類あり、拾うまでどのアイテムかはわからない  
回復薬：HPを一定値回復させる  
食料：空腹度を一定値回復させる  
矢：3本までまとめて持つことができて、使うと向いている方向にまっすぐ飛ばす  
　　射線上に敵がいると固定ダメージを与える  
　　遠かったり遮蔽物があったりすると当たらない  
杖：向いている方向にまっすぐ魔法を飛ばし、射線上にいる敵を別の部屋に転移させる  
　　遠かったり遮蔽物があったりすると当たらない  

ダンジョン画面では青い三角がプレイヤーを表し、赤い三角が敵を表し、黄色い丸がアイテム、黒い四角が階段を表す  
インベントリ画面ではプレイヤーの各種パラメータや所持アイテム、現在の階層を確認できる  

【細かい実装】  
ダンジョン生成：  
BSP法を用いてエリアを分割して部屋を作っている  
敵のAI：  
隣接した位置にプレイヤーがいるなら攻撃  
半径3マス以内にプレイヤーがいるなら幅優先探索でそこに向かう  
プレイヤーとの間に壁があるならランダムウォーク  
そうでなければランダムウォーク  

その他：  
各階層に敵は4体ずつ、アイテムは1/4の確率で3個、1/2の確率で4個、1/4の確率で5個生成される  
空腹度は毎ターン１ずつ減っていく  
空腹度が残っていれば毎ターンHPが１ずつ回復していく  
空腹度が0になると毎ターンHPが１ずつ減っていく  
攻撃した相手には攻撃力分のダメージを与える  
回復薬はHPを50回復、食料は空腹度を20回復、矢は相手に15ダメージ  
スポーンするとき、他のオブジェクトとは重ならないかつ部屋の中にスポーンする  
ゲームオーバー時には  
死因(敵に倒される/餓死)、到達階数、到達レベル、移動した歩数、倒した敵の数、受けたダメージ、拾ったアイテムの数  
が表示される  

【参考文献】  
ゲームシステムは主にこの論文のシステムを用いている  
高橋一幸、Temsiririrkkul Sila、池田心、ローグライクゲームの研究用ルール提案とモンテカルロ法の適用、The 22nd Game Programming Workshop 2017  

