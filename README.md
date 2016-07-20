#ʹ��Retrofit��RxJava������ѯ����
###����ͼ
```flow
st=>start: ��ʼ
e=>end: ����
op1=>operation: ��ѯuserId
op2=>operation: �ϴ�ͼƬ
cond=>condition: �ϴ��ɹ�?
op3=>operation: ��ʼ��ѯ
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
####��ѯ
ÿ��10����ѯһ�Σ���ѯ5�Σ��������1����-1��ֹͣ������5���Ժ�ֹͣ
```
Observable.interval(10, TimeUnit.SECONDS)
                .take(5)
                .observeOn(Schedulers.io())
                .flatMap(aLong -> service.polling(userId))
                .takeUntil(pollingJsonResult -> pollingJsonResult.getResult().equals("1") || pollingJsonResult.getResult().equals("-1"));
```
###���
userId: 1218
userId: 1244
upload image: ���ͳɹ�!
Polling: start polling
polling result: ���˺�æ����ȴ�����������
polling result: ���˺�æ����ȴ�����������
polling result: ���˺�æ����ȴ�����������
polling result: ���˺�æ����ȴ�����������
polling result: ���˺�æ����ȴ�����������
