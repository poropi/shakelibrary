# Shakeライブラリ

Android5.0以上で利用できるライブラリです。
端末を振ることで加速度センサーに反応し、コールバックを実装したクラスに通知するってだけのものです。
ただ、モンキー的に端末を連続で振っても、その度にコールバックが通知されないようにもしています。
（振りはじめの100ms後に通知される）

なお、Kotlin1.2.61で実装しています。
Javaでも動作するとは思いますが、未検証です。

## 実装方法
以下をbuild.gradle(app)に設定します。
```
repositories {
    maven { url 'https://poropi.github.io/shakelibrary/repository/' }
}

dependencies {
    implementation 'jp.ne.poropi:shakelibrary:1.0.1'
}
```

以下、Kotlinでの実装です。
例としてActivityに実装していますが、ServiceでもOKです。

```
import jp.ne.poropi.shakelibrary.ShakeSensor // これ追加

class MainActivity : AppCompatActivity(), ShakeSensor.OnShakeListener { // ←クラス実装する場合
    lateinit var mShakeSensor: ShakeSensor
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        mShakeSensor = ShakeSensor(this)
//        // setOnShakeListenerにLambdaで実装もOK
//        mShakeSensor.setOnShakeListener {
//            Log.d("","ShakeListener speed: $it")
//        }
    }

    // 振った結果は、以下のメソッドにコールバックされるので、お好きな処理を実装しましょう！
    override fun onShake(speed: Float) {
        Log.d("","mShakeSensor speed: $speed")
    }

    override fun onResume() {
        // resume時に以下のメソッドを呼んでください
        mShakeSensor.resume()
        super.onResume()
    }

    override fun onPause() {
        // pause時に以下のメソッドを呼んでください
        mShakeSensor.pause()
        super.onPause()
    }
}

```

以上です。
