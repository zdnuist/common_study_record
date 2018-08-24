
##### 示例
```
Flowable.just("abc")
        .subscribeOn(Schedulers.newThread())
        .doOnNext(new Consumer<String>() {
          @Override
          public void accept(String s) throws Exception {
            Log.e("RxJava", "doOnNext():" + Thread.currentThread());
          }
        })
        .doOnNext(new Consumer<String>() {
          @Override
          public void accept(String s) throws Exception {
            Log.e("RxJava", "doOnNext(2):" + Thread.currentThread());
          }
        })
        .doOnComplete(new io.reactivex.functions.Action() {
          @Override
          public void run() throws Exception {
            Log.e("RxJava", "doOnComplete():" + Thread.currentThread());
          }
        })
        .doOnNext(new Consumer<String>() {
          @Override
          public void accept(String s) throws Exception {
            Log.e("RxJava", "doOnNext(3):" + Thread.currentThread());
          }
        })
        .observeOn(Schedulers.io())
        .doOnNext(new Consumer<String>() {
          @Override
          public void accept(String s) throws Exception {
            Log.e("RxJava", "doOnNext2():" + Thread.currentThread());
          }
        })
        .subscribe(new Consumer<String>() {
          @Override
          public void accept(String s) throws Exception {
            Log.e("RxJava", "subscribe():" + Thread.currentThread());
          }
        });
```

运行结果:
```
E/RxJava: doOnNext():Thread[RxNewThreadScheduler-1,5,main]
E/RxJava: doOnNext(2):Thread[RxNewThreadScheduler-1,5,main]
E/RxJava: doOnNext(3):Thread[RxNewThreadScheduler-1,5,main]
E/RxJava: doOnComplete():Thread[RxNewThreadScheduler-1,5,main]
E/RxJava: doOnNext2():Thread[RxCachedThreadScheduler-1,5,main]
E/RxJava: subscribe():Thread[RxCachedThreadScheduler-1,5,main]
```

要点明确:
RxJava是从上到下订阅，然后从下到上发送通知，
