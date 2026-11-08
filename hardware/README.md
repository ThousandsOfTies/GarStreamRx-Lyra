# GarStreamRx hardware contract

`requirements.json` は GarStreamRx が必要とする装置・信号の宣言であり、特定ボードの
GPIO offset や接続先を含みません。`bindings/luckfox-rk3506.json` が、その要件を
Luckfox Lyra Plus の capability resource に接続します。`gar hw validate --json` は
この二つと Target capability を照合し、pin conflict、driver/voltage不足、SPI bus/CSの
重複および速度不足を配置前に報告します。

CSV は simulator 用の legacy device contract です。KY-040 は lines 20/21/22、ILI9341の
DC/RST は lines 23/24 のままです。これは実機 binding（DC/RST=3/2、A/B/SW=8/9/10）と
意図的に別で、実機の配線やmachine-local設定をartifactへ混ぜません。

## KY-040の初期化パラメーター

物理ターゲットの起動処理は`GAR_ENC_DEBOUNCE_US=1000`（要求値1ミリ秒）を初期値とします。
環境変数で機種ごとに変更でき、0で無効にできます。シミュレータは明示的に0を指定します。
値はCLK・DT・SWのGPIO入力に渡し、アプリのクリック数や番号判定の規則は変更しません。
この初期値の実機確認済み対象は別ProductのLyra Zero Wです。このProductの物理ボードでは
未確認のため、高速回転を含めて追従性を確認してから導入してください。
native版はGPIO確保時に設定します。旧Python版を単体で使う場合は、gar-toolsの共有`gar_gpio_debounce.py`も用意します。
