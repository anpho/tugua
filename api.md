Dapenti API inspect
===================

## WEBAPI ENDPOINT

```
public class WebAPI {
    public static final String ADD_FAVORITES_URL = "http://appb.dapenti.com/index.php?s=/Home/api/add_favorites";
    public static final String AD_URL = "http://appb.dapenti.com/index.php?s=/Home/api/loading_pic";
    public static final String CONVERT2_ENML_URL = "http://appb.dapenti.com/index.php?s=/Home/evernote/";
    public static final String DEL_FAVORITES_URL = "http://appb.dapenti.com/index.php?s=/Home/api/delete_favorites";
    public static final String DUANZI_LIST_URL = "http://appb.dapenti.com/index.php?s=/Home/api/duanzi";
    public static final String FAVORITES_URL = "http://appb.dapenti.com/index.php?s=/Home/api/favorites";
    private static final String HOST = "http://appb.dapenti.com/index.php?s=/Home";
    private static final String HOST_RELEASE = "http://appb.dapenti.com";
    public static final String LEHUO_LIST_URL = "http://appb.dapenti.com/index.php?s=/Home/api/lehuo";
    public static final String LOGIN_URL = "http://appb.dapenti.com/index.php?s=/Home/api/login";
    public static final String TUGUA_LIST_URL = "http://appb.dapenti.com/index.php?s=/Home/api/tugua";
    public static final String UPDATE_URL = "http://appb.dapenti.com/index.php?s=/Home/api/upgrade.html";
    public static final String YITU_LIST_URL = "http://appb.dapenti.com/index.php?s=/Home/api/yitu";
}
```

## API CALL STACK

```
private void getData(final int p) {
        RequestParams params = new RequestParams();
        params.put("p", new StringBuilder(String.valueOf(p)).toString());
        params.put("limit", "30");
        MyHttpClient.getInstance(this.context).get(WebAPI.TUGUA_LIST_URL, params, new MyAsyncHttpResponseHandler(new LoadDatahandler() {
            public void onSuccess(String data) {
                super.onSuccess(data);
                Timber.m1078i("图卦：" + data, new Object[0]);
                Base base = (Base) JSON.parseObject(data, Base.class);
                if (base.getError() == 0) {
                    List<Article> dataList = JSON.parseArray(base.getData(), Article.class);
                    if (!dataList.isEmpty()) {
                        if (p == 1) {
                            TuGuaChildLVFragment.this.articles = dataList;
                        } else {
                            TuGuaChildLVFragment.this.articles.addAll(JSON.parseArray(base.getData(), Article.class));
                        }
                        TuGuaChildLVFragment.this.adapter.setArticles(TuGuaChildLVFragment.this.articles);
                        TuGuaChildLVFragment.this.adapter.notifyDataSetChanged();
                        TuGuaChildLVFragment.this.pager = p;
                        return;
                    } else if (p == 1) {
                        TuGuaChildLVFragment.this.context.showToast("没有内容");
                        return;
                    } else {
                        TuGuaChildLVFragment.this.context.showToast(TuGuaChildLVFragment.this.getString(C0358R.string.toast_no_more_label));
                        return;
                    }
                }
                TuGuaChildLVFragment.this.context.showToast("网络数据错误");
            }

            public void onFinish() {
                super.onFinish();
                if (TuGuaChildLVFragment.this.state == 1) {
                    TuGuaChildLVFragment.this.mListView.onRefreshComplete();
                } else if (TuGuaChildLVFragment.this.state == 2) {
                    TuGuaChildLVFragment.this.mListView.onLoadMoreComplete();
                }
                if (TuGuaChildLVFragment.this.articles.isEmpty() && TuGuaChildLVFragment.this.mListView != null && TuGuaChildLVFragment.this.mListView.getFooterView() != null) {
                    ((TextView) TuGuaChildLVFragment.this.mListView.getFooterView().findViewById(C0358R.id.load_more)).setText("没有图卦内容~点击获取");
                }
            }

            public void onFailure(String error, String message) {
                super.onFailure(error, message);
                TuGuaChildLVFragment.this.context.showToast("加载失败：" + message);
            }
        }));
    }

```

## APP BOOT IMAGE 

curl "https://appb.dapenti.com/index.php?s=/Home/api/loading_pic"

```
{"msg":"","error":0,"data":"http:\/\/wx2.sinaimg.cn\/mw690\/6283e751gy1fjvxl3mkc3j20ci0m8t93.jpg"}
```

## Le Huo

curl "http://appb.dapenti.com/index.php?s=/Home/api/lehuo" -d "p=1&limit=50"

## Tu Gua

curl "http://appb.dapenti.com/index.php?s=/Home/api/tugua" -d "p=1&limit=10"