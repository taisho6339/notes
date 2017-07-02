## MVP

## Clean Architecture

## Architecture Component

- ViewModelはActivityのライフサイクルに紐づく。
 - 完全に消え去るまではViewModelは生き残る
 - 同一Activityに紐づくFragment間で情報を共有できる

- LiveData
 - ライフサイクルに気を使ったObserver。
 - Observeする側が死んだら勝手にObserver登録がはずれる
 - Observe登録が走ったときのCallBackがある。
 - 位置情報取得にはとっても便利
 - 画面は通信に対してのCallBackではなくLiveDataへのObserveをしておき、RepositoryはLiveDataを更新すれば良いのでは。
    - キャンセルが不要になる。
 - 複数の通信をマージして返したい場合は、統合Repositoryを作る？
