# COCO Annotator

## 1.前準備

* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)



## 2.環境構築

1. COCO AnotatorをCloneします。
	```sh
	git clone https://github.com/jsbroks/coco-annotator.git
	```
2. "coco-annotator/"へ移動し、docker-composeを起動します。
	:::message
	Docker desktopが起動してから実行してください
	:::
	:::message
	mac OSがMontereyの場合、port5000をAirPlayが占有しているため、"設定"→"共有"より"AirPlayレシーバー"のチェックボックスを外して対応するか、dockerのyamlファイル内にあるportの設定を変更してください。
	:::
	:::message alert
	M1 macの動作確認はできていないです。対応していないdocker imageが含まれていて、動作しない場合があります。
	:::
	```sh
	cd coco-annotator
	docker-compose up
	```
3. [localhost](http://localhost:5000/)に接続し、COCO Annotatorのsignup画面が表示されていることを確認します。
	* 確認できましたら、適当なユーザー名とパスワードを設定し、ユーザー情報の登録を行ってください。

4. ユーザー情報登録後に、"Categories"タブを選択し、"Create"ボタンよりキーポイントの定義に進みます。

5. "Creating a Category"内で、"Name"にアノテーション対象となるオブジェクトの名称を入力し、"Supercategory"は空白のままにして各キーポイントの設定に進みます。
	* 下図のように"Keypoints"の"Label"から各キーポイントの定義を行い、"Connects to"より各キーポイントの接続について定義します。
	* 各キーポイントの定義を終えたら、"Create Category"ボタンにて使用するキーポイントを確定します。
	* COCO形式のキーポイント定義と接続については、下表参照してください。

		| Label      | Connects to | ...        | ...        | ...   | ...   |
		| ---------- | ----------- | ---------- | ---------- | ----- | ----- |
		| nose       | r_eye       | l_eye      | neck       |       |       |
		| neck       | nose        | r_shoulder | l_shoulder | r_hip | l_hip |
		| r_shoulder | neck        | r_elbow    |            |       |       |
		| r_elbow    | r_shoulder  | r_wrist    |            |       |       |
		| r_wrist    | r_elbow     |            |            |       |       |
		| l_shoulder | neck        | l_elbow    |            |       |       |
		| l_elbow    | l_shoulder  | l_wrist    |            |       |       |
		| l_wrist    | l_elbow     |            |            |       |       |
		| r_hip      | neck        | r_knee     |            |       |       |
		| r_knee     | r_hip       | r_ankle    |            |       |       |
		| r_ankle    | r_knee      |            |            |       |       |
		| l_hip      | neck        | l_knee     |            |       |       |
		| l_knee     | l_hip       | l_ankle    |            |       |       |
		| l_ankle    | l_knee      |            |            |       |       |
		| r_eye      | nose        | r_ear      |            |       |       |
		| l_eye      | nose        | l_ear      |            |       |       |
		| r_ear      | r_eye       |            |            |       |       |
		| l_ear      | l_eye       |            |            |       |       |


6. キーポイント設定後、"Dataset"タブから"Create"ボタンを押し、アノテーションデータ用のデータセット作成に進みます。

7. "Creating a dataset"画面が表示されたら、"Dataset name"にデータセット名を入力し、"Default Categories"に先ほど定義したCategoryを設定します。設定後、"/datasets/データセット名/"のディレクトリが作成されている事が確認できます。
	```txt
	coco-annotator
	|
	~
	├── datasets
	│   └── データセット名
	~
	```
8. 先ほど作成したデータセットフォルダにアノテーションしたい画像を格納します。格納後に"Datasets"画面にあるデータセットを選択し、アノテーションを実施する画面に進みます。
	
# 3.アノテーション
1. 下図のような画面が表示されたら、アノテーションする画像をCOCO Annotatorに認識させるために、左側にある"Scan"ボタンを押し、画像をscanします。
2. 画像のscanを終えたら、画面に画像一覧が表示されるため、画像を選択しアノテーションへ進みます
3. 画像が表示されたら、右側にあるCategory名の"+"を押し、キーポイントを選択します（今回はnoseから開始）。
	* 開始のキーポイントを選択したら、対応する箇所へカーソルを持っていき、クリックすることでキーポイントの設定が可能となります。
	* 上記の工程を各部位に対し行い、終了したら右上にある日付の横にある"→"を押すことで次画像へと進んでいきます。

4. 全画像に対しアノテーション終了後、"Datasets"タブを選択し、表示されるデータセットの項目にある縦三点リーダーから、"Download COCO"を選択し、モデル学習時に用いるアノテーションデータのjsonファイルが取得できます。

# 4.まとめ
今回はCOCO形式に沿ってアノテーションを行いましたが、キーポイントの定義と接続を変えることで、カスタムデータについても作成出来ますので、試してみてください。