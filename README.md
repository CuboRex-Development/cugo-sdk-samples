# CugoArduinoSDKSampleクイックリファレンス
新製品のクローラロボット開発プラットフォームをご利用の方はこちらをご覧ください</br>
https://github.com/CuboRex-Development/cugo-sdk-samples/tree/pico

初心者向けサンプルコードや組み立て方はこちらをご参照ください  
https://github.com/CuboRex-Development/cugo-arduino-beginner-programming
<!-- クイックリファレンス はじめ-->

## 目次
- [1. はじめに](#1-はじめに)
- [2. 使い方](#2-使い方)
  - [2-1. 自動走行モード関数例](#2-1-自動走行モード関数例)
- [3. 各種関数](#3-各種関数)
  - [3-1. 自動走行関連](#3-1-自動走行関連)
    - [cugo\_move\_forward](#cugo_move_forward)
    - [cugo\_turn\_clockwise](#cugo_turn_clockwise)
    - [cugo\_turn\_counterclockwise](#cugo_turn_counterclockwise)
    - [cugo\_curve\_theta\_raw](#cugo_curve_theta_raw)
    - [cugo\_curve\_distance\_raw](#cugo_curve_distance_raw)
    - [cugo\_stop](#cugo_stop)
    - [cugo\_wait](#cugo_wait)
  - [3-2. 各種センサ取得関連](#3-2-各種センサ取得関連)
    - [cugo\_check\_odometer](#cugo_check_odometer)
    - [cugo\_check\_propo\_channel\_value](#cugo_check_propo_channel_value)
    - [cugo\_check\_button](#cugo_check_button)
    - [cugo\_check\_button\_times](#cugo_check_button_times)
    - [cugo\_check\_button\_press\_time](#cugo_check_button_press_time)
    - [cugo\_reset\_button\_times](#cugo_reset_button_times)
  - [3-3. その他](#3-3-その他)
    - [cugo\_setup](#cugo_setup)
    - [cugo\_rcmode](#cugo_rcmode)
    - [cugo\_motor\_direct\_instructions](#cugo_motor_direct_instructions)
- [4. 参考：MotorControllerクラス](#4-参考motorcontrollerクラス)
    - [driveMotor](#drivemotor)
    - [reset\_PID\_param](#reset_pid_param)
    - [setTargetRpm](#settargetrpm)
    - [getTargetRpm](#gettargetrpm)
    - [getCount](#getcount)
    - [getRpm](#getrpm)

<a id="anchor1"></a>

# 1. はじめに

- CuGoArduinoSDKSampleに関するクイックリファレンスです。CuGoArduinoSDKSampleを利用してCuGoを動かす上での各種関数等を説明します。
- CuGoArduinoSDKSampleを動かすための事前準備は以下を参考にしてください
  - [ArduinoKitアップグレード説明書](https://github.com/CuboRex-Development/cugo-arduino-beginner-programming/blob/462fc9e3d52d90777aff0116ad4912cc0e54ffc5/manuals/ArduinoKit%E3%82%A2%E3%83%83%E3%83%95%E3%82%9A%E3%82%AF%E3%82%99%E3%83%AC%E3%83%BC%E3%83%88%E3%82%99%E8%AA%AC%E6%98%8E%E6%9B%B8.pdf)

  - [CugoArduinoKitクイックスタートガイド](https://github.com/CuboRex-Development/cugo-arduino-beginner-programming/blob/main/README.md#cugoarduinokit%E3%82%AF%E3%82%A4%E3%83%83%E3%82%AF%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%88%E3%82%AC%E3%82%A4%E3%83%89)
    - ただし、CuGoArduinoKitクイックスタートガイドに記載のあるコマンドはbeginner用のため利用できません。
    - CuGoArduinoSDKのコマンドは本ページの [3. 各種関数](#3-各種関数)
をご利用ください。


<a id="anchor2"></a>

# 2. 使い方

- CuGoArduinoSDKSampleにて自作関数を利用する場合は`loop`内の `//ここから自動走行モードの記述`から `//ここまで自動走行モードの記述`までに記載してください。

```c
void loop()
{
  cugo_check_mode_change(cugo_motor_controllers);
  if (cugoRunMode == CUGO_RC_MODE){ //RC（ラジコン）操作    
    cugo_rcmode(cugoRcTime,cugo_motor_controllers);
  }
  if (cugoRunMode == CUGO_ARDUINO_MODE){
    //ここから自動走行モードの記述

    //ここまで自動走行モードの記述
      cugoRunMode = CUGO_RC_MODE; //自動走行をループさせたい場合はCUGO_ARDUINO_MODEに変更
  }
}
```
<a id="anchor2-1"></a>

## 2-1. 自動走行モード関数例

- 正方形に移動させたい場合は下記コードを参考にしてください

```c
void loop()
{
  cugo_check_mode_change(cugo_motor_controllers);
  if (cugoRunMode == CUGO_RC_MODE){ //RC（ラジコン）操作    
    cugo_rcmode(cugoRcTime,cugo_motor_controllers);
  }
  if (cugoRunMode == CUGO_ARDUINO_MODE){
    //ここから自動走行モードの記述
    Serial.println("1.0mの正方形移動の実施");

    cugo_move_forward(1.0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_turn_clockwise(90,0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_move_forward(1.0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_turn_clockwise(90,0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_move_forward(1.0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_turn_clockwise(90,0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_move_forward(1.0,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_turn_clockwise(90,0,cugo_motor_controllers);
    cugo_wait(1000);
    //ここまで自動走行モードの記述
      cugoRunMode = CUGO_RC_MODE; //自動走行をループさせたい場合はCUGO_ARDUINO_MODEに変更
  }
}
```
- S字に移動させたい場合は下記コードを参考にしてください

```c
void loop()
{
  cugo_check_mode_change(cugo_motor_controllers);
  if (cugoRunMode == CUGO_RC_MODE){ //RC（ラジコン）操作    
    cugo_rcmode(cugoRcTime,cugo_motor_controllers);
  }
  if (cugoRunMode == CUGO_ARDUINO_MODE){
    //ここから自動走行モードの記述
    Serial.println("半径1.0mのS字移動");

    cugo_curve_distance_raw(1.0,180,90,cugo_motor_controllers);
    cugo_wait(1000);
    cugo_curve_distance_raw(-1.0,180,90,cugo_motor_controllers);
    cugo_wait(1000);

    //ここまで自動走行モードの記述
      cugoRunMode = CUGO_RC_MODE; //自動走行をループさせたい場合はCUGO_ARDUINO_MODEに変更
  }
}
```
- その他、関数の使い方の例は`CugoArduinoSDK.cpp`内の`cugo_test`を参考にしてください。

<a id="anchor3"></a>

# 3. 各種関数

<a id="anchor3-1"></a>

## 3-1. 自動走行関連
<a id="anchor3-1-1"></a>
自動走行はMotorControllerクラスを利用し左右のモータを作動させます。

### cugo_move_forward
- 【説明】
  - CuGoが前進後進する関数です。
  - `cugo_move_forward_raw`は位置制御を実施していないため、上限速度まで急峻に到達し、目標距離を超えると急停止します。
- 【構文】
  -  `cugo_move_forward(float target_distance,MotorController cugo_motor_controllers[MOTOR_NUM])`
  -  `cugo_move_forward(float target_distance,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
  - `cugo_move_forward_raw(float target_distance,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
- 【パラメータ】
  - `target_distance` ：目標距離　単位はm
  - `target_rpm` ：上限速度　単位はrpm
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし

<a id="anchor3-1-2"></a>

### cugo_turn_clockwise
- 【説明】
  - CuGoが時計回りに回転する関数です。
  - `cugo_turn_clockwise_raw`は位置制御を実施していないため、上限速度まで急峻に到達し、目標距離を超えると急停止します。
- 【構文】
  - `cugo_turn_clockwise(float target_degree,MotorController cugo_motor_controllers[MOTOR_NUM])`
  - `cugo_turn_clockwise(float target_degree,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
  - `cugo_turn_clockwise_raw(float target_degree,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
- 【パラメータ】
  - `target_degree` ：目標距離　単位は度
  - `target_rpm` ：上限速度　単位はrpm
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし

<a id="anchor3-1-3"></a>

### cugo_turn_counterclockwise
- 【説明】
  - CuGoが反時計回りに回転する関数です。
  - `cugo_turn_counterclockwise_raw`は位置制御を実施していないため、上限速度まで急峻に到達し、目標距離を超えると急停止します。
- 【構文】
  - `cugo_turn_counterclockwise(float target_degree,MotorController cugo_motor_controllers[MOTOR_NUM])`
  - `cugo_turn_counterclockwise(float target_degree,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
  - `cugo_turn_counterclockwise_raw(float target_degree,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
- 【パラメータ】
  - `target_degree` ：目標距離　単位は度
  - `target_rpm` ：上限速度　単位はrpm
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし

<a id="anchor3-1-4"></a>

### cugo_curve_theta_raw

- 【説明】
  - CuGoが指定した半径を指定した角度まで円軌道する関数です。
  - `cugo_turn_counterclockwise_raw`は位置制御を実施していないため、上限速度まで急峻に到達し、目標距離を超えると急停止します。
- 【構文】
  - `cugo_curve_theta_raw(float target_radius,float target_theta,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`
- 【パラメータ】
  - `target_radius` ：円軌道半径　単位はm
    - 円軌道半径は指定した値が正の場合、原点はcugoの進行方向左側になります
  - `target_theta` ：円軌道角度　単位は度
  - `target_rpm` ：上限速度　単位はrpm
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし
 
<a id="anchor3-1-5"></a>

### cugo_curve_distance_raw
- 【説明】
  - CuGoが指定した半径を指定した移動距離まで円軌道する関数です。
  - `cugo_turn_counterclockwise_raw`は位置制御を実施していないため、上限速度まで急峻に到達し、目標距離を超えると急停止します。
- 【構文】
  - `cugo_curve_distance_raw(float target_radius,float target_disttance,float target_rpm,MotorController cugo_motor_controllers[MOTOR_NUM])`  
- 【パラメータ】
  - `target_radius` ：円軌道半径　単位はm 
    - 円軌道半径は指定した値が正の場合、原点はCuGoの進行方向左側になります
  - `target_disttance` ：円軌道距離　単位はm
  - `target_rpm` ：上限速度　単位はrpm
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし

<a id="anchor3-1-6"></a>

### cugo_stop
- 【説明】
  - CuGoが停止する関数です。
- 【構文】
  - `cugo_stop(MotorController cugo_motor_controllers[MOTOR_NUM])`  
- 【パラメータ】
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし
<a id="anchor3-1-7"></a>

### cugo_wait
- 【説明】
  - CugoArduinoSDKSampleでは割り込み処理を利用してモード遷移をしているため、**delay関数を使用しないことを強く推奨します。**
  - `cugo_wait`はdelay関数と同等の役割を果たす関数になります。
  - `cugo_wait`はCugoArduinoSDKSample内で利用する停止関数です。
  - `cugo_wait`の計測時間の上限は約70分です。７０分以上の計測には`cugo_long_wait`を利用してください。
  - ARDUINO内の水晶子を利用しているため時間は正確でない場合があります。

- 【構文】
  - `cugo_wait(unsigned long long int wait_ms)`  
  - `cugo_long_wait(unsigned long long int wait_seconds)`  
- 【パラメータ】
  - `wait_ms` ：停止時間　単位はミリ秒
  - `wait_sedonds` ：停止時間　単位は秒
- 【戻り値】
  - なし

<a id="anchor3-2"></a>

## 3-2. 各種センサ取得関連

<a id="anchor3-2-1"></a>

### cugo_check_odometer
- 【説明】
  - CuGoのオドメトリを取得します。
- 【構文】
  - `cugo_check_odometer(int check_number)`
- 【パラメータ】
    - `check_number` :以下から選択
    - `cugo_odometer_x`  , `cugo_odometer_y`  , `cugo_odometer_theta`, `cugo_odometer_degree`
    - 上記の定数はdefineによりそれぞれ0,1,2,3の数値に対応しています。  
- 【戻り値】
  - float型  
<a id="anchor3-2-1"></a>

### cugo_check_propo_channel_value
- 【説明】
  - プロポ入力の結果を出力します。
- 【構文】
  - `cugo_check_propo_channel_value(int channel_number)`
- 【パラメータ】
  - `channel_number` :以下のチャンネルから選択
    - `cugo_propo_Ach`  , `cugo_propo_Bch`  , `cugo_propo_Cch`
    - 上記の定数はdefineによりそれぞれ0,1,2の数値に対応しています。 
- 【戻り値】
  - int型

<a id="anchor3-2-3"></a>

### cugo_check_button
- 【説明】
  - ボタンが現在押されているかを出力します。
  - 押されている状態ではtrueを、押されていない場合falseを返します。
- 【構文】
  - `cugo_check_button`
- 【パラメータ】
  - なし
- 【戻り値】
  - bool型

<a id="anchor3-2-4"></a>

### cugo_check_button_times
- 【説明】
  - ボタンの押された回数を出力します。
  - この関数を使う際には`cugo_reset_button_times`も確認して下さい。
- 【構文】
  - `cugo_check_button_times`
- 【パラメータ】
  - なし
- 【戻り値】
  - int型

<a id="anchor3-2-5"></a>

### cugo_check_button_press_time
- 【説明】
  - ボタンの押されている時間(ms)を出力します。
  - 押されていない間は0を出力します。
- 【構文】
  - `cugo_button_press_time`
- 【パラメータ】
  - なし
- 【戻り値】
  - int型

<a id="anchor3-2-6"></a>

### cugo_reset_button_times
- 【説明】
  - `cugo_check_button_times`にて出力されるボタンの押された回数を初期化します。
- 【構文】
  - `cugo_reset_button_times` 
- 【パラメータ】
  - なし
- 【戻り値】
  - なし

<a id="anchor3-3"></a>

## 3-3. その他

<a id="anchor3-3-1"></a>

### cugo_setup
- 【説明】
  - CuGoを利用するための初期設定関数です。setup関数内にあります。
- 【構文】
  - `cugo_setup`

<a id="anchor3-3-2"></a>

### cugo_rcmode
- 【説明】
  - CuGoをプロポ入力で操作します。
  - ラジコン操作については★を参照して下さい。
- 【構文】
  - `cugo_rcmode(volatile unsigned long cugoRcTime[PWM_IN_MAX],MotorController cugo_motor_controllers[MOTOR_NUM])`

<a id="anchor3-3-3"></a>

### cugo_motor_direct_instructions
- 【説明】
  - モーターへプロポ入力します。
  - モーターを停止させたい場合は左右のパラメータに`1500`を入力します。
- 【構文】
  - `cugo_motor_direct_instructions(int left, int right,MotorController cugo_motor_controllers[MOTOR_NUM])` 
- 【パラメータ】
  - `left` 左モーターへのプロポ入力値
  - `right` 右モーターへのプロポ入力値
  - `cugo_motor_controllers[MOTOR_NUM]` ：MotorControllerクラス　変更不要
- 【戻り値】
  - なし

<a id="anchor4"></a>

# 4. 参考：MotorControllerクラス
CugoArduinoSDKSampleにおいてMotorControllerクラスを利用する際に便利な関数を紹介します。
<a id="anchor4-1"></a>

### driveMotor
- 【説明】
  - setTargetRpmで指定した速度になるようモーターへRC信号を入力します。
  - driveMotorへの実行間隔は5ms以上空けることを推奨します。
<a id="anchor4-2"></a>

### reset_PID_param
- 【説明】
  - MotorControllerクラス内の制御パラメータおよびエンコーダーのカウント値を初期化します。
<a id="anchor4-3"></a>

### setTargetRpm
- 【説明】
  - モーターの目標速度を設定します。
<a id="anchor4-4"></a>

### getTargetRpm
- 【説明】
  - モーターの目標速度を出力します。
<a id="anchor4-5"></a>

### getCount
- 【説明】
  - エンコーダーのカウントを出力します。
<a id="anchor4-6"></a>

### getRpm
- 【説明】
  - モーターの速度を出力します。

<!-- クイックリファレンス ここまで-->  
