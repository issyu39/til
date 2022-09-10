## ViewPager2

- ViewPagerは非推奨となっており、ViewPager2の使用が推奨されている
- ViewPagerはオンスクリーンとなるFragmentの両隣のFragmentが常に生成されるが(`offscreenPageLimit`のデフォルト値が`1`)、ViewPager2はそのFragmentがオンスクリーンとなったタイミングで初めてFragmentが生成される(`offscreenPageLimit`のデフォルト値が`-1`)
https://qiita.com/Lemon_Rgray/items/9250444ce57cb3e1a93b

### ViewPager2の特徴
- RTLレイアウトのサポート
- 縦スクロールのサポート
- 既存のViewPagerのNotifyDataSetChangedのバグ修正

### 使用するAdapter
- ViewPagerで`PagerAdapter`を使用してビューをページングしていた場合、ViewPager2では`RecyclerView.Adapter`を使用
- ViewPagerで`FragmentPagerAdapter`を使用して少数かつ固定数のフラグメントをページングしていた場合、ViewPager2では`FragmentStateAdapter`を使用
- ViewPagerで`FragmentStatePagerAdapter`を使用して大量または不明数のフラグメントをページングした場合、ViewPager2では`FragmentStateAdapter`を使用 

### オフスクリーンになったViewを破棄しないようにする   
タブ数が決まっている場合は、以下のように`offscreenPageLimit`の設定で対処する  
https://codehunter.cc/a/android/prevent-viewpager-from-destroying-off-screen-views
