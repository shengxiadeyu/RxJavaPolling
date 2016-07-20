#使用Retrofit和RxJava进行轮询操作
###流程图
```flow
st=>start: 开始
e=>end: 结束
op1=>operation: 查询userId
op2=>operation: 上传图片
cond=>condition: 上传成功?
op3=>operation: 开始轮询
st->op1->op2->cond->op3->e
cond(yes)->op3
cond(no)->e
```
```
new QueryUserInfo("22", "502").startQuery()
                .subscribeOn(Schedulers.io())
                .flatMap(userJsonResult -> new UpLoadImage("1218", "/storage/sdcard/Desert.jpg").upload())
                .filter(uploadJsonResult1 -> uploadJsonResult1.getFlag().equals("1"))
                .flatMap(uploadJsonResult2 -> new Polling("1218").startPolling())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(pollingJsonResult ->  Log.d("polling result", pollingJsonResult.getMessage()));
```
####轮询
每隔10秒轮询一次，轮询5次，如果返回1或者-1则停止，否则5次以后停止
```
Observable.interval(10, TimeUnit.SECONDS)
                .take(5)
                .observeOn(Schedulers.io())
                .flatMap(aLong -> service.polling(userId))
                .takeUntil(pollingJsonResult -> pollingJsonResult.getResult().equals("1") || pollingJsonResult.getResult().equals("-1"));
```
###结果
userId: 1218
userId: 1244
upload image: 发送成功!
Polling: start polling
polling result: 主人很忙，请等待。。。。！
polling result: 主人很忙，请等待。。。。！
polling result: 主人很忙，请等待。。。。！
polling result: 主人很忙，请等待。。。。！
polling result: 主人很忙，请等待。。。。！

