##  jupyterをもっとインラタクティブに

jupyterはインタラクティブにコードが記述できることで人気があるエディターですが、描いたグラフやテーブルデータももっと手軽に見れないかということで調べてみました。インタラクティブというテーマですが、諸処の事情で全部gifです。笑

今回はjupyterlabで行いました。jupyterlabで行う場合は以下のコマンドを事前に実行してください。（jupyter notebookの人は一番上だけで大丈夫なはず）

  

```

pip install ipywidgets

jupyter nbextension enable --py widgetsnbextension --sys-prefix

jupyter labextension install @jupyter-widgets/jupyterlab-manager

```

  

### seabornを使ってcolormapを選ぶ

グラフを描く際に色合いにこだわりたい！でもmatplotlibやseabornのデフォルトのcolormapは微妙、、みたいな時に役立ちます。

詳しい使い方は[ここ]([https://qiita.com/SaitoTsutomu/items/c79c9973a92e1e2c77a7](https://qiita.com/SaitoTsutomu/items/c79c9973a92e1e2c77a7))に書いてあるので、もっと知りたい人はみてください。

  

```

from __future__ import print_function

from ipywidgets import interact, interactive, fixed, interact_manual,FloatSlider

import ipywidgets as widgets

import seaborn as sns

sns.choose_colorbrewer_palette('diverging');

  

def show_pal4(h_neg, h_pos, s, l, center):

sns.palplot(sns.diverging_palette(h_neg, h_pos, n=24, s=s, l=l, center=center))

interact(show_pal4, h_neg=FloatSlider(220, max=360), h_pos=FloatSlider(20, max=360),

s=FloatSlider(75, max=99), l=FloatSlider(50, max=99), center=['light', 'dark']);

```

  

![](https://github.com/jun-sato/jupyter_interactive/blob/master/colorbar.gif)

### グラフを動かす

  

これに関してはplotly,bokehなど、有名ライブラリが数多く出ています。

ただ、僕はこういうの記述量多くなるのであまり使用していません。笑

pandasのplot機能やseabornで実務上は満足しているので、自己満足の世界です(個人の感想です)。![](https://github.com/jun-sato/jupyter_interactive/blob/master/graph.gif)

#### plotly

[kaggleのツールドフランスデータセット]([https://www.kaggle.com/ralle360/historic-tour-de-france-dataset](https://www.kaggle.com/ralle360/historic-tour-de-france-dataset))

からのデータを利用しました。国別のステージ優勝者数を示しています。

  
  
![](https://github.com/jun-sato/jupyter_interactive/blob/master/plotly.gif)

### テーブルデータを動かす

  

[qgrid](https://qgrid.readthedocs.io/en/latest/)というものを使います

```

pip install qgrid

jupyter nbextension enable --py --sys-prefix qgrid

jupyter labextension install qgrid

```

pd.read_csv()でデータを読み込んでqgrid.show_grid()で囲むだけです。

なんとセルの編集もできます。編集したものは.get_changed_df()で出力できます。

```

import qgrid

import pandas as pd

data = pd.read_csv('stages_TDF.csv')

qgrid_widget = qgrid.show_grid(data, show_toolbar=True)

qgrid_widget

```

  

![](https://github.com/jun-sato/jupyter_interactive/blob/master/table.gif)

他にもインタラクティブなライブラリはいっぱいありますが、結局導入して便利かどうかはケースバイケースだと思っています。いいライブラリがあったら教えてください。