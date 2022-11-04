source/***/indexやsource/***/treeのようなフォルダについて（YYDファイル）<br><br>
indexファイル：<br>
・リソースの順番を示す<br>
・行でindexを判定（sprite234の234がindex、sprite234は234行目に記述される）<br>
・空白も意味がある（空白を削除すれば、パフォーマンスがおそらく向上される）<br>
・spriteを作るときにsprite1900とかなるのは、空白分の空indexデータが存在するため<br><br>

treeファイル：<br>
・リソースのフォルダ構成を示す<br>
・＋はフォルダを示す<br>
・例<br>
|sprController<br>
+System<br>
	+User Interface<br>
		|sprFileBorder<br>
		|sprGameOver<br>
	+Saves<br>
		|sprSaveMedium<br>
		|sprSaveHard<br>
		|sprSaveVeryHard<br>
		|sprSaveVeryHardEditor<br>
