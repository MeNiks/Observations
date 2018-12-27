# Working of doonnext / map / create / just of observables

# Observation 1
```
Observable.create<String> { emitter ->
    logInfo("yep", "Observable.create  : " + getThreadName())
    emitter.onNext(getThreadName())
}
.switchMap {
    logInfo("yep", "Switch map Out : " + getThreadName())
    Observable.create<String> { emitter ->
        emitter.onNext(getThreadName())
        logInfo("yep", "Switch map create : " + getThreadName())
    }
}.switchMap {
    logInfo("yep", "Switch map create : " + getThreadName())
    Observable.just(getThreadName())
}
.doOnNext { logInfo("yep", "Doonnext : " + getThreadName())  }
.map {
    logInfo("yep", "Map : " + getThreadName())
    ""
}
.observeOn(AndroidSchedulers.mainThread())
.subscribeOn(Schedulers.io())
.subscribe({
    logInfo("yep", "Subscribed : " + getThreadName())
}, {
    logInfo("yep", "Error Occurred")
}, {
    logInfo("yep", "On Complete : " + getThreadName())
})
```
```
2018-12-27 11:26:52.765 13557-13592/com.niks.sampleapp I/yep: Observable.create  : IO Thread
2018-12-27 11:26:52.765 13557-13592/com.niks.sampleapp I/yep: Switch map Out : IO Thread
2018-12-27 11:26:52.766 13557-13592/com.niks.sampleapp I/yep: Switch map create : IO Thread
2018-12-27 11:26:52.767 13557-13592/com.niks.sampleapp I/yep: Doonnext : IO Thread
2018-12-27 11:26:52.767 13557-13592/com.niks.sampleapp I/yep: Map : IO Thread
2018-12-27 11:26:52.768 13557-13592/com.niks.sampleapp I/yep: Switch map create : IO Thread
2018-12-27 11:26:52.822 13557-13557/com.niks.sampleapp I/yep: Subscribed : Main Thread
```

# Observation 2

```
Observable.create<String> { emitter ->
    logInfo("yep", "Observable.create  : " + getThreadName())
    emitter.onNext(getThreadName())
}
.observeOn(AndroidSchedulers.mainThread())
.switchMap {
    logInfo("yep", "Switch map Out : " + getThreadName())
    Observable.create<String> { emitter ->
        emitter.onNext(getThreadName())
        logInfo("yep", "Switch map create : " + getThreadName())
    }
}.switchMap {
    logInfo("yep", "Switch map create : " + getThreadName())
    Observable.just(getThreadName())
}
.doOnNext { logInfo("yep", "Doonnext : " + getThreadName())  }
.map {
    logInfo("yep", "Map : " + getThreadName())
    ""
}
.observeOn(AndroidSchedulers.mainThread())
.subscribeOn(Schedulers.io())
.subscribe({
    logInfo("yep", "Subscribed : " + getThreadName())
}, {
    logInfo("yep", "Error Occurred")
}, {
    logInfo("yep", "On Complete : " + getThreadName())
})
```
```
2018-12-27 11:28:20.730 13885-13918/com.niks.sampleapp I/yep: Observable.create  : IO Thread
2018-12-27 11:28:20.786 13885-13885/com.niks.sampleapp I/yep: Switch map Out : Main Thread
2018-12-27 11:28:20.786 13885-13885/com.niks.sampleapp I/yep: Switch map create : Main Thread
2018-12-27 11:28:20.788 13885-13885/com.niks.sampleapp I/yep: Doonnext : Main Thread
2018-12-27 11:28:20.788 13885-13885/com.niks.sampleapp I/yep: Map : Main Thread
2018-12-27 11:28:20.788 13885-13885/com.niks.sampleapp I/yep: Switch map create : Main Thread
2018-12-27 11:28:20.877 13885-13885/com.niks.sampleapp I/yep: Subscribed : Main Thread
```

